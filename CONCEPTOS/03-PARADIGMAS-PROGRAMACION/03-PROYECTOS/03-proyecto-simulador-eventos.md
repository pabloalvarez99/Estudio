# 03. Proyecto: Simulador de Eventos con Paradigmas de Programación

## 🎯 Descripción del Proyecto

Implementar un simulador de eventos que demuestre el uso de **POO**, **Programación Funcional**, **Patrones de Diseño** y **Programación Reactiva** para manejar eventos, notificaciones y flujos de datos en tiempo real.

## 🏗️ Arquitectura del Sistema

### **Estructura de Clases**
```
SimuladorEventos/
├── Core/
│   ├── Eventos (POO)
│   ├── Simuladores (Funcional)
│   └── Observadores (Reactivo)
├── Patrones/
│   ├── Strategy (Simulación)
│   ├── Observer (Notificaciones)
│   └── Factory (Creación)
└── Utilidades/
    └── Helpers (Funcional)
```

## 🔧 Implementación

### **1. Entidades del Dominio (POO)**

```python
from dataclasses import dataclass
from datetime import datetime, timedelta
from typing import List, Dict, Optional, Any, Callable
from enum import Enum
import uuid
import random

class TipoEvento(Enum):
    DEPORTIVO = "deportivo"
    MUSICAL = "musical"
    CULTURAL = "cultural"
    TECNOLOGICO = "tecnologico"
    ACADEMICO = "academico"

class EstadoEvento(Enum):
    PLANIFICADO = "planificado"
    EN_CURSO = "en_curso"
    COMPLETADO = "completado"
    CANCELADO = "cancelado"
    POSPUESTO = "pospuesto"

@dataclass
class Evento:
    id: str
    nombre: str
    tipo: TipoEvento
    descripcion: str
    fecha_inicio: datetime
    fecha_fin: datetime
    ubicacion: str
    capacidad_maxima: int
    precio: float
    estado: EstadoEvento
    organizador: str
    participantes: List[str]
    recursos_necesarios: List[str]
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.participantes:
            self.participantes = []
        if not self.recursos_necesarios:
            self.recursos_necesarios = []
    
    def esta_activo(self) -> bool:
        ahora = datetime.now()
        return self.fecha_inicio <= ahora <= self.fecha_fin
    
    def esta_lleno(self) -> bool:
        return len(self.participantes) >= self.capacidad_maxima
    
    def agregar_participante(self, participante_id: str) -> bool:
        if not self.esta_lleno() and participante_id not in self.participantes:
            self.participantes.append(participante_id)
            return True
        return False
    
    def remover_participante(self, participante_id: str) -> bool:
        if participante_id in self.participantes:
            self.participantes.remove(participante_id)
            return True
        return False
    
    def obtener_ocupacion_porcentual(self) -> float:
        return (len(self.participantes) / self.capacidad_maxima) * 100
    
    def obtener_duracion(self) -> timedelta:
        return self.fecha_fin - self.fecha_inicio
    
    def obtener_info(self) -> str:
        return (f"Evento: {self.nombre} ({self.tipo.value})\n"
                f"Fecha: {self.fecha_inicio.strftime('%Y-%m-%d %H:%M')} - "
                f"{self.fecha_fin.strftime('%Y-%m-%d %H:%M')}\n"
                f"Ubicación: {self.ubicacion}\n"
                f"Estado: {self.estado.value}\n"
                f"Participantes: {len(self.participantes)}/{self.capacidad_maxima} "
                f"({self.obtener_ocupacion_porcentual():.1f}%)")

@dataclass
class Participante:
    id: str
    nombre: str
    email: str
    telefono: str
    fecha_registro: datetime
    eventos_inscritos: List[str]
    preferencias: List[TipoEvento]
    activo: bool = True
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha_registro:
            self.fecha_registro = datetime.now()
        if not self.eventos_inscritos:
            self.eventos_inscritos = []
        if not self.preferencias:
            self.preferencias = []
    
    def inscribirse_evento(self, evento_id: str) -> bool:
        if evento_id not in self.eventos_inscritos:
            self.eventos_inscritos.append(evento_id)
            return True
        return False
    
    def cancelar_evento(self, evento_id: str) -> bool:
        if evento_id in self.eventos_inscritos:
            self.eventos_inscritos.remove(evento_id)
            return True
        return False
    
    def agregar_preferencia(self, tipo_evento: TipoEvento):
        if tipo_evento not in self.preferencias:
            self.preferencias.append(tipo_evento)
    
    def obtener_info(self) -> str:
        return (f"Participante: {self.nombre} ({self.email})\n"
                f"Eventos inscritos: {len(self.eventos_inscritos)}\n"
                f"Preferencias: {[p.value for p in self.preferencias]}")

@dataclass
class Recurso:
    id: str
    nombre: str
    tipo: str
    descripcion: str
    disponible: bool
    costo_por_hora: float
    eventos_asignados: List[str]
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.eventos_asignados:
            self.eventos_asignados = []
    
    def asignar_a_evento(self, evento_id: str) -> bool:
        if self.disponible and evento_id not in self.eventos_asignados:
            self.eventos_asignados.append(evento_id)
            return True
        return False
    
    def liberar_de_evento(self, evento_id: str) -> bool:
        if evento_id in self.eventos_asignados:
            self.eventos_asignados.remove(evento_id)
            return True
        return False
    
    def obtener_info(self) -> str:
        return (f"Recurso: {self.nombre} ({self.tipo})\n"
                f"Disponible: {'Sí' if self.disponible else 'No'}\n"
                f"Costo: ${self.costo_por_hora}/hora\n"
                f"Eventos asignados: {len(self.eventos_asignados)}")
```

