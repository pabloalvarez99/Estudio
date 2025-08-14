# 08. Programación Reactiva

## 📖 ¿Qué es la Programación Reactiva?

La **programación reactiva** es un paradigma de programación que se enfoca en el manejo de flujos de datos asíncronos y la propagación automática de cambios. Se basa en el concepto de que los datos fluyen a través de un sistema y los componentes reaccionan automáticamente a esos cambios.

## 🎯 Conceptos Fundamentales

### **1. Streams (Flujos)**
Un **stream** es una secuencia de eventos que ocurren a lo largo del tiempo.

```python
# Concepto básico de stream
class Stream:
    def __init__(self):
        self.observadores = []
        self.valor_actual = None
    
    def suscribir(self, observador):
        self.observadores.append(observador)
        return self
    
    def emitir(self, valor):
        self.valor_actual = valor
        for observador in self.observadores:
            observador(valor)
    
    def obtener_valor(self):
        return self.valor_actual

# Uso básico
stream_temperatura = Stream()
stream_temperatura.suscribir(lambda temp: print(f"Temperatura actualizada: {temp}°C"))
stream_temperatura.emitir(25)  # Temperatura actualizada: 25°C
stream_temperatura.emitir(28)  # Temperatura actualizada: 28°C
```

### **2. Observables**
Un **observable** es un objeto que emite eventos y permite a los observadores suscribirse a ellos.

```python
from abc import ABC, abstractmethod
from typing import Callable, List

class Observable(ABC):
    @abstractmethod
    def suscribir(self, observador: Callable):
        pass
    
    @abstractmethod
    def desuscribir(self, observador: Callable):
        pass

class ObservableConcreto(Observable):
    def __init__(self):
        self.observadores: List[Callable] = []
        self.valor_actual = None
    
    def suscribir(self, observador: Callable):
        if observador not in self.observadores:
            self.observadores.append(observador)
        return self
    
    def desuscribir(self, observador: Callable):
        if observador in self.observadores:
            self.observadores.remove(observador)
    
    def notificar(self, valor):
        self.valor_actual = valor
        for observador in self.observadores:
            observador(valor)
    
    def obtener_valor(self):
        return self.valor_actual

# Uso
observable = ObservableConcreto()

def observador1(valor):
    print(f"Observador 1 recibió: {valor}")

def observador2(valor):
    print(f"Observador 2 recibió: {valor}")

observable.suscribir(observador1)
observable.suscribir(observador2)

observable.notificar("Hola Mundo")
# Observador 1 recibió: Hola Mundo
# Observador 2 recibió: Hola Mundo
```

### **3. Operadores**
Los **operadores** permiten transformar, filtrar y combinar streams.

```python
class StreamOperable:
    def __init__(self):
        self.observadores = []
        self.valor_actual = None
    
    def suscribir(self, observador):
        self.observadores.append(observador)
        return self
    
    def emitir(self, valor):
        self.valor_actual = valor
        for observador in self.observadores:
            observador(valor)
    
    def map(self, transformacion):
        """Operador map: transforma cada valor"""
        nuevo_stream = StreamOperable()
        
        def observador_transformado(valor):
            resultado = transformacion(valor)
            nuevo_stream.emitir(resultado)
        
        self.suscribir(observador_transformado)
        return nuevo_stream
    
    def filter(self, predicado):
        """Operador filter: filtra valores según una condición"""
        nuevo_stream = StreamOperable()
        
        def observador_filtrado(valor):
            if predicado(valor):
                nuevo_stream.emitir(valor)
        
        self.suscribir(observador_filtrado)
        return nuevo_stream
    
    def debounce(self, tiempo_ms):
        """Operador debounce: emite solo después de un período de inactividad"""
        import time
        nuevo_stream = StreamOperable()
        ultima_emision = 0
        
        def observador_debounced(valor):
            nonlocal ultima_emision
            tiempo_actual = time.time() * 1000
            
            if tiempo_actual - ultima_emision >= tiempo_ms:
                nuevo_stream.emitir(valor)
                ultima_emision = tiempo_actual
        
        self.suscribir(observador_debounced)
        return nuevo_stream

# Uso de operadores
stream_numeros = StreamOperable()

# Crear pipeline de operadores
stream_transformado = (stream_numeros
    .map(lambda x: x * 2)           # Duplicar cada número
    .filter(lambda x: x > 5)         # Solo números mayores que 5
    .debounce(1000))                 # Esperar 1 segundo entre emisiones

# Suscribirse al stream transformado
stream_transformado.suscribir(lambda x: print(f"Valor procesado: {x}"))

# Emitir valores
stream_numeros.emitir(3)  # 3 * 2 = 6 > 5, se emite después de 1 segundo
stream_numeros.emitir(4)  # 4 * 2 = 8 > 5, se emite después de 1 segundo
stream_numeros.emitir(1)  # 1 * 2 = 2 <= 5, no se emite
```

