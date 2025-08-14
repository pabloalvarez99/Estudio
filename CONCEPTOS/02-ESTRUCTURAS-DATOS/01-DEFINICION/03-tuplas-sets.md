# 📖 Definición: Tuplas y Sets en Python

## 🎯 Concepto Principal

### **¿Por qué son Importantes las Tuplas y Sets?**
Las tuplas y sets complementan las listas en Python, ofreciendo estructuras de datos especializadas para diferentes necesidades. Las tuplas proporcionan inmutabilidad y orden, mientras que los sets ofrecen operaciones de conjunto únicas y eficientes.

### **Definición Técnica**
- **Tupla**: Colección ordenada, inmutable y heterogénea de elementos
- **Set**: Colección no ordenada, mutable y de elementos únicos

---

## 🔍 ¿Qué son las Tuplas?

### **Definición Básica**
Una tupla es una **colección ordenada** de elementos que:
- **NO se pueden modificar** después de la creación (inmutable)
- Mantienen el **orden** de inserción
- Se acceden mediante **índices numéricos**
- Pueden contener elementos de **diferentes tipos**
- Se crean usando **paréntesis** o la función `tuple()`

### **Analogía del Mundo Real**
Imagina una **dirección postal**:
- La **dirección completa** es la tupla
- Cada **componente** (calle, número, ciudad, código postal) tiene una posición fija
- **NO puedes cambiar** la dirección una vez escrita
- El **orden** es importante para que la carta llegue al lugar correcto

---

## 🏗️ Estructura de una Tupla

### **Representación Visual**
```
Índice:    0      1      2      3
          ┌──────┬──────┬──────┬──────┐
Valor:    │ "Ana"│  25  │ True │ 3.14 │
          └──────┴──────┴──────┴──────┘
Tipo:     str    int    bool   float
```

### **Características Clave**
- **Inmutabilidad**: No se pueden cambiar, agregar o eliminar elementos
- **Orden**: Los elementos mantienen su posición
- **Acceso directo**: O(1) para acceder por índice
- **Heterogénea**: Puede contener elementos de diferentes tipos
- **Eficiencia**: Más rápida que las listas para operaciones de solo lectura

---

## 📊 Tipos de Tuplas

### **1. Tuplas Simples**
**Definición**: Tuplas que almacenan elementos básicos

**Ejemplos**:
```python
# Tupla de coordenadas
coordenadas = (10, 20)

# Tupla de información personal
persona = ("Ana", 25, "Ingeniera")

# Tupla de un solo elemento (nota la coma)
singleton = (42,)

# Tupla vacía
vacia = ()
```

**Uso común**: Coordenadas, información fija, valores constantes

---

### **2. Tuplas Anidadas**
**Definición**: Tuplas que contienen otras tuplas como elementos

**Ejemplos**:
```python
# Tupla de coordenadas 3D
punto_3d = (10, 20, 30)

# Tupla de puntos
puntos = ((0, 0), (1, 1), (2, 2), (3, 3))

# Tupla de información compleja
estudiante = ("Ana", 25, ("Matemáticas", "Física"), 85.5)

# Tupla de tuplas de diferentes tamaños
estructura = ((1, 2), (3, 4, 5), (6,))
```

**Uso común**: Estructuras de datos complejas, coordenadas múltiples

---

### **3. Tuplas Especializadas**
**Definición**: Tuplas diseñadas para propósitos específicos

**Ejemplos**:
```python
# Tupla de rangos (inicio, fin, paso)
rango = (0, 100, 5)

# Tupla de configuración (host, puerto, timeout)
config = ("localhost", 8080, 30)

# Tupla de colores RGB
color = (255, 128, 0)

# Tupla de fechas (año, mes, día)
fecha = (2024, 3, 15)
```

**Uso común**: Configuraciones, parámetros, valores constantes

---

## 🔧 Operaciones con Tuplas

