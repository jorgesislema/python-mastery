# Instalación y Configuración de Python

## 📌 Instalación de Python

### 🔹 Windows
1. Descarga la última versión de Python desde el sitio oficial: [python.org](https://www.python.org/downloads/).
2. Ejecuta el instalador y **activa la casilla "Add Python to PATH"**.
3. Verifica la instalación abriendo una terminal (CMD o PowerShell) y escribiendo:
   ```sh
   python --version
   ```

### 🔹 macOS
1. Abre la terminal y usa Homebrew (si no lo tienes, instálalo desde [brew.sh](https://brew.sh/)):
   ```sh
   brew install python
   ```
2. Verifica la instalación:
   ```sh
   python3 --version
   ```

### 🔹 Linux (Debian/Ubuntu)
1. Abre una terminal y ejecuta:
   ```sh
   sudo apt update && sudo apt install python3 python3-pip
   ```
2. Verifica la instalación:
   ```sh
   python3 --version
   ```

---

## 📌 Manejando Múltiples Versiones de Python
Si tienes varias versiones de Python instaladas, puedes elegir cuál usar:

### 🔹 Windows
- Usa `py` para seleccionar una versión específica:
  ```sh
  py -3.8
  py -3.10
  ```

### 🔹 macOS y Linux
- Usa `update-alternatives` (solo en Linux):
  ```sh
  sudo update-alternatives --config python3
  ```
- Alternativamente, usa `alias` para seleccionar una versión predeterminada:
  ```sh
  alias python=python3.9
  ```

---

## 📌 Entornos Virtuales en Python
Los entornos virtuales permiten trabajar con distintas dependencias sin afectar el sistema global.

### 🔹 Crear un Entorno Virtual
```sh
python -m venv my_env
```

### 🔹 Activar el Entorno Virtual
- **Windows:**
  ```sh
  my_env\Scripts\activate
  ```
- **macOS/Linux:**
  ```sh
  source my_env/bin/activate
  ```

### 🔹 Desactivar el Entorno Virtual
```sh
deactivate
```

---

## 📌 Uso en VS Code
1. Instala la extensión **Python** en VS Code.
2. Configura el intérprete correcto desde la barra inferior o con `Ctrl + Shift + P` → "Python: Select Interpreter".
3. Activa el entorno virtual en la terminal integrada:
   ```sh
   source my_env/bin/activate  # macOS/Linux
   my_env\Scripts\activate  # Windows
   ```

---

## 📌 Uso en Google Colab
Google Colab usa una versión predeterminada de Python pero puedes instalar paquetes y comprobar la versión con:
```sh
!python --version
!pip install nombre_paquete
```
Si necesitas otra versión de Python en Colab, usa:
```sh
!update-alternatives --config python3
```



