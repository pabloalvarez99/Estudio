# 02. Ejercicios de Herencia en POO

## 🎯 Ejercicios Básicos

### **Ejercicio 1: Herencia Simple - Vehículos**
Crea una jerarquía de vehículos:

```python
class Vehiculo:
    def __init__(self, marca, modelo):
        self.marca = marca
        self.modelo = modelo
    
    def arrancar(self):
        return f"{self.marca} {self.modelo} está arrancando"
    
    def detener(self):
        return f"{self.marca} {self.modelo} se ha detenido"

class Coche(Vehiculo):
    def __init__(self, marca, modelo, num_puertas):
        super().__init__(marca, modelo)
        self.num_puertas = num_puertas
    
    def abrir_puertas(self):
        return f"Abriendo {self.num_puertas} puertas"

class Moto(Vehiculo):
    def __init__(self, marca, modelo, cilindrada):
        super().__init__(marca, modelo)
        self.cilindrada = cilindrada
    
    def hacer_wheelie(self):
        return "¡Haciendo wheelie!"
```

### **Ejercicio 2: Herencia Multinivel - Formas Geométricas**
```python
from abc import ABC, abstractmethod

class Forma(ABC):
    @abstractmethod
    def calcular_area(self):
        pass
    
    @abstractmethod
    def calcular_perimetro(self):
        pass

class Poligono(Forma):
    def __init__(self, num_lados):
        self.num_lados = num_lados

class Triangulo(Poligono):
    def __init__(self, base, altura):
        super().__init__(3)
        self.base = base
        self.altura = altura
    
    def calcular_area(self):
        return (self.base * self.altura) / 2
    
    def calcular_perimetro(self):
        # Triángulo equilátero simplificado
        return self.base * 3

class Cuadrado(Poligono):
    def __init__(self, lado):
        super().__init__(4)
        self.lado = lado
    
    def calcular_area(self):
        return self.lado ** 2
    
    def calcular_perimetro(self):
        return self.lado * 4
```

## 🔧 Ejercicios Intermedios

### **Ejercicio 3: Sistema de Empleados con Herencia**
```python
from abc import ABC, abstractmethod

class Empleado(ABC):
    def __init__(self, nombre, salario_base):
        self.nombre = nombre
        self.salario_base = salario_base
    
    @abstractmethod
    def calcular_salario(self):
        pass

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
```

### **Ejercicio 4: Herencia Múltiple - Dispositivos**
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

## 🚀 Ejercicios Avanzados

### **Ejercicio 5: Sistema de Notificaciones**
```python
from abc import ABC, abstractmethod

class Notificable(ABC):
    @abstractmethod
    def enviar(self, mensaje):
        pass

class EmailNotificador(Notificable):
    def enviar(self, mensaje):
        return f"Email enviado: {mensaje}"

class SMSNotificador(Notificable):
    def enviar(self, mensaje):
        return f"SMS enviado: {mensaje}"

class NotificacionServicio:
    def __init__(self, notificadores):
        self.notificadores = notificadores
    
    def enviar_masivo(self, mensaje):
        resultados = []
        for notificador in self.notificadores:
            resultado = notificador.enviar(mensaje)
            resultados.append(resultado)
        return resultados
```

## 📝 Ejercicios de Práctica

### **Ejercicio 6: Implementa una Clase Animal**
Crea una jerarquía de animales con métodos específicos.

### **Ejercicio 7: Sistema de Pagos**
Implementa diferentes métodos de pago con herencia.

### **Ejercicio 8: Gestor de Archivos**
Crea clases para diferentes tipos de archivos.

## 🔍 Preguntas de Reflexión

1. **¿Cuándo usar herencia vs composición?**
2. **¿Cómo evitar la herencia profunda?**
3. **¿Qué ventajas tiene la herencia múltiple?**

---

**Anterior**: [01. Ejercicios de Clases](./01-ejercicios-clases.md)  
**Siguiente**: [03. Ejercicios de Interfaces](./03-ejercicios-interfaces.md) 