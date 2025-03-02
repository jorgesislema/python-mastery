# 📌 Estructuras de Control en Python

Las **estructuras de control** son las instrucciones que le damos a la computadora para que **tome decisiones o repita acciones**. Son fundamentales para crear programas dinámicos y flexibles. Vamos a explorarlas con ejemplos prácticos y consejos para mejorar el código.

---

## 1️⃣ Condicionales (`if`, `elif`, `else`) 🔀

Los condicionales permiten ejecutar diferentes bloques de código según **ciertas condiciones**.

### 🔹 Sintaxis Básica:
```python
if condicion:
    # Código si la condición es verdadera
elif otra_condicion:
    # Código si la otra condición es verdadera
else:
    # Código si ninguna condición anterior es verdadera
```

### 🔹 Ejemplo:
```python
edad = 18
if edad < 18:
    print("Eres menor de edad.")
elif edad == 18:
    print("¡Acabas de cumplir 18!")
else:
    print("Eres mayor de edad.")
```

📌 **Consejo:** Si solo tienes una línea de código, usa **operadores ternarios** para simplificar:
```python
mensaje = "Menor de edad" if edad < 18 else "Mayor de edad"
print(mensaje)
```

---

## 2️⃣ Bucles (`for`, `while`) 🔄

Los **bucles** permiten repetir un bloque de código varias veces.

### 🔹 `for`: Para un número específico de repeticiones
```python
for i in range(3):
    print(f"Vuelta número {i + 1}")
```
📌 **Mejora:** Usa `enumerate()` para recorrer listas con índice:
```python
frutas = ["manzana", "banana", "cereza"]
for indice, fruta in enumerate(frutas):
    print(f"{indice}: {fruta}")
```

### 🔹 `while`: Repite mientras una condición sea verdadera
```python
contador = 0
while contador < 3:
    print(f"Contador: {contador}")
    contador += 1
```

📌 **Consejo:** Usa `while True` con `break` para bucles infinitos seguros:
```python
while True:
    respuesta = input("Escribe 'salir' para terminar: ")
    if respuesta.lower() == "salir":
        break
```

---

## 3️⃣ Control de Bucles (`break`, `continue`) ⏭️

A veces, queremos **detener un bucle** o **saltar una iteración**. Para eso usamos `break` y `continue`.

### 🔹 `break`: Sale del bucle completamente
```python
for i in range(5):
    if i == 3:
        break  # Detiene el bucle cuando i es 3
    print(i)
```
**Salida:**
```
0
1
2
```

### 🔹 `continue`: Salta a la siguiente iteración
```python
for i in range(5):
    if i == 3:
        continue  # Salta la iteración cuando i es 3
    print(i)
```
**Salida:**
```
0
1
2
4
```

📌 **Consejo:** Evita `break` en bucles `while` sin condiciones de salida claras para evitar **bucles infinitos accidentales**.

---

## 4️⃣ Ejemplo Completo 📌
```python
# Condicionales
edad = 18
if edad < 18:
    print("Eres menor de edad.")
elif edad == 18:
    print("¡Acabas de cumplir 18!")
else:
    print("Eres mayor de edad.")

# Bucle for
print("\nBucle for:")
for i in range(3):
    print(f"Vuelta número {i + 1}")

# Bucle while
print("\nBucle while:")
contador = 0
while contador < 3:
    print(f"Contador: {contador}")
    contador += 1

# Control de bucles
print("\nControl de bucles:")
for i in range(5):
    if i == 3:
        break
    print(i)
```

---

## 🔟 Resumen de las Estructuras de Control 📋

| Estructura   | Descripción                                     | Ejemplo                     |
|-------------|--------------------------------|------------------------------|
| `if`        | Evalúa una condición y ejecuta código si es `True`. | `if edad < 18:`             |
| `elif`      | Evalúa una condición si el `if` es `False`. | `elif edad == 18:`          |
| `else`      | Código que se ejecuta si ninguna condición previa es `True`. | `else:` |
| `for`       | Repite un bloque de código un número específico de veces. | `for i in range(3):`        |
| `while`     | Repite un bloque mientras la condición sea `True`. | `while contador < 3:`       |
| `break`     | Detiene un bucle completamente. | `if i == 3: break`         |
| `continue`  | Salta a la siguiente iteración del bucle. | `if i == 3: continue`      |

---

## 🎯 Ejercicios Prácticos 💡

### 🔹 Condicionales:
✅ Crea un programa que verifique si un número es **positivo, negativo o cero**.

### 🔹 Bucles:
✅ Usa un `for` para imprimir los números **del 1 al 10**.
✅ Usa un `while` para imprimir los números **del 10 al 1**.

### 🔹 Control de Bucles:
✅ Usa `break` para detener un bucle **cuando encuentres el número 5**.
✅ Usa `continue` para **saltar la impresión del número 3** en un bucle.


