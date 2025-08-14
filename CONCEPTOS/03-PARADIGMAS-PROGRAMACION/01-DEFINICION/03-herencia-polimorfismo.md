# 03. Herencia y Polimorfismo

## 📖 Herencia en POO

La **herencia** es un mecanismo que permite crear nuevas clases basadas en clases existentes, reutilizando código y estableciendo una relación jerárquica entre clases.

## 🔗 Conceptos de Herencia

### **1. Clase Padre (Superclase)**
La clase de la cual se hereda, también llamada clase base o superclase:

```python
class Animal:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad
    
    def hacer_sonido(self):
        return "Hace algún sonido"
    
    def obtener_info(self):
        return f"{self.nombre}, {self.edad} años"
```

### **2. Clase Hija (Subclase)**
La clase que hereda de otra, también llamada clase derivada o subclase:

```python
class Perro(Animal):  # Perro hereda de Animal
    def __init__(self, nombre, edad, raza):
        # Llamar al constructor de la clase padre
        super().__init__(nombre, edad)
        self.raza = raza
    
    def hacer_sonido(self):
        return "¡Guau!"
    
    def obtener_info(self):
        # Llamar al método de la clase padre y extenderlo
        info_base = super().obtener_info()
        return f"{info_base}, Raza: {self.raza}"
```

### **3. Herencia Múltiple**
Python permite que una clase herede de múltiples clases padre:

```python
class Dispositivo:
    def __init__(self, marca):
        self.marca = marca
    
    def encender(self):
        return f"{self.marca} se está encendiendo"

class Comunicable:
    def enviar_mensaje(self, mensaje):
        return f"Enviando: {mensaje}"

class Smartphone(Dispositivo, Comunicable):
    def __init__(self, marca, modelo):
        Dispositivo.__init__(self, marca)
        self.modelo = modelo
    
    def obtener_info(self):
        return f"{self.marca} {self.modelo}"
```

## 🎭 Tipos de Herencia

### **1. Herencia Simple**
Una clase hereda de una sola clase padre:

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
    
    def arrancar(self):
        return f"{self.marca} {self.modelo} está arrancando"

class Coche(Vehiculo):
    def __init__(self, marca, modelo, num_puertas):
        super().__init__(marca, modelo)
        self.num_puertas = num_puertas
```

### **2. Herencia Multinivel**
Una clase hereda de otra clase que a su vez hereda de una tercera:

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo

class Coche(Vehiculo):
    def __init__(self, marca, modelo, num_puertas):
        super().__init__(marca, modelo)
        self.num_puertas = num_puertas

class CocheDeportivo(Coche):
    def __init__(self, marca, modelo, num_puertas, potencia):
        super().__init__(marca, modelo, num_puertas)
        self.potencia = potencia
```

### **3. Herencia Jerárquica**
Múltiples clases heredan de una sola clase padre:

```python
class Forma:
    def calcular_area(self):
        pass

class Rectangulo(Forma):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto

class Circulo(Forma):
    def __init__(self, radio):
        self.radio = radio
    
    def calcular_area(self):
        import math
        return math.pi * self.radio ** 2

class Triangulo(Forma):
    def __init__(self, base, altura):
        self.base = base
        self.altura = altura
    
    def calcular_area(self):
        return (self.base * self.altura) / 2
```

## 🔄 Polimorfismo

El **polimorfismo** permite que diferentes objetos respondan de manera diferente al mismo mensaje o método.

### **1. Polimorfismo de Sobrescritura (Override)**
Las clases hijas redefinen métodos de la clase padre:

```python
class Animal:
    def hacer_sonido(self):
        return "Hace algún sonido"
    
    def moverse(self):
        return "Se está moviendo"

class Perro(Animal):
    def hacer_sonido(self):
        return "¡Guau!"
    
    def moverse(self):
        return "Corre a cuatro patas"

class Pajaro(Animal):
    def hacer_sonido(self):
        return "¡Pío!"
    
    def moverse(self):
        return "Vuela"

# Polimorfismo en acción
animales = [Perro(), Pajaro()]
for animal in animales:
    print(f"Sonido: {animal.hacer_sonido()}")
    print(f"Movimiento: {animal.moverse()}")
    print("---")
```

### **2. Polimorfismo de Sobrecarga (Overloading)**
Python no soporta sobrecarga tradicional, pero se puede simular:

```python
class Calculadora:
    def sumar(self, a, b, c=None):
        if c is None:
            return a + b
        else:
            return a + b + c
    
    def multiplicar(self, *args):
        resultado = 1
        for num in args:
            resultado *= num
        return resultado

calc = Calculadora()
print(calc.sumar(5, 3))      # 8
print(calc.sumar(5, 3, 2))   # 10
print(calc.multiplicar(2, 3, 4))  # 24
```

### **3. Polimorfismo de Duck Typing**
"Si camina como un pato y suena como un pato, entonces es un pato":

```python
class Pato:
    def nadar(self):
        return "Nadando como un pato"
    
    def hacer_sonido(self):
        return "¡Cuac!"

class Avion:
    def volar(self):
        return "Volando como un avión"
    
    def hacer_sonido(self):
        return "¡Rrrrr!"

def hacer_sonido_animal(animal):
    return animal.hacer_sonido()

# Ambos objetos tienen el método hacer_sonido
print(hacer_sonido_animal(Pato()))   # ¡Cuac!
print(hacer_sonido_animal(Avion()))  # ¡Rrrrr!
```

