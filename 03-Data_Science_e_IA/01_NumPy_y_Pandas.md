# 📌 Introducción a NumPy y Pandas en Python 🐍

## 📢 ¿Qué son NumPy y Pandas? 🤔
En el mundo de **Ciencia de Datos e Ingeniería de Datos**, dos librerías súper importantes son **NumPy** y **Pandas**. 

- **NumPy** nos ayuda a manejar números y matrices de manera rápida y eficiente. 📊
- **Pandas** nos permite trabajar con tablas de datos, como si fueran hojas de cálculo en Excel. 📝

Piensa en **NumPy** como una calculadora gigante que maneja grandes cantidades de números con facilidad y **Pandas** como una súper hoja de Excel con superpoderes que facilita la organización y análisis de datos. 💡

---

## 1️⃣ ¿Qué es NumPy y por qué es importante? 🏗️

NumPy (**Numerical Python**) es una librería que nos permite manejar grandes cantidades de datos numéricos de manera rápida y eficiente. Su estructura principal es el **array de NumPy**, que es como una lista de Python, pero más potente. 💪

### 🔹 Instalación de NumPy
Antes de usar NumPy, debemos instalarlo si no lo tenemos.
```sh
pip install numpy
```

### 🔹 Crear un Array en NumPy
En Python, las listas pueden contener cualquier tipo de dato, pero son más lentas cuando trabajamos con grandes volúmenes de datos. Los **arrays de NumPy** están optimizados para manejar números de forma eficiente.

```python
import numpy as np

# Crear un array de NumPy
mi_array = np.array([1, 2, 3, 4, 5])
print(mi_array)  # Salida: [1 2 3 4 5]
```

### 🔹 Propiedades de un Array
```python
print(mi_array.shape)  # (5,) -> Indica que tiene 5 elementos
print(mi_array.ndim)   # 1 -> Es un array de una dimensión
print(mi_array.size)   # 5 -> Número total de elementos
```

Los **arrays** pueden tener múltiples dimensiones, lo que nos permite representar datos como si fueran **tablas, imágenes o incluso modelos matemáticos complejos**.

### 🔹 Crear Arrays Multidimensionales (Matrices)
```python
matriz = np.array([[1, 2, 3], [4, 5, 6]])
print(matriz)
```
Salida:
```
[[1 2 3]
 [4 5 6]]
```
📌 **¿Por qué usar NumPy en lugar de listas?**
- **Es más rápido** 🚀, ya que los arrays están optimizados a nivel de hardware.
- **Ocupa menos memoria** 🧠, al manejar datos de manera más eficiente.
- **Tiene muchas funciones matemáticas avanzadas** 📏 que facilitan los cálculos.

---

## 2️⃣ Operaciones Matemáticas con NumPy ➕➖

NumPy nos permite hacer cálculos matemáticos de forma muy sencilla sin necesidad de usar bucles for.

```python
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])

# Suma
print(arr1 + arr2)  # [5 7 9]

# Resta
print(arr1 - arr2)  # [-3 -3 -3]

# Multiplicación
print(arr1 * arr2)  # [ 4 10 18]
```

📌 **Otras operaciones útiles:**
```python
np.mean(arr1)  # Media o promedio
np.max(arr1)   # Valor máximo
np.min(arr1)   # Valor mínimo
np.sum(arr1)   # Suma de todos los valores
np.sqrt(arr1)  # Raíz cuadrada de cada elemento
```

Con estas herramientas, podemos manejar datos numéricos de manera rápida y sin esfuerzo. 📊

---

## 3️⃣ ¿Qué es Pandas y por qué es útil? 📊

Pandas es una librería que nos permite manejar datos en forma de tablas, como en Excel. 📑

Pandas introduce dos estructuras principales:
- **Series**: Una columna de datos.
- **DataFrame**: Una tabla con múltiples columnas.

### 🔹 Instalación de Pandas
```sh
pip install pandas
```

### 🔹 Crear un DataFrame (Tabla de Datos) 📝
```python
import pandas as pd

# Crear un DataFrame
datos = {
    "Nombre": ["Ana", "Luis", "Carlos"],
    "Edad": [25, 30, 35],
    "Ciudad": ["Madrid", "Bogotá", "México"]
}

df = pd.DataFrame(datos)
print(df)
```
📌 **Salida:**
```
   Nombre  Edad   Ciudad
0    Ana    25  Madrid
1   Luis    30  Bogotá
2  Carlos    35  México
```

📌 **Ventajas de Pandas:**
- **Leer archivos CSV y Excel** 📂 fácilmente.
- **Analizar y limpiar datos** 🧹 de manera sencilla.
- **Manejar grandes cantidades de información** sin problemas. 💡

---

## 4️⃣ Leer y Escribir Datos con Pandas 📖

### 🔹 Leer un archivo CSV 📂
```python
df = pd.read_csv("archivo.csv")
print(df.head())  # Muestra las primeras filas
```

### 🔹 Guardar un DataFrame en un CSV
```python
df.to_csv("datos_guardados.csv", index=False)
```

---

## 5️⃣ Manipulación de Datos en Pandas 🛠️

### 🔹 Filtrar datos 📌
```python
# Filtrar personas mayores de 30 años
df_filtrado = df[df["Edad"] > 30]
print(df_filtrado)
```

### 🔹 Agregar una nueva columna 🆕
```python
df["Salario"] = [2000, 2500, 3000]
print(df)
```

### 🔹 Eliminar una columna ❌
```python
df = df.drop(columns=["Ciudad"])
print(df)
```

---

## 🎯 Resumen 📌

| Librería | ¿Para qué se usa? | Ejemplo |
|----------|------------------|---------|
| **NumPy** | Manejo eficiente de números y matrices | `np.array([1,2,3])` |
| **Pandas** | Análisis y manipulación de datos en tablas | `pd.DataFrame(datos)` |

---

## 📌 Consejos Finales 🎯
✅ **Usa NumPy** para cálculos numéricos y optimización de rendimiento.  
✅ **Usa Pandas** para manipulación de datos tabulares, como archivos CSV y bases de datos.  
✅ **Practica con datasets reales**, puedes descargar archivos CSV de Kaggle para experimentar.  
✅ **Aprende a visualizar datos** con Matplotlib o Seaborn.  

 😊

