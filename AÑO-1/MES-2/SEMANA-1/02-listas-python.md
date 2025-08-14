# 🎯 Lección 1.2: Listas en Python

## 📚 Concepto Principal

### **¿Qué es una Lista?**
Una lista en Python es una estructura de datos que puede almacenar múltiples elementos de diferentes tipos. Es como una caja con compartimentos numerados donde puedes guardar, cambiar y acceder a la información.

### **Analogía Simple**
Imagina una estantería con libros:
- **Posición 0**: Primer libro (índice 0)
- **Posición 1**: Segundo libro (índice 1)
- **Posición 2**: Tercer libro (índice 2)
- Y así sucesivamente...

### **Características de las Listas**
- ✅ **Ordenadas**: Los elementos mantienen su posición
- ✅ **Mutables**: Puedes cambiar, agregar o eliminar elementos
- ✅ **Indexadas**: Accedes a elementos por su posición (0, 1, 2...)
- ✅ **Heterogéneas**: Pueden contener diferentes tipos de datos
- ✅ **Dinámicas**: Pueden crecer o reducirse

---

## 🛠️ Ejercicios Prácticos

### **Ejercicio 1: Crear y Acceder a Listas**
**Objetivo**: Practicar la creación de listas y acceso a elementos

**Instrucciones**: Completa el código para crear y mostrar listas

**Código a completar**:
```python
# 1. Crear una lista de frutas
frutas = ["manzana", "banana", "naranja", "uva"]
print("Lista de frutas:", frutas)

# 2. Acceder al primer elemento (índice 0)
primera_fruta = frutas[0]
print("Primera fruta:", primera_fruta)

# 3. Acceder al último elemento
ultima_fruta = frutas[-1]
print("Última fruta:", ultima_fruta)

# 4. Acceder al tercer elemento (índice 2)
tercera_fruta = frutas[2]
print("Tercera fruta:", tercera_fruta)

# 5. Crear una lista de números
numeros = [10, 20, 30, 40, 50]
print("Lista de números:", numeros)

# 6. Acceder a elementos específicos
primer_numero = numeros[0]
ultimo_numero = numeros[-1]
numero_medio = numeros[2]

print(f"Primer número: {primer_numero}")
print(f"Último número: {ultimo_numero}")
print(f"Número del medio: {numero_medio}")
```

**Tu turno - Crea estas listas**:
```python
# 1. Lista de colores
colores = 
print("Colores:", colores)

# 2. Lista de edades
edades = 
print("Edades:", edades)

# 3. Lista mixta (diferentes tipos)
mixta = 
print("Lista mixta:", mixta)

# 4. Accede a elementos específicos
primer_color = colores[0]
edad_especifica = edades[1]
elemento_mixto = mixta[2]

print(f"Primer color: {primer_color}")
print(f"Segunda edad: {edad_especifica}")
print(f"Tercer elemento mixto: {elemento_mixto}")
```

---

### **Ejercicio 2: Modificar Listas**
**Objetivo**: Aprender a cambiar elementos en listas

**Instrucciones**: Completa el código para modificar listas

**Código a completar**:
```python
# Lista inicial
estudiantes = ["Ana", "Carlos", "María", "Luis"]
print("Lista original:", estudiantes)

# 1. Cambiar el segundo elemento
estudiantes[1] = "Roberto"
print("Después de cambiar:", estudiantes)

# 2. Cambiar el último elemento
estudiantes[-1] = "Sofía"
print("Después de cambiar el último:", estudiantes)

# 3. Cambiar múltiples elementos
estudiantes[0:2] = ["Juan", "Carmen"]
print("Después de cambiar múltiples:", estudiantes)

# 4. Agregar un elemento al final
estudiantes.append("Diego")
print("Después de agregar:", estudiantes)

# 5. Insertar en posición específica
estudiantes.insert(1, "Elena")
print("Después de insertar:", estudiantes)
```

**Tu turno - Modifica esta lista**:
```python
# Lista de productos
productos = ["laptop", "mouse", "teclado", "monitor"]
print("Productos originales:", productos)

# 1. Cambia "mouse" por "webcam"
productos[1] = 
print("Después del cambio:", productos)

# 2. Agrega "auriculares" al final
productos.append()
print("Después de agregar:", productos)

# 3. Inserta "impresora" en la posición 2
productos.insert()
print("Después de insertar:", productos)

# 4. Cambia los dos primeros elementos
productos[0:2] = 
print("Después de cambiar múltiples:", productos)
```

