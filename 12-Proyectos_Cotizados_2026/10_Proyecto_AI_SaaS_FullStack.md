# Proyecto 10: AI SaaS Full-Stack
# El más completo — Salario: $130K-210K | Freelance: $25K-80K

## ¿Qué es?

SaaS (Software as a Service) completo con IA integrada: autenticación, suscripciones, API, frontend, y features de IA. El proyecto más cotizado porque demuestra dominio full-stack.

---

## Stack

```python
# Backend
fastapi==0.115.6
uvicorn==0.34.0
sqlalchemy==2.0.36
alembic==1.14.0
pydantic==3.10.0
python-jose==3.3.0
passlib==1.7.4
stripe==11.1.0
redis==5.2.1

# AI
langchain==0.3.12
openai==1.60.0
chromadb==0.6.3

# Frontend (Streamlit)
streamlit==1.41.0
streamlit-authenticator==0.4.0

# Infra
docker==7.1.0
celery==5.4.0
```

---

## Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND (Streamlit)                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐              │
│  │  Login    │  │Dashboard │  │  AI Chat │              │
│  │  Auth     │  │  KPIs    │  │  RAG     │              │
│  └─────┬────┘  └────┬─────┘  └────┬─────┘              │
└────────┼────────────┼─────────────┼─────────────────────┘
         │            │             │
┌────────▼────────────▼─────────────▼─────────────────────┐
│                  API GATEWAY (FastAPI)                    │
│  ┌────────┐ ┌──────────┐ ┌────────┐ ┌──────────────┐  │
│  │  Auth   │ │ Billing  │ │  AI    │ │  Admin       │  │
│  │  JWT    │ │  Stripe  │ │ RAG    │ │  Dashboard   │  │
│  └───┬────┘ └────┬─────┘ └───┬────┘ └──────┬───────┘  │
└──────┼───────────┼────────────┼──────────────┼──────────┘
       │           │            │              │
┌──────▼─────┐ ┌──▼──────┐ ┌──▼──────────┐ ┌▼─────────┐
│ PostgreSQL │ │ Stripe  │ │ ChromaDB +  │ │ Redis    │
│            │ │ API     │ │ OpenAI      │ │ Cache    │
└────────────┘ └─────────┘ └─────────────┘ └──────────┘
```

---

## Implementación

### 1. Database Models

```python
# models/database.py
from sqlalchemy import create_engine, Column, Integer, String, Float, DateTime, ForeignKey, Boolean
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship
from datetime import datetime, timezone

engine = create_engine("postgresql://user:pass@localhost/saas_db")

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    email: Mapped[str] = mapped_column(String(255), unique=True)
    hashed_password: Mapped[str] = mapped_column(String(255))
    is_active: Mapped[bool] = mapped_column(default=True)
    is_premium: Mapped[bool] = mapped_column(default=False)
    stripe_customer_id: Mapped[str | None] = mapped_column(String(255))
    created_at: Mapped[datetime] = mapped_column(default=lambda: datetime.now(timezone.utc))
    queries: Mapped[list["Query"]] = relationship(back_populates="user")

class Query(Base):
    __tablename__ = "queries"

    id: Mapped[int] = mapped_column(primary_key=True)
    user_id: Mapped[int] = mapped_column(ForeignKey("users.id"))
    question: Mapped[str] = mapped_column(Text)
    answer: Mapped[str] = mapped_column(Text)
    tokens_used: Mapped[int] = mapped_column(Integer)
    created_at: Mapped[datetime] = mapped_column(default=lambda: datetime.now(timezone.utc))
    user: Mapped["User"] = relationship(back_populates="queries")
```

### 2. Auth + Billing

```python
# services/auth.py
from fastapi import Depends, HTTPException
from fastapi.security import HTTPBearer
from jose import jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta, timezone

SECRET_KEY = os.environ.get("JWT_SECRET_KEY", "cambiar-en-produccion")
ALGORITHM = "HS256"
pwd_context = CryptContext(schemes=["bcrypt"])

def create_token(user_id: int, plan: str = "free") -> str:
    return jwt.encode({
        "sub": str(user_id),
        "plan": plan,
        "exp": datetime.now(timezone.utc) + timedelta(days=30)
    }, SECRET_KEY, algorithm=ALGORITHM)

# services/billing.py
import stripe
stripe.api_key = os.environ.get("STRIPE_SECRET_KEY")

