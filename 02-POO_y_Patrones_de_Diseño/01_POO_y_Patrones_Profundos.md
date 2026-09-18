# Programación Orientada a Objetos y Patrones de Diseño (Nivel PhD)

## 1. Modelo de Objetos de Python (Profundo)

```python
# Todo en Python tiene un tipo (cls) y una instancia (self)
class MiClase:
    x = 10  # Class variable — compartida

    def __init__(self, valor: int) -> None:
        self.valor = valor  # Instance variable — propia

    def metodo(self) -> int:
        return self.valor + MiClase.x

# MRO (Method Resolution Order) — C3 Linearization
class A:
    def metodo(self): return "A"

class B(A):
    def metodo(self): return "B"

class C(A):
    def metodo(self): return "C"

class D(B, C):
    pass

print(D().metodo())       # B
print(D.__mro__)          # (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)
print(D.mro())            # Lista de resolución
```

---

## 2. Dunder Methods (Magic Methods) — El Corazón de Python

```python
from __future__ import annotations
from typing import Self
import math

class Vector2D:
    """Vector 2D con todos los dunder methods esenciales."""

    __slots__ = ("_x", "_y")  # Ahorro de memoria, previene __dict__

    def __init__(self, x: float, y: float) -> None:
        self._x = x
        self._y = y

    # Representación
    def __repr__(self) -> str:
        return f"Vector2D({self._x!r}, {self._y!r})"

    def __str__(self) -> str:
        return f"({self._x}, {self._y})"

    def __format__(self, fmt: str) -> str:
        if fmt == "p":  # Formato polar
            r = math.hypot(self._x, self._y)
            theta = math.atan2(self._y, self._x)
            return f"({r:.2f} ∠ {math.degrees(theta):.1f}°)"
        return f"({self._x:{fmt}}, {self._y:{fmt}})"

    # Comparación
    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Vector2D):
            return NotImplemented
        return self._x == other._x and self._y == other._y

    def __lt__(self, other: Vector2D) -> bool:
        return self.magnitud() < other.magnitud()

    def __hash__(self) -> int:
        return hash((self._x, self._y))

    # Aritmética
    def __add__(self, other: Vector2D) -> Vector2D:
        return Vector2D(self._x + other._x, self._y + other._y)

    def __sub__(self, other: Vector2D) -> Vector2D:
        return Vector2D(self._x - other._x, self._y - other._y)

    def __mul__(self, scalar: float) -> Vector2D:
        return Vector2D(self._x * scalar, self._y * scalar)

    def __rmul__(self, scalar: float) -> Vector2D:
        return self.__mul__(scalar)

    def __abs__(self) -> float:
        return self.magnitud()

    def __bool__(self) -> bool:
        return self._x != 0 or self._y != 0

    # Indexación
    def __getitem__(self, index: int) -> float:
        if index == 0: return self._x
        if index == 1: return self._y
        raise IndexError("Vector2D solo tiene componentes 0 y 1")

    def __setitem__(self, index: int, value: float) -> None:
        if index == 0: self._x = value
        elif index == 1: self._y = value
        else: raise IndexError

    def __len__(self) -> int:
        return 2

    def __iter__(self):
        yield self._x
        yield self._y

    # Context manager
    def __enter__(self) -> Vector2D:
        print(f"Usando vector {self}")
        return self

    def __exit__(self, *args) -> None:
        print(f"Vector {self} liberado")

    # Callable
    def __call__(self, factor: float = 1.0) -> Vector2D:
        return Vector2D(self._x * factor, self._y * factor)

    # Propiedades
    @property
    def x(self) -> float:
        return self._x

    @property
    def y(self) -> float:
        return self._y

    def magnitud(self) -> float:
        return math.hypot(self._x, self._y)

    def normalizar(self) -> Vector2D:
        m = self.magnitud()
        return Vector2D(self._x / m, self._y / m) if m else Vector2D(0, 0)

    def punto(self, other: Vector2D) -> float:
        return self._x * other._x + self._y * other._y

# Uso completo
v1 = Vector2D(3, 4)
v2 = Vector2D(1, 2)
print(repr(v1))            # Vector2D(3, 4)
print(f"{v1:.2f}")         # (3.00, 4.00)
print(v1 + v2)             # (4, 6)
print(abs(v1))             # 5.0
print(v1[0])               # 3.0
print(v1(2.0))             # (6, 8) — callable
print(f"{v1:p}")           # (5.00 ∠ 53.1°) — formato polar
```

---

## 3. Herencia Avanzada

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass, field

# Clases abstractas
class Forma(ABC):
    @abstractmethod
    def area(self) -> float: ...

    @abstractmethod
    def perimetro(self) -> float: ...

    def descripcion(self) -> str:
        return f"{self.__class__.__name__}: área={self.area():.2f}"

@dataclass
class Circulo(Forma):
    radio: float

    def area(self) -> float:
        return math.pi * self.radio ** 2

    def perimetro(self) -> float:
        return 2 * math.pi * self.radio

@dataclass
class Rectangulo(Forma):
    ancho: float
    alto: float

    def area(self) -> float:
        return self.ancho * self.alto

    def perimetro(self) -> float:
        return 2 * (self.ancho + self.alto)

