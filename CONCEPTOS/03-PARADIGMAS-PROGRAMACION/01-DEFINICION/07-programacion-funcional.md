# 07. Programación Funcional

## 📖 ¿Qué es la Programación Funcional?

La **programación funcional** es un paradigma de programación que trata la computación como la evaluación de funciones matemáticas. Se enfoca en el uso de funciones puras, inmutabilidad y composición de funciones, evitando el estado mutable y los efectos secundarios.

## 🎯 Principios Fundamentales

### **1. Funciones Puras**
Una función pura siempre produce el mismo resultado para los mismos argumentos y no tiene efectos secundarios.

```python
# ✅ Función pura
def cuadrado(x):
    return x ** 2

# ✅ Función pura
def suma(a, b):
    return a + b

# ❌ Función impura (tiene efectos secundarios)
def cuadrado_con_print(x):
    print(f"Calculando cuadrado de {x}")  # Efecto secundario
    return x ** 2

# ❌ Función impura (depende de estado externo)
contador = 0
def incrementar_contador():
    global contador
    contador += 1  # Modifica estado externo
    return contador
```

### **2. Inmutabilidad**
Los datos no se modifican después de su creación, se crean nuevos datos.

```python
# ❌ Enfoque mutable
def agregar_usuario_mutable(usuarios, nombre):
    usuarios.append(nombre)  # Modifica la lista original
    return usuarios

# ✅ Enfoque inmutable
def agregar_usuario_inmutable(usuarios, nombre):
    return usuarios + [nombre]  # Crea una nueva lista

# Uso
usuarios_originales = ["Ana", "Bob"]
usuarios_nuevos = agregar_usuario_inmutable(usuarios_originales, "Carlos")

print(usuarios_originales)  # ["Ana", "Bob"] - No cambió
print(usuarios_nuevos)      # ["Ana", "Bob", "Carlos"] - Nueva lista
```

### **3. Funciones de Primera Clase**
Las funciones pueden ser tratadas como cualquier otro valor: pasadas como argumentos, retornadas por otras funciones, o asignadas a variables.

```python
def saludar(nombre):
    return f"¡Hola {nombre}!"

def despedir(nombre):
    return f"¡Adiós {nombre}!"

def procesar_nombre(funcion, nombre):
    return funcion(nombre)

# Pasar funciones como argumentos
print(procesar_nombre(saludar, "Ana"))    # ¡Hola Ana!
print(procesar_nombre(despedir, "Bob"))   # ¡Adiós Bob!

# Asignar funciones a variables
mi_funcion = saludar
print(mi_funcion("Carlos"))               # ¡Hola Carlos!

# Retornar funciones
def crear_saludo(tipo):
    if tipo == "formal":
        return lambda nombre: f"Estimado {nombre}, saludos cordiales"
    else:
        return lambda nombre: f"¡Qué tal {nombre}!"

saludo_formal = crear_saludo("formal")
saludo_informal = crear_saludo("informal")

print(saludo_formal("Dr. García"))        # Estimado Dr. García, saludos cordiales
print(saludo_informal("Juan"))            # ¡Qué tal Juan!
```

## 🔧 Funciones de Orden Superior

### **1. Map**
Aplica una función a cada elemento de una secuencia.

```python
# Forma tradicional
numeros = [1, 2, 3, 4, 5]
cuadrados = []
for num in numeros:
    cuadrados.append(num ** 2)

# Con map
cuadrados = list(map(lambda x: x ** 2, numeros))
print(cuadrados)  # [1, 4, 9, 16, 25]

# Con función definida
def cuadrado(x):
    return x ** 2

cuadrados = list(map(cuadrado, numeros))

# Múltiples secuencias
nombres = ["Ana", "Bob", "Carlos"]
edades = [25, 30, 35]
personas = list(map(lambda n, e: f"{n} tiene {e} años", nombres, edades))
print(personas)  # ['Ana tiene 25 años', 'Bob tiene 30 años', 'Carlos tiene 35 años']
```

### **2. Filter**
Filtra elementos de una secuencia basándose en una condición.

