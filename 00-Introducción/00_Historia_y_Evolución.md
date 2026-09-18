# Historia y Evolución de Python (1989 — Septiembre 2026)

## Origen (1989-1991)

Python fue creado por **Guido van Rossum** en el **CWI** (Países Bajos) durante las vacaciones de Navidad de 1989. Su primera versión pública, **Python 0.9.0**, se lanzó en **febrero de 1991**. El nombre proviene de **"Monty Python's Flying Circus"**, no de la serpiente.

Guido buscaba un sucesor para **ABC** (lenguaje educativo) que fuera:
- Fácil de aprender pero potente
- Sintaxis clara y legible
- Soporte para excepciones y módulos
- Interfaz con el sistema operativo

---

## Evolución por Eras

### Era 1: Los Cimientos (1991-2000)
| Versión | Año | Innovación Clave |
|---------|-----|-------------------|
| 0.9.0 | 1991 | Clases, excepciones, funciones, módulos |
| 1.0 | 1994 | `lambda`, `map`, `filter`, `reduce` |
| 1.5 | 1998 | Soporte para Arabic e Hebrew, documentación mejorada |
| 1.6 | 2000 | Acceso a C, `tkinter`, `email` |

### Era 2: Python 2.x (2000-2010)
| Versión | Año | Innovación Clave |
|---------|-----|-------------------|
| 2.0 | 2000 | List comprehensions, garbage collection cíclico |
| 2.2 | 2001 | New-style classes, iteradores, generators |
| 2.4 | 2004 | Decorators, generator expressions, `with` statement |
| 2.5 | 2006 | `conditional expressions`, `with` nativo |
| 2.6 | 2008 | `print()` como función (opcional), `json`, `argparse` |
| 2.7 | 2010 | Última versión 2.x, `set` literals, `pytest` |

### Era 3: Python 3.x — La Gran Ruptura (2008-2020)
| Versión | Año | Innovación Clave |
|---------|-----|-------------------|
| 3.0 | 2008 | `print()` función, int/long unificado, Unicode nativo |
| 3.3 | 2012 | `yield from`, `venv`, `unittest.mock` |
| 3.4 | 2014 | `enum`, `pathlib`, `asyncio`, pip integrado |
| 3.5 | 2015 | **`async`/`await`**, type hints (`typing`), `@` matrix multiply |
| 3.6 | 2016 | **f-strings**, `__init_subclass__`, variable annotations |
| 3.7 | 2018 | **Dataclasses**, `breakpoint()`, async generators, dict order |
| 3.8 | 2019 | Walrus operator `:=`, positional-only params, f-strings `=` |
| 3.9 | 2020 | Dictionary merge `|`, type hints genéricos, `zoneinfo` |

### Era 4: La Edad de Oro (2021-2026)
| Versión | Año | Innovación Clave |
|---------|-----|-------------------|
| 3.10 | 2021 | **Pattern matching** (`match`/`case`), mejoras en errores |
| 3.11 | 2022 | **60% más rápido** (Faster CPython), `ExceptionGroup`, `tomllib` |
| 3.12 | 2023 | **25% más rápido** aún, f-strings multilínea, type defaults, PEP 695 |
| 3.13 | 2024 | **JIT compiler** experimental, **free-threaded Python** (sin GIL), `@overload` mejorado |
| 3.14 | 2025 | **Template strings** (t-strings), deferred evaluation, `annotationlib`, `sqlite3` mejorado |
| 3.15 | Sept 2026 | **PEG parser** optimizado, mejoras en JIT, `typing` más expresivo, compilation speed +30% |

---

## El Fin del GIL (2023-2026)

El **Global Interpreter Lock** fue el talón de Aquiles de Python durante 30 años. La evolución:

```
Python 3.13 (2024): free-threaded Python (PEP 703) — experimental
Python 3.14 (2025): opt-in por defecto, mejor soporte para C extensions
Python 3.15 (2026): habilitado por defecto en nuevos proyectos, estabilidad completa
```

**Impacto real:** Python ahora puede ejecutar código CPU-bound en paralelo real sin multiprocessing, sin overhead de serialización, sin GIL. Esto cambia fundamentalmente la arquitectura de aplicaciones de alto rendimiento.

---

## Python en la Industria (Septiembre 2026)

### Dominio de Mercado
- **#1 en IEEE Spectrum** (2024-2026 consecutivos)
- **#1 en TIOBE** desde 2021
- **67%** de los científicos de datos lo usan como lenguaje principal
- **42%** de los desarrolladores de IA lo usan para modelado

### Empresas que Dependen de Python
| Empresa | Uso Principal |
|---------|---------------|
| OpenAI | Entrenamiento de GPT, API, herramientas internas |
| Google | TensorFlow, Vertex AI, infraestructura interna |
| Meta | PyTorch, DALL-E, investigación |
| Netflix | Análisis de datos, ML, automatización |
| Tesla | Autopilot (modelos de ML), data pipelines |
| NASA | Análisis científico, simulaciones |
| JP Morgan | Risk analytics, trading systems |
| Spotify | Recomendaciones, data engineering |

### Ecosistema en 2026
- **PyPI**: >500,000 paquetes
- **Conda**: ecosistema científico consolidado
- **uv**: gestor de paquetes ultrarrápido (reemplaza pip)
- **Ruff**: linter/formatter en Rust (reemplaza black+flake8+isort)
- **Mojo**: superset de Python para HPC (compatibilidad parcial)
- **Codon**: compilador Python nativo (subconjunto)

---

## ¿Por qué Python Domina en 2026?

1. **Universalidad:** Web, datos, IA, automatización, scripts, DevOps
2. **Ecosistema maduro:** >35 años de librerías probadas
3. **Comunidad:** >10M de desarrolladores activos
4. **Velocidad:** Python 3.13+ es 2-3x más rápido que Python 3.9
5. **IA nativa:** Cada modelo de IA se entrena y despliega con Python
6. **Sin GIL:** La barrera histórica de paralelismo fue eliminada
7. **Type hints maduros:** `typing` permite código robusto verificable estáticamente
8. **Herramientas modernas:** uv, ruff, mypy — desarrollo Python nunca fue tan rápido
