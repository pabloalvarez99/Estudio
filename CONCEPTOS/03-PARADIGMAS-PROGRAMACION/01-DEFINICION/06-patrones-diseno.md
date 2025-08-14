# 06. Patrones de Diseño

## 📖 ¿Qué son los Patrones de Diseño?

Los **patrones de diseño** son soluciones reutilizables y probadas para problemas comunes de diseño de software. Proporcionan un vocabulario común para los desarrolladores y ayudan a crear código más mantenible, flexible y comprensible.

## 🏗️ Clasificación de Patrones

### **1. Patrones Creacionales**
Se encargan de la creación de objetos, ocultando la lógica de instanciación.

### **2. Patrones Estructurales**
Se enfocan en la composición de clases y objetos para formar estructuras más grandes.

### **3. Patrones de Comportamiento**
Se centran en la comunicación entre objetos y la asignación de responsabilidades.

## 🎯 Patrones Creacionales

### **1. Singleton (Instancia Única)**
Garantiza que una clase tenga solo una instancia y proporciona un punto de acceso global.

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

### **2. Factory Method (Método Fábrica)**
Define una interfaz para crear objetos, pero permite a las subclases decidir qué clase instanciar.

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

class PaginaConclusiones(Pagina):
    def obtener_contenido(self):
        return "Página de conclusiones"

class PaginaBibliografia(Pagina):
    def obtener_contenido(self):
        return "Página de bibliografía"

class DocumentoInforme(Documento):
    def crear_pagina(self):
        return PaginaResumen()

class DocumentoTesis(Documento):
    def crear_pagina(self):
        return PaginaConclusiones()

class DocumentoArticulo(Documento):
    def crear_pagina(self):
        return PaginaBibliografia()

# Uso
def crear_documento(tipo_documento: Documento):
    pagina = tipo_documento.crear_pagina()
    return pagina.obtener_contenido()

# Crear diferentes tipos de documentos
informe = DocumentoInforme()
tesis = DocumentoTesis()
articulo = DocumentoArticulo()

print(crear_documento(informe))   # Página de resumen
print(crear_documento(tesis))     # Página de conclusiones
print(crear_documento(articulo))  # Página de bibliografía
```

### **3. Abstract Factory (Fábrica Abstracta)**
Proporciona una interfaz para crear familias de objetos relacionados sin especificar sus clases concretas.

```python
from abc import ABC, abstractmethod

# Interfaces abstractas
class Boton(ABC):
    @abstractmethod
    def renderizar(self):
        pass

class CampoTexto(ABC):
    @abstractmethod
    def renderizar(self):
        pass

class Ventana(ABC):
    @abstractmethod
    def renderizar(self):
        pass

# Implementaciones para Windows
class BotonWindows(Boton):
    def renderizar(self):
        return "Botón estilo Windows"

class CampoTextoWindows(CampoTexto):
    def renderizar(self):
        return "Campo de texto estilo Windows"

class VentanaWindows(Ventana):
    def renderizar(self):
        return "Ventana estilo Windows"

# Implementaciones para macOS
class BotonMacOS(Boton):
    def renderizar(self):
        return "Botón estilo macOS"

class CampoTextoMacOS(CampoTexto):
    def renderizar(self):
        return "Campo de texto estilo macOS"

class VentanaMacOS(Ventana):
    def renderizar(self):
        return "Ventana estilo macOS"

# Fábricas abstractas
class FabricaUI(ABC):
    @abstractmethod
    def crear_boton(self) -> Boton:
        pass
    
    @abstractmethod
    def crear_campo_texto(self) -> CampoTexto:
        pass
    
    @abstractmethod
    def crear_ventana(self) -> Ventana:
        pass

class FabricaWindows(FabricaUI):
    def crear_boton(self) -> Boton:
        return BotonWindows()
    
    def crear_campo_texto(self) -> CampoTexto:
        return CampoTextoWindows()
    
    def crear_ventana(self) -> Ventana:
        return VentanaWindows()

class FabricaMacOS(FabricaUI):
    def crear_boton(self) -> Boton:
        return BotonMacOS()
    
    def crear_campo_texto(self) -> CampoTexto:
        return CampoTextoMacOS()
    
    def crear_ventana(self) -> Ventana:
        return VentanaMacOS()

