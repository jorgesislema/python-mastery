# Visualización de Datos — Storytelling con Datos (Nivel PhD, Septiembre 2026)

## 1. Matplotlib 3.9+ — Control Total

```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
from matplotlib.patches import FancyBboxPatch
import numpy as np

# Estilo moderno
plt.style.use("seaborn-v0_8-whitegrid")
plt.rcParams.update({
    "figure.figsize": (12, 8),
    "font.size": 12,
    "axes.titlesize": 16,
    "axes.labelsize": 14,
    "figure.dpi": 150,
})

# Subplots con GridSpec (layout complejo)
fig = plt.figure(figsize=(16, 10))
gs = gridspec.GridSpec(2, 3, figure=fig, hspace=0.3, wspace=0.3)

# Panel principal (ocupa 2 columnas)
ax1 = fig.add_subplot(gs[0, :2])
ax1.plot(x, y, color="#2196F3", linewidth=2, label="Línea principal")
ax1.fill_between(x, y_low, y_high, alpha=0.2, color="#2196F3")
ax1.set_title("Tendencia con Intervalo de Confianza", fontweight="bold")
ax1.legend()

# Panel de distribución
ax2 = fig.add_subplot(gs[0, 2])
ax2.hist(data, bins=30, color="#4CAF50", alpha=0.7, edgecolor="white")
ax2.set_title("Distribución")

# Panel de barras
ax3 = fig.add_subplot(gs[1, 0])
ax3.barh(categories, values, color="#FF9800")
ax3.set_title("Comparativa por Categoría")

# Panel de dispersión
ax4 = fig.add_subplot(gs[1, 1])
scatter = ax4.scatter(x_scatter, y_scatter, c=colors, s=sizes,
                       cmap="viridis", alpha=0.6, edgecolors="white", linewidth=0.5)
plt.colorbar(scatter, ax=ax4, label="Valor")

# Panel de torta
ax5 = fig.add_subplot(gs[1, 2])
wedges, texts, autotexts = ax5.pie(
    proportions, labels=labels, autopct="%1.1f%%",
    colors=["#E91E63", "#2196F3", "#4CAF50", "#FF9800"],
    startangle=90, pctdistance=0.85
)

plt.suptitle("Dashboard de Análisis de Ventas — Q3 2026", fontsize=18, fontweight="bold")
plt.savefig("dashboard.png", dpi=150, bbox_inches="tight")
plt.show()
```

---

## 2. Seaborn 0.14+ — Visualización Estadística

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Paleta de colores moderna
sns.set_palette("husl")
sns.set_context("notebook", font_scale=1.2)

# Joint plot con regresión
g = sns.jointplot(
    data=df, x="ingresos", y="gasto",
    kind="reg", height=8,
    marginal_kws=dict(bins=30, fill=True),
    scatter_kws=dict(alpha=0.5, edgecolor="white", linewidth=0.5),
)
g.set_axis_labels("Inresos Anuales ($)", "Gasto Mensual ($)", fontsize=12)

# FacetGrid — múltiples paneles por categoría
g = sns.FacetGrid(df, col="region", row="año", height=3, aspect=1.2)
g.map_dataframe(sns.histplot, x="ventas", bins=20, color="#2196F3")
g.set_titles("{row_name} — {col_name}")

# Catplot — comparaciones categóricas
sns.catplot(
    data=df, x="categoria", y="ventas", hue="trimestre",
    kind="bar", height=6, aspect=1.5,
    estimator=np.median, errorbar="sd",
    palette="Set2", edgecolor="white", linewidth=1,
)

# Pair plot — correlaciones entre todas las variables
sns.pairplot(
    df[["var1", "var2", "var3", "var4", "categoria"]],
    hue="categoria", diag_kind="kde",
    plot_kws=dict(alpha=0.5, s=20),
    palette="husl"
)

# Heatmap con anotaciones
corr = df.select_dtypes(include=[np.number]).corr()
plt.figure(figsize=(12, 10))
mask = np.triu(np.ones_like(corr, dtype=bool))
sns.heatmap(
    corr, mask=mask, annot=True, fmt=".2f",
    cmap="RdBu_r", center=0,
    square=True, linewidths=0.5,
    cbar_kws={"shrink": 0.8}
)
plt.title("Matriz de Correlaciones", fontsize=16, pad=20)
```

---

## 3. Plotly 6.x — Visualización Interactiva

```python
import plotly.graph_objects as go
import plotly.express as px
from plotly.subplots import make_subplots

