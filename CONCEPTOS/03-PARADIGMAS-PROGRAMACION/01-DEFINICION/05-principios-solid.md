# 05. Principios SOLID

## 📖 ¿Qué son los Principios SOLID?

Los **principios SOLID** son cinco principios fundamentales de la programación orientada a objetos y el diseño de software, introducidos por Robert C. Martin. Estos principios ayudan a crear software más mantenible, extensible y comprensible.

## 🔤 Acrónimo SOLID

- **S** - Single Responsibility Principle (Principio de Responsabilidad Única)
- **O** - Open/Closed Principle (Principio Abierto/Cerrado)
- **L** - Liskov Substitution Principle (Principio de Sustitución de Liskov)
- **I** - Interface Segregation Principle (Principio de Segregación de Interfaces)
- **D** - Dependency Inversion Principle (Principio de Inversión de Dependencias)

## 🎯 S - Single Responsibility Principle (SRP)

### **Definición**
Una clase debe tener **una sola razón para cambiar**, es decir, debe tener una sola responsabilidad.

### **Ejemplo Incorrecto**
```python
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email
    
    def guardar_en_bd(self):
        # Responsabilidad 1: Lógica de negocio
        pass
    
    def enviar_email(self, mensaje):
        # Responsabilidad 2: Comunicación
        pass
    
    def validar_datos(self):
        # Responsabilidad 3: Validación
        pass
    
    def generar_reporte(self):
        # Responsabilidad 4: Generación de reportes
        pass
```

### **Ejemplo Correcto**
```python
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email
    
    def obtener_info(self):
        return f"{self.nombre} ({self.email})"

class UsuarioRepositorio:
    def guardar(self, usuario):
        # Solo se encarga de persistencia
        pass
    
    def obtener_por_id(self, id):
        pass

class UsuarioValidador:
    def validar(self, usuario):
        # Solo se encarga de validación
        if not usuario.nombre or not usuario.email:
            return False
        return '@' in usuario.email

class EmailServicio:
    def enviar(self, destinatario, mensaje):
        # Solo se encarga de envío de emails
        pass

class ReporteGenerador:
    def generar_reporte_usuarios(self, usuarios):
        # Solo se encarga de generar reportes
        pass
```

## 🔓 O - Open/Closed Principle (OCP)

### **Definición**
Las entidades de software deben estar **abiertas para extensión pero cerradas para modificación**.

### **Ejemplo Incorrecto**
```python
class CalculadoraDescuentos:
    def calcular_descuento(self, tipo_cliente, monto):
        if tipo_cliente == "regular":
            return monto * 0.05
        elif tipo_cliente == "premium":
            return monto * 0.10
        elif tipo_cliente == "vip":
            return monto * 0.15
        # Si queremos agregar un nuevo tipo, debemos modificar esta clase
```

### **Ejemplo Correcto**
```python
from abc import ABC, abstractmethod

class EstrategiaDescuento(ABC):
    @abstractmethod
    def calcular_descuento(self, monto):
        pass

class DescuentoRegular(EstrategiaDescuento):
    def calcular_descuento(self, monto):
        return monto * 0.05

class DescuentoPremium(EstrategiaDescuento):
    def calcular_descuento(self, monto):
        return monto * 0.10

class DescuentoVIP(EstrategiaDescuento):
    def calcular_descuento(self, monto):
        return monto * 0.15

class CalculadoraDescuentos:
    def __init__(self, estrategia_descuento):
        self.estrategia_descuento = estrategia_descuento
    
    def calcular_descuento(self, monto):
        return self.estrategia_descuento.calcular_descuento(monto)

# Uso
calculadora_regular = CalculadoraDescuentos(DescuentoRegular())
calculadora_premium = CalculadoraDescuentos(DescuentoPremium())

# Para agregar un nuevo tipo, solo creamos una nueva clase
class DescuentoBlackFriday(EstrategiaDescuento):
    def calcular_descuento(self, monto):
        return monto * 0.25

# No necesitamos modificar la clase CalculadoraDescuentos
```

## 🔄 L - Liskov Substitution Principle (LSP)

### **Definición**
Los objetos de una superclase deben poder ser reemplazados por objetos de una subclase sin afectar la funcionalidad del programa.

### **Ejemplo Incorrecto**
```python
class Rectangulo:
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto

class Cuadrado(Rectangulo):
    def __init__(self, lado):
        super().__init__(lado, lado)
    
    def set_ancho(self, ancho):
        self.ancho = ancho
        self.alto = ancho  # Rompe la expectativa de Rectangulo
    
    def set_alto(self, alto):
        self.ancho = alto  # Rompe la expectativa de Rectangulo
        self.alto = alto

# Problema: esto viola LSP
def cambiar_tamanio_rectangulo(rectangulo):
    rectangulo.set_ancho(5)
    rectangulo.set_alto(4)
    # Esperamos que el área sea 20, pero con un Cuadrado será 16
    assert rectangulo.calcular_area() == 20  # Fallará con Cuadrado
```