### **2. Interfaces y Abstracciones (Principios SOLID)**

```python
from abc import ABC, abstractmethod
from typing import Protocol, List, Dict, Any

class SimuladorEvento(ABC):
    @abstractmethod
    def simular(self, evento: Evento, parametros: Dict[str, Any]) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def obtener_nombre(self) -> str:
        pass

class GeneradorEventos(ABC):
    @abstractmethod
    def generar_evento(self, tipo: TipoEvento, fecha: datetime) -> Evento:
        pass
    
    @abstractmethod
    def generar_eventos_aleatorios(self, cantidad: int, fecha_inicio: datetime, 
                                 fecha_fin: datetime) -> List[Evento]:
        pass

class ObservadorEvento(ABC):
    @abstractmethod
    def notificar_evento(self, evento: Evento, tipo_notificacion: str):
        pass

class RepositorioEventos(ABC):
    @abstractmethod
    def guardar(self, evento: Evento) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_id(self, id: str) -> Optional[Evento]:
        pass
    
    @abstractmethod
    def obtener_por_fecha(self, fecha_inicio: datetime, fecha_fin: datetime) -> List[Evento]:
        pass
    
    @abstractmethod
    def obtener_por_tipo(self, tipo: TipoEvento) -> List[Evento]:
        pass

class ServicioNotificacion(ABC):
    @abstractmethod
    def enviar_notificacion(self, destinatario: str, mensaje: str, tipo: str):
        pass
```

### **3. Implementaciones de Simuladores (Strategy Pattern)**

