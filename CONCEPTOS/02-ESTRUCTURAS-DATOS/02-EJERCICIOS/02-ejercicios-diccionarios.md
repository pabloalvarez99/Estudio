# 🛠️ Ejercicios Prácticos: Diccionarios en Python

## 🎯 Objetivo de la Lección
Practicar el uso de diccionarios en Python, incluyendo creación, manipulación, métodos, comprensión de diccionarios y operaciones avanzadas. Comprender cuándo y cómo usar diccionarios de manera eficiente.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Conceptos Clave de Diccionarios**
- **Estructura clave-valor**: Cada clave mapea a un valor específico
- **Claves únicas**: No pueden haber claves duplicadas
- **No ordenado**: Los pares no tienen posición fija (antes de Python 3.7)
- **Mutable**: Se pueden modificar después de la creación
- **Acceso rápido**: O(1) para búsqueda, inserción y eliminación
- **Hashable**: Las claves deben ser inmutables

### **Sintaxis Básica**
```python
# Creación
mi_dict = {"clave": "valor", "otra": 42}

# Acceso
valor = mi_dict["clave"]
valor_seguro = mi_dict.get("clave", "por_defecto")

# Modificación
mi_dict["nueva_clave"] = "nuevo_valor"
mi_dict.update({"otra_clave": "otro_valor"})
```

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Creación y Acceso a Diccionarios**
**Objetivo**: Practicar la creación de diccionarios y acceso a valores

**Instrucciones**: Crea diccionarios y accede a sus valores

**Tu código**:
```python
# 1. Crea un diccionario con información personal
persona = {
    "nombre": "Ana García",
    "edad": 25,
    "ciudad": "Madrid",
    "profesion": "Ingeniera",
    "hobbies": ["leer", "viajar", "cocinar"]
}

# 2. Crea un diccionario de configuración
config = {
    "host": "localhost",
    "puerto": 8080,
    "timeout": 30,
    "debug": True,
    "version": "1.0.0"
}

# 3. Accede a valores específicos
print("Nombre:", persona["nombre"])
print("Edad:", persona["edad"])
print("Primer hobby:", persona["hobbies"][0])

# 4. Usa get() para acceso seguro
ciudad = persona.get("ciudad", "Desconocida")
telefono = persona.get("telefono", "No disponible")
print("Ciudad:", ciudad)
print("Teléfono:", telefono)

# 5. Verifica si existen claves
if "profesion" in persona:
    print("Profesión:", persona["profesion"])

if "salario" not in persona:
    print("Salario no está definido")
```

**Ejemplo de respuesta**:
```python
persona = {
    "nombre": "Ana García",
    "edad": 25,
    "ciudad": "Madrid",
    "profesion": "Ingeniera",
    "hobbies": ["leer", "viajar", "cocinar"]
}

config = {
    "host": "localhost",
    "puerto": 8080,
    "timeout": 30,
    "debug": True,
    "version": "1.0.0"
}

print("Nombre:", persona["nombre"])
print("Edad:", persona["edad"])
print("Primer hobby:", persona["hobbies"][0])

ciudad = persona.get("ciudad", "Desconocida")
telefono = persona.get("telefono", "No disponible")
print("Ciudad:", ciudad)
print("Teléfono:", telefono)

if "profesion" in persona:
    print("Profesión:", persona["profesion"])

if "salario" not in persona:
    print("Salario no está definido")
```

---

### **Ejercicio 2: Modificación de Diccionarios**
**Objetivo**: Practicar la modificación de diccionarios existentes

**Instrucciones**: Modifica diccionarios usando diferentes métodos

**Tu código**:
```python
# Diccionario inicial
estudiante = {
    "nombre": "Carlos López",
    "edad": 20,
    "carrera": "Informática"
}
print("Estudiante original:", estudiante)

# 1. Cambia la edad a 21
estudiante["edad"] = 21
print("Después de cambiar edad:", estudiante)

# 2. Agrega información de contacto
estudiante["email"] = "carlos@email.com"
estudiante["telefono"] = "123-456-789"
print("Después de agregar contacto:", estudiante)

# 3. Actualiza con otro diccionario
estudiante.update({
    "semestre": 3,
    "promedio": 8.5,
    "activo": True
})
print("Después de update:", estudiante)

# 4. Agrega múltiples elementos
estudiante.update(universidad="UAM", año_ingreso=2022)
print("Después de agregar universidad:", estudiante)

# 5. Cambia múltiples valores a la vez
estudiante.update(edad=22, semestre=4)
print("Después de cambios múltiples:", estudiante)
```

