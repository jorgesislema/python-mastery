# 📌 Exploración de Datos (EDA) y Transformación (ETL) en Python 🐍

La **Exploración de Datos (EDA)** y la **Extracción, Transformación y Carga (ETL)** son procesos fundamentales en la ciencia de datos y en la ingeniería de datos. Antes de entrenar un modelo de Machine Learning o realizar un análisis, es crucial entender los datos, limpiarlos y transformarlos correctamente. 📊🔍

---

## 1️⃣ ¿Qué es la Exploración de Datos (EDA)? 🤔

EDA (**Exploratory Data Analysis**) es el proceso de analizar y comprender un conjunto de datos a través de:
- **Visualizaciones**: ¿Cómo está distribuida la información? 📊
- **Estadísticas descriptivas**: ¿Cuáles son los valores más comunes? ¿Existen valores atípicos (outliers)? 📈
- **Relaciones entre variables**: ¿Cómo se correlacionan los datos? 🔄
- **Patrones ocultos**: ¿Existen grupos naturales en los datos? 📌

### 🔹 Diferencias entre EDA y ETL

| Característica | EDA (Exploración de Datos) | ETL (Extracción, Transformación y Carga) |
|--------------|------------------------|-------------------------------|
| **Objetivo** | Comprender los datos y encontrar patrones, tendencias y anomalías. | Transformar los datos para hacerlos utilizables en modelos o bases de datos. |
| **Fases** | Inspección, limpieza, visualización, análisis estadístico. | Extracción desde múltiples fuentes, transformación y limpieza, carga en bases de datos. |
| **Herramientas Principales** | Pandas, NumPy, Seaborn, Matplotlib. | Pandas, SQL, PySpark, Airflow. |
| **Ejemplo de Uso** | Verificar valores nulos, detectar outliers, explorar correlaciones. | Convertir formatos de datos, normalizar columnas, cargar datos en una base de datos. |
| **Momento en el Proceso** | Se realiza antes del modelado para conocer los datos. | Se realiza para preparar los datos y hacerlos accesibles para análisis posteriores. |

### 🔹 Pasos Claves en un Análisis EDA:

### 1️⃣ Cargar los Datos 📂
```python
import pandas as pd
import numpy as np

df = pd.read_csv("datos.csv")  # Cargar un archivo CSV
print(df.head())  # Mostrar las primeras filas
```

📌 **Posibles Problemas:**
❌ **El archivo CSV tiene delimitadores diferentes** → Usa `pd.read_csv("archivo.csv", delimiter=';')`  
❌ **Columnas mal nombradas** → Renómbralas con `df.rename(columns={'col_antigua': 'col_nueva'}, inplace=True)`

---

### 2️⃣ Verificar la Estructura del Dataset 🏗️
```python
print(df.info())  # Muestra tipos de datos y valores nulos
print(df.describe())  # Estadísticas generales (media, mínimo, máximo...)
```
📌 **Posibles Problemas:**
❌ **Columnas categóricas aparecen como numéricas** → Convierte con `df["col"] = df["col"].astype(str)`
❌ **Valores incorrectos en fechas** → Convierte con `df["fecha"] = pd.to_datetime(df["fecha"])`

---

### 3️⃣ Identificar Valores Nulos 🔍
```python
print(df.isnull().sum())  # Conteo de valores nulos por columna
```

📌 **Posibles Soluciones:**
✅ **Eliminar valores nulos:**
```python
df = df.dropna()
```
✅ **Rellenar valores nulos:**
```python
df.fillna(df.mean(), inplace=True)  # Rellenar con la media de la columna
```
✅ **Reemplazar valores vacíos en columnas categóricas:**
```python
df["col_categ"] = df["col_categ"].fillna("Desconocido")
```

---

### 4️⃣ Detectar Duplicados 🧐
```python
print(df.duplicated().sum())  # Contar filas duplicadas
df = df.drop_duplicates()
```
📌 **Posibles Problemas:**
❌ **Datos ingresados varias veces** → Usa `df.drop_duplicates(inplace=True)`

---

### 5️⃣ Análisis de Distribución de Datos 📊
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.histplot(df["edad"], bins=30, kde=True)
plt.show()
```
📌 **Posibles Problemas:**
❌ **Datos sesgados** → Aplicar transformaciones como `np.log(df["edad"])`
❌ **Datos con outliers** → Usar `sns.boxplot(x=df["edad"])`

---

### 6️⃣ Correlaciones y Relación entre Variables 🔄
```python
sns.heatmap(df.corr(), annot=True, cmap="coolwarm")
plt.show()
```
📌 **Posibles Problemas:**
❌ **Columnas con baja correlación** → Considerar eliminarlas
❌ **Falta de datos en variables categóricas** → Realizar codificación con `pd.get_dummies(df)`

---

## 📌 Resumen Final 📝

| Paso | Descripción |
|------|-------------|
| **EDA** | Análisis, visualización y limpieza de datos |
| **ETL** | Extraer, transformar y cargar datos |
| **Eliminar Nulos** | `df.fillna()`, `df.dropna()` |
| **Eliminar Duplicados** | `df.drop_duplicates()` |
| **Detectar Outliers** | Usar IQR y Boxplots |
| **Normalizar Datos** | `StandardScaler()` |
| **Guardar Datos** | `df.to_csv()`, `df.to_sql()` |

---

## 🎯 Consejos Finales 🎯
✅ **Explora siempre los datos antes de analizarlos o entrenar modelos.**  
✅ **Visualiza patrones y anomalías antes de tomar decisiones.**  
✅ **Limpia y normaliza los datos para evitar errores en los modelos.**  
✅ **Usa herramientas como Pandas, Seaborn y SQL para un mejor ETL.**  