```python
class SimuladorDeportivo(SimuladorEvento):
    def __init__(self):
        self.factores_clima = {
            'soleado': 1.2,
            'nublado': 1.0,
            'lluvia': 0.7,
            'tormenta': 0.3
        }
    
    def simular(self, evento: Evento, parametros: Dict[str, Any]) -> Dict[str, Any]:
        clima = parametros.get('clima', 'soleado')
        factor_clima = self.factores_clima.get(clima, 1.0)
        
        # Simulación de asistencia basada en clima y otros factores
        asistencia_base = min(evento.capacidad_maxima, 
                            int(evento.capacidad_maxima * 0.8))
        
        asistencia_final = int(asistencia_base * factor_clima)
        
        # Calcular ingresos
        ingresos = asistencia_final * evento.precio
        
        # Calcular costos (recursos, personal, etc.)
        costos = self._calcular_costos(evento, asistencia_final)
        
        # Calcular ganancia
        ganancia = ingresos - costos
        
        return {
            'tipo_simulacion': 'deportivo',
            'clima': clima,
            'factor_clima': factor_clima,
            'asistencia_esperada': asistencia_final,
            'asistencia_porcentual': (asistencia_final / evento.capacidad_maxima) * 100,
            'ingresos': ingresos,
            'costos': costos,
            'ganancia': ganancia,
            'rentabilidad': (ganancia / ingresos) * 100 if ingresos > 0 else 0
        }
    
    def _calcular_costos(self, evento: Evento, asistencia: int) -> float:
        # Costos base del evento
        costos_base = 1000  # Costos fijos
        
        # Costos por participante
        costos_por_participante = 5
        
        # Costos de recursos
        costos_recursos = len(evento.recursos_necesarios) * 200
        
        return costos_base + (costos_por_participante * asistencia) + costos_recursos
    
    def obtener_nombre(self) -> str:
        return "Simulador Deportivo"

class SimuladorMusical(SimuladorEvento):
    def __init__(self):
        self.factores_popularidad = {
            'baja': 0.6,
            'media': 1.0,
            'alta': 1.5,
            'muy_alta': 2.0
        }
    
    def simular(self, evento: Evento, parametros: Dict[str, Any]) -> Dict[str, Any]:
        popularidad = parametros.get('popularidad', 'media')
        factor_popularidad = self.factores_popularidad.get(popularidad, 1.0)
        
        # Simulación de asistencia basada en popularidad
        asistencia_base = min(evento.capacidad_maxima, 
                            int(evento.capacidad_maxima * 0.9))
        
        asistencia_final = int(asistencia_base * factor_popularidad)
        
        # Calcular ingresos
        ingresos = asistencia_final * evento.precio
        
        # Calcular costos
        costos = self._calcular_costos(evento, asistencia_final)
        
        # Calcular ganancia
        ganancia = ingresos - costos
        
        return {
            'tipo_simulacion': 'musical',
            'popularidad': popularidad,
            'factor_popularidad': factor_popularidad,
            'asistencia_esperada': asistencia_final,
            'asistencia_porcentual': (asistencia_final / evento.capacidad_maxima) * 100,
            'ingresos': ingresos,
            'costos': costos,
            'ganancia': ganancia,
            'rentabilidad': (ganancia / ingresos) * 100 if ingresos > 0 else 0
        }
    
    def _calcular_costos(self, evento: Evento, asistencia: int) -> float:
        # Costos base del evento
        costos_base = 2000  # Costos fijos más altos para eventos musicales
        
        # Costos por participante
        costos_por_participante = 8
        
        # Costos de equipos de sonido
        costos_equipos = 1500
        
        # Costos de recursos
        costos_recursos = len(evento.recursos_necesarios) * 300
        
        return costos_base + (costos_por_participante * asistencia) + costos_equipos + costos_recursos
    
    def obtener_nombre(self) -> str:
        return "Simulador Musical"

class SimuladorTecnologico(SimuladorEvento):
    def __init__(self):
        self.factores_innovacion = {
            'baja': 0.8,
            'media': 1.0,
            'alta': 1.3,
            'muy_alta': 1.6
        }
    
    def simular(self, evento: Evento, parametros: Dict[str, Any]) -> Dict[str, Any]:
        nivel_innovacion = parametros.get('nivel_innovacion', 'media')
        factor_innovacion = self.factores_innovacion.get(nivel_innovacion, 1.0)
        
        # Simulación de asistencia basada en innovación
        asistencia_base = min(evento.capacidad_maxima, 
                            int(evento.capacidad_maxima * 0.7))
        
        asistencia_final = int(asistencia_base * factor_innovacion)
        
        # Calcular ingresos
        ingresos = asistencia_final * evento.precio
        
        # Calcular costos
        costos = self._calcular_costos(evento, asistencia_final)
        
        # Calcular ganancia
        ganancia = ingresos - costos
        
        return {
            'tipo_simulacion': 'tecnologico',
            'nivel_innovacion': nivel_innovacion,
            'factor_innovacion': factor_innovacion,
            'asistencia_esperada': asistencia_final,
            'asistencia_porcentual': (asistencia_final / evento.capacidad_maxima) * 100,
            'ingresos': ingresos,
            'costos': costos,
            'ganancia': ganancia,
            'rentabilidad': (ganancia / ingresos) * 100 if ingresos > 0 else 0
        }
    
    def _calcular_costos(self, evento: Evento, asistencia: int) -> float:
        # Costos base del evento
        costos_base = 1500
        
        # Costos por participante
        costos_por_participante = 6
        
        # Costos de tecnología
        costos_tecnologia = 1000
        
        # Costos de recursos
        costos_recursos = len(evento.recursos_necesarios) * 250
        
        return costos_base + (costos_por_participante * asistencia) + costos_tecnologia + costos_recursos
    
    def obtener_nombre(self) -> str:
        return "Simulador Tecnológico"
```