**Ejemplo de respuesta**:
```python
estudiante = {
    "nombre": "Carlos López",
    "edad": 20,
    "carrera": "Informática"
}
print("Estudiante original:", estudiante)

estudiante["edad"] = 21
print("Después de cambiar edad:", estudiante)

estudiante["email"] = "carlos@email.com"
estudiante["telefono"] = "123-456-789"
print("Después de agregar contacto:", estudiante)

estudiante.update({
    "semestre": 3,
    "promedio": 8.5,
    "activo": True
})
print("Después de update:", estudiante)

estudiante.update(universidad="UAM", año_ingreso=2022)
print("Después de agregar universidad:", estudiante)

estudiante.update(edad=22, semestre=4)
print("Después de cambios múltiples:", estudiante)
```

---

### **Ejercicio 3: Eliminación de Elementos**
**Objetivo**: Practicar la eliminación de elementos de diccionarios

**Instrucciones**: Elimina elementos usando diferentes métodos

**Tu código**:
```python
# Diccionario con varios elementos
producto = {
    "id": "PROD001",
    "nombre": "Laptop Gaming",
    "precio": 1299.99,
    "categoria": "Electrónicos",
    "stock": 15,
    "disponible": True,
    "garantia": "2 años"
}
print("Producto original:", producto)

# 1. Elimina la garantía usando del
del producto["garantia"]
print("Después de eliminar garantía:", producto)

# 2. Elimina stock usando pop() y obtén el valor
stock_eliminado = producto.pop("stock")
print(f"Stock eliminado: {stock_eliminado}")
print("Después de eliminar stock:", producto)

# 3. Elimina disponible con pop() y valor por defecto
disponible = producto.pop("disponible", False)
print(f"Disponible eliminado: {disponible}")
print("Después de eliminar disponible:", producto)

# 4. Elimina el último elemento usando popitem()
ultimo_clave, ultimo_valor = producto.popitem()
print(f"Último elemento eliminado: {ultimo_clave}: {ultimo_valor}")
print("Después de popitem:", producto)

# 5. Limpia todo el diccionario
producto.clear()
print("Después de clear:", producto)
```

**Ejemplo de respuesta**:
```python
producto = {
    "id": "PROD001",
    "nombre": "Laptop Gaming",
    "precio": 1299.99,
    "categoria": "Electrónicos",
    "stock": 15,
    "disponible": True,
    "garantia": "2 años"
}
print("Producto original:", producto)

del producto["garantia"]
print("Después de eliminar garantía:", producto)

stock_eliminado = producto.pop("stock")
print(f"Stock eliminado: {stock_eliminado}")
print("Después de eliminar stock:", producto)

disponible = producto.pop("disponible", False)
print(f"Disponible eliminado: {disponible}")
print("Después de eliminar disponible:", producto)

ultimo_clave, ultimo_valor = producto.popitem()
print(f"Último elemento eliminado: {ultimo_clave}: {ultimo_valor}")
print("Después de popitem:", producto)

producto.clear()
print("Después de clear:", producto)
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Métodos de Diccionarios**
**Objetivo**: Practicar el uso de métodos incorporados de diccionarios

**Instrucciones**: Usa diferentes métodos para manipular diccionarios

**Tu código**:
```python
# Diccionario de ventas
ventas = {
    "enero": 1500,
    "febrero": 1800,
    "marzo": 2200,
    "abril": 1900,
    "mayo": 2500
}

# 1. Obtén todas las claves, valores e items
claves = list(ventas.keys())
valores = list(ventas.values())
items = list(ventas.items())

print("Claves:", claves)
print("Valores:", valores)
print("Items:", items)

# 2. Usa setdefault() para agregar un valor por defecto
junio = ventas.setdefault("junio", 0)
print(f"Valor de junio: {junio}")
print("Ventas después de setdefault:", ventas)

# 3. Crea una copia del diccionario
ventas_copia = ventas.copy()
ventas_copia["julio"] = 2800
print("Ventas original:", ventas)
print("Ventas copia:", ventas_copia)

# 4. Crea un diccionario con fromkeys()
meses = ["enero", "febrero", "marzo", "abril", "mayo", "junio"]
ventas_iniciales = dict.fromkeys(meses, 0)
print("Ventas iniciales:", ventas_iniciales)

