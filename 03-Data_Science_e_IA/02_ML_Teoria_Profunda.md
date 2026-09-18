# Machine Learning — Teoría Profunda desde Cero (Nivel PhD)

## 1. ¿Qué es el Aprendizaje Automático?

**Definición formal:** Machine Learning es el estudio de algoritmos que mejoran su rendimiento en una tarea T, medida por una métrica M, con experiencia E (Mitchell, 1997).

**Ejemplo:** Un email spam classifier.
- **T:** Clasificar emails como spam/no-spam
- **M:** Accuracy (porcentaje de emails clasificados correctamente)
- **E:** Emails etiquetados por humanos (datos de entrenamiento)

### Los 3 Paradigmas

```
SUPERVISADO:         Aprendizaje con etiquetas
  - Clasificación:   Email → spam/no-spam
  - Regresión:       Casa → precio

NO SUPERVISADO:      Sin etiquetas, descubre estructura
  - Clustering:      Clientes → segmentos
  - Reducción dim:   100 features → 2D para visualizar

REFUERZO:            Aprende por recompensa/penalización
  - Juegos:          AlphaGo jugando Go
  - Robótica:        Robot aprendiendo a caminar
```

---

## 2. Bias-Variance Tradeoff (El Corazón del ML)

### El problema fundamental
El error total de un modelo se descompone en:

```
Error Total = Bias² + Variance + Error Irreducible

Bias²:      Error por simplificar demasiado el modelo
            → Underfitting: el modelo no captura el patrón real
            
Variance:   Error por ser demasiado sensible a los datos de entrenamiento
            → Overfitting: el modelo memoriza ruido
            
Error Irr.: Ruido inherente en los datos (no se puede eliminar)
```

### Visualización intuición
```
                    Bias Alto
                    Variance Baja
    [XXXXX]         → Modelo simple, predice mal
                    → Ejemplo: regresión lineal para datos cuadráticos

                    Bias Bajo
                    Variance Alta
    [X X X X X]     → Modelo complejo, sobreajusta
                    → Ejemplo: árbol de decisión sin podar

                    Bias Bajo
                    Variance Baja
    [XXXXXXXXX]     → Modelo ideal (raro de encontrar)
                    → Ensemble methods se acercan
```

### Cómo diagnosticar
```python
from sklearn.model_selection import learning_curve
import numpy as np

# Curva de aprendizaje
train_sizes, train_scores, val_scores = learning_curve(
    modelo, X, y, cv=5,
    train_sizes=np.linspace(0.1, 1.0, 10),
    scoring="neg_mean_squared_error"
)

# Interpretación:
# Si train_score ALTO y val_score BAJO → Overfitting (alta variance)
# Si train_score BAJO y val_score BAJO → Underfitting (alto bias)
# Si ambos son buenos y convergen → Buen modelo
```

### Cómo combatir cada uno

| Problema | Soluciones |
|----------|-----------|
| **Alto Bias** (Underfitting) | Modelo más complejo, más features, reducir regularización, entrenar más tiempo |
| **Alta Variance** (Overfitting) | Más datos, regularización, dropout, early stopping, ensemble, reducir complejidad |
| **Ambos** | Más datos, ensemble methods, cross-validation |

---

## 3. Funciones de Loss (Pérdida)

La función de loss mide qué tan mal predice el modelo. El objetivo es MINIMIZAR la loss.

### Para Regresión

```python
# MSE (Mean Squared Error) — la más común
# Penaliza errores grandes exponencialmente
L_MSE = (1/n) * Σ(y_true - y_pred)²

# MAE (Mean Absolute Error) — más robusta a outliers
L_MAE = (1/n) * Σ|y_true - y_pred|

# Huber Loss — combina MSE y MAE
# Se comporta como MSE para errores pequeños, MAE para grandes
L_Huber = { 0.5*(y-y_pred)²           si |y-y_pred| ≤ δ
          { δ*|y-y_pred| - 0.5*δ²     si |y-y_pred| > δ

# Log-Cosh — suave approximation de MAE
L = log(cosh(y_pred - y_true))
```

