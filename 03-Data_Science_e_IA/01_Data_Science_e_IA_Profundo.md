# Data Science e IA — De NumPy a Agentes Autónomos (Nivel PhD, Septiembre 2026)

## 1. NumPy 2.x — Computación Numérica de Alto Rendimiento

### Arquitectura Interna
```python
import numpy as np

# Un ndarray es un bloque contiguo de memoria + metadata
a = np.array([1, 2, 3, 4, 5], dtype=np.float64)
print(a.shape)     # (5,)
print(a.strides)   # (8,) — bytes entre elementos (float64 = 8 bytes)
print(a.dtype)     # float64
print(a.nbytes)    # 40 bytes

# Broadcasting — operaciones sin copia de memoria
matriz = np.random.randn(1000, 100)
vector = np.random.randn(100)
resultado = matriz + vector  # Broadcasting: (1000,100) + (100,) → (1000,100)

# Strides negativos — reversión sin copia
a = np.array([1, 2, 3, 4, 5])
b = a[::-1]  # Misma memoria, strides negativos
print(b.base is a)  # True — comparten memoria
```

### Operaciones Vectorizadas Avanzadas
```python
# Comparación de rendimiento
import time

def suma_pure_python(n: int) -> float:
    lista = list(range(n))
    return sum(x**2 for x in lista)

def suma_numpy(n: int) -> float:
    arr = np.arange(n, dtype=np.float64)
    return np.sum(arr ** 2)

n = 10_000_000
# Python puro: ~2.5s | NumPy: ~0.03s (80x más rápido)

# Ufuncs personalizados
def custom_exp(x):
    return np.where(x > 0, np.exp(x), 0)

vectorized_exp = np.frompyfunc(custom_exp, 1, 1)

# Structured arrays — como tablas de base de datos
dt = np.dtype([
    ("nombre", "U20"),
    ("edad", "i4"),
    ("salario", "f8")
])
empleados = np.array([
    ("Ana", 30, 50000.0),
    ("Luis", 35, 60000.0),
], dtype=dt)
print(empleados["salario"])  # [50000. 60000.]
```

### Memoria y Rendimiento
```python
# Memoria compartida entre arrays
a = np.zeros((1000, 1000))
b = a[:500, :500]  # Vista, no copia
b[0, 0] = 999
print(a[0, 0])  # 999 — cambió en a también

# Contiguous arrays — rendimiento
c = np.ascontiguousarray(a)  # Garantiza memoria contigua

# np.einsum — notación de Einstein para operaciones tensoriales
A = np.random.randn(3, 4)
B = np.random.randn(4, 5)
C = np.einsum("ij,jk->ik", A, B)  # Multiplicación de matrices
trace = np.einsum("ii->", np.random.randn(3, 3))  # Traza
```

---

## 2. Pandas 3.x — Análisis de Datos

### Arquitectura: Extension Arrays
```python
import pandas as pd

# Pandas 3.x usa Extension arrays internamente
df = pd.DataFrame({
    "nombre": pd.array(["Ana", "Luis"], dtype="string"),
    "edad": pd.array([30, 35], dtype="Int64"),  # Nullable integer
    "activo": pd.array([True, False], dtype="boolean"),
    "fecha": pd.to_datetime(["2024-01-15", "2024-03-20"])
})

# NA propagation nativo — no más NaN donde no debería haberlos
print(df.dtypes)
# nombre     string
# edad       Int64
# activo     boolean
# fecha      datetime64[ns]
```