# 5. Verifica si hay claves específicas
tiene_enero = "enero" in ventas
tiene_diciembre = "diciembre" in ventas
print(f"¿Tiene enero? {tiene_enero}")
print(f"¿Tiene diciembre? {tiene_diciembre}")
```

**Ejemplo de respuesta**:
```python
ventas = {
    "enero": 1500,
    "febrero": 1800,
    "marzo": 2200,
    "abril": 1900,
    "mayo": 2500
}

claves = list(ventas.keys())
valores = list(ventas.values())
items = list(ventas.items())

print("Claves:", claves)
print("Valores:", valores)
print("Items:", items)

junio = ventas.setdefault("junio", 0)
print(f"Valor de junio: {junio}")
print("Ventas después de setdefault:", ventas)

ventas_copia = ventas.copy()
ventas_copia["julio"] = 2800
print("Ventas original:", ventas)
print("Ventas copia:", ventas_copia)

meses = ["enero", "febrero", "marzo", "abril", "mayo", "junio"]
ventas_iniciales = dict.fromkeys(meses, 0)
print("Ventas iniciales:", ventas_iniciales)

tiene_enero = "enero" in ventas
tiene_diciembre = "diciembre" in ventas
print(f"¿Tiene enero? {tiene_enero}")
print(f"¿Tiene diciembre? {tiene_diciembre}")
```

---

### **Ejercicio 5: Comprensión de Diccionarios**
**Objetivo**: Practicar la creación de diccionarios usando comprensión

**Instrucciones**: Crea diccionarios usando comprensión de diccionarios

**Tu código**:
```python
# 1. Crea un diccionario con los cuadrados de números del 1 al 10
cuadrados = {x: x**2 for x in range(1, 11)}
print("Cuadrados:", cuadrados)

# 2. Crea un diccionario con las longitudes de estas palabras
palabras = ["Python", "programacion", "diccionarios", "ejercicios", "aprendizaje"]
longitudes = {palabra: len(palabra) for palabra in palabras}
print("Longitudes de palabras:", longitudes)

# 3. Crea un diccionario con números pares del 2 al 20
pares = {x: x for x in range(2, 21, 2)}
print("Números pares:", pares)

# 4. Crea un diccionario que transforme números: pares se multiplican por 2, impares por 3
numeros = {x: x*2 if x % 2 == 0 else x*3 for x in range(1, 11)}
print("Números transformados:", numeros)

# 5. Crea un diccionario con solo las palabras que tienen más de 5 letras
palabras_largas = {palabra: len(palabra) for palabra in palabras if len(palabra) > 5}
print("Palabras largas:", palabras_largas)
```

**Ejemplo de respuesta**:
```python
cuadrados = {x: x**2 for x in range(1, 11)}
print("Cuadrados:", cuadrados)

palabras = ["Python", "programacion", "diccionarios", "ejercicios", "aprendizaje"]
longitudes = {palabra: len(palabra) for palabra in palabras}
print("Longitudes de palabras:", longitudes)

pares = {x: x for x in range(2, 21, 2)}
print("Números pares:", pares)

numeros = {x: x*2 if x % 2 == 0 else x*3 for x in range(1, 11)}
print("Números transformados:", numeros)

palabras_largas = {palabra: len(palabra) for palabra in palabras if len(palabra) > 5}
print("Palabras largas:", palabras_largas)
```

---

### **Ejercicio 6: Diccionarios Anidados**
**Objetivo**: Practicar el trabajo con diccionarios que contienen otros diccionarios

**Instrucciones**: Crea y manipula diccionarios anidados

**Tu código**:
```python
# 1. Crea un diccionario de estudiantes con información detallada
estudiantes = {
    "ana": {
        "edad": 20,
        "carrera": "Informática",
        "notas": [85, 90, 78],
        "contacto": {
            "email": "ana@email.com",
            "telefono": "123-456-789"
        }
    },
    "carlos": {
        "edad": 22,
        "carrera": "Matemáticas",
        "notas": [92, 88, 95],
        "contacto": {
            "email": "carlos@email.com",
            "telefono": "987-654-321"
        }
    },
    "maria": {
        "edad": 19,
        "carrera": "Física",
        "notas": [76, 85, 82],
        "contacto": {
            "email": "maria@email.com",
            "telefono": "555-123-456"
        }
    }
}

