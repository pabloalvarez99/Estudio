# 🛠️ Ejercicios Prácticos: Listas en Python

## 🎯 Objetivo de la Lección
Practicar el uso de listas en Python, incluyendo creación, manipulación, métodos, comprensión de listas y operaciones avanzadas. Comprender cuándo y cómo usar listas de manera eficiente.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Conceptos Clave de Listas**
- **Mutabilidad**: Las listas se pueden modificar después de la creación
- **Heterogeneidad**: Pueden contener elementos de diferentes tipos
- **Índice base 0**: El primer elemento está en la posición 0
- **Métodos incorporados**: append(), insert(), remove(), pop(), sort(), etc.
- **Comprensión**: Sintaxis elegante para crear listas

### **Sintaxis Básica**
```python
# Creación
mi_lista = [1, 2, 3, 4, 5]

# Acceso
primer = mi_lista[0]
ultimo = mi_lista[-1]

# Modificación
mi_lista[2] = 100
mi_lista.append(6)
```

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Creación y Acceso a Listas**
**Objetivo**: Practicar la creación de listas y acceso a elementos

**Instrucciones**: Crea listas y accede a sus elementos

**Tu código**:
```python
# 1. Crea una lista con los primeros 10 números naturales
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 2. Crea una lista con nombres de frutas
frutas = ["manzana", "banana", "naranja", "uva", "kiwi"]

# 3. Crea una lista mixta con diferentes tipos de datos
mixta = [42, "Python", True, 3.14, [1, 2, 3]]

# 4. Accede a elementos específicos
print("Primer número:", numeros[0])
print("Última fruta:", frutas[-1])
print("Tercer elemento de mixta:", mixta[2])

# 5. Usa slicing para obtener subconjuntos
primeros_tres = numeros[0:3]
ultimas_dos_frutas = frutas[-2:]
print("Primeros tres números:", primeros_tres)
print("Últimas dos frutas:", ultimas_dos_frutas)
```

**Ejemplo de respuesta**:
```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
frutas = ["manzana", "banana", "naranja", "uva", "kiwi"]
mixta = [42, "Python", True, 3.14, [1, 2, 3]]

print("Primer número:", numeros[0])
print("Última fruta:", frutas[-1])
print("Tercer elemento de mixta:", mixta[2])

primeros_tres = numeros[0:3]
ultimas_dos_frutas = frutas[-2:]
print("Primeros tres números:", primeros_tres)
print("Últimas dos frutas:", ultimas_dos_frutas)
```

---

### **Ejercicio 2: Modificación de Listas**
**Objetivo**: Practicar la modificación de listas existentes

**Instrucciones**: Modifica listas usando diferentes métodos

**Tu código**:
```python
# Lista inicial
numeros = [10, 20, 30, 40, 50]
print("Lista original:", numeros)

# 1. Cambia el tercer elemento a 35
numeros[2] = 35
print("Después de cambiar el tercer elemento:", numeros)

# 2. Agrega 60 al final
numeros.append(60)
print("Después de agregar 60:", numeros)

# 3. Inserta 15 en la posición 1
numeros.insert(1, 15)
print("Después de insertar 15:", numeros)

# 4. Extiende la lista con [70, 80, 90]
numeros.extend([70, 80, 90])
print("Después de extender:", numeros)

# 5. Cambia los elementos del índice 2 al 4 por [25, 45, 55]
numeros[2:5] = [25, 45, 55]
print("Después de cambiar rango:", numeros)
```

**Ejemplo de respuesta**:
```python
numeros = [10, 20, 30, 40, 50]
print("Lista original:", numeros)

numeros[2] = 35
print("Después de cambiar el tercer elemento:", numeros)

numeros.append(60)
print("Después de agregar 60:", numeros)

numeros.insert(1, 15)
print("Después de insertar 15:", numeros)

numeros.extend([70, 80, 90])
print("Después de extender:", numeros)

numeros[2:5] = [25, 45, 55]
print("Después de cambiar rango:", numeros)
```

