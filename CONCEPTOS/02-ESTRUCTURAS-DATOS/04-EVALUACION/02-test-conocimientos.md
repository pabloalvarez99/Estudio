# 📝 Test de Conocimientos: Estructuras de Datos y Algoritmos

## 🎯 Objetivo del Test
Este test te permitirá evaluar tu comprensión teórica y práctica de las estructuras de datos y algoritmos en Python. Responde todas las preguntas y ejercicios para identificar áreas de mejora.

## ⏱️ Tiempo Estimado: 60-90 minutos

---

## 📚 **SECCIÓN 1: PREGUNTAS TEÓRICAS**

### **Pregunta 1: Arrays y Estructuras Básicas**
**¿Qué es un array y cuáles son sus características principales?**

**Tu respuesta:**
```
[Escribe aquí tu respuesta]
```

**Respuesta esperada:**
Un array es una estructura de datos que almacena elementos del mismo tipo en posiciones contiguas de memoria. Sus características principales son:
- **Índice base 0**: El primer elemento está en la posición 0
- **Acceso directo**: O(1) para acceder a cualquier elemento
- **Tamaño fijo**: En la mayoría de lenguajes, el tamaño se define al crear
- **Tipos homogéneos**: Todos los elementos deben ser del mismo tipo
- **Posiciones contiguas**: Los elementos se almacenan uno tras otro en memoria

---

### **Pregunta 2: Listas en Python**
**¿Cuáles son las diferencias principales entre listas y arrays tradicionales?**

**Tu respuesta:**
```
[Escribe aquí tu respuesta]
```

**Respuesta esperada:**
Las listas en Python son más flexibles que los arrays tradicionales:
- **Tipos heterogéneos**: Pueden contener elementos de diferentes tipos
- **Tamaño dinámico**: Pueden crecer y reducirse automáticamente
- **Métodos incorporados**: Tienen muchos métodos útiles (append, sort, etc.)
- **Comprensión**: Sintaxis elegante para crear listas
- **Overhead**: Usan más memoria que arrays tradicionales
- **Velocidad**: Son más lentas para operaciones numéricas intensivas

---

### **Pregunta 3: Tuplas vs Sets**
**¿Cuándo preferirías usar una tupla vs un set? Explica con ejemplos.**

**Tu respuesta:**
```
[Escribe aquí tu respuesta]
```

**Respuesta esperada:**
**Tuplas** cuando:
- Necesitas orden y inmutabilidad (coordenadas, fechas, configuraciones)
- Los datos no cambiarán después de la creación
- Quieres usar la estructura como clave en un diccionario
- Ejemplo: `punto = (10, 20)`, `fecha = (2024, 3, 15)`

**Sets** cuando:
- Necesitas elementos únicos sin duplicados
- Quieres operaciones de conjunto (unión, intersección)
- El orden no importa
- Necesitas verificar pertenencia rápidamente
- Ejemplo: `usuarios_activos = {"ana", "carlos", "maria"}`

---

### **Pregunta 4: Diccionarios**
**¿Cómo funcionan internamente los diccionarios en Python? Explica el concepto de hash tables.**

**Tu respuesta:**
```
[Escribe aquí tu respuesta]
```

**Respuesta esperada:**
Los diccionarios en Python son implementados como **tablas hash**:
- **Función hash**: Cada clave se convierte en un número hash único
- **Tabla hash**: Array donde se almacenan los pares clave-valor
- **Colisiones**: Cuando dos claves tienen el mismo hash, se manejan con listas enlazadas
- **Acceso O(1)**: En promedio, búsqueda, inserción y eliminación son constantes
- **Claves inmutables**: Las claves deben ser hashables (no pueden cambiar)
- **Redimensionamiento**: La tabla se redimensiona automáticamente cuando es necesario

---

## 🛠️ **SECCIÓN 2: EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Manipulación de Listas**
**Escribe código para realizar las siguientes operaciones:**

```python
# Lista inicial
numeros = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]

# 1. Eliminar todos los duplicados manteniendo el orden
# Tu código aquí:

# 2. Encontrar el segundo número más grande
# Tu código aquí:

# 3. Crear una nueva lista con solo los números que aparecen más de una vez
# Tu código aquí:

# 4. Invertir la lista sin usar reverse() o [::-1]
# Tu código aquí:
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
# 1. Eliminar duplicados manteniendo orden
sin_duplicados = []
for num in numeros:
    if num not in sin_duplicados:
        sin_duplicados.append(num)

# 2. Segundo número más grande
numeros_ordenados = sorted(set(numeros), reverse=True)
segundo_mayor = numeros_ordenados[1] if len(numeros_ordenados) > 1 else None

# 3. Números que aparecen más de una vez
from collections import Counter
contador = Counter(numeros)
repetidos = [num for num, freq in contador.items() if freq > 1]

# 4. Invertir lista manualmente
invertida = []
for i in range(len(numeros) - 1, -1, -1):
    invertida.append(numeros[i])
```

