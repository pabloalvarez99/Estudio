# 📖 Definición: Diccionarios en Python

## 🎯 Concepto Principal

### **¿Por qué son Importantes los Diccionarios en Python?**
Los diccionarios son la estructura de datos más versátil y poderosa de Python. Permiten mapear claves a valores de manera eficiente, proporcionando acceso rápido a la información y una forma natural de representar datos estructurados del mundo real.

### **Definición Técnica**
Un diccionario en Python es una colección no ordenada de pares clave-valor, donde cada clave es única y se mapea a un valor específico. Los diccionarios son mutables, heterogéneos y proporcionan acceso O(1) a los valores mediante sus claves.

---

## 🔍 ¿Qué es un Diccionario?

### **Definición Básica**
Un diccionario es una **colección de pares clave-valor** que:
- **NO tienen orden** definido (antes de Python 3.7)
- **NO permiten claves duplicadas**
- Se pueden **modificar** después de la creación (mutable)
- Se acceden mediante **claves** en lugar de índices
- Pueden contener valores de **diferentes tipos**
- Se crean usando **llaves** o la función `dict()`

### **Analogía del Mundo Real**
Imagina un **diccionario de idiomas**:
- Cada **palabra** es una clave
- Cada **definición** es un valor
- **NO puedes tener** la misma palabra dos veces
- Puedes **buscar** cualquier palabra rápidamente
- Puedes **agregar** nuevas palabras y definiciones
- Puedes **cambiar** las definiciones existentes

---

## 🏗️ Estructura de un Diccionario

### **Representación Visual**
```
Clave:     "nombre"    "edad"    "profesion"    "hobbies"
          ┌──────────┬──────────┬──────────────┬──────────┐
Valor:    │  "Ana"   │    25    │ "Ingeniera"  │ ["leer", │
          └──────────┴──────────┴──────────────┴──────────┘
          │           │          │              │ "viajar"]│
          └──────────┴──────────┴──────────────┴──────────┘
```

### **Características Clave**
- **No ordenado**: Los pares clave-valor no tienen posición fija (antes de Python 3.7)
- **Claves únicas**: Cada clave aparece solo una vez
- **Mutable**: Se pueden agregar, modificar y eliminar pares clave-valor
- **Heterogéneo**: Las claves y valores pueden ser de diferentes tipos
- **Eficiente**: O(1) para búsqueda, inserción y eliminación
- **Hashable**: Las claves deben ser inmutables

---

## 📊 Tipos de Diccionarios

### **1. Diccionarios Simples**
**Definición**: Diccionarios que mapean claves básicas a valores simples

**Ejemplos**:
```python
# Diccionario de información personal
persona = {
    "nombre": "Ana",
    "edad": 25,
    "ciudad": "Madrid",
    "profesion": "Ingeniera"
}

# Diccionario de configuración
config = {
    "host": "localhost",
    "puerto": 8080,
    "timeout": 30,
    "debug": True
}

# Diccionario de traducciones
traducciones = {
    "hola": "hello",
    "adios": "goodbye",
    "gracias": "thank you"
}

# Diccionario vacío
vacio = {}
```

**Uso común**: Almacenamiento de datos estructurados, configuraciones, mapeos simples

---

### **2. Diccionarios Anidados**
**Definición**: Diccionarios que contienen otros diccionarios como valores

**Ejemplos**:
```python
# Diccionario de estudiantes con información detallada
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
    }
}

# Diccionario de configuración de base de datos
db_config = {
    "desarrollo": {
        "host": "localhost",
        "puerto": 5432,
        "usuario": "dev_user",
        "password": "dev_pass"
    },
    "produccion": {
        "host": "prod-server.com",
        "puerto": 5432,
        "usuario": "prod_user",
        "password": "prod_pass"
    }
}
```

**Uso común**: Bases de datos simples, configuraciones complejas, estructuras jerárquicas

---

### **3. Diccionarios Especializados**
**Definición**: Diccionarios diseñados para propósitos específicos

**Ejemplos**:
```python
# Diccionario de contadores
contadores = {
    "exitos": 0,
    "fallos": 0,
    "total": 0
}

# Diccionario de caché
cache = {
    "usuario_123": {"datos": "info", "timestamp": 1647123456},
    "usuario_456": {"datos": "info", "timestamp": 1647123457}
}

# Diccionario de índices
indice_palabras = {
    "python": [1, 15, 23, 45],
    "programacion": [2, 8, 12, 34],
    "algoritmos": [5, 18, 29]
}
```

