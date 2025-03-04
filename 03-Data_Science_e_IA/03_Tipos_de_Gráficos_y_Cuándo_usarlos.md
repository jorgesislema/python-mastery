# 📌 Tipos de Gráficos y Cuándo Usarlos en Análisis de Datos

En el análisis de datos, la visualización es clave para entender patrones, tendencias y anomalías en los datos. Dependiendo del tipo de información, existen diferentes tipos de gráficos que pueden ser más adecuados. A continuación, explicamos cada tipo de gráfico, su uso y ejemplos en Python.

---
22

# ⭐️ Gráficos de Distribución en Python: Conceptos, Ejemplos y Cuándo Usarlos

En esta sección, exploraremos los diferentes tipos de **gráficos de distribución** utilizados en Python para visualizar la distribución de datos, identificación de valores atípicos y patrones en un conjunto de datos.

---

## 📚 **Resumen de los Gráficos de Distribución**

| **Tipo de Gráfico**                 | **Descripción**                                                  | **Código Base**                                                                      |
| ----------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Boxplot**                         | Muestra la distribución de una variable y valores atípicos.      | `plt.boxplot(datos)`                                                                 |
| **Violin Plot**                     | Similar al Boxplot, pero con distribución de densidad.           | `sns.violinplot(y=datos)`                                                            |
| **Stem Plot**                       | Representa datos discretos con líneas verticales desde el eje X. | `plt.stem(x, y)`                                                                     |
| **Gráfico de Pirámide Poblacional** | Dos histogramas reflejados para comparar poblaciones.            | `plt.barh(categorias, valores_1)`, `plt.barh(categorias, valores_2, left=valores_1)` |

---


---

## 📖 ** Gráficos Avanzados**

| **Tipo de Gráfico**                 | **Descripción**                                         | **Código Base**                                      |
|-------------------------------------|-------------------------------------------------|--------------------------------------------------|
| **Heatmap**                        | Representación en color de una matriz de datos. | `plt.imshow(matriz, cmap='viridis')`            |
| **Gráfico de Contorno**            | Líneas de nivel de una función 3D en 2D.       | `plt.contour(X, Y, Z)`                          |
| **Contorno Relleno**                | Contorno con áreas rellenas de color.          | `plt.contourf(X, Y, Z)`                         |
| **Quiver Plot**                     | Representación de vectores en 2D.              | `plt.quiver(X, Y, U, V)`                        |
| **Streamplot**                      | Flujo de partículas en un campo vectorial.     | `plt.streamplot(X, Y, U, V)`                    |
| **Error Bars**                      | Barras de error en mediciones.                  | `plt.errorbar(x, y, yerr=errores, fmt='o')`    |

Estos gráficos avanzados permiten analizar patrones más complejos y obtener insights profundos en conjuntos de datos. Si necesitas ejemplos adicionales o ajustes, ¡házmelo saber! 🚀


22
## 1️⃣ Gráficos de Barras 📊
**Uso:** Comparar cantidades entre diferentes categorías.

**Ejemplo:** Comparar las ventas de productos en diferentes meses.

```python
import matplotlib.pyplot as plt
import seaborn as sns

datos = {"Producto A": 150, "Producto B": 200, "Producto C": 180}
nombres = list(datos.keys())
valores = list(datos.values())

plt.bar(nombres, valores, color='blue')
plt.xlabel("Productos")
plt.ylabel("Ventas")
plt.title("Ventas por Producto")
plt.show()
```

📌 **Cuándo Usarlo:** Cuando tienes **categorías** y quieres compararlas en términos de frecuencia o cantidad.

---

## 2️⃣ Histogramas 📈
**Uso:** Analizar la distribución de una variable numérica.

**Ejemplo:** Distribución de edades en una población.

```python
import numpy as np
import seaborn as sns

edades = np.random.randint(10, 80, 100)
sns.histplot(edades, bins=10, kde=True, color='green')
plt.xlabel("Edad")
plt.ylabel("Frecuencia")
plt.title("Distribución de Edades")
plt.show()
```

📌 **Cuándo Usarlo:** Cuando quieres ver cómo se distribuye una variable **continua**.

---