### **4. Generador de Eventos (Factory Pattern)**

```python
class GeneradorEventosAleatorio(GeneradorEventos):
    def __init__(self):
        self.nombres_deportivos = [
            "Torneo de Fútbol", "Maratón Urbana", "Copa de Tenis", "Liga de Baloncesto",
            "Campeonato de Natación", "Carrera de Bicicletas", "Torneo de Voleibol"
        ]
        
        self.nombres_musicales = [
            "Festival de Rock", "Concierto Clásico", "Show de Jazz", "Festival Electrónico",
            "Concierto Pop", "Recital de Ópera", "Festival Folk"
        ]
        
        self.nombres_tecnologicos = [
            "Conferencia de IA", "Hackathon", "Workshop de Blockchain", "Summit de Cloud",
            "Meetup de Desarrollo", "Expo de Startups", "Conferencia de Ciberseguridad"
        ]
        
        self.ubicaciones = [
            "Centro Deportivo Municipal", "Estadio Olímpico", "Auditorio Principal",
            "Centro de Convenciones", "Parque Tecnológico", "Universidad", "Teatro Municipal"
        ]
        
        self.organizadores = [
            "Deportes Unidos", "Música Viva", "TechCorp", "Cultura Local", "Academia Nacional"
        ]
    
    def generar_evento(self, tipo: TipoEvento, fecha: datetime) -> Evento:
        if tipo == TipoEvento.DEPORTIVO:
            nombre = random.choice(self.nombres_deportivos)
            duracion = timedelta(hours=random.randint(2, 6))
            precio = random.uniform(20, 100)
            capacidad = random.randint(100, 5000)
        
        elif tipo == TipoEvento.MUSICAL:
            nombre = random.choice(self.nombres_musicales)
            duracion = timedelta(hours=random.randint(1, 4))
            precio = random.uniform(30, 150)
            capacidad = random.randint(200, 10000)
        
        elif tipo == TipoEvento.TECNOLOGICO:
            nombre = random.choice(self.nombres_tecnologicos)
            duracion = timedelta(hours=random.randint(3, 8))
            precio = random.uniform(50, 300)
            capacidad = random.randint(50, 1000)
        
        else:
            nombre = f"Evento {tipo.value.title()}"
            duracion = timedelta(hours=random.randint(2, 5))
            precio = random.uniform(25, 100)
            capacidad = random.randint(100, 2000)
        
        fecha_fin = fecha + duracion
        
        return Evento(
            id="",
            nombre=nombre,
            tipo=tipo,
            descripcion=f"Descripción del evento {nombre}",
            fecha_inicio=fecha,
            fecha_fin=fecha_fin,
            ubicacion=random.choice(self.ubicaciones),
            capacidad_maxima=capacidad,
            precio=precio,
            estado=EstadoEvento.PLANIFICADO,
            organizador=random.choice(self.organizadores),
            participantes=[],
            recursos_necesarios=self._generar_recursos(tipo)
        )
    
    def _generar_recursos(self, tipo: TipoEvento) -> List[str]:
        recursos_base = ["Personal de seguridad", "Sistema de sonido", "Iluminación"]
        
        if tipo == TipoEvento.DEPORTIVO:
            recursos_adicionales = ["Equipos deportivos", "Árbitros", "Médicos", "Ambulancia"]
        elif tipo == TipoEvento.MUSICAL:
            recursos_adicionales = ["Instrumentos", "Técnicos de sonido", "Backline", "Dressing rooms"]
        elif tipo == TipoEvento.TECNOLOGICO:
            recursos_adicionales = ["Proyectores", "WiFi", "Computadoras", "Cables de red"]
        else:
            recursos_adicionales = ["Sillas", "Mesas", "Material de presentación"]
        
        return recursos_base + recursos_adicionales
    
    def generar_eventos_aleatorios(self, cantidad: int, fecha_inicio: datetime, 
                                 fecha_fin: datetime) -> List[Evento]:
        eventos = []
        tipos_evento = list(TipoEvento)
        
        for _ in range(cantidad):
            # Generar fecha aleatoria dentro del rango
            dias_aleatorios = random.randint(0, (fecha_fin - fecha_inicio).days)
            fecha_aleatoria = fecha_inicio + timedelta(days=dias_aleatorios)
            
            # Generar hora aleatoria
            hora_aleatoria = random.randint(9, 18)  # Entre 9 AM y 6 PM
            fecha_aleatoria = fecha_aleatoria.replace(hour=hora_aleatoria, minute=0, second=0)
            
            # Seleccionar tipo aleatorio
            tipo_aleatorio = random.choice(tipos_evento)
            
            evento = self.generar_evento(tipo_aleatorio, fecha_aleatoria)
            eventos.append(evento)
        
        return eventos
```

