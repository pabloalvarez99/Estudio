# 05. Ejercicios de Patrones de Diseño

## 🎯 Ejercicios Básicos

### **Ejercicio 1: Singleton Pattern**
```python
class Configuracion:
    _instancia = None
    _inicializado = False
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
        return cls._instancia
    
    def __init__(self):
        if not Configuracion._inicializado:
            self.database_url = "localhost:5432"
            self.api_key = "secret_key_123"
            self.debug_mode = True
            Configuracion._inicializado = True
    
    def obtener_configuracion(self):
        return {
            "database_url": self.database_url,
            "api_key": self.api_key,
            "debug_mode": self.debug_mode
        }

# Uso
config1 = Configuracion()
config2 = Configuracion()
print(config1 is config2)  # True - misma instancia
```

### **Ejercicio 2: Factory Method Pattern**
```python
from abc import ABC, abstractmethod

class Documento(ABC):
    @abstractmethod
    def crear_pagina(self):
        pass

class Pagina(ABC):
    @abstractmethod
    def obtener_contenido(self):
        pass

class PaginaResumen(Pagina):
    def obtener_contenido(self):
        return "Página de resumen"

class DocumentoInforme(Documento):
    def crear_pagina(self):
        return PaginaResumen()

# Uso
def crear_documento(tipo_documento: Documento):
    pagina = tipo_documento.crear_pagina()
    return pagina.obtener_contenido()

informe = DocumentoInforme()
print(crear_documento(informe))  # Página de resumen
```

## 🔧 Ejercicios Intermedios

### **Ejercicio 3: Strategy Pattern**
```python
from abc import ABC, abstractmethod

class EstrategiaPago(ABC):
    @abstractmethod
    def procesar_pago(self, monto: float) -> str:
        pass

class PagoTarjeta(EstrategiaPago):
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} con tarjeta"

class PagoPayPal(EstrategiaPago):
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} con PayPal"

class CarritoCompras:
    def __init__(self, estrategia_pago: EstrategiaPago):
        self.estrategia_pago = estrategia_pago
    
    def procesar_compra(self, monto: float) -> str:
        return self.estrategia_pago.procesar_pago(monto)

# Uso
carrito_tarjeta = CarritoCompras(PagoTarjeta())
carrito_paypal = CarritoCompras(PagoPayPal())

print(carrito_tarjeta.procesar_compra(100))  # Pago con tarjeta
print(carrito_paypal.procesar_compra(100))   # Pago con PayPal
```

### **Ejercicio 4: Observer Pattern**
```python
from abc import ABC, abstractmethod
from typing import List

class Observador(ABC):
    @abstractmethod
    def actualizar(self, mensaje: str):
        pass

class Sujeto:
    def __init__(self):
        self._observadores: List[Observador] = []
    
    def agregar_observador(self, observador: Observador):
        if observador not in self._observadores:
            self._observadores.append(observador)
    
    def notificar_observadores(self, mensaje: str):
        for observador in self._observadores:
            observador.actualizar(mensaje)

class ObservadorConcreto(Observador):
    def __init__(self, nombre: str):
        self.nombre = nombre
    
    def actualizar(self, mensaje: str):
        print(f"[{self.nombre}] {mensaje}")

# Uso
sujeto = Sujeto()
obs1 = ObservadorConcreto("Observador 1")
obs2 = ObservadorConcreto("Observador 2")

sujeto.agregar_observador(obs1)
sujeto.agregar_observador(obs2)

sujeto.notificar_observadores("¡Hola Mundo!")
```

## 🚀 Ejercicios Avanzados

### **Ejercicio 5: Command Pattern**
```python
from abc import ABC, abstractmethod
from typing import List

class Comando(ABC):
    @abstractmethod
    def ejecutar(self):
        pass
    
    @abstractmethod
    def deshacer(self):
        pass

class EditorTexto:
    def __init__(self):
        self.texto = ""
        self.historial: List[str] = []
    
    def escribir(self, texto_nuevo: str):
        self.historial.append(self.texto)
        self.texto += texto_nuevo
    
    def deshacer_ultimo_cambio(self):
        if self.historial:
            self.texto = self.historial.pop()

class ComandoEscribir(Comando):
    def __init__(self, editor: EditorTexto, texto: str):
        self.editor = editor
        self.texto = texto
    
    def ejecutar(self):
        self.editor.escribir(self.texto)
    
    def deshacer(self):
        self.editor.deshacer_ultimo_cambio()

class Invocador:
    def __init__(self):
        self.comandos: List[Comando] = []
    
    def ejecutar_comando(self, comando: Comando):
        comando.ejecutar()
        self.comandos.append(comando)
    
    def deshacer_ultimo_comando(self):
        if self.comandos:
            ultimo_comando = self.comandos.pop()
            ultimo_comando.deshacer()

# Uso
editor = EditorTexto()
invocador = Invocador()

comando1 = ComandoEscribir(editor, "Hola ")
comando2 = ComandoEscribir(editor, "Mundo!")

invocador.ejecutar_comando(comando1)
invocador.ejecutar_comando(comando2)
print(f"Texto: '{editor.texto}'")

invocador.deshacer_ultimo_comando()
print(f"Deshecho: '{editor.texto}'")
```

## 📝 Ejercicios de Práctica

### **Ejercicio 6: Implementa un Sistema de Logging**
Crea un sistema de logging usando diferentes patrones.

### **Ejercicio 7: Sistema de Plugins**
Implementa un sistema de plugins usando Factory y Strategy.

### **Ejercicio 8: Gestor de Transacciones**
Crea un gestor de transacciones usando Command y Observer.

## 🔍 Preguntas de Reflexión

1. **¿Cuándo usar cada patrón de diseño?**
2. **¿Cómo combinar múltiples patrones?**
3. **¿Qué ventajas tiene usar patrones?**

---

**Anterior**: [04. Ejercicios Funcionales](./04-ejercicios-funcional.md)  
**Siguiente**: [Proyectos de Paradigmas](./../03-PROYECTOS/01-proyecto-sistema-bancario.md) 