```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Números pares
pares = list(filter(lambda x: x % 2 == 0, numeros))
print(pares)  # [2, 4, 6, 8, 10]

# Números mayores que 5
mayores_5 = list(filter(lambda x: x > 5, numeros))
print(mayores_5)  # [6, 7, 8, 9, 10]

# Con función definida
def es_primo(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

primos = list(filter(es_primo, numeros))
print(primos)  # [2, 3, 5, 7]
```

### **3. Reduce**
Reduce una secuencia a un solo valor aplicando una función acumulativa.

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5]

# Suma de todos los números
suma_total = reduce(lambda acc, x: acc + x, numeros, 0)
print(suma_total)  # 15

# Producto de todos los números
producto_total = reduce(lambda acc, x: acc * x, numeros, 1)
print(producto_total)  # 120

# Encontrar el máximo
maximo = reduce(lambda acc, x: x if x > acc else acc, numeros)
print(maximo)  # 5

# Concatenar strings
palabras = ["Hola", " ", "Mundo", "!"]
texto = reduce(lambda acc, x: acc + x, palabras, "")
print(texto)  # Hola Mundo!
```

## 🎨 List Comprehensions

### **1. Sintaxis Básica**
```python
numeros = [1, 2, 3, 4, 5]

# Cuadrados
cuadrados = [x ** 2 for x in numeros]
print(cuadrados)  # [1, 4, 9, 16, 25]

# Números pares
pares = [x for x in numeros if x % 2 == 0]
print(pares)  # [2, 4]

# Combinación de transformación y filtrado
cuadrados_pares = [x ** 2 for x in numeros if x % 2 == 0]
print(cuadrados_pares)  # [4, 16]
```

### **2. List Comprehensions Anidadas**
```python
# Matriz 3x3
matriz = [[i + j for j in range(3)] for i in range(0, 9, 3)]
print(matriz)  # [[0, 1, 2], [3, 4, 5], [6, 7, 8]]

# Aplanar matriz
matriz_aplanada = [elemento for fila in matriz for elemento in fila]
print(matriz_aplanada)  # [0, 1, 2, 3, 4, 5, 6, 7, 8]

# Producto cartesiano
producto_cartesiano = [(x, y) for x in [1, 2, 3] for y in ['a', 'b']]
print(producto_cartesiano)  # [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b'), (3, 'a'), (3, 'b')]
```

### **3. Dict y Set Comprehensions**
```python
# Dictionary comprehension
nombres = ["Ana", "Bob", "Carlos"]
edades = [25, 30, 35]

personas = {nombre: edad for nombre, edad in zip(nombres, edades)}
print(personas)  # {'Ana': 25, 'Bob': 30, 'Carlos': 35}

# Con condición
personas_mayores = {nombre: edad for nombre, edad in personas.items() if edad > 28}
print(personas_mayores)  # {'Bob': 30, 'Carlos': 35}

# Set comprehension
vocales = {letra for letra in "programacion funcional" if letra in "aeiou"}
print(vocales)  # {'a', 'e', 'i', 'o', 'u'}
```

## 🔄 Recursión

### **1. Recursión Simple**
```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))  # 120

def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(10))  # 55
```

### **2. Recursión con Acumulador**
```python
def factorial_acumulador(n, acumulador=1):
    if n <= 1:
        return acumulador
    return factorial_acumulador(n - 1, n * acumulador)

print(factorial_acumulador(5))  # 120

def suma_lista_acumulador(lista, acumulador=0):
    if not lista:
        return acumulador
    return suma_lista_acumulador(lista[1:], acumulador + lista[0])

print(suma_lista_acumulador([1, 2, 3, 4, 5]))  # 15
```

### **3. Recursión de Cola**
```python
def factorial_cola(n, acumulador=1):
    if n <= 1:
        return acumulador
    return factorial_cola(n - 1, n * acumulador)

# En Python, la recursión de cola no está optimizada automáticamente
# pero es una buena práctica para claridad del código
```

## 🧩 Composición de Funciones

### **1. Composición Simple**
```python
def f(x):
    return x + 2

