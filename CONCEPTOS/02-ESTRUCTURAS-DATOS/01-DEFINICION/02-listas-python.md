# 📖 Definición: Listas en Python

## 🎯 Concepto Principal

### **¿Por qué son Importantes las Listas en Python?**
Las listas son la estructura de datos más versátil y utilizada en Python. Son arrays dinámicos que pueden almacenar elementos de diferentes tipos, cambiar de tamaño automáticamente y proporcionar métodos poderosos para manipulación de datos.

### **Definición Técnica**
Una lista en Python es una colección ordenada, mutable y heterogénea de elementos que se almacenan en posiciones contiguas de memoria, accesibles mediante índices numéricos.

---

## 🔍 ¿Qué es una Lista en Python?

### **Definición Básica**
Una lista es una **colección ordenada** de elementos que:
- Pueden ser de **diferentes tipos** (heterogénea)
- Se pueden **modificar** después de la creación (mutable)
- Mantienen el **orden** de inserción
- Se acceden mediante **índices numéricos**
- Pueden **cambiar de tamaño** dinámicamente

### **Analogía del Mundo Real**
Imagina una **caja de herramientas**:
- La **caja** es la lista
- Cada **herramienta** es un elemento
- Cada **compartimento** tiene un número (índice)
- Puedes **agregar** o **quitar** herramientas
- Las herramientas pueden ser de **diferentes tipos** (martillo, destornillador, llave)

---

## 🏗️ Estructura de una Lista

### **Representación Visual**
```
Índice:    0      1      2      3      4
          ┌──────┬──────┬──────┬──────┬──────┐
Valor:    │ "Ana"│  25  │ True │ 3.14 │ [1,2]│
          └──────┴──────┴──────┴──────┴──────┘
Tipo:     str    int    bool   float  list
```

### **Características Clave**
- **Índice base 0**: El primer elemento está en la posición 0
- **Tamaño dinámico**: Puede crecer o reducirse según sea necesario
- **Acceso directo**: O(1) para acceder a cualquier elemento por índice
- **Heterogénea**: Puede contener elementos de diferentes tipos
- **Mutable**: Los elementos se pueden cambiar, agregar o eliminar

---

## 📊 Tipos de Listas

### **1. Listas Simples (Unidimensionales)**
**Definición**: Listas que almacenan elementos en una sola dimensión

**Ejemplos**:
```python
# Lista de números
numeros = [1, 2, 3, 4, 5]

# Lista de strings
nombres = ["Ana", "Carlos", "María", "Luis"]

# Lista mixta
mixta = [42, "Python", True, 3.14, [1, 2, 3]]

# Lista vacía
vacia = []
```

**Uso común**: Almacenamiento de datos simples, colecciones de elementos

---

### **2. Listas Anidadas (Multidimensionales)**
**Definición**: Listas que contienen otras listas como elementos

**Ejemplos**:
```python
# Lista 2D (matriz)
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Lista de listas de diferentes tamaños
irregular = [
    [1, 2],
    [3, 4, 5],
    [6],
    [7, 8, 9, 10]
]

# Lista 3D
cubo = [
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
]
```

**Uso común**: Matrices, tablas de datos, estructuras jerárquicas

---

### **3. Listas Especializadas**
**Definición**: Listas diseñadas para propósitos específicos

**Ejemplos**:
```python
# Lista de diccionarios (registros)
estudiantes = [
    {"nombre": "Ana", "edad": 20, "nota": 85},
    {"nombre": "Carlos", "edad": 22, "nota": 92},
    {"nombre": "María", "edad": 19, "nota": 78}
]

# Lista de tuplas (coordenadas)
coordenadas = [(0, 0), (1, 1), (2, 2), (3, 3)]

# Lista de funciones
funciones = [len, str, int, float]
```

**Uso común**: Bases de datos simples, estructuras de datos complejas

---

## 🔧 Operaciones Básicas con Listas

