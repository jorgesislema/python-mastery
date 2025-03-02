# Primeros Pasos con Python

## 📌 Verificando la Instalación
Antes de empezar a programar, asegúrate de que Python esté instalado correctamente en tu sistema. Para ello, abre una terminal o consola y ejecuta:
```sh
python --version  # Windows o macOS con Python agregado al PATH
python3 --version  # macOS y Linux
```
Si ves una versión como `Python 3.x.x`, significa que la instalación fue exitosa.

---

## 📌 Ejecutando Python en Diferentes Entornos

### 🔹 Terminal/Consola
Puedes ejecutar el intérprete interactivo de Python escribiendo:
```sh
python  # Windows
python3  # macOS y Linux
```
Esto abrirá el **modo interactivo (REPL)**, donde puedes escribir código Python y ver los resultados inmediatamente.
Para salir, usa `exit()` o presiona `Ctrl + D` en Linux/macOS o `Ctrl + Z` en Windows.

### 🔹 Ejecutando un Archivo `.py`
Puedes escribir código en un archivo con extensión `.py` y ejecutarlo con:
```sh
python script.py  # Windows
python3 script.py  # macOS y Linux
```

### 🔹 Usando VS Code
1. Instala la extensión **Python**.
2. Abre un archivo `.py` y selecciona **Run Python File** desde la parte superior derecha o usa `Ctrl + Shift + P` → "Run Python File in Terminal".
3. Asegúrate de seleccionar el intérprete correcto en la barra inferior.

### 🔹 Google Colab
Google Colab permite ejecutar código Python en la nube sin necesidad de instalar nada. Solo accede a [Google Colab](https://colab.research.google.com/), crea un nuevo notebook y empieza a escribir código en las celdas.

```python
print("Hola, mundo!")
```
Presiona `Shift + Enter` para ejecutar la celda.

---

## 📌 Escribiendo tu Primer Programa

Crea un archivo `hola_mundo.py` con el siguiente contenido:
```python
print("¡Hola, Python!")
```
Ejecuta el script desde la terminal:
```sh
python hola_mundo.py
```

Si ves `¡Hola, Python!`, ¡felicidades! Has ejecutado tu primer programa en Python.

---

## 📌 Comentarios en Python
Los comentarios permiten agregar notas en el código sin afectar su ejecución.

### 🔹 Comentarios de una línea:
```python
# Esto es un comentario en Python
print("Hola, mundo!")  # Esto también es un comentario
```

### 🔹 Comentarios multilínea:
```python
"""
Este es un comentario de varias líneas.
Se usa comúnmente para documentar funciones y módulos.
"""
print("Python es genial")
```

---

## 📌 Variables y Tipos de Datos
Python no requiere declarar tipos de variables explícitamente. Algunos ejemplos:

```python
nombre = "Alice"  # String
edad = 25  # Entero
altura = 1.75  # Flotante
es_programador = True  # Booleano
```
Puedes verificar el tipo de una variable con:
```python
print(type(nombre))  # <class 'str'>
print(type(edad))    # <class 'int'>
print(type(altura))  # <class 'float'>
print(type(es_programador))  # <class 'bool'>
```

---

## 📌 Entrada y Salida de Datos
### 🔹 Entrada del usuario
```python
nombre = input("¿Cómo te llamas? ")
print("Hola, " + nombre + "!")
```

### 🔹 Salida con `print()`
```python
edad = 25
print(f"Mi edad es {edad} años")  # f-string para formateo
```

---

## 📌 Próximo Tema: Sintaxis y Operadores 🚀

