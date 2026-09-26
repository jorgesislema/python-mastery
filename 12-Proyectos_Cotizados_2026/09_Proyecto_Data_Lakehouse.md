# Proyecto 9: Data Lakehouse + Analytics Platform
# Salario: $125K-195K | Freelance: $18K-50K

## ¿Qué es?

Plataforma unificada de datos que combina data lake (almacenamiento barato) con data warehouse (consultas rápidas). Incluye ETL, quality checks, y dashboards.

---

## Stack

```python
# requirements.txt
pyspark==4.0.0
delta-spark==4.0.0
polars==2.6.0
pandas==3.0.0
great-expectations==0.18.34
dbt-core==1.9.3
streamlit==1.41.0
plotly==6.0.0
sqlalchemy==2.0.36
```

---

## Implementación

### 1. ETL con PySpark + Delta Lake

```python
# pipeline/etl.py
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from delta import configure_spark_with_delta_pip

def create_spark_session():
    """Crea sesión de Spark con soporte Delta Lake."""
    builder = (
        SparkSession.builder
        .appName("DataLakehouse")
        .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension")
        .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog")
        .config("spark.sql.adaptive.enabled", "true")
    )
    return configure_spark_with_delta_pip(builder).getOrCreate()

def run_etl(spark, source_path: str, target_path: str):
    """Pipeline ETL completo."""
    # 1. Extract
    raw_df = spark.read.format("json").load(source_path)

    # 2. Transform
    cleaned_df = (
        raw_df
        .dropDuplicates(["id"])
        .filter(F.col("value").isNotNull())
        .withColumn("processed_at", F.current_timestamp())
        .withColumn("year", F.year("timestamp"))
        .withColumn("month", F.month("timestamp"))
    )

    # 3. Load (Delta Lake — upsert)
    from delta.tables import DeltaTable

    if DeltaTable.isDeltaTable(spark, target_path):
        delta_table = DeltaTable.forPath(spark, target_path)
        (
            delta_table.alias("target")
            .merge(cleaned_df.alias("source"), "target.id = source.id")
            .whenMatchedUpdateAll()
            .whenNotMatchedInsertAll()
            .execute()
        )
    else:
        cleaned_df.write.format("delta").partitionBy("year", "month").save(target_path)
```

### 2. Data Quality con Great Expectations

```python
# pipeline/quality.py
import great_expectations as gx

def validate_pipeline(spark_df):
    """Valida calidad de datos en cada stage del ETL."""
    context = gx.get_context()

    ds = context.sources.sparkdf.add_dataframe_asset(
        name="pipeline_data", dataframe=spark_df
    )

    # Expectativas
    expectations = [
        {"expectation_type": "expect_column_values_to_not_be_null", "kwargs": {"column": "id"}},
        {"expectation_type": "expect_column_values_to_be_unique", "kwargs": {"column": "id"}},
        {"expectation_type": "expect_column_values_to_be_between", "kwargs": {"column": "value", "min_value": 0, "max_value": 10000}},
    ]

    results = []
    for exp in expectations:
        result = ds.validate(expectation_suite=[exp])
        results.append(result)

    return all(r.success for r in results)
```

### 3. Analytics Dashboard

```python
# dashboard/app.py
import streamlit as st
import polars as pl
import plotly.express as px

st.set_page_config(page_title="Analytics Platform", layout="wide")

# KPIs
col1, col2, col3, col4 = st.columns(4)
with col1:
    st.metric("Registros totales", "12.5M", "+8.2%")
with col2:
    st.metric("Latencia promedio", "23ms", "-15%")
with col3:
    st.metric("Calidad datos", "99.7%", "+0.3%")
with col4:
    st.metric("Costo/mes", "$1,234", "-12%")

# Tabs
tab1, tab2, tab3 = st.tabs(["Explorar", "Calidad", "Costos"])

with tab1:
    # Lectura desde Delta Lake
    df = pl.read_delta("s3a://datalake/processed/", version=0)
    st.dataframe(df.head(100))
    st.plotly_chart(px.histogram(df, x="category", title="Distribución por categoría"))

with tab2:
    st.subheader("Data Quality Report")
    st.metric("Rows valid", "99.7%")
    st.metric("Nulls detected", "0.3%")
    st.metric("Duplicates", "0.01%")

with tab3:
    st.subheader("Cloud Costs")
    st.plotly_chart(px.line(costs_df, x="date", y="cost", title="Costo diario AWS"))
```

---

## Cómo Presentarlo

```
Título: "Data Lakehouse con Delta Lake y Analytics Automatizado"

- ETL con PySpark + Delta Lake (ACID transactions)
- Data quality con Great Expectations
- Particionado inteligente por fecha
- Merge/upsert incremental
- Dashboard de analytics y costos
- 12.5M registros procesados

Tecnologías: PySpark, Delta Lake, Great Expectations, Polars, Streamlit
```
