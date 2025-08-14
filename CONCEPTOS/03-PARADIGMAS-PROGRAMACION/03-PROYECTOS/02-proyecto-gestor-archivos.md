# 02. Proyecto: Gestor de Archivos con Paradigmas de Programación

## 🎯 Descripción del Proyecto

Implementar un gestor de archivos avanzado que demuestre el uso de **POO**, **Programación Funcional**, **Patrones de Diseño** y **Programación Reactiva** para el procesamiento y gestión de archivos.

## 🏗️ Arquitectura del Sistema

### **Estructura de Clases**
```
GestorArchivos/
├── Core/
│   ├── Entidades (POO)
│   ├── Procesadores (Funcional)
│   └── Observadores (Reactivo)
├── Patrones/
│   ├── Strategy (Procesamiento)
│   ├── Observer (Monitoreo)
│   └── Factory (Creación)
└── Utilidades/
    └── Helpers (Funcional)
```

## 🔧 Implementación

### **1. Entidades del Dominio (POO)**

```python
from dataclasses import dataclass
from datetime import datetime
from typing import List, Dict, Optional, Any
from pathlib import Path
import hashlib
import os

@dataclass
class Archivo:
    ruta: Path
    nombre: str
    extension: str
    tamano: int
    fecha_creacion: datetime
    fecha_modificacion: datetime
    hash_md5: str
    contenido: Optional[str] = None
    
    def __post_init__(self):
        if not self.nombre:
            self.nombre = self.ruta.name
        if not self.extension:
            self.extension = self.ruta.suffix
        if not self.hash_md5:
            self.hash_md5 = self._calcular_hash()
    
    def _calcular_hash(self) -> str:
        try:
            with open(self.ruta, 'rb') as f:
                contenido = f.read()
                return hashlib.md5(contenido).hexdigest()
        except Exception:
            return ""
    
    def obtener_info(self) -> str:
        return (f"Archivo: {self.nombre} "
                f"({self.extension}) - "
                f"Tamaño: {self.tamano} bytes - "
                f"Hash: {self.hash_md5[:8]}...")
    
    def es_texto(self) -> bool:
        extensiones_texto = {'.txt', '.md', '.py', '.js', '.html', '.css', '.json', '.xml'}
        return self.extension.lower() in extensiones_texto
    
    def es_imagen(self) -> bool:
        extensiones_imagen = {'.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg'}
        return self.extension.lower() in extensiones_imagen
    
    def es_documento(self) -> bool:
        extensiones_documento = {'.pdf', '.doc', '.docx', '.xls', '.xlsx', '.ppt', '.pptx'}
        return self.extension.lower() in extensiones_documento

@dataclass
class Directorio:
    ruta: Path
    nombre: str
    archivos: List[Archivo]
    subdirectorios: List['Directorio']
    fecha_escaneo: datetime
    
    def __post_init__(self):
        if not self.nombre:
            self.nombre = self.ruta.name
        if not self.fecha_escaneo:
            self.fecha_escaneo = datetime.now()
    
    def obtener_total_archivos(self) -> int:
        total = len(self.archivos)
        for subdir in self.subdirectorios:
            total += subdir.obtener_total_archivos()
        return total
    
    def obtener_tamano_total(self) -> int:
        tamano = sum(archivo.tamano for archivo in self.archivos)
        for subdir in self.subdirectorios:
            tamano += subdir.obtener_tamano_total()
        return tamano
    
    def obtener_info(self) -> str:
        return (f"Directorio: {self.nombre} - "
                f"Archivos: {self.obtener_total_archivos()} - "
                f"Tamaño: {self.obtener_tamano_total()} bytes")

@dataclass
class OperacionArchivo:
    id: str
    tipo: str  # 'copiar', 'mover', 'eliminar', 'procesar'
    archivo_origen: Archivo
    archivo_destino: Optional[Archivo]
    fecha_operacion: datetime
    estado: str  # 'pendiente', 'en_proceso', 'completada', 'fallida'
    resultado: Optional[str] = None
    
    def __post_init__(self):
        if not self.fecha_operacion:
            self.fecha_operacion = datetime.now()
    
    def obtener_info(self) -> str:
        return (f"Operación {self.tipo} - "
                f"Archivo: {self.archivo_origen.nombre} - "
                f"Estado: {self.estado}")
```

### **2. Interfaces y Abstracciones (Principios SOLID)**

