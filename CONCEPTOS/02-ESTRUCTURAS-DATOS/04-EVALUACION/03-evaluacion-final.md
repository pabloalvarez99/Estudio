# 📊 Evaluación Final: Estructuras de Datos y Algoritmos

## 🎯 Objetivo de la Evaluación
Esta evaluación final te permitirá consolidar todo lo aprendido en el segundo concepto del curso, demostrando tu dominio de estructuras de datos, algoritmos y su implementación práctica en Python.

## ⏱️ Tiempo Estimado: 90-120 minutos

---

## 📚 **SECCIÓN 1: EVALUACIÓN TEÓRICA COMPREHENSIVA**

### **Pregunta 1: Fundamentos de Estructuras de Datos**
**Explica en detalle las diferencias entre las siguientes estructuras de datos en Python:**

1. **Listas vs Arrays tradicionales**
2. **Tuplas vs Sets**
3. **Diccionarios vs Listas de tuplas**

**Tu respuesta:**
```
[Escribe aquí tu análisis detallado]
```

**Puntos clave esperados:**
- **Listas vs Arrays**: Mutabilidad, tipos heterogéneos, métodos incorporados, overhead de memoria
- **Tuplas vs Sets**: Inmutabilidad vs mutabilidad, orden vs no orden, hashable vs no hashable
- **Diccionarios vs Listas de tuplas**: Acceso por clave vs acceso por índice, eficiencia de búsqueda

**Puntuación**: ___/15

---

### **Pregunta 2: Análisis de Complejidad**
**Analiza la complejidad temporal y espacial de los siguientes algoritmos:**

```python
def algoritmo_1(lista):
    resultado = []
    for i in range(len(lista)):
        for j in range(i, len(lista)):
            resultado.append(lista[i:j+1])
    return resultado

def algoritmo_2(lista):
    if len(lista) <= 1:
        return lista
    medio = len(lista) // 2
    izquierda = algoritmo_2(lista[:medio])
    derecha = algoritmo_2(lista[medio:])
    return ordenar_combininar(izquierda, derecha)

def algoritmo_3(lista, elemento):
    inicio = 0
    fin = len(lista) - 1
    while inicio <= fin:
        medio = (inicio + fin) // 2
        if lista[medio] == elemento:
            return medio
        elif lista[medio] < elemento:
            inicio = medio + 1
        else:
            fin = medio - 1
    return -1
```

**Tu análisis:**
```
[Escribe aquí tu análisis de complejidad]
```

**Puntuación**: ___/15

---

### **Pregunta 3: Casos de Uso y Selección**
**Para cada escenario, selecciona la estructura de datos más apropiada y justifica tu elección:**

1. **Sistema de caché** que debe mantener solo los 100 elementos más recientemente usados
2. **Base de datos simple** que almacena información de usuarios con búsqueda por ID
3. **Sistema de etiquetas** donde cada elemento puede tener múltiples etiquetas y necesitas encontrar elementos por etiqueta
4. **Cola de prioridad** donde los elementos se procesan según su prioridad
5. **Índice de búsqueda** que mapea palabras a las páginas donde aparecen

**Tu respuesta:**
```
[Escribe aquí tu análisis y justificación]
```

**Puntuación**: ___/20

---

## 🛠️ **SECCIÓN 2: IMPLEMENTACIÓN PRÁCTICA AVANZADA**

### **Ejercicio 1: Sistema de Gestión de Biblioteca**
**Implementa un sistema completo de gestión de biblioteca con las siguientes funcionalidades:**

```python
class Biblioteca:
    def __init__(self):
        """
        Inicializa la biblioteca con:
        - libros: diccionario de libros por ID
        - usuarios: diccionario de usuarios por ID
        - prestamos: diccionario de préstamos activos
        - categorias: set de categorías disponibles
        """
        pass
    
    def agregar_libro(self, titulo, autor, categoria, isbn, copias=1):
        """Agrega un nuevo libro a la biblioteca"""
        pass
    
    def registrar_usuario(self, nombre, email, tipo="estudiante"):
        """Registra un nuevo usuario"""
        pass
    
    def prestar_libro(self, usuario_id, libro_id):
        """Presta un libro a un usuario"""
        pass
    
    def devolver_libro(self, usuario_id, libro_id):
        """Devuelve un libro prestado"""
        pass
    
    def buscar_libros(self, criterio, valor):
        """Busca libros por diferentes criterios"""
        pass
    
    def generar_reporte_prestamos(self):
        """Genera un reporte de préstamos activos"""
        pass
    
    def estadisticas_biblioteca(self):
        """Retorna estadísticas generales de la biblioteca"""
        pass

# Implementa también las clases Libro, Usuario y Prestamo
```

**Tu implementación:**
```python
# Escribe aquí tu implementación completa
```

**Criterios de evaluación:**
- [ ] Estructura de clases bien diseñada (5 puntos)
- [ ] Funcionalidades CRUD implementadas (10 puntos)
- [ ] Búsqueda y filtrado eficiente (5 puntos)
- [ ] Manejo de errores y validaciones (5 puntos)
- [ ] Reportes y estadísticas (5 puntos)