### **1. Creación de Tuplas**
```python
# Creación directa con paréntesis
tupla1 = (1, 2, 3, 4, 5)

# Creación sin paréntesis (tupla por defecto)
tupla2 = 1, 2, 3, 4, 5

# Creación con tuple()
tupla3 = tuple([1, 2, 3, 4, 5])

# Creación con range()
tupla4 = tuple(range(5))  # (0, 1, 2, 3, 4)

# Creación de tupla vacía
tupla_vacia = ()

# Creación de tupla con un elemento
singleton = (42,)
```

### **2. Acceso a Elementos**
```python
coordenadas = (10, 25, 42, 17, 33)

# Acceso por índice positivo
primer = coordenadas[0]      # 10
segundo = coordenadas[1]     # 25
ultimo = coordenadas[4]      # 33

# Acceso por índice negativo
ultimo = coordenadas[-1]     # 33
penultimo = coordenadas[-2]  # 17

# Slicing (rebanadas)
primeros_tres = coordenadas[0:3]    # (10, 25, 42)
ultimos_tres = coordenadas[-3:]     # (42, 17, 33)
todos = coordenadas[:]              # (10, 25, 42, 17, 33)
```

### **3. Operaciones Permitidas**
```python
coordenadas = (10, 25, 42)

# ✅ PERMITIDO: Acceso a elementos
x = coordenadas[0]
y = coordenadas[1]

# ✅ PERMITIDO: Slicing
sub_tupla = coordenadas[1:3]

# ✅ PERMITIDO: Concatenación
nueva_tupla = coordenadas + (100, 200)

# ✅ PERMITIDO: Repetición
tupla_repetida = coordenadas * 3

# ❌ NO PERMITIDO: Modificación
# coordenadas[0] = 100  # TypeError

# ❌ NO PERMITIDO: Agregar elementos
# coordenadas.append(100)  # AttributeError

# ❌ NO PERMITIDO: Eliminar elementos
# del coordenadas[0]  # TypeError
```

---

## 📈 Métodos de Tuplas

### **Métodos Disponibles**
```python
coordenadas = (10, 25, 42, 17, 33, 42)

# count(): Contar ocurrencias
cantidad_42 = coordenadas.count(42)    # 2

# index(): Encontrar índice de un elemento
indice_25 = coordenadas.index(25)      # 1

# len(): Longitud de la tupla
longitud = len(coordenadas)            # 6

# in: Verificar si existe
existe_100 = 100 in coordenadas        # False
existe_42 = 42 in coordenadas          # True
```

### **Operaciones de Tuplas**
```python
tupla1 = (1, 2, 3)
tupla2 = (4, 5, 6)

# Concatenación
concatenada = tupla1 + tupla2          # (1, 2, 3, 4, 5, 6)

# Repetición
repetida = tupla1 * 3                  # (1, 2, 3, 1, 2, 3)

# Comparación
tupla1 < tupla2                        # True (comparación lexicográfica)

# Desempaquetado
a, b, c = tupla1                       # a=1, b=2, c=3

# Desempaquetado con *
primero, *resto = tupla1               # primero=1, resto=[2, 3]
```

---

## 🔍 ¿Qué son los Sets?

### **Definición Básica**
Un set es una **colección no ordenada** de elementos que:
- **NO tienen orden** definido
- **NO permiten duplicados**
- Se pueden **modificar** después de la creación (mutable)
- Se crean usando **llaves** o la función `set()`
- Son **muy eficientes** para operaciones de conjunto

### **Analogía del Mundo Real**
Imagina una **caja de herramientas sin compartimentos**:
- Las **herramientas** son los elementos
- **NO hay orden** específico
- **NO puedes tener** herramientas duplicadas
- Puedes **agregar o quitar** herramientas
- Es **muy rápido** encontrar si tienes una herramienta específica

---

## 🏗️ Estructura de un Set