### **Ejemplo Correcto**
```python
from abc import ABC, abstractmethod

class Forma(ABC):
    @abstractmethod
    def calcular_area(self):
        pass

class Rectangulo(Forma):
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto
    
    def set_ancho(self, ancho):
        self.ancho = ancho
    
    def set_alto(self, alto):
        self.alto = alto

class Cuadrado(Forma):
    def __init__(self, lado):
        self.lado = lado
    
    def calcular_area(self):
        return self.lado ** 2
    
    def set_lado(self, lado):
        self.lado = lado

# Ahora ambas clases pueden ser usadas de manera intercambiable
def calcular_area_total(formas):
    return sum(forma.calcular_area() for forma in formas)

formas = [Rectangulo(3, 4), Cuadrado(5)]
print(calcular_area_total(formas))  # 12 + 25 = 37
```

## 🎭 I - Interface Segregation Principle (ISP)

### **Definición**
Los clientes no deben verse forzados a depender de interfaces que no utilizan.

### **Ejemplo Incorrecto**
```python
from abc import ABC, abstractmethod

class Trabajador(ABC):
    @abstractmethod
    def trabajar(self):
        pass
    
    @abstractmethod
    def comer(self):
        pass
    
    @abstractmethod
    def dormir(self):
        pass

class Humano(Trabajador):
    def trabajar(self):
        return "Trabajando"
    
    def comer(self):
        return "Comiendo"
    
    def dormir(self):
        return "Durmiendo"

class Robot(Trabajador):
    def trabajar(self):
        return "Trabajando 24/7"
    
    def comer(self):
        # Los robots no comen, pero están forzados a implementar este método
        raise NotImplementedError("Los robots no comen")
    
    def dormir(self):
        # Los robots no duermen, pero están forzados a implementar este método
        raise NotImplementedError("Los robots no duermen")
```

### **Ejemplo Correcto**
```python
from abc import ABC, abstractmethod

class Trabajador(ABC):
    @abstractmethod
    def trabajar(self):
        pass

class SerVivo(ABC):
    @abstractmethod
    def comer(self):
        pass
    
    @abstractmethod
    def dormir(self):
        pass

class Humano(Trabajador, SerVivo):
    def trabajar(self):
        return "Trabajando"
    
    def comer(self):
        return "Comiendo"
    
    def dormir(self):
        return "Durmiendo"

class Robot(Trabajador):
    def trabajar(self):
        return "Trabajando 24/7"
    
    # No necesita implementar comer() ni dormir()

class Perro(SerVivo):
    def comer(self):
        return "Comiendo croquetas"
    
    def dormir(self):
        return "Durmiendo en su cama"
    
    # No necesita implementar trabajar()
```

## 🔀 D - Dependency Inversion Principle (DIP)

### **Definición**
Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.

### **Ejemplo Incorrecto**
```python
class BaseDeDatosMySQL:
    def guardar(self, datos):
        return f"Guardando {datos} en MySQL"

class BaseDeDatosPostgreSQL:
    def guardar(self, datos):
        return f"Guardando {datos} en PostgreSQL"

class UsuarioServicio:
    def __init__(self):
        # Dependencia directa de una implementación específica
        self.db = BaseDeDatosMySQL()
    
    def crear_usuario(self, usuario):
        return self.db.guardar(usuario)

# Si queremos cambiar a PostgreSQL, debemos modificar UsuarioServicio
```

### **Ejemplo Correcto**
```python
from abc import ABC, abstractmethod

class RepositorioUsuario(ABC):
    @abstractmethod
    def guardar(self, usuario):
        pass

class BaseDeDatosMySQL(RepositorioUsuario):
    def guardar(self, usuario):
        return f"Guardando {usuario} en MySQL"

class BaseDeDatosPostgreSQL(RepositorioUsuario):
    def guardar(self, usuario):
        return f"Guardando {usuario} en PostgreSQL"

class UsuarioServicio:
    def __init__(self, repositorio: RepositorioUsuario):
        # Depende de una abstracción, no de una implementación específica
        self.repositorio = repositorio
    
    def crear_usuario(self, usuario):
        return self.repositorio.guardar(usuario)

# Uso con inyección de dependencias
servicio_mysql = UsuarioServicio(BaseDeDatosMySQL())
servicio_postgres = UsuarioServicio(BaseDeDatosPostgreSQL())

# Fácil de cambiar sin modificar UsuarioServicio
```

## 🏗️ Aplicación Práctica: Sistema de Notificaciones

