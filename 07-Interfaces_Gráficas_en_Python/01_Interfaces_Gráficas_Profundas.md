# Interfaces Gráficas en Python (Nivel PhD, Septiembre 2026)

## 1. Tkinter — GUI Nativa

```python
import tkinter as tk
from tkinter import ttk, messagebox, filedialog
from tkinter.font import Font

class Aplicacion(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Gestor de Datos v2.0")
        self.geometry("900x600")
        self.configure(bg="#f0f0f0")

        # Estilo ttk
        style = ttk.Style()
        style.theme_use("clam")
        style.configure("Custom.TButton", font=("Segoe UI", 11), padding=10)

        self._crear_menu()
        self._crear_widgets()

    def _crear_menu(self):
        menubar = tk.Menu(self)
        archivo = tk.Menu(menubar, tearoff=0)
        archivo.add_command(label="Abrir", command=self.abrir_archivo)
        archivo.add_command(label="Guardar", command=self.guardar)
        archivo.add_separator()
        archivo.add_command(label="Salir", command=self.quit)
        menubar.add_cascade(label="Archivo", menu=archivo)

        ayuda = tk.Menu(menubar, tearoff=0)
        ayuda.add_command(label="Acerca de", command=lambda: messagebox.showinfo("Acerca", "v2.0"))
        menubar.add_cascade(label="Ayuda", menu=ayuda)
        self.config(menu=menubar)

    def _crear_widgets(self):
        # Frame principal
        main_frame = ttk.Frame(self, padding=20)
        main_frame.pack(fill=tk.BOTH, expand=True)

        # Treeview con datos
        columns = ("id", "nombre", "email", "rol")
        self.tree = ttk.Treeview(main_frame, columns=columns, show="headings", height=15)

        for col in columns:
            self.tree.heading(col, text=col.title())
            self.tree.column(col, width=200)

        scrollbar = ttk.Scrollbar(main_frame, orient=tk.VERTICAL, command=self.tree.yview)
        self.tree.configure(yscrollcommand=scrollbar.set)

        self.tree.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
        scrollbar.pack(side=tk.RIGHT, fill=tk.Y)

        # Botones
        btn_frame = ttk.Frame(main_frame)
        btn_frame.pack(pady=10)
        ttk.Button(btn_frame, text="Agregar", command=self.agregar, style="Custom.TButton").pack(side=tk.LEFT, padx=5)
        ttk.Button(btn_frame, text="Eliminar", command=self.eliminar, style="Custom.TButton").pack(side=tk.LEFT, padx=5)
        ttk.Button(btn_frame, text="Exportar CSV", command=self.exportar, style="Custom.TButton").pack(side=tk.LEFT, padx=5)

if __name__ == "__main__":
    app = Aplicacion()
    app.mainloop()
```

---

## 2. PyQt6 — GUI Profesional

```python
from PyQt6.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout,
    QHBoxLayout, QTableWidget, QTableWidgetItem, QPushButton,
    QLineEdit, QLabel, QComboBox, QProgressBar, QStatusBar
)
from PyQt6.QtCore import Qt, QThread, pyqtSignal
import sys

class WorkerThread(QThread):
    progreso = pyqtSignal(int)
    completado = pyqtSignal(list)

    def run(self):
        for i in range(101):
            self.progreso.emit(i)
            self.msleep(50)
        self.completado.emit([{"id": i, "nombre": f"Item {i}"} for i in range(100)])

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Analizador de Datos — PyQt6")
        self.setMinimumSize(1000, 600)

        # Widget central
        central = QWidget()
        self.setCentralWidget(central)
        layout = QVBoxLayout(central)

        # Barra de búsqueda
        search_layout = QHBoxLayout()
        self.search_input = QLineEdit()
        self.search_input.setPlaceholderText("Buscar...")
        self.search_input.textChanged.connect(self.filtrar)
        search_layout.addWidget(self.search_input)

        self filtro_combo = QComboBox()
        self.filtro_combo.addItems(["Todos", "Activos", "Inactivos"])
        search_layout.addWidget(self.filtro_combo)
        layout.addLayout(search_layout)

        # Tabla
        self.tabla = QTableWidget()
        self.tabla.setColumnCount(4)
        self.tabla.setHorizontalHeaderLabels(["ID", "Nombre", "Email", "Estado"])
        self.tabla.setAlternatingRowColors(True)
        layout.addWidget(self.tabla)

        # Barra de progreso
        self.progress = QProgressBar()
        layout.addWidget(self.progress)

        # Status bar
        self.statusBar().showMessage("Listo")

        self._iniciar_carga()

    def _iniciar_carga(self):
        self.worker = WorkerThread()
        self.worker.progreso.connect(self.progress.setValue)
        self.worker.completado.connect(self._cargar_datos)
        self.worker.start()

    def _cargar_datos(self, datos):
        self.tabla.setRowCount(len(datos))
        for i, d in enumerate(datos):
            self.tabla.setItem(i, 0, QTableWidgetItem(str(d["id"])))
            self.tabla.setItem(i, 1, QTableWidgetItem(d["nombre"]))
        self.statusBar().showMessage(f"Cargados {len(datos)} registros")

    def filtrar(self, texto):
        for i in range(self.tabla.rowCount()):
            visible = any(
                texto.lower() in (self.tabla.item(i, j).text().lower() if self.tabla.item(i, j) else "")
                for j in range(self.tabla.columnCount())
            )
            self.tabla.setRowHidden(i, not visible)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())
```

