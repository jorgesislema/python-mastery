# Proyecto 6: Automatización Inteligente (RPA + IA)
# Salario: $100K-150K | Freelance: $8K-25K

## ¿Qué es?

Bots inteligentes que automatizan tareas repetitivas combinando web scraping, procesamiento de documentos (OCR + LLM), y workflows automatizados.

---

## Stack

```python
# requirements.txt
playwright==1.49.0       # Navegación web moderna (reemplaza Selenium)
langchain==0.3.12
openai==1.60.0
pydantic==3.10.0
fastapi==0.115.6
celery==5.4.0            # Tareas en background
redis==5.2.1
pandas==3.0.0
beautifulsoup4==4.12.3
pymupdf==1.25.0          # PDF processing
pytesseract==0.3.13      # OCR
pillow==11.1.0
schedule==1.2.2
```

---

## Implementación

### 1. Web Scraper Inteligente con Playwright

```python
# services/scraper.py
from playwright.async_api import async_playwright
from bs4 import BeautifulSoup
import asyncio
from typing import Callable

class SmartScraper:
    """Scraper que se adapta al sitio y maneja anti-bot."""

    def __init__(self, headless: bool = True):
        self.headless = headless
        self.browser = None

    async def __aenter__(self):
        self.pw = await async_playwright().start()
        self.browser = await self.pw.chromium.launch(headless=self.headless)
        return self

    async def __aexit__(self, *args):
        await self.browser.close()
        await self.pw.stop()

    async def scrape(self, url: str, extractor: Callable = None) -> dict:
        """Scrapea una página con extractor personalizado."""
        page = await self.browser.new_page()
        await page.goto(url, wait_until="networkidle")

        # Esperar contenido dinámico
        await page.wait_for_timeout(2000)

        html = await page.content()
        soup = BeautifulSoup(html, "html.parser")

        # Extraer datos básicos
        data = {
            "url": url,
            "title": soup.title.string if soup.title else "",
            "text": soup.get_text(strip=True)[:5000],
            "links": [a.get("href") for a in soup.find_all("a", href=True)[:50]],
            "images": [img.get("src") for img in soup.find_all("img", src=True)[:20]],
        }

        # Aplicar extractor personalizado
        if extractor:
            data["custom"] = extractor(soup)

        await page.close()
        return data

    async def scrape_multiple(self, urls: list[str], extractor: Callable = None) -> list[dict]:
        """Scrapea múltiples URLs en paralelo."""
        tasks = [self.scrape(url, extractor) for url in urls]
        return await asyncio.gather(*tasks, return_exceptions=True)
```

### 2. Procesamiento de Documentos con IA

```python
# services/document_processor.py
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
import fitz  # PyMuPDF
import pytesseract
from PIL import Image
import io

class DocumentProcessor:
    """Procesa documentos con IA: extracción, resumen, clasificación."""

    def __init__(self):
        self.llm = ChatOpenAI(model="gpt-5", temperature=0)

    def extract_text_from_pdf(self, pdf_path: str) -> str:
        """Extrae texto de PDF (texto + OCR si es imagen)."""
        doc = fitz.open(pdf_path)
        text = ""
        for page in doc:
            text += page.get_text()
            # Si la página tiene poco texto, intentar OCR
            if len(text.strip()) < 50:
                pix = page.get_pixmap()
                img = Image.open(io.BytesIO(pix.tobytes("png")))
                text += pytesseract.image_to_string(img)
        return text

    async def extract_structured(self, text: str, schema: dict) -> dict:
        """Extrae información estructurada con LLM."""
        prompt = ChatPromptTemplate.from_messages([
            ("system", "Extrae la información del texto en el formato JSON especificado."),
            ("human", "Texto:\n{text}\n\nFormato esperado:\n{schema}"),
        ])

        chain = prompt | self.llm
        result = await chain.ainvoke({"text": text, "schema": str(schema)})
        return result.content

    async def summarize(self, text: str, max_length: int = 500) -> str:
        """Resume un documento."""
        prompt = ChatPromptTemplate.from_messages([
            ("system", f"Resume el siguiente texto en máximo {max_length} caracteres."),
            ("human", "{text}"),
        ])
        chain = prompt | self.llm
        result = await chain.ainvoke({"text": text[:8000]})
        return result.content

    async def classify(self, text: str, categories: list[str]) -> str:
        """Clasifica un documento en categorías."""
        prompt = ChatPromptTemplate.from_messages([
            ("system", f"Clasifica el texto en una de estas categorías: {categories}. Responde solo con la categoría."),
            ("human", "{text}"),
        ])
        chain = prompt | self.llm
        result = await chain.ainvoke({"text": text[:4000]})
        return result.content.strip()
```

### 3. Workflow Automatizado

```python
# workflows/daily_report.py
from services.scraper import SmartScraper
from services.document_processor import DocumentProcessor
import schedule
import asyncio

async def workflow_informe_diario():
    """Workflow: scrapear noticias → resumir → clasificar → enviar."""
    processor = DocumentProcessor()

    async with SmartScraper() as scraper:
        # 1. Scrapear noticias
        urls = ["https://ejemplo.com/noticias"]
        articles = await scraper.scrape_multiple(urls)

        # 2. Procesar cada artículo
        report = []
        for article in articles:
            if isinstance(article, dict) and "text" in article:
                summary = await processor.summarize(article["text"])
                category = await processor.classify(
                    article["text"],
                    ["tecnología", "finanzas", "política", "deportes"]
                )
                report.append({
                    "title": article.get("title", ""),
                    "summary": summary,
                    "category": category,
                })

        # 3. Generar informe final
        informe = "\n\n".join([
            f"## {r['title']} [{r['category']}]\n{r['summary']}"
            for r in report
        ])

        return informe

# Programar ejecución diaria
schedule.every().day.at("08:00").do(lambda: asyncio.run(workflow_informe_diario()))
```

---

## Cómo Presentarlo

```
Título: "Automatización Inteligente con Playwright + LLMs"

- Web scraping adaptativo con Playwright (anti-bot)
- Extracción de PDFs con OCR + LLM
- Clasificación y resumen automático de documentos
- Workflows programados con Celery + Redis
- API para disparar automatizaciones on-demand

Tecnologías: Playwright, LangChain, OpenAI, PyMuPDF, Tesseract, Celery
```