def g(x):
    return x * 3

def h(x):
    return x ** 2

# Composición manual
def composicion_manual(x):
    return f(g(h(x)))

# Composición con función helper
def componer(*funciones):
    def composicion(x):
        resultado = x
        for funcion in reversed(funciones):
            resultado = funcion(resultado)
        return resultado
    return composicion

# Crear función compuesta
funcion_compuesta = componer(f, g, h)
resultado = funcion_compuesta(2)
print(resultado)  # f(g(h(2))) = f(g(4)) = f(12) = 14
```

### **2. Pipeline de Funciones**
```python
def pipeline(*funciones):
    def ejecutar_pipeline(datos):
        resultado = datos
        for funcion in funciones:
            resultado = funcion(resultado)
        return resultado
    return ejecutar_pipeline

# Funciones de procesamiento
def limpiar_texto(texto):
    return texto.strip().lower()

def dividir_palabras(texto):
    return texto.split()

def filtrar_palabras_cortas(palabras):
    return [palabra for palabra in palabras if len(palabra) > 3]

def contar_palabras(palabras):
    return len(palabras)

# Crear pipeline
procesar_texto = pipeline(
    limpiar_texto,
    dividir_palabras,
    filtrar_palabras_cortas,
    contar_palabras
)

# Usar pipeline
texto = "  Hola Mundo Python Programacion Funcional  "
resultado = procesar_texto(texto)
print(resultado)  # 3 (solo palabras con más de 3 caracteres)
```

## 🎭 Funciones Lambda

### **1. Sintaxis Básica**
```python
# Función lambda simple
cuadrado = lambda x: x ** 2
print(cuadrado(5))  # 25

# Lambda con múltiples argumentos
suma = lambda a, b: a + b
print(suma(3, 7))  # 10

# Lambda con condicional
es_par = lambda x: True if x % 2 == 0 else False
print(es_par(4))  # True
print(es_par(7))  # False
```

### **2. Uso con Funciones de Orden Superior**
```python
numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filtrar y transformar
resultado = list(map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, numeros)))
print(resultado)  # [4, 16, 36, 64, 100]

# Ordenar con lambda
personas = [("Ana", 25), ("Bob", 30), ("Carlos", 20)]
personas_ordenadas = sorted(personas, key=lambda p: p[1])
print(personas_ordenadas)  # [('Carlos', 20), ('Ana', 25), ('Bob', 30)]

# Agrupar con lambda
from itertools import groupby
numeros_agrupados = groupby(sorted(numeros), key=lambda x: x % 2 == 0)
for es_par, grupo in numeros_agrupados:
    print(f"{'Pares' if es_par else 'Impares'}: {list(grupo)}")
```

## 🔒 Inmutabilidad y Estructuras de Datos

### **1. Tuplas Inmutables**
```python
# Tuplas son inmutables
coordenadas = (3, 4)
# coordenadas[0] = 5  # TypeError: 'tuple' object does not support item assignment

# Crear nueva tupla
nuevas_coordenadas = (5, 4)
print(nuevas_coordenadas)  # (5, 4)

# Desempaquetado
x, y = coordenadas
print(f"x: {x}, y: {y}")  # x: 3, y: 4
```

### **2. Named Tuples**
```python
from collections import namedtuple

# Definir estructura
Persona = namedtuple('Persona', ['nombre', 'edad', 'ciudad'])

# Crear instancias
ana = Persona("Ana", 25, "Madrid")
bob = Persona("Bob", 30, "Barcelona")

# Acceso por nombre
print(ana.nombre)    # Ana
print(ana.edad)      # 25
print(ana.ciudad)    # Madrid

# Inmutabilidad
# ana.edad = 26  # AttributeError: can't set attribute

# Crear nueva instancia
ana_nueva = ana._replace(edad=26)
print(ana_nueva)     # Persona(nombre='Ana', edad=26, ciudad='Madrid')
```

### **3. Dataclasses Inmutables**
```python
from dataclasses import dataclass
from typing import List

