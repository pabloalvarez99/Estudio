# 04. Proyecto: API REST con Paradigmas de Programación

## 🎯 Descripción del Proyecto

Implementar una API REST completa que demuestre el uso de **POO**, **Programación Funcional**, **Patrones de Diseño** y **Principios SOLID** para crear un sistema de gestión de tareas.

## 🏗️ Arquitectura del Sistema

### **Estructura del Proyecto**
```
API-REST/
├── app/
│   ├── models/ (POO)
│   ├── services/ (Funcional)
│   ├── controllers/ (POO)
│   └── middleware/ (Patrones)
├── tests/ (Testing)
└── main.py
```

## 🔧 Implementación

### **1. Modelos del Dominio (POO)**

```python
from dataclasses import dataclass
from datetime import datetime
from typing import List, Optional
from enum import Enum
import uuid

class EstadoTarea(Enum):
    PENDIENTE = "pendiente"
    EN_PROGRESO = "en_progreso"
    COMPLETADA = "completada"
    CANCELADA = "cancelada"

class PrioridadTarea(Enum):
    BAJA = "baja"
    MEDIA = "media"
    ALTA = "alta"
    URGENTE = "urgente"

@dataclass
class Usuario:
    id: str
    nombre: str
    email: str
    fecha_registro: datetime
    activo: bool = True
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha_registro:
            self.fecha_registro = datetime.now()

@dataclass
class Tarea:
    id: str
    titulo: str
    descripcion: str
    usuario_id: str
    estado: EstadoTarea
    prioridad: PrioridadTarea
    fecha_creacion: datetime
    fecha_limite: Optional[datetime]
    fecha_completada: Optional[datetime]
    etiquetas: List[str]
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha_creacion:
            self.fecha_creacion = datetime.now()
        if not self.etiquetas:
            self.etiquetas = []
    
    def cambiar_estado(self, nuevo_estado: EstadoTarea):
        self.estado = nuevo_estado
        if nuevo_estado == EstadoTarea.COMPLETADA:
            self.fecha_completada = datetime.now()
    
    def esta_vencida(self) -> bool:
        if not self.fecha_limite:
            return False
        return datetime.now() > self.fecha_limite and self.estado != EstadoTarea.COMPLETADA
    
    def obtener_info(self) -> str:
        return f"Tarea: {self.titulo} - Estado: {self.estado.value} - Prioridad: {self.prioridad.value}"
```

### **2. Interfaces y Abstracciones (SOLID)**

```python
from abc import ABC, abstractmethod
from typing import List, Optional, Dict, Any

class RepositorioUsuario(ABC):
    @abstractmethod
    def guardar(self, usuario: Usuario) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_id(self, id: str) -> Optional[Usuario]:
        pass
    
    @abstractmethod
    def obtener_todos(self) -> List[Usuario]:
        pass

class RepositorioTarea(ABC):
    @abstractmethod
    def guardar(self, tarea: Tarea) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_id(self, id: str) -> Optional[Tarea]:
        pass
    
    @abstractmethod
    def obtener_por_usuario(self, usuario_id: str) -> List[Tarea]:
        pass
    
    @abstractmethod
    def obtener_por_estado(self, estado: EstadoTarea) -> List[Tarea]:
        pass

class ServicioValidacion(ABC):
    @abstractmethod
    def validar_usuario(self, usuario: Usuario) -> bool:
        pass
    
    @abstractmethod
    def validar_tarea(self, tarea: Tarea) -> bool:
        pass

class ServicioNotificacion(ABC):
    @abstractmethod
    def enviar_notificacion(self, usuario_id: str, mensaje: str):
        pass
```

### **3. Implementaciones de Repositorios**

```python
class RepositorioUsuarioMemoria(RepositorioUsuario):
    def __init__(self):
        self.usuarios: Dict[str, Usuario] = {}
    
    def guardar(self, usuario: Usuario) -> bool:
        try:
            self.usuarios[usuario.id] = usuario
            return True
        except Exception:
            return False
    
    def obtener_por_id(self, id: str) -> Optional[Usuario]:
        return self.usuarios.get(id)
    
    def obtener_todos(self) -> List[Usuario]:
        return list(self.usuarios.values())

class RepositorioTareaMemoria(RepositorioTarea):
    def __init__(self):
        self.tareas: Dict[str, Tarea] = {}
    
    def guardar(self, tarea: Tarea) -> bool:
        try:
            self.tareas[tarea.id] = tarea
            return True
        except Exception:
            return False
    
    def obtener_por_id(self, id: str) -> Optional[Tarea]:
        return self.tareas.get(id)
    
    def obtener_por_usuario(self, usuario_id: str) -> List[Tarea]:
        return [t for t in self.tareas.values() if t.usuario_id == usuario_id]
    
    def obtener_por_estado(self, estado: EstadoTarea) -> List[Tarea]:
        return [t for t in self.tareas.values() if t.estado == estado]
```