```python
from abc import ABC, abstractmethod
from typing import Protocol, Callable, List

class ProcesadorArchivo(ABC):
    @abstractmethod
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def obtener_nombre(self) -> str:
        pass

class ValidadorArchivo(ABC):
    @abstractmethod
    def validar(self, archivo: Archivo) -> bool:
        pass
    
    @abstractmethod
    def obtener_errores(self) -> List[str]:
        pass

class ObservadorArchivo(ABC):
    @abstractmethod
    def notificar_cambio(self, archivo: Archivo, evento: str):
        pass

class RepositorioArchivo(ABC):
    @abstractmethod
    def guardar(self, archivo: Archivo) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_ruta(self, ruta: str) -> Optional[Archivo]:
        pass
    
    @abstractmethod
    def listar_todos(self) -> List[Archivo]:
        pass

class ServicioNotificacion(ABC):
    @abstractmethod
    def enviar_notificacion(self, mensaje: str, tipo: str = "info"):
        pass
```

### **3. Implementaciones de Procesadores (Strategy Pattern)**

```python
class ProcesadorTexto(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_texto():
            return {"error": "Archivo no es de texto"}
        
        try:
            with open(archivo.ruta, 'r', encoding='utf-8') as f:
                contenido = f.read()
            
            resultado = {
                "lineas": len(contenido.splitlines()),
                "palabras": len(contenido.split()),
                "caracteres": len(contenido),
                "caracteres_sin_espacios": len(contenido.replace(" ", "")),
                "palabras_unicas": len(set(contenido.lower().split())),
                "extension": archivo.extension,
                "tamano_kb": archivo.tamano / 1024
            }
            
            return resultado
            
        except Exception as e:
            return {"error": str(e)}
    
    def obtener_nombre(self) -> str:
        return "Procesador de Texto"

class ProcesadorImagen(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_imagen():
            return {"error": "Archivo no es una imagen"}
        
        # Simulación de procesamiento de imagen
        resultado = {
            "tipo": "imagen",
            "extension": archivo.extension,
            "tamano_mb": archivo.tamano / (1024 * 1024),
            "formato": archivo.extension[1:].upper(),
            "compresion": "JPEG" if archivo.extension.lower() in ['.jpg', '.jpeg'] else "Otro"
        }
        
        return resultado
    
    def obtener_nombre(self) -> str:
        return "Procesador de Imagen"

class ProcesadorDocumento(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_documento():
            return {"error": "Archivo no es un documento"}
        
        resultado = {
            "tipo": "documento",
            "extension": archivo.extension,
            "tamano_mb": archivo.tamano / (1024 * 1024),
            "categoria": self._categorizar_documento(archivo.extension),
            "version": "1.0"
        }
        
        return resultado
    
    def _categorizar_documento(self, extension: str) -> str:
        categorias = {
            '.pdf': 'PDF',
            '.doc': 'Word',
            '.docx': 'Word',
            '.xls': 'Excel',
            '.xlsx': 'Excel',
            '.ppt': 'PowerPoint',
            '.pptx': 'PowerPoint'
        }
        return categorias.get(extension.lower(), 'Desconocido')
    
    def obtener_nombre(self) -> str:
        return "Procesador de Documento"
```

### **4. Sistema de Observadores (Observer Pattern + Reactivo)**

```python
class GestorObservadores:
    def __init__(self):
        self.observadores: List[ObservadorArchivo] = []
    
    def agregar_observador(self, observador: ObservadorArchivo):
        if observador not in self.observadores:
            self.observadores.append(observador)
    
    def remover_observador(self, observador: ObservadorArchivo):
        if observador in self.observadores:
            self.observadores.remove(observador)
    
    def notificar_todos(self, archivo: Archivo, evento: str):
        for observador in self.observadores:
            observador.notificar_cambio(archivo, evento)

class ObservadorLog(ObservadorArchivo):
    def __init__(self, archivo_log: str = "archivos.log"):
        self.archivo_log = archivo_log
    
    def notificar_cambio(self, archivo: Archivo, evento: str):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        mensaje = f"[{timestamp}] {evento}: {archivo.ruta}\n"
        
        try:
            with open(self.archivo_log, 'a', encoding='utf-8') as f:
                f.write(mensaje)
        except Exception as e:
            print(f"Error al escribir log: {e}")

class ObservadorConsola(ObservadorArchivo):
    def notificar_cambio(self, archivo: Archivo, evento: str):
        emoji = {
            'creado': '📁',
            'modificado': '✏️',
            'eliminado': '🗑️',
            'procesado': '⚙️',
            'error': '❌'
        }.get(evento, '📋')
        
        print(f"{emoji} {evento.upper()}: {archivo.nombre}")

class ObservadorEstadisticas(ObservadorArchivo):
    def __init__(self):
        self.estadisticas = {
            'archivos_creados': 0,
            'archivos_modificados': 0,
            'archivos_eliminados': 0,
            'archivos_procesados': 0,
            'errores': 0
        }
    
    def notificar_cambio(self, archivo: Archivo, evento: str):
        if evento in self.estadisticas:
            self.estadisticas[evento] += 1
        
        # Mostrar estadísticas cada 10 eventos
        total = sum(self.estadisticas.values())
        if total % 10 == 0:
            self.mostrar_estadisticas()
    
    def mostrar_estadisticas(self):
        print("\n📊 ESTADÍSTICAS DEL SISTEMA:")
        for evento, cantidad in self.estadisticas.items():
            print(f"  {evento.replace('_', ' ').title()}: {cantidad}")
        print()
```

