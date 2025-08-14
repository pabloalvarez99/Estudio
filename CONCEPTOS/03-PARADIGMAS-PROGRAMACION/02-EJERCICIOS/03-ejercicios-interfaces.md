# 03. Ejercicios de Interfaces en POO

## 🎯 Ejercicios Básicos

### **Ejercicio 1: Interfaz Reproducible**
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

class ReproductorMP3(Reproducible):
    def __init__(self):
        self.archivo_actual = None
        self.reproduciendo = False
    
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
```

### **Ejercicio 2: Interfaz Persistible**
```python
class Persistible(ABC):
    @abstractmethod
    def guardar(self, datos):
        pass
    
    @abstractmethod
    def cargar(self, id):
        pass

class BaseDatosSQL(Persistible):
    def guardar(self, datos):
        return f"Guardando en SQL: {datos}"
    
    def cargar(self, id):
        return f"Datos cargados de SQL con ID: {id}"

class ArchivoJSON(Persistible):
    def guardar(self, datos):
        return f"Guardando en JSON: {datos}"
    
    def cargar(self, id):
        return f"Datos cargados de JSON con ID: {id}"
```

## 🔧 Ejercicios Intermedios

### **Ejercicio 3: Sistema de Plugins**
```python
class Plugin(ABC):
    @abstractmethod
    def ejecutar(self, datos):
        pass
    
    @abstractmethod
    def obtener_nombre(self):
        pass

class PluginFiltro(Plugin):
    def ejecutar(self, datos):
        return [x for x in datos if x > 0]
    
    def obtener_nombre(self):
        return "Filtro Positivos"

class PluginTransformacion(Plugin):
    def ejecutar(self, datos):
        return [x * 2 for x in datos]
    
    def obtener_nombre(self):
        return "Duplicador"

class GestorPlugins:
    def __init__(self):
        self.plugins = []
    
    def registrar_plugin(self, plugin):
        self.plugins.append(plugin)
    
    def ejecutar_todos(self, datos):
        resultado = datos
        for plugin in self.plugins:
            resultado = plugin.ejecutar(resultado)
        return resultado
```

## 🚀 Ejercicios Avanzados

### **Ejercicio 4: Sistema de Validación**
```python
class Validador(ABC):
    @abstractmethod
    def validar(self, dato):
        pass
    
    @abstractmethod
    def obtener_error(self):
        pass

class ValidadorEmail(Validador):
    def __init__(self):
        self.error = None
    
    def validar(self, email):
        if '@' in email and '.' in email:
            self.error = None
            return True
        self.error = "Email inválido"
        return False
    
    def obtener_error(self):
        return self.error

class ValidadorEdad(Validador):
    def __init__(self):
        self.error = None
    
    def validar(self, edad):
        if isinstance(edad, int) and 0 <= edad <= 120:
            self.error = None
            return True
        self.error = "Edad inválida"
        return False
    
    def obtener_error(self):
        return self.error
```

## 📝 Ejercicios de Práctica

### **Ejercicio 5: Implementa una Interfaz de Cálculo**
Crea diferentes calculadoras que implementen la misma interfaz.

### **Ejercicio 6: Sistema de Logging**
Implementa diferentes tipos de loggers con una interfaz común.

### **Ejercicio 7: Gestor de Archivos**
Crea clases para diferentes tipos de archivos con interfaz común.

## 🔍 Preguntas de Reflexión

1. **¿Cuándo usar interfaces vs clases abstractas?**
2. **¿Cómo implementar interfaces en Python?**
3. **¿Qué ventajas tiene el uso de interfaces?**

---

**Anterior**: [02. Ejercicios de Herencia](./02-ejercicios-herencia.md)  
**Siguiente**: [04. Ejercicios Funcionales](./04-ejercicios-funcional.md) 