### **1. Creación de Listas**
```python
# Creación directa
lista1 = [1, 2, 3, 4, 5]

# Creación con list()
lista2 = list([1, 2, 3, 4, 5])

# Creación con range()
lista3 = list(range(5))  # [0, 1, 2, 3, 4]

# Creación con comprensión
lista4 = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# Creación con multiplicación
lista5 = [0] * 5  # [0, 0, 0, 0, 0]
```

### **2. Acceso a Elementos**
```python
numeros = [10, 25, 42, 17, 33, 89]

# Acceso por índice positivo
primer = numeros[0]      # 10
segundo = numeros[1]     # 25
ultimo = numeros[5]      # 89

# Acceso por índice negativo
ultimo = numeros[-1]     # 89
penultimo = numeros[-2]  # 33
primero = numeros[-6]    # 10

# Slicing (rebanadas)
primeros_tres = numeros[0:3]    # [10, 25, 42]
ultimos_tres = numeros[-3:]     # [17, 33, 89]
todos = numeros[:]              # [10, 25, 42, 17, 33, 89]
```

### **3. Modificación de Elementos**
```python
numeros = [10, 25, 42, 17, 33, 89]

# Cambiar un elemento
numeros[2] = 100    # [10, 25, 100, 17, 33, 89]

# Cambiar múltiples elementos
numeros[1:4] = [50, 60, 70]    # [10, 50, 60, 70, 33, 89]

# Agregar elementos
numeros.append(200)             # [10, 50, 60, 70, 33, 89, 200]
numeros.insert(0, 5)           # [5, 10, 50, 60, 70, 33, 89, 200]
numeros.extend([300, 400])     # [5, 10, 50, 60, 70, 33, 89, 200, 300, 400]
```

### **4. Eliminación de Elementos**
```python
numeros = [10, 25, 42, 17, 33, 89, 42]

# Eliminar por valor
numeros.remove(42)    # Elimina el primer 42

# Eliminar por índice
elemento_eliminado = numeros.pop(2)    # Elimina y retorna el elemento en índice 2

# Eliminar por índice (sin retornar)
del numeros[1]        # Elimina el elemento en índice 1

# Eliminar rango
del numeros[1:4]      # Elimina elementos del índice 1 al 3

# Limpiar toda la lista
numeros.clear()        # []
```

---

## 📈 Métodos de Listas

### **1. Métodos de Agregado**
```python
lista = [1, 2, 3]

# append(): Agregar al final
lista.append(4)        # [1, 2, 3, 4]

# insert(): Insertar en posición específica
lista.insert(1, 10)   # [1, 10, 2, 3, 4]

# extend(): Agregar múltiples elementos
lista.extend([5, 6])  # [1, 10, 2, 3, 4, 5, 6]
```

### **2. Métodos de Eliminación**
```python
lista = [1, 2, 3, 2, 4, 2, 5]

# remove(): Eliminar primera ocurrencia
lista.remove(2)        # [1, 3, 2, 4, 2, 5]

# pop(): Eliminar y retornar por índice
elemento = lista.pop(2)    # elemento = 2, lista = [1, 3, 4, 2, 5]

# clear(): Limpiar toda la lista
lista.clear()           # []
```

### **3. Métodos de Búsqueda**
```python
lista = [10, 25, 42, 17, 33, 89, 42]

# index(): Encontrar índice de un elemento
indice = lista.index(42)    # 2 (primer 42)

# count(): Contar ocurrencias
cantidad = lista.count(42)  # 2

# in: Verificar si existe
existe = 25 in lista        # True
no_existe = 100 in lista    # False
```

### **4. Métodos de Ordenamiento**
```python
lista = [3, 1, 4, 1, 5, 9, 2, 6]

# sort(): Ordenar in-place
lista.sort()           # [1, 1, 2, 3, 4, 5, 6, 9]

# sort() con reverse
lista.sort(reverse=True)   # [9, 6, 5, 4, 3, 2, 1, 1]

# sorted(): Crear nueva lista ordenada
lista_ordenada = sorted(lista)    # No modifica la original

# reverse(): Invertir orden
lista.reverse()        # [1, 1, 2, 3, 4, 5, 6, 9]
```

