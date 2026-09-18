# Instalación y Configuración del Entorno (2026)

## Python 3.13+ (Recomendado: Python 3.15)

### Windows
```bash
# Descargar desde python.org (marcar "Add Python to PATH")
# O con winget:
winget install Python.Python.3.15

# Verificar:
python --version
python -m pip --version
```

### macOS
```bash
# Con Homebrew (recomendado):
brew install python@3.15

# O con pyenv (múltiples versiones):
pyenv install 3.15.0
pyenv global 3.15.0
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update && sudo apt install python3.15 python3.15-venv python3.15-dev
```

---

## Gestor de Paquetes: uv (Recomendado sobre pip)

**uv** es el gestor de paquetes del futuro: escrito en Rust, 10-100x más rápido que pip.

```bash
# Instalar uv:
pip install uv

# Crear proyecto:
uv init mi-proyecto
cd mi-proyecto

# Crear entorno virtual:
uv venv

# Instalar dependencias:
uv pip install numpy pandas scikit-learn

# Instalar desde requirements.txt:
uv pip install -r requirements.txt

# Sincronizar entorno exacto:
uv pip compile requirements.in -o requirements.txt
uv pip sync
```

---

## Entorno Virtual (Obligatorio)

```bash
# Opción 1: venv (nativo)
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS/Linux

# Opción 2: uv (recomendado)
uv venv
source .venv/bin/activate

# Verificar:
which python   # Debe apuntar al .venv
pip list
```

---

## Editor: VS Code (Recomendado)

### Extensiones Esenciales
| Extensión | Propósito |
|-----------|-----------|
| **Python** (ms-python) | IntelliSense, debugging |
| **Pylance** | Type checking, autocomplete |
| **Ruff** | Linting y formatting (Rust) |
| **Jupyter** | Notebooks en VS Code |
| **GitLens** | Historia de git inline |
| **Error Lens** | Errores inline en el editor |
| **Data Wrangler** | Exploración de DataFrames |
| **GitHub Copilot** | Asistente IA de código |

### Configuración `.vscode/settings.json`
```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/Scripts/python.exe",
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": "explicit",
      "source.organizeImports.ruff": "explicit"
    }
  },
  "python.analysis.typeCheckingMode": "strict",
  "python.analysis.autoImportCompletions": true
}
```

---

## Google Colab (Gratis, GPU/TPU)

```python
# En una celda de Colab:
!pip install -q numpy pandas scikit-learn polars

# Verificar GPU:
!nvidia-smi

# Conectar Drive:
from google.colab import drive
drive.mount('/content/drive')
```

---

## Estructura de un Proyecto Profesional

```
mi-proyecto/
├── .venv/                    # Entorno virtual
├── .vscode/
│   └── settings.json
├── src/
│   ├── __init__.py
│   ├── main.py
│   ├── models/
│   │   └── __init__.py
│   ├── services/
│   │   └── __init__.py
│   └── utils/
│       └── __init__.py
├── tests/
│   ├── __init__.py
│   └── test_main.py
├── notebooks/
│   └── exploracion.ipynb
├── data/
│   ├── raw/
│   └── processed/
├── pyproject.toml            # Config moderna (reemplaza setup.py)
├── uv.lock                   # Lockfile exacto
├── .gitignore
└── README.md
```

### `pyproject.toml` (Estándar 2026)
```toml
[project]
name = "mi-proyecto"
version = "0.1.0"
description = "Descripción del proyecto"
readme = "README.md"
requires-python = ">=3.13"
license = "MIT"

dependencies = [
    "numpy>=2.1",
    "pandas>=3.0",
    "polars>=2.0",
    "scikit-learn>=1.6",
    "fastapi>=0.115",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0",
    "ruff>=0.8",
    "mypy>=1.13",
    "ipykernel>=6.29",
]

[tool.ruff]
target-version = "py313"

[tool.ruff.lint]
select = ["E", "F", "W", "I", "N", "UP", "B", "A", "SIM"]

[tool.mypy]
python_version = "3.13"
strict = true

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-v --tb=short"
```

---

## Herramientas de Desarrollo Modernas

| Herramienta | Reemplaza | Ventaja |
|-------------|-----------|---------|
| **uv** | pip, poetry, pipenv | 10-100x más rápido |
| **ruff** | black, flake8, isort, pylint | Todo-en-uno, escrito en Rust |
| **mypy** | — | Type checking estático |
| **pytest** | unittest | Syntax más limpia, fixtures, plugins |
| **ipykernel** | ipython | Notebooks en VS Code |
| **pre-commit** | — | Hooks de calidad antes de commit |
| **pyright** | mypy (alternativa) | Type checker más rápido |

---

## Verificación Final

```bash
python --version          # Python 3.15.x
uv --version              # uv 0.5+
ruff version              # ruff 0.8+
mypy --version            # mypy 1.13+
pytest --version          # pytest 8.x
```