### **Representación Visual**
```
Elementos:  ┌──────┬──────┬──────┬──────┬──────┐
            │ "Ana"│  25  │ True │ 3.14 │ [1,2]│
            └──────┴──────┴──────┴──────┴──────┘
            (Sin orden específico, sin duplicados)
```

### **Características Clave**
- **No ordenado**: Los elementos no tienen posición fija
- **Sin duplicados**: Cada elemento es único
- **Mutable**: Se pueden agregar y eliminar elementos
- **Heterogéneo**: Puede contener elementos de diferentes tipos
- **Eficiente**: O(1) para búsqueda, inserción y eliminación

---

## 📊 Tipos de Sets

### **1. Sets Simples**
**Definición**: Sets que almacenan elementos básicos

**Ejemplos**:
```python
# Set de números
numeros = {1, 2, 3, 4, 5}

# Set de strings
colores = {"rojo", "verde", "azul", "amarillo"}

# Set mixto
mixto = {42, "Python", True, 3.14}

# Set vacío
vacio = set()

# Set desde una lista
desde_lista = set([1, 2, 2, 3, 3, 4])  # {1, 2, 3, 4}
```

**Uso común**: Eliminación de duplicados, verificación de pertenencia

---

### **2. Sets de Conjuntos**
**Definición**: Sets que contienen otros sets o estructuras

**Ejemplos**:
```python
# Set de tuplas
coordenadas = {(0, 0), (1, 1), (2, 2)}

# Set de frozensets (sets inmutables)
conjuntos = {frozenset([1, 2]), frozenset([3, 4])}

# Set de strings únicos
palabras_unicas = {"hola", "mundo", "python", "programacion"}
```

**Uso común**: Operaciones de conjunto, análisis de datos

---

## 🔧 Operaciones con Sets

### **1. Creación de Sets**
```python
# Creación directa con llaves
set1 = {1, 2, 3, 4, 5}

# Creación con set()
set2 = set([1, 2, 3, 4, 5])

# Creación desde string (caracteres únicos)
set3 = set("hello")  # {'h', 'e', 'l', 'o'}

# Creación desde range
set4 = set(range(5))  # {0, 1, 2, 3, 4}

# Creación de set vacío
set_vacio = set()
```

### **2. Operaciones de Modificación**
```python
mi_set = {1, 2, 3}

# Agregar elementos
mi_set.add(4)                    # {1, 2, 3, 4}
mi_set.update([5, 6, 7])         # {1, 2, 3, 4, 5, 6, 7}

# Eliminar elementos
mi_set.remove(3)                 # {1, 2, 4, 5, 6, 7}
mi_set.discard(10)               # No hace nada si no existe
elemento = mi_set.pop()          # Elimina y retorna un elemento aleatorio
mi_set.clear()                    # {}

# Verificar pertenencia
existe = 5 in mi_set             # True/False
```

### **3. Operaciones de Conjunto**
```python
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}

# Unión
union = set1 | set2              # {1, 2, 3, 4, 5, 6, 7, 8}
union = set1.union(set2)         # Método alternativo

# Intersección
interseccion = set1 & set2       # {4, 5}
interseccion = set1.intersection(set2)

# Diferencia
diferencia = set1 - set2         # {1, 2, 3}
diferencia = set1.difference(set2)

# Diferencia simétrica
diferencia_sim = set1 ^ set2     # {1, 2, 3, 6, 7, 8}
diferencia_sim = set1.symmetric_difference(set2)
```

---

## 📈 Métodos de Sets

### **Métodos de Modificación**
```python
mi_set = {1, 2, 3}

# add(): Agregar un elemento
mi_set.add(4)

# update(): Agregar múltiples elementos
mi_set.update([5, 6, 7])

# remove(): Eliminar elemento (error si no existe)
mi_set.remove(3)

# discard(): Eliminar elemento (no error si no existe)
mi_set.discard(10)

# pop(): Eliminar y retornar elemento aleatorio
elemento = mi_set.pop()

# clear(): Limpiar todo el set
mi_set.clear()
```

