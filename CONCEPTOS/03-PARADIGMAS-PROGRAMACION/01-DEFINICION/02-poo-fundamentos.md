# 02. Fundamentos de Programación Orientada a Objetos (POO)

## 📖 ¿Qué es la Programación Orientada a Objetos?

La **Programación Orientada a Objetos (POO)** es un paradigma de programación que organiza el código en **objetos** que contienen tanto **datos** como **comportamiento**. Los objetos son instancias de **clases**, que actúan como plantillas o moldes para crear objetos.

## 🎯 Conceptos Fundamentales

### **1. Clase (Class)**
Una **clase** es una plantilla o blueprint que define:
- **Atributos**: Características o propiedades del objeto
- **Métodos**: Comportamientos o acciones que puede realizar
- **Constructor**: Método especial para inicializar objetos

```python
class Coche:
    # Constructor
    def __init__(self, marca, modelo, año):
        # Atributos de instancia
        self.marca = marca
        self.modelo = modelo
        self.año = año
        self.velocidad = 0
    
    # Métodos
    def acelerar(self, incremento):
        self.velocidad += incremento
    
    def frenar(self, decremento):
        self.velocidad = max(0, self.velocidad - decremento)
    
    def obtener_info(self):
        return f"{self.marca} {self.modelo} ({self.año})"
```

### **2. Objeto (Object)**
Un **objeto** es una instancia específica de una clase, con valores concretos para sus atributos:

```python
# Crear objetos (instancias) de la clase Coche
mi_coche = Coche("Toyota", "Corolla", 2020)
coche_familiar = Coche("Honda", "CR-V", 2021)

# Usar métodos del objeto
mi_coche.acelerar(50)
print(mi_coche.velocidad)  # 50
print(mi_coche.obtener_info())  # Toyota Corolla (2020)
```

### **3. Atributos (Attributes)**
Los **atributos** almacenan el estado del objeto:

#### **Tipos de Atributos**
- **Atributos de instancia**: Únicos para cada objeto
- **Atributos de clase**: Compartidos por todas las instancias
- **Atributos privados**: Solo accesibles desde dentro de la clase

```python
class Empleado:
    # Atributo de clase (compartido)
    empresa = "TechCorp"
    
    def __init__(self, nombre, salario):
        # Atributos de instancia
        self.nombre = nombre
        self.salario = salario
        # Atributo privado (convención Python)
        self._id_interno = None
    
    def asignar_id(self, id):
        self._id_interno = id
```

### **4. Métodos (Methods)**
Los **métodos** definen el comportamiento del objeto:

#### **Tipos de Métodos**
- **Métodos de instancia**: Operan sobre la instancia específica
- **Métodos de clase**: Operan sobre la clase en general
- **Métodos estáticos**: No requieren acceso a la clase ni instancia

```python
class Calculadora:
    def __init__(self):
        self.historial = []
    
    # Método de instancia
    def sumar(self, a, b):
        resultado = a + b
        self.historial.append(f"{a} + {b} = {resultado}")
        return resultado
    
    # Método de clase
    @classmethod
    def crear_calculadora_cientifica(cls):
        return cls()
    
    # Método estático
    @staticmethod
    def validar_numero(valor):
        return isinstance(valor, (int, float))
```

## 🔧 Pilares de la POO

### **1. Encapsulamiento (Encapsulation)**
Agrupa datos y métodos relacionados en una sola unidad (clase) y controla el acceso a ellos:

```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial):
        self._titular = titular  # Atributo protegido
        self.__saldo = saldo_inicial  # Atributo privado
    
    def depositar(self, cantidad):
        if cantidad > 0:
            self.__saldo += cantidad
            return True
        return False
    
    def obtener_saldo(self):
        return self.__saldo
    
    def transferir(self, otra_cuenta, cantidad):
        if self.__saldo >= cantidad:
            self.__saldo -= cantidad
            otra_cuenta.depositar(cantidad)
            return True
        return False
```

### **2. Herencia (Inheritance)**
Permite crear nuevas clases basadas en clases existentes, reutilizando código:

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
    
    def arrancar(self):
        return f"{self.marca} {self.modelo} está arrancando"
    
    def detener(self):
        return f"{self.marca} {self.modelo} se ha detenido"

class Coche(Vehiculo):  # Coche hereda de Vehiculo
    def __init__(self, marca, modelo, num_puertas):
        # Llamar al constructor de la clase padre
        super().__init__(marca, modelo)
        self.num_puertas = num_puertas
    
    def abrir_puertas(self):
        return f"Abriendo {self.num_puertas} puertas"

class Moto(Vehiculo):  # Moto también hereda de Vehiculo
    def __init__(self, marca, modelo, cilindrada):
        super().__init__(marca, modelo)
        self.cilindrada = cilindrada
    
    def hacer_wheelie(self):
        return "¡Haciendo wheelie!"
```

### **3. Polimorfismo (Polymorphism)**
Permite que diferentes objetos respondan de manera diferente al mismo mensaje:

```python
class Animal:
    def hacer_sonido(self):
        pass

class Perro(Animal):
    def hacer_sonido(self):
        return "¡Guau!"