### **Implementación con Principios SOLID**
```python
from abc import ABC, abstractmethod
from typing import List

# ISP: Interfaces pequeñas y específicas
class Notificable(ABC):
    @abstractmethod
    def enviar(self, mensaje: str) -> bool:
        pass

class Persistible(ABC):
    @abstractmethod
    def guardar(self, datos: dict) -> bool:
        pass

# SRP: Cada clase tiene una sola responsabilidad
class Usuario:
    def __init__(self, nombre: str, email: str):
        self.nombre = nombre
        self.email = email
    
    def obtener_info(self) -> str:
        return f"{self.nombre} ({self.email})"

class UsuarioValidador:
    def validar(self, usuario: Usuario) -> bool:
        return bool(usuario.nombre and '@' in usuario.email)

class UsuarioRepositorio(Persistible):
    def guardar(self, datos: dict) -> bool:
        # Simulación de guardado en base de datos
        print(f"Guardando usuario: {datos}")
        return True

# OCP: Fácil de extender sin modificar
class EmailNotificador(Notificable):
    def enviar(self, mensaje: str) -> bool:
        print(f"Enviando email: {mensaje}")
        return True

class SMSPushNotificador(Notificable):
    def enviar(self, mensaje: str) -> bool:
        print(f"Enviando SMS/Push: {mensaje}")
        return True

class WhatsAppNotificador(Notificable):
    def enviar(self, mensaje: str) -> bool:
        print(f"Enviando WhatsApp: {mensaje}")
        return True

# LSP: Todas las implementaciones son intercambiables
class NotificacionServicio:
    def __init__(self, notificadores: List[Notificable]):
        self.notificadores = notificadores
    
    def enviar_notificacion_masiva(self, mensaje: str) -> List[bool]:
        resultados = []
        for notificador in self.notificadores:
            resultado = notificador.enviar(mensaje)
            resultados.append(resultado)
        return resultados

# DIP: Depende de abstracciones
class UsuarioServicio:
    def __init__(self, repositorio: Persistible, notificadores: List[Notificable]):
        self.repositorio = repositorio
        self.notificacion_servicio = NotificacionServicio(notificadores)
    
    def crear_usuario(self, nombre: str, email: str) -> bool:
        usuario = Usuario(nombre, email)
        
        # Validación
        validador = UsuarioValidador()
        if not validador.validar(usuario):
            return False
        
        # Persistencia
        if not self.repositorio.guardar({"nombre": nombre, "email": email}):
            return False
        
        # Notificación
        mensaje = f"Usuario {nombre} creado exitosamente"
        self.notificacion_servicio.enviar_notificacion_masiva(mensaje)
        
        return True

# Uso del sistema
def main():
    # Configuración de dependencias
    repositorio = UsuarioRepositorio()
    notificadores = [
        EmailNotificador(),
        SMSPushNotificador(),
        WhatsAppNotificador()
    ]
    
    # Inyección de dependencias
    servicio = UsuarioServicio(repositorio, notificadores)
    
    # Crear usuario
    resultado = servicio.crear_usuario("Ana García", "ana@email.com")
    print(f"Usuario creado: {resultado}")

if __name__ == "__main__":
    main()
```

## 🔍 Beneficios de Aplicar SOLID

### **1. Mantenibilidad**
- Código más fácil de entender y modificar
- Cambios localizados en clases específicas
- Menor riesgo de introducir bugs

### **2. Extensibilidad**
- Fácil agregar nuevas funcionalidades
- Nuevas implementaciones sin modificar código existente
- Sistema más flexible y adaptable

### **3. Testabilidad**
- Código más fácil de probar
- Dependencias claras y controladas
- Mejor cobertura de pruebas

### **4. Reutilización**
- Componentes más modulares
- Fácil reutilizar en diferentes contextos
- Mejor organización del código

## ⚠️ Consideraciones y Advertencias

### **1. No Aplicar Ciegamente**
- Los principios SOLID son guías, no reglas absolutas
- Evaluar el contexto y la complejidad del problema
- Evitar sobre-ingeniería en proyectos simples

### **2. Balance entre Principios**
- A veces los principios pueden entrar en conflicto
- Priorizar según el contexto del proyecto
- Buscar el equilibrio adecuado

### **3. Curva de Aprendizaje**
- Requiere práctica y experiencia
- Puede ser complejo inicialmente
- Beneficios se ven a largo plazo

## 💡 Mejores Prácticas

### **1. Empezar Simple**
```python
# Comenzar con SRP y OCP
# Agregar LSP, ISP y DIP gradualmente
# Refactorizar código existente paso a paso
```

### **2. Usar Patrones de Diseño**
```python
# Strategy Pattern para OCP
# Factory Pattern para DIP
# Adapter Pattern para ISP
```

### **3. Testing Continuo**
```python
# Escribir tests antes de refactorizar
# Verificar que la funcionalidad se mantiene
# Usar tests para validar principios
```

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Clean Code" - Robert C. Martin
- "Clean Architecture" - Robert C. Martin
- "SOLID Principles" - Sandi Metz

### **Práctica Recomendada**
- Refactorizar código existente aplicando SOLID
- Implementar un sistema de plugins
- Crear un framework de testing con inyección de dependencias

---

**Anterior**: [04. Encapsulamiento y Abstracción](./04-encapsulamiento-abstraccion.md)  
**Siguiente**: [06. Patrones de Diseño](./06-patrones-diseno.md) 