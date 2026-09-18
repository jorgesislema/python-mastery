# Ingeniería de Datos (Nivel PhD, Septiembre 2026)

## 1. ETL con Apache Airflow 3.x

### DAG (Directed Acyclic Graph)
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.apache.spark.operators.spark_submit import SparkSubmitOperator
from airflow.providers.amazon.aws.transfers.s3_to_redshift import S3ToRedshiftOperator
from datetime import datetime, timedelta, timezone

default_args = {
    "owner": "data-engineer",
    "retries": 3,
    "retry_delay": timedelta(minutes=5),
    "email_on_failure": True,
    "email": ["alertas@empresa.com"],
}

with DAG(
    dag_id="pipeline_ventas_diario",
    default_args=default_args,
    schedule_interval="0 6 * * *",
    start_date=datetime(2024, 1, 1),
    catchup=False,
    max_active_runs=1,
    tags=["ventas", "etl", "diario"],
) as dag:

    extraer = SparkSubmitOperator(
        task_id="extraer_datos",
        application="jobs/extract_ventas.py",
        conn_id="spark_default",
        conf={"spark.sql.shuffle.partitions": "200"},
    )

    transformar = PythonOperator(
        task_id="transformar",
        python_callable=transformar_ventas,
        op_kwargs={"fecha": "{{ ds }}"},
    )

    cargar = S3ToRedshiftOperator(
        task_id="cargar_redshift",
        schema="ventas",
        table="ventas_diarias",
        s3_bucket="data-lake",
        s3_key="processed/ventas/{{ ds }}/ventas.parquet",
        copy_options=["FORMAT AS PARQUET"],
    )

    extraer >> transformar >> cargar
```

---

## 2. Apache Spark 4.0 con PySpark

```python
from pyspark.sql import SparkSession, Window
from pyspark.sql import functions as F
from pyspark.sql.types import StructType, StructField, StringType, IntegerType, DoubleType