### **5. Sistema de Observadores (Observer Pattern + Reactivo)**

```python
class GestorObservadoresEventos:
    def __init__(self):
        self.observadores: List[ObservadorEvento] = []
    
    def agregar_observador(self, observador: ObservadorEvento):
        if observador not in self.observadores:
            self.observadores.append(observador)
    
    def remover_observador(self, observador: ObservadorEvento):
        if observador in self.observadores:
            self.observadores.remove(observador)
    
    def notificar_todos(self, evento: Evento, tipo_notificacion: str):
        for observador in self.observadores:
            observador.notificar_evento(evento, tipo_notificacion)

class ObservadorLogEventos(ObservadorEvento):
    def __init__(self, archivo_log: str = "eventos.log"):
        self.archivo_log = archivo_log
    
    def notificar_evento(self, evento: Evento, tipo_notificacion: str):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        mensaje = f"[{timestamp}] {tipo_notificacion}: {evento.nombre} ({evento.tipo.value})\n"
        
        try:
            with open(self.archivo_log, 'a', encoding='utf-8') as f:
                f.write(mensaje)
        except Exception as e:
            print(f"Error al escribir log: {e}")

class ObservadorConsolaEventos(ObservadorEvento):
    def notificar_evento(self, evento: Evento, tipo_notificacion: str):
        emoji = {
            'creado': '📅',
            'modificado': '✏️',
            'simulado': '⚙️',
            'completado': '✅',
            'cancelado': '❌',
            'error': '🚨'
        }.get(tipo_notificacion, '📋')
        
        print(f"{emoji} {tipo_notificacion.upper()}: {evento.nombre}")

class ObservadorEstadisticasEventos(ObservadorEvento):
    def __init__(self):
        self.estadisticas = {
            'eventos_creados': 0,
            'eventos_simulados': 0,
            'eventos_completados': 0,
            'eventos_cancelados': 0,
            'total_ingresos': 0.0,
            'total_costos': 0.0,
            'total_ganancias': 0.0
        }
    
    def notificar_evento(self, evento: Evento, tipo_notificacion: str):
        if tipo_notificacion in self.estadisticas:
            self.estadisticas[tipo_notificacion] += 1
        
        # Mostrar estadísticas cada 10 eventos
        total_eventos = sum(v for k, v in self.estadisticas.items() if 'eventos' in k)
        if total_eventos % 10 == 0:
            self.mostrar_estadisticas()
    
    def mostrar_estadisticas(self):
        print("\n📊 ESTADÍSTICAS DEL SIMULADOR:")
        for evento, cantidad in self.estadisticas.items():
            if 'total' in evento:
                print(f"  {evento.replace('_', ' ').title()}: ${cantidad:.2f}")
            else:
                print(f"  {evento.replace('_', ' ').title()}: {cantidad}")
        print()
    
    def actualizar_financiero(self, ingresos: float, costos: float, ganancias: float):
        self.estadisticas['total_ingresos'] += ingresos
        self.estadisticas['total_costos'] += costos
        self.estadisticas['total_ganancias'] += ganancias
```

