# Referencia Completa de Palabras y Funciones de Python
# Orden de aprendizaje — Desde básico hasta avanzado

## NIVEL 1: Primeros Pasos

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `print()` | Imprimir / Mostrar | Muestra información en pantalla. La función más básica para ver resultados. |
| `input()` | Entrada / Capturar | Pide al usuario que escriba algo por teclado y lo guarda en una variable. |
| `#` | Comentario | Escribe notas que Python ignora. Sirve para documentar el código. |
| `""" """` | Comentario multilínea | Comentario de varias líneas. También sirve como docstring de funciones. |
| `=` | Asignar / Guardar | Guarda un valor en una variable. `x = 5` significa "x guarda el valor 5". |
| `+` | Suma | Suma números o une textos. `3 + 2` = 5, `"hola" + " mundo"` = "hola mundo". |
| `-` | Resta | Resta números. `10 - 3` = 7. También funciona como negativo: `-5`. |
| `*` | Multiplicación | Multiplica números o repite textos. `"ha" * 3` = "hahaha". |
| `/` | División | Divide un número entre otro. `10 / 2` = 5.0 (siempre devuelve decimal). |
| `//` | División entera | Divide y redondea hacia abajo. `7 // 2` = 3. |
| `%` | Módulo / Resto | Devuelve el resto de una división. `7 % 2` = 1. |
| `**` | Potencia | Eleva un número a una potencia. `2 ** 3` = 8. |

---

## NIVEL 2: Variables y Tipos de Datos

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `str` | String / Cadena de texto | Tipo de dato para texto. `"Hola"`, `'Mundo'`, `"""Largo"""`. |
| `int` | Integer / Entero | Tipo de dato para números sin decimales. `42`, `-7`, `0`. |
| `float` | Flotante / Decimal | Tipo de dato para números con decimales. `3.14`, `-0.5`. |
| `bool` | Booleano / Verdadero o Falso | Tipo de dato solo tiene dos valores: `True` o `False`. |
| `None` | Nulo / Vacío | Representa "nada". Es el valor por defecto de variables sin valor. |
| `True` | Verdadero | Uno de los dos valores booleanos. Significa "sí" o "correcto". |
| `False` | Falso | Uno de los dos valores booleanos. Significa "no" o "incorrecto". |
| `type()` | Tipo | Muestra el tipo de dato de una variable. `type(42)` → `<class 'int'>`. |
| `str()` | Convertir a texto | Convierte cualquier cosa a texto. `str(42)` → `"42"`. |
| `int()` | Convertir a entero | Convierte a entero. `int("42")` → `42`. `int(3.9)` → `3`. |
| `float()` | Convertir a decimal | Convierte a decimal. `float("3.14")` → `3.14`. |
| `len()` | Longitud | Cuenta cuántos elementos tiene algo. `len("hola")` → `4`. |

---

## NIVEL 3: Estructuras de Datos

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `[]` | Lista (creación) | Crea una lista ordenada y modificable. `[1, 2, 3]`. |
| `()` | Tupla (creación) | Crea una tupla ordenada e INMUTABLE. `(1, 2, 3)`. |
| `{}` | Diccionario o Set | `{}` vacío = set. `{"k": "v"}` = diccionario. |
| `list` | Lista | Tipo de dato para colecciones ordenadas y modificables. |
| `tuple` | Tupla | Tipo de dato para colecciones ordenadas e inmutables. |
| `dict` | Diccionario | Tipo de dato para pares clave-valor. `{"nombre": "Ana"}`. |
| `set` | Conjunto | Tipo de dato sin orden, sin duplicados. `{1, 2, 3}`. |
| `frozenset` | Conjunto congelado | Set pero inmutable. No se puede modificar después de crearlo. |
| `.append()` | Agregar / Añadir | Agrega un elemento al final de una lista. `lista.append(4)`. |
| `.insert()` | Insertar | Agrega un elemento en una posición específica. `lista.insert(0, 1)`. |
| `.remove()` | Eliminar | Elimina la primera ocurrencia de un valor. `lista.remove(3)`. |
| `.pop()` | Sacar / Extraer | Elimina y devuelve el último elemento (o el de una posición). |
| `.sort()` | Ordenar | Ordena la lista de menor a mayor. `lista.sort()`. |
| `.reverse()` | Invertir | Invierte el orden de la lista. `lista.reverse()`. |
| `.copy()` | Copiar | Crea una copia de la lista (no una referencia). |
| `.keys()` | Claves | Devuelve todas las claves de un diccionario. |
| `.values()` | Valores | Devuelve todos los valores de un diccionario. |
| `.items()` | Elementos | Devuelve pares (clave, valor) de un diccionario. |
| `.get()` | Obtener | Obtiene un valor por clave sin error si no existe. `d.get("k", "default")`. |
| `.update()` | Actualizar | Actualiza un diccionario con otros pares clave-valor. |
| `.add()` | Agregar (set) | Agrega un elemento a un set. `s.add(4)`. |

