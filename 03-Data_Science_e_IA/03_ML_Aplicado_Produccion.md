# Machine Learning Aplicado — De la Teoría a Producción (Nivel PhD)

## 1. Feature Engineering (La Arte del ML)

### ¿Por qué importa?
```
"Garbage in, garbage out"
Un buen feature engineer puede mejorar el rendimiento más que cambiar de algoritmo.
En competiciones de Kaggle, el 80% del tiempo se dedica a feature engineering.
```

### Tipos de Features

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler, RobustScaler, MinMaxScaler, LabelEncoder, PolynomialFeatures

# 1. NUMÉRICAS
# Escalado
scaler = StandardScaler()          # Media=0, Std=1
robust = RobustScaler()            # Mediana=0, IQR=1 (resistente a outliers)
minmax = MinMaxScaler()            # Rango [0, 1]

# Discretización
df["edad_grupo"] = pd.cut(df["edad"], bins=[0, 18, 35, 60, 100],
                           labels=["joven", "adulto", "adulto_mayor", "mayor"])

# Binning por frecuencia
df["precio_q"] = pd.qcut(df["precio"], q=5, labels=["Muy bajo", "Bajo", "Medio", "Alto", "Muy alto"])

# Log transform (para distribuciones con cola larga)
df["log_salario"] = np.log1p(df["salario"])

# 2. CATEGÓRICAS
# One-Hot Encoding
df_encoded = pd.get_dummies(df, columns=["ciudad"], drop_first=True)

# Label Encoding (para ordinales)
le = LabelEncoder()
df["educacion_encoded"] = le.fit_transform(df["educacion"])

# Target Encoding (para cardinalidad alta)
# Reemplaza categoría con la media del target
target_mean = df.groupby("producto")["ventas"].mean()
df["producto_target"] = df["producto"].map(target_mean)

# Frequency Encoding
freq = df["ciudad"].value_counts() / len(df)
df["ciudad_freq"] = df["ciudad"].map(freq)

# 3. TEMPORALES
df["fecha"] = pd.to_datetime(df["fecha"])
df["año"] = df["fecha"].dt.year
df["mes"] = df["fecha"].dt.month
df["dia_semana"] = df["fecha"].dt.dayofweek
df["es_fin_semana"] = df["dia_semana"].isin([5, 6]).astype(int)
df["trimestre"] = df["fecha"].dt.quarter
df["dia_del_año"] = df["fecha"].dt.dayofyear
df["hora"] = df["fecha"].dt.hour

# Ciclical encoding (para variables que "vuelven")
df["mes_sin"] = np.sin(2 * np.pi * df["mes"] / 12)
df["mes_cos"] = np.cos(2 * np.pi * df["mes"] / 12)

# 4. DE TEXTOS
# TF-IDF
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf = TfidfVectorizer(max_features=1000, stop_words="english")
X_text = tfidf.fit_transform(df["descripcion"])

# Longitud del texto
df["n_palabras"] = df["descripcion"].str.split().str.len()
df["n_caracteres"] = df["descripcion"].str.len()

# 5. DE INTERACCIÓN
# Polynomial features
poly = PolynomialFeatures(degree=2, interaction_only=True)
X_interacciones = poly.fit_transform(X_num)

# Ratio features
df["precio_por_kg"] = df["precio"] / (df["peso"] + 1)
df["ingreso_per_capita"] = df["ingreso"] / (df["familia"] + 1)

# Aggregate features
df["media_precio_cliente"] = df.groupby("cliente_id")["precio"].transform("mean")
df["max_ventas_producto"] = df.groupby("producto_id")["ventas"].transform("max")
```

### Feature Selection

```python
from sklearn.feature_selection import (
    SelectKBest, mutual_info_classif, f_classif,
    RFE, SequentialFeatureSelector
)

# 1. Filter methods (rápidos, independientes del modelo)
selector = SelectKBest(f_classif, k=10)
X_selected = selector.fit_transform(X, y)
print(selector.scores_)  # Puntuación de cada feature

# 2. Wrapper methods (usar el modelo para evaluar)
# Recursive Feature Elimination
rfe = RFE(estimator=model, n_features_to_select=10)
X_rfe = rfe.fit_transform(X, y)

# 3. Embedded methods (regularización)
from sklearn.linear_model import LassoCV
lasso = LassoCV(cv=5).fit(X, y)
importancias = pd.Series(np.abs(lasso.coef_), index=feature_names)
print(importancias.sort_values(ascending=False))