**¿Cuándo usar cada una?**
- **MSE:** Cuando los errores grandes son muy costosos (ej: predicción de precios)
- **MAE:** Cuando hay outliers (ej: datos de sensores defectuosos)
- **Huber:** Cuando quieres robustez pero también penalizar errores grandes

### Para Clasificación

```python
# Binary Cross-Entropy (Log Loss) — para 2 clases
L = -[y*log(p) + (1-y)*log(1-p)]

# Categorical Cross-Entropy — para K clases
L = -Σ y_k * log(p_k)

# Focal Loss — para datasets desbalanceados (reduce peso de ejemplos fáciles)
FL = -α * (1-p)^γ * log(p)
# γ=0 → Cross-Entropy normal
# γ=2 → Focal Loss (reduce peso de ejemplos bien clasificados)
```

**¿Por qué no usamos accuracy como loss?**
- Accuracy no es diferenciable (no se puede hacer gradient descent)
- Cross-Entropy sí es diferenciable y penaliza más las predicciones confiadas pero incorrectas

```python
import torch
import torch.nn.functional as F

# En PyTorch:
criterion = F.cross_entropy(logits, labels)  # Internamente hace softmax + cross-entropy
```

---

## 4. Gradiente Descendente y Optimización

### El algoritmo fundamental
```
1. Inicializar pesos aleatoriamente
2. Calcular loss (predicción vs realidad)
3. Calcular gradiente (∂loss/∂pesos) ← Backpropagation
4. Actualizar pesos: peso = peso - lr * gradiente
5. Repetir hasta convergencia
```

### Backpropagation desde cero
```python
import numpy as np

# Red simple: input → hidden → output
def forward(x, W1, b1, W2, b2):
    z1 = x @ W1 + b1           # Linear
    a1 = np.maximum(0, z1)     # ReLU
    z2 = a1 @ W2 + b2          # Linear
    a2 = 1 / (1 + np.exp(-z2)) # Sigmoid
    return z1, a1, z2, a2

def backward(x, y, z1, a1, z2, a2, W2):
    m = x.shape[0]

    # Gradientes de la capa de salida
    dz2 = a2 - y                          # (a2 - y) por cross-entropy + sigmoid
    dW2 = (a1.T @ dz2) / m
    db2 = np.sum(dz2, axis=0, keepdims=True) / m

    # Gradientes de la capa oculta
    da1 = dz2 @ W2.T
    dz1 = da1 * (z1 > 0).astype(float)    # Derivada de ReLU
    dW1 = (x.T @ dz1) / m
    db1 = np.sum(dz1, axis=0, keepdims=True) / m

    return dW1, db1, dW2, db2

# Training loop
lr = 0.01
for epoch in range(1000):
    z1, a1, z2, a2 = forward(X, W1, b1, W2, b2)
    loss = -np.mean(y * np.log(a2 + 1e-8) + (1-y) * np.log(1-a2 + 1e-8))
    dW1, db1, dW2, db2 = backward(X, y, z1, a1, z2, a2, W2)

    W1 -= lr * dW1
    b1 -= lr * db1
    W2 -= lr * dW2
    b2 -= lr * db2
```

### Optimizadores

```
SGD (Stochastic Gradient Descent):
  peso = peso - lr * gradiente
  → Simple pero oscila mucho

Momentum:
  v = β*v + gradiente
  peso = peso - lr * v
  → Acumula velocidad, converge más rápido

RMSprop:
  s = β*s + (1-β)*gradiente²
  peso = peso - lr * gradiente / (√s + ε)
  → Adapta learning rate por parámetro

Adam (Adaptive Moment Estimation):
  m = β1*m + (1-β1)*gradiente           # Momento 1 (media)
  v = β2*v + (1-β2)*gradiente²          # Momento 2 (varianza)
  m̂ = m / (1-β1^t)                      # Bias correction
  v̂ = v / (1-β2^t)                      # Bias correction
  peso = peso - lr * m̂ / (√v̂ + ε)
  → El más usado, combina Momentum + RMSprop
```