### **5. Procesadores Funcionales**

```python
from functools import reduce
from typing import Callable, List, Dict, Any

class ProcesadorFuncional:
    @staticmethod
    def aplicar_filtros(archivos: List[Archivo], *filtros: Callable[[Archivo], bool]) -> List[Archivo]:
        """Aplica múltiples filtros a una lista de archivos"""
        def aplicar_filtro(lista: List[Archivo], filtro: Callable[[Archivo], bool]) -> List[Archivo]:
            return list(filter(filtro, lista))
        
        return reduce(aplicar_filtro, filtros, archivos)
    
    @staticmethod
    def aplicar_transformaciones(archivos: List[Archivo], *transformaciones: Callable[[Archivo], Any]) -> List[Any]:
        """Aplica múltiples transformaciones a una lista de archivos"""
        def aplicar_transformacion(lista: List[Any], transformacion: Callable[[Archivo], Any]) -> List[Any]:
            return [transformacion(archivo) for archivo in archivos]
        
        return reduce(aplicar_transformacion, transformaciones, archivos)
    
    @staticmethod
    def agrupar_por_extension(archivos: List[Archivo]) -> Dict[str, List[Archivo]]:
        """Agrupa archivos por extensión"""
        def agregar_a_grupo(grupos: Dict[str, List[Archivo]], archivo: Archivo) -> Dict[str, List[Archivo]]:
            extension = archivo.extension.lower()
            if extension not in grupos:
                grupos[extension] = []
            grupos[extension].append(archivo)
            return grupos
        
        return reduce(agregar_a_grupo, archivos, {})
    
    @staticmethod
    def agrupar_por_tamano(archivos: List[Archivo], rangos: List[tuple]) -> Dict[str, List[Archivo]]:
        """Agrupa archivos por rangos de tamaño"""
        def clasificar_por_tamano(archivo: Archivo) -> str:
            tamano_kb = archivo.tamano / 1024
            for rango_min, rango_max, etiqueta in rangos:
                if rango_min <= tamano_kb < rango_max:
                    return etiqueta
            return "Muy grande"
        
        def agregar_a_grupo_tamano(grupos: Dict[str, List[Archivo]], archivo: Archivo) -> Dict[str, List[Archivo]]:
            categoria = clasificar_por_tamano(archivo)
            if categoria not in grupos:
                grupos[categoria] = []
            grupos[categoria].append(archivo)
            return grupos
        
        return reduce(agregar_a_grupo_tamano, archivos, {})

# Funciones de filtrado
def filtro_por_extension(extension: str) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.extension.lower() == extension.lower()

def filtro_por_tamano_minimo(tamano_minimo: int) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.tamano >= tamano_minimo

def filtro_por_tamano_maximo(tamano_maximo: int) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.tamano <= tamano_maximo

def filtro_por_fecha_reciente(dias: int) -> Callable[[Archivo], bool]:
    from datetime import timedelta
    fecha_limite = datetime.now() - timedelta(days=dias)
    return lambda archivo: archivo.fecha_modificacion >= fecha_limite

# Funciones de transformación
def transformar_a_info(archivo: Archivo) -> str:
    return f"{archivo.nombre} ({archivo.extension}) - {archivo.tamano} bytes"

def transformar_a_dict(archivo: Archivo) -> Dict[str, Any]:
    return {
        "nombre": archivo.nombre,
        "extension": archivo.extension,
        "tamano": archivo.tamano,
        "fecha_modificacion": archivo.fecha_modificacion.isoformat(),
        "hash": archivo.hash_md5[:8]
    }

def transformar_a_tamano_kb(archivo: Archivo) -> float:
    return archivo.tamano / 1024
```

### **6. Gestor Principal de Archivos**

