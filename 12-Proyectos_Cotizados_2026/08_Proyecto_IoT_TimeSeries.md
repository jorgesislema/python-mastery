# Proyecto 8: IoT + Edge AI + Time Series Forecasting
# Salario: $115K-175K | Freelance: $12K-35K

## ¿Qué es?

Sistema que recibe datos de sensores IoT en tiempo real, predice fallas con ML, y ejecuta acciones automatizadas. Combina edge computing con cloud analytics.

---

## Stack

```python
# requirements.txt
paho-mqtt==2.1.0         # IoT messaging
influxdb-client==1.46.0  # Time series database
prophet==1.1.7           # Forecasting
sktime==0.35.0           # Time series ML
torch==2.7.0             # Deep learning
fastapi==0.115.6
polars==2.6.0
plotly==6.0.0
apscheduler==3.11.0
```

---

## Implementación

### 1. MQTT IoT Data Collector

```python
# services/iot_collector.py
import paho.mqtt.client as mqtt
import json
from datetime import datetime, timezone
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS

class IoTCollector:
    """Recibe datos de sensores IoT vía MQTT y los almacena."""

    def __init__(self, mqtt_broker: str, influx_url: str, influx_token: str):
        self.client = mqtt.Client()
        self.influx = InfluxDBClient(url=influx_url, token=influx_token)
        self.write_api = self.influx.write_api(write_options=SYNCHRONOUS)
        self.buffer = []

    def on_message(self, client, userdata, msg):
        """Procesa mensaje MQTT entrante."""
        data = json.loads(msg.payload.decode())
        point = (
            Point("sensor_reading")
            .tag("device_id", data["device_id"])
            .tag("sensor_type", data["sensor_type"])
            .field("value", float(data["value"]))
            .time(datetime.now(timezone.utc))
        )
        self.write_api.write(bucket="iot_data", record=point)
        self.buffer.append(data)

        # Alerta si valor crítico
        if data.get("alert", False):
            self.trigger_alert(data)

    def trigger_alert(self, data: dict):
        """Envía alerta cuando un sensor detecta anomalía."""
        print(f"ALERTA: {data['device_id']} - {data['sensor_type']}: {data['value']}")

    def start(self, topics: list[str]):
        """Inicia la escucha de sensores."""
        for topic in topics:
            self.client.subscribe(topic)
        self.client.on_message = self.on_message
        self.client.connect("mqtt-broker", 1883)
        self.client.loop_forever()
```

### 2. Time Series Forecasting

```python
# services/forecasting.py
from prophet import Prophet
from sktime.classification.interval_based import TimeSeriesForestClassifier
import polars as pl
import numpy as np

class SensorForecaster:
    """Predice valores futuros de sensores."""

    def __init__(self):
        self.prophet_models = {}

    def train_prophet(self, sensor_id: str, df: pl.DataFrame):
        """Entrena Prophet para un sensor."""
        pdf = df.select(["datetime", "value"]).rename(
            {"datetime": "ds", "value": "y"}
        ).to_pandas()

        model = Prophet(
            daily_seasonality=True,
            weekly_seasonality=True,
            changepoint_prior_scale=0.05,
        )
        model.fit(pdf)
        self.prophet_models[sensor_id] = model

    def forecast(self, sensor_id: str, periods: int = 24) -> pl.DataFrame:
        """Genera predicción futura."""
        model = self.prophet_models[sensor_id]
        future = model.make_future_dataframe(periods=periods, freq="h")
        forecast = model.predict(future)
        return pl.from_pandas(forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]].tail(periods))

class AnomalyDetector:
    """Detecta anomalías en series temporales."""

    def __init__(self, window: int = 24, threshold: float = 3.0):
        self.window = window
        self.threshold = threshold

    def detect(self, values: list[float]) -> list[dict]:
        """Detecta anomalías usando z-score rolling."""
        arr = np.array(values)
        anomalies = []
        for i in range(self.window, len(arr)):
            window_data = arr[i-self.window:i]
            mean = np.mean(window_data)
            std = np.std(window_data)
            if std > 0:
                z_score = (arr[i] - mean) / std
                if abs(z_score) > self.threshold:
                    anomalies.append({
                        "index": i,
                        "value": float(arr[i]),
                        "z_score": float(z_score),
                        "expected_range": [float(mean - 2*std), float(mean + 2*std)],
                    })
        return anomalies
```

---

## Cómo Presentarlo

```
Título: "Plataforma IoT con Forecasting y Detección de Anomalías"

- Recepción de datos en tiempo real vía MQTT
- Almacenamiento en InfluxDB (time series DB)
- Forecasting con Prophet (24h adelante)
- Detección de anomalías con z-score rolling
- Dashboard con Plotly
- Alertas automáticas

Tecnologías: MQTT, InfluxDB, Prophet, Polars, Plotly, FastAPI
```