---

## NIVEL 4: Condicionales (Decisiones)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `if` | Si / En caso de que | Ejecuta código SOLO SI una condición es verdadera. |
| `elif` | Si no, si / También si | Evalúa otra condición si la anterior era falsa. Abreviación de "else if". |
| `else` | Si no / De lo contrario | Ejecuta código cuando TODAS las condiciones anteriores son falsas. |
| `and` | Y / Ambos | Verdadero SOLO SI ambas condiciones son verdaderas. `x > 5 and x < 10`. |
| `or` | O / Al menos uno | Verdadero SI AL MENOS UNA condición es verdadera. |
| `not` | No / Negación | Invierte el valor booleano. `not True` → `False`. |
| `in` | En / Contenido en | Verifica si algo está dentro de algo. `"a" in "hola"` → `True`. |
| `is` | Es / Identidad | Compara si dos cosas son el Mismo objeto (no solo igual valor). `a is b`. |
| `is not` | No es | Compara si dos cosas NO son el mismo objeto. |
| `==` | Igual a | Compara si dos valores son iguales. `5 == 5` → `True`. |
| `!=` | Diferente a | Compara si dos valores son diferentes. `5 != 3` → `True`. |
| `>` | Mayor que | Compara si el izquierdo es mayor. `5 > 3` → `True`. |
| `<` | Menor que | Compara si el izquierdo es menor. `3 < 5` → `True`. |
| `>=` | Mayor o igual que | Compara si es mayor o igual. `5 >= 5` → `True`. |
| `<=` | Menor o igual que | Compara si es menor o igual. `3 <= 5` → `True`. |

---

## NIVEL 5: Bucles (Repetición)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `for` | Para / Por cada | Repite código para cada elemento de una secuencia. `for i in range(5)`. |
| `while` | Mientras / Mientras que | Repite código MIENTRAS una condición sea verdadera. |
| `break` | Romper / Salir | Detiene el bucle completamente y sale de él. |
| `continue` | Continuar / Saltar | Salta a la siguiente iteración del bucle, ignora el resto. |
| `range()` | Rango / Secuencia | Genera una secuencia de números. `range(5)` → 0,1,2,3,4. |
| `enumerate()` | Enumerar | Devuelve índice y valor al mismo tiempo. `enumerate(lista)` → `(0, val), (1, val)...`. |
| `zip()` | Cremallera / Empaquetar | Une dos listas elemento por elemento. `zip([1,2], ["a","b"])`. |
| `reversed()` | Invertido | Invierte una secuencia sin modificarla. `reversed([1,2,3])` → 3,2,1. |
| `iter()` | Iterador | Crea un iterador desde una secuencia. Se usa con `next()`. |
| `next()` | Siguiente | Obtiene el siguiente elemento de un iterador. |

---

## NIVEL 6: Funciones

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `def` | Definir / Definición | Crea una función nueva. `def saludar():`. |
| `return` | Devolver / Retornar | Hace que una función devuelva un valor. `return 42`. |
| `yield` | Producir / Entregar | Como return pero pausa la función. Crea un generador (lazy evaluation). |
| `lambda` | Función anónima | Crea funciones pequeñas en una línea. `lambda x: x * 2`. |
| `*args` | Argumentos posicionales | Permite pasar cualquier número de argumentos a una función como tupla. |
| `**kwargs` | Argumentos con palabra clave | Permite pasar cualquier número de argumentos nombrados como diccionario. |
| `global` | Global | Indica que una variable viene del scope global (fuera de la función). |
| `nonlocal` | No local | Indica que una variable viene del scope del padre (no global, no local). |
| `@` | Decorador | Aplica una función a otra función. `@staticmethod`, `@login_required`. |
| `pass` | Pasar / No hacer nada | Placeholder. Se usa cuando el código necesita algo pero no hace nada. |