# Cliente
class Aplicacion:
    def __init__(self, fabrica: FabricaUI):
        self.fabrica = fabrica
        self.boton = fabrica.crear_boton()
        self.campo_texto = fabrica.crear_campo_texto()
        self.ventana = fabrica.crear_ventana()
    
    def renderizar_ui(self):
        return [
            self.ventana.renderizar(),
            self.boton.renderizar(),
            self.campo_texto.renderizar()
        ]

# Uso
app_windows = Aplicacion(FabricaWindows())
app_macos = Aplicacion(FabricaMacOS())

print("UI Windows:", app_windows.renderizar_ui())
print("UI macOS:", app_macos.renderizar_ui())
```

## 🔧 Patrones Estructurales

### **1. Adapter (Adaptador)**
Permite que interfaces incompatibles trabajen juntas.

```python
# Interfaz existente (legacy)
class SistemaLegacy:
    def obtener_datos(self):
        return "datos_legacy_123"
    
    def procesar_datos(self, datos):
        return f"Procesado: {datos}"

# Nueva interfaz deseada
class SistemaNuevo:
    def obtener_informacion(self):
        return "informacion_nueva_456"
    
    def procesar_informacion(self, info):
        return f"Información procesada: {info}"

# Adaptador que hace compatible el sistema legacy
class AdaptadorSistemaLegacy:
    def __init__(self, sistema_legacy: SistemaLegacy):
        self.sistema_legacy = sistema_legacy
    
    def obtener_informacion(self):
        # Adapta el método legacy al nuevo formato
        datos_legacy = self.sistema_legacy.obtener_datos()
        return datos_legacy.replace("legacy", "nuevo")
    
    def procesar_informacion(self, info):
        # Adapta el procesamiento
        return self.sistema_legacy.procesar_datos(info)

# Cliente que usa la nueva interfaz
class Cliente:
    def __init__(self, sistema):
        self.sistema = sistema
    
    def ejecutar_proceso(self):
        info = self.sistema.obtener_informacion()
        resultado = self.sistema.procesar_informacion(info)
        return resultado

# Uso
sistema_legacy = SistemaLegacy()
sistema_nuevo = SistemaNuevo()
adaptador = AdaptadorSistemaLegacy(sistema_legacy)

cliente1 = Cliente(sistema_nuevo)
cliente2 = Cliente(adaptador)

print(cliente1.ejecutar_proceso())  # Usa sistema nuevo
print(cliente2.ejecutar_proceso())  # Usa sistema legacy adaptado
```

### **2. Decorator (Decorador)**
Añade responsabilidades a objetos individuales de forma dinámica.

```python
from abc import ABC, abstractmethod

class Cafe(ABC):
    @abstractmethod
    def costo(self):
        pass
    
    @abstractmethod
    def descripcion(self):
        pass

class CafeSimple(Cafe):
    def costo(self):
        return 2.0
    
    def descripcion(self):
        return "Café simple"

class DecoradorCafe(Cafe):
    def __init__(self, cafe: Cafe):
        self._cafe = cafe
    
    def costo(self):
        return self._cafe.costo()
    
    def descripcion(self):
        return self._cafe.descripcion()

class Leche(DecoradorCafe):
    def costo(self):
        return self._cafe.costo() + 0.5
    
    def descripcion(self):
        return self._cafe.descripcion() + ", leche"

class Azucar(DecoradorCafe):
    def costo(self):
        return self._cafe.costo() + 0.2
    
    def descripcion(self):
        return self._cafe.descripcion() + ", azúcar"

class Crema(DecoradorCafe):
    def costo(self):
        return self._cafe.costo() + 0.8
    
    def descripcion(self):
        return self._cafe.descripcion() + ", crema"

# Uso
cafe = CafeSimple()
print(f"{cafe.descripcion()}: ${cafe.costo():.2f}")

cafe_con_leche = Leche(cafe)
print(f"{cafe_con_leche.descripcion()}: ${cafe_con_leche.costo():.2f}")

cafe_completo = Crema(Azucar(Leche(cafe)))
print(f"{cafe_completo.descripcion()}: ${cafe_completo.costo():.2f}")
```

### **3. Facade (Fachada)**
Proporciona una interfaz simplificada a un subsistema complejo.

```python
class SubsistemaAudio:
    def reproducir_audio(self, archivo):
        return f"Reproduciendo audio: {archivo}"
    
    def detener_audio(self):
        return "Audio detenido"