---

### **Ejercicio 3: Eliminación de Elementos**
**Objetivo**: Practicar la eliminación de elementos de listas

**Instrucciones**: Elimina elementos usando diferentes métodos

**Tu código**:
```python
# Lista con elementos duplicados
numeros = [10, 20, 30, 20, 40, 50, 20, 60]
print("Lista original:", numeros)

# 1. Elimina el primer 20 usando remove()
numeros.remove(20)
print("Después de remove(20):", numeros)

# 2. Elimina y obtén el elemento en el índice 3 usando pop()
elemento_eliminado = numeros.pop(3)
print(f"Elemento eliminado: {elemento_eliminado}")
print("Después de pop(3):", numeros)

# 3. Elimina el elemento en el índice 1 usando del
del numeros[1]
print("Después de del numeros[1]:", numeros)

# 4. Elimina los elementos del índice 2 al 4
del numeros[2:5]
print("Después de eliminar rango:", numeros)

# 5. Limpia toda la lista
numeros.clear()
print("Después de clear():", numeros)
```

**Ejemplo de respuesta**:
```python
numeros = [10, 20, 30, 20, 40, 50, 20, 60]
print("Lista original:", numeros)

numeros.remove(20)
print("Después de remove(20):", numeros)

elemento_eliminado = numeros.pop(3)
print(f"Elemento eliminado: {elemento_eliminado}")
print("Después de pop(3):", numeros)

del numeros[1]
print("Después de del numeros[1]:", numeros)

del numeros[2:5]
print("Después de eliminar rango:", numeros)

numeros.clear()
print("Después de clear():", numeros)
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Métodos de Listas**
**Objetivo**: Practicar el uso de métodos incorporados de listas

**Instrucciones**: Usa diferentes métodos para manipular listas

**Tu código**:
```python
# Lista desordenada
numeros = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
print("Lista original:", numeros)

# 1. Cuenta cuántas veces aparece el número 5
cantidad_cincos = numeros.count(5)
print(f"El número 5 aparece {cantidad_cincos} veces")

# 2. Encuentra el índice del primer 1
indice_uno = numeros.index(1)
print(f"El primer 1 está en el índice {indice_uno}")

# 3. Ordena la lista de menor a mayor
numeros.sort()
print("Lista ordenada:", numeros)

# 4. Invierte el orden de la lista
numeros.reverse()
print("Lista invertida:", numeros)

# 5. Crea una nueva lista ordenada sin modificar la original
numeros_original = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
numeros_ordenados = sorted(numeros_original)
print("Lista original:", numeros_original)
print("Nueva lista ordenada:", numeros_ordenados)
```

**Ejemplo de respuesta**:
```python
numeros = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
print("Lista original:", numeros)

cantidad_cincos = numeros.count(5)
print(f"El número 5 aparece {cantidad_cincos} veces")

indice_uno = numeros.index(1)
print(f"El primer 1 está en el índice {indice_uno}")

numeros.sort()
print("Lista ordenada:", numeros)

numeros.reverse()
print("Lista invertida:", numeros)

numeros_original = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]
numeros_ordenados = sorted(numeros_original)
print("Lista original:", numeros_original)
print("Nueva lista ordenada:", numeros_ordenados)
```

---

### **Ejercicio 5: Comprensión de Listas**
**Objetivo**: Practicar la creación de listas usando comprensión

**Instrucciones**: Crea listas usando comprensión de listas

**Tu código**:
```python
# 1. Crea una lista con los cuadrados de los números del 1 al 10
cuadrados = [x**2 for x in range(1, 11)]
print("Cuadrados:", cuadrados)

# 2. Crea una lista con los números pares del 2 al 20
pares = [x for x in range(2, 21, 2)]
print("Números pares:", pares)

# 3. Crea una lista con los números del 1 al 15 que son divisibles por 3
divisibles_tres = [x for x in range(1, 16) if x % 3 == 0]
print("Divisibles por 3:", divisibles_tres)