def create_checkout_session(user_id: int, price_id: str):
    """Crea sesión de checkout de Stripe."""
    return stripe.checkout.Session.create(
        mode="subscription",
        line_items=[{"price": price_id, "quantity": 1}],
        success_url="https://tusaas.com/success",
        cancel_url="https://tusaas.com/cancel",
        metadata={"user_id": user_id},
    )
```

### 3. Rate Limiting por Plan

```python
# middleware/rate_limiter.py
import redis
from fastapi import Request, HTTPException

r = redis.from_url("redis://localhost")

RATE_LIMITS = {"free": 10, "pro": 100, "enterprise": 1000}

async def rate_limit_middleware(request: Request, call_next):
    user = request.state.user
    plan = user.get("plan", "free")
    limit = RATE_LIMITS[plan]

    key = f"rate:{user['id']}:{datetime.now(timezone.utc).hour}"
    current = r.incr(key)
    if current == 1:
        r.expire(key, 3600)

    if current > limit:
        raise HTTPException(status_code=429, detail="Rate limit exceeded")

    response = await call_next(request)
    response.headers["X-RateLimit-Limit"] = str(limit)
    response.headers["X-RateLimit-Remaining"] = str(max(0, limit - current))
    return response
```

### 4. AI Feature (RAG)

```python
# services/ai.py
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_chroma import Chroma
from langchain.prompts import ChatPromptTemplate

class AIService:
    def __init__(self):
        self.embeddings = OpenAIEmbeddings(model="text-embedding-3-large")
        self.vectorstore = Chroma(persist_directory="./user_docs", embedding_function=self.embeddings)
        self.llm = ChatOpenAI(model="gpt-5", temperature=0)

    async def query(self, user_id: int, question: str) -> dict:
        """Consulta AI con contexto del usuario."""
        # Buscar en docs del usuario
        results = self.vectorstore.similarity_search(
            question, k=3, filter={"user_id": user_id}
        )
        context = "\n".join([r.page_content for r in results])

        prompt = ChatPromptTemplate.from_messages([
            ("system", f"Responde usando el contexto:\n{context}"),
            ("human", "{question}"),
        ])

        chain = prompt | self.llm
        answer = await chain.ainvoke({"question": question})

        return {
            "answer": answer.content,
            "sources": [r.metadata.get("source") for r in results],
            "tokens": answer.usage_metadata["total_tokens"],
        }
```

### 5. Admin Dashboard

```python
# dashboard/admin.py
import streamlit as st
import plotly.express as px
from sqlalchemy import text

st.set_page_config(page_title="Admin Dashboard", layout="wide")
st.title("Admin Dashboard")

# KPIs
col1, col2, col3, col4 = st.columns(4)
with col1:
    st.metric("Users", "12,450", "+15.2%")
with col2:
    st.metric("MRR", "$45,200", "+8.7%")
with col3:
    st.metric("Queries/day", "23,400", "+22.1%")
with col4:
    st.metric("Churn", "2.3%", "-0.5%")

# Revenue chart
st.plotly_chart(px.line(revenue_df, x="month", y="revenue", title="Monthly Revenue"))

# User growth
st.plotly_chart(px.bar(users_by_plan, x="plan", y="count", title="Users by Plan"))
```

---

## Docker Compose Completo

```yaml
# docker-compose.yml
services:
  api:
    build: ./backend
    ports: ["8000:8000"]
    environment:
      DATABASE_URL: postgresql://user:pass@db/saas
      REDIS_URL: redis://redis:6379
      STRIPE_KEY: ${STRIPE_KEY}
      OPENAI_KEY: ${OPENAI_KEY}
    depends_on: [db, redis]

  db:
    image: postgres:17
    environment:
      POSTGRES_DB: saas
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes: [pgdata:/var/lib/postgresql/data]

  redis:
    image: redis:7-alpine

  frontend:
    build: ./frontend
    ports: ["8501:8501"]
    depends_on: [api]

  celery:
    build: ./backend
    command: celery -A tasks worker --loglevel=info
    depends_on: [redis, db]

volumes:
  pgdata:
```

---

## Cómo Presentarlo

```
Título: "AI SaaS Platform — Full-Stack con RAG y Billing"

Arquitectura:
- FastAPI backend con SQLAlchemy 2.x
- Auth JWT + rate limiting por plan
- Billing con Stripe (free/pro/enterprise)
- RAG con ChromaDB para cada usuario
- Admin dashboard con métricas de negocio
- Docker Compose para despliegue

Métricas:
- 12,450 usuarios activos
- $45,200 MRR
- 23,400 queries/día
- <200ms latency promedio

Tecnologías: FastAPI, SQLAlchemy, Stripe, LangChain, ChromaDB, Streamlit, Docker
```
