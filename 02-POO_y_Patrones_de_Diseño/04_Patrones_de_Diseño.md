# 📌 Patrones de Diseño en Python 🏗️

Los **patrones de diseño** son soluciones reutilizables a problemas comunes en el desarrollo de software. Nos ayudan a escribir código más organizado, escalable y mantenible. En este documento exploraremos algunos de los patrones más utilizados en **Python**. 🚀

---

## 1️⃣ ¿Qué son los Patrones de Diseño? 🤔

Un **patrón de diseño** es una estrategia probada para resolver problemas recurrentes en el desarrollo de software. Existen tres tipos principales de patrones:

| Tipo | Descripción |
|------|------------|
| **Patrones Creacionales** | Se centran en la creación de objetos de manera eficiente y flexible. |
| **Patrones Estructurales** | Se enfocan en la organización y relaciones entre objetos y clases. |
| **Patrones de Comportamiento** | Se encargan de la comunicación y la interacción entre objetos. |

---

## 2️⃣ Patrones Creacionales 🏗️
Estos patrones optimizan la creación de objetos para hacerla más flexible y eficiente.

### 🔹 Patrón Singleton 🔄
Garantiza que una clase tenga **una única instancia** en todo el programa.

```python
class Singleton:
    _instancia = None
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super(Singleton, cls).__new__(cls)
        return cls._instancia

# Prueba del Singleton
obj1 = Singleton()
obj2 = Singleton()
print(obj1 is obj2)  # True
```
📌 **Uso:** Configuración global, acceso a bases de datos, logs.

### 🔹 Patrón Factory 🏭
Permite la creación de objetos sin especificar la clase exacta de cada objeto.

```python
class Coche:
    def conducir(self):
        return "Conduciendo un coche 🚗"

class Moto:
    def conducir(self):
        return "Conduciendo una moto 🏍️"

class VehiculoFactory:
    @staticmethod
    def crear_vehiculo(tipo):
        if tipo == "coche":
            return Coche()
        elif tipo == "moto":
            return Moto()
        else:
            raise ValueError("Tipo de vehículo desconocido")

vehiculo = VehiculoFactory.crear_vehiculo("coche")
print(vehiculo.conducir())  # "Conduciendo un coche 🚗"
```
📌 **Uso:** Creación dinámica de objetos en bases de datos, interfaces gráficas.

---

## 3️⃣ Patrones Estructurales 🏗️
Estos patrones ayudan a definir la organización y estructura de los objetos.

### 🔹 Patrón Adapter 🔌
Permite que una clase incompatible funcione con otra sin modificar su código.

```python
class EnchufeEuropeo:
    def conectar(self):
        return "Conectado con enchufe europeo 🇪🇺"

class AdaptadorAmericano:
    def __init__(self, enchufe):
        self.enchufe = enchufe
    
    def conectar(self):
        return self.enchufe.conectar() + " usando un adaptador para EE.UU. 🇺🇸"

enchufe = EnchufeEuropeo()
adaptador = AdaptadorAmericano(enchufe)
print(adaptador.conectar())  # "Conectado con enchufe europeo 🇪🇺 usando un adaptador para EE.UU. 🇺🇸"
```
📌 **Uso:** Integración de sistemas con diferentes APIs, compatibilidad de software.

### 🔹 Patrón Decorator 🎭
Permite añadir funcionalidades a un objeto sin modificar su estructura.

```python
def decorador(func):
    def nueva_funcion():
        print("Ejecutando antes de la función principal...")
        func()
        print("Ejecutando después de la función principal...")
    return nueva_funcion

@decorador
def mi_funcion():
    print("¡Hola, mundo!")

mi_funcion()
```
📌 **Uso:** Logging, autorización, cacheo.

---

## 4️⃣ Patrones de Comportamiento 🔄
Estos patrones definen cómo los objetos se comunican entre sí.

### 🔹 Patrón Observer 👀
Define una relación de **suscriptor-publicador**, donde los objetos pueden suscribirse a eventos y recibir notificaciones.

```python
class Observador:
    def actualizar(self, mensaje):
        print(f"Recibí el mensaje: {mensaje}")

class Publicador:
    def __init__(self):
        self.observadores = []
    
    def agregar_observador(self, observador):
        self.observadores.append(observador)
    
    def notificar(self, mensaje):
        for observador in self.observadores:
            observador.actualizar(mensaje)

# Uso del patrón Observer
pub = Publicador()
obs1 = Observador()
obs2 = Observador()
pub.agregar_observador(obs1)
pub.agregar_observador(obs2)
pub.notificar("¡Hola, observadores!")
```
📌 **Uso:** Sistemas de eventos, interfaces gráficas, sistemas de mensajería.

### 🔹 Patrón Strategy 🎯
Define una **familia de algoritmos intercambiables** sin cambiar el código del cliente.

```python
class EstrategiaA:
    def ejecutar(self):
        print("Ejecutando Estrategia A 🅰️")

class EstrategiaB:
    def ejecutar(self):
        print("Ejecutando Estrategia B 🅱️")

class Contexto:
    def __init__(self, estrategia):
        self.estrategia = estrategia
    
    def ejecutar_estrategia(self):
        self.estrategia.ejecutar()

contexto = Contexto(EstrategiaA())
contexto.ejecutar_estrategia()  # "Ejecutando Estrategia A 🅰️"

contexto.estrategia = EstrategiaB()
contexto.ejecutar_estrategia()  # "Ejecutando Estrategia B 🅱️"
```
📌 **Uso:** Algoritmos de ordenamiento, validaciones, autenticación.

---

## 🎯 Resumen 📌

| Patrón | Descripción | Ejemplo de Uso |
|--------|------------|---------------|
| **Singleton** | Garantiza que haya solo una instancia de una clase. | Configuración global, logging. |
| **Factory** | Crea objetos sin especificar la clase exacta. | Creación dinámica de objetos. |
| **Adapter** | Permite que clases incompatibles trabajen juntas. | Integración de APIs. |
| **Decorator** | Añade funcionalidades a un objeto sin modificarlo. | Logging, autenticación. |
| **Observer** | Notifica a múltiples objetos cuando hay un cambio. | Eventos, sistemas de notificaciones. |
| **Strategy** | Permite intercambiar algoritmos sin modificar el código. | Métodos de pago, IA. |

---

## 📌 Consejos Finales 🎯
✅ Usa patrones de diseño para hacer tu código más escalable y mantenible.
✅ No abuses de los patrones; úsalos solo cuando sean necesarios.
✅ Aprende a identificar cuándo un problema puede resolverse con un patrón específico.
✅ Practica implementando estos patrones en proyectos reales. 🚀