## 3️⃣ Gráficos de Dispersión (Scatter Plot) 🔵
**Uso:** Analizar la relación entre dos variables numéricas.

**Ejemplo:** Relación entre la altura y el peso de una muestra de personas.

```python
altura = np.random.normal(170, 10, 100)
peso = np.random.normal(70, 15, 100)

sns.scatterplot(x=altura, y=peso, color='red')
plt.xlabel("Altura (cm)")
plt.ylabel("Peso (kg)")
plt.title("Relación entre Altura y Peso")
plt.show()
```

📌 **Cuándo Usarlo:** Cuando quieres analizar la **correlación** entre dos variables numéricas.

---

## 4️⃣ Gráfico de Cajas (Boxplot) 📦
**Uso:** Detectar valores atípicos y analizar la distribución de datos.

**Ejemplo:** Comparar los salarios en diferentes industrias.

```python
salarios = [np.random.normal(3000, 500, 100), np.random.normal(5000, 700, 100), np.random.normal(7000, 900, 100)]
industria = ["IT", "Salud", "Educación"]

sns.boxplot(data=salarios)
plt.xticks([0, 1, 2], industria)
plt.xlabel("Industria")
plt.ylabel("Salario")
plt.title("Distribución de Salarios por Industria")
plt.show()
```

📌 **Cuándo Usarlo:** Para identificar valores **atípicos** y la distribución de una variable.

---

## 5️⃣ Gráfico de Líneas 📉
**Uso:** Representar tendencias a lo largo del tiempo.

**Ejemplo:** Ventas mensuales en un año.

```python
meses = ["Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep", "Oct", "Nov", "Dic"]
ventas = np.random.randint(100, 500, 12)

plt.plot(meses, ventas, marker='o', linestyle='-', color='purple')
plt.xlabel("Meses")
plt.ylabel("Ventas")
plt.title("Tendencia de Ventas")
plt.show()
```

📌 **Cuándo Usarlo:** Para visualizar **tendencias temporales** en datos.

---

## 6️⃣ Gráfico de Torta (Pie Chart) 🍕
**Uso:** Representar la proporción de diferentes categorías.

**Ejemplo:** Porcentaje de ventas de diferentes marcas.

```python
marcas = ["Marca A", "Marca B", "Marca C"]
ventas = [40, 35, 25]

plt.pie(ventas, labels=marcas, autopct="%1.1f%%", colors=['blue', 'red', 'green'])
plt.title("Participación de Mercado")
plt.show()
```

📌 **Cuándo Usarlo:** Para comparar proporciones dentro de un **conjunto total**.

---

## 7️⃣ Heatmap (Mapa de Calor) 🔥
**Uso:** Mostrar la correlación entre variables numéricas.

**Ejemplo:** Matriz de correlación entre variables de un dataset.

```python
import pandas as pd

# Crear un dataset de ejemplo
datos = pd.DataFrame(np.random.rand(10, 5), columns=["A", "B", "C", "D", "E"])

sns.heatmap(datos.corr(), annot=True, cmap="coolwarm")
plt.title("Matriz de Correlación")
plt.show()
```

📌 **Cuándo Usarlo:** Para analizar **relaciones** entre varias variables numéricas.

---

## 📌 Resumen Final 📊

| Tipo de Gráfico | Cuándo Usarlo |
|----------------|-----------------|
| **Barras** | Comparar cantidades entre categorías |
| **Histogramas** | Ver distribución de una variable numérica |
| **Dispersión** | Analizar relación entre dos variables numéricas |
| **Boxplot** | Detectar valores atípicos y distribución |
| **Líneas** | Visualizar tendencias temporales |
| **Torta** | Comparar proporciones |
| **Heatmap** | Analizar correlaciones |

---

## 🎯 Consejos Finales 🎯
✅ **Elige el gráfico adecuado según tu tipo de datos.**  
✅ **Evita el exceso de información en una sola gráfica.**  
✅ **Usa colores adecuados para mejorar la interpretación.**  
✅ **No uses gráficos de torta con demasiadas categorías.**  

---

Si necesitas ayuda con algún gráfico en específico, ¡pregunta! 🚀