### **Métodos de Información**
```python
mi_set = {1, 2, 3, 4, 5}

# len(): Longitud del set
longitud = len(mi_set)

# in: Verificar pertenencia
existe = 3 in mi_set

# isdisjoint(): Verificar si no hay intersección
set1 = {1, 2, 3}
set2 = {4, 5, 6}
disjuntos = set1.isdisjoint(set2)  # True

# issubset(): Verificar si es subconjunto
es_subconjunto = {1, 2}.issubset(set1)  # True

# issuperset(): Verificar si es superconjunto
es_superconjunto = set1.issuperset({1, 2})  # True
```

---

## 🚀 Comprensión de Sets

### **Comprensión Básica**
```python
# Crear set de cuadrados
cuadrados = {x**2 for x in range(10)}
# Resultado: {0, 1, 64, 4, 36, 9, 16, 49, 81, 25}

# Crear set de números pares
pares = {x for x in range(20) if x % 2 == 0}
# Resultado: {0, 2, 4, 6, 8, 10, 12, 14, 16, 18}

# Crear set de longitudes de palabras
palabras = ["Python", "programación", "sets", "tuplas"]
longitudes = {len(palabra) for palabra in palabras}
# Resultado: {6, 12, 4, 6}
```

### **Comprensión con Condiciones**
```python
numeros = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

# Números mayores a 5
mayores_5 = {x for x in numeros if x > 5}

# Números pares mayores a 5
pares_mayores_5 = {x for x in numeros if x > 5 and x % 2 == 0}

# Transformar números
transformados = {x*2 if x % 2 == 0 else x*3 for x in numeros}
```

---

## 📊 Ventajas y Desventajas

### **Tuplas - ✅ Ventajas**
- **Inmutabilidad**: Garantiza que los datos no cambien
- **Rendimiento**: Más rápida que las listas
- **Memoria**: Usa menos memoria que las listas
- **Hashable**: Se pueden usar como claves en diccionarios
- **Orden**: Mantiene el orden de los elementos

### **Tuplas - ❌ Desventajas**
- **Inmutabilidad**: No se pueden modificar después de la creación
- **Flexibilidad limitada**: Menos métodos disponibles
- **Tamaño fijo**: No se pueden agregar o eliminar elementos

### **Sets - ✅ Ventajas**
- **Eficiencia**: Muy rápido para búsqueda y eliminación de duplicados
- **Operaciones de conjunto**: Unión, intersección, diferencia
- **Sin duplicados**: Garantiza elementos únicos automáticamente
- **Mutabilidad**: Se pueden modificar después de la creación

### **Sets - ❌ Desventajas**
- **No ordenado**: Los elementos no tienen posición fija
- **No indexable**: No se puede acceder por índice
- **No hashable**: Los elementos deben ser inmutables
- **Memoria**: Puede usar más memoria que otras estructuras

---

## 🎯 Casos de Uso Comunes

### **Tuplas - Cuándo Usarlas**
```python
# 1. Coordenadas y puntos
punto = (10, 20)
rectangulo = ((0, 0), (100, 100))

# 2. Información fija
persona = ("Ana", 25, "Ingeniera")
configuracion = ("localhost", 8080, 30)

# 3. Claves de diccionario
coordenadas_dict = {(0, 0): "origen", (1, 1): "punto_1"}

# 4. Retorno de múltiples valores
def obtener_coordenadas():
    return (10, 20)

x, y = obtener_coordenadas()
```