---

## NIVEL 7: Manejo de Errores

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `try` | Intentar / Probar | Intenta ejecutar código que podría fallar. |
| `except` | Capturar / Atrapar | Captura un error y ejecuta código alternativo. |
| `else` | Si no hay error | Se ejecuta SOLO SI no hubo excepciones en el try. |
| `finally` | Siempre / Finalmente | Se ejecuta SIEMPRE, haya error o no. Ideal para limpiar recursos. |
| `raise` | Lanzar / Levantar | Lanza una excepción manualmente. `raise ValueError("mal")`. |
| `Exception` | Excepción general | La excepción base para la mayoría de errores. |
| `ValueError` | Error de valor | Se lanza cuando un valor no es válido. `int("abc")`. |
| `TypeError` | Error de tipo | Se lanza cuando el tipo de dato es incorrecto. `"3" + 3`. |
| `KeyError` | Error de clave | Se lanza cuando no existe una clave en un diccionario. |
| `IndexError` | Error de índice | Se lanza cuando el índice está fuera de rango. `lista[100]`. |
| `FileNotFoundError` | Archivo no encontrado | Se lanza cuando no se encuentra un archivo. |
| `ImportError` | Error de importación | Se lanza cuando no se puede importar un módulo. |
| `AttributeError` | Error de atributo | Se lanza cuando un objeto no tiene ese atributo/método. |
| `StopIteration` | Fin de iteración | Se lanza cuando un iterador no tiene más elementos. |

---

## NIVEL 8: Entrada/Salida de Archivos

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `open()` | Abrir | Abre un archivo. `open("archivo.txt", "r")`. |
| `with` | Con / Usando | Abre un recurso que se cierra automáticamente al salir del bloque. |
| `"r"` | Lectura / Read | Modo de apertura para leer un archivo. |
| `"w"` | Escritura / Write | Modo para escribir (sobrescribe si existe). |
| `"a"` | Adjuntar / Append | Modo para agregar al final sin sobrescribir. |
| `"rb"` | Lectura binaria | Leer archivos binarios (imágenes, etc). |
| `"wb"` | Escritura binaria | Escribir archivos binarios. |
| `.read()` | Leer todo | Lee todo el contenido del archivo como texto. |
| `.readline()` | Leer línea | Lee una sola línea del archivo. |
| `.readlines()` | Leer líneas | Lee todas las líneas y las devuelve como lista. |
| `.write()` | Escribir | Escribe texto en el archivo. |
| `.close()` | Cerrar | Cierra el archivo. Con `with` se hace automáticamente. |
| `pathlib` | Rutas de archivo | Módulo moderno para manejar rutas de archivos de forma elegante. |
| `Path()` | Ruta | Crea un objeto de ruta. `Path("carpeta/archivo.txt")`. |

---

## NIVEL 9: Módulos y Paquetes

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `import` | Importar | Trae un módulo o función para usarlo. `import math`. |
| `from` | Desde | Importa algo específico de un módulo. `from math import sqrt`. |
| `as` | Como / Alias | Da un nombre corto a algo importado. `import numpy as np`. |
| `__name__` | Nombre del módulo | Variable especial: es `"__main__"` si se ejecuta directamente. |
| `__init__.py` | Inicialización de paquete | Archivo que marca un directorio como paquete Python. |
| `__all__` | Lista pública | Define qué se exporta al hacer `from modulo import *`. |
| `sys` | Sistema | Módulo para acceder al sistema operativo y argumentos. |
| `os` | Sistema operativo | Módulo para interactuar con el SO (archivos, directorios, etc). |
| `math` | Matemáticas | Funciones matemáticas: `sqrt`, `pi`, `log`, `sin`, etc. |
| `random` | Aleatorio | Genera números y selecciones aleatorias. |
| `datetime` | Fecha y hora | Trabaja con fechas, horas y tiempo. |
| `json` | JSON | Lee y escribe datos en formato JSON (intercambio de datos). |
| `re` | Expresiones regulares | Busca y manipula patrones en textos. Muy potente y complejo. |
| `collections` | Colecciones especializadas | Contenedores extra: `Counter`, `defaultdict`, `deque`, `OrderedDict`. |
| `functools` | Funciones de utilidad | Herramientas para funciones: `lru_cache`, `partial`, `reduce`. |
| `itertools` | Iteradores de utilidad | Iteradores eficientes: `chain`, `product`, `combinations`. |
| `typing` | Tipado | Anotaciones de tipo: `List`, `Dict`, `Optional`, `TypeVar`. |
| `abc` | Clases abstractas | Crea clases que no se pueden instanciar directamente (interfaces). |
| `dataclasses` | Clases de datos | Crea clases automáticamente con `__init__`, `__repr__`, `__eq__`. |
| `enum` | Enumeraciones | Crea constantes con nombre. `class Color(Enum): ROJO = 1`. |
| `logging` | Registro / Bitácora | Sistema profesional de logs (reemplaza `print` en producción). |
| `unittest` | Pruebas unitarias | Framework para probar que el código funciona correctamente. |
| `pytest` | Pruebas (alternativa) | Framework de testing más popular y fácil que unittest. |
| `argparse` | Análisis de argumentos | Parsea argumentos de línea de comandos. Para scripts profesionales. |