class SubsistemaVideo:
    def reproducir_video(self, archivo):
        return f"Reproduciendo video: {archivo}"
    
    def detener_video(self):
        return "Video detenido"

class SubsistemaSubtitulos:
    def cargar_subtitulos(self, archivo):
        return f"Subtítulos cargados: {archivo}"
    
    def mostrar_subtitulos(self):
        return "Subtítulos visibles"

class SubsistemaRed:
    def conectar(self):
        return "Conectado a la red"
    
    def desconectar(self):
        return "Desconectado de la red"

# Fachada que simplifica el uso de los subsistemas
class ReproductorMultimedia:
    def __init__(self):
        self.audio = SubsistemaAudio()
        self.video = SubsistemaVideo()
        self.subtitulos = SubsistemaSubtitulos()
        self.red = SubsistemaRed()
    
    def reproducir_pelicula(self, archivo_video, archivo_audio, archivo_subtitulos):
        print("=== Iniciando reproducción de película ===")
        print(self.red.conectar())
        print(self.video.reproducir_video(archivo_video))
        print(self.audio.reproducir_audio(archivo_audio))
        print(self.subtitulos.cargar_subtitulos(archivo_subtitulos))
        print(self.subtitulos.mostrar_subtitulos())
        print("=== Película reproduciéndose ===")
    
    def detener_pelicula(self):
        print("=== Deteniendo película ===")
        print(self.video.detener_video())
        print(self.audio.detener_audio())
        print(self.red.desconectar())
        print("=== Película detenida ===")

# Cliente simplificado
def main():
    reproductor = ReproductorMultimedia()
    
    # Reproducir película (interfaz simple)
    reproductor.reproducir_pelicula(
        "pelicula.mp4",
        "audio.mp3",
        "subtitulos.srt"
    )
    
    print("\n" + "="*50 + "\n")
    
    # Detener película
    reproductor.detener_pelicula()

if __name__ == "__main__":
    main()
```

## 🎭 Patrones de Comportamiento

### **1. Observer (Observador)**
Define una dependencia uno-a-muchos entre objetos para que cuando un objeto cambie, todos sus dependientes sean notificados.

```python
from abc import ABC, abstractmethod
from typing import List

class Observador(ABC):
    @abstractmethod
    def actualizar(self, mensaje: str):
        pass

class Sujeto(ABC):
    def __init__(self):
        self._observadores: List[Observador] = []
    
    def agregar_observador(self, observador: Observador):
        if observador not in self._observadores:
            self._observadores.append(observador)
    
    def eliminar_observador(self, observador: Observador):
        if observador in self._observadores:
            self._observadores.remove(observador)
    
    def notificar_observadores(self, mensaje: str):
        for observador in self._observadores:
            observador.actualizar(mensaje)

class EstacionMeteorologica(Sujeto):
    def __init__(self):
        super().__init__()
        self._temperatura = 20
        self._humedad = 60
    
    def cambiar_condiciones(self, temperatura: int, humedad: int):
        self._temperatura = temperatura
        self._humedad = humedad
        mensaje = f"Temperatura: {temperatura}°C, Humedad: {humedad}%"
        self.notificar_observadores(mensaje)
    
    def obtener_condiciones(self):
        return self._temperatura, self._humedad

class PantallaTemperatura(Observador):
    def __init__(self, nombre: str):
        self.nombre = nombre
    
    def actualizar(self, mensaje: str):
        print(f"[{self.nombre}] {mensaje}")

class PantallaHumedad(Observador):
    def __init__(self, nombre: str):
        self.nombre = nombre
    
    def actualizar(self, mensaje: str):
        print(f"[{self.nombre}] {mensaje}")

class AlertaClima(Observador):
    def __init__(self, nombre: str):
        self.nombre = nombre
    
    def actualizar(self, mensaje: str):
        if "Temperatura: 30" in mensaje:
            print(f"[{self.nombre}] ⚠️ ALERTA: Temperatura alta detectada!")
        elif "Humedad: 80" in mensaje:
            print(f"[{self.nombre}] ⚠️ ALERTA: Humedad alta detectada!")

# Uso
estacion = EstacionMeteorologica()

# Crear observadores
pantalla_temp = PantallaTemperatura("Pantalla Principal")
pantalla_humedad = PantallaHumedad("Pantalla Secundaria")
alerta = AlertaClima("Sistema de Alertas")

# Registrar observadores
estacion.agregar_observador(pantalla_temp)
estacion.agregar_observador(pantalla_humedad)
estacion.agregar_observador(alerta)