# 4. Crea una lista con las longitudes de estas palabras
palabras = ["Python", "programación", "listas", "ejercicios", "aprendizaje"]
longitudes = [len(palabra) for palabra in palabras]
print("Longitudes de palabras:", longitudes)

# 5. Crea una lista que transforme números: pares se multiplican por 2, impares por 3
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
transformados = [x*2 if x % 2 == 0 else x*3 for x in numeros]
print("Números transformados:", transformados)
```

**Ejemplo de respuesta**:
```python
cuadrados = [x**2 for x in range(1, 11)]
print("Cuadrados:", cuadrados)

pares = [x for x in range(2, 21, 2)]
print("Números pares:", pares)

divisibles_tres = [x for x in range(1, 16) if x % 3 == 0]
print("Divisibles por 3:", divisibles_tres)

palabras = ["Python", "programación", "listas", "ejercicios", "aprendizaje"]
longitudes = [len(palabra) for palabra in palabras]
print("Longitudes de palabras:", longitudes)

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
transformados = [x*2 if x % 2 == 0 else x*3 for x in numeros]
print("Números transformados:", transformados)
```

---

### **Ejercicio 6: Listas Anidadas**
**Objetivo**: Practicar el trabajo con listas que contienen otras listas

**Instrucciones**: Crea y manipula listas anidadas

**Tu código**:
```python
# 1. Crea una matriz 3x3 con números del 1 al 9
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print("Matriz 3x3:")
for fila in matriz:
    print(fila)

# 2. Accede al elemento en la fila 1, columna 2
elemento = matriz[1][2]
print(f"Elemento en fila 1, columna 2: {elemento}")

# 3. Cambia el elemento en la fila 0, columna 1 a 10
matriz[0][1] = 10
print("Matriz después del cambio:")
for fila in matriz:
    print(fila)

# 4. Crea una lista de listas con diferentes tamaños
irregular = [
    [1, 2],
    [3, 4, 5],
    [6],
    [7, 8, 9, 10]
]
print("Lista irregular:")
for i, sublista in enumerate(irregular):
    print(f"Lista {i}: {sublista} (longitud: {len(sublista)})")

# 5. Aplana la lista irregular en una sola lista
aplanada = [x for sublista in irregular for x in sublista]
print("Lista aplanada:", aplanada)
```

**Ejemplo de respuesta**:
```python
matriz = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print("Matriz 3x3:")
for fila in matriz:
    print(fila)

elemento = matriz[1][2]
print(f"Elemento en fila 1, columna 2: {elemento}")

matriz[0][1] = 10
print("Matriz después del cambio:")
for fila in matriz:
    print(fila)

irregular = [
    [1, 2],
    [3, 4, 5],
    [6],
    [7, 8, 9, 10]
]
print("Lista irregular:")
for i, sublista in enumerate(irregular):
    print(f"Lista {i}: {sublista} (longitud: {len(sublista)})")

aplanada = [x for sublista in irregular for x in sublista]
print("Lista aplanada:", aplanada)
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 7: Operaciones con Listas**
**Objetivo**: Practicar operaciones complejas con listas

**Instrucciones**: Realiza operaciones avanzadas con listas