```python
# En PyTorch:
optimizer = torch.optim.Adam(model.parameters(), lr=0.001, weight_decay=1e-4)
# weight_decay es L2 regularization

# Learning rate scheduling
scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=100)
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, patience=10)
```

---

## 5. Regularización (Combatir Overfitting)

### L1 Regularization (Lasso)
```
Loss_total = Loss_original + λ * Σ|pesos|
```
- Tiende a hacer pesos exactamente cero → **feature selection**
- Útil cuando tienes muchas features y sospechas que pocas son relevantes

### L2 Regularization (Ridge)
```
Loss_total = Loss_original + λ * Σ pesos²
```
- Tiende a hacer pesos pequeños pero no cero
- Útil cuando todas las features son potencialmente relevantes

### ElasticNet (combina L1 + L2)
```
Loss_total = Loss_original + λ1 * Σ|pesos| + λ2 * Σ pesos²
```

### Dropout (Redes Neuronales)
```python
# Durante entrenamiento: apaga neuronas aleatoriamente
class MiRed(nn.Module):
    def __init__(self):
        self.fc1 = nn.Linear(784, 256)
        self.dropout = nn.Dropout(0.5)  # Apaga 50% de las neuronas
        self.fc2 = nn.Linear(256, 10)

    def forward(self, x):
        x = F.relu(self.fc1(x))
        x = self.dropout(x)  # Solo activo durante entrenamiento
        return self.fc2(x)

# ¿Por qué funciona?
# - Evita que las neuronas se "co-adapten" demasiado
# - Fuerza a la red a aprender features redundantes
# - Equivalente a un ensemble de 2^n sub-redes
```

### Early Stopping
```python
best_val_loss = float("inf")
patience = 10
counter = 0

for epoch in range(1000):
    train_loss = train(model)
    val_loss = validate(model)

    if val_loss < best_val_loss:
        best_val_loss = val_loss
        counter = 0
        save_model(model)  # Guardar mejor modelo
    else:
        counter += 1
        if counter >= patience:
            break  # Parar entrenamiento
```

---

## 6. Métricas de Evaluación

### Para Clasificación

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    roc_auc_score, confusion_matrix, classification_report,
    precision_recall_curve, roc_curve
)

# Matriz de Confusión
#                    Predicho
#                  Pos    Neg
# Real  Pos  |    TP  |  FN  |
#       Neg  |    FP  |  TN  |

# Accuracy = (TP + TN) / (TP + TN + FP + FN)
# → Mala métrica para datasets desbalanceados

# Precision = TP / (TP + FP)
# → "De los que predije como positivos, ¿cuántos lo son?"
# → Importante cuando FP es costoso (ej: diagnosticar enfermedad)

# Recall = TP / (TP + FN)
# → "De los que son positivos, ¿cuántos detecté?"
# → Importante cuando FN es costoso (ej: detectar fraude)

# F1 = 2 * (Precision * Recall) / (Precision + Recall)
# → Balance entre precision y recall

# AUC-ROC
# → Mide la capacidad de discriminación a TODOS los umbrales
# → 0.5 = random, 1.0 = perfecto
```

### Para Regresión

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
import numpy as np

# R² (Coeficiente de Determinación)
# R² = 1 - (SS_res / SS_tot)
# SS_res = Σ(y_true - y_pred)²
# SS_tot = Σ(y_true - y_mean)²
# → 1.0 = predicción perfecta, 0.0 = predecir la media

# RMSE
# RMSE = √(Σ(y_true - y_pred)² / n)
# → En las mismas unidades que y

# MAE
# MAE = Σ|y_true - y_pred| / n
# → Más robusto a outliers que RMSE

# MAPE
# MAPE = (100/n) * Σ|y_true - y_pred| / |y_true|
# → Porcentual, más interpretable
```

### Para Clustering

