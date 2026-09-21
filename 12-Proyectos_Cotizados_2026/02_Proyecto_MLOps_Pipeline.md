# Proyecto 2: Pipeline MLOps Completo
# Salario: $130K-200K | Freelance: $20K-60K

## ¿Qué es?

MLOps (Machine Learning Operations) es la ingeniería detrás del ML en producción. Un pipeline MLOps cubre: experiment tracking, model training, validation, deployment, monitoring, y retraining automático.

**¿Por qué es tan cotizado?**
- El 87% de modelos ML nunca llegan a producción (Gartner)
- Las empresas necesitan gente que sepa operationalizar ML
- Combina ML + DevOps + Data Engineering = perfil raro y valioso

---

## Arquitectura

```
┌─────────────────────────────────────────────────────────────┐
│                    MLOps PIPELINE                            │
│                                                              │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│  │  Data     │───▶│  Train   │───▶│ Evaluate │              │
│  │  Version  │    │  Model   │    │  Model   │              │
│  │  (DVC)    │    │ (MLflow) │    │ (Custom) │              │
│  └──────────┘    └──────────┘    └────┬─────┘              │
│                                       │                     │
│                              ┌────────▼────────┐           │
│                              │  Register Model │           │
│                              │  (MLflow)       │           │
│                              └────────┬────────┘           │
│                                       │                     │
│                    ┌──────────────────┼──────────────┐     │
│                    │                  │              │     │
│              ┌─────▼─────┐    ┌──────▼──────┐ ┌────▼───┐ │
│              │  Deploy    │    │   Monitor   │ │Retrain │ │
│              │  (BentoML) │    │ (Evidently) │ │(Airflow│ │
│              └───────────┘    └─────────────┘ └────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Stack Actualizado 2026

```python
# requirements.txt
mlflow==2.21.0
scikit-learn==1.6.1
xgboost==2.1.3
lightgbm==4.6.0
optuna==4.2.0
evidently==0.6.5
bentoml==1.4.5
great-expectations==0.18.34
apache-airflow==3.0.0
dvc==3.61.0
docker==7.1.0
redis==5.2.1
prometheus-client==0.21.1
```

---

## Implementación Completa

### 1. Data Validation con Great Expectations

```python
# pipeline/data_validation.py
import great_expectations as gx
import pandas as pd

def validate_data(df: pd.DataFrame) -> dict:
    """Valida calidad de datos antes de entrenar."""
    context = gx.get_context()

    ds = context.sources.pandas_default.add_dataframe_asset(
        name="training_data", dataframe=df
    )
    batch_def = ds.add_batch_definition_whole_dataframe("batch")
    batch = batch_def.get_batch(batch_parameters={"dataframe": df})

    # Expectativas
    results = context.run_checkpoint(
        checkpoint_name="training_checkpoint",
        batch_request=batch.batch_request,
    )

    return {
        "success": results.success,
        "statistics": results.statistics,
    }
```

### 2. Experiment Tracking con MLflow

```python
# pipeline/training.py
import mlflow
import mlflow.sklearn
import optuna
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import cross_val_score
from sklearn.metrics import accuracy_score, f1_score, roc_auc_score
import numpy as np

mlflow.set_tracking_uri("http://localhost:5000")

def objective(trial, X, y):
    """Optuna objective function."""
    params = {
        "n_estimators": trial.suggest_int("n_estimators", 50, 500),
        "max_depth": trial.suggest_int("max_depth", 2, 15),
        "learning_rate": trial.suggest_float("learning_rate", 0.001, 0.3, log=True),
        "subsample": trial.suggest_float("subsample", 0.5, 1.0),
        "min_samples_split": trial.suggest_int("min_samples_split", 2, 20),
    }

    model = GradientBoostingClassifier(**params, random_state=42)
    scores = cross_val_score(model, X, y, cv=5, scoring="roc_auc")
    return scores.mean()

