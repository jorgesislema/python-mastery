# 📌 Clases y Objetos en Python 🚀

## 1️⃣ Introducción a la Programación Orientada a Objetos (POO) 🏗️

En Python, la **Programación Orientada a Objetos (POO)** es un enfoque que nos permite organizar el código en torno a "objetos", que representan cosas del mundo real. Es como construir un mundo digital con piezas reutilizables. 🧩

Pero, ¿qué es un **objeto**? 🤔

Un **objeto** es algo que tiene **características** (atributos) y **puede hacer cosas** (métodos). Por ejemplo:
- Un **carro** tiene atributos como color, marca y modelo, y métodos como acelerar y frenar.
- Un **perro** tiene atributos como raza y edad, y métodos como ladrar y correr.

## 2️⃣ ¿Qué es una Clase? 📦

Una **clase** es como un molde o plano para crear objetos. Define qué atributos y métodos tendrán los objetos que creemos a partir de ella.

**Ejemplo:** Imagina que una clase es como un molde de galletas 🍪. Con un solo molde, podemos hacer muchas galletas similares.

### 🔹 Sintaxis de una Clase
```python
class Carro:
    def __init__(self, marca, color):
        self.marca = marca  # Atributo
        self.color = color  # Atributo

    def acelerar(self):
        print(f"El carro {self.marca} está acelerando.")  # Método
```

📌 **Explicación:**
- `class Carro:` → Creamos una clase llamada `Carro`.
- `def __init__(self, marca, color):` → Es el **constructor**, se ejecuta al crear un nuevo objeto.
- `self.marca = marca` → Asigna un atributo al objeto.
- `def acelerar(self):` → Es un **método**, una acción que el objeto puede realizar.

## 3️⃣ Crear Objetos en Python 🏎️

Para crear un objeto a partir de una clase, lo "instanciamos".

### 🔹 Ejemplo de Creación de Objetos
```python
mi_carro = Carro("Toyota", "Rojo")  # Creamos un objeto
print(mi_carro.marca)  # Toyota
print(mi_carro.color)  # Rojo
mi_carro.acelerar()  # "El carro Toyota está acelerando."
```

📌 **Explicación:**
- `mi_carro = Carro("Toyota", "Rojo")` → Creamos un objeto con valores específicos.
- `mi_carro.marca` → Accedemos a su atributo `marca`.
- `mi_carro.acelerar()` → Llamamos a su método `acelerar()`.

## 4️⃣ Métodos en Clases ⚙️

Los **métodos** son funciones dentro de una clase que definen qué acciones puede realizar un objeto.

```python
class Perro:
    def __init__(self, nombre):
        self.nombre = nombre
    
    def ladrar(self):
        print(f"{self.nombre} está ladrando 🐶")
```

```python
mi_perro = Perro("Max")
mi_perro.ladrar()  # "Max está ladrando 🐶"
```

📌 **Consejo:** Usa métodos para definir comportamientos específicos en tus objetos.

---

## 5️⃣ Encapsulamiento: Controlando el Acceso 🔒

A veces, queremos que ciertos atributos sean privados, es decir, que solo sean accesibles dentro de la clase. Para eso, usamos un guion bajo `_` o doble guion `__`.

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self.__saldo = saldo  # Atributo privado
    
    def mostrar_saldo(self):
        print(f"Saldo disponible: ${self.__saldo}")
```

```python
cuenta = CuentaBancaria(1000)
cuenta.mostrar_saldo()  # "Saldo disponible: $1000"
print(cuenta.__saldo)  # Error: No se puede acceder directamente
```

📌 **Consejo:** Usa atributos privados (`__`) para proteger información sensible.

---

## 6️⃣ Herencia: Reutilizando Código 📜

La **herencia** permite crear una nueva clase basada en otra. La nueva clase (hija) hereda atributos y métodos de la clase original (padre).

```python
class Animal:
    def __init__(self, nombre):
        self.nombre = nombre
    
    def hacer_sonido(self):
        print("Este animal hace un sonido.")

class Gato(Animal):  # Hereda de Animal
    def hacer_sonido(self):
        print("Miau 🐱")
```

```python
gato = Gato("Michi")
gato.hacer_sonido()  # "Miau 🐱"
```

📌 **Consejo:** Usa la herencia para evitar repetir código y organizar mejor las clases.

---

## 7️⃣ Polimorfismo: Métodos con Diferentes Comportamientos 🔄

El **polimorfismo** permite redefinir métodos en clases hijas para que tengan un comportamiento diferente al de la clase padre.

```python
class Ave:
    def volar(self):
        print("Esta ave vuela.")

class Pinguino(Ave):
    def volar(self):
        print("Los pingüinos no pueden volar. 🐧")
```

```python
pinguino = Pinguino()
pinguino.volar()  # "Los pingüinos no pueden volar. 🐧"
```

📌 **Consejo:** Usa el polimorfismo cuando necesites que clases hijas tengan su propio comportamiento.

---

## 🎯 Resumen 📌

| Concepto | Descripción |
|----------|------------|
| **Clase** | Plantilla para crear objetos con atributos y métodos. |
| **Objeto** | Instancia de una clase. |
| **Método** | Función dentro de una clase que define acciones del objeto. |
| **Encapsulamiento** | Restringe el acceso a atributos y métodos privados. |
| **Herencia** | Permite crear clases nuevas basadas en otras existentes. |
| **Polimorfismo** | Métodos con el mismo nombre en clases diferentes pero con distinto comportamiento. |

---

## 📌 Consejos Finales 🎯
✅ **Piensa en objetos reales:** Define clases basadas en objetos del mundo real.
✅ **Organiza bien tu código:** Usa clases para hacer tu código más reutilizable.
✅ **Aplica la herencia cuando sea útil:** No la uses si una clase no necesita compartir características con otra.
✅ **Evita acceder directamente a atributos privados:** Usa métodos para modificar y acceder a ellos.
✅ **Practica, practica y practica:** La mejor forma de aprender POO es aplicándola en proyectos reales. 🚀

