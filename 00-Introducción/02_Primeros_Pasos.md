# Primeros Pasos en Python

## Python como Calculadora Interactiva

```python
# REPL (Read-Eval-Print Loop)
>>> 2 + 2
4
>>> "Hello" * 3
'HelloHelloHello'
>>> [i**2 for i in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

---

## Estructura Básica de un Programa

```python
#!/usr/bin/env python3
"""Módulo principal — descripción del programa."""

from __future__ import annotations

import sys
from pathlib import Path
from typing import reveal_type


def main() -> None:
    """Punto de entrada del programa."""
    nombre: str = input("¿Cómo te llamas? ")
    print(f"Hola, {nombre}. Bienvenido a Python 3.15.")


if __name__ == "__main__":
    main()
```

---

## Variables y Tipado

```python
# Python es tipado dinámicamente, pero soporta anotaciones
nombre: str = "Ana"
edad: int = 30
altura: float = 1.75
es_estudiante: bool = True

# Type inference (Python infiere el tipo)
x = 42          # int
y = 3.14        # float
z = "texto"     # str

# reveal_type() — ver qué tipo infiere mypy (solo en notebooks o stubs)
reveal_type(x)  # int
```

---

## Estructuras de Datos Fundamentales

```python
# List — mutable, ordenada
numeros: list[int] = [1, 2, 3, 4, 5]
numeros.append(6)
numeros[0]  # 1

# Tuple — inmutable, ordenada
coordenadas: tuple[float, float] = (3.14, 2.71)

# Dict — mapeo clave-valor
persona: dict[str, int | str] = {"nombre": "Ana", "edad": 30}

# Set — sin duplicados, sin orden
uniones: set[int] = {1, 2, 3, 3, 4}  # {1, 2, 3, 4}

# Frozenset — inmutable
fs: frozenset[int] = frozenset({1, 2, 3})
```

---

## Strings Modernos (Python 3.12+)

```python
# F-strings (desde 3.6, mejorados en 3.12)
nombre = "Mundo"
precio = 49.99
print(f"¡Hola {nombre}! Precio: {precio:.2f}€")

# F-strings multilínea (3.12+)
mensaje = f"""
    Nombre: {nombre}
    Precio: {price:.2f}€
    """

# Debugging con = (3.8+)
x = 42
print(f"{x=}")  # x=42

# Template strings (3.14+)
t"Hello {name}"  # deferred evaluation — evalúa cuando se necesita
```

---

## Operadores Avanzados

```python
# Walrus operator := (3.8+) — asignar y usar en una expresión
if (n := len(mi_lista)) > 10:
    print(f"Lista muy larga: {n} elementos")

# Operator overloading con dunder methods
class Vector:
    def __init__(self, x: float, y: float):
        self.x, self.y = x, y

    def __add__(self, other: "Vector") -> "Vector":
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self) -> str:
        return f"Vector({self.x}, {self.y})"
```

---

## Comprensiones (Comprehensions)

```python
# List comprehension
cuadrados = [x**2 for x in range(10) if x % 2 == 0]

# Dict comprehension
invertido = {v: k for k, v in d.items()}

# Set comprehension
palabras_unicas = {w.lower() for w in oracion.split()}

# Generator expression (lazy evaluation)
suma = sum(x**2 for x in range(1_000_000))

# Nested comprehension
matriz = [[i*j for j in range(5)] for i in range(5)]
```

---

## Manejo de Errores

```python
# Exception hierarchy completa
try:
    resultado = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:
    print(f"Error de tipo/valor: {e}")
except Exception as e:
    print(f"Error inesperado: {e}")
finally:
    print("Siempre se ejecuta")

# Context managers — recursos gestionados
with open("archivo.txt", "w") as f:
    f.write("contenido")
# El archivo se cierra automáticamente incluso si hay excepción

# Exception groups (3.11+)
try:
    raise ExceptionGroup("errores", [ValueError("bad value"), TypeError("bad type")])
except* ValueError as eg:
    print(f"ValueErrors: {eg.exceptions}")
except* TypeError as eg:
    print(f"TypeErrors: {eg.exceptions}")
```

---

## Ejecución

```bash
# Script directo
python mi_script.py

# Module (con -m)
python -m mi_modulo

# Jupyter notebook
jupyter notebook
# O en VS Code: Ctrl+Shift+P → "Jupyter: Create Notebook"

# REPL interactivo
python -i mi_script.py  # Ejecuta y queda en REPL
```