### Operaciones Avanzadas
```python
# Window functions
df["promedio_movil"] = df["ventas"].rolling(window=7).mean()
df["zscore"] = df["ventas"].transform(lambda x: (x - x.mean()) / x.std())

# GroupBy con múltiples agregaciones
resumen = df.groupby("categoria").agg(
    ventas_total=("ventas", "sum"),
    ventas_media=("ventas", "mean"),
    desviacion=("ventas", "std"),
    n_transacciones=("ventas", "count"),
    productos_unicos=("producto", "nunique")
).reset_index()

# Merge estrategias
pd.merge(df_izq, df_der, on="id", how="left")   # Left join
pd.merge(df_izq, df_der, left_on="key", right_on="id")

# Cross-tabulation
ct = pd.crosstab(df["region"], df["producto"], margins=True)

# Pivot tables
pivot = df.pivot_table(
    values="ventas",
    index="region",
    columns="mes",
    aggfunc="sum",
    fill_value=0,
    margins=True
)

# String operations (vectorized)
df["nombre_limpio"] = (
    df["nombre"]
    .str.strip()
    .str.lower()
    .str.replace(r"[^a-zA-Záéíóú]", "", regex=True)
)

# Date operations
df["año"] = df["fecha"].dt.year
df["dia_semana"] = df["fecha"].dt.day_name()
df["semana"] = df["fecha"].dt.isocalendar().week
```

---

## 3. Polars 2.x — DataFrame Ultrarrápido (Rust-backed)

```python
import polars as pl

# Polars es 10-50x más rápido que Pandas para operaciones comunes
df = pl.scan_csv("datos_masivos.csv")  # Lazy evaluation — no carga en memoria

# Query con lazy evaluation
resultado = (
    df
    .filter(pl.col("edad") > 18)
    .group_by("ciudad")
    .agg([
        pl.col("salario").mean().alias("salario_medio"),
        pl.col("nombre").count().alias("n_personas"),
        pl.col("salario").quantile(0.95).alias("percentil_95"),
    ])
    .sort("salario_medio", descending=True)
    .collect()  # Ejecuta todo el plan optimizado
)

# Expressions — framework de transformación
df = pl.DataFrame({
    "x": [1, 2, 3, 4, 5],
    "y": [10, 20, 30, 40, 50],
})

# Operaciones encadenadas
resultado = df.with_columns([
    (pl.col("x") * 2).alias("x_doble"),
    (pl.col("x") + pl.col("y")).alias("suma"),
    pl.col("y").rolling_mean(window_size=3).alias("media_movil"),
]).filter(
    pl.col("suma") > 20
).select(["x", "suma", "media_movil"])

# Join y concat
df_merged = df1.join(df2, on="id", how="left")
df_concat = pl.concat([df1, df2], how="vertical")

# UDFs con PyArrow backend
@pl.api.register_expr_namespace("stats")
class Stats:
    def __init__(self, expr: pl.Expr):
        self._expr = expr

    def zscore(self):
        return (self._expr - self._expr.mean()) / self._expr.std()

df = df.with_columns(
    pl.col("valor").stats.zscore().alias("zscore")
)
```

---

## 4. Exploratory Data Analysis (EDA) Completo

```python
import pandas as pd
import numpy as np
from scipy import stats

def eda_completo(df: pd.DataFrame) -> dict:
    """EDA automatizado de nivel producción."""
    reporte = {}

    # 1. Forma y tipos
    reporte["forma"] = df.shape
    reporte["tipos"] = df.dtypes.to_dict()

    # 2. Valores nulos
    nulos = df.isnull().sum()
    reporte["nulos_por_columna"] = nulos[nulos > 0].to_dict()
    reporte["pct_nulos"] = (nulos / len(df) * 100).round(2).to_dict()

    # 3. Duplicados
    reporte["duplicados"] = df.duplicated().sum()

    # 4. Estadísticas numéricas
    numericos = df.select_dtypes(include=[np.number])
    reporte["stats_numericos"] = numericos.describe().to_dict()

    # 5. Distribución (skewness, kurtosis)
    for col in numericos.columns:
        reporte[f"skew_{col}"] = stats.skew(numericos[col].dropna())
        reporte[f"kurtosis_{col}"] = stats.kurtosis(numericos[col].dropna())

    # 6. Correlaciones
    if len(numericos.columns) > 1:
        reporte["correlaciones"] = numericos.corr().to_dict()

    # 7. Outliers (IQR method)
    outliers = {}
    for col in numericos.columns:
        q1 = numericos[col].quantile(0.25)
        q3 = numericos[col].quantile(0.75)
        iqr = q3 - q1
        outliers[col] = int(((numericos[col] < q1 - 1.5*iqr) | (numericos[col] > q3 + 1.5*iqr)).sum())
    reporte["outliers"] = outliers

    return reporte
```