---

## 🚀 Comprensión de Listas

### **1. Comprensión Básica**
```python
# Crear lista de cuadrados
cuadrados = [x**2 for x in range(10)]
# Resultado: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Crear lista de números pares
pares = [x for x in range(20) if x % 2 == 0]
# Resultado: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Crear lista de strings
nombres = ["Ana", "Carlos", "María", "Luis"]
longitudes = [len(nombre) for nombre in nombres]
# Resultado: [3, 6, 4, 4]
```

### **2. Comprensión con Condiciones**
```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Números mayores a 5
mayores_5 = [x for x in numeros if x > 5]
# Resultado: [6, 7, 8, 9, 10]

# Números pares mayores a 5
pares_mayores_5 = [x for x in numeros if x > 5 and x % 2 == 0]
# Resultado: [6, 8, 10]

# Aplicar función condicional
transformados = [x*2 if x % 2 == 0 else x*3 for x in numeros]
# Resultado: [3, 4, 9, 8, 15, 12, 21, 16, 27, 20]
```

### **3. Comprensión Anidada**
```python
# Crear matriz 3x3
matriz = [[i+j for j in range(3)] for i in range(0, 9, 3)]
# Resultado: [[0, 1, 2], [3, 4, 5], [6, 7, 8]]

# Aplanar lista anidada
lista_anidada = [[1, 2], [3, 4], [5, 6]]
aplanada = [x for sublista in lista_anidada for x in sublista]
# Resultado: [1, 2, 3, 4, 5, 6]
```

---

## 🔍 Iteración y Búsqueda

### **1. Iteración Básica**
```python
numeros = [10, 25, 42, 17, 33, 89]

# Iteración directa
for numero in numeros:
    print(numero)

# Iteración con índice
for i, numero in enumerate(numeros):
    print(f"Índice {i}: {numero}")

# Iteración con range
for i in range(len(numeros)):
    print(f"Índice {i}: {numeros[i]}")

# Iteración inversa
for numero in reversed(numeros):
    print(numero)
```

### **2. Búsqueda y Filtrado**
```python
numeros = [15, 23, 8, 42, 17, 9, 31, 5]

# Encontrar máximo y mínimo
maximo = max(numeros)    # 42
minimo = min(numeros)    # 5

# Encontrar suma y promedio
suma = sum(numeros)      # 150
promedio = suma / len(numeros)  # 18.75

# Filtrar elementos
pares = [x for x in numeros if x % 2 == 0]      # [8, 42]
impares = [x for x in numeros if x % 2 != 0]    # [15, 23, 17, 9, 31, 5]

# Buscar elemento
if 42 in numeros:
    indice = numeros.index(42)
    print(f"42 encontrado en índice {indice}")
```

---

## 📊 Ventajas y Desventajas

### **✅ Ventajas**
- **Flexibilidad**: Puede almacenar elementos de diferentes tipos
- **Mutabilidad**: Se puede modificar después de la creación
- **Métodos ricos**: Muchos métodos incorporados para manipulación
- **Comprensión**: Sintaxis elegante para crear listas
- **Integración**: Excelente integración con el resto de Python
- **Eficiencia**: Acceso O(1) por índice

### **❌ Desventajas**
- **Memoria**: Puede usar más memoria que arrays tradicionales
- **Velocidad**: Más lenta que arrays de NumPy para operaciones numéricas
- **Heterogeneidad**: La flexibilidad puede llevar a errores de tipo
- **Mutabilidad**: Puede causar efectos secundarios inesperados

---

## 🎯 Casos de Uso Comunes

### **1. Almacenamiento de Datos**
```python
# Lista de estudiantes
estudiantes = ["Ana", "Carlos", "María", "Luis", "Elena"]

# Lista de calificaciones
calificaciones = [85, 92, 78, 95, 88]

# Combinar información
for i, nombre in enumerate(estudiantes):
    print(f"{nombre}: {calificaciones[i]}")
```