**Tu código**:
```python
# Lista de estudiantes con información
estudiantes = [
    {"nombre": "Ana", "edad": 20, "notas": [85, 90, 78]},
    {"nombre": "Carlos", "edad": 22, "notas": [92, 88, 95]},
    {"nombre": "María", "edad": 19, "notas": [76, 85, 82]},
    {"nombre": "Luis", "edad": 21, "notas": [89, 91, 87]}
]

# 1. Calcula el promedio de notas de cada estudiante
for estudiante in estudiantes:
    promedio = sum(estudiante["notas"]) / len(estudiante["notas"])
    estudiante["promedio"] = round(promedio, 2)
    print(f"{estudiante['nombre']}: Promedio {estudiante['promedio']}")

# 2. Encuentra el estudiante con el mejor promedio
mejor_estudiante = max(estudiantes, key=lambda x: x["promedio"])
print(f"\nMejor estudiante: {mejor_estudiante['nombre']} con promedio {mejor_estudiante['promedio']}")

# 3. Crea una lista con solo los nombres de los estudiantes
nombres = [estudiante["nombre"] for estudiante in estudiantes]
print("Nombres de estudiantes:", nombres)

# 4. Filtra estudiantes mayores de 20 años
mayores_20 = [estudiante for estudiante in estudiantes if estudiante["edad"] > 20]
print(f"Estudiantes mayores de 20: {[e['nombre'] for e in mayores_20]}")

# 5. Ordena la lista por promedio de notas (descendente)
estudiantes_ordenados = sorted(estudiantes, key=lambda x: x["promedio"], reverse=True)
print("\nEstudiantes ordenados por promedio:")
for estudiante in estudiantes_ordenados:
    print(f"{estudiante['nombre']}: {estudiante['promedio']}")
```

**Ejemplo de respuesta**:
```python
estudiantes = [
    {"nombre": "Ana", "edad": 20, "notas": [85, 90, 78]},
    {"nombre": "Carlos", "edad": 22, "notas": [92, 88, 95]},
    {"nombre": "María", "edad": 19, "notas": [76, 85, 82]},
    {"nombre": "Luis", "edad": 21, "notas": [89, 91, 87]}
]

for estudiante in estudiantes:
    promedio = sum(estudiante["notas"]) / len(estudiante["notas"])
    estudiante["promedio"] = round(promedio, 2)
    print(f"{estudiante['nombre']}: Promedio {estudiante['promedio']}")

mejor_estudiante = max(estudiantes, key=lambda x: x["promedio"])
print(f"\nMejor estudiante: {mejor_estudiante['nombre']} con promedio {mejor_estudiante['promedio']}")

nombres = [estudiante["nombre"] for estudiante in estudiantes]
print("Nombres de estudiantes:", nombres)

mayores_20 = [estudiante for estudiante in estudiantes if estudiante["edad"] > 20]
print(f"Estudiantes mayores de 20: {[e['nombre'] for e in mayores_20]}")

estudiantes_ordenados = sorted(estudiantes, key=lambda x: x["promedio"], reverse=True)
print("\nEstudiantes ordenados por promedio:")
for estudiante in estudiantes_ordenados:
    print(f"{estudiante['nombre']}: {estudiante['promedio']}")
```

---

### **Ejercicio 8: Algoritmos con Listas**
**Objetivo**: Implementar algoritmos básicos usando listas

**Instrucciones**: Implementa algoritmos de búsqueda y ordenamiento

**Tu código**:
```python
# Lista para probar algoritmos
numeros = [64, 34, 25, 12, 22, 11, 90]

# 1. Implementa búsqueda lineal
def busqueda_lineal(lista, elemento):
    for i, valor in enumerate(lista):
        if valor == elemento:
            return i
    return -1

# 2. Implementa búsqueda binaria (lista debe estar ordenada)
def busqueda_binaria(lista, elemento):
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

# 3. Implementa ordenamiento por burbuja
def ordenamiento_burbuja(lista):
    n = len(lista)
    for i in range(n):
        for j in range(0, n - i - 1):
            if lista[j] > lista[j + 1]:
                lista[j], lista[j + 1] = lista[j + 1], lista[j]
    return lista

# 4. Prueba los algoritmos
print("Lista original:", numeros)

# Búsqueda lineal
indice_22 = busqueda_lineal(numeros, 22)
print(f"Búsqueda lineal: 22 está en el índice {indice_22}")

# Ordenar para búsqueda binaria
numeros_ordenados = ordenamiento_burbuja(numeros.copy())
print("Lista ordenada:", numeros_ordenados)

# Búsqueda binaria
indice_22_bin = busqueda_binaria(numeros_ordenados, 22)
print(f"Búsqueda binaria: 22 está en el índice {indice_22_bin}")

# 5. Encuentra el segundo elemento más grande
numeros_ordenados.sort(reverse=True)
segundo_mayor = numeros_ordenados[1]
print(f"Segundo elemento más grande: {segundo_mayor}")
```