---

## NIVEL 10: POO (Programación Orientada a Objetos)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `class` | Clase | Crea un molde/tipo de objeto. Define qué datos y acciones tendrá. |
| `self` | Yo mismo / Esta instancia | Referencia al objeto actual dentro de una clase. Siempre el primer parámetro. |
| `__init__()` | Constructor / Inicializar | Se ejecuta al crear un objeto. Define qué datos iniciales tiene. |
| `__str__()` | Representación en texto | Devuelve cómo se ve un objeto como texto. `str(obj)`. |
| `__repr__()` | Representación técnica | Devuelve representación para depuradores. Debe ser claro para programadores. |
| `__eq__()` | Igualdad | Define qué significa que dos objetos sean iguales (`==`). |
| `__lt__()` | Menor que | Define el orden de objetos (`<`, `>`). Para sorting. |
| `__len__()` | Longitud | Define qué devuelve `len(obj)`. |
| `__getitem__()` | Obtener elemento | Define cómo acceder por índice: `obj[0]`. |
| `__setitem__()` | Establecer elemento | Define cómo asignar por índice: `obj[0] = valor`. |
| `__call__()` | Ejecutable / Callable | Permite llamar a un objeto como función: `obj()`. |
| `__enter__()` | Entrar contexto | Se ejecuta al entrar a un bloque `with`. Para recursos. |
| `__exit__()` | Salir contexto | Se ejecuta al salir de un bloque `with`. Limpia recursos. |
| `__add__()` | Suma de objetos | Define qué hace `obj1 + obj2`. |
| `__mul__()` | Multiplicación | Define qué hace `obj1 * obj2`. |
| `__contains__()` | Contiene | Define qué hace `"algo" in obj`. |
| `__iter__()` | Iterador | Hace que un objeto sea iterable con `for`. |
| `__next__()` | Siguiente | Define qué devuelve `next(obj)`. Para iteradores. |
| `property` | Propiedad | Convierte un método en atributo: `obj.nombre` en vez de `obj.nombre()`. |
| `staticmethod` | Método estático | Método que no necesita `self`. Se llama desde la clase, no desde un objeto. |
| `classmethod` | Método de clase | Recibe la clase (`cls`) en vez de la instancia (`self`). |
| `@dataclass` | Clase de datos | Genera automáticamente `__init__`, `__repr__`, `__eq__` para la clase. |
| `frozen` | Congelado / Inmutable | Hace que un dataclass no se pueda modificar después de crearlo. |
| `slots` | Ranuras / Espacios | Ahorra memoria definiendo los atributos permitidos de antemano. |
| `super()` | Superclase / Padre | Llama al método del padre. Se usa en herencia. |
| `isinstance()` | Es instancia de | Verifica si un objeto es de una clase: `isinstance(x, int)`. |
| `issubclass()` | Es subclase de | Verifica si una clase hereda de otra. |
| `getattr()` | Obtener atributo | Obtiene un atributo por nombre: `getattr(obj, "nombre")`. |
| `setattr()` | Establecer atributo | Establece un atributo por nombre: `setattr(obj, "nombre", val)`. |
| `hasattr()` | Tiene atributo | Verifica si un objeto tiene un atributo: `hasattr(obj, "nombre")`. |
| `vars()` | Variables | Devuelve el diccionario de atributos de un objeto. |
| `dir()` | Directorio | Lista todos los atributos y métodos disponibles de un objeto. |
| `callable()` | Ejecutable | Verifica si algo se puede llamar como función: `callable(print)`. |