# 2. Accede a información específica
print("Edad de Ana:", estudiantes["ana"]["edad"])
print("Primera nota de Carlos:", estudiantes["carlos"]["notas"][0])
print("Email de María:", estudiantes["maria"]["contacto"]["email"])

# 3. Calcula el promedio de notas de cada estudiante
for nombre, info in estudiantes.items():
    promedio = sum(info["notas"]) / len(info["notas"])
    info["promedio"] = round(promedio, 2)
    print(f"{nombre}: Promedio {info['promedio']}")

# 4. Encuentra el estudiante con mejor promedio
mejor_estudiante = max(estudiantes.items(), key=lambda x: x[1]["promedio"])
print(f"\nMejor estudiante: {mejor_estudiante[0]} con promedio {mejor_estudiante[1]['promedio']}")

# 5. Crea una lista con solo los nombres de los estudiantes
nombres = list(estudiantes.keys())
print("Nombres de estudiantes:", nombres)
```

**Ejemplo de respuesta**:
```python
estudiantes = {
    "ana": {
        "edad": 20,
        "carrera": "Informática",
        "notas": [85, 90, 78],
        "contacto": {
            "email": "ana@email.com",
            "telefono": "123-456-789"
        }
    },
    "carlos": {
        "edad": 22,
        "carrera": "Matemáticas",
        "notas": [92, 88, 95],
        "contacto": {
            "email": "carlos@email.com",
            "telefono": "987-654-321"
        }
    },
    "maria": {
        "edad": 19,
        "carrera": "Física",
        "notas": [76, 85, 82],
        "contacto": {
            "email": "maria@email.com",
            "telefono": "555-123-456"
        }
    }
}

print("Edad de Ana:", estudiantes["ana"]["edad"])
print("Primera nota de Carlos:", estudiantes["carlos"]["notas"][0])
print("Email de María:", estudiantes["maria"]["contacto"]["email"])

for nombre, info in estudiantes.items():
    promedio = sum(info["notas"]) / len(info["notas"])
    info["promedio"] = round(promedio, 2)
    print(f"{nombre}: Promedio {info['promedio']}")

mejor_estudiante = max(estudiantes.items(), key=lambda x: x[1]["promedio"])
print(f"\nMejor estudiante: {mejor_estudiante[0]} con promedio {mejor_estudiante[1]['promedio']}")

nombres = list(estudiantes.keys())
print("Nombres de estudiantes:", nombres)
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 7: Operaciones con Diccionarios**
**Objetivo**: Practicar operaciones complejas con diccionarios

**Instrucciones**: Realiza operaciones avanzadas con diccionarios

**Tu código**:
```python
# Diccionario de productos con información detallada
productos = {
    "laptop": {
        "nombre": "Laptop Gaming",
        "precio": 1299.99,
        "categoria": "Electrónicos",
        "stock": 15,
        "rating": 4.5
    },
    "mouse": {
        "nombre": "Mouse Gaming",
        "precio": 49.99,
        "categoria": "Accesorios",
        "stock": 50,
        "rating": 4.2
    },
    "teclado": {
        "nombre": "Teclado Mecánico",
        "precio": 89.99,
        "categoria": "Accesorios",
        "stock": 25,
        "rating": 4.7
    },
    "monitor": {
        "nombre": "Monitor 4K",
        "precio": 399.99,
        "categoria": "Electrónicos",
        "stock": 10,
        "rating": 4.8
    }
}

# 1. Encuentra el producto más caro
producto_mas_caro = max(productos.items(), key=lambda x: x[1]["precio"])
print(f"Producto más caro: {producto_mas_caro[1]['nombre']} - ${producto_mas_caro[1]['precio']}")

# 2. Encuentra el producto con mejor rating
mejor_rating = max(productos.items(), key=lambda x: x[1]["rating"])
print(f"Mejor rating: {mejor_rating[1]['nombre']} - {mejor_rating[1]['rating']}")

# 3. Calcula el valor total del inventario
valor_total = sum(producto["precio"] * producto["stock"] for producto in productos.values())
print(f"Valor total del inventario: ${valor_total:.2f}")

# 4. Agrupa productos por categoría
por_categoria = {}
for nombre, info in productos.items():
    categoria = info["categoria"]
    if categoria not in por_categoria:
        por_categoria[categoria] = []
    por_categoria[categoria].append(nombre)

print("Productos por categoría:")
for categoria, nombres in por_categoria.items():
    print(f"{categoria}: {', '.join(nombres)}")

# 5. Encuentra productos con stock bajo (menos de 20)
stock_bajo = {nombre: info for nombre, info in productos.items() if info["stock"] < 20}
print(f"\nProductos con stock bajo: {list(stock_bajo.keys())}")
```