```python
import logging
from dataclasses import dataclass
from datetime import datetime
from typing import List, Dict, Optional, Any, Callable
from pathlib import Path
import hashlib
import os

@dataclass
class Archivo:
    ruta: Path
    nombre: str
    extension: str
    tamano: int
    fecha_creacion: datetime
    fecha_modificacion: datetime
    hash_md5: str
    contenido: Optional[str] = None
    
    def __post_init__(self):
        if not self.nombre:
            self.nombre = self.ruta.name
        if not self.extension:
            self.extension = self.ruta.suffix
        if not self.hash_md5:
            self.hash_md5 = self._calcular_hash()
    
    def _calcular_hash(self) -> str:
        try:
            with open(self.ruta, 'rb') as f:
                contenido = f.read()
                return hashlib.md5(contenido).hexdigest()
        except Exception:
            return ""
    
    def obtener_info(self) -> str:
        return (f"Archivo: {self.nombre} "
                f"({self.extension}) - "
                f"Tamaño: {self.tamano} bytes - "
                f"Hash: {self.hash_md5[:8]}...")
    
    def es_texto(self) -> bool:
        extensiones_texto = {'.txt', '.md', '.py', '.js', '.html', '.css', '.json', '.xml'}
        return self.extension.lower() in extensiones_texto
    
    def es_imagen(self) -> bool:
        extensiones_imagen = {'.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg'}
        return self.extension.lower() in extensiones_imagen
    
    def es_documento(self) -> bool:
        extensiones_documento = {'.pdf', '.doc', '.docx', '.xls', '.xlsx', '.ppt', '.pptx'}
        return self.extension.lower() in extensiones_documento

@dataclass
class Directorio:
    ruta: Path
    nombre: str
    archivos: List[Archivo]
    subdirectorios: List['Directorio']
    fecha_escaneo: datetime
    
    def __post_init__(self):
        if not self.nombre:
            self.nombre = self.ruta.name
        if not self.fecha_escaneo:
            self.fecha_escaneo = datetime.now()
    
    def obtener_total_archivos(self) -> int:
        total = len(self.archivos)
        for subdir in self.subdirectorios:
            total += subdir.obtener_total_archivos()
        return total
    
    def obtener_tamano_total(self) -> int:
        tamano = sum(archivo.tamano for archivo in self.archivos)
        for subdir in self.subdirectorios:
            tamano += subdir.obtener_tamano_total()
        return tamano
    
    def obtener_info(self) -> str:
        return (f"Directorio: {self.nombre} - "
                f"Archivos: {self.obtener_total_archivos()} - "
                f"Tamaño: {self.obtener_tamano_total()} bytes")

@dataclass
class OperacionArchivo:
    id: str
    tipo: str  # 'copiar', 'mover', 'eliminar', 'procesar'
    archivo_origen: Archivo
    archivo_destino: Optional[Archivo]
    fecha_operacion: datetime
    estado: str  # 'pendiente', 'en_proceso', 'completada', 'fallida'
    resultado: Optional[str] = None
    
    def __post_init__(self):
        if not self.fecha_operacion:
            self.fecha_operacion = datetime.now()
    
    def obtener_info(self) -> str:
        return (f"Operación {self.tipo} - "
                f"Archivo: {self.archivo_origen.nombre} - "
                f"Estado: {self.estado}")

class GestorObservadores:
    def __init__(self):
        self.observadores: List[ObservadorArchivo] = []
    
    def agregar_observador(self, observador: ObservadorArchivo):
        if observador not in self.observadores:
            self.observadores.append(observador)
    
    def remover_observador(self, observador: ObservadorArchivo):
        if observador in self.observadores:
            self.observadores.remove(observador)
    
    def notificar_todos(self, archivo: Archivo, evento: str):
        for observador in self.observadores:
            observador.notificar_cambio(archivo, evento)

class ObservadorLog(ObservadorArchivo):
    def __init__(self, archivo_log: str = "archivos.log"):
        self.archivo_log = archivo_log
    
    def notificar_cambio(self, archivo: Archivo, evento: str):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        mensaje = f"[{timestamp}] {evento}: {archivo.ruta}\n"
        
        try:
            with open(self.archivo_log, 'a', encoding='utf-8') as f:
                f.write(mensaje)
        except Exception as e:
            print(f"Error al escribir log: {e}")

class ObservadorConsola(ObservadorArchivo):
    def notificar_cambio(self, archivo: Archivo, evento: str):
        emoji = {
            'creado': '📁',
            'modificado': '✏️',
            'eliminado': '🗑️',
            'procesado': '⚙️',
            'error': '❌'
        }.get(evento, '📋')
        
        print(f"{emoji} {evento.upper()}: {archivo.nombre}")

class ObservadorEstadisticas(ObservadorArchivo):
    def __init__(self):
        self.estadisticas = {
            'archivos_creados': 0,
            'archivos_modificados': 0,
            'archivos_eliminados': 0,
            'archivos_procesados': 0,
            'errores': 0
        }
    
    def notificar_cambio(self, archivo: Archivo, evento: str):
        if evento in self.estadisticas:
            self.estadisticas[evento] += 1
        
        # Mostrar estadísticas cada 10 eventos
        total = sum(self.estadisticas.values())
        if total % 10 == 0:
            self.mostrar_estadisticas()
    
    def mostrar_estadisticas(self):
        print("\n📊 ESTADÍSTICAS DEL SISTEMA:")
        for evento, cantidad in self.estadisticas.items():
            print(f"  {evento.replace('_', ' ').title()}: {cantidad}")
        print()

class ProcesadorArchivo(ABC):
    @abstractmethod
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        pass
    
    @abstractmethod
    def obtener_nombre(self) -> str:
        pass

class ValidadorArchivo(ABC):
    @abstractmethod
    def validar(self, archivo: Archivo) -> bool:
        pass
    
    @abstractmethod
    def obtener_errores(self) -> List[str]:
        pass

class ObservadorArchivo(ABC):
    @abstractmethod
    def notificar_cambio(self, archivo: Archivo, evento: str):
        pass

class RepositorioArchivo(ABC):
    @abstractmethod
    def guardar(self, archivo: Archivo) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_ruta(self, ruta: str) -> Optional[Archivo]:
        pass
    
    @abstractmethod
    def listar_todos(self) -> List[Archivo]:
        pass

class ServicioNotificacion(ABC):
    @abstractmethod
    def enviar_notificacion(self, mensaje: str, tipo: str = "info"):
        pass

class ProcesadorTexto(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_texto():
            return {"error": "Archivo no es de texto"}
        
        try:
            with open(archivo.ruta, 'r', encoding='utf-8') as f:
                contenido = f.read()
            
            resultado = {
                "lineas": len(contenido.splitlines()),
                "palabras": len(contenido.split()),
                "caracteres": len(contenido),
                "caracteres_sin_espacios": len(contenido.replace(" ", "")),
                "palabras_unicas": len(set(contenido.lower().split())),
                "extension": archivo.extension,
                "tamano_kb": archivo.tamano / 1024
            }
            
            return resultado
            
        except Exception as e:
            return {"error": str(e)}
    
    def obtener_nombre(self) -> str:
        return "Procesador de Texto"

class ProcesadorImagen(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_imagen():
            return {"error": "Archivo no es una imagen"}
        
        # Simulación de procesamiento de imagen
        resultado = {
            "tipo": "imagen",
            "extension": archivo.extension,
            "tamano_mb": archivo.tamano / (1024 * 1024),
            "formato": archivo.extension[1:].upper(),
            "compresion": "JPEG" if archivo.extension.lower() in ['.jpg', '.jpeg'] else "Otro"
        }
        
        return resultado
    
    def obtener_nombre(self) -> str:
        return "Procesador de Imagen"

class ProcesadorDocumento(ProcesadorArchivo):
    def __init__(self):
        self.errores = []
    
    def procesar(self, archivo: Archivo) -> Dict[str, Any]:
        if not archivo.es_documento():
            return {"error": "Archivo no es un documento"}
        
        resultado = {
            "tipo": "documento",
            "extension": archivo.extension,
            "tamano_mb": archivo.tamano / (1024 * 1024),
            "categoria": self._categorizar_documento(archivo.extension),
            "version": "1.0"
        }
        
        return resultado
    
    def _categorizar_documento(self, extension: str) -> str:
        categorias = {
            '.pdf': 'PDF',
            '.doc': 'Word',
            '.docx': 'Word',
            '.xls': 'Excel',
            '.xlsx': 'Excel',
            '.ppt': 'PowerPoint',
            '.pptx': 'PowerPoint'
        }
        return categorias.get(extension.lower(), 'Desconocido')
    
    def obtener_nombre(self) -> str:
        return "Procesador de Documento"

class ProcesadorFuncional:
    @staticmethod
    def aplicar_filtros(archivos: List[Archivo], *filtros: Callable[[Archivo], bool]) -> List[Archivo]:
        """Aplica múltiples filtros a una lista de archivos"""
        def aplicar_filtro(lista: List[Archivo], filtro: Callable[[Archivo], bool]) -> List[Archivo]:
            return list(filter(filtro, lista))
        
        return reduce(aplicar_filtro, filtros, archivos)
    
    @staticmethod
    def aplicar_transformaciones(archivos: List[Archivo], *transformaciones: Callable[[Archivo], Any]) -> List[Any]:
        """Aplica múltiples transformaciones a una lista de archivos"""
        def aplicar_transformacion(lista: List[Any], transformacion: Callable[[Archivo], Any]) -> List[Any]:
            return [transformacion(archivo) for archivo in archivos]
        
        return reduce(aplicar_transformacion, transformaciones, archivos)
    
    @staticmethod
    def agrupar_por_extension(archivos: List[Archivo]) -> Dict[str, List[Archivo]]:
        """Agrupa archivos por extensión"""
        def agregar_a_grupo(grupos: Dict[str, List[Archivo]], archivo: Archivo) -> Dict[str, List[Archivo]]:
            extension = archivo.extension.lower()
            if extension not in grupos:
                grupos[extension] = []
            grupos[extension].append(archivo)
            return grupos
        
        return reduce(agregar_a_grupo, archivos, {})
    
    @staticmethod
    def agrupar_por_tamano(archivos: List[Archivo], rangos: List[tuple]) -> Dict[str, List[Archivo]]:
        """Agrupa archivos por rangos de tamaño"""
        def clasificar_por_tamano(archivo: Archivo) -> str:
            tamano_kb = archivo.tamano / 1024
            for rango_min, rango_max, etiqueta in rangos:
                if rango_min <= tamano_kb < rango_max:
                    return etiqueta
            return "Muy grande"
        
        def agregar_a_grupo_tamano(grupos: Dict[str, List[Archivo]], archivo: Archivo) -> Dict[str, List[Archivo]]:
            categoria = clasificar_por_tamano(archivo)
            if categoria not in grupos:
                grupos[categoria] = []
            grupos[categoria].append(archivo)
            return grupos
        
        return reduce(agregar_a_grupo_tamano, archivos, {})

# Funciones de filtrado
def filtro_por_extension(extension: str) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.extension.lower() == extension.lower()

def filtro_por_tamano_minimo(tamano_minimo: int) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.tamano >= tamano_minimo

def filtro_por_tamano_maximo(tamano_maximo: int) -> Callable[[Archivo], bool]:
    return lambda archivo: archivo.tamano <= tamano_maximo

def filtro_por_fecha_reciente(dias: int) -> Callable[[Archivo], bool]:
    from datetime import timedelta
    fecha_limite = datetime.now() - timedelta(days=dias)
    return lambda archivo: archivo.fecha_modificacion >= fecha_limite

# Funciones de transformación
def transformar_a_info(archivo: Archivo) -> str:
    return f"{archivo.nombre} ({archivo.extension}) - {archivo.tamano} bytes"

def transformar_a_dict(archivo: Archivo) -> Dict[str, Any]:
    return {
        "nombre": archivo.nombre,
        "extension": archivo.extension,
        "tamano": archivo.tamano,
        "fecha_modificacion": archivo.fecha_modificacion.isoformat(),
        "hash": archivo.hash_md5[:8]
    }

def transformar_a_tamano_kb(archivo: Archivo) -> float:
    return archivo.tamano / 1024

class GestorArchivos:
    def __init__(self, directorio_raiz: str):
        self.directorio_raiz = Path(directorio_raiz)
        self.observadores = GestorObservadores()
        self.procesadores = {
            'texto': ProcesadorTexto(),
            'imagen': ProcesadorImagen(),
            'documento': ProcesadorDocumento()
        }
        self.procesador_funcional = ProcesadorFuncional()
        
        # Agregar observadores por defecto
        self.observadores.agregar_observador(ObservadorConsola())
        self.observadores.agregar_observador(ObservadorLog())
        self.observadores.agregar_observador(ObservadorEstadisticas())
        
        self.logger = logging.getLogger(__name__)
    
    def escanear_directorio(self, ruta: str = None) -> Directorio:
        """Escanea un directorio y retorna su estructura"""
        ruta_escaneo = Path(ruta) if ruta else self.directorio_raiz
        
        if not ruta_escaneo.exists() or not ruta_escaneo.is_dir():
            raise ValueError(f"Directorio no válido: {ruta_escaneo}")
        
        return self._escanear_recursivo(ruta_escaneo)
    
    def _escanear_recursivo(self, ruta: Path) -> Directorio:
        """Escanea recursivamente un directorio"""
        archivos = []
        subdirectorios = []
        
        try:
            for item in ruta.iterdir():
                if item.is_file():
                    archivo = self._crear_archivo(item)
                    archivos.append(archivo)
                    self.observadores.notificar_todos(archivo, 'creado')
                
                elif item.is_dir():
                    subdir = self._escanear_recursivo(item)
                    subdirectorios.append(subdir)
        
        except PermissionError:
            self.logger.warning(f"Sin permisos para acceder a: {ruta}")
        except Exception as e:
            self.logger.error(f"Error al escanear {ruta}: {e}")
        
        directorio = Directorio(
            ruta=ruta,
            nombre=ruta.name,
            archivos=archivos,
            subdirectorios=subdirectorios,
            fecha_escaneo=datetime.now()
        )
        
        return directorio
    
    def _crear_archivo(self, ruta: Path) -> Archivo:
        """Crea un objeto Archivo desde una ruta"""
        try:
            stat = ruta.stat()
            return Archivo(
                ruta=ruta,
                nombre=ruta.name,
                extension=ruta.suffix,
                tamano=stat.st_size,
                fecha_creacion=datetime.fromtimestamp(stat.st_ctime),
                fecha_modificacion=datetime.fromtimestamp(stat.st_mtime),
                hash_md5=""
            )
        except Exception as e:
            self.logger.error(f"Error al crear archivo {ruta}: {e}")
            raise
    
    def procesar_archivos(self, archivos: List[Archivo], tipo_procesador: str = None) -> List[Dict[str, Any]]:
        """Procesa una lista de archivos usando el procesador apropiado"""
        resultados = []
        
        for archivo in archivos:
            try:
                if tipo_procesador and tipo_procesador in self.procesadores:
                    procesador = self.procesadores[tipo_procesador]
                    resultado = procesador.procesar(archivo)
                else:
                    # Auto-detectar tipo de archivo
                    if archivo.es_texto():
                        procesador = self.procesadores['texto']
                    elif archivo.es_imagen():
                        procesador = self.procesadores['imagen']
                    elif archivo.es_documento():
                        procesador = self.procesadores['documento']
                    else:
                        resultado = {"tipo": "desconocido", "extension": archivo.extension}
                        resultados.append(resultado)
                        continue
                    
                    resultado = procesador.procesar(archivo)
                
                resultado['archivo'] = archivo.nombre
                resultados.append(resultado)
                
                self.observadores.notificar_todos(archivo, 'procesado')
                
            except Exception as e:
                resultado = {"archivo": archivo.nombre, "error": str(e)}
                resultados.append(resultado)
                self.observadores.notificar_todos(archivo, 'error')
        
        return resultados
    
    def buscar_archivos(self, directorio: Directorio, *filtros: Callable[[Archivo], bool]) -> List[Archivo]:
        """Busca archivos en un directorio aplicando filtros"""
        archivos = self._obtener_todos_archivos(directorio)
        return self.procesador_funcional.aplicar_filtros(archivos, *filtros)
    
    def _obtener_todos_archivos(self, directorio: Directorio) -> List[Archivo]:
        """Obtiene todos los archivos de un directorio recursivamente"""
        archivos = directorio.archivos.copy()
        for subdir in directorio.subdirectorios:
            archivos.extend(self._obtener_todos_archivos(subdir))
        return archivos
    
    def generar_reporte(self, directorio: Directorio) -> str:
        """Genera un reporte completo del directorio"""
        archivos = self._obtener_todos_archivos(directorio)
        
        # Agrupar por extensión
        grupos_extension = self.procesador_funcional.agrupar_por_extension(archivos)
        
        # Agrupar por tamaño
        rangos_tamano = [
            (0, 1, "Muy pequeño (< 1 KB)"),
            (1, 100, "Pequeño (1-100 KB)"),
            (100, 1024, "Mediano (100 KB - 1 MB)"),
            (1024, 10240, "Grande (1-10 MB)"),
            (10240, float('inf'), "Muy grande (> 10 MB)")
        ]
        grupos_tamano = self.procesador_funcional.agrupar_por_tamano(archivos, rangos_tamano)
        
        # Generar reporte
        reporte = f"""
=== REPORTE DEL DIRECTORIO: {directorio.nombre} ===

📁 INFORMACIÓN GENERAL:
  - Ruta: {directorio.ruta}
  - Total archivos: {directorio.obtener_total_archivos()}
  - Tamaño total: {directorio.obtener_tamano_total() / (1024*1024):.2f} MB
  - Fecha de escaneo: {directorio.fecha_escaneo.strftime('%Y-%m-%d %H:%M:%S')}

📊 DISTRIBUCIÓN POR EXTENSIÓN:
"""
        
        for extension, archivos_grupo in grupos_extension.items():
            tamano_total = sum(a.tamano for a in archivos_grupo)
            reporte += f"  {extension}: {len(archivos_grupo)} archivos ({tamano_total / 1024:.2f} KB)\n"
        
        reporte += "\n📏 DISTRIBUCIÓN POR TAMAÑO:\n"
        for categoria, archivos_grupo in grupos_tamano.items():
            reporte += f"  {categoria}: {len(archivos_grupo)} archivos\n"
        
        # Top 5 archivos más grandes
        archivos_ordenados = sorted(archivos, key=lambda x: x.tamano, reverse=True)[:5]
        reporte += "\n🔝 TOP 5 ARCHIVOS MÁS GRANDES:\n"
        for i, archivo in enumerate(archivos_ordenados, 1):
            tamano_mb = archivo.tamano / (1024 * 1024)
            reporte += f"  {i}. {archivo.nombre} ({tamano_mb:.2f} MB)\n"
        
        return reporte
    
    def agregar_observador(self, observador: ObservadorArchivo):
        """Agrega un nuevo observador"""
        self.observadores.agregar_observador(observador)
    
    def remover_observador(self, observador: ObservadorArchivo):
        """Remueve un observador"""
        self.observadores.remover_observador(observador)
```