# Cambiar condiciones (notifica automáticamente a todos)
print("=== Cambio de condiciones climáticas ===")
estacion.cambiar_condiciones(25, 65)
print()

estacion.cambiar_condiciones(30, 70)
print()

estacion.cambiar_condiciones(28, 80)
```

### **2. Strategy (Estrategia)**
Define una familia de algoritmos, encapsula cada uno y los hace intercambiables.

```python
from abc import ABC, abstractmethod

class EstrategiaPago(ABC):
    @abstractmethod
    def procesar_pago(self, monto: float) -> str:
        pass

class PagoTarjetaCredito(EstrategiaPago):
    def __init__(self, numero_tarjeta: str):
        self.numero_tarjeta = numero_tarjeta
    
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} procesado con tarjeta de crédito terminada en {self.numero_tarjeta[-4:]}"

class PagoPayPal(EstrategiaPago):
    def __init__(self, email: str):
        self.email = email
    
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} procesado con PayPal desde {self.email}"

class PagoTransferencia(EstrategiaPago):
    def __init__(self, cuenta_bancaria: str):
        self.cuenta_bancaria = cuenta_bancaria
    
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} procesado por transferencia a cuenta {self.cuenta_bancaria}"

class PagoCriptomoneda(EstrategiaPago):
    def __init__(self, direccion_wallet: str):
        self.direccion_wallet = direccion_wallet
    
    def procesar_pago(self, monto: float) -> str:
        return f"Pago de ${monto:.2f} procesado con criptomoneda a wallet {self.direccion_wallet[:8]}..."

class CarritoCompras:
    def __init__(self):
        self.items = []
        self.estrategia_pago = None
    
    def agregar_item(self, item: str, precio: float):
        self.items.append({"item": item, "precio": precio})
    
    def establecer_estrategia_pago(self, estrategia: EstrategiaPago):
        self.estrategia_pago = estrategia
    
    def calcular_total(self) -> float:
        return sum(item["precio"] for item in self.items)
    
    def procesar_compra(self) -> str:
        if not self.estrategia_pago:
            return "Error: No se ha establecido método de pago"
        
        total = self.calcular_total()
        return self.estrategia_pago.procesar_pago(total)
    
    def mostrar_items(self):
        print("Items en el carrito:")
        for item in self.items:
            print(f"- {item['item']}: ${item['precio']:.2f}")
        print(f"Total: ${self.calcular_total():.2f}")

# Uso
carrito = CarritoCompras()
carrito.agregar_item("Laptop", 999.99)
carrito.agregar_item("Mouse", 29.99)
carrito.agregar_item("Teclado", 59.99)

carrito.mostrar_items()
print()

# Procesar con diferentes estrategias de pago
estrategias = [
    PagoTarjetaCredito("1234-5678-9012-3456"),
    PagoPayPal("usuario@email.com"),
    PagoTransferencia("ES91-2100-0418-4502-0005-1332"),
    PagoCriptomoneda("1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa")
]

for estrategia in estrategias:
    carrito.establecer_estrategia_pago(estrategia)
    resultado = carrito.procesar_compra()
    print(resultado)
    print()