---

## 5. Machine Learning — Pipeline Completo

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score, GridSearchCV
from sklearn.metrics import (
    classification_report, confusion_matrix,
    roc_auc_score, precision_recall_curve
)
import numpy as np

# Pipeline completo
def crear_pipeline(X_train, y_train):
    # Columnas por tipo
    numericas = X_train.select_dtypes(include=[np.number]).columns
    categoricas = X_train.select_dtypes(include=["object", "category"]).columns

    # Preprocessing
    pipeline_numerico = Pipeline([
        ("imputer", SimpleImputer(strategy="median")),
        ("scaler", StandardScaler()),
    ])

    pipeline_categorico = Pipeline([
        ("imputer", SimpleImputer(strategy="most_frequent")),
        ("encoder", OneHotEncoder(handle_unknown="ignore", sparse_output=False)),
    ])

    preprocessor = ColumnTransformer([
        ("num", pipeline_numerico, numericas),
        ("cat", pipeline_categorico, categoricas),
    ])

    # Modelo completo
    pipeline = Pipeline([
        ("preprocessor", preprocessor),
        ("classifier", GradientBoostingClassifier(
            n_estimators=200,
            learning_rate=0.1,
            max_depth=5,
            random_state=42
        )),
    ])

    # Cross-validation
    scores = cross_val_score(pipeline, X_train, y_train, cv=5, scoring="roc_auc")
    print(f"AUC-ROC: {scores.mean():.4f} (+/- {scores.std():.4f})")

    return pipeline

# Feature Engineering avanzado
from sklearn.preprocessing import PolynomialFeatures
from sklearn.feature_selection import SelectKBest, mutual_info_classif

def feature_engineering(df):
    # Interacciones
    poly = PolynomialFeatures(degree=2, interaction_only=True)
    # Select features
    selector = SelectKBest(mutual_info_classif, k=20)
```

---

## 6. Deep Learning — PyTorch 3.x

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader, Dataset
from torch.optim.lr_scheduler import CosineAnnealingLR

# Device agnostic
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# Modelo moderno con PyTorch 3.x
class TransformerClassifier(nn.Module):
    def __init__(self, vocab_size: int, d_model: int = 256,
                 nhead: int = 8, num_layers: int = 4,
                 num_classes: int = 10, dropout: float = 0.1):
        super().__init__()
        self.embedding = nn.Embedding(vocab_size, d_model)
        self.pos_encoding = nn.Parameter(torch.randn(1, 512, d_model) * 0.02)

        encoder_layer = nn.TransformerEncoderLayer(
            d_model=d_model, nhead=nhead,
            dim_feedforward=d_model * 4, dropout=dropout,
            batch_first=True
        )
        self.transformer = nn.TransformerEncoder(encoder_layer, num_layers)
        self.classifier = nn.Sequential(
            nn.Linear(d_model, d_model // 2),
            nn.GELU(),
            nn.Dropout(dropout),
            nn.Linear(d_model // 2, num_classes),
        )

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        seq_len = x.size(1)
        x = self.embedding(x) + self.pos_encoding[:, :seq_len, :]
        x = self.transformer(x)
        x = x.mean(dim=1)  # Global average pooling
        return self.classifier(x)

# Training loop moderno
def train_epoch(model, loader, optimizer, criterion, scheduler):
    model.train()
    total_loss = 0
    correct = 0
    total = 0

    for batch_idx, (data, targets) in enumerate(loader):
        data, targets = data.to(device), targets.to(device)

        optimizer.zero_grad(set_to_none=True)  # Más rápido que zero_grad()
        outputs = model(data)
        loss = criterion(outputs, targets)
        loss.backward()

        # Gradient clipping
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)

        optimizer.step()
        scheduler.step()

        total_loss += loss.item()
        _, predicted = outputs.max(1)
        correct += predicted.eq(targets).sum().item()
        total += targets.size(0)

    return total_loss / len(loader), 100. * correct / total
```

