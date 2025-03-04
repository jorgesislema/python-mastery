# 📌 Ejercicios de Programación Orientada a Objetos (POO) en Python 🐍

La mejor manera de aprender **Programación Orientada a Objetos (POO)** en Python es practicando con ejercicios. Aquí tienes una serie de ejercicios desde lo más básico hasta conceptos avanzados. 💡🚀

---

## 1️⃣ Ejercicios Básicos 🎯

### 🔹 Ejercicio 1: Crear una Clase y un Objeto 🏗️
Crea una clase `Persona` con los atributos `nombre` y `edad`. Luego, instancia un objeto de esta clase e imprime sus atributos.

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

# Crear un objeto de la clase Persona
persona1 = Persona("Juan", 30)
print(persona1.nombre)  # Juan
print(persona1.edad)  # 30
```

### 🔹 Ejercicio 2: Métodos de una Clase ⚙️
Añade un método `saludar()` a la clase `Persona` que imprima un saludo con el nombre de la persona.

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

    def saludar(self):
        print(f"Hola, mi nombre es {self.nombre} y tengo {self.edad} años.")

persona1 = Persona("Juan", 30)
persona1.saludar()
```

---

## 2️⃣ Ejercicios Intermedios ⚡

### 🔹 Ejercicio 3: Encapsulamiento 🔒
Modifica la clase `CuentaBancaria` para que el saldo sea un atributo privado y accede a él mediante un método `get_saldo()`.

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.__saldo = saldo  # Atributo privado
    
    def get_saldo(self):
        return self.__saldo

cuenta = CuentaBancaria(1000)
print(cuenta.get_saldo())  # 1000
```

### 🔹 Ejercicio 4: Herencia 👨‍👩‍👦
Crea una clase `Estudiante` que herede de `Persona` y tenga un atributo adicional `grado`.

```python
class Estudiante(Persona):
    def __init__(self, nombre, edad, grado):
        super().__init__(nombre, edad)
        self.grado = grado

    def estudiar(self):
        print(f"{self.nombre} está estudiando en el grado {self.grado}.")

estudiante1 = Estudiante("Ana", 20, "Universidad")
estudiante1.saludar()
estudiante1.estudiar()
```

---

## 3️⃣ Ejercicios Avanzados 🚀

### 🔹 Ejercicio 5: Polimorfismo 🎭
Crea una función que acepte diferentes tipos de clases y llame a su método `hacer_sonido()`.

```python
class Perro:
    def hacer_sonido(self):
        return "Guau!"

class Gato:
    def hacer_sonido(self):
        return "Miau!"

# Polimorfismo
animales = [Perro(), Gato()]
for animal in animales:
    print(animal.hacer_sonido())
```

### 🔹 Ejercicio 6: Patrón Singleton 🔄
Implementa el patrón Singleton en una clase `Configuracion`.

```python
class Configuracion:
    _instancia = None
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
        return cls._instancia

config1 = Configuracion()
config2 = Configuracion()
print(config1 is config2)  # True
```

---

## 🎯 Resumen 📌

| Concepto | Ejercicio |
|----------|------------|
| **Clases y Objetos** | Crear una clase `Persona` con atributos y métodos. |
| **Encapsulamiento** | Hacer el saldo de `CuentaBancaria` privado. |
| **Herencia** | Crear una clase `Estudiante` que herede de `Persona`. |
| **Polimorfismo** | Implementar un método `hacer_sonido()` en diferentes clases. |
| **Patrón Singleton** | Asegurar que `Configuracion` solo tenga una instancia. |

---

## 📌 Consejos Finales 🎯
✅ **Practica constantemente** para reforzar tus conocimientos en POO.  
✅ **Aplica los conceptos en proyectos reales** para entender su utilidad.  
✅ **No te quedes solo con la teoría**. Implementa estos ejercicios y experimenta con ellos.  
✅ **Lee y mejora tu código** para que sea más eficiente y organizado.  