# Dashboard interactivo completo
fig = make_subplots(
    rows=2, cols=2,
    subplot_titles=("Ventas por Mes", "Top Productos", "Distribución", "Heatmap"),
    specs=[[{"type": "bar"}, {"type": "pie"}],
           [{"type": "histogram"}, {"type": "heatmap"}]],
)

# Barras con hover personalizado
fig.add_trace(
    go.Bar(
        x=meses, y=ventas,
        marker_color=px.colors.sequential.Viridis,
        hovertemplate="<b>%{x}</b><br>Ventas: $%{y:,.0f}<extra></extra>",
        name="Ventas",
    ),
    row=1, col=1
)

# Treemap jerárquico
fig_treemap = px.treemap(
    df, path=["region", "categoria", "producto"],
    values="ventas", color="margen",
    color_continuous_scale="RdYlGn",
    title="Ventas por Región → Categoría → Producto"
)

# Sunburst
fig_sunburst = px.sunburst(
    df, path=["region", "categoria", "producto"],
    values="ventas", color="margen"
)

# Animación temporal
fig_anim = px.scatter(
    df, x="ingresos", y="esperanza_vida",
    size="poblacion", color="continente",
    animation_frame="año", animation_group="pais",
    range_x=[0, 80000], range_y=[30, 90],
    title="Evolución: Ingresos vs Esperanza de Vida"
)

fig.update_layout(
    height=800,
    template="plotly_white",
    font=dict(family="Inter, sans-serif"),
)
fig.show()
```

---

## 4. Streamlit 1.40+ — Apps de Datos

```python
import streamlit as st
import pandas as pd
import plotly.express as px

# Configuración de página
st.set_page_config(
    page_title="Dashboard de Ventas",
    page_icon=":bar_chart:",
    layout="wide",
    initial_sidebar_state="expanded",
)

# Sidebar — filtros
st.sidebar.header("Filtros")
region = st.sidebar.selectbox("Región", ["Todas", "Norte", "Sur", "Este", "Oeste"])
fecha = st.sidebar.date_input("Fecha", value=pd.Timestamp.now())
rango_precio = st.sidebar.slider("Rango de Precio", 0, 1000, (100, 500))

# Layout con columnas
col1, col2, col3 = st.columns(3)

with col1:
    st.metric("Ventas Totales", "$1.2M", "+12.5%")

with col2:
    st.metric("Clientes Activos", "3,847", "+8.3%")

with col3:
    st.metric("Tasa de Conversión", "4.2%", "+0.8%")

# Tabs
tab1, tab2, tab3 = st.tabs(["Análisis", "Predicciones", "Datos"])

with tab1:
    st.subheader("Tendencia de Ventas")
    fig = px.line(df, x="fecha", y="ventas", color="producto")
    st.plotly_chart(fig, use_container_width=True)

with tab2:
    st.subheader("Modelo Predictivo")
    if st.button("Entrenar Modelo"):
        with st.spinner("Entrenando..."):
            modelo = entrenar_modelo(df)
        st.success("Modelo entrenado")
        st.plotly_chart(fig_prediccion, use_container_width=True)

# Cache de datos
@st.cache_data(ttl=3600)
def cargar_datos():
    return pd.read_csv("datos.csv")
```

---

## 5. Principios de Storytelling con Datos

### Jerarquía Visual
```
1. Datos (el protagonista)
2. Título descriptivo (concluyente, no descriptivo)
3. Anotaciones (contexto directo en el gráfico)
4. Ejes y etiquetas
5. Leyenda (si es necesaria)
6. Gridlines sutiles
```

### Errores Comunes
- **NO usar** colores arcoíris sin propósito
- **NO** gráficos 3D (distorsionan percepción)
- **NO** ejes que no empiezan en cero para barras
- **SÍ** usar color para enfatizar, no decorar
- **SÍ** títulos que cuenten una historia ("Las ventas crecieron 15% en Q3")
- **SÍ** direct labeling sobre leyendas

### Paletas de Color Recomendadas
```python
# Para datos secuenciales
secuencial = ["#eff3ff", "#bdd7e7", "#6baed6", "#3182bd", "#08519c"]

# Para datos divergentes
divergente = ["#d73027", "#fc8d59", "#fee090", "#e0f3f8", "#91bfdb", "#4575b4"]

# Para datos categóricos (max 8 categorías)
categorico = ["#4e79a7", "#f28e2b", "#e15759", "#76b7b2", "#59a14f", "#edc948"]
```