@dataclass(frozen=True)
class Usuario:
    nombre: str
    email: str
    roles: List[str]
    
    def tiene_rol(self, rol: str) -> bool:
        return rol in self.roles

# Crear usuario
usuario = Usuario("Ana", "ana@email.com", ["admin", "usuario"])

# Verificar roles
print(usuario.tiene_rol("admin"))     # True
print(usuario.tiene_rol("editor"))    # False

# Inmutabilidad
# usuario.nombre = "Ana García"  # FrozenInstanceError
```

## 🚀 Aplicación Práctica: Sistema de Procesamiento de Datos

### **Implementación Funcional**
```python
from dataclasses import dataclass
from typing import List, Dict, Callable
from functools import reduce
import json

@dataclass
class Transaccion:
    id: str
    monto: float
    tipo: str
    fecha: str
    categoria: str

@dataclass
class ResumenFinanciero:
    total_ingresos: float
    total_gastos: float
    balance: float
    categorias: Dict[str, float]

# Funciones puras de procesamiento
def filtrar_por_tipo(tipo: str) -> Callable[[Transaccion], bool]:
    return lambda transaccion: transaccion.tipo == tipo

def filtrar_por_categoria(categoria: str) -> Callable[[Transaccion], bool]:
    return lambda transaccion: transaccion.categoria == categoria

def filtrar_por_monto(minimo: float, maximo: float) -> Callable[[Transaccion], bool]:
    return lambda transaccion: minimo <= transaccion.monto <= maximo

def agrupar_por_categoria(transacciones: List[Transaccion]) -> Dict[str, List[Transaccion]]:
    def agregar_a_grupo(grupos: Dict[str, List[Transaccion]], transaccion: Transaccion):
        categoria = transaccion.categoria
        if categoria not in grupos:
            grupos[categoria] = []
        grupos[categoria].append(transaccion)
        return grupos
    
    return reduce(agregar_a_grupo, transacciones, {})

def calcular_total_categoria(transacciones: List[Transaccion]) -> float:
    return reduce(lambda acc, t: acc + t.monto, transacciones, 0.0)

def generar_resumen(transacciones: List[Transaccion]) -> ResumenFinanciero:
    # Filtrar por tipo
    ingresos = list(filter(filtrar_por_tipo("ingreso"), transacciones))
    gastos = list(filter(filtrar_por_tipo("gasto"), transacciones))
    
    # Calcular totales
    total_ingresos = calcular_total_categoria(ingresos)
    total_gastos = calcular_total_categoria(gastos)
    balance = total_ingresos - total_gastos
    
    # Agrupar por categoría
    grupos_categoria = agrupar_por_categoria(transacciones)
    resumen_categorias = {
        categoria: calcular_total_categoria(transacciones_grupo)
        for categoria, transacciones_grupo in grupos_categoria.items()
    }
    
    return ResumenFinanciero(
        total_ingresos=total_ingresos,
        total_gastos=total_gastos,
        balance=balance,
        categorias=resumen_categorias
    )

def aplicar_filtros(transacciones: List[Transaccion], *filtros: Callable) -> List[Transaccion]:
    """Aplica múltiples filtros a las transacciones"""
    def aplicar_filtro(lista: List[Transaccion], filtro: Callable) -> List[Transaccion]:
        return list(filter(filtro, lista))
    
    return reduce(aplicar_filtro, filtros, transacciones)

def generar_reporte(transacciones: List[Transaccion], filtros: List[Callable] = None) -> str:
    """Genera un reporte formateado de las transacciones"""
    if filtros:
        transacciones_filtradas = aplicar_filtros(transacciones, *filtros)
    else:
        transacciones_filtradas = transacciones
    
    resumen = generar_resumen(transacciones_filtradas)
    
    reporte = f"""
=== REPORTE FINANCIERO ===
Total Ingresos: ${resumen.total_ingresos:.2f}
Total Gastos: ${resumen.total_gastos:.2f}
Balance: ${resumen.balance:.2f}

Desglose por Categorías:
"""
    
    for categoria, monto in resumen.categorias.items():
        reporte += f"- {categoria}: ${monto:.2f}\n"
    
    return reporte