# Spark 4.0 — conectores nativos
spark = (
    SparkSession.builder
    .appName("AnalisisVentas")
    .config("spark.sql.adaptive.enabled", "true")  # AQE — optimización adaptativa
    .config("spark.sql.adaptive.coalescePartitions.enabled", "true")
    .config("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
    .getOrCreate()
)

# Leer con esquema explícito
schema = StructType([
    StructField("transaction_id", StringType(), False),
    StructField("customer_id", StringType(), False),
    StructField("product_id", StringType(), False),
    StructField("quantity", IntegerType(), False),
    StructField("price", DoubleType(), False),
    StructField("timestamp", StringType(), False),
])

df = (
    spark.read
    .format("delta")  # Delta Lake 4.0
    .load("s3a://data-lake/raw/ventas/")
)

# Transformaciones con Window functions
window_cliente = (
    Window
    .partitionBy("customer_id")
    .orderBy(F.col("timestamp").cast("timestamp"))
)

df_enriquecido = (
    df
    .withColumn("fecha", F.to_date("timestamp"))
    .withColumn("monto_total", F.col("quantity") * F.col("price"))
    .withColumn("compra_anterior", F.lag("monto_total").over(window_cliente))
    .withColumn("variacion", F.col("monto_total") - F.col("compra_anterior"))
    .withColumn("running_total", F.sum("monto_total").over(window_cliente))
    .withColumn("rank_cliente", F.row_number().over(
        Window.partitionBy("customer_id").orderBy(F.desc("monto_total"))
    ))
)

# Escritura incremental (Delta Lake)
(
    df_enriquecido
    .write
    .format("delta")
    .mode("merge")
    .option("mergeSchema", "true")
    .partitionBy("fecha")
    .save("s3a://data-lake/processed/ventas/")
)

# SQL directo
df.createOrReplaceTempView("ventas")
spark.sql("""
    SELECT customer_id, SUM(monto_total) as total_gastado
    FROM ventas
    WHERE fecha >= '2024-01-01'
    GROUP BY customer_id
    ORDER BY total_gastado DESC
    LIMIT 100
""").show()
```

---

## 3. SQL y NoSQL con Python

### SQLAlchemy 2.x (ORM Moderno)
```python
from sqlalchemy import create_engine, Column, Integer, String, Float, DateTime, ForeignKey, text, select, func
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, relationship, Session
from datetime import datetime, timezone

engine = create_engine("postgresql://user:pass@localhost:5432/db", pool_size=20)

class Base(DeclarativeBase):
    pass

class Cliente(Base):
    __tablename__ = "clientes"

    id: Mapped[int] = mapped_column(primary_key=True)
    nombre: Mapped[str] = mapped_column(String(100))
    email: Mapped[str] = mapped_column(String(255), unique=True)
    creado_en: Mapped[datetime] = mapped_column(default=lambda: datetime.now(timezone.utc))
    pedidos: Mapped[list["Pedido"]] = relationship(back_populates="cliente")

class Pedido(Base):
    __tablename__ = "pedidos"

    id: Mapped[int] = mapped_column(primary_key=True)
    cliente_id: Mapped[int] = mapped_column(ForeignKey("clientes.id"))
    monto: Mapped[float] = mapped_column(Float)
    cliente: Mapped["Cliente"] = relationship(back_populates="pedidos")

# Consultas modernas (2.x style)
with Session(engine) as session:
    # Select
    clientes = session.execute(
        select(Cliente).where(Cliente.nombre.ilike("%ana%")).limit(10)
    ).scalars().all()

    # Insert
    nuevo = Cliente(nombre="Luis", email="luis@ejemplo.com")
    session.add(nuevo)
    session.commit()

    # Subqueries
    subq = (
        select(Pedido.cliente_id, func.sum(Pedido.monto).alias("total"))
        .group_by(Pedido.cliente_id)
        .subquery()
    )
    resultado = session.execute(
        select(Cliente, subq.c.total)
        .join(subq, Cliente.id == subq.c.cliente_id)
    ).all()
```

### MongoDB con Motor (async)
```python
from motor.motor_asyncio import AsyncIOMotorClient
from pymongo import IndexModel, ASCENDING

client = AsyncIOMotorClient("mongodb://localhost:27017")
db = client.empresa
collection = db.productos

# Índices
await collection.create_indexes([
    IndexModel([("nombre", ASCENDING), ("precio", ASCENDING)]),
])

# CRUD async
await collection.insert_one({
    "nombre": "Laptop",
    "precio": 999.99,
    "tags": ["electrónica", "computadora"]
})

# Aggregation pipeline
pipeline = [
    {"$match": {"precio": {"$gt": 500}}},
    {"$group": {"_id": "$categoria", "promedio": {"$avg": "$precio"}}},
    {"$sort": {"promedio": -1}},
]
resultados = await collection.aggregate(pipeline).to_list(length=100)
```

---

## 4. Streaming con Kafka y Python

```python
from aiokafka import AIOKafkaProducer, AIOKafkaConsumer
import json
from dataclasses import dataclass, asjson

@dataclass
class Evento:
    tipo: str
    usuario_id: str
    datos: dict
    timestamp: str

# Productor async
producer = AIOKafkaProducer(
    bootstrap_servers="localhost:9092",
    value_serializer=lambda v: json.dumps(v).encode(),
)

async def publicar_evento(topic: str, evento: Evento):
    await producer.start()
    try:
        await producer.send_and_wait(topic, value=asjson(evento))
    finally:
        await producer.stop()

# Consumidor con procesamiento
async def consumir_eventos(topic: str):
    consumer = AIOKafkaConsumer(
        topic,
        bootstrap_servers="localhost:9092",
        group_id="procesadores",
        auto_offset_reset="earliest",
    )
    await consumer.start()
    try:
        async for msg in consumer:
            evento = json.loads(msg.value)
            await procesar(evento)
    finally:
        await consumer.stop()
```

---

## 5. Delta Lake y Lakehouse

```python
# Delta Lake 4.0 — transacciones ACID en data lakes
from deltalake import DeltaTable, write_deltalake
import polars as pl

# Leer tabla Delta
dt = DeltaTable("s3a://data-lake/tabla_delta")

# Histórico de versiones
versions = dt.history()

# Time travel
df_v0 = dt.load_version(0)
df_hoy = dt.to_polars()

# Merge (upsert)
nuevos_datos = pl.DataFrame({
    "id": [1, 2, 3],
    "valor": [100, 200, 300],
    "actualizado": ["2024-01-15", "2024-01-15", "2024-01-15"],
})

(
    dt.merge(
        target=dt.to_polars(),
        source=nuevos_datos,
        predicate="target.id = source.id"
    )
    .when_matched_update_all()
    .when_not_matched_insert_all()
    .execute()
)

# Write
write_deltalake(
    "s3a://data-lake/nueva_tabla",
    data=nuevos_datos.to_arrow(),
    mode="overwrite",
    partition_by=["fecha"],
)
```

---

## 6. MLOps con MLflow

```python
import mlflow
import mlflow.sklearn
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, f1_score

mlflow.set_tracking_uri("http://localhost:5000")

# Experimento
with mlflow.start_run(run_name="gradient_boosting_v2"):
    # Parámetros
    params = {
        "n_estimators": 200,
        "learning_rate": 0.1,
        "max_depth": 5,
    }
    mlflow.log_params(params)

    # Entrenar
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
    model = GradientBoostingClassifier(**params, random_state=42)
    model.fit(X_train, y_train)

    # Métricas
    y_pred = model.predict(X_test)
    mlflow.log_metrics({
        "accuracy": accuracy_score(y_test, y_pred),
        "f1": f1_score(y_test, y_pred, average="weighted"),
    })

    # Modelo
    mlflow.sklearn.log_model(model, "modelo")

    # Artifacts
    mlflow.log_artifact("confusion_matrix.png")
```

---

## 7. Docker para Data Pipelines

```dockerfile
FROM python:3.13-slim

WORKDIR /app

# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    gcc g++ libffi-dev \
    && rm -rf /var/lib/apt/lists/*

COPY pyproject.toml uv.lock ./
RUN pip install uv && uv sync --no-dev

COPY src/ ./src/

# Multi-stage build para imagen ligera
FROM python:3.13-slim AS runtime
COPY --from=0 /app/.venv /app/.venv
COPY --from=0 /app/src /app/src
ENV PATH="/app/.venv/bin:$PATH"

CMD ["python", "-m", "src.main"]
```

```yaml
# docker-compose.yml
services:
  airflow:
    image: apache/airflow:3.0.0
    environment:
      AIRFLOW__CORE__EXECUTOR: LocalExecutor
    volumes:
      - ./dags:/opt/airflow/dags
    ports:
      - "8080:8080"

  spark:
    image: bitnami/spark:4.0
    environment:
      - SPARK_MODE=master
    ports:
      - "7077:7077"

  postgres:
    image: postgres:17
    environment:
      POSTGRES_DB: datawarehouse
    volumes:
      - pgdata:/var/lib/postgresql/data
```