## 🔄 Programación Asíncrona

### **1. Async/Await en Python**
```python
import asyncio
import aiohttp
from typing import List

class ServicioDatos:
    def __init__(self):
        self.datos_cache = {}
    
    async def obtener_datos(self, id: str) -> dict:
        """Obtiene datos de forma asíncrona"""
        if id in self.datos_cache:
            return self.datos_cache[id]
        
        # Simular llamada HTTP asíncrona
        await asyncio.sleep(0.1)
        datos = {"id": id, "valor": f"dato_{id}", "timestamp": asyncio.get_event_loop().time()}
        self.datos_cache[id] = datos
        return datos
    
    async def obtener_multiples_datos(self, ids: List[str]) -> List[dict]:
        """Obtiene múltiples datos en paralelo"""
        tareas = [self.obtener_datos(id) for id in ids]
        return await asyncio.gather(*tareas)

# Uso
async def main():
    servicio = ServicioDatos()
    
    # Obtener un dato
    dato = await servicio.obtener_datos("user_123")
    print(f"Dato obtenido: {dato}")
    
    # Obtener múltiples datos en paralelo
    ids = ["user_1", "user_2", "user_3", "user_4", "user_5"]
    datos = await servicio.obtener_multiples_datos(ids)
    print(f"Datos obtenidos: {len(datos)}")

# Ejecutar
asyncio.run(main())
```

### **2. Event Loop y Corrutinas**
```python
import asyncio
from typing import Callable

class EventEmitter:
    def __init__(self):
        self.eventos = {}
        self.loop = asyncio.get_event_loop()
    
    def on(self, evento: str, callback: Callable):
        """Registra un callback para un evento"""
        if evento not in self.eventos:
            self.eventos[evento] = []
        self.eventos[evento].append(callback)
    
    def emit(self, evento: str, *args, **kwargs):
        """Emite un evento y ejecuta todos los callbacks registrados"""
        if evento in self.eventos:
            for callback in self.eventos[evento]:
                # Ejecutar callback de forma asíncrona
                asyncio.create_task(self._ejecutar_callback(callback, *args, **kwargs))
    
    async def _ejecutar_callback(self, callback: Callable, *args, **kwargs):
        """Ejecuta un callback de forma asíncrona"""
        if asyncio.iscoroutinefunction(callback):
            await callback(*args, **kwargs)
        else:
            # Si no es corrutina, ejecutar en thread pool
            await self.loop.run_in_executor(None, callback, *args, **kwargs)

# Uso
async def callback_asincrono(mensaje):
    print(f"Callback asíncrono: {mensaje}")
    await asyncio.sleep(0.1)
    print(f"Callback asíncrono completado: {mensaje}")

def callback_sincrono(mensaje):
    print(f"Callback síncrono: {mensaje}")

async def main():
    emitter = EventEmitter()
    
    # Registrar callbacks
    emitter.on("mensaje", callback_asincrono)
    emitter.on("mensaje", callback_sincrono)
    
    # Emitir eventos
    emitter.emit("mensaje", "¡Hola Mundo!")
    
    # Esperar a que se completen los callbacks
    await asyncio.sleep(0.2)

asyncio.run(main())
```

## 🎭 Patrones Reactivos