---

### **Ejercicio 2: Operaciones con Sets**
**Implementa las siguientes funciones usando sets:**

```python
def encontrar_elementos_comunes(lista1, lista2):
    """Encuentra elementos que están en ambas listas"""
    # Tu código aquí
    pass

def encontrar_elementos_unicos(lista1, lista2):
    """Encuentra elementos que están en una lista pero no en la otra"""
    # Tu código aquí
    pass

def verificar_superconjunto(lista1, lista2):
    """Verifica si lista1 contiene todos los elementos de lista2"""
    # Tu código aquí
    pass

# Prueba las funciones
lista_a = [1, 2, 3, 4, 5]
lista_b = [4, 5, 6, 7, 8]

print("Elementos comunes:", encontrar_elementos_comunes(lista_a, lista_b))
print("Elementos únicos:", encontrar_elementos_unicos(lista_a, lista_b))
print("¿A es superconjunto de B?:", verificar_superconjunto(lista_a, lista_b))
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
def encontrar_elementos_comunes(lista1, lista2):
    """Encuentra elementos que están en ambas listas"""
    set1 = set(lista1)
    set2 = set(lista2)
    return set1 & set2

def encontrar_elementos_unicos(lista1, lista2):
    """Encuentra elementos que están en una lista pero no en la otra"""
    set1 = set(lista1)
    set2 = set(lista2)
    return set1 ^ set2

def verificar_superconjunto(lista1, lista2):
    """Verifica si lista1 contiene todos los elementos de lista2"""
    set1 = set(lista1)
    set2 = set(lista2)
    return set2.issubset(set1)
```

---

### **Ejercicio 3: Comprensión de Estructuras**
**Usa comprensión para crear las siguientes estructuras:**

```python
# Lista de números del 1 al 20
numeros = list(range(1, 21))

# 1. Lista de cuadrados de números pares
cuadrados_pares = # Tu código aquí

# 2. Diccionario que mapea cada número a su factorial
factoriales = # Tu código aquí

# 3. Set de números que son divisibles por 3 o 5
divisibles_3_5 = # Tu código aquí

# 4. Lista de tuplas (número, es_primo)
def es_primo(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

primos_info = # Tu código aquí
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
# 1. Lista de cuadrados de números pares
cuadrados_pares = [x**2 for x in numeros if x % 2 == 0]

# 2. Diccionario que mapea cada número a su factorial
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

factoriales = {x: factorial(x) for x in numeros}

# 3. Set de números que son divisibles por 3 o 5
divisibles_3_5 = {x for x in numeros if x % 3 == 0 or x % 5 == 0}

# 4. Lista de tuplas (número, es_primo)
primos_info = [(x, es_primo(x)) for x in numeros]
```

---

### **Ejercicio 4: Algoritmos Básicos**
**Implementa los siguientes algoritmos:**

```python
def busqueda_binaria(lista, elemento):
    """Implementa búsqueda binaria (lista debe estar ordenada)"""
    # Tu código aquí
    pass

def ordenamiento_burbuja(lista):
    """Implementa ordenamiento por burbuja"""
    # Tu código aquí
    pass

def encontrar_duplicados(lista):
    """Encuentra elementos duplicados en una lista"""
    # Tu código aquí
    pass

# Prueba los algoritmos
numeros = [64, 34, 25, 12, 22, 11, 90]
print("Lista original:", numeros)

# Ordenar
ordenados = ordenamiento_burbuja(numeros.copy())
print("Lista ordenada:", ordenados)

# Buscar elemento
elemento_buscar = 22
indice = busqueda_binaria(ordenados, elemento_buscar)
print(f"Elemento {elemento_buscar} en índice: {indice}")

# Encontrar duplicados
lista_con_duplicados = [1, 2, 2, 3, 4, 4, 5]
duplicados = encontrar_duplicados(lista_con_duplicados)
print("Elementos duplicados:", duplicados)
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
def busqueda_binaria(lista, elemento):
    """Implementa búsqueda binaria (lista debe estar ordenada)"""
    izquierda, derecha = 0, len(lista) - 1
    
    while izquierda <= derecha:
        medio = (izquierda + derecha) // 2
        if lista[medio] == elemento:
            return medio
        elif lista[medio] < elemento:
            izquierda = medio + 1
        else:
            derecha = medio - 1
    return -1

def ordenamiento_burbuja(lista):
    """Implementa ordenamiento por burbuja"""
    n = len(lista)
    for i in range(n):
        for j in range(0, n - i - 1):
            if lista[j] > lista[j + 1]:
                lista[j], lista[j + 1] = lista[j + 1], lista[j]
    return lista

def encontrar_duplicados(lista):
    """Encuentra elementos duplicados en una lista"""
    contador = {}
    duplicados = []
    
    for elemento in lista:
        contador[elemento] = contador.get(elemento, 0) + 1
        if contador[elemento] == 2:  # Solo agregar la primera vez que se encuentra
            duplicados.append(elemento)
    
    return duplicados
```

