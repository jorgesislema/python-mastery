# Tendencias y Librerías 2025-2026 (Nivel PhD, Septiembre 2026)

## 1. Polars 2.x — El DataFrame del Futuro

```python
import polars as pl

# Polars 2.x: Lazy by default, Rust-powered
df = pl.scan_csv("datos.csv")  # No carga en memoria

# Query optimizado automáticamente
resultado = (
    df
    .filter(pl.col("edad") > 18)
    .group_by("ciudad")
    .agg([
        pl.col("salario").mean().alias("salario_medio"),
        pl.col("salario").quantile(0.95).alias("p95"),
    ])
    .sort("salario_medio", descending=True)
    .collect()  # Optimiza y ejecuta todo el plan
)

# Streaming — procesa datasets de TB
df = pl.scan_csv("datos_masivos.csv")
df.sink_parquet("output.parquet")  # Procesa sin cargar en RAM

# Funciones PyArrow nativas
df = df.with_columns([
    pl.col("fecha").str.to_datetime(),
    pl.col("texto").str.split(" ").list.len().alias("n_palabras"),
])
```

---

## 2. PyTorch 3.x + Torch compile

```python
import torch
import torch.nn as nn

# Torch compile — compilación automática (PyTorch 2.x+)
class Modelo(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(784, 256)
        self.fc2 = nn.Linear(256, 10)

    def forward(self, x):
        return self.fc2(torch.relu(self.fc1(x)))

# Compilar para 20-40% más de rendimiento
model = torch.compile(Modelo(), mode="reduce-overhead")

# torch.export — serialización de modelos
exported = torch.export.export(model, (torch.randn(1, 784),))
torch.export.save(exported, "modelo.pt2")
```

---

## 3. LangChain v0.3 + LangGraph — Agentes Complejos

```python
from langchain_openai import ChatOpenAI
from langgraph.graph import StateGraph, END
from langgraph.prebuilt import ToolNode
from typing import TypedDict, Annotated
import operator

# Estado del agente
class EstadoAgente(TypedDict):
    mensajes: Annotated[list, operator.add]
    siguiente_paso: str

# Grafo de agentes con LangGraph
graph = StateGraph(EstadoAgente)

# Nodos
llm = ChatOpenAI(model="gpt-5")

def razonar(state):
    respuesta = llm.invoke(state["mensajes"])
    return {"mensajes": [respuesta]}

graph.add_node("razonar", razonar)
graph.add_node("herramientas", ToolNode([search_tool, calculator]))

# Condiciones
def decidir(state):
    ultimo = state["mensajes"][-1]
    if hasattr(ultimo, "tool_calls") and ultimo.tool_calls:
        return "herramientas"
    return END

graph.add_conditional_edges("razonar", decidir)
graph.add_edge("herramientas", "razonar")
graph.set_entry_point("razonar")

app = graph.compile()
resultado = app.invoke({"mensajes": [("user", "Analiza las ventas de Q3")]})
```

---

## 4. MCP (Model Context Protocol) — Septiembre 2026

```python
# MCP — protocolo estándar para conectar LLMs con herramientas
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# Servidor MCP
server_params = StdioServerParameters(
    command="python",
    args=["mi_servidor_mcp.py"],
)

async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()

        # Listar herramientas disponibles
        tools = await session.list_tools()

        # Usar herramienta
        resultado = await session.call_tool(
            "buscar_datos",
            arguments={"query": "ventas Q3 2026"}
        )
```

---

## 5. Pydantic v3 — Validación de Datos

```python
from pydantic import BaseModel, Field, field_validator, model_validator
from typing import Annotated, Literal
from datetime import datetime

class Configuracion(BaseModel):
    model_config = ConfigDict(
        str_strip_whitespace=True,
        validate_default=True,
        frozen=True,
    )

    host: str = Field(..., pattern=r"^https?://.+")
    puerto: Annotated[int, Field(ge=1, le=65535)]
    debug: bool = False
    entorno: Literal["dev", "staging", "prod"]

    @field_validator("host")
    @classmethod
    def validar_host(cls, v: str) -> str:
        if not v.startswith(("http://", "https://")):
            raise ValueError("Host debe empezar con http:// o https://")
        return v.lower()

    @model_validator(mode="after")
    def validar_produccion(self) -> "Configuracion":
        if self.entorno == "prod" and self.debug:
            raise ValueError("Debug no puede estar activo en producción")
        return self
```

---

## 6. Ruff — Linter y Formatter en Rust

```bash
# Instalar
pip install ruff

# Lint
ruff check . --fix

# Format
ruff format .

# Configuración en pyproject.toml
[tool.ruff]
target-version = "py313"
line-length = 88

[tool.ruff.lint]
select = [
    "E",    # pycodestyle errors
    "W",    # pycodestyle warnings
    "F",    # pyflakes
    "I",    # isort
    "N",    # pep8-naming
    "UP",   # pyupgrade
    "B",    # flake8-bugbear
    "A",    # flake8-builtins
    "SIM",  # flake8-simplify
    "TCH",  # flake8-type-checking
    "RUF",  # ruff-specific
]
```

---

## 7. Mojo — Python para HPC

```mojo
# Mojo: superset de Python, 68,000x más rápido para numeración
from math import sqrt

fn distancia[x: Float32, y: Float32]() -> Float32:
    return sqrt(x*x + y*y)

# Compila a nativo, sin GIL, con GPU support
# Compatibilidad parcial con Python — importa librerías Python directamente
```

---

## 8. uv — El Gestor de Paquetes del Futuro

```bash
# 10-100x más rápido que pip
uv init mi-proyecto
uv add numpy pandas scikit-learn
uv add --dev pytest ruff mypy
uv pip compile requirements.in -o requirements.txt
uv sync
uv run python main.py

# Crear entorno con versión específica
uv python install 3.15
uv venv --python 3.15
```

---

## 9. Trending en 2026

| Tecnología | Estado | Uso Principal |
|------------|--------|---------------|
| **Free-threaded Python** | Estable | Paralelismo real sin GIL |
| **Torch compile** | Production | Compilación automática de modelos |
| **LangGraph** | Production | Agentes con grafo de estados |
| **MCP** | Estándar | Protocolo LLM ↔ herramientas |
| **Polars 2.x** | Production | DataFrames en Rust |
| **uv** | Estándar | Gestión de paquetes |
| **Ruff** | Estándar | Linting/formatting |
| **Pydantic v3** | Production | Validación de datos |
| **Mojo** | Stable | HPC con sintaxis Python-like |
