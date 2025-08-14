# 04. Encapsulamiento y Abstracción

## 📖 Encapsulamiento en POO

El **encapsulamiento** es uno de los pilares fundamentales de la POO que agrupa datos y métodos relacionados en una sola unidad (clase) y controla el acceso a ellos.

## 🔒 Conceptos de Encapsulamiento

### **1. Agrupación de Datos y Comportamiento**
```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial):
        # Datos (atributos) y comportamiento (métodos) en una sola clase
        self._titular = titular
        self.__saldo = saldo_inicial
        self._historial_transacciones = []
    
    def depositar(self, cantidad):
        if cantidad > 0:
            self.__saldo += cantidad
            self._registrar_transaccion("Depósito", cantidad)
            return True
        return False
    
    def retirar(self, cantidad):
        if 0 < cantidad <= self.__saldo:
            self.__saldo -= cantidad
            self._registrar_transaccion("Retiro", -cantidad)
            return True
        return False
    
    def _registrar_transaccion(self, tipo, cantidad):
        self._historial_transacciones.append({
            "tipo": tipo,
            "cantidad": cantidad,
            "saldo_restante": self.__saldo
        })
```

### **2. Control de Acceso**
Python utiliza convenciones para controlar el acceso a los miembros de una clase:

```python
class Producto:
    def __init__(self, nombre, precio):
        # Público: accesible desde cualquier lugar
        self.nombre = nombre
        
        # Protegido: accesible desde la clase y subclases (convención)
        self._precio = precio
        
        # Privado: solo accesible desde dentro de la clase
        self.__codigo_interno = self._generar_codigo()
    
    def _generar_codigo(self):
        # Método protegido
        import uuid
        return str(uuid.uuid4())[:8]
    
    def __validar_precio(self, precio):
        # Método privado
        return precio > 0
    
    def actualizar_precio(self, nuevo_precio):
        if self.__validar_precio(nuevo_precio):
            self._precio = nuevo_precio
            return True
        return False
```

## 🎯 Niveles de Acceso

### **1. Público (Public)**
```python
class Usuario:
    def __init__(self, nombre, email):
        # Atributos públicos
        self.nombre = nombre
        self.email = email
    
    # Método público
    def actualizar_email(self, nuevo_email):
        self.email = nuevo_email
        return True

# Acceso desde fuera de la clase
usuario = Usuario("Ana", "ana@email.com")
print(usuario.nombre)  # ✅ Acceso directo
usuario.actualizar_email("nuevo@email.com")  # ✅ Llamada directa
```

### **2. Protegido (Protected)**
```python
class Empleado:
    def __init__(self, nombre, salario):
        # Atributo protegido (convención)
        self._nombre = nombre
        self._salario = salario
    
    # Método protegido
    def _calcular_bonificacion(self):
        return self._salario * 0.1

class Gerente(Empleado):
    def __init__(self, nombre, salario, departamento):
        super().__init__(nombre, salario)
        self.departamento = departamento
    
    def obtener_bonificacion(self):
        # Acceso a método protegido desde subclase
        return self._calcular_bonificacion()

# ⚠️ Aunque es posible, no se recomienda acceder desde fuera
empleado = Empleado("Bob", 5000)
print(empleado._nombre)  # Funciona pero no es recomendable
```

### **3. Privado (Private)**
```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial):
        # Atributo privado
        self.__saldo = saldo_inicial
        self.__pin = self.__generar_pin()
    
    def __generar_pin(self):
        # Método privado
        import random
        return str(random.randint(1000, 9999))
    
    def validar_pin(self, pin_ingresado):
        # Método público que usa método privado
        return pin_ingresado == self.__pin

# ❌ Esto generará un error
cuenta = CuentaBancaria("Juan", 1000)
# print(cuenta.__saldo)  # AttributeError
# cuenta.__generar_pin()  # AttributeError

# ✅ Solo a través de métodos públicos
print(cuenta.validar_pin("1234"))  # False (probablemente)
```

## 🎨 Abstracción en POO

La **abstracción** oculta la complejidad y muestra solo las características esenciales de un objeto.