**Uso común**: Contadores, caché, índices, estadísticas

---

## 🔧 Operaciones con Diccionarios

### **1. Creación de Diccionarios**
```python
# Creación directa con llaves
dic1 = {"a": 1, "b": 2, "c": 3}

# Creación con dict()
dic2 = dict([("a", 1), ("b", 2), ("c", 3)])

# Creación con argumentos nombrados
dic3 = dict(a=1, b=2, c=3)

# Creación desde dos listas
claves = ["a", "b", "c"]
valores = [1, 2, 3]
dic4 = dict(zip(claves, valores))

# Creación con comprensión
dic5 = {x: x**2 for x in range(5)}

# Creación de diccionario vacío
vacio = {}
vacio = dict()
```

### **2. Acceso a Valores**
```python
persona = {
    "nombre": "Ana",
    "edad": 25,
    "profesion": "Ingeniera",
    "hobbies": ["leer", "viajar", "cocinar"]
}

# Acceso directo por clave
nombre = persona["nombre"]                    # "Ana"
edad = persona["edad"]                        # 25

# Acceso con get() (más seguro)
profesion = persona.get("profesion")          # "Ingeniera"
ciudad = persona.get("ciudad", "Desconocida") # "Desconocida" (valor por defecto)

# Acceso a valores anidados
primer_hobby = persona["hobbies"][0]          # "leer"

# Verificar si existe una clave
if "edad" in persona:
    print(f"Edad: {persona['edad']}")

# Obtener todas las claves y valores
claves = persona.keys()                       # dict_keys(['nombre', 'edad', 'profesion', 'hobbies'])
valores = persona.values()                    # dict_values(['Ana', 25, 'Ingeniera', ['leer', 'viajar', 'cocinar']])
items = persona.items()                       # dict_items([('nombre', 'Ana'), ('edad', 25), ...])
```

### **3. Modificación de Diccionarios**
```python
persona = {"nombre": "Ana", "edad": 25}

# Cambiar valor existente
persona["edad"] = 26

# Agregar nueva clave-valor
persona["ciudad"] = "Madrid"
persona["profesion"] = "Ingeniera"

# Actualizar con otro diccionario
persona.update({
    "telefono": "123-456-789",
    "email": "ana@email.com"
})

# Agregar múltiples elementos
persona.update(ciudad="Barcelona", pais="España")

# Eliminar clave-valor
del persona["telefono"]
valor_eliminado = persona.pop("email", "No encontrado")  # Con valor por defecto
persona.popitem()  # Elimina el último elemento (Python 3.7+)

# Limpiar todo el diccionario
persona.clear()
```

---

## 📈 Métodos de Diccionarios

### **Métodos de Acceso**
```python
mi_dict = {"a": 1, "b": 2, "c": 3}

# get(): Obtener valor con valor por defecto
valor = mi_dict.get("a")           # 1
valor = mi_dict.get("z", "No existe")  # "No existe"

# setdefault(): Obtener valor, crear si no existe
valor = mi_dict.setdefault("d", 4)  # 4, y se agrega al diccionario

# keys(), values(), items(): Obtener vistas
claves = list(mi_dict.keys())      # ['a', 'b', 'c', 'd']
valores = list(mi_dict.values())   # [1, 2, 3, 4]
items = list(mi_dict.items())      # [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```

### **Métodos de Modificación**
```python
mi_dict = {"a": 1, "b": 2}

# update(): Actualizar con otro diccionario
mi_dict.update({"c": 3, "d": 4})

# pop(): Eliminar y retornar valor
valor = mi_dict.pop("a")           # 1

# popitem(): Eliminar y retornar último elemento
clave, valor = mi_dict.popitem()   # ('d', 4)

# clear(): Limpiar todo
mi_dict.clear()
```

### **Métodos de Información**
```python
mi_dict = {"a": 1, "b": 2, "c": 3}

# len(): Longitud del diccionario
longitud = len(mi_dict)            # 3

# in: Verificar si existe una clave
existe = "a" in mi_dict            # True

# copy(): Crear copia superficial
copia = mi_dict.copy()

# fromkeys(): Crear diccionario con claves y valor por defecto
nuevo = dict.fromkeys(["x", "y", "z"], 0)  # {'x': 0, 'y': 0, 'z': 0}
```

---

## 🚀 Comprensión de Diccionarios

