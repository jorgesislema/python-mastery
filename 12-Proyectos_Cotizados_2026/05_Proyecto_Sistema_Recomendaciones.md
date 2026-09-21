# Proyecto 5: Sistema de Recomendaciones
# Salario: $115K-175K | Freelance: $12K-35K

## ¿Qué es?

Motor de recomendaciones que sugiere productos/contenido basándose en historial de usuario, similitud de contenido, y comportamiento en tiempo real.

---

## Stack

```python
# requirements.txt
lightfm==1.17            # Collaborative + Content-based hybrid
faiss-cpu==1.9.0         # Vector similarity (Meta)
redis==5.2.1             # Cache de recommendations
fastapi==0.115.6
pandas==3.0.0
numpy==2.2.0
scikit-learn==1.6.1
sentence-transformers==3.4.1  # Para content embeddings
```

---

## Implementación

### 1. Collaborative Filtering con LightFM

```python
# services/collaborative.py
from lightfm import LightFM
from lightfm.data import Dataset
import numpy as np
import pandas as pd

class CollaborativeRecommender:
    """Recomendación basada en interacciones usuario-item."""

    def __init__(self, no_components=30, learning_rate=0.05):
        self.model = LightFM(
            no_components=no_components,
            learning_rate=learning_rate,
            loss="warp",
            item_alpha=1e-6,
            user_alpha=1e-6,
        )
        self.dataset = Dataset()

    def fit(self, interactions_df: pd.DataFrame):
        """Entrena el modelo con interacciones."""
        self.dataset.fit(
            users=interactions_df["user_id"].unique(),
            items=interactions_df["item_id"].unique(),
        )

        interactions, weights = self.dataset.build_interactions(
            [(row["user_id"], row["item_id"], row["rating"])
             for _, row in interactions_df.iterrows()]
        )

        self.model.fit(interactions, epochs=30, num_threads=4)
        self.user_id_map, self.item_id_map, _, _ = self.dataset.mapping()

    def recommend(self, user_id: int, n: int = 10) -> list[dict]:
        """Recomienda items para un usuario."""
        n_items = len(self.item_id_map)
        scores = self.model.predict(
            self.user_id_map[user_id],
            np.arange(n_items)
        )
        top_items = np.argsort(-scores)[:n]

        # Mapear de vuelta a item_ids
        reverse_item_map = {v: k for k, v in self.item_id_map.items()}
        return [{"item_id": reverse_item_map[i], "score": float(scores[i])} for i in top_items]
```

### 2. Content-Based con FAISS

```python
# services/content_based.py
import faiss
import numpy as np
from sentence_transformers import SentenceTransformer

class ContentRecommender:
    """Recomendación basada en similitud de contenido."""

    def __init__(self):
        self.model = SentenceTransformer("all-MiniLM-L6-v2")
        self.index = None
        self.items = []

    def fit(self, items: list[dict]):
        """Construye el índice FAISS."""
        self.items = items
        texts = [f"{item['title']} {item['description']}" for item in items]
        embeddings = self.model.encode(texts, show_progress_bar=True)

        dimension = embeddings.shape[1]
        self.index = faiss.IndexFlatIP(dimension)  # Inner product
        faiss.normalize_L2(embeddings)
        self.index.add(embeddings.astype("float32"))

    def recommend(self, query: str, n: int = 10) -> list[dict]:
        """Recomienda items similares a un query."""
        query_embedding = self.model.encode([query])
        faiss.normalize_L2(query_embedding)

        scores, indices = self.index.search(query_embedding.astype("float32"), n)

        return [
            {"item": self.items[i], "similarity": float(s)}
            for i, s in zip(indices[0], scores[0])
        ]

    def recommend_from_item(self, item_id: int, n: int = 5) -> list[dict]:
        """Recomienda items similares a uno dado."""
        idx = next(i for i, item in enumerate(self.items) if item["id"] == item_id)
        embedding = self.index.reconstruct(idx).reshape(1, -1)
        scores, indices = self.index.search(embedding, n + 1)
        return [
            {"item": self.items[i], "similarity": float(s)}
            for i, s in zip(indices[0], scores[0]) if i != idx
        ][:n]
```

### 3. Hybrid Recommender

```python
# services/hybrid.py
import redis
import json

class HybridRecommender:
    """Combina collaborative + content-based con cache."""

    def __init__(self, collaborative, content_based, redis_url="redis://localhost"):
        self.collab = collaborative
        self.content = content_based
        self.cache = redis.from_url(redis_url)

    def recommend(self, user_id: int, context: dict = None, n: int = 10) -> list[dict]:
        """Recomendación híbrida con cache."""
        cache_key = f"rec:{user_id}:{n}"
        cached = self.cache.get(cache_key)
        if cached:
            return json.loads(cached)

        # Collaborative (60% weight)
        collab_recs = self.collab.recommend(user_id, n=n*2)

        # Content-based (40% weight)
        if context and "recent_items" in context:
            content_recs = []
            for item_id in context["recent_items"][-3:]:
                content_recs.extend(self.content.recommend_from_item(item_id, n=3))
        else:
            content_recs = []

        # Merge scores
        scores = {}
        for rec in collab_recs:
            scores[rec["item_id"]] = scores.get(rec["item_id"], 0) + rec["score"] * 0.6
        for rec in content_recs:
            item_id = rec["item"]["id"]
            scores[item_id] = scores.get(item_id, 0) + rec["similarity"] * 0.4

        # Top N
        sorted_items = sorted(scores.items(), key=lambda x: -x[1])[:n]
        results = [{"item_id": iid, "score": s} for iid, s in sorted_items]

        # Cache 1 hora
        self.cache.setex(cache_key, 3600, json.dumps(results))
        return results
```

---

## Cómo Presentarlo

```
Título: "Sistema de Recomendaciones Híbrido con Faiss"

- Collaborative filtering con LightFM (WARP loss)
- Content-based con sentence-transformers + FAISS
- Score híbrido: 60% collaborative + 40% content
- Cache con Redis (TTL 1h)
- <50ms latency por request
- A/B testing framework integrado

Tecnologías: LightFM, FAISS, Sentence-Transformers, Redis, FastAPI
```
