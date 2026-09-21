# Backend y APIs con Python (Nivel PhD, Septiembre 2026)

## 1. FastAPI 0.115+ — El Framework Moderno

```python
from fastapi import FastAPI, Depends, HTTPException, status, Query, Path, Header
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import StreamingResponse
from pydantic import BaseModel, Field, ConfigDict
from typing import Annotated
from datetime import datetime, timezone
from sqlalchemy.ext.asyncio import AsyncSession
import asyncio

app = FastAPI(
    title="API de Análisis de Datos",
    version="2.0.0",
    description="API para procesamiento y análisis de datos en tiempo real",
)

# CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Modelos Pydantic v3
class DatosEntrada(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True, frozen=True)

    valores: list[float] = Field(..., min_length=1, max_length=10000)
    metodo: str = Field(default="media", pattern="^(media|mediana|moda|varianza)$")
    pesos: list[float] | None = Field(None, min_length=1)

class RespuestaEstadistica(BaseModel):
    metodo: str
    resultado: float
    n_valores: int
    tiempo_ms: float
    timestamp: datetime = Field(default_factory=lambda: datetime.now(timezone.utc))

# Dependency Injection
async def verificar_api_key(
    api_key: str = Header(..., description="API key"),
    db: AsyncSession = Depends(get_db),
) -> Usuario:
    usuario = await db.execute(select(Usuario).where(Usuario.api_key == api_key))
    if not usuario:
        raise HTTPException(status_code=401, detail="API key inválida")
    return usuario

# Endpoints
@app.post("/analisis/estadistico", response_model=RespuestaEstadistica)
async def analisis_estadistico(
    datos: DatosEntrada,
    usuario: Usuario = Depends(verificar_api_key),
):
    """Calcula estadísticas sobre una lista de valores."""
    import time
    start = time.perf_counter()

    calculos = {
        "media": lambda v, p: np.average(v, weights=p) if p else np.mean(v),
        "mediana": lambda v, p: float(np.median(v)),
        "moda": lambda v, p: float(stats.mode(v, keepdims=False).mode),
        "varianza": lambda v, p: float(np.var(v)),
    }

    resultado = calculos[datos.metodo](datos.valores, datos.pesos)
    elapsed = (time.perf_counter() - start) * 1000

    return RespuestaEstadistica(
        metodo=datos.metodo,
        resultado=resultado,
        n_valores=len(datos.valores),
        tiempo_ms=round(elapsed, 3),
    )

# Streaming de datos
@app.get("/stream/datos")
async def stream_datos(usuario: Usuario = Depends(verificar_api_key)):
    async def generar():
        for i in range(100):
            yield f"data: {json.dumps({'i': i, 'valor': i**2})}\n\n"
            await asyncio.sleep(0.1)
    return StreamingResponse(generar(), media_type="text/event-stream")

# Background tasks
from fastapi import BackgroundTasks

@app.post("/procesar")
async def procesar(
    datos: DatosEntrada,
    background_tasks: BackgroundTasks,
    usuario: Usuario = Depends(verificar_api_key),
):
    background_tasks.add_task(procesar_en_bg, datos, usuario.id)
    return {"status": "procesamiento iniciado"}
```

---

## 2. Autenticación JWT Completa

```python
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from jose import JWTError, jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta

# Configuración
SECRET_KEY = "tu-secret-key-segura-aqui"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
security = HTTPBearer()

# Hash de contraseñas
def verificar_contrasena(plain: str, hashed: str) -> bool:
    return pwd_context.verify(plain, hashed)

def hashear_contrasena(password: str) -> str:
    return pwd_context.hash(password)

# JWT
def crear_token(data: dict, expires_delta: timedelta | None = None) -> str:
    to_encode = data.copy()
    expire = datetime.now(timezone.utc) + (expires_delta or timedelta(minutes=15))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def verificar_token(credentials: HTTPAuthorizationCredentials = Depends(security)) -> dict:
    try:
        payload = jwt.decode(credentials.credentials, SECRET_KEY, algorithms=[ALGORITHM])
        return payload
    except JWTError:
        raise HTTPException(status_code=401, detail="Token inválido")

# Login
@app.post("/auth/login")
async def login(usuario: LoginRequest, db: Session = Depends(get_db)):
    user = db.execute(select(Usuario).where(Usuario.email == usuario.email)).scalar_one_or_none()
    if not user or not verificar_contrasena(usuario.password, user.hash_password):
        raise HTTPException(status_code=401, detail="Credenciales inválidas")

    access_token = crear_token(
        data={"sub": user.email, "role": user.role},
        expires_delta=timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    )
    return {"access_token": access_token, "token_type": "bearer"}
```

---

## 3. Microservicios con Docker

```python
# Servicio de usuarios
from fastapi import FastAPI
import redis.asyncio as redis

app = FastAPI(title="User Service")
redis_client = redis.from_url("redis://localhost:6379")

@app.get("/users/{user_id}")
async def get_user(user_id: str):
    cached = await redis_client.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)

    user = await db.get_user(user_id)
    await redis_client.setex(f"user:{user_id}", 300, json.dumps(user))
    return user
```

```yaml
# docker-compose.yml — microservicios
services:
  api-gateway:
    build: ./gateway
    ports: ["8000:8000"]
    depends_on: [users-service, data-service]

  users-service:
    build: ./users
    environment:
      DATABASE_URL: postgresql://user:pass@db/users
      REDIS_URL: redis://redis:6379

  data-service:
    build: ./data
    environment:
      DATABASE_URL: postgresql://user:pass@db/data
      KAFKA_BROKERS: kafka:9092

  db:
    image: postgres:17
    volumes: [pgdata:/var/lib/postgresql/data]

  redis:
    image: redis:7-alpine

  kafka:
    image: confluentinc/cp-kafka:7.7.0
```

---

## 4. WebSockets para Tiempo Real

```python
from fastapi import WebSocket, WebSocketDisconnect
from typing import dict

class ConnectionManager:
    def __init__(self):
        self.active: dict[str, list[WebSocket]] = {}

    async def connect(self, ws: WebSocket, room: str):
        await ws.accept()
        self.active.setdefault(room, []).append(ws)

    async def disconnect(self, ws: WebSocket, room: str):
        self.active[room].remove(ws)

    async def broadcast(self, room: str, message: str):
        for ws in self.active.get(room, []):
            await ws.send_text(message)

manager = ConnectionManager()

@app.websocket("/ws/{room}")
async def websocket_endpoint(ws: WebSocket, room: str):
    await manager.connect(ws, room)
    try:
        while True:
            data = await ws.receive_text()
            await manager.broadcast(room, f"Room {room}: {data}")
    except WebSocketDisconnect:
        await manager.disconnect(ws, room)
```

---

## 5. Testing de APIs

```python
import pytest
from httpx import AsyncClient, ASGITransport
from app.main import app

@pytest.fixture
async def client():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as ac:
        yield ac

@pytest.mark.asyncio
async def test_crear_usuario(client):
    response = await client.post("/users", json={
        "nombre": "Ana",
        "email": "ana@test.com",
        "password": "secreta123"
    })
    assert response.status_code == 201
    data = response.json()
    assert data["nombre"] == "Ana"
    assert "id" in data

@pytest.mark.asyncio
async def test_login(client):
    await client.post("/users", json={
        "nombre": "Test",
        "email": "test@test.com",
        "password": "123456"
    })
    response = await client.post("/auth/login", json={
        "email": "test@test.com",
        "password": "123456"
    })
    assert response.status_code == 200
    assert "access_token" in response.json()
```
