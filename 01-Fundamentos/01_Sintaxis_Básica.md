# 📌 Sintaxis Básica en Python

Python es como un lenguaje que hablamos con la computadora. Para que la computadora nos entienda, debemos seguir algunas reglas básicas. Estas reglas se llaman **sintaxis**. En esta sección, aprenderás los fundamentos esenciales de Python.

---

## 1️⃣ Comentarios en Python 📝

Los **comentarios** son notas que ayudan a los programadores a entender el código, pero la computadora los ignora. Son útiles para documentar y explicar qué hace cada parte del programa.

### 🔹 Comentario de una línea:
```python
# Esto es un comentario
print("¡Hola, mundo!")  # Comentario al final de la línea
```

### 🔹 Comentarios de varias líneas:
```python
"""
Esto es un comentario
que abarca varias líneas.
"""
print("¡Bienvenido a Python!")
```

---

## 2️⃣ Variables y Tipos Básicos 📦

### 🔹 ¿Qué es una variable?
Las **variables** son contenedores de información. En Python, **no necesitas especificar el tipo de dato**, ya que se asigna automáticamente.

### 🔹 Creación de variables:
```python
nombre = "Juanito"  # Cadena de texto (str)
edad = 10           # Número entero (int)
es_feliz = True     # Booleano (bool)
altura = 1.75       # Número decimal (float)
```

### 🔹 Reglas para nombrar variables:
✅ No pueden comenzar con números.
✅ No pueden contener espacios.
✅ Pueden contener letras, números y guiones bajos (`_`).

---

## 3️⃣ Imprimir en Pantalla 🖥️

Para mostrar información en la pantalla, usamos `print()`:
```python
print("¡Hola, mundo!")
print(f"Mi nombre es {nombre} y tengo {edad} años.")
```

📌 **Consejo:** Usa `f"{variable}"` para incluir variables en cadenas de texto.

---

## 4️⃣ Tipos de Datos en Python 🔢🔤

### 🔹 Enteros (`int`):
```python
numero_entero = 42
```

### 🔹 Flotantes (`float`):
```python
numero_flotante = 3.14
```

### 🔹 Cadenas (`str`):
```python
mensaje = "¡Amo Python!"
```

### 🔹 Booleanos (`bool`):
```python
es_python_genial = True
```

---

## 5️⃣ Operaciones Básicas 🔢

### 🔹 Operaciones Matemáticas:
```python
suma = 5 + 3          # 8
resta = 10 - 4        # 6
multiplicacion = 4 * 2 # 8
division = 10 / 2      # 5.0
potencia = 2 ** 3      # 8
```

### 🔹 Operaciones con Textos (Concatenación):
```python
saludo = "Hola, " + "Juanito"
print(saludo)  # "Hola, Juanito"
```

---

## 6️⃣ Estructuras de Control 🔄

### 🔹 Condicionales (`if`, `elif`, `else`):
Permiten tomar decisiones basadas en condiciones.
```python
edad = 18
if edad >= 18:
    print("Eres mayor de edad.")
elif edad == 17:
    print("Casi eres mayor de edad.")
else:
    print("Eres menor de edad.")
```

### 🔹 Bucles (`for`, `while`):
Sirven para repetir código varias veces.
```python
for i in range(3):
    print("¡Hola!")  # Se imprimirá 3 veces
```
```python
contador = 1
while contador <= 3:
    print(f"Iteración {contador}")
    contador += 1
```

---

## 7️⃣ Funciones 🛠️

Las funciones son bloques de código reutilizables.
```python
def saludar(nombre):
    return f"¡Hola, {nombre}!"

print(saludar("Juanito"))  # "¡Hola, Juanito!"
```

---

## 8️⃣ Indentación (Sangría) 🔍

En Python, **la indentación es obligatoria**. Se usa para definir bloques de código.

✅ **Ejemplo Correcto:**
```python
if True:
    print("Esto está indentado correctamente.")
```
❌ **Ejemplo Incorrecto:**
```python
if True:
print("Esto dará un error.")  # Falta la indentación
```

---

## 9️⃣ Ejemplo Completo 🎯
```python
# Definir variables
nombre = "Juanito"
edad = 10

# Función para saludar
def saludar(nombre):
    return f"¡Hola, {nombre}!"

# Condicional
if edad < 18:
    mensaje = "Eres un niño genial."
else:
    mensaje = "Eres un adulto genial."

# Imprimir resultados
print(saludar(nombre))  # "¡Hola, Juanito!"
print(mensaje)  # "Eres un niño genial."
```

---

## 🔟 Resumen de la Sintaxis Básica 🏆

| Concepto         | Ejemplo                             |
|-----------------|-----------------------------------|
| Comentarios     | `# Esto es un comentario`         |
| Variables       | `nombre = "Juanito"`              |
| Impresión       | `print("¡Hola!")`                 |
| Números         | `edad = 10`                       |
| Textos          | `mensaje = "Hola"`                |
| Booleanos       | `es_genial = True`                |
| Operaciones     | `suma = 5 + 3`                    |
| Condicionales   | `if edad >= 18:`                  |
| Bucles          | `for i in range(3):`              |
| Funciones       | `def saludar(nombre):`            |
| Indentación     | `if True: print("Correcto")`      |

---

## 🎯 Consejos Finales 🏁
✅ **Practica:** Escribe código todos los días.
✅ **Experimenta:** Cambia los ejemplos y observa qué sucede.
✅ **Diviértete:** La programación es como un juego. ¡Disfruta el proceso!

---

**🔹 Próximo Tema:** Estructuras de Datos 🚀