---

## NIVEL 11: Funciones Lambda y Avanzadas

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `map()` | Mapear / Aplicar a todos | Aplica una función a cada elemento. `map(str, [1,2])` → `["1","2"]`. |
| `filter()` | Filtrar / Seleccionar | Filtra elementos que cumplan una condición. |
| `reduce()` | Reducir / Acumular | Aplica una función acumulativa. `reduce(lambda a,b: a+b, [1,2,3])` → `6`. |
| `sorted()` | Ordenar | Devuelve una lista nueva ordenada. No modifica la original. |
| `reversed()` | Invertir | Devuelve una secuencia invertida. |
| `any()` | Alguno / Cualquiera | Devuelve True si AL MENOS UNO cumple. `any([0,1,0])` → `True`. |
| `all()` | Todos / Todos cumplen | Devuelve True si TODOS cumplen. `all([1,1,1])` → `True`. |
| `sum()` | Sumar | Suma todos los elementos. `sum([1,2,3])` → `6`. |
| `min()` | Mínimo | Devuelve el valor más pequeño. |
| `max()` | Máximo | Devuelve el valor más grande. |
| `abs()` | Absoluto | Devuelve el valor absoluto. `abs(-5)` → `5`. |
| `round()` | Redondear | Redondea un número. `round(3.14, 1)` → `3.1`. |
| `divmod()` | División completa | Devuelve cociente y resto juntos. `divmod(7,3)` → `(2,1)`. |
| `pow()` | Potencia | Eleva a la potencia. `pow(2,10)` → `1024`. |
| `id()` | Identidad | Devuelve la dirección de memoria de un objeto. |
| `hash()` | Hash / Resumen | Convierte un valor a un número hash. Para usar como clave de dict. |
| `callable()` | Llamable | Verifica si algo se puede ejecutar como función. |
| `exec()` | Ejecutar | Ejecuta código Python dinámicamente desde un string. Peligroso. |
| `eval()` | Evaluar | Evalúa una expresión Python desde un string. Peligroso. |
| `compile()` | Compilar | Compila código fuente a bytecode para ejecutarlo después. |

---

## NIVEL 12: Comprensiones (Sintaxis Especial)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `[x for x in ...]` | Comprensión de lista | Crea una lista de forma compacta. `[x**2 for x in range(10)]`. |
| `{k:v for k,v in ...}` | Comprensión de dict | Crea un diccionario de forma compacta. `{k:v for k,v in d.items()}`. |
| `{x for x in ...}` | Comprensión de set | Crea un set de forma compacta. `{x for x in [1,1,2,3]}` → `{1,2,3}`. |
| `(x for x in ...)` | Expresión generadora | Crea un generador (lazy, no carga en memoria). |

---

## NIVEL 13: Context Managers y Recursos

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `with` | Con / Usando | Abre un recurso que se cierra solo. `with open("f.txt") as f:`. |
| `as` | Como / Alias | Da un nombre al recurso abierto. `as f`. |
| `contextmanager` | Gestor de contexto | Decorador que convierte una función en context manager. |
| `suppress()` | Suprimir | Ignora excepciones específicas. `suppress(FileNotFoundError)`. |
| `redirect_stdout()` | Redirigir salida | Redirige el print a otro destino ( archivo, buffer, etc). |

---

## NIVEL 14: Asincronía (async/await)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `async` | Asíncrono | Define una función que puede pausarse y continuar después. |
| `await` | Esperar | Pausa la función hasta que algo termine (I/O, red, etc). |
| `asyncio` | Librería de asincronía | Módulo para programación asíncrona. Gestiona event loop. |
| `gather()` | Reunir / Juntar | Ejecuta múltiples tareas asíncronas en paralelo. |
| `create_task()` | Crear tarea | Lanza una tarea asíncrona sin esperar que termine. |
| `Queue()` | Cola | Cola para comunicación entre productor y consumidor asíncronos. |
| `sleep()` | Dormir / Esperar | Pausa asíncrona (no bloquea otros procesos). |
| `TaskGroup` | Grupo de tareas | Agrupa múltiples tareas con manejo de errores (3.11+). |
| `__aenter__()` | Entrar contexto async | Context manager asíncrono (para recursos de red). |
| `__aexit__()` | Salir contexto async | Limpia recursos asíncronos. |