---

## 🧠 **SECCIÓN 3: PROBLEMAS DE LÓGICA**

### **Problema 1: Análisis de Texto**
**Implementa una función que analice un texto y retorne estadísticas:**

```python
def analizar_texto(texto):
    """
    Analiza un texto y retorna un diccionario con estadísticas:
    - total_palabras: número total de palabras
    - palabras_unicas: número de palabras únicas
    - palabra_mas_larga: la palabra más larga
    - palabra_mas_comun: la palabra que aparece más veces
    - promedio_longitud: longitud promedio de las palabras
    """
    # Tu código aquí
    pass

# Prueba la función
texto_ejemplo = """
Python es un lenguaje de programación de alto nivel, interpretado y orientado a objetos. 
Fue creado por Guido van Rossum y lanzado por primera vez en 1991. Python tiene una 
sintaxis simple y legible que enfatiza la legibilidad del código.
"""

resultado = analizar_texto(texto_ejemplo)
for clave, valor in resultado.items():
    print(f"{clave}: {valor}")
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
def analizar_texto(texto):
    """Analiza un texto y retorna estadísticas"""
    # Limpiar y dividir el texto
    palabras = texto.lower().split()
    
    if not palabras:
        return {
            "total_palabras": 0,
            "palabras_unicas": 0,
            "palabra_mas_larga": "",
            "palabra_mas_comun": "",
            "promedio_longitud": 0
        }
    
    # Contar palabras
    contador = {}
    for palabra in palabras:
        palabra_limpia = palabra.strip(".,!?;:")
        if palabra_limpia:
            contador[palabra_limpia] = contador.get(palabra_limpia, 0) + 1
    
    # Calcular estadísticas
    total_palabras = len(palabras)
    palabras_unicas = len(contador)
    palabra_mas_larga = max(contador.keys(), key=len)
    palabra_mas_comun = max(contador.items(), key=lambda x: x[1])[0]
    promedio_longitud = sum(len(palabra) for palabra in contador.keys()) / palabras_unicas
    
    return {
        "total_palabras": total_palabras,
        "palabras_unicas": palabras_unicas,
        "palabra_mas_larga": palabra_mas_larga,
        "palabra_mas_comun": palabra_mas_comun,
        "promedio_longitud": round(promedio_longitud, 2)
    }
```

---

### **Problema 2: Sistema de Caché**
**Implementa un sistema de caché simple usando diccionarios:**

```python
class Cache:
    def __init__(self, capacidad_maxima=100):
        """
        Inicializa un caché con capacidad máxima
        - capacidad_maxima: número máximo de elementos en el caché
        """
        # Tu código aquí
        pass
    
    def obtener(self, clave, valor_por_defecto=None):
        """
        Obtiene un valor del caché
        - clave: clave del elemento
        - valor_por_defecto: valor a retornar si la clave no existe
        """
        # Tu código aquí
        pass
    
    def establecer(self, clave, valor):
        """
        Establece un valor en el caché
        - clave: clave del elemento
        - valor: valor a almacenar
        """
        # Tu código aquí
        pass
    
    def limpiar(self):
        """Limpia todo el caché"""
        # Tu código aquí
        pass
    
    def estadisticas(self):
        """Retorna estadísticas del caché"""
        # Tu código aquí
        pass

# Prueba el caché
cache = Cache(5)
cache.establecer("usuario_1", {"nombre": "Ana", "edad": 25})
cache.establecer("usuario_2", {"nombre": "Carlos", "edad": 30})

print("Usuario 1:", cache.obtener("usuario_1"))
print("Usuario 3:", cache.obtener("usuario_3", "No encontrado"))
print("Estadísticas:", cache.estadisticas())
```

**Tu código:**
```python
# Escribe aquí tu solución
```