# 4. Feature importance de árboles
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(n_estimators=100).fit(X, y)
importancias = pd.Series(rf.feature_importances_, index=feature_names)
print(importancias.sort_values(ascending=False).head(20))
```

---

## 2. Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
from scipy.stats import randint, uniform
import optuna

# 1. Grid Search (búsqueda exhaustiva)
param_grid = {
    "n_estimators": [100, 200, 300],
    "max_depth": [3, 5, 7, 10],
    "learning_rate": [0.01, 0.05, 0.1, 0.2],
}

grid = GridSearchCV(
    GradientBoostingClassifier(random_state=42),
    param_grid, cv=5, scoring="roc_auc", n_jobs=-1
)
grid.fit(X_train, y_train)
print(f"Best: {grid.best_params_} → AUC: {grid.best_score_:.4f}")

# 2. Random Search (más eficiente en espacio grande)
param_dist = {
    "n_estimators": randint(50, 500),
    "max_depth": randint(2, 20),
    "learning_rate": uniform(0.001, 0.3),
    "subsample": uniform(0.5, 1.0),
}

random_search = RandomizedSearchCV(
    GradientBoostingClassifier(random_state=42),
    param_dist, n_iter=50, cv=5, scoring="roc_auc", n_jobs=-1
)
random_search.fit(X_train, y_train)

# 3. Optuna (Bayesian optimization — más inteligente)
def objective(trial):
    params = {
        "n_estimators": trial.suggest_int("n_estimators", 50, 500),
        "max_depth": trial.suggest_int("max_depth", 2, 20),
        "learning_rate": trial.suggest_float("learning_rate", 0.001, 0.3, log=True),
        "subsample": trial.suggest_float("subsample", 0.5, 1.0),
    }
    model = GradientBoostingClassifier(**params, random_state=42)
    scores = cross_val_score(model, X_train, y_train, cv=5, scoring="roc_auc")
    return scores.mean()

study = optuna.create_study(direction="maximize")
study.optimize(objective, n_trials=100)
print(f"Best: {study.best_params} → AUC: {study.best_value:.4f}")
```

---

## 3. Pipelines de Producción

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
import joblib

# Pipeline completo de producción
def crear_pipeline_produccion(df):
    numericas = df.select_dtypes(include=[np.number]).columns.tolist()
    categoricas = df.select_dtypes(include=["object", "category"]).columns.tolist()

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

    pipeline_completo = Pipeline([
        ("preprocessor", preprocessor),
        ("classifier", GradientBoostingClassifier(
            n_estimators=200, learning_rate=0.1, max_depth=5, random_state=42
        )),
    ])

    return pipeline_completo

# Entrenar y guardar
pipeline = crear_pipeline_produccion(df_train)
pipeline.fit(X_train, y_train)
joblib.dump(pipeline, "modelo_pipeline.joblib")

# Cargar y predecir
pipeline_cargado = joblib.load("modelo_pipeline.joblib")
predicciones = pipeline_cargado.predict(X_nuevos)
```

---

## 4. Model Interpretability (XAI)

```python
import shap
from sklearn.inspection import permutation_importance

# 1. Feature Importance (ya mostrado)
# 2. SHAP Values — explica cada predicción individual
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)

# Resumen global
shap.summary_plot(shap_values, X_test, feature_names=feature_names)

# Explicación individual
shap.force_plot(explainer.expected_value, shap_values[0], X_test.iloc[0])

# Dependencia parcial
shap.dependence_plot("feature_1", shap_values, X_test)

# 3. Permutation Importance (model-agnostic)
result = permutation_importance(model, X_test, y_test, n_repeats=10)
importancias = pd.Series(result.importances_mean, index=feature_names)
print(importancias.sort_values(ascending=False))

# 4. LIME (Local Interpretable Model-Agnostic Explanations)
from lime.lime_tabular import LimeTabularExplainer

explainer = LimeTabularExplainer(
    X_train.values, feature_names=feature_names,
    class_names=["No", "Sí"], mode="classification"
)
explanation = explainer.explain_instance(
    X_test.iloc[0].values, model.predict_proba, num_features=10
)
explanation.show_in_notebook()
```

---

## 5. Detección de Data Drift

```python
from scipy.stats import ks_2samp, chi2_contingency

# KS Test para features numéricas
def detectar_drift(train_data, test_data, feature, alpha=0.05):
    stat, p_value = ks_2samp(train_data[feature], test_data[feature])
    return {
        "feature": feature,
        "statistic": stat,
        "p_value": p_value,
        "drift": p_value < alpha
    }

# Para todas las features
drift_report = []
for col in numericas:
    result = detectar_drift(df_train, df_test, col)
    drift_report.append(result)

drift_df = pd.DataFrame(drift_report)
print(drift_df[drift_df["drift"] == True])  # Features con drift

# Chi² para features categóricas
def drift_categorico(train, test, feature):
    contingency = pd.crosstab(
        pd.Series(train[feature], name="train"),
        pd.Series(test[feature], name="test")
    )
    stat, p_value, _, _ = chi2_contingency(contingency)
    return p_value < 0.05
```

---

## 6. MLOps en Producción

```
CICLO DE VIDA DE UN MODELO:
  Data → Train → Evaluate → Register → Deploy → Monitor → Retrain

Herramientas:
  - MLflow: experiment tracking, model registry
  - Evidently AI: data quality, drift detection
  - BentoML / TorchServe: model serving
  - Airflow / Prefect: orchestration
  - Grafana + Prometheus: monitoring

MONITOREO:
  - Data drift: ¿los datos de producción cambian?
  - Concept drift: ¿la relación features→target cambia?
  - Performance metrics: ¿las métricas bajan?
  - Latency: ¿el modelo responde a tiempo?
  - Errors: ¿aumentan los errores?
```