```python
from sklearn.metrics import silhouette_score, calinski_harabasz_score

# Silhouette Score: [-1, 1]
# → Cuánto se parece un punto a su propio cluster vs otros clusters
# → > 0.5 = bueno, > 0.7 = excelente

# Calinski-Harabasz (Variance Ratio)
# → Ratio entre varianza intra-cluster y inter-cluster
# → Mayor = mejor

# Davies-Bouldin
# → Ratio de dispersión entre clusters
# → Menor = mejor
```

---

## 7. Cross-Validation (Validación Cruzada)

```python
from sklearn.model_selection import (
    KFold, StratifiedKFold, TimeSeriesSplit,
    cross_val_score, cross_validate
)

# K-Fold básico
kf = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=kf, scoring="accuracy")

# Stratified K-Fold (preserva proporción de clases)
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Time Series Split (no mezcla temporal)
tscv = TimeSeriesSplit(n_splits=5)

# Cross-validate con múltiples métricas
resultados = cross_validate(
    model, X, y, cv=5,
    scoring=["accuracy", "f1_weighted", "roc_auc"],
    return_train_score=True
)
# train_score vs val_score → diagnóstico de overfitting
```

---

## 8. Algoritmos Clásicos Explicados

### Regresión Lineal
```
Modelo: y = X*w + b
Objetivo: Minimizar Σ(y - X*w)²

Solución analítica: w = (X^T * X)^(-1) * X^T * y
Gradiente: ∂L/∂w = (2/n) * X^T * (X*w - y)

Pros: Rápido, interpretable, base para otros modelos
Contras: Asume relación lineal, sensible a outliers
```

### Regresión Logística
```
Modelo: p(y=1|x) = sigmoid(X*w + b)
sigmoid(z) = 1 / (1 + e^(-z))

Loss: Binary Cross-Entropy
      L = -[y*log(p) + (1-y)*log(1-p)]

Pros: Salida probabilística, interpretable, rápido
Contras: Solo problemas lineales en el espacio de features
```

### Árboles de Decisión
```
Algoritmo:
1. Para cada feature y umbral:
   - Calcular ganancia de información (Gini o Entropía)
2. Elegir el split con mayor ganancia
3. Repetir recursivamente
4. Parar cuando: max_depth, min_samples, o pureza perfecta

Gini = 1 - Σ(p_i²)
Entropía = -Σ(p_i * log(p_i))

Pros: Interpretable, no necesita scaling, maneja no-linealidad
Contras: Overfitting fácil, inestable (pequeño cambio en datos cambia el árbol)
```

### Random Forest
```
Idea: Ensemble de árboles con:
1. Bagging: cada árbol entrena con subset aleatorio de datos (con reemplazo)
2. Feature random: cada split usa solo un subset de features

Predicción: Voto mayoritario (clasificación) o promedio (regresión)

Pros: Resistente a overfitting, paralelizable, feature importance
Contras: Menos interpretable, más lento que un árbol, requiere más memoria
```

### Gradient Boosting (XGBoost, LightGBM)
```
Idea: Ensemble secuencial donde cada árbol corrige los errores del anterior

1. Empezar con predicción base (media)
2. Calcular residuos (errores)
3. Entrenar árbol para predecir residuos
4. Actualizar predicción: y_pred = y_pred + lr * nuevo_árbol
5. Repetir

XGBoost: Regularización + parallel tree building
LightGBM: Histogram-based splitting + leaf-wise growth (más rápido)
CatBoost: Categorical features nativas + ordered boosting

Pros: Estado del arte en tabular data, maneja missing values
Contras: Más lento de entrenar que RF, más hiperparámetros
```

### K-Means
```
Algoritmo:
1. Elegir K centroides aleatoriamente
2. Asignar cada punto al centroide más cercano
3. Recalcular centroides como la media de sus puntos
4. Repetir 2-3 hasta convergencia

K-Means++: Mejor inicialización
1. Elegir primer centroide aleatorio
2. Para cada punto, calcular distancia al centroide más cercano
3. Elegir siguiente centroide con probabilidad proporcional a distancia²
4. Repetir hasta tener K centroides

Pros: Simple, escalable, O(n*K*d*iteraciones)
Contras: Asume clusters esféricos, necesita K, sensible a inicialización
```