### **Sets - Cuándo Usarlas**
```python
# 1. Eliminación de duplicados
numeros = [1, 2, 2, 3, 3, 4, 4, 5]
unicos = set(numeros)  # {1, 2, 3, 4, 5}

# 2. Verificación de pertenencia
usuarios_activos = {"ana", "carlos", "maria"}
if "ana" in usuarios_activos:
    print("Usuario activo")

# 3. Operaciones de conjunto
matematicas = {"ana", "carlos", "luis"}
fisica = {"carlos", "maria", "pedro"}
ambas = matematicas & fisica  # {"carlos"}

# 4. Análisis de datos
palabras_texto = set("hola mundo python".split())
palabras_busqueda = {"python", "programacion"}
coincidencias = palabras_texto & palabras_busqueda
```

---

## 🔍 Comparación de Estructuras

### **Tuplas vs Listas**
| Aspecto | Tuplas | Listas |
|---------|--------|--------|
| **Mutabilidad** | Inmutable | Mutable |
| **Rendimiento** | Más rápido | Más lento |
| **Memoria** | Menos memoria | Más memoria |
| **Uso** | Datos fijos | Datos que cambian |
| **Hashable** | Sí | No |

### **Sets vs Listas**
| Aspecto | Sets | Listas |
|---------|------|--------|
| **Orden** | No ordenado | Ordenado |
| **Duplicados** | No permite | Permite |
| **Acceso** | No indexable | Indexable |
| **Búsqueda** | O(1) | O(n) |
| **Uso** | Conjuntos únicos | Secuencias ordenadas |

---

## 🚀 Implementación Interna

### **Cómo Funcionan las Tuplas**
```python
# Las tuplas son arrays inmutables
# Una vez creadas, no se puede cambiar su contenido

mi_tupla = (1, 2, 3)

# Internamente:
# - Se asigna memoria para los elementos
# - Los elementos se almacenan en posiciones contiguas
# - No hay overhead para mutabilidad
# - Se pueden usar como claves en diccionarios
```

### **Cómo Funcionan los Sets**
```python
# Los sets son tablas hash
# Cada elemento se convierte en un hash para búsqueda rápida

mi_set = {1, 2, 3}

# Internamente:
# - Se crea una tabla hash
# - Cada elemento se convierte en hash
# - Las colisiones se manejan con listas enlazadas
# - Búsqueda, inserción y eliminación son O(1) en promedio
```

---

## 📚 Conceptos Clave a Recordar

### **Tuplas**
- ✅ **Tupla** = Colección ordenada, inmutable y heterogénea
- ✅ **Inmutabilidad** = No se pueden modificar después de la creación
- ✅ **Orden** = Los elementos mantienen su posición
- ✅ **Eficiencia** = Más rápida que las listas para operaciones de solo lectura
- ✅ **Hashable** = Se pueden usar como claves en diccionarios

### **Sets**
- ✅ **Set** = Colección no ordenada, mutable y de elementos únicos
- ✅ **Sin duplicados** = Cada elemento es único automáticamente
- ✅ **No ordenado** = Los elementos no tienen posición fija
- ✅ **Eficiente** = O(1) para búsqueda, inserción y eliminación
- ✅ **Operaciones de conjunto** = Unión, intersección, diferencia

---

## 📝 Preguntas de Reflexión

1. **¿Por qué crees que las tuplas son inmutables? ¿Qué ventajas tiene esto?**
2. **¿En qué situaciones preferirías usar un set vs una lista?**
3. **¿Cómo crees que la inmutabilidad de las tuplas afecta el código?**
4. **¿Qué ventajas y desventajas ves en los sets no ordenados?**

---

## 🚀 Próximo Paso

**Archivo siguiente**: [04-Diccionarios-Python.md](./04-diccionarios-python.md)

**En la siguiente lección aprenderás sobre diccionarios, la estructura de datos más versátil de Python para mapear claves a valores.**

---

**¡Excelente! Ahora entiendes cómo funcionan las tuplas y sets en Python. En la próxima lección exploraremos los diccionarios, que te darán aún más opciones para organizar y acceder a tu información de manera eficiente.** 