def train_best_model(X_train, y_train, X_test, y_test, n_trials=50):
    """Entrena el mejor modelo con Optuna + MLflow."""
    study = optuna.create_study(direction="maximize")
    study.optimize(lambda trial: objective(trial, X_train, y_train), n_trials=n_trials)

    best_params = study.best_params

    with mlflow.start_run(run_name="best_model"):
        # Log params
        mlflow.log_params(best_params)

        # Train final model
        model = GradientBoostingClassifier(**best_params, random_state=42)
        model.fit(X_train, y_train)

        # Evaluate
        y_pred = model.predict(X_test)
        y_prob = model.predict_proba(X_test)[:, 1]

        metrics = {
            "accuracy": accuracy_score(y_test, y_pred),
            "f1": f1_score(y_test, y_pred, average="weighted"),
            "roc_auc": roc_auc_score(y_test, y_prob),
        }
        mlflow.log_metrics(metrics)

        # Log model
        mlflow.sklearn.log_model(model, "model")

        # Log artifact
        import json
        with open("best_params.json", "w") as f:
            json.dump(best_params, f)
        mlflow.log_artifact("best_params.json")

    return model, metrics
```

### 3. Model Monitoring con Evidently

```python
# pipeline/monitoring.py
from evidently.report import Report
from evidently.metric_preset import (
    DataDriftPreset, TargetDriftPreset,
    ClassificationPreset, DataQualityPreset
)
import pandas as pd

def monitor_model(reference_data, current_data, model=None):
    """Genera reporte de monitoreo completo."""
    report = Report(metrics=[
        DataDriftPreset(),
        DataQualityPreset(),
        TargetDriftPreset(),
    ])

    report.run(
        reference_data=reference_data,
        current_data=current_data,
    )

    report.save_html("monitoring_report.html")

    # Extraer métricas clave
    result = report.as_dict()
    drift_detected = result["metrics"][0]["result"]["dataset_drift"]

    return {
        "drift_detected": drift_detected,
        "report_path": "monitoring_report.html",
        "details": result,
    }
```

### 4. Model Serving con BentoML

```python
# service/bento_service.py
import bentoml
import numpy as np
from sklearn.ensemble import GradientBoostingClassifier

@bentoml.service(resources={"cpu": "2", "memory": "4Gi"})
class MLService:
    def __init__(self):
        self.model = bentoml.sklearn.get("best_model:latest").load_model()

    @bentoml.api(batchable=True, max_batch_size=32)
    def predict(self, input_data: np.ndarray) -> np.ndarray:
        return self.model.predict(input_data)

    @bentoml.api(batchable=True, max_batch_size=32)
    def predict_proba(self, input_data: np.ndarray) -> np.ndarray:
        return self.model.predict_proba(input_data)

# Build y save
# bentoml models save MLService
```

### 5. Orchestration con Airflow

```python
# dags/mlops_pipeline.py
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.docker.operators.docker import DockerOperator
from datetime import datetime, timedelta

default_args = {
    "owner": "ml-team",
    "retries": 2,
    "retry_delay": timedelta(minutes=5),
}

with DAG(
    dag_id="mlops_retraining",
    default_args=default_args,
    schedule_interval="0 2 * * 0",  # Domingos a las 2am
    start_date=datetime(2024, 1, 1),
    catchup=False,
) as dag:

    validate = PythonOperator(
        task_id="validate_data",
        python_callable=validate_data_task,
    )

    train = PythonOperator(
        task_id="train_model",
        python_callable=train_model_task,
    )

    evaluate = PythonOperator(
        task_id="evaluate_model",
        python_callable=evaluate_model_task,
    )

    deploy = DockerOperator(
        task_id="deploy_model",
        image="ml-service:latest",
        command="bentoml serve",
    )

    validate >> train >> evaluate >> deploy
```

---

## Cómo Presentarlo en Portfolio

```
Título: "Pipeline MLOps End-to-End con Retraining Automático"

Descripción:
- Pipeline automatizado de ML: validación → training → evaluación → deploy
- Experiment tracking con MLflow (100+ experimentos registrados)
- Hyperparameter tuning con Optuna (50 trials)
- Model monitoring con Evidently AI (detección de drift)
- Retraining semanal automatizado con Airflow
- Serving con BentoML (batch predictions + real-time)

Resultados:
- Reducción de tiempo de deploy: de 2 semanas a 15 minutos
- Detección automática de data drift
- Retraining automático cuando el modelo degrada >5%
```