### **4. Servicios de Negocio (Funcional)**

```python
from functools import reduce
from typing import Callable, List

class ServicioTareas:
    def __init__(self, repositorio: RepositorioTarea, validador: ServicioValidacion):
        self.repositorio = repositorio
        self.validador = validador
    
    def crear_tarea(self, titulo: str, descripcion: str, usuario_id: str, 
                    prioridad: PrioridadTarea, fecha_limite: Optional[datetime] = None) -> Optional[Tarea]:
        tarea = Tarea(
            id="",
            titulo=titulo,
            descripcion=descripcion,
            usuario_id=usuario_id,
            estado=EstadoTarea.PENDIENTE,
            prioridad=prioridad,
            fecha_creacion=datetime.now(),
            fecha_limite=fecha_limite,
            fecha_completada=None,
            etiquetas=[]
        )
        
        if self.validador.validar_tarea(tarea):
            if self.repositorio.guardar(tarea):
                return tarea
        return None
    
    def filtrar_tareas(self, tareas: List[Tarea], *filtros: Callable[[Tarea], bool]) -> List[Tarea]:
        """Aplica múltiples filtros usando programación funcional"""
        def aplicar_filtro(lista: List[Tarea], filtro: Callable[[Tarea], bool]) -> List[Tarea]:
            return list(filter(filtro, lista))
        
        return reduce(aplicar_filtro, filtros, tareas)
    
    def transformar_tareas(self, tareas: List[Tarea], transformacion: Callable[[Tarea], Any]) -> List[Any]:
        """Transforma tareas usando map"""
        return list(map(transformacion, tareas))
    
    def agrupar_por_estado(self, tareas: List[Tarea]) -> Dict[EstadoTarea, List[Tarea]]:
        """Agrupa tareas por estado usando reduce"""
        def agregar_a_grupo(grupos: Dict[EstadoTarea, List[Tarea]], tarea: Tarea) -> Dict[EstadoTarea, List[Tarea]]:
            estado = tarea.estado
            if estado not in grupos:
                grupos[estado] = []
            grupos[estado].append(tarea)
            return grupos
        
        return reduce(agregar_a_grupo, tareas, {})
    
    def obtener_estadisticas(self, usuario_id: str) -> Dict[str, Any]:
        """Obtiene estadísticas de las tareas de un usuario"""
        tareas = self.repositorio.obtener_por_usuario(usuario_id)
        
        if not tareas:
            return {"total": 0, "completadas": 0, "pendientes": 0, "vencidas": 0}
        
        total = len(tareas)
        completadas = len([t for t in tareas if t.estado == EstadoTarea.COMPLETADA])
        pendientes = len([t for t in tareas if t.estado == EstadoTarea.PENDIENTE])
        vencidas = len([t for t in tareas if t.esta_vencida()])
        
        return {
            "total": total,
            "completadas": completadas,
            "pendientes": pendientes,
            "vencidas": vencidas,
            "porcentaje_completadas": (completadas / total) * 100 if total > 0 else 0
        }

# Funciones de filtrado
def filtro_por_estado(estado: EstadoTarea) -> Callable[[Tarea], bool]:
    return lambda tarea: tarea.estado == estado

def filtro_por_prioridad(prioridad: PrioridadTarea) -> Callable[[Tarea], bool]:
    return lambda tarea: tarea.prioridad == prioridad

def filtro_por_etiqueta(etiqueta: str) -> Callable[[Tarea], bool]:
    return lambda tarea: etiqueta in tarea.etiquetas

def filtro_vencidas() -> Callable[[Tarea], bool]:
    return lambda tarea: tarea.esta_vencida()

# Funciones de transformación
def transformar_a_dict(tarea: Tarea) -> Dict[str, Any]:
    return {
        "id": tarea.id,
        "titulo": tarea.titulo,
        "estado": tarea.estado.value,
        "prioridad": tarea.prioridad.value,
        "fecha_creacion": tarea.fecha_creacion.isoformat()
    }

def transformar_a_resumen(tarea: Tarea) -> str:
    return f"{tarea.titulo} ({tarea.estado.value}) - {tarea.prioridad.value}"
```