---

## 3. Streamlit — Dashboard Web

```python
import streamlit as st
import plotly.express as px
import pandas as pd

st.set_page_config(page_title="Analytics Dashboard", layout="wide")

# Sidebar
st.sidebar.title("Configuración")
uploaded = st.sidebar.file_uploader("Subir CSV", type=["csv"])

if uploaded:
    df = pd.read_csv(uploaded)
    st.sidebar.success(f"{len(df)} filas, {len(df.columns)} columnas")

    # KPIs
    c1, c2, c3, c4 = st.columns(4)
    c1.metric("Filas", f"{len(df):,}")
    c2.metric("Columnas", len(df.columns))
    c3.metric("Nulos", f"{df.isnull().sum().sum():,}")
    c4.metric("Memoria", f"{df.memory_usage(deep=True).sum() / 1024**2:.1f} MB")

    # Tabs
    tab1, tab2, tab3 = st.tabs(["Explorar", "Visualizar", "Exportar"])

    with tab1:
        st.dataframe(df.head(100), use_container_width=True)
        st.write(df.describe())

    with tab2:
        col_num = df.select_dtypes(include=["number"]).columns.tolist()
        if col_num:
            x_axis = st.selectbox("Eje X", col_num)
            y_axis = st.selectbox("Eje Y", col_num, index=min(1, len(col_num)-1))
            chart_type = st.selectbox("Tipo", ["scatter", "line", "bar", "histogram"])

            if chart_type == "scatter":
                fig = px.scatter(df, x=x_axis, y=y_axis)
            elif chart_type == "line":
                fig = px.line(df, x=x_axis, y=y_axis)
            elif chart_type == "bar":
                fig = px.bar(df, x=x_axis, y=y_axis)
            else:
                fig = px.histogram(df, x=x_axis)

            st.plotly_chart(fig, use_container_width=True)

    with tab3:
        csv = df.to_csv(index=False)
        st.download_button("Descargar CSV", csv, "datos_exportados.csv", "text/csv")
else:
    st.info("Sube un archivo CSV para comenzar")
```

---

## 4. Kivy — Apps Móviles

```python
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.textinput import TextInput

class TaskApp(App):
    def build(self):
        self.title = "Gestor de Tareas"
        layout = BoxLayout(orientation="vertical", padding=20, spacing=10)

        self.input = TextInput(hint_text="Nueva tarea...", size_hint_y=None, height=50)
        layout.add_widget(self.input)

        btn_add = Button(text="Agregar", size_hint_y=None, height=50)
        btn_add.bind(on_press=self.agregar_tarea)
        layout.add_widget(btn_add)

        self.lista = BoxLayout(orientation="vertical", spacing=5)
        layout.add_widget(self.lista)

        return layout

    def agregar_tarea(self, instance):
        if self.input.text:
            item = BoxLayout(size_hint_y=None, height=40)
            item.add_widget(Label(text=self.input.text))
            btn = Button(text="X", size_hint_x=0.2)
            item.add_widget(btn)
            self.lista.add_widget(item)
            self.input.text = ""

if __name__ == "__main__":
    TaskApp().run()
```

---

## 5. Dash — Dashboards Analíticos

```python
import dash
from dash import dcc, html, Input, Output, callback
import plotly.express as px

app = dash.Dash(__name__)

app.layout = html.Div([
    html.H1("Dashboard Analítico"),
    dcc.Dropdown(
        id="region",
        options=[{"label": r, "value": r} for r in df["region"].unique()],
        value="Norte"
    ),
    dcc.Graph(id="grafico"),
    dcc.Interval(id="timer", interval=5000, n_intervals=0),
])

@callback(Output("grafico", "figure"), Input("region", "value"))
def actualizar(region):
    filtrado = df[df["region"] == region]
    return px.bar(filtrado.groupby("producto")["ventas"].sum().reset_index(),
                  x="producto", y="ventas")
```
