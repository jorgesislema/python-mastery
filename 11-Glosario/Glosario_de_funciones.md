# 📌 Glosario de Funciones y Bibliotecas Esenciales para Ciencia de Datos, Ingeniería de Datos, Análisis de Datos e Inteligencia Artificial en 2025 🚀

Este glosario recopila todas las funciones y bibliotecas más utilizadas en ciencia de datos, ingeniería de datos, análisis de datos, inteligencia artificial y despliegue de aplicaciones en Python. Contiene herramientas esenciales que todo experto en datos debe conocer. 📊🤖

---
# 📌 Glosario de Funciones y Bibliotecas Esenciales para Ciencia de Datos, Ingeniería de Datos, Análisis de Datos e Inteligencia Artificial en 2025 🚀

Este glosario recopila todas las funciones y bibliotecas más utilizadas en ciencia de datos, ingeniería de datos, análisis de datos, inteligencia artificial y despliegue de aplicaciones en Python. Contiene herramientas esenciales que todo experto en datos debe conocer. 📊🤖

---

# Funciones Básicas en Python ⚡
✅ `print(valor)` → Imprime un valor en la consola.
✅ `len(iterable)` → Devuelve la longitud de una lista, string o conjunto.
✅ `max(iterable)` → Devuelve el valor máximo.
✅ `min(iterable)` → Devuelve el valor mínimo.
✅ `sum(iterable)` → Suma todos los elementos de una lista.
✅ `range(inicio, fin, paso)` → Genera una secuencia de números.
✅ `abs(numero)` → Devuelve el valor absoluto.
✅ `round(numero, decimales)` → Redondea un número a los decimales especificados.
✅ `type(valor)` → Devuelve el tipo de dato.
✅ `sorted(iterable)` → Ordena una lista.
✅ `zip(iterable1, iterable2)` → Combina dos listas en pares.
✅ `map(funcion, iterable)` → Aplica una función a cada elemento de una lista.
✅ `filter(funcion, iterable)` → Filtra elementos según una condición.
✅ `enumerate(iterable)` → Devuelve índices y valores de un iterable.
✅ `input("mensaje")` → Solicita entrada del usuario.
✅ `isinstance(valor, tipo)` → Verifica si un valor es de un tipo específico.
✅ `all(iterable)` → Devuelve True si todos los elementos son verdaderos.
✅ `any(iterable)` → Devuelve True si al menos un elemento es verdadero.
✅ `eval(expresion)` → Evalúa una expresión de Python en forma de string.
✅ `exec(codigo)` → Ejecuta código Python dinámicamente.
✅ `id(objeto)` → Devuelve la dirección en memoria de un objeto.
✅ `hex(numero)` → Convierte un número a hexadecimal.
✅ `bin(numero)` → Convierte un número a binario.
✅ `oct(numero)` → Convierte un número a octal.
✅ `chr(numero)` → Devuelve el carácter ASCII de un número.
✅ `ord(caracter)` → Devuelve el código ASCII de un carácter.
✅ `reversed(iterable)` → Invierte un iterable.
✅ `pow(base, exponente)` → Calcula la potencia de un número.
✅ `divmod(a, b)` → Devuelve el cociente y el residuo de una división.
✅ `bytearray(longitud)` → Crea un array de bytes mutable.
✅ `bytes(longitud)` → Crea un array de bytes inmutable.
✅ `memoryview(bytes_obj)` → Crea una vista de memoria sobre un objeto de bytes.
✅ `hash(objeto)` → Devuelve el hash de un objeto.
✅ `frozenset(iterable)` → Crea un conjunto inmutable.
✅ `slice(inicio, fin, paso)` → Crea una rebanada de una secuencia.
✅ `format(valor, formato)` → Formatea un valor en una cadena de texto.
✅ `next(iterador)` → Devuelve el siguiente elemento de un iterador.
✅ `iter(iterable)` → Convierte un iterable en un iterador.
✅ `open("archivo", "modo")` → Abre un archivo con el modo especificado.
✅ `help(objeto)` → Muestra la documentación de un objeto.
✅ `dir(objeto)` → Muestra los atributos y métodos disponibles de un objeto.
✅ `vars(objeto)` → Devuelve el diccionario de atributos de un objeto.

---

## 1️⃣ Manipulación de Datos 🛠️

### 📌 `pandas` (Manipulación de DataFrames)
✅ `pd.read_csv("archivo.csv")` → Carga un archivo CSV en un DataFrame.
✅ `df.head(n)` → Muestra las primeras `n` filas.
✅ `df.info()` → Muestra información del DataFrame.
✅ `df.describe()` → Genera estadísticas descriptivas.
✅ `df.groupby("columna")` → Agrupa datos por una columna.
✅ `df.merge(df2, on="columna")` → Une dos DataFrames.
✅ `df.pivot_table(values, index, columns, aggfunc)` → Crea tablas dinámicas.
✅ `df.explode("columna")` → Expande listas en celdas en filas separadas.
✅ `df.melt(id_vars, value_vars)` → Convierte columnas en filas.

### 📌 `numpy` (Cálculo Numérico)
✅ `np.array(lista)` → Crea un array de NumPy.
✅ `np.mean(array)` → Calcula la media.
✅ `np.std(array)` → Calcula la desviación estándar.
✅ `np.reshape(array, (filas, columnas))` → Cambia la forma de un array.
✅ `np.linspace(inicio, fin, num)` → Genera `num` valores equidistantes.
✅ `np.where(condicion, valor_si_True, valor_si_False)` → Condicionales en arrays.