class Gato(Animal):
    def hacer_sonido(self):
        return "¡Miau!"

class Vaca(Animal):
    def hacer_sonido(self):
        return "¡Muu!"

# Polimorfismo en acción
animales = [Perro(), Gato(), Vaca()]
for animal in animales:
    print(animal.hacer_sonido())
# Salida:
# ¡Guau!
# ¡Miau!
# ¡Muu!
```

### **4. Abstracción (Abstraction)**
Oculta la complejidad y muestra solo las características esenciales:

```python
from abc import ABC, abstractmethod

class Forma(ABC):  # Clase abstracta
    @abstractmethod
    def calcular_area(self):
        pass
    
    @abstractmethod
    def calcular_perimetro(self):
        pass

class Rectangulo(Forma):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto
    
    def calcular_perimetro(self):
        return 2 * (self.ancho + self.alto)

class Circulo(Forma):
    def __init__(self, radio):
        self.radio = radio
    
    def calcular_area(self):
        import math
        return math.pi * self.radio ** 2
    
    def calcular_perimetro(self):
        import math
        return 2 * math.pi * self.radio
```

## 🚀 Ventajas de la POO

### **1. Reutilización de Código**
- Las clases pueden ser reutilizadas en diferentes proyectos
- La herencia permite extender funcionalidad existente
- Los métodos pueden ser llamados múltiples veces

### **2. Mantenibilidad**
- Código organizado y estructurado
- Cambios localizados en clases específicas
- Fácil identificación de problemas

### **3. Escalabilidad**
- Fácil agregar nuevas funcionalidades
- Estructura clara para proyectos grandes
- Separación de responsabilidades

### **4. Modelado del Mundo Real**
- Representación natural de entidades
- Fácil de entender para nuevos desarrolladores
- Mapeo directo entre problema y solución

## ⚠️ Desventajas y Consideraciones

### **1. Complejidad**
- Curva de aprendizaje inicial
- Puede ser excesivo para problemas simples
- Posible sobre-ingeniería

### **2. Performance**
- Overhead de creación de objetos
- Llamadas a métodos más lentas que funciones
- Uso de memoria adicional

### **3. Diseño**
- Requiere buen diseño de clases
- Posibles problemas de acoplamiento
- Difícil refactorización de jerarquías complejas

## 💡 Mejores Prácticas

### **1. Nombres Descriptivos**
```python
# ✅ Bueno
class UsuarioAutenticado:
    def validar_credenciales(self):
        pass

# ❌ Malo
class U:
    def vc(self):
        pass
```

### **2. Responsabilidad Única**
```python
# ✅ Cada clase tiene una responsabilidad
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email

class Autenticador:
    def autenticar(self, usuario, password):
        pass

class GestorPerfil:
    def actualizar_perfil(self, usuario, nuevos_datos):
        pass
```

### **3. Encapsulamiento Apropiado**
```python
class Producto:
    def __init__(self, nombre, precio):
        self._nombre = nombre
        self._precio = precio
    
    @property
    def precio(self):
        return self._precio
    
    @precio.setter
    def precio(self, nuevo_precio):
        if nuevo_precio >= 0:
            self._precio = nuevo_precio
        else:
            raise ValueError("El precio no puede ser negativo")
```

## 🔍 Ejemplo Completo: Sistema de Biblioteca

```python
class Libro:
    def __init__(self, titulo, autor, isbn):
        self.titulo = titulo
        self.autor = autor
        self.isbn = isbn
        self.disponible = True
        self.prestado_a = None
    
    def prestar(self, usuario):
        if self.disponible:
            self.disponible = False
            self.prestado_a = usuario
            return True
        return False
    
    def devolver(self):
        self.disponible = True
        self.prestado_a = None

class Usuario:
    def __init__(self, nombre, id_usuario):
        self.nombre = nombre
        self.id_usuario = id_usuario
        self.libros_prestados = []
    
    def tomar_prestado(self, libro):
        if libro.prestar(self):
            self.libros_prestados.append(libro)
            return True
        return False
    
    def devolver_libro(self, libro):
        if libro in self.libros_prestados:
            libro.devolver()
            self.libros_prestados.remove(libro)
            return True
        return False

class Biblioteca:
    def __init__(self):
        self.libros = []
        self.usuarios = []
    
    def agregar_libro(self, libro):
        self.libros.append(libro)
    
    def registrar_usuario(self, usuario):
        self.usuarios.append(usuario)
    
    def buscar_libro(self, titulo):
        for libro in self.libros:
            if libro.titulo.lower() == titulo.lower():
                return libro
        return None
```

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Object-Oriented Programming in Python" - Steven F. Lott
- "Clean Code" - Robert C. Martin
- "Head First Object-Oriented Analysis and Design" - Brett McLaughlin

### **Práctica Recomendada**
- Implementar un sistema de gestión de estudiantes
- Crear un juego simple con múltiples clases
- Diseñar una API REST con clases de modelo

---

**Anterior**: [01. Concepto de Paradigmas](./01-concepto-paradigmas.md)  
**Siguiente**: [03. Herencia y Polimorfismo](./03-herencia-polimorfismo.md) 