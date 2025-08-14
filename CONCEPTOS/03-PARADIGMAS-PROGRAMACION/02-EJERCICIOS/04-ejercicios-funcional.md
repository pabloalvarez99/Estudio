# 04. Ejercicios de Programación Funcional

## 🎯 Ejercicios Básicos

### **Ejercicio 1: Funciones Puras**
```python
# ✅ Función pura
def cuadrado(x):
    return x ** 2

# ✅ Función pura
def suma(a, b):
    return a + b

# ❌ Función impura
contador = 0
def incrementar_contador():
    global contador
    contador += 1
    return contador

# Convertir a función pura
def incrementar(valor):
    return valor + 1
```

### **Ejercicio 2: Map, Filter, Reduce**
```python
from functools import reduce

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Usar map para duplicar números
duplicados = list(map(lambda x: x * 2, numeros))

# Usar filter para números pares
pares = list(filter(lambda x: x % 2 == 0, numeros))

# Usar reduce para suma
suma_total = reduce(lambda acc, x: acc + x, numeros, 0)

print(f"Duplicados: {duplicados}")
print(f"Pares: {pares}")
print(f"Suma total: {suma_total}")
```

### **Ejercicio 3: List Comprehensions**
```python
# Cuadrados de números pares
cuadrados_pares = [x ** 2 for x in range(1, 11) if x % 2 == 0]

# Diccionario de números y sus cuadrados
numeros_cuadrados = {x: x ** 2 for x in range(1, 6)}

# Set de vocales en una palabra
vocales = {letra for letra in "programacion funcional" if letra in "aeiou"}

print(f"Cuadrados pares: {cuadrados_pares}")
print(f"Números al cuadrado: {numeros_cuadrados}")
print(f"Vocales: {vocales}")
```

## 🔧 Ejercicios Intermedios

### **Ejercicio 4: Composición de Funciones**
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
print(f"Palabras procesadas: {resultado}")
```

### **Ejercicio 5: Funciones de Orden Superior**
```python
def aplicar_funcion(funcion, lista):
    return [funcion(elemento) for elemento in lista]

def filtrar_lista(predicado, lista):
    return [elemento for elemento in lista if predicado(elemento)]

def combinar_funciones(*funciones):
    def funcion_combinada(x):
        resultado = x
        for funcion in reversed(funciones):
            resultado = funcion(resultado)
        return resultado
    return funcion_combinada

# Uso
numeros = [1, 2, 3, 4, 5]

# Aplicar función
cuadrados = aplicar_funcion(lambda x: x ** 2, numeros)

# Filtrar lista
mayores_3 = filtrar_lista(lambda x: x > 3, numeros)

# Combinar funciones
f = combinar_funciones(lambda x: x + 1, lambda x: x * 2)
resultado = aplicar_funcion(f, numeros)

print(f"Cuadrados: {cuadrados}")
print(f"Mayores que 3: {mayores_3}")
print(f"Combinación: {resultado}")
```

## 🚀 Ejercicios Avanzados

### **Ejercicio 6: Sistema de Procesamiento de Datos**
```python
from dataclasses import dataclass
from typing import List, Callable
from functools import reduce

@dataclass
class Transaccion:
    id: str
    monto: float
    tipo: str
    categoria: str

# Funciones puras de procesamiento
def filtrar_por_tipo(tipo: str) -> Callable[[Transaccion], bool]:
    return lambda transaccion: transaccion.tipo == tipo

def filtrar_por_categoria(categoria: str) -> Callable[[Transaccion], bool]:
    return lambda transaccion: transaccion.categoria == categoria

def agrupar_por_categoria(transacciones: List[Transaccion]):
    def agregar_a_grupo(grupos, transaccion):
        categoria = transaccion.categoria
        if categoria not in grupos:
            grupos[categoria] = []
        grupos[categoria].append(transaccion)
        return grupos
    
    return reduce(agregar_a_grupo, transacciones, {})

# Datos de ejemplo
transacciones = [
    Transaccion("1", 100.0, "ingreso", "salario"),
    Transaccion("2", 50.0, "gasto", "comida"),
    Transaccion("3", 200.0, "ingreso", "bonus"),
    Transaccion("4", 75.0, "gasto", "transporte")
]

# Aplicar filtros
ingresos = list(filter(filtrar_por_tipo("ingreso"), transacciones))
gastos = list(filter(filtrar_por_tipo("gasto"), transacciones))

# Agrupar por categoría
grupos = agrupar_por_categoria(transacciones)

print(f"Ingresos: {len(ingresos)}")
print(f"Gastos: {len(gastos)}")
print(f"Grupos por categoría: {list(grupos.keys())}")
```

## 📝 Ejercicios de Práctica

### **Ejercicio 7: Implementa un Pipeline de Validación**
Crea un sistema de validación usando composición de funciones.

### **Ejercicio 8: Calculadora Funcional**
Implementa operaciones matemáticas usando funciones puras.

### **Ejercicio 9: Procesador de Texto**
Crea un procesador de texto usando programación funcional.

## 🔍 Preguntas de Reflexión

1. **¿Cuándo usar programación funcional vs POO?**
2. **¿Cómo manejar efectos secundarios en programación funcional?**
3. **¿Qué ventajas tiene la inmutabilidad?**

---

**Anterior**: [03. Ejercicios de Interfaces](./03-ejercicios-interfaces.md)  
**Siguiente**: [05. Ejercicios de Patrones](./05-ejercicios-patrones.md) 