# Herencia múltiple y MRO
class LoggingMixin:
    def log(self, msg: str) -> None:
        print(f"[{self.__class__.__name__}] {msg}")

class ValidacionMixin:
    def validar(self) -> bool:
        return self.area() > 0

class CirculoLoggeado(Circulo, LoggingMixin, ValidacionMixin):
    pass

# frozen dataclass (inmutable)
@dataclass(frozen=True, order=True)
class Punto:
    x: float
    y: float
    nombre: str = field(compare=False)
```

---

## 4. Descriptors

```python
class Typed:
    """Descriptor que valida tipos en runtime."""
    def __init__(self, tipo):
        self.tipo = tipo

    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return obj.__dict__.get(self.name)

    def __set__(self, obj, value):
        if not isinstance(value, self.tipo):
            raise TypeError(f"{self.name} debe ser {self.tipo.__name__}")
        obj.__dict__[self.name] = value

class Persona:
    nombre = Typed(str)
    edad = Typed(int)

    def __init__(self, nombre: str, edad: int):
        self.nombre = nombre
        self.edad = edad

# Persona("Ana", 30)    # OK
# Persona("Ana", "30")  # TypeError
```

---

## 5. Patrones de Diseño GoF (Implementados en Python)

### Singleton
```python
from functools import wraps

def singleton(cls):
    instancias = {}
    @wraps(cls)
    def get_instance(*args, **kwargs):
        if cls not in instancias:
            instancias[cls] = cls(*args, **kwargs)
        return instancias[cls]
    return get_instance

@singleton
class BaseDatos:
    def __init__(self):
        self.conexion = "activa"
```

### Factory
```python
from enum import Enum

class TipoForma(Enum):
    CIRCULO = "circulo"
    RECTANGULO = "rectangulo"

class FabricaForma:
    @staticmethod
    def crear(tipo: TipoForma, **kwargs) -> Forma:
        mapa = {
            TipoForma.CIRCULO: Circulo,
            TipoForma.RECTANGULO: Rectangulo,
        }
        return mapa[tipo](**kwargs)
```

### Observer
```python
from collections import defaultdict

class Observable:
    def __init__(self):
        self._observadores: dict[str, list[callable]] = defaultdict(list)

    def suscribir(self, evento: str, callback: callable):
        self._observadores[evento].append(callback)

    def notificar(self, evento: str, *args, **kwargs):
        for cb in self._observadores[evento]:
            cb(*args, **kwargs)

class Pedido(Observable):
    def __init__(self, total: float):
        super().__init__()
        self.total = total

    def confirmar(self):
        self.notificar("confirmado", self.total)
```

### Strategy
```python
from typing import Protocol

class EstrategiaDescuento(Protocol):
    def calcular(self, total: float) -> float: ...

class DescuentoPorcentaje:
    def __init__(self, porcentaje: float):
        self.porcentaje = porcentaje
    def calcular(self, total: float) -> float:
        return total * (1 - self.porcentaje / 100)

class DescuentoFijo:
    def __init__(self, monto: float):
        self.monto = monto
    def calcular(self, total: float) -> float:
        return max(0, total - self.monto)

class Carrito:
    def __init__(self, estrategia: EstrategiaDescuento):
        self.estrategia = estrategia
        self.items: list[float] = []

    def total(self) -> float:
        return sum(self.items)

    def total_con_descuento(self) -> float:
        return self.estrategia.calcular(self.total())
```

### Decorator (Patrón, no decorador Python)
```python
class Componente(ABC):
    @abstractmethod
    def ejecutar(self) -> str: ...

class ComponenteBase(Componente):
    def ejecutar(self) -> str:
        return "componente base"

class LogDecorator(Componente):
    def __init__(self, wrapped: Componente):
        self._wrapped = wrapped

    def ejecutar(self) -> str:
        print(f"[LOG] Antes de ejecutar")
        resultado = self._wrapped.ejecutar()
        print(f"[LOG] Después de ejecutar")
        return resultado

class CacheDecorator(Componente):
    def __init__(self, wrapped: Componente):
        self._wrapped = wrapped
        self._cache = None

    def ejecutar(self) -> str:
        if self._cache is None:
            self._cache = self._wrapped.ejecutar()
        return self._cache

# Composición:
componente = CacheDecorator(LogDecorator(ComponenteBase()))
```

---

## 6. Dataclasses Avanzadas (3.13+)

```python
from dataclasses import dataclass, field, asdict, astuple
from typing import ClassVar

@dataclass(slots=True, frozen=True, kw_only=True)
class ConfiguracionBase:
    """Dataclass inmutable con slots para máximo rendimiento."""
    host: str
    puerto: int = 8080
    debug: bool = False

@dataclass
class Configuracion(ConfiguracionBase):
    tags: list[str] = field(default_factory=list)
    _contador: ClassVar[int] = 0  # Variable de clase

    def __post_init__(self):
        if not 1 <= self.puerto <= 65535:
            raise ValueError(f"Puerto inválido: {self.puerto}")
        Configuracion._contador += 1

config = Configuracion(host="localhost", puerto=3000, tags=["dev"])
print(asdict(config))   # Diccionario serializable
```
