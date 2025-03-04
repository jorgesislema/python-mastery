# 📌 Encapsulamiento en Python 🔒

El **encapsulamiento** es uno de los pilares de la **Programación Orientada a Objetos (POO)** y nos permite **proteger los datos** dentro de una clase, evitando que se accedan directamente desde fuera. Con esto, podemos **controlar el acceso** a los atributos y métodos, asegurando que los datos solo sean modificados de manera controlada. 🛡️

---

## 1️⃣ ¿Qué es el Encapsulamiento? 🏠

El encapsulamiento restringe el acceso a ciertos atributos y métodos de una clase. En Python, podemos definir niveles de acceso usando **modificadores de acceso**:

| Modificador | Notación | Accesibilidad |
|------------|----------|---------------|
| **Público** | `self.atributo` | Accesible desde cualquier parte del código. |
| **Protegido** | `_self.atributo` | Indica que no debería ser modificado directamente (convención, pero accesible). |
| **Privado** | `__self.atributo` | No accesible directamente desde fuera de la clase. |

📌 **Nota:** En Python, todos los atributos son técnicamente accesibles, pero los privados (`__atributo`) tienen una convención especial que evita su acceso accidental.

---

## 2️⃣ Atributos Públicos 📢

Los atributos **públicos** pueden ser accedidos y modificados desde cualquier parte del programa.

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre  # Atributo público
        self.edad = edad      # Atributo público

persona = Persona("Juan", 25)
print(persona.nombre)  # "Juan"
persona.edad = 30  # Modificar directamente
print(persona.edad)  # 30
```

📌 **Consejo:** Usa atributos públicos solo cuando no haya riesgo de modificar datos de manera incorrecta.

---

## 3️⃣ Atributos Protegidos 🛡️

Los atributos **protegidos** indican que **no deberían modificarse directamente**, aunque aún es posible acceder a ellos. Se usan con un guion bajo `_`.

```python
class CuentaBancaria:
    def __init__(self, saldo):
        self._saldo = saldo  # Atributo protegido

    def mostrar_saldo(self):
        print(f"Saldo: ${self._saldo}")

cuenta = CuentaBancaria(1000)
print(cuenta._saldo)  # Se puede acceder, pero no es recomendable
```

📌 **Consejo:** No modifiques atributos protegidos fuera de la clase a menos que sea necesario.

---

## 4️⃣ Atributos Privados 🔐

Los atributos **privados** no pueden ser accedidos directamente desde fuera de la clase. Se definen con `__` (doble guion bajo).

```python
class Usuario:
    def __init__(self, nombre, contraseña):
        self.nombre = nombre
        self.__contraseña = contraseña  # Atributo privado
    
    def mostrar_contraseña(self):
        return "*** (Oculto)"

usuario = Usuario("Pedro", "secreta123")
print(usuario.nombre)  # "Pedro"
print(usuario.__contraseña)  # ERROR: No accesible directamente
print(usuario.mostrar_contraseña())  # "*** (Oculto)"
```

📌 **Consejo:** Usa atributos privados para proteger información sensible, como contraseñas o datos bancarios.

---

## 5️⃣ Métodos Getter y Setter 🎛️

Para acceder y modificar atributos privados, usamos **métodos getter y setter**.

### 🔹 Método Getter (Obtener un Valor)
```python
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.__precio = precio  # Atributo privado

    def get_precio(self):  # Getter
        return self.__precio
```
```python
p = Producto("Laptop", 1500)
print(p.get_precio())  # 1500
```

### 🔹 Método Setter (Modificar un Valor)
```python
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.__precio = precio

    def set_precio(self, nuevo_precio):  # Setter
        if nuevo_precio > 0:
            self.__precio = nuevo_precio
        else:
            print("El precio debe ser positivo.")
```
```python
p = Producto("Laptop", 1500)
p.set_precio(1800)  # Modifica el precio
print(p.get_precio())  # 1800
p.set_precio(-5)  # "El precio debe ser positivo."
```

📌 **Consejo:** Usa métodos getter y setter para controlar el acceso y validación de atributos privados.

---

## 6️⃣ Propiedades con `@property` 🏡

Podemos usar `@property` para crear getters y setters de manera más elegante.

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.__edad = edad
    
    @property
    def edad(self):  # Getter
        return self.__edad
    
    @edad.setter
    def edad(self, nueva_edad):  # Setter
        if nueva_edad > 0:
            self.__edad = nueva_edad
        else:
            print("La edad debe ser positiva.")
```
```python
p = Persona("Ana", 25)
print(p.edad)  # 25
p.edad = 30  # Modifica la edad
print(p.edad)  # 30
p.edad = -5  # "La edad debe ser positiva."
```

📌 **Consejo:** Usa `@property` para hacer más intuitivo el acceso a atributos privados.

---

## 🎯 Resumen 📌

| Concepto | Descripción |
|----------|------------|
| **Público** | Accesible desde cualquier parte. |
| **Protegido** | Indica que no debería modificarse directamente (convención). |
| **Privado** | No accesible directamente desde fuera de la clase. |
| **Getter** | Método para obtener un valor privado. |
| **Setter** | Método para modificar un valor privado. |
| **@property** | Forma elegante de definir getters y setters. |

---

## 📌 Consejos Finales 🎯
✅ Usa **atributos privados** para datos sensibles.
✅ Implementa **getters y setters** para validar datos antes de modificarlos.
✅ Aplica `@property` cuando quieras un acceso más intuitivo.
✅ No modifiques **atributos protegidos** fuera de la clase.
✅ **Practica creando clases con diferentes niveles de acceso.** 🚀