---

### **Ejercicio 3: Operaciones con Listas**
**Objetivo**: Practicar operaciones comunes con listas

**Instrucciones**: Completa las operaciones listadas

**Código a completar**:
```python
# Lista de números para operaciones
numeros = [5, 2, 8, 1, 9, 3, 7, 4, 6]

print("Lista original:", numeros)

# 1. Longitud de la lista
longitud = len(numeros)
print("Longitud:", longitud)

# 2. Valor máximo
maximo = max(numeros)
print("Valor máximo:", maximo)

# 3. Valor mínimo
minimo = min(numeros)
print("Valor mínimo:", minimo)

# 4. Suma de todos los elementos
suma_total = sum(numeros)
print("Suma total:", suma_total)

# 5. Promedio
promedio = suma_total / longitud
print("Promedio:", promedio)

# 6. Ordenar la lista (ascendente)
numeros_ordenados = sorted(numeros)
print("Ordenados ascendente:", numeros_ordenados)

# 7. Ordenar la lista (descendente)
numeros_descendente = sorted(numeros, reverse=True)
print("Ordenados descendente:", numeros_descendente)
```

**Tu turno - Realiza estas operaciones**:
```python
# Lista de precios
precios = [25.50, 15.75, 32.00, 8.99, 45.25]
print("Precios:", precios)

# 1. Calcula la longitud
longitud_precios = 
print("Cantidad de productos:", longitud_precios)

# 2. Encuentra el precio más alto
precio_maximo = 
print("Precio más alto:", precio_maximo)

# 3. Encuentra el precio más bajo
precio_minimo = 
print("Precio más bajo:", precio_minimo)

# 4. Calcula el total
total_precios = 
print("Total:", total_precios)

# 5. Calcula el promedio
promedio_precios = 
print("Promedio:", promedio_precios)

# 6. Ordena de menor a mayor
precios_ordenados = 
print("Precios ordenados:", precios_ordenados)
```

---

### **Ejercicio 4: Slicing (Rebanado) de Listas**
**Objetivo**: Aprender a extraer porciones de listas

**Instrucciones**: Practica el slicing con diferentes rangos

**Código a completar**:
```python
# Lista para practicar slicing
letras = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J"]
print("Lista completa:", letras)

# 1. Primeros 3 elementos
primeros_tres = letras[0:3]
print("Primeros 3:", primeros_tres)

# 2. Últimos 3 elementos
ultimos_tres = letras[-3:]
print("Últimos 3:", ultimos_tres)

# 3. Elementos del medio (posiciones 3 a 6)
medio = letras[3:7]
print("Medio:", medio)

# 4. Cada segundo elemento
cada_segundo = letras[::2]
print("Cada segundo:", cada_segundo)

# 5. Lista en reversa
reversa = letras[::-1]
print("Reversa:", reversa)

# 6. Elementos de la posición 2 a 8, cada tercero
especial = letras[2:9:3]
print("Especial:", especial)
```

**Tu turno - Practica slicing**:
```python
# Lista de números para slicing
numeros = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
print("Números:", numeros)

# 1. Extrae los primeros 4 números
primeros_cuatro = 
print("Primeros 4:", primeros_cuatro)

# 2. Extrae los últimos 4 números
ultimos_cuatro = 
print("Últimos 4:", ultimos_cuatro)

# 3. Extrae del 30 al 70
rango_medio = 
print("Rango medio:", rango_medio)

# 4. Extrae cada tercer número
cada_tercero = 
print("Cada tercero:", cada_tercero)

# 5. Invierte la lista
invertida = 
print("Invertida:", invertida)
```

---

### **Ejercicio 5: Aplicación Práctica**
**Objetivo**: Crear un programa que gestione una lista de tareas

**Problema**: Crear un gestor de tareas simple

**Instrucciones**: Completa el código del gestor de tareas