```

### **3. Command (Comando)**
Encapsula una solicitud como un objeto, permitiendo parametrizar clientes con diferentes solicitudes.

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
        print(f"Texto actual: '{self.texto}'")
    
    def borrar(self, caracteres: int):
        if caracteres <= len(self.texto):
            self.historial.append(self.texto)
            self.texto = self.texto[:-caracteres]
            print(f"Texto actual: '{self.texto}'")
        else:
            print("No hay suficientes caracteres para borrar")
    
    def deshacer_ultimo_cambio(self):
        if self.historial:
            self.texto = self.historial.pop()
            print(f"Deshecho. Texto actual: '{self.texto}'")
        else:
            print("No hay cambios para deshacer")
    
    def obtener_texto(self) -> str:
        return self.texto

class ComandoEscribir(Comando):
    def __init__(self, editor: EditorTexto, texto: str):
        self.editor = editor
        self.texto = texto
    
    def ejecutar(self):
        self.editor.escribir(self.texto)
    
    def deshacer(self):
        self.editor.deshacer_ultimo_cambio()

class ComandoBorrar(Comando):
    def __init__(self, editor: EditorTexto, caracteres: int):
        self.editor = editor
        self.caracteres = caracteres
    
    def ejecutar(self):
        self.editor.borrar(self.caracteres)
    
    def deshacer(self):
        self.editor.deshacer_ultimo_cambio()

class Invocador:
    def __init__(self):
        self.comandos: List[Comando] = []
        self.comandos_ejecutados: List[Comando] = []
    
    def ejecutar_comando(self, comando: Comando):
        comando.ejecutar()
        self.comandos_ejecutados.append(comando)
    
    def deshacer_ultimo_comando(self):
        if self.comandos_ejecutados:
            ultimo_comando = self.comandos_ejecutados.pop()
            ultimo_comando.deshacer()
    
    def ejecutar_macro(self, comandos: List[Comando]):
        for comando in comandos:
            self.ejecutar_comando(comando)

# Uso
editor = EditorTexto()
invocador = Invocador()

# Crear comandos
comando_hola = ComandoEscribir(editor, "Hola ")
comando_mundo = ComandoEscribir(editor, "Mundo!")
comando_borrar = ComandoBorrar(editor, 6)

# Ejecutar comandos individuales
print("=== Ejecutando comandos individuales ===")
invocador.ejecutar_comando(comando_hola)
invocador.ejecutar_comando(comando_mundo)
print()

# Ejecutar macro de comandos
print("=== Ejecutando macro de comandos ===")
macro = [ComandoEscribir(editor, "¡Hola de nuevo! "), ComandoEscribir(editor, "¿Cómo estás?")]
invocador.ejecutar_macro(macro)
print()

# Deshacer comandos
print("=== Deshaciendo comandos ===")
invocador.deshacer_ultimo_comando()
invocador.deshacer_ultimo_comando()
invocador.deshacer_ultimo_comando()
```

## 🔍 Aplicación Práctica: Sistema de Logging