### **6. Simulador Principal**

```python
class SimuladorEventos:
    def __init__(self):
        self.simuladores = {
            TipoEvento.DEPORTIVO: SimuladorDeportivo(),
            TipoEvento.MUSICAL: SimuladorMusical(),
            TipoEvento.TECNOLOGICO: SimuladorTecnologico()
        }
        
        self.generador = GeneradorEventosAleatorio()
        self.observadores = GestorObservadoresEventos()
        
        # Agregar observadores por defecto
        self.observadores.agregar_observador(ObservadorConsolaEventos())
        self.observadores.agregar_observador(ObservadorLogEventos())
        self.observadores.agregar_observador(ObservadorEstadisticasEventos())
        
        self.logger = logging.getLogger(__name__)
    
    def simular_evento(self, evento: Evento, parametros: Dict[str, Any] = None) -> Dict[str, Any]:
        """Simula un evento específico"""
        if parametros is None:
            parametros = {}
        
        try:
            # Seleccionar simulador apropiado
            simulador = self.simuladores.get(evento.tipo)
            if not simulador:
                # Usar simulador genérico
                simulador = SimuladorDeportivo()  # Por defecto
            
            # Ejecutar simulación
            resultado = simulador.simular(evento, parametros)
            
            # Notificar observadores
            self.observadores.notificar_todos(evento, 'simulado')
            
            return resultado
            
        except Exception as e:
            self.logger.error(f"Error al simular evento {evento.nombre}: {e}")
            return {"error": str(e)}
    
    def simular_multiples_eventos(self, eventos: List[Evento], 
                                parametros_globales: Dict[str, Any] = None) -> List[Dict[str, Any]]:
        """Simula múltiples eventos"""
        resultados = []
        
        for evento in eventos:
            # Generar parámetros específicos para cada evento
            parametros_evento = self._generar_parametros_evento(evento, parametros_globales)
            
            # Simular evento
            resultado = self.simular_evento(evento, parametros_evento)
            resultados.append(resultado)
        
        return resultados
    
    def _generar_parametros_evento(self, evento: Evento, 
                                 parametros_globales: Dict[str, Any]) -> Dict[str, Any]:
        """Genera parámetros específicos para cada tipo de evento"""
        parametros = {}
        
        if evento.tipo == TipoEvento.DEPORTIVO:
            parametros['clima'] = random.choice(['soleado', 'nublado', 'lluvia', 'tormenta'])
        
        elif evento.tipo == TipoEvento.MUSICAL:
            parametros['popularidad'] = random.choice(['baja', 'media', 'alta', 'muy_alta'])
        
        elif evento.tipo == TipoEvento.TECNOLOGICO:
            parametros['nivel_innovacion'] = random.choice(['baja', 'media', 'alta', 'muy_alta'])
        
        # Agregar parámetros globales si existen
        if parametros_globales:
            parametros.update(parametros_globales)
        
        return parametros
    
    def generar_y_simular_eventos(self, cantidad: int, fecha_inicio: datetime, 
                                fecha_fin: datetime) -> List[Dict[str, Any]]:
        """Genera eventos aleatorios y los simula"""
        # Generar eventos
        eventos = self.generador.generar_eventos_aleatorios(cantidad, fecha_inicio, fecha_fin)
        
        # Notificar creación
        for evento in eventos:
            self.observadores.notificar_todos(evento, 'creado')
        
        # Simular eventos
        resultados = self.simular_multiples_eventos(eventos)
        
        return resultados
    
    def analizar_resultados(self, resultados: List[Dict[str, Any]]) -> Dict[str, Any]:
        """Analiza los resultados de las simulaciones"""
        if not resultados:
            return {"error": "No hay resultados para analizar"}
        
        # Estadísticas generales
        total_eventos = len(resultados)
        eventos_exitosos = [r for r in resultados if 'error' not in r]
        eventos_fallidos = [r for r in resultados if 'error' in r]
        
        if eventos_exitosos:
            # Calcular promedios
            asistencia_promedio = sum(r.get('asistencia_esperada', 0) for r in eventos_exitosos) / len(eventos_exitosos)
            ingresos_promedio = sum(r.get('ingresos', 0) for r in eventos_exitosos) / len(eventos_exitosos)
            ganancia_promedio = sum(r.get('ganancia', 0) for r in eventos_exitosos) / len(eventos_exitosos)
            
            # Encontrar mejor y peor evento
            mejor_evento = max(eventos_exitosos, key=lambda x: x.get('ganancia', 0))
            peor_evento = min(eventos_exitosos, key=lambda x: x.get('ganancia', 0))
            
            analisis = {
                'total_eventos': total_eventos,
                'eventos_exitosos': len(eventos_exitosos),
                'eventos_fallidos': len(eventos_fallidos),
                'tasa_exito': (len(eventos_exitosos) / total_eventos) * 100,
                'asistencia_promedio': asistencia_promedio,
                'ingresos_promedio': ingresos_promedio,
                'ganancia_promedio': ganancia_promedio,
                'mejor_evento': {
                    'ganancia': mejor_evento.get('ganancia', 0),
                    'tipo': mejor_evento.get('tipo_simulacion', 'desconocido')
                },
                'peor_evento': {
                    'ganancia': peor_evento.get('ganancia', 0),
                    'tipo': peor_evento.get('tipo_simulacion', 'desconocido')
                }
            }
        else:
            analisis = {
                'total_eventos': total_eventos,
                'eventos_exitosos': 0,
                'eventos_fallidos': total_eventos,
                'tasa_exito': 0,
                'error': 'Todos los eventos fallaron'
            }
        
        return analisis
    
    def generar_reporte_simulacion(self, resultados: List[Dict[str, Any]]) -> str:
        """Genera un reporte detallado de las simulaciones"""
        analisis = self.analizar_resultados(resultados)
        
        reporte = f"""
=== REPORTE DE SIMULACIÓN DE EVENTOS ===

📊 ESTADÍSTICAS GENERALES:
  - Total de eventos: {analisis['total_eventos']}
  - Eventos exitosos: {analisis['eventos_exitosos']}
  - Eventos fallidos: {analisis['eventos_fallidos']}
  - Tasa de éxito: {analisis['tasa_exito']:.1f}%

📈 MÉTRICAS PROMEDIO:
  - Asistencia promedio: {analisis.get('asistencia_promedio', 0):.0f} personas
  - Ingresos promedio: ${analisis.get('ingresos_promedio', 0):.2f}
  - Ganancia promedio: ${analisis.get('ganancia_promedio', 0):.2f}

🏆 MEJOR EVENTO:
  - Ganancia: ${analisis.get('mejor_evento', {}).get('ganancia', 0):.2f}
  - Tipo: {analisis.get('mejor_evento', {}).get('tipo', 'N/A')}

📉 PEOR EVENTO:
  - Ganancia: ${analisis.get('peor_evento', {}).get('ganancia', 0):.2f}
  - Tipo: {analisis.get('peor_evento', {}).get('tipo', 'N/A')}

📋 DETALLES POR EVENTO:
"""
        
        for i, resultado in enumerate(resultados, 1):
            if 'error' not in resultado:
                reporte += (f"  {i}. {resultado.get('tipo_simulacion', 'N/A').title()}: "
                          f"Asistencia: {resultado.get('asistencia_esperada', 0)}, "
                          f"Ganancia: ${resultado.get('ganancia', 0):.2f}\n")
            else:
                reporte += f"  {i}. Error: {resultado['error']}\n"
        
        return reporte
    
    def agregar_observador(self, observador: ObservadorEvento):
        """Agrega un nuevo observador"""
        self.observadores.agregar_observador(observador)
    
    def remover_observador(self, observador: ObservadorEvento):
        """Remueve un observador"""
        self.observadores.remover_observador(observador)
```