### **1. Clases Abstractas**
```python
from abc import ABC, abstractmethod

class Forma(ABC):
    @abstractmethod
    def calcular_area(self):
        """Método abstracto que debe ser implementado"""
        pass
    
    @abstractmethod
    def calcular_perimetro(self):
        """Método abstracto que debe ser implementado"""
        pass
    
    def obtener_info(self):
        """Método concreto que puede ser usado por todas las subclases"""
        return f"Área: {self.calcular_area():.2f}, Perímetro: {self.calcular_perimetro():.2f}"

class Rectangulo(Forma):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto
    
    def calcular_perimetro(self):
        return 2 * (self.ancho + self.alto)

# ❌ No se puede instanciar una clase abstracta
# forma = Forma()  # TypeError

# ✅ Solo las clases concretas pueden ser instanciadas
rectangulo = Rectangulo(5, 3)
print(rectangulo.obtener_info())
```

### **2. Interfaces (Simuladas en Python)**
```python
from abc import ABC, abstractmethod

class Reproducible(ABC):
    @abstractmethod
    def reproducir(self):
        pass
    
    @abstractmethod
    def pausar(self):
        pass
    
    @abstractmethod
    def detener(self):
        pass

class ReproducibleConVolumen(Reproducible):
    @abstractmethod
    def ajustar_volumen(self, nivel):
        pass

class ReproductorMP3(ReproducibleConVolumen):
    def __init__(self):
        self.archivo_actual = None
        self.reproduciendo = False
        self.volumen = 50
    
    def reproducir(self):
        if self.archivo_actual:
            self.reproduciendo = True
            return f"Reproduciendo {self.archivo_actual}"
        return "No hay archivo para reproducir"
    
    def pausar(self):
        if self.reproduciendo:
            self.reproduciendo = False
            return "Reproducción pausada"
        return "No hay nada reproduciéndose"
    
    def detener(self):
        self.reproduciendo = False
        return "Reproducción detenida"
    
    def ajustar_volumen(self, nivel):
        if 0 <= nivel <= 100:
            self.volumen = nivel
            return f"Volumen ajustado a {nivel}%"
        return "Nivel de volumen inválido"
```

## 🔧 Propiedades y Decoradores

### **1. Propiedades (Getters y Setters)**
```python
class Producto:
    def __init__(self, nombre, precio):
        self._nombre = nombre
        self._precio = precio
    
    @property
    def nombre(self):
        """Getter para el nombre"""
        return self._nombre
    
    @nombre.setter
    def nombre(self, nuevo_nombre):
        """Setter para el nombre con validación"""
        if isinstance(nuevo_nombre, str) and len(nuevo_nombre.strip()) > 0:
            self._nombre = nuevo_nombre.strip()
        else:
            raise ValueError("El nombre debe ser una cadena no vacía")
    
    @property
    def precio(self):
        """Getter para el precio"""
        return self._precio
    
    @precio.setter
    def precio(self, nuevo_precio):
        """Setter para el precio con validación"""
        if isinstance(nuevo_precio, (int, float)) and nuevo_precio >= 0:
            self._precio = nuevo_precio
        else:
            raise ValueError("El precio debe ser un número no negativo")

# Uso de propiedades
producto = Producto("Laptop", 999.99)
print(producto.nombre)  # Laptop
print(producto.precio)  # 999.99

producto.nombre = "Laptop Gaming"  # Usa el setter
producto.precio = 1299.99  # Usa el setter

# ❌ Esto generará un error
# producto.precio = -100  # ValueError
```

