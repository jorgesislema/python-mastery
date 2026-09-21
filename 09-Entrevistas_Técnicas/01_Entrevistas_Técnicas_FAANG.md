# Entrevistas Técnicas — Guía Completa FAANG (Nivel PhD, Septiembre 2026)

## 1. Big-O Notation y Complejidad

### Tabla de Complejidades
```
O(1)           Acceso por índice, push/pop stack, hash lookup
O(log n)       Binary search, BST balanced, heap push/pop
O(n)           Linear scan, copy array, BFS/DFS
O(n log n)     Merge sort, quicksort (avg), heap sort
O(n²)          Bubble sort, insertion sort, nested loops
O(n³)          Floyd-Warshall, matrix multiplication naive
O(2ⁿ)          Subsets, Fibonacci recursivo
O(n!)          Permutations, brute force TSP
```

### Regla de Oro
```
Si tienes:
- 1 loop simple → O(n)
- 2 loops anidados → O(n²)
- Loop que divide entre 2 → O(log n)
- Loop que recorre y divide → O(n log n)
- Recursión que llama 2 veces → O(2^n)
- Recursión que llama n veces → O(n!)
```

---

## 2. Estructuras de Datos — Cuándo Usar Cada Una

| Estructura | Lookup | Insert | Delete | Uso Ideal |
|-----------|--------|--------|--------|-----------|
| Array/List | O(n) | O(1) tail | O(n) | Acceso por índice, iteración |
| Linked List | O(n) | O(1) head | O(1) head | Inserción frecuente al inicio |
| Hash Map/Dict | O(1) | O(1) | O(1) | Lookup rápido, counting |
| Set | O(1) | O(1) | O(1) | Membership testing, dedup |
| Stack | O(n) | O(1) | O(1) | LIFO, undo, parentheses |
| Queue | O(n) | O(1) | O(1) | FIFO, BFS |
| Heap/Priority Queue | O(1) min | O(log n) | O(log n) | Top-K, median, scheduling |
| BST (balanced) | O(log n) | O(log n) | O(log n) | Ordered data, range queries |
| Trie | O(m) | O(m) | O(m) | Autocomplete, prefix search |
| Union-Find | O(α(n)) | O(α(n)) | O(α(n)) | Connected components |

---

## 3. Algoritmos Clásicos — Soluciones Completas

### Sliding Window
```python
# Problema: Subarray más largo con suma ≤ k
def max_subarray_len(nums, k):
    left = 0
    current_sum = 0
    max_len = 0
    for right in range(len(nums)):
        current_sum += nums[right]
        while current_sum > k:
            current_sum -= nums[left]
            left += 1
        max_len = max(max_len, right - left + 1)
    return max_len
```

### Two Pointers
```python
# Problema: Contar pares con suma = target en array ordenado
def count_pairs(nums, target):
    left, right = 0, len(nums) - 1
    count = 0
    while left < right:
        total = nums[left] + nums[right]
        if total == target:
            count += 1
            left += 1
            right -= 1
        elif total < target:
            left += 1
        else:
            right -= 1
    return count
```

### BFS (Breadth-First Search)
```python
# Problema: Distancia más corta en grid
from collections import deque

def shortest_path(grid, start, end):
    rows, cols = len(grid), len(grid[0])
    queue = deque([(start, 0)])
    visited = {start}
    while queue:
        (r, c), dist = queue.popleft()
        if (r, c) == end:
            return dist
        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            nr, nc = r+dr, c+dc
            if 0<=nr<rows and 0<=nc<cols and (nr,nc) not in visited:
                visited.add((nr,nc))
                queue.append(((nr,nc), dist+1))
    return -1
```

### DFS + Backtracking
```python
# Problema: Todos los subsets
def subsets(nums):
    result = []
    def backtrack(start, current):
        result.append(current[:])
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()
    backtrack(0, [])
    return result
```

### Dynamic Programming Patterns
```python
# 1. 0/1 Knapsack
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            dp[i][w] = dp[i-1][w]  # No tomar item i
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i][w], dp[i-1][w-weights[i-1]] + values[i-1])
    return dp[n][capacity]

# 2. Longest Common Subsequence
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n+1) for _ in range(m+1)]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[m][n]

# 3. Edit Distance
def edit_distance(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n+1) for _ in range(m+1)]
    for i in range(m+1): dp[i][0] = i
    for j in range(n+1): dp[0][j] = j
    for i in range(1, m+1):
        for j in range(1, n+1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[m][n]
```

---

## 4. Machine Learning — Preguntas de Entrevista

### P: ¿Qué es Bias-Variance Tradeoff?
```
Error Total = Bias² + Variance + Error Irreducible

Bias: Error por simplificar. Underfitting. Solución: modelo más complejo.
Variance: Error por memorizar. Overfitting. Solución: más datos, regularización.
Irreducible: Ruido inherente. No se puede eliminar.
```