**Puntuación Ejercicio 1**: ___/30

---

### **Ejercicio 2: Algoritmo de Ordenamiento Personalizado**
**Implementa un algoritmo de ordenamiento híbrido que combine las ventajas de diferentes algoritmos:**

```python
def ordenamiento_hibrido(lista, umbral=10):
    """
    Algoritmo híbrido que:
    - Usa quicksort para listas grandes (> umbral)
    - Usa insertion sort para listas pequeñas (≤ umbral)
    - Optimiza el pivote del quicksort usando mediana de tres
    """
    pass

def quicksort_optimizado(lista, inicio, fin):
    """Implementación optimizada de quicksort"""
    pass

def insertion_sort(lista):
    """Implementación de insertion sort"""
    pass

def mediana_de_tres(lista, inicio, fin):
    """Encuentra la mediana de tres elementos para optimizar el pivote"""
    pass

# Prueba el algoritmo
numeros = [64, 34, 25, 12, 22, 11, 90, 88, 76, 54, 32, 19, 87, 65, 43]
print("Lista original:", numeros)
ordenados = ordenamiento_hibrido(numeros.copy())
print("Lista ordenada:", ordenados)
```

**Tu implementación:**
```python
# Escribe aquí tu implementación completa
```

**Criterios de evaluación:**
- [ ] Algoritmo híbrido funcional (10 puntos)
- [ ] Quicksort optimizado implementado (10 puntos)
- [ ] Insertion sort implementado (5 puntos)
- [ ] Selección de pivote optimizada (5 puntos)

**Puntuación Ejercicio 2**: ___/30

---

## 🧠 **SECCIÓN 3: PROBLEMAS COMPLEJOS DE ALGORITMOS**

### **Problema 1: Sistema de Recomendaciones**
**Implementa un sistema simple de recomendaciones basado en similitud de usuarios:**

```python
class SistemaRecomendaciones:
    def __init__(self):
        """
        Sistema que recomienda productos basándose en:
        - Historial de compras de usuarios
        - Similitud entre usuarios
        - Calificaciones de productos
        """
        self.usuarios = {}  # ID usuario -> {productos: calificaciones}
        self.productos = {}  # ID producto -> {usuarios: calificaciones}
    
    def agregar_calificacion(self, usuario_id, producto_id, calificacion):
        """Agrega una calificación de usuario a producto"""
        pass
    
    def calcular_similitud(self, usuario1_id, usuario2_id):
        """Calcula similitud entre dos usuarios usando correlación de Pearson"""
        pass
    
    def encontrar_usuarios_similares(self, usuario_id, n=5):
        """Encuentra los n usuarios más similares"""
        pass
    
    def generar_recomendaciones(self, usuario_id, n=5):
        """Genera recomendaciones para un usuario"""
        pass
    
    def predecir_calificacion(self, usuario_id, producto_id):
        """Predice la calificación que daría un usuario a un producto"""
        pass

# Implementa el sistema y pruébalo con datos de ejemplo
```

**Tu implementación:**
```python
# Escribe aquí tu implementación completa
```

**Criterios de evaluación:**
- [ ] Cálculo de similitud implementado (10 puntos)
- [ ] Algoritmo de recomendaciones funcional (10 puntos)
- [ ] Predicción de calificaciones (5 puntos)
- [ ] Manejo de casos edge (5 puntos)

**Puntuación Problema 1**: ___/30

---

### **Problema 2: Optimización de Memoria**
**Implementa un sistema de gestión de memoria que optimice el uso de estructuras de datos:**

```python
class GestorMemoria:
    def __init__(self, capacidad_maxima=1000):
        """
        Sistema que:
        - Monitorea el uso de memoria de diferentes estructuras
        - Optimiza automáticamente el almacenamiento
        - Implementa estrategias de limpieza
        """
        self.estructuras = {}  # nombre -> {datos, tipo, tamaño_estimado}
        self.capacidad_maxima = capacidad_maxima
        self.uso_actual = 0
    
    def agregar_estructura(self, nombre, datos, tipo_estructura):
        """Agrega una nueva estructura de datos al gestor"""
        pass
    
    def optimizar_estructura(self, nombre):
        """Optimiza una estructura específica según su tipo"""
        pass
    
    def limpiar_memoria(self):
        """Limpia estructuras poco usadas o redundantes"""
        pass
    
    def reporte_memoria(self):
        """Genera reporte detallado del uso de memoria"""
        pass
    
    def sugerir_optimizaciones(self):
        """Sugiere optimizaciones basadas en el uso actual"""
        pass

# Implementa estrategias de optimización para diferentes tipos de estructuras
```

**Tu implementación:**
```python
# Escribe aquí tu implementación completa
```

**Criterios de evaluación:**
- [ ] Monitoreo de memoria implementado (10 puntos)
- [ ] Estrategias de optimización (10 puntos)
- [ ] Sistema de limpieza automática (5 puntos)
- [ ] Reportes y sugerencias (5 puntos)

