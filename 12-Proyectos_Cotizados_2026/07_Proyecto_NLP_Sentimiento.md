# Proyecto 7: Plataforma NLP / Análisis de Sentimiento
# Salario: $110K-165K | Freelance: $10K-30K

## ¿Qué es?

Plataforma que analiza texto (reviews, redes sociales, tickets) para extraer sentimiento, temas, entidades, y tendencias en tiempo real.

---

## Stack

```python
# requirements.txt
transformers==4.48.0
torch==2.7.0
spacy==3.8.4
fastapi==0.115.6
polars==2.6.0
plotly==6.0.0
streamlit==1.41.0
textblob==0.18.0
wordcloud==1.9.4
```

---

## Implementación

### 1. Análisis de Sentimiento con Transformers

```python
# services/sentiment.py
from transformers import pipeline
import torch

class SentimentAnalyzer:
    """Análisis de sentimiento multilingüe con transformers."""

    def __init__(self, model: str = "nlptown/bert-base-multilingual-uncased-sentiment"):
        device = 0 if torch.cuda.is_available() else -1
        self.pipeline = pipeline(
            "sentiment-analysis",
            model=model,
            device=device,
            top_k=None,
        )

    def analyze(self, text: str) -> dict:
        """Analiza sentimiento de un texto."""
        results = self.pipeline(text[:512])[0]
        scores = {r["label"]: r["score"] for r in results}

        # Mapear estrellas a sentimiento
        star_map = {
            "1 star": "muy_negativo", "2 stars": "negativo",
            "3 stars": "neutro", "4 stars": "positivo",
            "5 stars": "muy_positivo"
        }
        best = max(results, key=lambda x: x["score"])

        return {
            "sentiment": star_map.get(best["label"], best["label"]),
            "confidence": best["score"],
            "scores": scores,
        }

    def analyze_batch(self, texts: list[str]) -> list[dict]:
        """Analiza múltiples textos."""
        results = self.pipeline(texts, batch_size=32, truncation=True)
        return [self.analyze(text) for text, results in zip(texts, results)]

    def get_sentiment_trend(self, texts_with_dates: list[dict]) -> list[dict]:
        """Calcula tendencia de sentimiento a lo largo del tiempo."""
        sentiments = []
        for item in texts_with_dates:
            result = self.analyze(item["text"])
            sentiments.append({
                "date": item["date"],
                "sentiment": result["sentiment"],
                "score": result["confidence"],
            })
        return sentiments
```

### 2. Named Entity Recognition

```python
# services/ner.py
import spacy

class NamedEntityRecognizer:
    """Extrae entidades nombradas del texto."""

    def __init__(self, model: str = "es_core_news_lg"):
        self.nlp = spacy.load(model)

    def extract(self, text: str) -> dict:
        """Extrae entidades del texto."""
        doc = self.nlp(text)
        entities = {}
        for ent in doc.ents:
            if ent.label_ not in entities:
                entities[ent.label_] = []
            entities[ent.label_].append({
                "text": ent.text,
                "start": ent.start_char,
                "end": ent.end_char,
            })
        return {
            "entities": entities,
            "summary": {
                label: len(ents) for label, ents in entities.items()
            },
        }
```

### 3. Topic Modeling

```python
# services/topics.py
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.decomposition import LatentDirichletAllocation
import polars as pl

class TopicModeler:
    """Descubre temas ocultos en colecciones de texto."""

    def __init__(self, n_topics: int = 5):
        self.n_topics = n_topics
        self.vectorizer = TfidfVectorizer(max_features=1000, stop_words="spanish")
        self.lda = LatentDirichletAllocation(
            n_components=n_topics, random_state=42
        )

    def fit(self, texts: list[str]):
        """Entrena el modelo de temas."""
        tfidf = self.vectorizer.fit_transform(texts)
        self.lda.fit(tfidf)
        self.feature_names = self.vectorizer.get_feature_names_out()

    def get_topics(self, n_words: int = 10) -> list[dict]:
        """Retorna los temas con sus palabras clave."""
        topics = []
        for idx, topic in enumerate(self.lda.components_):
            top_indices = topic.argsort()[-n_words:][::-1]
            top_words = [self.feature_names[i] for i in top_indices]
            topics.append({
                "topic_id": idx,
                "words": top_words,
                "weights": [float(topic[i]) for i in top_indices],
            })
        return topics

    def predict(self, text: str) -> dict:
        """Predice el tema de un texto."""
        tfidf = self.vectorizer.transform([text])
        topic_dist = self.lda.transform(tfidf)[0]
        dominant = topic_dist.argmax()
        return {
            "dominant_topic": int(dominant),
            "distribution": topic_dist.tolist(),
        }
```

---

## Cómo Presentarlo

```
Título: "Plataforma de NLP para Análisis de Sentimiento y Temas"

- Sentimiento multilingüe con BERT (1-5 estrellas)
- NER (Named Entity Recognition) con spaCy
- Topic modeling con LDA
- Dashboard interactivo con Streamlit
- API REST para integración
- Procesamiento batch de miles de documentos

Tecnologías: Transformers, spaCy, Scikit-learn, Streamlit, FastAPI
```
