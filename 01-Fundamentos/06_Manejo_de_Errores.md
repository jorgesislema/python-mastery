# 📌 Manejo de Errores en Python 🚀

El manejo de errores es una parte esencial de cualquier programa robusto. Python ofrece excepciones y herramientas para manejar errores de forma eficiente. En esta guía, aprenderás cómo gestionar errores correctamente y evitar que tu programa falle inesperadamente. 🔥💡

---

## 1️⃣ ¿Qué es una Excepción? ⚠️

Una **excepción** es un error que ocurre durante la ejecución del programa, lo que interrumpe su flujo normal. Python tiene excepciones predefinidas como:

✅ `ZeroDivisionError` → División por cero.
✅ `TypeError` → Operaciones incompatibles entre tipos de datos.
✅ `ValueError` → Valor incorrecto para una función o método.
✅ `IndexError` → Índice fuera del rango en listas o tuplas.
✅ `KeyError` → Clave inexistente en un diccionario.
✅ `FileNotFoundError` → Archivo no encontrado.
✅ `ImportError` → Módulo no encontrado o importación incorrecta.
✅ `NameError` → Uso de una variable no definida.
✅ `AttributeError` → Intentar acceder a un atributo que no existe.
✅ `TimeoutError` → Error de tiempo de espera en una operación.

📌 **Consejo:** Conocer las excepciones más comunes te ayuda a prevenir errores antes de que ocurran.

---

## 2️⃣ Captura de Excepciones con `try-except` 🔄

Usamos `try-except` para manejar errores sin que el programa se detenga abruptamente.

### 🔹 Sintaxis Básica
```python
try:
    # Código que puede generar un error
    resultado = 10 / 0  # ZeroDivisionError
except ZeroDivisionError:
    print("Error: No se puede dividir por cero.")
```

📌 **Consejo:** Siempre maneja excepciones específicas antes de usar `except` genérico.

---

## 3️⃣ Capturar Múltiples Excepciones 🛑

Podemos manejar diferentes tipos de errores en un solo bloque `try-except`.

```python
try:
    lista = [1, 2, 3]
    print(lista[5])  # IndexError
except (IndexError, KeyError):
    print("Error: Índice o clave fuera de rango.")
```

También podemos capturar el mensaje de error:
```python
try:
    resultado = int("texto")  # ValueError
except ValueError as e:
    print(f"Error capturado: {e}")
```

📌 **Consejo:** Guardar el mensaje del error con `as e` facilita la depuración.

---

## 4️⃣ `else` y `finally` en Excepciones 🔄

✅ **`else`** → Se ejecuta si **no** ocurre una excepción.
✅ **`finally`** → Se ejecuta siempre, ocurra o no una excepción (ideal para liberar recursos).

```python
try:
    archivo = open("datos.txt", "r")
    contenido = archivo.read()
except FileNotFoundError:
    print("Error: El archivo no existe.")
else:
    print("Archivo leído correctamente.")
finally:
    print("Finalizando ejecución...")
```

📌 **Consejo:** Usa `finally` para cerrar conexiones a bases de datos o archivos.

---

## 5️⃣ Lanzar Excepciones con `raise` 🚀

Podemos generar nuestras propias excepciones usando `raise`.

```python
def verificar_edad(edad):
    if edad < 18:
        raise ValueError("Debes ser mayor de edad.")
    return "Acceso permitido"

try:
    print(verificar_edad(15))
except ValueError as e:
    print(f"Error: {e}")
```

📌 **Consejo:** Usa `raise` cuando necesites validar condiciones en tu código.

---

## 6️⃣ Crear Excepciones Personalizadas ⚡

Podemos definir nuestras propias excepciones heredando de `Exception`.

```python
class ErrorPersonalizado(Exception):
    pass

try:
    raise ErrorPersonalizado("Ocurrió un error personalizado.")
except ErrorPersonalizado as e:
    print(f"Error capturado: {e}")
```

📌 **Consejo:** Útil cuando quieres manejar errores específicos en tus programas.

---

## 7️⃣ Manejo de Errores en Funciones 🌟

Las funciones deben manejar errores para ser más robustas.

```python
def dividir(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "No se puede dividir por cero."

print(dividir(10, 0))  # No se puede dividir por cero.
```

📌 **Consejo:** Es mejor manejar errores dentro de funciones en lugar de propagarlos.

---

## 🎯 Resumen 📌

| Concepto | Descripción |
|----------|------------|
| **Excepción** | Error que ocurre durante la ejecución del código. |
| **`try-except`** | Captura y maneja errores. |
| **Captura múltiple** | Se pueden manejar diferentes errores en un solo bloque. |
| **`else` y `finally`** | `else` se ejecuta si no hay error, `finally` siempre se ejecuta. |
| **`raise`** | Lanza una excepción manualmente. |
| **Excepciones personalizadas** | Definir errores propios usando `class MiError(Exception)`. |

---

## 📌 Consejos Finales 🎯
✅ **Anticípate a los errores:** Usa `try-except` en código que interactúe con archivos, bases de datos o entrada de usuario.
✅ **Específica siempre que puedas:** Captura excepciones específicas en lugar de usar `except Exception` general.
✅ **Registra los errores:** Usa `logging` en lugar de `print()` para guardar errores en logs.
✅ **Prueba tu código:** Simula errores y asegúrate de que el programa los maneja correctamente.
✅ **No abuses de `try-except`:** Úsalo cuando sea necesario, pero no para ocultar errores sin corregirlos.

---

Si necesitas más detalles o ejemplos adicionales, ¡avísame! 😊