---

## NIVEL 15: Type Hints (Anotaciones de Tipo)

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `:` | Tipo | Anota el tipo de una variable. `nombre: str = "Ana"`. |
| `->` | Retorna tipo | Anota qué devuelve una función. `def f() -> int:`. |
| `Union` | Unión / O | Un tipo u otro. `Union[int, str]` → int o str. |
| `Optional` | Opcional | Un tipo o None. `Optional[int]` → int o None. |
| `TypeVar` | Tipo variable | Crea un tipo genérico reutilizable. `T = TypeVar("T")`. |
| `Protocol` | Protocolo / Interfaz | Define estructura sin herencia. Structural subtyping. |
| `Annotated` | Anotado | Agrega metadata a tipos. `Annotated[int, Gt(0)]`. |
| `Literal` | Literal / Exacto | Solo acepta valores exactos. `Literal["a", "b"]`. |
| `TypeAlias` | Alias de tipo | Da un nombre abreviado a un tipo. `Vector = list[float]`. |
| `reveal_type()` | Revelar tipo | Muestra qué tipo infiere mypy. Solo para verificación estática. |
| `cast()` | Convertir tipo | Le dice a mypy que un valor es de cierto tipo (sin cambiarlo). |
| `ParamSpec` | Parámetros variable | Captura los parámetros de una función para decoradores. |
| `Concatenate` | Concatenar tipos | Agrega parámetros al tipo de una función. |

---

## NIVEL 16: Concurrencia y Paralelismo

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `threading` | Hilos / Threads | Ejecuta múltiples tareas al mismo tiempo (I/O-bound). |
| `multiprocessing` | Multiproceso | Ejecuta en procesos separados (CPU-bound). Salta el GIL. |
| `ThreadPoolExecutor` | Executor de hilos | Gestiona un pool de threads. Más fácil que crear threads manualmente. |
| `ProcessPoolExecutor` | Executor de procesos | Gestiona un pool de procesos. Para cálculos pesados. |
| `concurrent.futures` | Futures concurrentes | API de alto nivel para concurrencia con futures. |
| `GIL` | Global Interpreter Lock | Lock que permite solo un thread ejecutando Python. Limitación histórica. |
| `free-threaded` | Sin GIL | Python 3.13+ puede ejecutar sin GIL (experimental). |

---

## NIVEL 17: Metaprogramación

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `type()` | Tipo (también crea clases) | Crea clases dinámicamente: `type("Clase", (Base,), {})`. |
| `metaclass` | Metaclase | Clase de una clase. Intercepta la creación de clases. |
| `__new__()` | Nuevo | Crea una instancia (antes de `__init__`). Para inmutables y singletons. |
| `__init_subclass__()` | Subclase init | Hook que se ejecuta cuando una clase hereda de esta. Para registrar plugins. |
| `__set_name__()` | Establecer nombre | Descriptor que recibe el nombre del atributo al definir la clase. |
| `descriptor` | Descriptor | Objeto con `__get__`/`__set__` que controla acceso a atributos. |
| `__slots__` | Espacios / Ranuras | Define atributos permitidos y ahorra memoria (no crea `__dict__`). |
| `__dict__` | Diccionario de attrs | Diccionario con todos los atributos de un objeto/instancia. |
| `__class__` | Clase | Referencia a la clase de un objeto. |
| `__module__` | Módulo | Módulo donde se definió la clase/función. |
| `__qualname__` | Nombre cualificado | Nombre completo de la función/clase (incluye anidados). |
| `__doc__` | Documentación | Docstring de la función/clase. |

---