### **Comprensión Básica**
```python
# Crear diccionario de cuadrados
cuadrados = {x: x**2 for x in range(5)}
# Resultado: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Crear diccionario de longitudes de palabras
palabras = ["Python", "programacion", "diccionarios"]
longitudes = {palabra: len(palabra) for palabra in palabras}
# Resultado: {'Python': 6, 'programacion': 12, 'diccionarios': 13}

# Crear diccionario con condición
numeros = {x: x*2 for x in range(10) if x % 2 == 0}
# Resultado: {0: 0, 2: 4, 4: 8, 6: 12, 8: 16}
```

### **Comprensión con Condiciones**
```python
# Transformar valores según condición
numeros = {1: "uno", 2: "dos", 3: "tres", 4: "cuatro"}
transformados = {k: v.upper() if len(v) > 3 else v for k, v in numeros.items()}
# Resultado: {1: 'uno', 2: 'dos', 3: 'tres', 4: 'CUATRO'}

# Filtrar por valor
personas = {
    "Ana": 25,
    "Carlos": 30,
    "María": 22,
    "Luis": 35
}
mayores_25 = {k: v for k, v in personas.items() if v > 25}
# Resultado: {'Carlos': 30, 'Luis': 35}
```

---

## 🔍 Iteración y Búsqueda

### **Iteración Básica**
```python
persona = {
    "nombre": "Ana",
    "edad": 25,
    "profesion": "Ingeniera",
    "ciudad": "Madrid"
}

# Iterar sobre claves
for clave in persona:
    print(f"Clave: {clave}")

# Iterar sobre claves explícitamente
for clave in persona.keys():
    print(f"Clave: {clave}")

# Iterar sobre valores
for valor in persona.values():
    print(f"Valor: {valor}")

# Iterar sobre pares clave-valor
for clave, valor in persona.items():
    print(f"{clave}: {valor}")

# Iterar con enumerate
for i, (clave, valor) in enumerate(persona.items()):
    print(f"{i+1}. {clave}: {valor}")
```

### **Búsqueda y Filtrado**
```python
estudiantes = {
    "ana": {"edad": 20, "nota": 85},
    "carlos": {"edad": 22, "nota": 92},
    "maria": {"edad": 19, "nota": 78},
    "luis": {"edad": 21, "nota": 89}
}

# Encontrar estudiante con mejor nota
mejor_estudiante = max(estudiantes.items(), key=lambda x: x[1]["nota"])
print(f"Mejor estudiante: {mejor_estudiante[0]} con nota {mejor_estudiante[1]['nota']}")

# Filtrar estudiantes mayores de 20 años
mayores_20 = {k: v for k, v in estudiantes.items() if v["edad"] > 20}

# Encontrar promedio de notas
notas = [estudiante["nota"] for estudiante in estudiantes.values()]
promedio = sum(notas) / len(notas)

# Buscar estudiantes por nota mínima
nota_minima = 80
destacados = {k: v for k, v in estudiantes.items() if v["nota"] >= nota_minima}
```

---

## 📊 Ventajas y Desventajas

### **✅ Ventajas**
- **Acceso rápido**: O(1) para búsqueda, inserción y eliminación
- **Flexibilidad**: Estructura natural para datos del mundo real
- **Mutabilidad**: Se pueden modificar después de la creación
- **Heterogeneidad**: Claves y valores pueden ser de diferentes tipos
- **Funcionalidad rica**: Muchos métodos incorporados
- **JSON nativo**: Fácil conversión a/desde JSON

### **❌ Desventajas**
- **No ordenado**: Los pares clave-valor no tienen posición fija (antes de Python 3.7)
- **Memoria**: Puede usar más memoria que otras estructuras
- **Claves inmutables**: Las claves deben ser hashables
- **Complejidad**: Puede ser más complejo que estructuras lineales

---

## 🎯 Casos de Uso Comunes

### **1. Almacenamiento de Datos**
```python
# Base de datos simple
usuarios = {
    "ana123": {
        "nombre": "Ana García",
        "email": "ana@email.com",
        "edad": 25,
        "activo": True
    },
    "carlos456": {
        "nombre": "Carlos López",
        "email": "carlos@email.com",
        "edad": 30,
        "activo": False
    }
}

# Acceso a datos
if "ana123" in usuarios:
    usuario = usuarios["ana123"]
    print(f"Usuario: {usuario['nombre']}")
```

### **2. Configuraciones**
```python
# Configuración de aplicación
config = {
    "database": {
        "host": "localhost",
        "port": 5432,
        "name": "myapp",
        "user": "admin"
    },
    "server": {
        "host": "0.0.0.0",
        "port": 8000,
        "debug": True
    },
    "logging": {
        "level": "INFO",
        "file": "app.log"
    }
}

# Acceso a configuración
db_host = config["database"]["host"]
server_port = config["server"]["port"]
```