### P: Explica Gradient Descent
```
1. Inicializar pesos aleatoriamente
2. Forward pass: calcular predicción
3. Calcular loss (qué tan mal)
4. Backward pass: calcular gradiente (∂loss/∂pesos)
5. Actualizar: peso = peso - lr × gradiente
6. Repetir hasta convergencia

Variantes:
- Batch: usa todos los datos (lento, estable)
- Stochastic: 1 dato (rápido, ruidoso)
- Mini-batch: 32-256 datos (compromiso)
```

### P: ¿Cuándo usar Random Forest vs XGBoost?
```
Random Forest:
- Paralelizable (árboles independientes)
- Menos overfitting
- Rápido de entrenar
- Baseline sólido

XGBoost:
- Mejor rendimiento general
- Secuencial (cada árbol corrige errores)
- Más hiperparámetros
- Maneja missing values nativamente

Regla práctica: Empieza con RF. Si necesitas más rendimiento → XGBoost.
```

### P: AUC-ROC vs F1-Score
```
AUC-ROC: Mide discriminación a TODOS los umbrales
- Clases balanceadas
- Cuando importa el ranking

F1: Media armónica de precision y recall
- Clases desbalanceadas
- Cuando FP y FN son igual de costosos

Precision: TP/(TP+FP) → Cuando FP es costoso (spam detection)
Recall: TP/(TP+FN) → Cuando FN es costoso (disease detection)
```

### P: Regularización — L1 vs L2
```
L1 (Lasso): λ × Σ|w|
- Tiende pesos a cero → Feature selection
- Usa cuando tienes muchas features irrelevantes

L2 (Ridge): λ × Σw²
- Tiende pesos a ser pequeños
- Usa cuando todas las features son relevantes

ElasticNet: Combina L1 + L2
- Balance entre ambos
- Usa cuando no estás seguro
```

### P: BatchNorm vs LayerNorm
```
BatchNorm: Normaliza por feature a lo largo del batch
- Depende del batch size
- Mejor para CNNs

LayerNorm: Normaliza por ejemplo a lo largo de features
- Independiente del batch size
- Mejor para Transformers
- Lo usan GPT, BERT, etc.
```

### P: ¿Por qué Transformers y no RNNs?
```
1. Paralelismo: Transformers procesan toda la secuencia a la vez
2. Memoria: Attention O(1) vs RNN O(n) para acceder a contexto lejano
3. Largo plazo: Sin vanishing gradient en secuencias largas
4. Escalabilidad: Mejor con GPUs/TPUs
```

### P: Attention Mechanism
```
Attention(Q, K, V) = softmax(QK^T / √d_k) · V

Q (Query): "¿Qué estoy buscando?"
K (Key): "¿Qué tengo para ofrecer?"
V (Value): "¿Qué información entrego?"

Multi-Head: Múltiples atenciones en paralelo, capturan diferentes relaciones.
```

---

## 5. Sistemas Distribuidos

### CAP Theorem
```
Consistency: Todos ven los mismos datos
Availability: Cada petición recibe respuesta
Partition Tolerance: Funciona con particiones de red

Solo 2 de 3:
- CP: MongoDB, HBase (consistencia sobre disponibilidad)
- AP: Cassandra, DynamoDB (disponibilidad sobre consistencia)
```

### Eventual vs Strong Consistency
```
Strong: Siempre retorna último valor. Más lento. Bancos.
Eventual: Eventualmente converge. Más rápido. Redes sociales.
```

### Consistencia de Lectura
```
Strong: Lee el último write garantizado
Monotonic: Nunca retrocede en tiempo
Eventual: Puede leer datos viejos temporalmente
```

---

## 6. System Design — Casos Prácticos

### Diseña un Chat como WhatsApp
```
1. WebSocket para mensajes en tiempo real
2. Redis para presence status
3. Message queue (Kafka) para persistencia
4. CDN para multimedia
5. End-to-end encryption
6. Group messages: fan-out on write
```

### Diseña un News Feed como Twitter
```
1. Fan-out on write: Pre-computar feeds al postear
2. Fan-out on read: Calcular al solicitar
3. Hybrid: Fan-out on write para usuarios populares
4. Redis sorted sets para almacenar feeds
5. Pagination con cursor (no offset)
```

### Diseña un Sistema de Recomendaciones
```
1. Candidate Generation: collaborative filtering + content-based
2. Feature Store: usuario, item, contexto, interacciones
3. Scoring Model: Gradient Boosting o Neural Network
4. Serving: Batch scoring + real-time con cache
5. A/B testing para métricas
```

---

## 7. Preguntas Comportamentales (STAR Method)

```
S - Situation: Contexto del problema
T - Task: Tu responsabilidad específica
A - Action: Qué hiciste (detallado)
R - Resultado: Métricas concretas

Ejemplo: "Cuéntame de un tiempo que mejoraste el rendimiento de un sistema"

S: "El API de recomendaciones tardaba 3 segundos en responder"
T: "Reducir la latencia a <500ms sin cambiar la infraestructura"
A: "Agregué Redis cache, optimicé queries SQL con indexes, implementé lazy loading"
R: "Reduje latencia de 3s a 180ms (94% mejor), throughput de 100 a 500 req/s"
```