### **7. Ejemplo de Uso**

```python
def main():
    # Configurar logging
    logging.basicConfig(level=logging.INFO)
    
    print("🎯 SIMULADOR DE EVENTOS INICIADO\n")
    
    # Crear simulador
    simulador = SimuladorEventos()
    
    # Configurar fechas para la simulación
    fecha_inicio = datetime.now()
    fecha_fin = fecha_inicio + timedelta(days=30)  # Simular eventos en los próximos 30 días
    
    try:
        # Generar y simular eventos
        print("🎲 Generando eventos aleatorios...")
        resultados = simulador.generar_y_simular_eventos(15, fecha_inicio, fecha_fin)
        
        print(f"✅ Simulación completada: {len(resultados)} eventos procesados\n")
        
        # Analizar resultados
        print("📊 Analizando resultados...")
        analisis = simulador.analizar_resultados(resultados)
        
        # Generar reporte
        print("📋 Generando reporte...")
        reporte = simulador.generar_reporte_simulacion(resultados)
        print(reporte)
        
        # Mostrar detalles de algunos eventos
        print("\n🔍 DETALLES DE EVENTOS SELECCIONADOS:")
        eventos_exitosos = [r for r in resultados if 'error' not in r]
        
        if eventos_exitosos:
            # Mostrar los 3 eventos más rentables
            eventos_ordenados = sorted(eventos_exitosos, key=lambda x: x.get('ganancia', 0), reverse=True)
            
            print("\n🥇 TOP 3 EVENTOS MÁS RENTABLES:")
            for i, resultado in enumerate(eventos_ordenados[:3], 1):
                print(f"  {i}. {resultado.get('tipo_simulacion', 'N/A').title()}")
                print(f"     Asistencia: {resultado.get('asistencia_esperada', 0)} personas")
                print(f"     Ingresos: ${resultado.get('ingresos', 0):.2f}")
                print(f"     Costos: ${resultado.get('costos', 0):.2f}")
                print(f"     Ganancia: ${resultado.get('ganancia', 0):.2f}")
                print(f"     Rentabilidad: {resultado.get('rentabilidad', 0):.1f}%")
                print()
        
        # Mostrar estadísticas por tipo de evento
        print("📊 ESTADÍSTICAS POR TIPO DE EVENTO:")
        tipos_evento = {}
        for resultado in eventos_exitosos:
            tipo = resultado.get('tipo_simulacion', 'desconocido')
            if tipo not in tipos_evento:
                tipos_evento[tipo] = []
            tipos_evento[tipo].append(resultado)
        
        for tipo, eventos_tipo in tipos_evento.items():
            ganancia_total = sum(e.get('ganancia', 0) for e in eventos_tipo)
            asistencia_total = sum(e.get('asistencia_esperada', 0) for e in eventos_tipo)
            print(f"  {tipo.title()}: {len(eventos_tipo)} eventos, "
                  f"Ganancia total: ${ganancia_total:.2f}, "
                  f"Asistencia total: {asistencia_total}")
        
    except Exception as e:
        print(f"❌ Error en la simulación: {e}")

if __name__ == "__main__":
    main()
```