---

## 9. Redes Neuronales desde Cero

### Perceptrón
```
Salida = activate(Σ(w_i * x_i) + b)

Funciones de activación:
- Step: f(x) = 1 si x > 0, 0 si no (perceptrón original)
- Sigmoid: f(x) = 1/(1+e^(-x)) → (0, 1) — para probabilidades
- Tanh: f(x) = (e^x - e^(-x))/(e^x + e^(-x)) → (-1, 1)
- ReLU: f(x) = max(0, x) → [0, ∞) — el más usado
- LeakyReLU: f(x) = max(0.01*x, x) — evita "dead neurons"
- GELU: f(x) = x * Φ(x) — usado en Transformers
- SiLU/Swish: f(x) = x * sigmoid(x) — usado en EfficientNet
```

### Backpropagation paso a paso
```
Forward pass:
  z1 = W1*x + b1
  a1 = relu(z1)
  z2 = W2*a1 + b2
  a2 = softmax(z2)        # Para clasificación multiclase

Loss = cross_entropy(a2, y)

Backward pass (chain rule):
  dz2 = a2 - y                              # Gradiente output
  dW2 = a1.T @ dz2                           # Gradiente W2
  da1 = W2.T @ dz2                           # Gradiente hacia atrás
  dz1 = da1 * (z1 > 0)                       # Gradiente con ReLU
  dW1 = x.T @ dz1                            # Gradiente W1

Actualización:
  W1 -= lr * dW1
  W2 -= lr * dW2
```

### Arquitecturas Fundamentales

```
FCN (Fully Connected Network):
  x → [Dense → ReLU] × N → Dense → output
  Para: datos tabulares, clasificación simple

CNN (Convolutional Neural Network):
  x → [Conv2d → ReLU → Pooling] × N → Flatten → Dense → output
  Para: imágenes, series temporales con patrones locales
  - Kernel: filtro que detecta patrones (bordes, texturas)
  - Pooling: reduce dimensionalidad (max pooling, average pooling)
  - Parámetros compartidos: el mismo kernel se aplica a toda la imagen

RNN/LSTM/GRU:
  x₁ → x₂ → x₃ → ... → xₙ → output
  Para: secuencias, NLP, series temporales
  - Memoria: información fluye entre pasos temporales
  - LSTM: gates que controlan qué recordar/olvidar
  - GRU: versión simplificada de LSTM

Transformer:
  x → [Multi-Head Attention → Feed Forward] × N → output
  Para: NLP, vision, todo
  - Self-attention: cada token "mira" a todos los demás
  - Paralelizable: procesa toda la secuencia a la vez
  - Pos encoding: inyecta información de posición
```

---

## 10. Bias Ético y Fairness en ML

```python
# Tipos de sesgo:
# 1. Selection bias: datos de entrenamiento no representativos
# 2. Confirmation bias: buscar solo datos que confirmen hipótesis
# 3. Historical bias: datos reflejan desigualdades históricas
# 4. Measurement bias: features mal medidas

# Métricas de fairness
from fairlearn.metrics import (
    MetricFrame,
    demographic_parity_difference,
    equalized_odds_difference,
)

# demographic_parity_difference: 
#   P(ŷ=1 | grupo=A) - P(ŷ=1 | grupo=B)
#   Ideal: 0

# equalized_odds_difference:
#   max(
#     |TPR_A - TPR_B|,  # True positive rate
#     |FPR_A - FPR_B|   # False positive rate
#   )
#   Ideal: 0

# Mitigación:
# 1. Pre-processing: re-muestrear para balancear
# 2. In-processing: constrained optimization (fairlearn)
# 3. Post-processing: ajustar umbrales por grupo
```