### **1. Observer Pattern**
```python
from abc import ABC, abstractmethod
from typing import List, Dict, Any

class Sujeto(ABC):
    @abstractmethod
    def suscribir(self, observador, evento: str = None):
        pass
    
    @abstractmethod
    def desuscribir(self, observador, evento: str = None):
        pass
    
    @abstractmethod
    def notificar(self, evento: str, datos: Any = None):
        pass

class SujetoConcreto(Sujeto):
    def __init__(self):
        self.observadores: Dict[str, List] = {}
        self.observadores_generales: List = []
    
    def suscribir(self, observador, evento: str = None):
        if evento:
            if evento not in self.observadores:
                self.observadores[evento] = []
            self.observadores[evento].append(observador)
        else:
            self.observadores_generales.append(observador)
    
    def desuscribir(self, observador, evento: str = None):
        if evento and evento in self.observadores:
            if observador in self.observadores[evento]:
                self.observadores[evento].remove(observador)
        else:
            if observador in self.observadores_generales:
                self.observadores_generales.remove(observador)
    
    def notificar(self, evento: str, datos: Any = None):
        # Notificar observadores específicos del evento
        if evento in self.observadores:
            for observador in self.observadores[evento]:
                observador.actualizar(evento, datos)
        
        # Notificar observadores generales
        for observador in self.observadores_generales:
            observador.actualizar(evento, datos)

class Observador(ABC):
    @abstractmethod
    def actualizar(self, evento: str, datos: Any):
        pass

class ObservadorConcreto(Observador):
    def __init__(self, nombre: str):
        self.nombre = nombre
    
    def actualizar(self, evento: str, datos: Any):
        print(f"[{self.nombre}] Evento '{evento}' recibido con datos: {datos}")

# Uso
sujeto = SujetoConcreto()

# Crear observadores
observador1 = ObservadorConcreto("Observador 1")
observador2 = ObservadorConcreto("Observador 2")
observador3 = ObservadorConcreto("Observador General")

# Suscribir observadores
sujeto.suscribir(observador1, "usuario_creado")
sujeto.suscribir(observador2, "usuario_eliminado")
sujeto.suscribir(observador3)  # Observador general

# Emitir eventos
sujeto.notificar("usuario_creado", {"id": 123, "nombre": "Ana"})
sujeto.notificar("usuario_eliminado", {"id": 456, "nombre": "Bob"})
sujeto.notificar("evento_desconocido", {"mensaje": "Evento sin observadores específicos"})
```

### **2. Publisher/Subscriber Pattern**
```python
from typing import Dict, List, Callable, Any
import threading
import queue
import time

class MessageBroker:
    def __init__(self):
        self.topics: Dict[str, List[Callable]] = {}
        self.message_queue = queue.Queue()
        self.running = True
        
        # Iniciar worker thread
        self.worker_thread = threading.Thread(target=self._process_messages)
        self.worker_thread.start()
    
    def subscribe(self, topic: str, callback: Callable):
        """Suscribe un callback a un topic"""
        if topic not in self.topics:
            self.topics[topic] = []
        self.topics[topic].append(callback)
    
    def unsubscribe(self, topic: str, callback: Callable):
        """Desuscribe un callback de un topic"""
        if topic in self.topics and callback in self.topics[topic]:
            self.topics[topic].remove(callback)
    
    def publish(self, topic: str, message: Any):
        """Publica un mensaje en un topic"""
        self.message_queue.put((topic, message))
    
    def _process_messages(self):
        """Procesa mensajes en background"""
        while self.running:
            try:
                topic, message = self.message_queue.get(timeout=1)
                if topic in self.topics:
                    for callback in self.topics[topic]:
                        try:
                            callback(message)
                        except Exception as e:
                            print(f"Error en callback: {e}")
                self.message_queue.task_done()
            except queue.Empty:
                continue
    
    def stop(self):
        """Detiene el broker"""
        self.running = False
        self.worker_thread.join()

# Uso
def callback_usuario(mensaje):
    print(f"Callback usuario: {mensaje}")

def callback_sistema(mensaje):
    print(f"Callback sistema: {mensaje}")

def callback_general(mensaje):
    print(f"Callback general: {mensaje}")

# Crear broker
broker = MessageBroker()

# Suscribir callbacks
broker.subscribe("usuarios", callback_usuario)
broker.subscribe("sistema", callback_sistema)
broker.subscribe("usuarios", callback_general)  # Múltiples callbacks para el mismo topic

# Publicar mensajes
broker.publish("usuarios", {"accion": "crear", "id": 123})
broker.publish("sistema", {"estado": "activo", "timestamp": time.time()})

# Esperar a que se procesen los mensajes
time.sleep(0.1)

# Detener broker
broker.stop()
```

## 🚀 Aplicación Práctica: Sistema de Monitoreo en Tiempo Real