**Ejemplo de respuesta**:
```python
numeros = [64, 34, 25, 12, 22, 11, 90]

def busqueda_lineal(lista, elemento):
    for i, valor in enumerate(lista):
        if valor == elemento:
            return i
    return -1

def busqueda_binaria(lista, elemento):
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
    n = len(lista)
    for i in range(n):
        for j in range(0, n - i - 1):
            if lista[j] > lista[j + 1]:
                lista[j], lista[j + 1] = lista[j + 1], lista[j]
    return lista

print("Lista original:", numeros)

indice_22 = busqueda_lineal(numeros, 22)
print(f"Búsqueda lineal: 22 está en el índice {indice_22}")

numeros_ordenados = ordenamiento_burbuja(numeros.copy())
print("Lista ordenada:", numeros_ordenados)

indice_22_bin = busqueda_binaria(numeros_ordenados, 22)
print(f"Búsqueda binaria: 22 está en el índice {indice_22_bin}")

numeros_ordenados.sort(reverse=True)
segundo_mayor = numeros_ordenados[1]
print(f"Segundo elemento más grande: {segundo_mayor}")
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 9: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores en listas

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
numeros = [1, 2, 3, 4, 5]

# Error 1: Índice fuera de rango
print(numeros[5])

# Error 2: Método incorrecto
numeros.add(6)

# Error 3: Slicing incorrecto
sub_lista = numeros[1:3:0]

# Error 4: Comprensión mal formada
cuadrados = [x^2 for x in numeros]

# Error 5: Modificación durante iteración
for i in range(len(numeros)):
    if numeros[i] % 2 == 0:
        numeros.remove(numeros[i])
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
numeros = [1, 2, 3, 4, 5]

# Corregido: Índice válido
print(numeros[4])  # Último elemento

# Corregido: Método correcto
numeros.append(6)

# Corregido: Slicing válido
sub_lista = numeros[1:3]

# Corregido: Operador correcto
cuadrados = [x**2 for x in numeros]

# Corregido: Crear nueva lista en lugar de modificar durante iteración
numeros_pares = [x for x in numeros if x % 2 == 0]
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Creación y acceso a listas completado
- [ ] Ejercicio 2: Modificación de listas completado
- [ ] Ejercicio 3: Eliminación de elementos completado
- [ ] Ejercicio 4: Métodos de listas completado
- [ ] Ejercicio 5: Comprensión de listas completado
- [ ] Ejercicio 6: Listas anidadas completado
- [ ] Ejercicio 7: Operaciones con listas completado
- [ ] Ejercicio 8: Algoritmos con listas completado
- [ ] Ejercicio 9: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/3
- **Ejercicios avanzados**: ___/2
- **Depuración**: ___/1

**Puntuación Total**: ___/9

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Lista de Números Primos**
Crea una función que genere una lista de números primos hasta un límite dado usando comprensión de listas.

### **Desafío 2: Matriz Transpuesta**
Implementa una función que transponga una matriz (intercambie filas por columnas).

### **Desafío 3: Lista de Permutaciones**
Crea una función que genere todas las permutaciones posibles de una lista de elementos.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy sobre listas en Python?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué operación con listas te resultó más difícil de entender?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que usarás las listas en proyectos futuros?**
```
[Escribe aquí tus planes]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [02-Ejercicios-Diccionarios.md](./02-ejercicios-diccionarios.md)

**En la siguiente lección practicarás con diccionarios, otra estructura de datos fundamental en Python.**

---

**¡Excelente trabajo! Has completado los ejercicios de listas. Ahora estás listo para explorar los diccionarios y expandir tu conocimiento de estructuras de datos.** 