## NIVEL 18: Funciones Built-in Especializadas

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `super()` | Superclase | Accede a métodos de la clase padre en herencia múltiple. |
| `property()` | Propiedad | Convierte un método en atributo calculado. |
| `classmethod()` | Método de clase | Decorador que recibe la clase en vez de la instancia. |
| `staticmethod()` | Método estático | Decorador para métodos que no necesitan ni `self` ni `cls`. |
| `del` | Eliminar | Elimina una variable o elemento. `del lista[0]`. |
| `assert` | Afirmar / Verificar | Verifica que algo sea verdadero, si no lanza AssertionError. |
| `importlib` | Importar dinámico | Importa módulos en runtime: `importlib.import_module("mod")`. |
| `pickle` | Serializar | Convierte objetos Python a bytes para guardarlos. `pickle.dump()`. |
| `copy` | Copiar | Copia superficiales (`copy()`) o profundas (`deepcopy()`) de objetos. |
| `gc` | Garbage Collector | Control del recolector de basura. Para depurar memory leaks. |
| `tracemalloc` | Trazar memoria | Rastrea asignación de memoria. Para encontrar memory leaks. |
| `timeit` | Cronometrar | Mide el tiempo de ejecución de código de forma precisa. |
| `perf_counter()` | Contador de rendimiento | Reloj de alta resolución para medir tiempo. El más preciso. |
| `memoryview()` | Vista de memoria | Accede a buffers sin copiar datos. Para optimización. |
| `bytearray()` | Arreglo de bytes | Como `bytes` pero mutable. Para manipular datos binarios. |
| `struct` | Estructura | Convierte entre datos Python y bytes C-style. Para protocolos binarios. |
| `sqlite3` | SQLite | Cliente SQLite integrado. Base de datos en un archivo. |
| `email` | Correo | Genera y parsea emails. Para enviar correos desde Python. |
| `csv` | CSV | Lee y escribe archivos CSV. Alternativa a pandas para archivos simples. |
| `xml` | XML | Parsea archivos XML. |
| `html` | HTML | Escapa y formatea HTML. |
| `urllib` | URL | Abre y descarga URLs. Para web scraping básico. |
| `http` | HTTP | Cliente HTTP integrado. |
| `socket` | Socket | Comunicación de bajo nivel entre máquinas. |
| `threading` | Hilos | Ejecución concurrente con threads. |
| `multiprocessing` | Multiproceso | Ejecución paralela con procesos separados. |
| `subprocess` | Subproceso | Ejecuta comandos del sistema operativo. |
| `shutil` | Shell Utilities | Copia, mueve y elimina archivos/directorios. |
| `tempfile` | Archivos temporales | Crea archivos y directorios temporales seguros. |
| `hashlib` | Hash | Genera hashes: MD5, SHA-256, etc. Para contraseñas y verificación. |
| `secrets` | Secretos | Genera números/tokens criptográficamente seguros. Para passwords. |
| `uuid` | UUID | Genera identificadores únicos universales. |
| `decimal` | Decimal | Números decimales de precisión exacta (para dinero). |
| `fractions` | Fracciones | Números racionales como fracciones exactas. |

---

## NIVEL 19: Decoradores Especiales de Python

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `@property` | Propiedad | Convierte método en atributo de solo lectura. |
| `@setter` | Establecedor | Define cómo se asigna valor a una propiedad. |
| `@deleter` | Eliminador | Define qué pasa al hacer `del obj.atributo`. |
| `@staticmethod` | Estático | Método que no necesita ni instancia ni clase. Como función pero dentro de la clase. |
| `@classmethod` | De clase | Recibe la clase como primer argumento. Útil para factory methods. |
| `@abstractmethod` | Abstracto | Obliga a que las subclases implementen este método. |
| `@functools.wraps` | Envolver | Preserva metadata original al usar decorators. SIEMPRE usar en decorators. |
| `@lru_cache` | Cache LRU | Cachea resultados de función (Least Recently Used). Para cálculos repetidos. |
| `@total_ordering` | Orden total | Genera automáticamente métodos de comparación faltantes. |
| `@contextmanager` | Gestor de contexto | Convierte una función en context manager con `yield`. |
| `@dataclass` | Clase de datos | Genera `__init__`, `__repr__`, `__eq__` automáticamente. |
| `@retry` | Reintentar | Decorador custom para reintentar funciones que fallan. |
| `@timer` | Cronometrar | Decorador custom que mide tiempo de ejecución. |
| `@rate_limit` | Límite de tasa | Decorador custom para limitar llamadas por tiempo. |

---

## NIVEL 20: Operadores Especiales