### **2. Propiedades de Solo Lectura**
```python
class Usuario:
    def __init__(self, nombre, email):
        self._nombre = nombre
        self._email = email
        self._fecha_registro = None
        self._generar_fecha_registro()
    
    def _generar_fecha_registro(self):
        from datetime import datetime
        self._fecha_registro = datetime.now()
    
    @property
    def nombre(self):
        return self._nombre
    
    @property
    def email(self):
        return self._email
    
    @property
    def fecha_registro(self):
        """Propiedad de solo lectura"""
        return self._fecha_registro
    
    def actualizar_nombre(self, nuevo_nombre):
        """Método para actualizar el nombre con validación"""
        if len(nuevo_nombre.strip()) > 0:
            self._nombre = nuevo_nombre.strip()
            return True
        return False

# Uso
usuario = Usuario("Ana", "ana@email.com")
print(usuario.fecha_registro)  # ✅ Lectura permitida

# ❌ No se puede modificar directamente
# usuario.fecha_registro = "2024-01-01"  # AttributeError
# usuario.email = "nuevo@email.com"  # AttributeError
```

## 🏗️ Encapsulamiento con Métodos Privados

### **1. Métodos de Validación Privados**
```python
class CuentaBancaria:
    def __init__(self, titular, saldo_inicial, pin):
        if self.__validar_titular(titular):
            self._titular = titular
        else:
            raise ValueError("Titular inválido")
        
        if self.__validar_saldo(saldo_inicial):
            self.__saldo = saldo_inicial
        else:
            raise ValueError("Saldo inicial inválido")
        
        if self.__validar_pin(pin):
            self.__pin = pin
        else:
            raise ValueError("PIN inválido")
    
    def __validar_titular(self, titular):
        """Método privado para validar titular"""
        return isinstance(titular, str) and len(titular.strip()) > 0
    
    def __validar_saldo(self, saldo):
        """Método privado para validar saldo"""
        return isinstance(saldo, (int, float)) and saldo >= 0
    
    def __validar_pin(self, pin):
        """Método privado para validar PIN"""
        return isinstance(pin, str) and len(pin) == 4 and pin.isdigit()
    
    def transferir(self, otra_cuenta, cantidad, pin):
        if not self.__validar_pin(pin):
            raise ValueError("PIN incorrecto")
        
        if not self.__validar_saldo(cantidad):
            raise ValueError("Cantidad inválida")
        
        if self.__saldo < cantidad:
            raise ValueError("Saldo insuficiente")
        
        # Realizar transferencia
        self.__saldo -= cantidad
        otra_cuenta.__saldo += cantidad
        return True
```

### **2. Métodos de Utilidad Privados**
```python
class GestorArchivos:
    def __init__(self, directorio_base):
        self._directorio_base = directorio_base
        self._archivos_procesados = []
    
    def procesar_archivo(self, nombre_archivo):
        """Método público para procesar un archivo"""
        if self.__archivo_existe(nombre_archivo):
            contenido = self.__leer_archivo(nombre_archivo)
            resultado = self.__procesar_contenido(contenido)
            self.__guardar_resultado(nombre_archivo, resultado)
            self._archivos_procesados.append(nombre_archivo)
            return True
        return False
    
    def __archivo_existe(self, nombre_archivo):
        """Método privado para verificar existencia"""
        import os
        ruta_completa = os.path.join(self._directorio_base, nombre_archivo)
        return os.path.exists(ruta_completa)
    
    def __leer_archivo(self, nombre_archivo):
        """Método privado para leer archivo"""
        ruta_completa = os.path.join(self._directorio_base, nombre_archivo)
        with open(ruta_completa, 'r', encoding='utf-8') as archivo:
            return archivo.read()
    
    def __procesar_contenido(self, contenido):
        """Método privado para procesar contenido"""
        # Lógica de procesamiento
        return contenido.upper()
    
    def __guardar_resultado(self, nombre_archivo, resultado):
        """Método privado para guardar resultado"""
        nombre_resultado = f"procesado_{nombre_archivo}"
        ruta_resultado = os.path.join(self._directorio_base, nombre_resultado)
        with open(ruta_resultado, 'w', encoding='utf-8') as archivo:
            archivo.write(resultado)
```

## 💡 Ejemplo Completo: Sistema de Biblioteca