### **5. Controladores de la API (POO)**

```python
from flask import Flask, request, jsonify
from typing import Dict, Any

class ControladorUsuario:
    def __init__(self, repositorio: RepositorioUsuario, validador: ServicioValidacion):
        self.repositorio = repositorio
        self.validador = validador
    
    def crear_usuario(self) -> tuple[Dict[str, Any], int]:
        try:
            data = request.get_json()
            
            if not data or 'nombre' not in data or 'email' not in data:
                return {"error": "Datos incompletos"}, 400
            
            usuario = Usuario(
                id="",
                nombre=data['nombre'],
                email=data['email'],
                fecha_registro=datetime.now()
            )
            
            if self.validador.validar_usuario(usuario):
                if self.repositorio.guardar(usuario):
                    return {"id": usuario.id, "mensaje": "Usuario creado exitosamente"}, 201
                else:
                    return {"error": "Error al guardar usuario"}, 500
            else:
                return {"error": "Datos de usuario inválidos"}, 400
                
        except Exception as e:
            return {"error": str(e)}, 500
    
    def obtener_usuario(self, usuario_id: str) -> tuple[Dict[str, Any], int]:
        usuario = self.repositorio.obtener_por_id(usuario_id)
        
        if usuario:
            return {
                "id": usuario.id,
                "nombre": usuario.nombre,
                "email": usuario.email,
                "fecha_registro": usuario.fecha_registro.isoformat(),
                "activo": usuario.activo
            }, 200
        else:
            return {"error": "Usuario no encontrado"}, 404
    
    def listar_usuarios(self) -> tuple[Dict[str, Any], int]:
        usuarios = self.repositorio.obtener_todos()
        
        return {
            "usuarios": [
                {
                    "id": u.id,
                    "nombre": u.nombre,
                    "email": u.email,
                    "activo": u.activo
                } for u in usuarios
            ],
            "total": len(usuarios)
        }, 200

class ControladorTarea:
    def __init__(self, servicio: ServicioTareas):
        self.servicio = servicio
    
    def crear_tarea(self) -> tuple[Dict[str, Any], int]:
        try:
            data = request.get_json()
            
            if not data or 'titulo' not in data or 'usuario_id' not in data:
                return {"error": "Datos incompletos"}, 400
            
            prioridad = PrioridadTarea(data.get('prioridad', 'media'))
            fecha_limite = None
            
            if 'fecha_limite' in data:
                fecha_limite = datetime.fromisoformat(data['fecha_limite'])
            
            tarea = self.servicio.crear_tarea(
                titulo=data['titulo'],
                descripcion=data.get('descripcion', ''),
                usuario_id=data['usuario_id'],
                prioridad=prioridad,
                fecha_limite=fecha_limite
            )
            
            if tarea:
                return {
                    "id": tarea.id,
                    "mensaje": "Tarea creada exitosamente"
                }, 201
            else:
                return {"error": "Error al crear tarea"}, 500
                
        except Exception as e:
            return {"error": str(e)}, 500
    
    def obtener_tarea(self, tarea_id: str) -> tuple[Dict[str, Any], int]:
        tarea = self.servicio.repositorio.obtener_por_id(tarea_id)
        
        if tarea:
            return {
                "id": tarea.id,
                "titulo": tarea.titulo,
                "descripcion": tarea.descripcion,
                "estado": tarea.estado.value,
                "prioridad": tarea.prioridad.value,
                "fecha_creacion": tarea.fecha_creacion.isoformat(),
                "fecha_limite": tarea.fecha_limite.isoformat() if tarea.fecha_limite else None,
                "etiquetas": tarea.etiquetas
            }, 200
        else:
            return {"error": "Tarea no encontrada"}, 404
    
    def listar_tareas_usuario(self, usuario_id: str) -> tuple[Dict[str, Any], int]:
        tareas = self.servicio.repositorio.obtener_por_usuario(usuario_id)
        
        # Aplicar filtros si se especifican
        estado_filtro = request.args.get('estado')
        prioridad_filtro = request.args.get('prioridad')
        
        filtros = []
        if estado_filtro:
            filtros.append(filtro_por_estado(EstadoTarea(estado_filtro)))
        if prioridad_filtro:
            filtros.append(filtro_por_prioridad(PrioridadTarea(prioridad_filtro)))
        
        if filtros:
            tareas = self.servicio.filtrar_tareas(tareas, *filtros)
        
        # Transformar a formato de respuesta
        tareas_dict = self.servicio.transformar_tareas(tareas, transformar_a_dict)
        
        return {
            "tareas": tareas_dict,
            "total": len(tareas_dict)
        }, 200
    
    def cambiar_estado_tarea(self, tarea_id: str) -> tuple[Dict[str, Any], int]:
        try:
            data = request.get_json()
            
            if not data or 'estado' not in data:
                return {"error": "Estado no especificado"}, 400
            
            tarea = self.servicio.repositorio.obtener_por_id(tarea_id)
            if not tarea:
                return {"error": "Tarea no encontrada"}, 404
            
            nuevo_estado = EstadoTarea(data['estado'])
            tarea.cambiar_estado(nuevo_estado)
            
            if self.servicio.repositorio.guardar(tarea):
                return {"mensaje": f"Estado cambiado a {nuevo_estado.value}"}, 200
            else:
                return {"error": "Error al actualizar tarea"}, 500
                
        except Exception as e:
            return {"error": str(e)}, 500
    
    def obtener_estadisticas_usuario(self, usuario_id: str) -> tuple[Dict[str, Any], int]:
        estadisticas = self.servicio.obtener_estadisticas(usuario_id)
        return estadisticas, 200
```