### **2. Procesamiento de Datos**
```python
# Filtrar datos
temperaturas = [22.5, 18.3, 25.1, 19.8, 23.4, 27.2]
altas = [t for t in temperaturas if t > 25]
bajas = [t for t in temperaturas if t < 20]

# Calcular estadísticas
promedio = sum(temperaturas) / len(temperaturas)
maxima = max(temperaturas)
minima = min(temperaturas)
```

### **3. Estructuras de Datos**
```python
# Pila (stack) usando lista
pila = []
pila.append(1)    # Push
pila.append(2)
pila.append(3)
ultimo = pila.pop()    # Pop

# Cola (queue) usando lista
cola = []
cola.append(1)    # Enqueue
cola.append(2)
cola.append(3)
primero = cola.pop(0)    # Dequeue
```

---

## 🔍 Listas vs Otras Estructuras

### **Listas vs Tuplas**
| Aspecto | Listas | Tuplas |
|---------|--------|--------|
| **Mutabilidad** | Mutable | Inmutable |
| **Rendimiento** | Más lento | Más rápido |
| **Uso** | Datos que cambian | Datos fijos |
| **Memoria** | Más memoria | Menos memoria |

### **Listas vs Arrays (NumPy)**
| Aspecto | Listas | Arrays NumPy |
|---------|--------|--------------|
| **Tipos** | Heterogéneos | Homogéneos |
| **Operaciones** | Básicas | Avanzadas |
| **Rendimiento** | Más lento | Mucho más rápido |
| **Memoria** | Más memoria | Menos memoria |

---

## 🚀 Implementación Interna

### **Cómo Funcionan las Listas**
```python
# Las listas en Python son arrays dinámicos
# Cuando se llenan, se redimensionan automáticamente

# Crear lista
mi_lista = [1, 2, 3]

# Internamente:
# - Se asigna memoria para 3 elementos
# - Cuando se agrega un 4to elemento:
mi_lista.append(4)
# - Python asigna nueva memoria para más elementos
# - Copia los elementos existentes
# - Libera la memoria anterior
```

### **Optimizaciones**
- **Over-allocation**: Se asigna más memoria de la necesaria para evitar redimensionamientos frecuentes
- **Growth pattern**: El tamaño crece de manera predecible (generalmente 1.125x)
- **Memory views**: Para operaciones de slicing, se crean vistas en lugar de copias

---

## 📚 Conceptos Clave a Recordar

- ✅ **Lista** = Colección ordenada, mutable y heterogénea
- ✅ **Índice base 0** = El primer elemento está en la posición 0
- ✅ **Mutabilidad** = Se pueden modificar después de la creación
- ✅ **Heterogeneidad** = Pueden contener elementos de diferentes tipos
- ✅ **Tamaño dinámico** = Pueden crecer o reducirse automáticamente
- ✅ **Métodos ricos** = Muchos métodos incorporados para manipulación
- ✅ **Comprensión** = Sintaxis elegante para crear listas
- ✅ **Acceso directo** = O(1) para acceder por índice

---

## 📝 Preguntas de Reflexión

1. **¿Por qué crees que las listas son tan versátiles en Python?**
2. **¿En qué situaciones preferirías usar una lista vs una tupla?**
3. **¿Cómo crees que la mutabilidad de las listas afecta el código?**
4. **¿Qué ventajas y desventajas ves en la heterogeneidad de las listas?**

---

## 🚀 Próximo Paso

**Archivo siguiente**: [03-Tuplas-Sets.md](./03-tuplas-sets.md)

**En la siguiente lección aprenderás sobre tuplas (inmutables) y sets (conjuntos), que complementan las listas en Python.**

---

**¡Excelente! Ahora entiendes cómo funcionan las listas en Python. En la próxima lección exploraremos otras estructuras de datos que te darán más opciones para organizar tu información.** 