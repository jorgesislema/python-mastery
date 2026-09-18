# Fundamentos Profundos de Python (Nivel PhD)

## 1. El Modelo de Objetos de Python

Todo en Python es un objeto. Incluso las funciones, las clases y los módulos son objetos con memoria, tipo y métodos.

```python
# Demostración: todo es objeto
x = 42
print(type(x))          # <class 'int'>
print(type(int))         # <class 'type'>
print(type(type))        # <class 'type'>  (meta-clase)

# Los objetos tienen atributos de memoria
print(x.__class__.__name__)  # 'int'
print(x.__doc__)             # Docstring del int
```

### `id()` vs `is` vs `==`
```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)     # True  (valor igual)
print(a is b)     # False (objetos diferentes)
print(a is c)     # True  (mismo objeto)
print(id(a) == id(c))  # True

# Small integer caching (CPython optimiza -5 a 256)
x = 256
y = 256
print(x is y)     # True  (cached)

x = 257
y = 257
print(x is y)     # Puede ser False (depende del contexto)
```

---

## 2. Namespaces y Scope (LEGB)

```python
# Regla LEGB: Local → Enclosing → Global → Built-in
x = "global"

def exterior():
    x = "enclosing"

    def interior():
        x = "local"
        print(x)       # local

    interior()
    print(x)           # enclosing

exterior()
print(x)               # global

# nonlocal y global
contador = 0

def incrementar():
    global contador
    contador += 1

def fabrica_contador():
    n = 0
    def incrementar():
        nonlocal n
        n += 1
        return n
    return incrementar
```

---

## 3. Modelos de Mutabilidad

```python
# Mutable: list, dict, set, bytearray, objetos custom
# Immutable: int, float, str, bytes, tuple, frozenset, None, bool

# Mutable default argument — ERROR CLÁSICO
def agregar(item, lista=None):
    if lista is None:
        lista = []
    lista.append(item)
    return lista

# Aliasing de mutables
a = [1, 2, 3]
b = a           # b apunta al Mismo objeto
b.append(4)
print(a)        # [1, 2, 3, 4] — a también cambió

# Copy vs deepcopy
import copy
original = [[1, 2], [3, 4]]
shallow = copy.copy(original)       # Solo copia el primer nivel
deep = copy.deepcopy(original)      # Copia recursiva

original[0][0] = 99
print(shallow[0][0])  # 99 (afectado)
print(deep[0][0])     # 1  (no afectado)
```

---

## 4. Iteradores y Generadores

```python
# Protocolo iterator: __iter__ + __next__
class Contador:
    def __init__(self, n: int):
        self.n = n
        self.actual = 0

    def __iter__(self) -> "Contador":
        return self

    def __next__(self) -> int:
        if self.actual >= self.n:
            raise StopIteration
        self.actual += 1
        return self.actual

# Generadores — lazy evaluation
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Generador con send() — coroutine
def media_corutina():
    total = 0.0
    count = 0
    average = None
    while True:
        valor = yield average
        total += valor
        count += 1
        average = total / count

# Uso:
coro = media_corutina()
next(coro)           # Inicializa (hasta el primer yield)
coro.send(10)        # 10.0
coro.send(20)        # 15.0
coro.send(30)        # 20.0

# yield from — delegación de generadores
def cadena(*generadores):
    for gen in generadores:
        yield from gen
```

---

## 5. Context Managers

```python
# Protocolo: __enter__ + __exit__
class Timer:
    def __init__(self, label: str = ""):
        self.label = label
        self.tiempo = 0.0

    def __enter__(self) -> "Timer":
        import time
        self._start = time.perf_counter()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb) -> bool:
        import time
        self.tiempo = time.perf_counter() - self._start
        print(f"{self.label}: {self.tiempo:.4f}s")
        return False  # No suprime excepciones

# contextmanager decorator (más simple)
from contextlib import contextmanager, suppress, redirect_stdout
from io import StringIO

@contextmanager
def gestionar_archivo(ruta: str, modo: str = "r"):
    f = open(ruta, modo)
    try:
        yield f
    finally:
        f.close()

# suppress — ignorar excepciones específicas
with suppress(FileNotFoundError):
    open("no_existe.txt")

# redirigir stdout
f = StringIO()
with redirect_stdout(f):
    print("esto va al buffer")
salida = f.getvalue()
```

---

## 6. Decoradores Avanzados

```python
from functools import wraps, lru_cache, total_ordering
from time import perf_counter
from typing import Callable, Any
import logging

# Decorador con argumentos
def retry(max_intentos: int = 3, delay: float = 1.0):
    def decorator(func: Callable) -> Callable:
        @wraps(func)
        def wrapper(*args, **kwargs):
            for intento in range(1, max_intentos + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if intento == max_intentos:
                        raise
                    logging.warning(f"Intento {intento} falló: {e}")
                    time.sleep(delay)
        return wrapper
    return decorator

# Clase como decorador
class RateLimit:
    def __init__(self, max_calls: int, period: float):
        self.max_calls = max_calls
        self.period = period
        self.calls = []

    def __call__(self, func: Callable) -> Callable:
        @wraps(func)
        def wrapper(*args, **kwargs):
            ahora = perf_counter()
            self.calls = [t for t in self.calls if ahora - t < self.period]
            if len(self.calls) >= self.max_calls:
                raise RuntimeError(f"Rate limit: {self.max_calls}/{self.period}s")
            self.calls.append(ahora)
            return func(*args, **kwargs)
        return wrapper

# LRU Cache con estadísticas
@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# Verificar cache
print(fibonacci.cache_info())
```