```python
from abc import ABC, abstractmethod
from datetime import datetime, timedelta

class ItemBiblioteca(ABC):
    def __init__(self, titulo, autor, isbn):
        self._titulo = titulo
        self._autor = autor
        self._isbn = isbn
        self._disponible = True
        self._fecha_prestamo = None
        self._usuario_prestado = None
    
    @property
    def titulo(self):
        return self._titulo
    
    @property
    def disponible(self):
        return self._disponible
    
    @abstractmethod
    def calcular_fecha_devolucion(self):
        pass
    
    def prestar(self, usuario):
        if self._disponible:
            self._disponible = False
            self._fecha_prestamo = datetime.now()
            self._usuario_prestado = usuario
            return True
        return False
    
    def devolver(self):
        self._disponible = True
        self._fecha_prestamo = None
        self._usuario_prestado = None

class Libro(ItemBiblioteca):
    def __init__(self, titulo, autor, isbn, num_paginas):
        super().__init__(titulo, autor, isbn)
        self._num_paginas = num_paginas
    
    def calcular_fecha_devolucion(self):
        if self._fecha_prestamo:
            return self._fecha_prestamo + timedelta(days=21)  # 3 semanas
        return None

class DVD(ItemBiblioteca):
    def __init__(self, titulo, autor, isbn, duracion):
        super().__init__(titulo, autor, isbn)
        self._duracion = duracion
    
    def calcular_fecha_devolucion(self):
        if self._fecha_prestamo:
            return self._fecha_prestamo + timedelta(days=7)  # 1 semana
        return None

class Usuario:
    def __init__(self, nombre, id_usuario):
        self._nombre = nombre
        self._id_usuario = id_usuario
        self._items_prestados = []
        self._historial_prestamos = []
    
    @property
    def nombre(self):
        return self._nombre
    
    def tomar_prestado(self, item):
        if item.prestar(self):
            self._items_prestados.append(item)
            self._historial_prestamos.append({
                "item": item,
                "fecha_prestamo": datetime.now()
            })
            return True
        return False
    
    def devolver_item(self, item):
        if item in self._items_prestados:
            item.devolver()
            self._items_prestados.remove(item)
            return True
        return False
    
    def obtener_items_vencidos(self):
        items_vencidos = []
        for item in self._items_prestados:
            fecha_devolucion = item.calcular_fecha_devolucion()
            if fecha_devolucion and datetime.now() > fecha_devolucion:
                items_vencidos.append(item)
        return items_vencidos
```

## 🔍 Mejores Prácticas

### **1. Usar Propiedades para Acceso Controlado**
```python
# ✅ Bueno
class Producto:
    def __init__(self, precio):
        self._precio = precio
    
    @property
    def precio(self):
        return self._precio
    
    @precio.setter
    def precio(self, valor):
        if valor >= 0:
            self._precio = valor

# ❌ Evitar
class Producto:
    def __init__(self, precio):
        self.precio = precio  # Acceso directo sin control
```

### **2. Encapsular Lógica de Validación**
```python
# ✅ Bueno
class Usuario:
    def __init__(self, email):
        self._email = self.__validar_email(email)
    
    def __validar_email(self, email):
        if '@' in email and '.' in email:
            return email
        raise ValueError("Email inválido")

# ❌ Evitar
class Usuario:
    def __init__(self, email):
        self.email = email  # Sin validación
```

### **3. Usar Métodos Privados para Funcionalidad Interna**
```python
# ✅ Bueno
class Calculadora:
    def sumar(self, a, b):
        resultado = self.__realizar_suma(a, b)
        self.__registrar_operacion("suma", a, b, resultado)
        return resultado
    
    def __realizar_suma(self, a, b):
        return a + b
    
    def __registrar_operacion(self, operacion, a, b, resultado):
        # Lógica interna de registro
        pass
```

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Effective Python" - Brett Slatkin
- "Python Object-Oriented Programming" - Steven F. Lott
- "Clean Code" - Robert C. Martin

### **Práctica Recomendada**
- Implementar un sistema de gestión de inventario
- Crear un sistema de autenticación con encapsulamiento
- Diseñar un framework de validación de datos

---

**Anterior**: [03. Herencia y Polimorfismo](./03-herencia-polimorfismo.md)  
**Siguiente**: [05. Principios SOLID](./05-principios-solid.md) 