### **6. Aplicación Principal**

```python
from flask import Flask
from flask_cors import CORS

def crear_app() -> Flask:
    app = Flask(__name__)
    CORS(app)
    
    # Inicializar repositorios y servicios
    repositorio_usuario = RepositorioUsuarioMemoria()
    repositorio_tarea = RepositorioTareaMemoria()
    
    # Inicializar controladores
    controlador_usuario = ControladorUsuario(repositorio_usuario, None)  # Sin validador por simplicidad
    controlador_tarea = ControladorTarea(ServicioTareas(repositorio_tarea, None))
    
    # Rutas de usuarios
    @app.route('/usuarios', methods=['POST'])
    def crear_usuario():
        return controlador_usuario.crear_usuario()
    
    @app.route('/usuarios', methods=['GET'])
    def listar_usuarios():
        return controlador_usuario.listar_usuarios()
    
    @app.route('/usuarios/<usuario_id>', methods=['GET'])
    def obtener_usuario(usuario_id):
        return controlador_usuario.obtener_usuario(usuario_id)
    
    # Rutas de tareas
    @app.route('/tareas', methods=['POST'])
    def crear_tarea():
        return controlador_tarea.crear_tarea()
    
    @app.route('/tareas/<tarea_id>', methods=['GET'])
    def obtener_tarea(tarea_id):
        return controlador_tarea.obtener_tarea(tarea_id)
    
    @app.route('/usuarios/<usuario_id>/tareas', methods=['GET'])
    def listar_tareas_usuario(usuario_id):
        return controlador_tarea.listar_tareas_usuario(usuario_id)
    
    @app.route('/tareas/<tarea_id>/estado', methods=['PUT'])
    def cambiar_estado_tarea(tarea_id):
        return controlador_tarea.cambiar_estado_tarea(tarea_id)
    
    @app.route('/usuarios/<usuario_id>/estadisticas', methods=['GET'])
    def obtener_estadisticas_usuario(usuario_id):
        return controlador_tarea.obtener_estadisticas_usuario(usuario_id)
    
    return app

if __name__ == "__main__":
    app = crear_app()
    app.run(debug=True, host='0.0.0.0', port=5000)
```

## 🎯 Características del Proyecto

### **Paradigmas Implementados**
- **POO**: Clases, herencia, encapsulamiento
- **Funcional**: Funciones puras, map/filter/reduce, composición
- **Patrones**: Repository, Strategy, Factory
- **SOLID**: Separación de responsabilidades, inversión de dependencias

### **Funcionalidades**
- ✅ CRUD de usuarios y tareas
- ✅ Filtrado y transformación funcional
- ✅ Estadísticas de tareas
- ✅ API REST completa
- ✅ Manejo de errores
- ✅ CORS habilitado

## 🚀 Mejoras Futuras

1. **Base de Datos**: SQLAlchemy, MongoDB
2. **Autenticación**: JWT, OAuth
3. **Validación**: Marshmallow, Pydantic
4. **Testing**: pytest, unittest
5. **Documentación**: Swagger, OpenAPI
6. **Logging**: Structured logging
7. **Monitoreo**: Health checks, metrics

---

**Anterior**: [03. Proyecto Simulador de Eventos](./03-proyecto-simulador-eventos.md)  
**Siguiente**: [Evaluación de Paradigmas](./../04-EVALUACION/01-checklist-conceptos.md) 