---

## 7. Type Hints y Type Checking (Python 3.13+)

```python
from typing import (
    TypeVar, Generic, Protocol, TypeAlias,
    overload, reveal_type, cast, Annotated
)
from collections.abc import Callable, Iterable, Sequence
from dataclasses import dataclass

# TypeVar y Generics
T = TypeVar("T")
T_co = TypeVar("T_co", covariant=True)

def primero(lista: Sequence[T]) -> T:
    return lista[0]

# Protocol (structural subtyping)
class Printable(Protocol):
    def __str__(self) -> str: ...

def imprimir(obj: Printable) -> None:
    print(str(obj))

# Annotated — metadata en types
from annotated_types import Gt, Le
Edad = Annotated[int, Gt(0), Le(150)]

# TypeAlias
Vector = list[float]
Matrix = list[Vector]

# Función overloaded
@overload
def buscar(datos: list[str], clave: str) -> int | None: ...
@overload
def buscar(datos: list[int], clave: int) -> int | None: ...

def buscar(datos, clave):
    try:
        return datos.index(clave)
    except ValueError:
        return None

# ParamSpec — para decoradores (3.10+)
from typing import ParamSpec, Concatenate
P = ParamSpec("P")

def log_call(func: Callable[P, T]) -> Callable[Concatenate[str, P], T]:
    @wraps(func)
    def wrapper(logger: str, *args: P.args, **kwargs: P.kwargs) -> T:
        print(f"[{logger}] Llamando a {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log_call
def sumar(a: int, b: int) -> int:
    return a + b

sumar("mi_logger", 1, 2)  # El tipo de logger se infiere correctamente
```

---

## 8. Asincronía Profunda (asyncio)

```python
import asyncio
from asyncio import Task, TaskGroup

# Coroutine básica
async def saludar(nombre: str) -> str:
    await asyncio.sleep(1)
    return f"Hola, {nombre}"

# Ejecutar múltiples coroutines en paralelo
async def main():
    # Forma 1: gather
    resultados = await asyncio.gather(
        saludar("Ana"),
        saludar("Luis"),
        saludar("Carlos")
    )

    # Forma 2: TaskGroup (3.11+) — manejo de errores mejorado
    async with TaskGroup() as tg:
        t1 = tg.create_task(saludar("Ana"))
        t2 = tg.create_task(saludar("Luis"))
    # Todas las tareas completan antes de salir del bloque

# Async generators
async def fibonacci_async():
    a, b = 0, 1
    while True:
        yield a
        await asyncio.sleep(0.1)
        a, b = b, a + b

# Async context managers
class AsyncDB:
    async def __aenter__(self) -> "AsyncDB":
        self.conn = await self._conectar()
        return self

    async def __aexit__(self, *args) -> None:
        await self.conn.close()

# Async comprehensions
async def procesar():
    resultados = [x async for x in fibonacci_async() if x < 100]
```

---

## 9. Metaclasses y Descriptors

```python
# Metaclass — clase de una clase
class ValidadorMeta(type):
    def __new__(mcs, nombre, bases, dct):
        # Validar que todas las clases tengan docstring
        if bases and "__doc__" not in dct:
            raise TypeError(f"{nombre} necesita docstring")
        return super().__new__(mcs, nombre, bases, dct)

class MiClase(metaclass=ValidadorMeta):
    """Clase válida."""
    pass

# Descriptor protocol
class Property:
    def __init__(self, fget, fset=None):
        self.fget = fget
        self.fset = fset

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return self.fget(obj)

    def __set__(self, obj, value):
        if self.fset is None:
            raise AttributeError("Attribute is read-only")
        self.fset(obj, value)

# __init_subclass__ — hook de herencia (3.6+)
class Plugin:
    registry: dict[str, type] = {}

    def __init_subclass__(cls, nombre: str = "", **kwargs):
        super().__init_subclass__(**kwargs)
        if nombre:
            Plugin.registry[nombre] = cls

class MotorBusqueda(Plugin, nombre="busqueda"):
    pass

class MotorRecomendacion(Plugin, nombre="recomendacion"):
    pass

print(Plugin.registry)  # {'busqueda': <class 'MotorBusqueda'>, ...}
```

---

## 10. Concurrencia vs Paralelismo

```python
import asyncio
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import multiprocessing as mp

# I/O-bound → asyncio o threading
# CPU-bound → multiprocessing (o free-threaded Python 3.13+)

# ThreadPoolExecutor — para I/O bloqueante
def descargar_url(url: str) -> str:
    import urllib.request
    return urllib.request.urlopen(url).read().decode()

with ThreadPoolExecutor(max_workers=10) as executor:
    resultados = list(executor.map(descargar_url, urls))

# ProcessPoolExecutor — para CPU-bound
def calcular_heavy(n: int) -> int:
    return sum(i**2 for i in range(n))

with ProcessPoolExecutor() as executor:
    resultados = list(executor.map(calcular_heavy, [10**6] * 4))

# Python 3.13+ free-threaded (sin GIL)
# Compilar con --disable-gil y usar threads normales
```