**Solución esperada:**
```python
class Cache:
    def __init__(self, capacidad_maxima=100):
        """Inicializa un caché con capacidad máxima"""
        self.capacidad_maxima = capacidad_maxima
        self.cache = {}
        self.contador_accesos = {}
    
    def obtener(self, clave, valor_por_defecto=None):
        """Obtiene un valor del caché"""
        if clave in self.cache:
            self.contador_accesos[clave] += 1
            return self.cache[clave]
        return valor_por_defecto
    
    def establecer(self, clave, valor):
        """Establece un valor en el caché"""
        # Si el caché está lleno, eliminar el elemento menos usado
        if len(self.cache) >= self.capacidad_maxima and clave not in self.cache:
            # Encontrar elemento menos usado
            clave_menos_usada = min(self.contador_accesos.keys(), 
                                   key=lambda k: self.contador_accesos[k])
            del self.cache[clave_menos_usada]
            del self.contador_accesos[clave_menos_usada]
        
        self.cache[clave] = valor
        if clave not in self.contador_accesos:
            self.contador_accesos[clave] = 0
    
    def limpiar(self):
        """Limpia todo el caché"""
        self.cache.clear()
        self.contador_accesos.clear()
    
    def estadisticas(self):
        """Retorna estadísticas del caché"""
        return {
            "elementos": len(self.cache),
            "capacidad_maxima": self.capacidad_maxima,
            "uso_memoria": f"{len(self.cache)}/{self.capacidad_maxima}",
            "elementos_mas_usados": sorted(self.contador_accesos.items(), 
                                          key=lambda x: x[1], reverse=True)[:3]
        }
```

---

## 📊 **EVALUACIÓN DEL TEST**

### **Puntuación por Sección**
- **Preguntas Teóricas**: ___/20
- **Ejercicios Prácticos**: ___/20
- **Problemas de Lógica**: ___/20

**PUNTUACIÓN TOTAL**: ___/60

---

## 🎯 **CRITERIOS DE EVALUACIÓN**

### **Nivel de Dominio**
- **54-60 puntos**: 🏆 **MAESTRO** - Dominas completamente todos los conceptos
- **48-53 puntos**: 🥇 **AVANZADO** - Tienes un dominio sólido de la mayoría de conceptos
- **42-47 puntos**: 🥈 **INTERMEDIO** - Tienes un buen dominio de los conceptos básicos
- **36-41 puntos**: 🥉 **BÁSICO** - Entiendes los conceptos fundamentales
- **Menos de 36 puntos**: 📚 **EN DESARROLLO** - Necesitas más práctica

---

## 🔍 **ANÁLISIS DE RESULTADOS**

### **Áreas de Fortaleza**
```
[Escribe aquí las áreas donde te sientes más fuerte]
```

### **Áreas de Mejora**
```
[Escribe aquí las áreas donde necesitas más práctica]
```

### **Conceptos que debo revisar**
```
[Escribe aquí los conceptos específicos que debo repasar]
```

---

## 📝 **PLAN DE ACCIÓN POST-TEST**

### **Si tu puntuación es 48+ puntos:**
1. **Revisar respuestas incorrectas** para entender errores
2. **Practicar ejercicios difíciles** para consolidar conocimiento
3. **Implementar mejoras** en los proyectos existentes
4. **Continuar** con el siguiente concepto del curso

### **Si tu puntuación es menor a 48 puntos:**
1. **Repasar teoría** de las secciones con puntuación baja
2. **Practicar ejercicios** similares a los del test
3. **Implementar proyectos** paso a paso
4. **Buscar recursos adicionales** para conceptos difíciles
5. **No continuar** hasta dominar estos conceptos

---

## 🚀 **RECURSOS DE ESTUDIO RECOMENDADOS**

### **Para Teoría:**
- [ ] Documentación oficial de Python sobre estructuras de datos
- [ ] Libros: "Python Data Structures and Algorithms" por Benjamin Baka
- [ ] Videos: Curso de estructuras de datos en YouTube

### **Para Práctica:**
- [ ] HackerRank: Problemas de estructuras de datos
- [ ] LeetCode: Ejercicios de arrays, strings, etc.
- [ ] Proyectos personales usando diferentes estructuras

### **Para Algoritmos:**
- [ ] "Grokking Algorithms" por Aditya Bhargava
- [ ] Visualizaciones de algoritmos en VisuAlgo
- [ ] Implementación de algoritmos clásicos

---

## 🎯 **PRÓXIMO PASO**

**Archivo siguiente**: [03-Evaluacion-Final.md](./03-evaluacion-final.md)

**En la siguiente lección realizarás la evaluación final del segundo concepto, consolidando todo lo aprendido.**

---

**¡Excelente trabajo completando este test! Has demostrado tu comprensión de estructuras de datos y algoritmos. Usa los resultados para identificar áreas de mejora y continuar tu aprendizaje.** 