### **3. Contadores y Estadísticas**
```python
# Contador de palabras
texto = "hola mundo hola python mundo python hola"
palabras = texto.split()

contador = {}
for palabra in palabras:
    contador[palabra] = contador.get(palabra, 0) + 1

# Resultado: {'hola': 3, 'mundo': 2, 'python': 2}

# Encontrar palabra más frecuente
palabra_mas_frecuente = max(contador.items(), key=lambda x: x[1])
print(f"Palabra más frecuente: {palabra_mas_frecuente[0]} ({palabra_mas_frecuente[1]} veces)")
```

### **4. Caché y Memoria**
```python
# Caché simple
cache = {}

def fibonacci(n):
    if n in cache:
        return cache[n]
    
    if n <= 1:
        resultado = n
    else:
        resultado = fibonacci(n-1) + fibonacci(n-2)
    
    cache[n] = resultado
    return resultado

# Uso del caché
print(fibonacci(10))  # Primera llamada (lenta)
print(fibonacci(10))  # Segunda llamada (rápida, desde caché)
```

---

## 🔍 Comparación con Otras Estructuras

### **Diccionarios vs Listas**
| Aspecto | Diccionarios | Listas |
|---------|--------------|--------|
| **Acceso** | Por clave | Por índice |
| **Orden** | No ordenado (antes de 3.7) | Ordenado |
| **Búsqueda** | O(1) | O(n) |
| **Uso** | Datos estructurados | Secuencias ordenadas |
| **Memoria** | Más memoria | Menos memoria |

### **Diccionarios vs Sets**
| Aspecto | Diccionarios | Sets |
|---------|--------------|------|
| **Estructura** | Clave-valor | Solo valores |
| **Acceso** | Por clave | No indexable |
| **Duplicados** | Claves únicas | Valores únicos |
| **Uso** | Mapeo de datos | Conjuntos únicos |
| **Mutabilidad** | Mutable | Mutable |

---

## 🚀 Implementación Interna

### **Cómo Funcionan los Diccionarios**
```python
# Los diccionarios son tablas hash
# Cada clave se convierte en un hash para búsqueda rápida

mi_dict = {"a": 1, "b": 2, "c": 3}

# Internamente:
# - Se crea una tabla hash
# - Cada clave se convierte en hash
# - Las colisiones se manejan con listas enlazadas
# - Búsqueda, inserción y eliminación son O(1) en promedio
# - Python 3.7+ mantiene el orden de inserción
```

### **Optimizaciones**
- **Hash tables**: Estructura de datos subyacente para acceso rápido
- **Resizing**: La tabla se redimensiona automáticamente cuando es necesario
- **Memory layout**: Los pares clave-valor se almacenan de manera eficiente
- **Order preservation**: Python 3.7+ mantiene el orden de inserción

---

## 📚 Conceptos Clave a Recordar

- ✅ **Diccionario** = Colección de pares clave-valor
- ✅ **Claves únicas** = Cada clave aparece solo una vez
- ✅ **No ordenado** = Los pares no tienen posición fija (antes de Python 3.7)
- ✅ **Mutable** = Se pueden modificar después de la creación
- ✅ **Acceso rápido** = O(1) para búsqueda, inserción y eliminación
- ✅ **Hashable** = Las claves deben ser inmutables
- ✅ **Heterogéneo** = Claves y valores pueden ser de diferentes tipos

---

## 📝 Preguntas de Reflexión

1. **¿Por qué crees que los diccionarios son tan útiles en Python?**
2. **¿En qué situaciones preferirías usar un diccionario vs una lista?**
3. **¿Cómo crees que la estructura clave-valor facilita el acceso a datos?**
4. **¿Qué ventajas y desventajas ves en los diccionarios no ordenados?**

---

## 🚀 Próximo Paso

**Archivo siguiente**: [01-Ejercicios-Listas.md](./../02-EJERCICIOS/01-ejercicios-listas.md)

**En la siguiente lección practicarás con ejercicios sobre listas, aplicando los conceptos que has aprendido sobre estructuras de datos.**

---

**¡Excelente! Ahora entiendes cómo funcionan los diccionarios en Python. En la próxima lección pondrás en práctica todo lo aprendido con ejercicios prácticos que te ayudarán a dominar estas estructuras de datos fundamentales.** 