### **7. Ejemplo de Uso**

```python
import logging
from datetime import datetime
from typing import List, Dict, Optional, Any, Callable
from pathlib import Path
import hashlib
import os

def main():
    # Configurar logging
    logging.basicConfig(level=logging.INFO)
    
    print("📁 GESTOR DE ARCHIVOS INICIADO\n")
    
    # Crear gestor de archivos
    gestor = GestorArchivos(".")
    
    try:
        # Escanear directorio actual
        print("🔍 Escaneando directorio...")
        directorio = gestor.escanear_directorio()
        print(f"✅ Escaneo completado: {directorio.obtener_total_archivos()} archivos encontrados\n")
        
        # Generar reporte
        print("📊 Generando reporte...")
        reporte = gestor.generar_reporte(directorio)
        print(reporte)
        
        # Buscar archivos específicos
        print("🔍 Buscando archivos Python...")
        archivos_python = gestor.buscar_archivos(
            directorio, 
            filtro_por_extension('.py')
        )
        print(f"📝 Archivos Python encontrados: {len(archivos_python)}")
        
        # Buscar archivos grandes
        print("\n🔍 Buscando archivos grandes (> 1 MB)...")
        archivos_grandes = gestor.buscar_archivos(
            directorio,
            filtro_por_tamano_minimo(1024 * 1024)
        )
        print(f"📦 Archivos grandes encontrados: {len(archivos_grandes)}")
        
        # Procesar archivos de texto
        print("\n⚙️ Procesando archivos de texto...")
        archivos_texto = gestor.buscar_archivos(
            directorio,
            lambda a: a.es_texto()
        )
        resultados = gestor.procesar_archivos(archivos_texto[:3])  # Solo los primeros 3
        
        print("📊 Resultados del procesamiento:")
        for resultado in resultados:
            if 'error' not in resultado:
                print(f"  {resultado['archivo']}: {resultado['lineas']} líneas, {resultado['palabras']} palabras")
            else:
                print(f"  {resultado['archivo']}: Error - {resultado['error']}")
        
        # Agrupar archivos por extensión
        print("\n📂 Agrupando archivos por extensión...")
        archivos = gestor._obtener_todos_archivos(directorio)
        grupos = gestor.procesador_funcional.agrupar_por_extension(archivos)
        
        print("📊 Grupos por extensión:")
        for extension, archivos_grupo in grupos.items():
            print(f"  {extension}: {len(archivos_grupo)} archivos")
        
    except Exception as e:
        print(f"❌ Error: {e}")

if __name__ == "__main__":
    main()
```