### **Implementación Completa**
```python
import asyncio
import time
import random
from typing import Dict, List, Callable, Any
from dataclasses import dataclass
from abc import ABC, abstractmethod

@dataclass
class MetricData:
    timestamp: float
    value: float
    metric_name: str
    tags: Dict[str, str]

class MetricObserver(ABC):
    @abstractmethod
    def on_metric_received(self, metric: MetricData):
        pass

class MetricStream:
    def __init__(self):
        self.observers: List[MetricObserver] = []
        self.filters: List[Callable[[MetricData], bool]] = []
        self.transformers: List[Callable[[MetricData], MetricData]] = []
    
    def subscribe(self, observer: MetricObserver):
        self.observers.append(observer)
    
    def unsubscribe(self, observer: MetricObserver):
        if observer in self.observers:
            self.observers.remove(observer)
    
    def add_filter(self, filter_func: Callable[[MetricData], bool]):
        self.filters.append(filter_func)
    
    def add_transformer(self, transform_func: Callable[[MetricData], MetricData]):
        self.transformers.append(transform_func)
    
    def emit(self, metric: MetricData):
        # Aplicar filtros
        for filter_func in self.filters:
            if not filter_func(metric):
                return  # Métrica filtrada
        
        # Aplicar transformadores
        transformed_metric = metric
        for transform_func in self.transformers:
            transformed_metric = transform_func(transformed_metric)
        
        # Notificar observadores
        for observer in self.observers:
            observer.on_metric_received(transformed_metric)

class MetricCollector:
    def __init__(self):
        self.streams: Dict[str, MetricStream] = {}
    
    def create_stream(self, name: str) -> MetricStream:
        if name not in self.streams:
            self.streams[name] = MetricStream()
        return self.streams[name]
    
    def get_stream(self, name: str) -> MetricStream:
        return self.streams.get(name)
    
    def emit_metric(self, stream_name: str, metric: MetricData):
        if stream_name in self.streams:
            self.streams[stream_name].emit(metric)

class ConsoleObserver(MetricObserver):
    def __init__(self, name: str):
        self.name = name
    
    def on_metric_received(self, metric: MetricData):
        print(f"[{self.name}] {metric.metric_name}: {metric.value} "
              f"({time.strftime('%H:%M:%S', time.localtime(metric.timestamp))})")

class AlertObserver(MetricObserver):
    def __init__(self, threshold: float):
        self.threshold = threshold
        self.alert_count = 0
    
    def on_metric_received(self, metric: MetricData):
        if metric.value > self.threshold:
            self.alert_count += 1
            print(f"🚨 ALERTA: {metric.metric_name} = {metric.value} "
                  f"(umbral: {self.threshold}) - Alerta #{self.alert_count}")

class AggregatorObserver(MetricObserver):
    def __init__(self, window_size: int = 10):
        self.window_size = window_size
        self.metrics_buffer: List[MetricData] = []
    
    def on_metric_received(self, metric: MetricData):
        self.metrics_buffer.append(metric)
        
        if len(self.metrics_buffer) >= self.window_size:
            self._calculate_aggregates()
            self.metrics_buffer.clear()
    
    def _calculate_aggregates(self):
        if not self.metrics_buffer:
            return
        
        values = [m.value for m in self.metrics_buffer]
        avg_value = sum(values) / len(values)
        max_value = max(values)
        min_value = min(values)
        
        print(f"📊 AGREGADOS: Promedio={avg_value:.2f}, "
              f"Máximo={max_value:.2f}, Mínimo={min_value:.2f}")

# Funciones de filtrado y transformación
def filter_high_values(metric: MetricData) -> bool:
    return metric.value < 1000

def filter_by_tag(metric: MetricData, tag_key: str, tag_value: str) -> bool:
    return metric.tags.get(tag_key) == tag_value

def transform_to_percentage(metric: MetricData) -> MetricData:
    if metric.value <= 100:
        return MetricData(
            timestamp=metric.timestamp,
            value=metric.value,
            metric_name=f"{metric.metric_name}_percentage",
            tags=metric.tags
        )
    return metric

def transform_normalize(metric: MetricData, max_value: float = 100) -> MetricData:
    normalized_value = (metric.value / max_value) * 100
    return MetricData(
        timestamp=metric.timestamp,
        value=normalized_value,
        metric_name=f"{metric.metric_name}_normalized",
        tags=metric.tags
    )

# Simulador de métricas
class MetricSimulator:
    def __init__(self, collector: MetricCollector):
        self.collector = collector
        self.running = False
    
    async def start(self):
        self.running = True
        while self.running:
            # Simular métricas de CPU
            cpu_usage = random.uniform(0, 100)
            cpu_metric = MetricData(
                timestamp=time.time(),
                value=cpu_usage,
                metric_name="cpu_usage",
                tags={"host": "server-01", "type": "system"}
            )
            self.collector.emit_metric("cpu", cpu_metric)
            
            # Simular métricas de memoria
            memory_usage = random.uniform(0, 1000)
            memory_metric = MetricData(
                timestamp=time.time(),
                value=memory_usage,
                metric_name="memory_usage",
                tags={"host": "server-01", "type": "system"}
            )
            self.collector.emit_metric("memory", memory_metric)
            
            # Simular métricas de red
            network_traffic = random.uniform(0, 500)
            network_metric = MetricData(
                timestamp=time.time(),
                value=network_traffic,
                metric_name="network_traffic",
                tags={"host": "server-01", "type": "network"}
            )
            self.collector.emit_metric("network", network_metric)
            
            await asyncio.sleep(1)  # Emitir cada segundo
    
    def stop(self):
        self.running = False

# Configuración del sistema
async def main():
    # Crear colector de métricas
    collector = MetricCollector()
    
    # Crear streams para diferentes tipos de métricas
    cpu_stream = collector.create_stream("cpu")
    memory_stream = collector.create_stream("memory")
    network_stream = collector.create_stream("network")
    
    # Configurar filtros y transformadores
    cpu_stream.add_filter(filter_high_values)
    cpu_stream.add_transformer(lambda m: transform_normalize(m, 100))
    
    memory_stream.add_filter(lambda m: filter_by_tag(m, "type", "system"))
    memory_stream.add_transformer(lambda m: transform_to_percentage(m))
    
    # Crear observadores
    console_observer = ConsoleObserver("Consola")
    alert_observer = AlertObserver(threshold=80)
    aggregator_observer = AggregatorObserver(window_size=5)
    
    # Suscribir observadores
    cpu_stream.subscribe(console_observer)
    cpu_stream.subscribe(alert_observer)
    cpu_stream.subscribe(aggregator_observer)
    
    memory_stream.subscribe(console_observer)
    network_stream.subscribe(console_observer)
    
    # Iniciar simulador
    simulator = MetricSimulator(collector)
    simulator_task = asyncio.create_task(simulator.start())
    
    # Ejecutar por 10 segundos
    await asyncio.sleep(10)
    
    # Detener simulador
    simulator.stop()
    await simulator_task

# Ejecutar sistema
if __name__ == "__main__":
    asyncio.run(main())
```