**Ejemplo de respuesta**:
```python
productos = {
    "laptop": {
        "nombre": "Laptop Gaming",
        "precio": 1299.99,
        "categoria": "Electrónicos",
        "stock": 15,
        "rating": 4.5
    },
    "mouse": {
        "nombre": "Mouse Gaming",
        "precio": 49.99,
        "categoria": "Accesorios",
        "stock": 50,
        "rating": 4.2
    },
    "teclado": {
        "nombre": "Teclado Mecánico",
        "precio": 89.99,
        "categoria": "Accesorios",
        "stock": 25,
        "rating": 4.7
    },
    "monitor": {
        "nombre": "Monitor 4K",
        "precio": 399.99,
        "categoria": "Electrónicos",
        "stock": 10,
        "rating": 4.8
    }
}

producto_mas_caro = max(productos.items(), key=lambda x: x[1]["precio"])
print(f"Producto más caro: {producto_mas_caro[1]['nombre']} - ${producto_mas_caro[1]['precio']}")

mejor_rating = max(productos.items(), key=lambda x: x[1]["rating"])
print(f"Mejor rating: {mejor_rating[1]['nombre']} - {mejor_rating[1]['rating']}")

valor_total = sum(producto["precio"] * producto["stock"] for producto in productos.values())
print(f"Valor total del inventario: ${valor_total:.2f}")

por_categoria = {}
for nombre, info in productos.items():
    categoria = info["categoria"]
    if categoria not in por_categoria:
        por_categoria[categoria] = []
    por_categoria[categoria].append(nombre)

print("Productos por categoría:")
for categoria, nombres in por_categoria.items():
    print(f"{categoria}: {', '.join(nombres)}")

stock_bajo = {nombre: info for nombre, info in productos.items() if info["stock"] < 20}
print(f"\nProductos con stock bajo: {list(stock_bajo.keys())}")
```

---

### **Ejercicio 8: Algoritmos con Diccionarios**
**Objetivo**: Implementar algoritmos básicos usando diccionarios

**Instrucciones**: Implementa algoritmos de búsqueda y procesamiento

**Tu código**:
```python
# 1. Implementa un contador de palabras
def contar_palabras(texto):
    """Cuenta la frecuencia de cada palabra en un texto"""
    palabras = texto.lower().split()
    contador = {}
    
    for palabra in palabras:
        # Limpia la palabra de signos de puntuación
        palabra_limpia = palabra.strip(".,!?;:")
        if palabra_limpia:
            contador[palabra_limpia] = contador.get(palabra_limpia, 0) + 1
    
    return contador

# 2. Implementa búsqueda en diccionario
def buscar_por_valor(diccionario, valor_buscar):
    """Encuentra todas las claves que tienen un valor específico"""
    claves_encontradas = []
    
    for clave, valor in diccionario.items():
        if valor == valor_buscar:
            claves_encontradas.append(clave)
    
    return claves_encontradas

# 3. Implementa inversión de diccionario
def invertir_diccionario(diccionario):
    """Invierte un diccionario (claves se convierten en valores y viceversa)"""
    invertido = {}
    
    for clave, valor in diccionario.items():
        if valor not in invertido:
            invertido[valor] = []
        invertido[valor].append(clave)
    
    return invertido

# 4. Prueba los algoritmos
texto_ejemplo = "hola mundo hola python mundo python hola"
print("Texto:", texto_ejemplo)

# Contar palabras
frecuencias = contar_palabras(texto_ejemplo)
print("Frecuencias:", frecuencias)

# Encontrar palabra más frecuente
palabra_mas_frecuente = max(frecuencias.items(), key=lambda x: x[1])
print(f"Palabra más frecuente: '{palabra_mas_frecuente[0]}' ({palabra_mas_frecuente[1]} veces)")

# Buscar por valor
claves_con_2 = buscar_por_valor(frecuencias, 2)
print(f"Palabras que aparecen 2 veces: {claves_con_2}")

# Invertir diccionario
invertido = invertir_diccionario(frecuencias)
print("Diccionario invertido:", invertido)
```