**Código a completar**:
```python
# Gestor de Tareas Simple
tareas = []

def agregar_tarea(tarea):
    """Agrega una nueva tarea a la lista"""
    tareas.append(tarea)
    print(f"Tarea '{tarea}' agregada exitosamente")

def mostrar_tareas():
    """Muestra todas las tareas"""
    if len(tareas) == 0:
        print("No hay tareas pendientes")
    else:
        print("\n=== LISTA DE TAREAS ===")
        for i, tarea in enumerate(tareas, 1):
            print(f"{i}. {tarea}")

def eliminar_tarea(posicion):
    """Elimina una tarea por posición"""
    if 1 <= posicion <= len(tareas):
        tarea_eliminada = tareas.pop(posicion - 1)
        print(f"Tarea '{tarea_eliminada}' eliminada")
    else:
        print("Posición no válida")

def contar_tareas():
    """Cuenta el total de tareas"""
    return len(tareas)

# Programa principal
print("=== GESTOR DE TAREAS ===")

# Agregar algunas tareas
agregar_tarea("Estudiar Python")
agregar_tarea("Hacer ejercicio")
agregar_tarea("Comprar víveres")
agregar_tarea("Llamar al médico")

# Mostrar todas las tareas
mostrar_tareas()

# Mostrar estadísticas
total = contar_tareas()
print(f"\nTotal de tareas: {total}")

# Eliminar una tarea
eliminar_tarea(2)
mostrar_tareas()
```

**Tu turno - Mejora el gestor de tareas**:
```python
# Agrega estas funcionalidades:

# 1. Función para marcar tarea como completada
def marcar_completada(posicion):
    """Marca una tarea como completada"""
    if 1 <= posicion <= len(tareas):
        tarea = tareas[posicion - 1]
        tareas[posicion - 1] = f"✓ {tarea}"
        print(f"Tarea '{tarea}' marcada como completada")
    else:
        print("Posición no válida")

# 2. Función para buscar tareas
def buscar_tarea(palabra_clave):
    """Busca tareas que contengan una palabra"""
    tareas_encontradas = []
    for tarea in tareas:
        if palabra_clave.lower() in tarea.lower():
            tareas_encontradas.append(tarea)
    
    if tareas_encontradas:
        print(f"Tareas que contienen '{palabra_clave}':")
        for tarea in tareas_encontradas:
            print(f"- {tarea}")
    else:
        print(f"No se encontraron tareas con '{palabra_clave}'")

# 3. Prueba las nuevas funciones
print("\n=== PRUEBAS DE NUEVAS FUNCIONES ===")
marcar_completada(1)
buscar_tarea("ejercicio")
mostrar_tareas()
```

---

## 🔍 Conceptos Clave a Recordar

- ✅ **Lista** = Estructura de datos ordenada y mutable
- ✅ **Índice** = Posición del elemento (comienza en 0)
- ✅ **Slicing** = Extraer porciones de la lista [inicio:fin:paso]
- ✅ **append()** = Agregar elemento al final
- ✅ **insert()** = Insertar en posición específica
- ✅ **pop()** = Eliminar y retornar elemento
- ✅ **len()** = Longitud de la lista
- ✅ **max()** y **min()** = Valores máximo y mínimo
- ✅ **sum()** = Suma de elementos numéricos
- ✅ **sorted()** = Ordenar lista

---

## 📝 Tarea para la Próxima Lección

**Práctica**:
- Crea listas de diferentes tipos de datos
- Experimenta con slicing y operaciones
- Implementa el gestor de tareas completo
- Practica con listas anidadas

**Preparación**:
- Revisa los métodos de listas (append, remove, extend)
- Piensa en casos de uso para listas anidadas

---

## 🎯 Evaluación de la Lección

**Autoevaluación**:
- [ ] Entiendo qué es una lista en Python
- [ ] Puedo crear y acceder a elementos de listas
- [ ] Sé modificar listas (agregar, cambiar, eliminar)
- [ ] Puedo realizar operaciones básicas con listas
- [ ] Entiendo el concepto de slicing
- [ ] Estoy listo para operaciones avanzadas con listas

**Puntuación**: ___/10

---

**¡Excelente progreso! Ahora dominas las listas básicas en Python. En la próxima lección aprenderemos operaciones más avanzadas con listas.** 