## 🔍 Beneficios de la Programación Reactiva

### **1. Manejo Eficiente de Eventos**
- Respuesta automática a cambios
- Propagación eficiente de datos
- Manejo de múltiples fuentes de eventos

### **2. Escalabilidad**
- Fácil agregar nuevos observadores
- Composición de streams
- Procesamiento paralelo natural

### **3. Desacoplamiento**
- Componentes independientes
- Fácil testing y mantenimiento
- Arquitectura modular

## ⚠️ Consideraciones y Desafíos

### **1. Complejidad**
- Curva de aprendizaje pronunciada
- Debugging de streams complejos
- Manejo de errores en pipelines

### **2. Memory Management**
- Posibles memory leaks
- Gestión de suscripciones
- Cleanup de recursos

### **3. Testing**
- Testing de flujos asíncronos
- Mocking de streams
- Verificación de comportamiento reactivo

## 💡 Mejores Prácticas

### **1. Diseño de Streams**
```python
# Mantener streams simples y enfocados
# Usar nombres descriptivos para streams
# Documentar el flujo de datos
```

### **2. Manejo de Errores**
```python
# Implementar error handling en streams
# Usar timeouts para operaciones asíncronas
# Logging de errores para debugging
```

### **3. Performance**
```python
# Usar backpressure para streams rápidos
# Implementar caching cuando sea apropiado
# Monitorear uso de memoria
```

## 📚 Recursos para Aprender Más

### **Librerías Recomendadas**
- **Python**: RxPY, asyncio, trio
- **JavaScript**: RxJS, Bacon.js
- **Java**: RxJava, Project Reactor

### **Libros Recomendados**
- "Reactive Programming with RxJava" - Tomasz Nurkiewicz
- "Reactive Design Patterns" - Roland Kuhn
- "Programming Reactive Systems" - Konrad Malawski

### **Práctica Recomendada**
- Implementar un sistema de chat en tiempo real
- Crear un dashboard de métricas reactivo
- Desarrollar un sistema de notificaciones push

---

**Anterior**: [07. Programación Funcional](./07-programacion-funcional.md)  
**Siguiente**: [Ejercicios de Paradigmas](./../02-EJERCICIOS/01-ejercicios-clases.md) 