## 🎯 Características del Proyecto

### **Paradigmas Implementados**
- **POO**: Clases, herencia, encapsulamiento
- **Funcional**: Funciones puras, map/filter/reduce, composición
- **Reactivo**: Sistema de observadores, notificaciones en tiempo real
- **Patrones**: Strategy, Observer, Factory

### **Funcionalidades**
- ✅ Escaneo recursivo de directorios
- ✅ Procesamiento inteligente de archivos
- ✅ Sistema de filtros y búsquedas
- ✅ Agrupación y análisis de archivos
- ✅ Monitoreo en tiempo real
- ✅ Generación de reportes
- ✅ Logging y estadísticas

### **Extensibilidad**
- Fácil agregar nuevos tipos de procesadores
- Nuevos observadores para diferentes eventos
- Filtros personalizables
- Transformaciones flexibles

## 🚀 Mejoras Futuras

1. **Interfaz Gráfica**: GUI con tkinter o web con Flask
2. **Base de Datos**: Persistencia de metadatos
3. **Compresión**: Comprimir archivos automáticamente
4. **Backup**: Sistema de respaldos automáticos
5. **Sincronización**: Sincronizar entre directorios
6. **Plugins**: Sistema de plugins para procesadores
7. **API REST**: Endpoints para integración

---

**Siguiente**: [03. Proyecto Simulador de Eventos](./03-proyecto-simulador-eventos.md) 