| Palabra/Función | Significado en Español | Para qué se usa |
|----------------|----------------------|-----------------|
| `:=` | Walrus / Asignación en expresión | Asigna y usa en la misma línea. `if (n := len(x)) > 5:`. |
| `@` | Multiplicación matricial | Multiplica matrices (numpy). `A @ B` = producto matricial. |
| `...` | Ellipsis / Puntos suspensivos | Placeholder en funciones, slices, y tipos. |
| `:=` | Asignación walrus | Evalúa una expresión y la guarda en una variable dentro de otra expresión. |
| `|` | Unión de tipos (3.10+) | `int | str` en vez de `Union[int, str]`. |
| `match` | Coincidir / Pattern matching | Like switch/case pero más poderoso (3.10+). |
| `case` | Caso | Define un patrón en match. |
| `_` | Variable descarte | Convención para valores que no se usan. `for _ in range(5)`. |
| `__` | Doble guión bajo | Prefijo/sufijo para métodos especiales de Python. |
| `_` | Separador de miles | `1_000_000` = 1000000. Mejor legibilidad. |

---

## NIVEL 21: Excepciones Completo

| Excepción | Cuándo ocurre | Ejemplo |
|-----------|---------------|---------|
| `ValueError` | Valor inválido | `int("abc")` |
| `TypeError` | Tipo incorrecto | `"3" + 3` |
| `KeyError` | Clave no existe | `{"a":1}["b"]` |
| `IndexError` | Índice fuera de rango | `[1,2][5]` |
| `AttributeError` | Atributo no existe | `"hi".foob` |
| `FileNotFoundError` | Archivo no encontrado | `open("no_existe.txt")` |
| `ImportError` | Error al importar | `import modulo_falso` |
| `StopIteration` | Iterador agotado | `next(iter([]))` |
| `ZeroDivisionError` | División por cero | `1/0` |
| `RecursionError` | Mucha recursión | `def f(): f()` |
| `MemoryError` | Sin memoria | `list(range(10**10))` |
| `OverflowError` | Número demasiado grande | `math.exp(1000)` |
| `AssertionError` | Assert falló | `assert False, "error"` |
| `NameError` | Variable no definida | `print(variable_que_no_existe)` |
| `UnboundLocalError` | Variable local sin valor | Usar antes de asignar en función |
| `RuntimeError` | Error en runtime | Error general no clasificado |
| `NotImplementedError` | Método no implementado | En clases abstractas |
| `IOError` | Error de entrada/salida | Problemas con archivos |
| `PermissionError` | Sin permisos | Abrir archivo sin permiso |
| `ConnectionError` | Error de conexión | Internet caído |
| `TimeoutError` | Tiempo agotado | Request que tarda demasiado |
| `SyntaxError` | Error de sintaxis | Código mal escrito |
| `IndentationError` | Error de sangría | Indentación inconsistente |
| `TabError` | Error de tabulación | Mezclar tabs y espacios |
| `EOFError` | Fin de archivo inesperado | `input()` en archivo sin datos |

---

## NIVEL 22: Convenciones de Nombres (Lo que significa cada prefijo/sufijo)

| Convención | Ejemplo | Significado |
|-----------|---------|-------------|
| `_variable` | `_contador` | Privado por convención (no acceder fuera de la clase). |
| `__variable` | `__password` | Name mangling — Python lo renombra para evitar conflictos. |
| `__variable__` | `__init__` | Método especial de Python. No crear los tuyos con este formato. |
| `variable_` | `class_` | Evita conflicto con palabra reservada. `class_` en vez de `class`. |
| `MAX_VALOR` | `MAX_CONEXIONES` | Constante — no se debería modificar. |
| `ClaseNombre` | `MiClase` | CamelCase para clases. |
| `nombre_funcion` | `calcular_total` | snake_case para funciones y variables. |
| `FILENAME` | `CONFIG_PATH` | SCREAMING_SNAKE para constantes globales. |
| `i`, `j`, `k` | `for i in range(10)` | Iteradores genéricos. |
| `_` | `for _ in range(5)` | Valor descarte (no se usa). |
| `fn`, `cb` | `callback`, `func` | Abreviaciones comunes de función. |
| `df` | `df = pd.DataFrame()` | DataFrame en pandas. |
| `ax` | `ax = plt.subplot()` | Axes en matplotlib. |
| `fig` | `fig = plt.figure()` | Figure en matplotlib. |
| `np`, `pd` | `import numpy as np` | Alias estándar de librerías. |
