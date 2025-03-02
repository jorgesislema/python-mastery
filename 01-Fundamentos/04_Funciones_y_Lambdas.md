# Funciones y Lambdas en Python

Las **funciones** en Python son bloques de código reutilizables que realizan una tarea específica. Nos ayudan a organizar y estructurar mejor nuestros programas, evitando la repetición de código y mejorando la legibilidad.

---

## 📌 Definiendo Funciones en Python

### 🔹 ¿Qué es una función?
Una función es un conjunto de instrucciones agrupadas bajo un nombre que pueden ejecutarse en cualquier momento simplemente llamando a ese nombre.

### 🔹 Sintaxis básica de una función
```python
def nombre_de_la_funcion(parametros):
    """Docstring: Explica qué hace la función"""
    # Código de la función
    return resultado  # (opcional)
```

### 🔹 Ejemplo básico
```python
def saludar(nombre):
    """Función que imprime un saludo"""
    print(f"Hola, {nombre}!")

saludar("Carlos")
```

📌 **Consejo:** Usa docstrings (`"""Texto"""`) para documentar las funciones.

---

## 📌 Parámetros y Argumentos en Funciones

### 🔹 Parámetros por defecto
Puedes definir valores predeterminados para los parámetros.
```python
def presentar(nombre, edad=18):
    print(f"Hola, soy {nombre} y tengo {edad} años.")

presentar("Ana")  # Usa el valor por defecto para edad
presentar("Luis", 25)  # Sobrescribe el valor por defecto
```

### 🔹 Argumentos posicionales y nombrados
```python
def datos_persona(nombre, edad, ciudad):
    print(f"Nombre: {nombre}, Edad: {edad}, Ciudad: {ciudad}")

# Llamada con argumentos posicionales
datos_persona("Carlos", 30, "Madrid")

# Llamada con argumentos nombrados (permite cambiar el orden)
datos_persona(edad=25, ciudad="Barcelona", nombre="Marta")
```

---

## 📌 Retorno de Valores (`return`)

### 🔹 Función que devuelve un resultado
```python
def sumar(a, b):
    return a + b

resultado = sumar(5, 3)
print(f"La suma es: {resultado}")
```

📌 **Consejo:** `return` detiene la ejecución de la función y devuelve el valor indicado.

---

## 📌 Funciones Anónimas: `lambda`

Las funciones `lambda` son funciones pequeñas y anónimas que pueden tener múltiples argumentos pero solo una expresión.

### 🔹 Sintaxis de `lambda`
```python
lambda parametros: expresión
```

### 🔹 Ejemplo básico
```python
suma = lambda a, b: a + b
print(suma(4, 6))  # 10
```

### 🔹 Uso de `lambda` dentro de `map()`, `filter()` y `sorted()`
```python
numeros = [1, 2, 3, 4, 5]
cuadrados = list(map(lambda x: x**2, numeros))
print(cuadrados)  # [1, 4, 9, 16, 25]
```

```python
pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)  # [2, 4]
```

---

## 🎯 Conclusión
Las funciones y `lambda` en Python permiten crear código modular y reutilizable. Es importante entender cómo definir funciones, pasar argumentos y usar funciones anónimas para simplificar ciertas operaciones.

---

**🔹 Próximo Tema:** Manejo de Excepciones 🚀