**Ejemplo de respuesta**:
```python
def contar_palabras(texto):
    palabras = texto.lower().split()
    contador = {}
    
    for palabra in palabras:
        palabra_limpia = palabra.strip(".,!?;:")
        if palabra_limpia:
            contador[palabra_limpia] = contador.get(palabra_limpia, 0) + 1
    
    return contador

def buscar_por_valor(diccionario, valor_buscar):
    claves_encontradas = []
    
    for clave, valor in diccionario.items():
        if valor == valor_buscar:
            claves_encontradas.append(clave)
    
    return claves_encontradas

def invertir_diccionario(diccionario):
    invertido = {}
    
    for clave, valor in diccionario.items():
        if valor not in invertido:
            invertido[valor] = []
        invertido[valor].append(clave)
    
    return invertido

texto_ejemplo = "hola mundo hola python mundo python hola"
print("Texto:", texto_ejemplo)

frecuencias = contar_palabras(texto_ejemplo)
print("Frecuencias:", frecuencias)

palabra_mas_frecuente = max(frecuencias.items(), key=lambda x: x[1])
print(f"Palabra más frecuente: '{palabra_mas_frecuente[0]}' ({palabra_mas_frecuente[1]} veces)")

claves_con_2 = buscar_por_valor(frecuencias, 2)
print(f"Palabras que aparecen 2 veces: {claves_con_2}")

invertido = invertir_diccionario(frecuencias)
print("Diccionario invertido:", invertido)
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 9: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores en diccionarios

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
persona = {
    "nombre": "Ana",
    "edad": 25,
    "hobbies": ["leer", "viajar"]
}

# Error 1: Acceso a clave que no existe
print(persona["telefono"])

# Error 2: Modificación de clave inmutable
persona[["ciudad"]] = "Madrid"

# Error 3: Uso incorrecto de get()
telefono = persona.get("telefono")

# Error 4: Eliminación incorrecta
persona.remove("edad")

# Error 5: Iteración incorrecta
for clave, valor in persona:
    print(f"{clave}: {valor}")
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
persona = {
    "nombre": "Ana",
    "edad": 25,
    "hobbies": ["leer", "viajar"]
}

# Corregido: Acceso seguro con get()
print(persona.get("telefono", "No disponible"))

# Corregido: Clave como string
persona["ciudad"] = "Madrid"

# Corregido: Uso correcto de get() con valor por defecto
telefono = persona.get("telefono", "No disponible")

# Corregido: Eliminación correcta
del persona["edad"]

# Corregido: Iteración correcta con items()
for clave, valor in persona.items():
    print(f"{clave}: {valor}")
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Creación y acceso a diccionarios completado
- [ ] Ejercicio 2: Modificación de diccionarios completado
- [ ] Ejercicio 3: Eliminación de elementos completado
- [ ] Ejercicio 4: Métodos de diccionarios completado
- [ ] Ejercicio 5: Comprensión de diccionarios completado
- [ ] Ejercicio 6: Diccionarios anidados completado
- [ ] Ejercicio 7: Operaciones con diccionarios completado
- [ ] Ejercicio 8: Algoritmos con diccionarios completado
- [ ] Ejercicio 9: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/3
- **Ejercicios avanzados**: ___/2
- **Depuración**: ___/1

**Puntuación Total**: ___/9

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Sistema de Caché**
Implementa un sistema de caché simple usando diccionarios que almacene resultados de funciones y los reutilice.

### **Desafío 2: Base de Datos Simple**
Crea un sistema de base de datos simple que permita CRUD (Create, Read, Update, Delete) de registros usando diccionarios.

### **Desafío 3: Analizador de Configuración**
Implementa un analizador que lea archivos de configuración en formato diccionario y valide que todas las claves requeridas estén presentes.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy sobre diccionarios en Python?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué operación con diccionarios te resultó más difícil de entender?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que usarás los diccionarios en proyectos futuros?**
```
[Escribe aquí tus planes]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [03-Ejercicios-Tuplas-Sets.md](./03-ejercicios-tuplas-sets.md)

**En la siguiente lección practicarás con tuplas y sets, completando tu conocimiento de estructuras de datos en Python.**

---

**¡Excelente trabajo! Has completado los ejercicios de diccionarios. Ahora estás listo para explorar las tuplas y sets, que te darán aún más opciones para organizar y manipular tu información de manera eficiente.** 