---

## 7. LLM y Agentes IA (Septiembre 2026)

### OpenAI API v2
```python
from openai import OpenAI

client = OpenAI()

# Chat completions
response = client.chat.completions.create(
    model="gpt-5",
    messages=[
        {"role": "system", "content": "Eres un experto en Python."},
        {"role": "user", "content": "Explica closures en Python con ejemplos avanzados."}
    ],
    temperature=0.7,
    max_tokens=2000,
)

print(response.choices[0].message.content)

# Structured outputs
from pydantic import BaseModel

class AnalisisCodigo(BaseModel):
    complejidad: str  # "baja", "media", "alta"
    patrones_detectados: list[str]
    sugerencias: list[str]

response = client.beta.chat.completions.parse(
    model="gpt-5",
    messages=[{"role": "user", "content": f"Analiza:\n```python\n{codigo}\n```"}],
    response_format=AnalisisCodigo,
)
analisis = response.choices[0].message.parsed
```

### RAG (Retrieval-Augmented Generation)
```python
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_community.vectorstores import Chroma
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import PyPDFLoader
from langchain.chains import RetrievalQA

# 1. Cargar documentos
loader = PyPDFLoader("documento.pdf")
docs = loader.load()

# 2. Chunking
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", ". ", " "]
)
chunks = splitter.split_documents(docs)

# 3. Vector store
vectorstore = Chroma.from_documents(
    chunks,
    OpenAIEmbeddings(model="text-embedding-3-large"),
    persist_directory="./chroma_db"
)

# 4. RAG Chain
qa_chain = RetrievalQA.from_chain_type(
    llm=ChatOpenAI(model="gpt-5", temperature=0),
    retriever=vectorstore.as_retriever(search_kwargs={"k": 5}),
    return_source_documents=True,
)

result = qa_chain.invoke({"query": "¿Cuál es la conclusión principal?"})
```

### Agentes Autónomos (LangChain v0.3)
```python
from langchain_openai import ChatOpenAI
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.tools import tool
from langchain_community.tools.tavily_search import TavilySearchResults

# Herramientas personalizadas
@tool
def calcular_expresion(expresion: str) -> str:
    """Evalúa una expresión matemática de forma segura."""
    import ast
    try:
        tree = ast.parse(expresion, mode="eval")
        for node in ast.walk(tree):
            if isinstance(node, ast.Call):
                return "No se permiten llamadas a funciones"
        return str(eval(compile(tree, "<string>", "eval")))
    except Exception as e:
        return f"Error: {e}"

# Herramientas de búsqueda web
search_tool = TavilySearchResults(max_results=3)

# Agente
llm = ChatOpenAI(model="gpt-5", temperature=0)
prompt = ChatPromptTemplate.from_messages([
    ("system", "Eres un asistente experto. Usa las herramientas disponibles."),
    MessagesPlaceholder("chat_history", optional=True),
    ("human", "{input}"),
    MessagesPlaceholder("agent_scratchpad"),
])

agent = create_tool_calling_agent(llm, [search_tool, calcular_expresion], prompt)
executor = AgentExecutor(agent=agent, tools=[search_tool, calcular_expresion], verbose=True)

result = executor.invoke({"input": "¿Cuánto es 2^100? Y busca las últimas noticias de Python"})
```