### **Implementación con Múltiples Patrones**
```python
from abc import ABC, abstractmethod
from typing import List, Dict
from datetime import datetime
import json

# Strategy Pattern para diferentes formatos de logging
class FormatoLog(ABC):
    @abstractmethod
    def formatear(self, nivel: str, mensaje: str, timestamp: datetime) -> str:
        pass

class FormatoSimple(FormatoLog):
    def formatear(self, nivel: str, mensaje: str, timestamp: datetime) -> str:
        return f"[{nivel}] {mensaje}"

class FormatoDetallado(FormatoLog):
    def formatear(self, nivel: str, mensaje: str, timestamp: datetime) -> str:
        return f"[{timestamp.strftime('%Y-%m-%d %H:%M:%S')}] [{nivel}] {mensaje}"

class FormatoJSON(FormatoLog):
    def formatear(self, nivel: str, mensaje: str, timestamp: datetime) -> str:
        log_data = {
            "timestamp": timestamp.isoformat(),
            "level": nivel,
            "message": mensaje
        }
        return json.dumps(log_data)

# Observer Pattern para múltiples destinos de logging
class DestinoLog(ABC):
    @abstractmethod
    def escribir(self, mensaje: str):
        pass

class ConsolaLog(DestinoLog):
    def escribir(self, mensaje: str):
        print(f"CONSOLA: {mensaje}")

class ArchivoLog(DestinoLog):
    def __init__(self, nombre_archivo: str):
        self.nombre_archivo = nombre_archivo
    
    def escribir(self, mensaje: str):
        with open(self.nombre_archivo, 'a', encoding='utf-8') as archivo:
            archivo.write(mensaje + '\n')

class RedLog(DestinoLog):
    def __init__(self, endpoint: str):
        self.endpoint = endpoint
    
    def escribir(self, mensaje: str):
        print(f"RED ({self.endpoint}): {mensaje}")

# Singleton Pattern para el logger principal
class Logger:
    _instancia = None
    _inicializado = False
    
    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
        return cls._instancia
    
    def __init__(self):
        if not Logger._inicializado:
            self.formato = FormatoSimple()
            self.destinos: List[DestinoLog] = []
            self.nivel_minimo = "INFO"
            Logger._inicializado = True
    
    def establecer_formato(self, formato: FormatoLog):
        self.formato = formato
    
    def agregar_destino(self, destino: DestinoLog):
        if destino not in self.destinos:
            self.destinos.append(destino)
    
    def establecer_nivel(self, nivel: str):
        niveles = {"DEBUG": 0, "INFO": 1, "WARNING": 2, "ERROR": 3, "CRITICAL": 4}
        if nivel in niveles:
            self.nivel_minimo = nivel
    
    def _debe_loggear(self, nivel: str) -> bool:
        niveles = {"DEBUG": 0, "INFO": 1, "WARNING": 2, "ERROR": 3, "CRITICAL": 4}
        return niveles.get(nivel, 0) >= niveles.get(self.nivel_minimo, 0)
    
    def _log(self, nivel: str, mensaje: str):
        if not self._debe_loggear(nivel):
            return
        
        timestamp = datetime.now()
        mensaje_formateado = self.formato.formatear(nivel, mensaje, timestamp)
        
        for destino in self.destinos:
            destino.escribir(mensaje_formateado)
    
    def debug(self, mensaje: str):
        self._log("DEBUG", mensaje)
    
    def info(self, mensaje: str):
        self._log("INFO", mensaje)
    
    def warning(self, mensaje: str):
        self._log("WARNING", mensaje)
    
    def error(self, mensaje: str):
        self._log("ERROR", mensaje)
    
    def critical(self, mensaje: str):
        self._log("CRITICAL", mensaje)

# Factory Pattern para crear diferentes tipos de loggers
class LoggerFactory:
    @staticmethod
    def crear_logger_consola():
        logger = Logger()
        logger.agregar_destino(ConsolaLog())
        return logger
    
    @staticmethod
    def crear_logger_archivo(nombre_archivo: str):
        logger = Logger()
        logger.agregar_destino(ArchivoLog(nombre_archivo))
        return logger
    
    @staticmethod
    def crear_logger_completo(nombre_archivo: str, endpoint_red: str):
        logger = Logger()
        logger.agregar_destino(ConsolaLog())
        logger.agregar_destino(ArchivoLog(nombre_archivo))
        logger.agregar_destino(RedLog(endpoint_red))
        logger.establecer_formato(FormatoDetallado())
        return logger

# Uso del sistema
def main():
    # Crear diferentes tipos de loggers
    logger_consola = LoggerFactory.crear_logger_consola()
    logger_archivo = LoggerFactory.crear_logger_archivo("app.log")
    logger_completo = LoggerFactory.crear_logger_completo("app_detallado.log", "https://logs.api.com")
    
    # Configurar niveles
    logger_consola.establecer_nivel("DEBUG")
    logger_archivo.establecer_nivel("INFO")
    logger_completo.establecer_nivel("WARNING")
    
    # Usar loggers
    print("=== Logger de Consola ===")
    logger_consola.debug("Mensaje de debug")
    logger_consola.info("Mensaje de info")
    logger_consola.warning("Mensaje de warning")
    
    print("\n=== Logger de Archivo ===")
    logger_archivo.info("Mensaje guardado en archivo")
    logger_archivo.warning("Advertencia guardada en archivo")
    
    print("\n=== Logger Completo ===")
    logger_completo.warning("Advertencia enviada a todos los destinos")
    logger_completo.error("Error crítico enviado a todos los destinos")
    
    # Cambiar formato dinámicamente
    logger_consola.establecer_formato(FormatoJSON())
    logger_consola.info("Mensaje en formato JSON")

if __name__ == "__main__":
    main()
```

## 💡 Mejores Prácticas

### **1. Elegir el Patrón Adecuado**
- **Singleton**: Solo cuando realmente se necesita una instancia única
- **Factory**: Para crear objetos complejos o con lógica de creación
- **Observer**: Para comunicación entre componentes desacoplados
- **Strategy**: Para algoritmos intercambiables
- **Decorator**: Para añadir funcionalidad sin modificar clases existentes

### **2. No Sobre-Usar Patrones**
- Los patrones son herramientas, no objetivos
- Evaluar si realmente resuelven el problema
- Evitar la sobre-ingeniería

### **3. Combinar Patrones**
- Los patrones se complementan bien entre sí
- Crear soluciones más robustas y flexibles
- Mantener la coherencia del diseño

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Design Patterns" - Gang of Four
- "Head First Design Patterns" - Eric Freeman
- "Patterns of Enterprise Application Architecture" - Martin Fowler

### **Práctica Recomendada**
- Implementar un sistema de plugins
- Crear un framework de testing
- Diseñar una API REST con patrones

---

**Anterior**: [05. Principios SOLID](./05-principios-solid.md)  
**Siguiente**: [07. Programación Funcional](./07-programacion-funcional.md) 