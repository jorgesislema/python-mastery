# Proyecto 4: Plataforma de Computer Vision
# Salario: $120K-190K | Freelance: $15K-50K

## ¿Qué es?

Sistema de visión por computadora que detecta, clasifica y segmenta objetos en imágenes/videos. Incluye entrenamiento custom, API de inference, y dashboard de métricas.

**¿Por qué es cotizado?**
- Autopilot, drones, manufacturing QA, retail analytics
- Mercado de CV: $21B en 2026
- Requiere ML + optimización + deployment edge

---

## Stack

```python
# requirements.txt
torch==2.7.0
torchvision==0.22.0
ultralytics==8.3.50      # YOLOv10
opencv-python==4.11.0
fastapi==0.115.6
pillow==11.1.0
numpy==2.2.0
onnxruntime==1.21.0      # Optimización para inference
```

---

## Implementación

### 1. Entrenamiento con YOLO

```python
# training/train_yolo.py
from ultralytics import YOLO

def train_model(dataset_yaml: str, epochs: int = 100):
    """Entrena YOLOv10 en dataset custom."""
    model = YOLO("yolov10n.yaml")  # nano — más rápido

    results = model.train(
        data=dataset_yaml,
        epochs=epochs,
        imgsz=640,
        batch=16,
        name="custom_model",
        patience=20,
        save=True,
        plots=True,
    )

    # Exportar a ONNX para inference rápida
    model.export(format="onnx", imgsz=640, half=True)

    return results
```

### 2. Inference API

```python
# services/detector.py
import cv2
import numpy as np
from ultralytics import YOLO
from pathlib import Path
import time

class ObjectDetector:
    """Detector de objetos con YOLO optimizado."""

    def __init__(self, model_path: str = "best.onnx"):
        self.model = YOLO(model_path)

    def detect(
        self, image: np.ndarray,
        conf_threshold: float = 0.5,
        iou_threshold: float = 0.45,
    ) -> dict:
        """Detecta objetos en una imagen."""
        start = time.perf_counter()

        results = self.model.predict(
            image,
            conf=conf_threshold,
            iou=iou_threshold,
            verbose=False,
        )

        detections = []
        for r in results:
            for box in r.boxes:
                detections.append({
                    "class": r.names[int(box.cls)],
                    "confidence": float(box.conf),
                    "bbox": box.xyxy[0].tolist(),
                })

        latency = (time.perf_counter() - start) * 1000

        return {
            "detections": detections,
            "count": len(detections),
            "latency_ms": round(latency, 2),
        }

    def detect_video(self, video_path: str, output_path: str = "output.mp4"):
        """Procesa un video completo."""
        cap = cv2.VideoCapture(video_path)
        fps = int(cap.get(cv2.CAP_PROP_FPS))
        w = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
        h = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

        writer = cv2.VideoWriter(output_path, cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

        while cap.isOpened():
            ret, frame = cap.read()
            if not ret:
                break
            result = self.detect(frame)
            # Dibujar bounding boxes
            for det in result["detections"]:
                x1, y1, x2, y2 = map(int, det["bbox"])
                cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
                cv2.putText(frame, f'{det["class"]} {det["confidence"]:.2f}',
                           (x1, y1-10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0,255,0), 2)
            writer.write(frame)

        cap.release()
        writer.release()
        return output_path
```

### 3. API FastAPI

```python
# main.py
from fastapi import FastAPI, UploadFile, File
from fastapi.responses import StreamingResponse
import cv2
import numpy as np
from services.detector import ObjectDetector
import io

app = FastAPI(title="Computer Vision API")
detector = ObjectDetector()

@app.post("/detect")
async def detect(file: UploadFile = File(...)):
    contents = await file.read()
    nparr = np.frombuffer(contents, np.uint8)
    image = cv2.imdecode(nparr, cv2.IMREAD_COLOR)
    result = detector.detect(image)
    return result

@app.post("/detect/video")
async def detect_video(file: UploadFile = File(...)):
    contents = await file.read()
    # Guardar temporalmente y procesar
    with open("temp_video.mp4", "wb") as f:
        f.write(contents)
    output = detector.detect_video("temp_video.mp4")
    return {"output_path": output}
```

---

## Cómo Presentarlo

```
Título: "Sistema de Computer Vision para Detección de Objetos"

- Entrenamiento custom con YOLOv10 en dataset propio
- API REST para inference (<100ms por imagen)
- Optimización ONNX (2x más rápido que PyTorch)
- Procesamiento de video completo con bounding boxes
- Soporte para edge deployment (TensorRT, OpenVINO)

Tecnologías: PyTorch, YOLOv10, OpenCV, FastAPI, ONNX Runtime
```