## 🎯 Características del Proyecto

### **Paradigmas Implementados**
- **POO**: Clases, herencia, encapsulamiento, polimorfismo
- **Funcional**: Funciones puras, composición, pipelines
- **Reactivo**: Sistema de observadores, notificaciones en tiempo real
- **Patrones**: Strategy, Observer, Factory

### **Funcionalidades**
- ✅ Generación automática de eventos
- ✅ Simulación por tipo de evento
- ✅ Sistema de observadores reactivo
- ✅ Análisis de resultados
- ✅ Generación de reportes
- ✅ Logging y estadísticas
- ✅ Parámetros configurables

### **Extensibilidad**
- Fácil agregar nuevos tipos de simuladores
- Nuevos observadores para diferentes eventos
- Parámetros personalizables por tipo de evento
- Sistema de plugins para simulaciones

## 🚀 Mejoras Futuras

1. **Interfaz Gráfica**: Dashboard web con Flask/Django
2. **Base de Datos**: Persistencia de eventos y resultados
3. **Machine Learning**: Predicciones basadas en datos históricos
4. **API REST**: Endpoints para integración externa
5. **Reportes Avanzados**: Gráficos y visualizaciones
6. **Simulaciones en Tiempo Real**: Eventos que se ejecutan en vivo
7. **Integración con Calendarios**: Google Calendar, Outlook

---

**Siguiente**: [04. Proyecto API REST](./04-proyecto-api-rest.md) 