# Datos de ejemplo
transacciones = [
    Transaccion("1", 1000.0, "ingreso", "2024-01-01", "salario"),
    Transaccion("2", 500.0, "ingreso", "2024-01-02", "bonus"),
    Transaccion("3", 200.0, "gasto", "2024-01-03", "comida"),
    Transaccion("4", 150.0, "gasto", "2024-01-04", "transporte"),
    Transaccion("5", 300.0, "gasto", "2024-01-05", "entretenimiento"),
    Transaccion("6", 800.0, "ingreso", "2024-01-06", "freelance"),
]

# Generar reporte completo
print(generar_reporte(transacciones))

# Generar reporte con filtros
print("\n=== REPORTE SOLO GASTOS ===")
reporte_gastos = generar_reporte(
    transacciones,
    [filtrar_por_tipo("gasto")]
)
print(reporte_gastos)

# Generar reporte de categoría específica
print("\n=== REPORTE SOLO COMIDA ===")
reporte_comida = generar_reporte(
    transacciones,
    [filtrar_por_categoria("comida")]
)
print(reporte_comida)

# Análisis avanzado con composición de funciones
def analizar_tendencia(transacciones: List[Transaccion]) -> Dict[str, float]:
    """Analiza la tendencia de gastos por categoría"""
    grupos = agrupar_por_categoria(
        list(filter(filtrar_por_tipo("gasto"), transacciones))
    )
    
    return {
        categoria: calcular_total_categoria(transacciones_grupo)
        for categoria, transacciones_grupo in grupos.items()
    }

tendencias = analizar_tendencia(transacciones)
print("\n=== TENDENCIAS DE GASTOS ===")
for categoria, monto in tendencias.items():
    print(f"{categoria}: ${monto:.2f}")
```

## 🔍 Beneficios de la Programación Funcional

### **1. Código Más Predecible**
- Sin estado mutable, el comportamiento es más predecible
- Funciones puras son más fáciles de testear
- Menos bugs relacionados con estado compartido

### **2. Mejor Paralelización**
- Funciones puras pueden ejecutarse en paralelo
- Sin dependencias de estado, es más fácil distribuir trabajo
- Mejor rendimiento en sistemas multicore

### **3. Código Más Expresivo**
- Sintaxis concisa y legible
- Menos código boilerplate
- Mejor composición de funcionalidades

## ⚠️ Consideraciones y Limitaciones

### **1. Curva de Aprendizaje**
- Conceptos como monads pueden ser complejos
- Cambio de mentalidad desde programación imperativa
- Requiere práctica para pensar funcionalmente

### **2. Performance**
- Algunas operaciones pueden ser menos eficientes
- Creación de nuevos objetos en lugar de modificar existentes
- Overhead de llamadas a funciones

### **3. Debugging**
- Stack traces pueden ser más complejos
- Flujo de datos menos lineal
- Requiere herramientas específicas para debugging funcional

## 💡 Mejores Prácticas

### **1. Empezar Simple**
```python
# Comenzar con funciones puras simples
# Usar map, filter, reduce gradualmente
# Implementar list comprehensions
```

### **2. Evitar Efectos Secundarios**
```python
# No modificar variables globales
# No hacer I/O en funciones puras
# Retornar nuevos valores en lugar de modificar
```

### **3. Usar Funciones de Orden Superior**
```python
# Componer funciones pequeñas
# Crear pipelines de procesamiento
# Aprovechar la composición funcional
```

## 📚 Recursos para Aprender Más

### **Libros Recomendados**
- "Functional Programming in Python" - David Mertz
- "Real World Haskell" - Bryan O'Sullivan
- "Structure and Interpretation of Computer Programs" - Abelson & Sussman

### **Práctica Recomendada**
- Implementar algoritmos de ordenamiento funcional
- Crear un sistema de procesamiento de datos
- Desarrollar un DSL (Domain Specific Language) funcional

---

**Anterior**: [06. Patrones de Diseño](./06-patrones-diseno.md)  
**Siguiente**: [08. Programación Reactiva](./08-programacion-reactiva.md) 