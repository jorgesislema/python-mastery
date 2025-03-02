# 📌 Tipos de Datos en Python

Ah, los tipos de datos! Son como los ingredientes que usamos en la cocina de la programación. Cada tipo de dato es diferente y tiene su propio propósito. Vamos a explorarlos de una manera divertida y fácil de entender, como si estuviéramos en una clase de cocina donde cada ingrediente tiene su propia personalidad. 🍳✨

---

## 1️⃣ Números 🔢

Los números son como los ingredientes básicos de cualquier receta. En Python, hay dos tipos principales de números:

### 🔹 Enteros (`int`)
Son números sin decimales, como los que usas para contar.
```python
edad = 10
cantidad_manzanas = 5
```

### 🔹 Flotantes (`float`)
Son números con decimales, como los que usas para medir.
```python
peso = 55.5
precio = 19.99
```

---

## 2️⃣ Textos (`str`) 📝

Los textos, o **strings**, son como las palabras que usas para escribir una receta. Van entre comillas (`""` o `''`).
```python
nombre = "Juanito"
mensaje = '¡Hola, mundo!'
```

### 🔹 Operaciones con Textos
✅ **Concatenación:** Unir textos con `+`.
```python
saludo = "Hola, " + "Juanito"  # Resultado: "Hola, Juanito"
```
✅ **Longitud:** Saber cuántos caracteres tiene un texto con `len()`.
```python
print(len("Hola"))  # Resultado: 4
```

---

## 3️⃣ Booleanos (`bool`) 🔘

Los booleanos son como interruptores: solo pueden estar encendidos (`True`) o apagados (`False`).
```python
es_feliz = True
es_mayor_de_edad = False
```

---

## 4️⃣ Listas (`list`) 📦

Las **listas** son como una caja donde puedes guardar varios ingredientes (datos) en orden. Pueden contener números, textos, ¡y hasta otras listas!
```python
frutas = ["manzana", "banana", "cereza"]
numeros = [1, 2, 3, 4, 5]
```

### 🔹 Operaciones con Listas
✅ **Acceder a un elemento:**
```python
print(frutas[0])  # Resultado: "manzana"
```
✅ **Agregar un elemento:**
```python
frutas.append("uva")  # Ahora frutas es ["manzana", "banana", "cereza", "uva"]
```

---

## 5️⃣ Tuplas (`tuple`) 🔒

Las **tuplas** son como listas, pero **no se pueden modificar** después de crearlas. Son como una receta escrita en piedra.
```python
coordenadas = (10, 20)
colores = ("rojo", "verde", "azul")
```

### 🔹 Operaciones con Tuplas
✅ **Acceder a un elemento:**
```python
print(coordenadas[0])  # Resultado: 10
```

---

## 6️⃣ Diccionarios (`dict`) 📖

Los **diccionarios** son como un libro de recetas donde cada ingrediente tiene una etiqueta. Guardan datos en **pares clave-valor**.
```python
persona = {
    "nombre": "Juanito",
    "edad": 10,
    "es_feliz": True
}
```

### 🔹 Operaciones con Diccionarios
✅ **Acceder a un valor:**
```python
print(persona["nombre"])  # Resultado: "Juanito"
```
✅ **Agregar un valor:**
```python
persona["ciudad"] = "Madrid"
```

---

## 7️⃣ Conjuntos (`set`) 🎲

Los **conjuntos** son como una bolsa de ingredientes donde **no puede haber duplicados**. Son útiles para eliminar repeticiones.
```python
numeros = {1, 2, 3, 4, 4, 2}
print(numeros)  # Resultado: {1, 2, 3, 4}
```

### 🔹 Operaciones con Conjuntos
✅ **Agregar un elemento:**
```python
numeros.add(5)  # Ahora numeros es {1, 2, 3, 4, 5}
```

---

## 8️⃣ `None` 🔍

`None` es como un ingrediente que **no existe**. Se usa para representar la **ausencia de valor**.
```python
resultado = None
```

---

## 🔟 Resumen de los Tipos de Datos 📋

| Tipo de Dato   | Ejemplo                           | Descripción                              |
|---------------|----------------------------------|------------------------------------------|
| **Entero (`int`)** | `edad = 10`                     | Números sin decimales.                   |
| **Flotante (`float`)** | `peso = 55.5`                   | Números con decimales.                   |
| **Texto (`str`)** | `nombre = "Juanito"`         | Secuencias de caracteres.                |
| **Booleano (`bool`)** | `es_feliz = True`          | Valores `True` o `False`.                |
| **Lista (`list`)** | `frutas = ["manzana", "banana"]` | Colección ordenada y mutable.           |
| **Tupla (`tuple`)** | `coordenadas = (10, 20)`  | Colección ordenada e inmutable.         |
| **Diccionario (`dict`)** | `persona = {"nombre": "Juanito"}` | Colección de pares clave-valor. |
| **Conjunto (`set`)** | `numeros = {1, 2, 3}`      | Colección no ordenada y sin duplicados. |
| **None** | `resultado = None`         | Ausencia de valor.                      |

---

## 🎯 Consejos Finales 🏁
✅ **Practica:** Usa cada tipo de dato en diferentes situaciones.
✅ **Experimenta:** Intenta convertir entre tipos de datos (`int()`, `str()`, `float()`).
✅ **Recuerda:** Usa listas cuando necesites modificar los datos y tuplas cuando no.