### 📌 `pyspark` (Big Data)
✅ `spark.read.csv("archivo.csv")` → Carga archivos CSV en Spark.
✅ `df.filter(df["columna"] > 100)` → Filtra filas.
✅ `df.groupBy("columna").agg(...)` → Agrupa y agrega datos.
✅ `df.selectExpr("columna as nueva_columna")` → Renombra columnas en Spark.

---

## 2️⃣ Limpieza y Preprocesamiento de Datos 🧹

### 📌 `pandas` (Limpieza de Datos)
✅ `df.dropna()` → Elimina valores nulos.
✅ `df.fillna(valor)` → Rellena valores nulos.
✅ `df.replace({"valor_antiguo": "valor_nuevo"})` → Reemplaza valores.
✅ `df.duplicated()` → Identifica duplicados.
✅ `df.astype({"columna": "tipo"})` → Convierte tipos de datos.

### 📌 `scikit-learn` (Preprocesamiento)
✅ `SimpleImputer(strategy="mean")` → Imputa valores faltantes.
✅ `StandardScaler().fit_transform(df)` → Normaliza datos.
✅ `OneHotEncoder().fit_transform(df)` → Codifica variables categóricas.
✅ `MinMaxScaler().fit_transform(df)` → Escala valores entre 0 y 1.

### 📌 `re` (Expresiones Regulares)
✅ `re.sub("patrón", "reemplazo", texto)` → Reemplaza patrones en texto.
✅ `re.findall("patrón", texto)` → Encuentra todas las coincidencias.
✅ `re.search("patrón", texto)` → Devuelve la primera coincidencia.

---

## 3️⃣ Análisis Exploratorio de Datos (EDA) 📊

### 📌 `pandas`
✅ `df.corr()` → Calcula correlaciones.
✅ `df.value_counts()` → Cuenta valores únicos.
✅ `df.nunique()` → Cuenta valores únicos por columna.

### 📌 `seaborn`
✅ `sns.histplot(df["columna"])` → Histograma.
✅ `sns.boxplot(x="columna", data=df)` → Gráfico de caja.
✅ `sns.pairplot(df)` → Relaciones entre variables.
✅ `sns.heatmap(df.corr(), annot=True)` → Mapa de calor de correlaciones.

---

## 4️⃣ Machine Learning 🤖

### 📌 `scikit-learn`
✅ `train_test_split(X, y, test_size=0.2)` → Divide datos en entrenamiento y prueba.
✅ `LinearRegression().fit(X, y)` → Regresión lineal.
✅ `RandomForestClassifier().fit(X, y)` → Clasificación con Random Forest.
✅ `GridSearchCV(modelo, param_grid)` → Búsqueda de hiperparámetros.
✅ `metrics.accuracy_score(y_true, y_pred)` → Precisión del modelo.
✅ `Pipeline(steps=[...])` → Construcción de flujo de preprocesamiento + modelo.

### 📌 `tensorflow` / `keras`
✅ `Sequential()` → Modelo de red neuronal.
✅ `model.compile(optimizer="adam", loss="mse")` → Configura el modelo.
✅ `model.fit(X_train, y_train, epochs=10)` → Entrena el modelo.
✅ `model.save("modelo.h5")` → Guarda el modelo.

---

## 5️⃣ Ingeniería de Datos ⚙️

### 📌 `sqlalchemy`
✅ `create_engine("sqlite:///database.db")` → Conectar a una base de datos.
✅ `pd.read_sql("SELECT * FROM tabla", con=engine)` → Leer datos de SQL.

### 📌 `pyspark`
✅ `spark.sql("SELECT * FROM tabla")` → Ejecutar consultas SQL en Spark.
✅ `df.write.parquet("archivo.parquet")` → Guardar en formato Parquet.
✅ `df.repartition(4)` → Reparticiona el DataFrame.

---

## 📌 Otras Herramientas Claves ⚙️

### 📌 `json` (Manejo de Datos JSON)
✅ `json.loads(texto_json)` → Convierte JSON a diccionario.
✅ `json.dumps(diccionario)` → Convierte un diccionario a JSON.

### 📌 `os` (Interacción con el Sistema Operativo)
✅ `os.listdir("ruta")` → Lista archivos de un directorio.
✅ `os.path.join("directorio", "archivo")` → Une rutas de archivos.

### 📌 `ast` (Procesamiento de Código en Python)
✅ `ast.literal_eval("[1, 2, 3]")` → Convierte cadenas en estructuras de Python.

---

## 🎯 Resumen 📌

| Categoría | Herramientas Principales |
|-----------|-------------------------|
| Manipulación de Datos | pandas, numpy, pyspark |
| Limpieza de Datos | pandas, scikit-learn, re |
| EDA | pandas, seaborn, matplotlib |
| Machine Learning | scikit-learn, tensorflow, xgboost |
| Ingeniería de Datos | sqlalchemy, pyspark, airflow |
| Visualización | plotly, matplotlib, seaborn |
| Despliegue | flask, fastapi, docker, kubernetes |
| Cloud | boto3, google-cloud-storage |
| Otras | json, os, ast |

---

## 📌 Próximos Pasos 🚀
Si necesitas más detalles sobre alguna función o herramienta, ¡dímelo! Puedo proporcionarte ejemplos detallados o explicaciones más profundas. 😊