**Puntuación Problema 2**: ___/30

---

## 📊 **EVALUACIÓN FINAL COMPREHENSIVA**

### **Resumen de Puntuaciones**
- **Evaluación Teórica**: ___/50
- **Implementación Práctica**: ___/60
- **Problemas Complejos**: ___/60

**PUNTUACIÓN TOTAL FINAL**: ___/170

---

## 🎯 **CRITERIOS DE EVALUACIÓN FINAL**

### **Nivel de Dominio**
- **153-170 puntos**: 🏆 **MAESTRO** - Dominas completamente todos los conceptos y aplicaciones
- **136-152 puntos**: 🥇 **AVANZADO** - Tienes un dominio sólido de la mayoría de conceptos
- **119-135 puntos**: 🥈 **INTERMEDIO** - Tienes un buen dominio de los conceptos básicos
- **102-118 puntos**: 🥉 **BÁSICO** - Entiendes los conceptos fundamentales
- **Menos de 102 puntos**: 📚 **EN DESARROLLO** - Necesitas más práctica y estudio

---

## 🔍 **ANÁLISIS COMPREHENSIVO DE RESULTADOS**

### **Fortalezas Identificadas**
```
[Escribe aquí las áreas donde demostraste mayor dominio]
```

### **Áreas de Mejora Críticas**
```
[Escribe aquí las áreas que requieren atención inmediata]
```

### **Conceptos que debo dominar antes de continuar**
```
[Escribe aquí los conceptos específicos que debo repasar]
```

---

## 📝 **PLAN DE ACCIÓN POST-EVALUACIÓN**

### **Si tu puntuación es 136+ puntos:**
1. **Revisar implementaciones** para identificar mejoras posibles
2. **Optimizar código** implementando mejores algoritmos
3. **Documentar soluciones** para referencia futura
4. **Continuar** con el siguiente concepto del curso

### **Si tu puntuación es 119-135 puntos:**
1. **Repasar teoría** de las secciones con puntuación baja
2. **Practicar implementaciones** paso a paso
3. **Mejorar algoritmos** existentes
4. **No continuar** hasta alcanzar al menos 136 puntos

### **Si tu puntuación es menor a 119 puntos:**
1. **Repasar completamente** el segundo concepto
2. **Implementar proyectos** desde cero
3. **Practicar ejercicios** adicionales
4. **Buscar ayuda** de recursos externos
5. **No continuar** hasta dominar completamente estos conceptos

---

## 🚀 **RECURSOS DE ESTUDIO AVANZADOS**

### **Para Consolidar Conocimientos:**
- [ ] **Libros Avanzados**: "Introduction to Algorithms" (CLRS)
- [ ] **Cursos Online**: MIT OpenCourseWare, Stanford CS
- [ ] **Competencias**: Codeforces, TopCoder, Google Code Jam
- [ ] **Proyectos Open Source**: Contribuir a proyectos de Python

### **Para Práctica Avanzada:**
- [ ] **Implementar estructuras** desde cero (árboles, grafos, heaps)
- [ ] **Optimizar algoritmos** existentes
- [ ] **Crear bibliotecas** de utilidades
- [ ] **Participar en hackathons** y competencias

---

## 🎯 **PRÓXIMO PASO RECOMENDADO**

### **Si estás listo para continuar:**
**CONCEPTO 3: Paradigmas de Programación**
- Programación Procedural
- Programación Orientada a Objetos
- Programación Funcional
- Programación Event-Driven

### **Si necesitas más práctica:**
1. **Repasar** conceptos del segundo concepto
2. **Implementar** proyectos adicionales
3. **Practicar** con ejercicios más complejos
4. **Tomar** cursos complementarios

---

## 🏆 **LOGROS DEL CONCEPTO COMPLETADO**

### **Al completar exitosamente este concepto has logrado:**
- ✅ **Dominar** las estructuras de datos fundamentales de Python
- ✅ **Implementar** algoritmos complejos y optimizados
- ✅ **Crear** aplicaciones robustas y escalables
- ✅ **Desarrollar** pensamiento algorítmico avanzado
- ✅ **Adquirir** base sólida para conceptos avanzados
- ✅ **Prepararte** para el desarrollo de software profesional

---

## 📚 **REFLEXIONES FINALES**

### **¿Qué aprendiste más durante este concepto?**
```
[Escribe aquí tus aprendizajes más significativos]
```

### **¿Qué te resultó más desafiante?**
```
[Escribe aquí los desafíos que enfrentaste]
```

### **¿Cómo aplicarás estos conocimientos en el futuro?**
```
[Escribe aquí tus planes de aplicación práctica]
```

---

**¡Felicitaciones por completar la evaluación final del segundo concepto! Has demostrado un dominio sólido de estructuras de datos y algoritmos. Continúa tu aprendizaje y recuerda que la práctica constante es la clave del éxito en programación. 🚀** 