## 🏗️ Herencia y Composición

### **1. Herencia "Es un" (Is-A)**
```python
class Empleado:
    def __init__(self, nombre, salario):
        self.nombre = nombre
        self.salario = salario

class Gerente(Empleado):  # Un Gerente ES UN Empleado
    def __init__(self, nombre, salario, departamento):
        super().__init__(nombre, salario)
        self.departamento = departamento
```

### **2. Composición "Tiene un" (Has-A)**
```python
class Motor:
    def __init__(self, potencia):
        self.potencia = potencia
    
    def arrancar(self):
        return f"Motor de {self.potencia}HP arrancando"

class Coche:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
        self.motor = Motor(150)  # El coche TIENE UN motor
    
    def arrancar(self):
        return f"{self.marca} {self.modelo}: {self.motor.arrancar()}"
```

## ⚠️ Consideraciones Importantes

### **1. Orden de Resolución de Métodos (MRO)**
```python
class A:
    def metodo(self):
        return "A"

class B(A):
    def metodo(self):
        return "B"

class C(A):
    def metodo(self):
        return "C"

class D(B, C):
    pass

# MRO: D -> B -> C -> A
d = D()
print(d.metodo())  # "B" (primera en la cadena de herencia)
print(D.__mro__)   # Muestra el orden de resolución
```

### **2. Evitar Herencia Profunda**
```python
# ❌ Evitar herencia muy profunda
class A: pass
class B(A): pass
class C(B): pass
class D(C): pass
class E(D): pass
class F(E): pass

# ✅ Preferir composición
class Componente:
    def funcionalidad(self):
        return "Funcionalidad básica"

class Sistema:
    def __init__(self):
        self.componente = Componente()
    
    def funcionalidad(self):
        return self.componente.funcionalidad()
```

## 💡 Ejemplos Prácticos

### **1. Sistema de Formas Geométricas**
```python
from abc import ABC, abstractmethod
import math

class Forma(ABC):
    @abstractmethod
    def calcular_area(self):
        pass
    
    @abstractmethod
    def calcular_perimetro(self):
        pass
    
    def obtener_info(self):
        return f"Área: {self.calcular_area():.2f}, Perímetro: {self.calcular_perimetro():.2f}"

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
        return math.pi * self.radio ** 2
    
    def calcular_perimetro(self):
        return 2 * math.pi * self.radio

# Polimorfismo en acción
formas = [Rectangulo(5, 3), Circulo(4)]
for forma in formas:
    print(f"{forma.__class__.__name__}: {forma.obtener_info()}")
```

### **2. Sistema de Empleados**
```python
class Empleado:
    def __init__(self, nombre, salario_base):
        self.nombre = nombre
        self.salario_base = salario_base
    
    def calcular_salario(self):
        return self.salario_base
    
    def obtener_info(self):
        return f"{self.nombre}: ${self.calcular_salario():.2f}"

class EmpleadoPorHora(Empleado):
    def __init__(self, nombre, salario_hora, horas_trabajadas):
        super().__init__(nombre, salario_hora)
        self.horas_trabajadas = horas_trabajadas
    
    def calcular_salario(self):
        return self.salario_base * self.horas_trabajadas

class EmpleadoComision(Empleado):
    def __init__(self, nombre, salario_base, ventas, comision_porcentaje):
        super().__init__(nombre, salario_base)
        self.ventas = ventas
        self.comision_porcentaje = comision_porcentaje
    
    def calcular_salario(self):
        comision = self.ventas * (self.comision_porcentaje / 100)
        return self.salario_base + comision

# Sistema polimórfico
empleados = [
    Empleado("Ana", 3000),
    EmpleadoPorHora("Bob", 15, 160),
    EmpleadoComision("Carlos", 2000, 5000, 5)
]

for empleado in empleados:
    print(empleado.obtener_info())
```

## 🔍 Mejores Prácticas

### **1. Usar Herencia para Relaciones "Es un"**
```python
# ✅ Correcto
class Vehiculo: pass
class Coche(Vehiculo): pass  # Un coche ES UN vehículo

# ❌ Incorrecto
class Vehiculo: pass
class Motor: pass
class Coche(Motor): pass  # Un coche NO ES UN motor
```

### **2. Preferir Composición sobre Herencia**
```python
# ✅ Mejor: Composición
class Coche:
    def __init__(self):
        self.motor = Motor()
        self.ruedas = [Rueda() for _ in range(4)]

# ❌ Peor: Herencia múltiple compleja
class Coche(Motor, Rueda, Chasis, Carroceria):
    pass
```

### **3. Mantener Jerarquías Simples**
```python
# ✅ Simple y clara
class Animal: pass
class Mamifero(Animal): pass
class Perro(Mamifero): pass

# ❌ Compleja y difícil de mantener
class A: pass
class B(A): pass
class C(B): pass
class D(C): pass
class E(D): pass
```

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Effective Python" - Brett Slatkin
- "Python Object-Oriented Programming" - Steven F. Lott
- "Design Patterns" - Gang of Four

### **Práctica Recomendada**
- Implementar un sistema de figuras geométricas
- Crear una jerarquía de empleados con diferentes tipos de pago
- Diseñar un sistema de vehículos con herencia y composición

---

**Anterior**: [02. Fundamentos de POO](./02-poo-fundamentos.md)  
**Siguiente**: [04. Encapsulamiento y Abstracción](./04-encapsulamiento-abstraccion.md) 