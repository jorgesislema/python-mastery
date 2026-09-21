# Glosario Actualizado — Python (Septiembre 2026)

## Métodos Esenciales

| Método | Uso | Ejemplo |
|--------|-----|---------|
| `.strip()` | Eliminar espacios | `" hola ".strip()` → `"hola"` |
| `.split()` | Dividir cadena | `"a,b,c".split(",")` → `["a","b","c"]` |
| `.join()` | Unir cadena | `",".join(["a","b"])` → `"a,b"` |
| `.replace()` | Reemplazar | `"hola".replace("o","0")` → `"h0la"` |
| `.encode()` | A bytes | `"hola".encode()` → `b"hola"` |
| `.decode()` | De bytes | `b"hola".decode()` → `"hola"` |
| `.format()` | Formatear | `"{}:{}".format("a",1)` → `"a:1"` |
| `.startswith()` | Prefijo | `"hola".startswith("ho")` → `True` |
| `.endswith()` | Sufijo | `"hola".endswith("la")` → `True` |
| `.find()` | Buscar índice | `"hola".find("ola")` → `1` |
| `.count()` | Contar ocurrencias | `"aab".count("a")` → `2` |
| `.zfill()` | Rellenar con ceros | `"42".zfill(5)` → `"00042"` |
| `.title()` | Title case | `"hola mundo".title()` → `"Hola Mundo"` |
| `.capitalize()` | Primera mayús | `"hola".capitalize()` → `"Hola"` |

---

## Funciones Built-in Avanzadas

| Función | Uso | Ejemplo |
|---------|-----|---------|
| `map(func, iterable)` | Aplicar función | `list(map(str, [1,2]))` → `["1","2"]` |
| `filter(func, iterable)` | Filtrar | `list(filter(bool, [0,1,""]))` → `[1]` |
| `zip(*iterables)` | Empaquetar | `zip([1,2],["a","b"])` → `[(1,"a"),(2,"b")]` |
| `enumerate(iter, start)` | Índice+valor | `enumerate(["a","b"],1)` → `[(1,"a"),(2,"b")]` |
| `reduce(func, iterable)` | Acumular | `reduce(lambda a,b: a+b, [1,2,3])` → `6` |
| `sorted(iter, key)` | Ordenar | `sorted([3,1,2], reverse=True)` → `[3,2,1]` |
| `reversed(iter)` | Invertir | `list(reversed([1,2,3]))` → `[3,2,1]` |
| `any(iterable)` | ¿Alguna? | `any([0,1,0])` → `True` |
| `all(iterable)` | ¿Todas? | `all([1,1,1])` → `True` |
| `divmod(a,b)` | División completa | `divmod(7,3)` → `(2,1)` |
| `pow(a,b)` | Potencia | `pow(2,10)` → `1024` |
| `id(obj)` | Identidad | `id(x)` → dirección memoria |
| `isinstance(obj, cls)` | Tipo | `isinstance(42, int)` → `True` |
| `hasattr(obj, name)` | Atributo | `hasattr(x, "name")` → `True` |
| `getattr(obj, name)` | Obtener attr | `getattr(x, "name")` → valor |
| `setattr(obj, name, v)` | Setter | `setattr(x, "name", "Ana")` |
| `vars(obj)` | __dict__ | `vars(x)` → dict atributos |
| `dir(obj)` | Atributos | `dir(str)` → métodos de str |
| `callable(obj)` | ¿Ejecutable? | `callable(print)` → `True` |

---

## Librerías por Categoría (Septiembre 2026)

### Data Science
| Librería | Versión | Uso |
|----------|---------|-----|
| NumPy | 2.x | Arrays numéricos, computación vectorizada |
| Pandas | 3.x | DataFrames, análisis de datos |
| Polars | 2.x | DataFrames ultrarrápidos (Rust) |
| SciPy | 1.14+ | Científico, estadística, optimización |
| Statsmodels | 0.14+ | Modelos estadísticos |

### Machine Learning
| Librería | Versión | Uso |
|----------|---------|-----|
| Scikit-learn | 1.6+ | ML clásico (regresión, clasificación, clustering) |
| XGBoost | 2.1+ | Gradient boosting |
| LightGBM | 4.5+ | Gradient boosting ultrarrápido |
| CatBoost | 1.2+ | Gradient boosting con categorical features |

### Deep Learning
| Librería | Versión | Uso |
|----------|---------|-----|
| PyTorch | 3.x | Framework DL principal |
| TensorFlow | 2.19+ | Framework DL (Google) |
| JAX | 0.5+ | Autodiff + XLA (Google Research) |
| Keras | 3.x | API de alto nivel multi-backend |

### IA y LLMs
| Librería | Versión | Uso |
|----------|---------|-----|
| LangChain | 0.3+ | Framework para LLM apps |
| LangGraph | 0.2+ | Agentes con grafos de estados |
| OpenAI SDK | 2.x | API de OpenAI |
| Anthropic SDK | 0.40+ | API de Claude |
| ChromaDB | 0.6+ | Vector database |
| FAISS | 1.9+ | Similaridad vectorial (Meta) |
| Sentence-Transformers | 3.x | Embeddings |

### Backend
| Librería | Versión | Uso |
|----------|---------|-----|
| FastAPI | 0.115+ | API web async |
| Flask | 3.1+ | API web microframework |
| SQLAlchemy | 2.x | ORM para SQL |
| Pydantic | 3.x | Validación de datos |
| Alembic | 1.14+ | Migraciones de BD |
| uvicorn | 0.34+ | ASGI server |

### Ingeniería de Datos
| Librería | Versión | Uso |
|----------|---------|-----|
| PySpark | 4.0 | Apache Spark |
| Delta Lake | 4.0 | Data lake con ACID |
| Apache Airflow | 3.x | Orchestration |
| Kafka (aiokafka) | 0.12+ | Streaming |
| Great Expectations | 0.18+ | Data quality |

### Visualización
| Librería | Versión | Uso |
|----------|---------|-----|
| Matplotlib | 3.9+ | Gráficos estáticos |
| Seaborn | 0.14+ | Gráficos estadísticos |
| Plotly | 6.x | Gráficos interactivos |
| Streamlit | 1.40+ | Apps de datos |
| Dash | 2.18+ | Dashboards analíticos |

### DevOps y Herramientas
| Librería | Versión | Uso |
|----------|---------|-----|
| uv | 0.5+ | Gestor de paquetes (Rust) |
| Ruff | 0.8+ | Linter/formatter (Rust) |
| mypy | 1.13+ | Type checking |
| pytest | 8.x | Testing |
| Docker SDK | 7.x | Contenedores |
| MLflow | 2.x | ML lifecycle |
