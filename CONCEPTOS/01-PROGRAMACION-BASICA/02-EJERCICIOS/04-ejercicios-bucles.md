# 🛠️ Ejercicios Prácticos: Bucles en Python

## 🎯 Objetivo de la Lección
Profundizar en el uso de bucles en Python, incluyendo bucles anidados, comprensión de listas, y aplicaciones avanzadas. Comprender cuándo usar cada tipo de bucle y cómo optimizarlos.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Tipos de Bucles en Python**
- **`for`**: Para iterar sobre secuencias (listas, strings, rangos)
- **`while`**: Para repetir mientras una condición sea verdadera
- **`enumerate()`**: Para obtener índice y valor simultáneamente
- **`zip()`**: Para iterar sobre múltiples secuencias en paralelo
- **Comprensión de listas**: Para crear listas de manera concisa

### **Sintaxis Avanzada**
```python
# Enumerate
for indice, valor in enumerate(secuencia):
    print(f"Índice {indice}: {valor}")

# Zip
for a, b in zip(lista1, lista2):
    print(f"{a} - {b}")

# Comprensión de listas
cuadrados = [x**2 for x in range(10)]
```

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Bucle For con Enumerate**
**Objetivo**: Practicar el uso de enumerate para obtener índices

**Instrucciones**: Crea un programa que muestre la posición de cada elemento en una lista

**Tu código**:
```python
# Lista de países
paises = ["España", "México", "Argentina", "Colombia", "Chile"]

print("=== LISTA DE PAÍSES CON POSICIÓN ===")
# Escribe aquí tu código usando enumerate
for indice, pais in enumerate(paises, 1):
    print(f"{indice}. {pais}")

print("\n=== PAÍSES EN POSICIONES PARES ===")
# Escribe aquí tu código para mostrar solo países en posiciones pares
for indice, pais in enumerate(paises):
    if indice % 2 == 0:
        print(f"Posición {indice}: {pais}")
```

**Ejemplo de respuesta**:
```python
paises = ["España", "México", "Argentina", "Colombia", "Chile"]

print("=== LISTA DE PAÍSES CON POSICIÓN ===")
for indice, pais in enumerate(paises, 1):
    print(f"{indice}. {pais}")

print("\n=== PAÍSES EN POSICIONES PARES ===")
for indice, pais in enumerate(paises):
    if indice % 2 == 0:
        print(f"Posición {indice}: {pais}")
```

---

### **Ejercicio 2: Bucle For con Zip**
**Objetivo**: Practicar el uso de zip para iterar sobre múltiples listas

**Instrucciones**: Crea un programa que combine información de dos listas

**Tu código**:
```python
# Información de estudiantes
nombres = ["Ana", "Carlos", "María", "Luis", "Elena"]
edades = [22, 19, 25, 20, 23]

print("=== INFORMACIÓN DE ESTUDIANTES ===")
# Escribe aquí tu código usando zip
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")

print("\n=== ESTUDIANTES MAYORES DE 21 ===")
# Escribe aquí tu código para mostrar solo estudiantes mayores de 21
for nombre, edad in zip(nombres, edades):
    if edad > 21:
        print(f"{nombre} es mayor de 21 años")

print("\n=== PROMEDIO DE EDADES ===")
# Escribe aquí tu código para calcular el promedio de edades
suma_edades = 0
for edad in edades:
    suma_edades += edad
promedio = suma_edades / len(edades)
print(f"El promedio de edades es: {promedio:.1f} años")
```

**Ejemplo de respuesta**:
```python
nombres = ["Ana", "Carlos", "María", "Luis", "Elena"]
edades = [22, 19, 25, 20, 23]

print("=== INFORMACIÓN DE ESTUDIANTES ===")
for nombre, edad in zip(nombres, edades):
    print(f"{nombre} tiene {edad} años")

print("\n=== ESTUDIANTES MAYORES DE 21 ===")
for nombre, edad in zip(nombres, edades):
    if edad > 21:
        print(f"{nombre} es mayor de 21 años")

print("\n=== PROMEDIO DE EDADES ===")
suma_edades = 0
for edad in edades:
    suma_edades += edad
promedio = suma_edades / len(edades)
print(f"El promedio de edades es: {promedio:.1f} años")
```

---

### **Ejercicio 3: Bucles Anidados**
**Objetivo**: Practicar el uso de bucles dentro de bucles

**Instrucciones**: Crea un programa que genere una tabla de multiplicar

**Tu código**:
```python
# Generador de tabla de multiplicar
print("=== TABLA DE MULTIPLICAR ===")

# Escribe aquí tu código para generar la tabla del 1 al 5
for i in range(1, 6):
    print(f"\nTabla del {i}:")
    for j in range(1, 11):
        resultado = i * j
        print(f"  {i} x {j} = {resultado}")

print("\n=== PATRÓN DE ASTERISCOS ===")
# Escribe aquí tu código para generar un patrón triangular
for i in range(1, 6):
    for j in range(i):
        print("*", end="")
    print()  # Nueva línea
```

**Ejemplo de respuesta**:
```python
print("=== TABLA DE MULTIPLICAR ===")

for i in range(1, 6):
    print(f"\nTabla del {i}:")
    for j in range(1, 11):
        resultado = i * j
        print(f"  {i} x {j} = {resultado}")

print("\n=== PATRÓN DE ASTERISCOS ===")
for i in range(1, 6):
    for j in range(i):
        print("*", end="")
    print()
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Comprensión de Listas**
**Objetivo**: Practicar la creación de listas usando comprensión

**Instrucciones**: Crea listas usando comprensión de listas

**Tu código**:
```python
# Comprensión de listas básica
print("=== COMPRENSIÓN DE LISTAS BÁSICA ===")

# 1. Números del 1 al 10
numeros = [x for x in range(1, 11)]
print(f"Números del 1 al 10: {numeros}")

# 2. Cuadrados de números del 1 al 10
cuadrados = [x**2 for x in range(1, 11)]
print(f"Cuadrados: {cuadrados}")

# 3. Números pares del 2 al 20
pares = [x for x in range(2, 21, 2)]
print(f"Números pares: {pares}")

# 4. Números impares del 1 al 19
impares = [x for x in range(1, 20, 2)]
print(f"Números impares: {impares}")

# 5. Múltiplos de 3 del 3 al 30
multiplos_3 = [x for x in range(3, 31, 3)]
print(f"Múltiplos de 3: {multiplos_3}")

print("\n=== COMPRENSIÓN DE LISTAS CON CONDICIONES ===")

# 6. Números del 1 al 20 que son divisibles por 4
divisibles_4 = [x for x in range(1, 21) if x % 4 == 0]
print(f"Divisibles por 4: {divisibles_4}")

# 7. Números del 1 al 15 que son mayores a 8
mayores_8 = [x for x in range(1, 16) if x > 8]
print(f"Mayores a 8: {mayores_8}")

# 8. Números del 1 al 25 que son pares y mayores a 10
pares_mayores_10 = [x for x in range(1, 26) if x % 2 == 0 and x > 10]
print(f"Pares y mayores a 10: {pares_mayores_10}")
```

**Ejemplo de respuesta**:
```python
print("=== COMPRENSIÓN DE LISTAS BÁSICA ===")

numeros = [x for x in range(1, 11)]
print(f"Números del 1 al 10: {numeros}")

cuadrados = [x**2 for x in range(1, 11)]
print(f"Cuadrados: {cuadrados}")

pares = [x for x in range(2, 21, 2)]
print(f"Números pares: {pares}")

impares = [x for x in range(1, 20, 2)]
print(f"Números impares: {impares}")

multiplos_3 = [x for x in range(3, 31, 3)]
print(f"Múltiplos de 3: {multiplos_3}")

print("\n=== COMPRENSIÓN DE LISTAS CON CONDICIONES ===")

divisibles_4 = [x for x in range(1, 21) if x % 4 == 0]
print(f"Divisibles por 4: {divisibles_4}")

mayores_8 = [x for x in range(1, 16) if x > 8]
print(f"Mayores a 8: {mayores_8}")

pares_mayores_10 = [x for x in range(1, 26) if x % 2 == 0 and x > 10]
print(f"Pares y mayores a 10: {pares_mayores_10}")
```

---

### **Ejercicio 5: Bucles con Strings**
**Objetivo**: Practicar el procesamiento de strings usando bucles

**Instrucciones**: Crea un programa que analice y procese texto

**Tu código**:
```python
# Análisis de texto
texto = "Python es un lenguaje de programación versátil y poderoso"

print("=== ANÁLISIS DE TEXTO ===")
print(f"Texto original: {texto}")

# 1. Contar caracteres
total_caracteres = len(texto)
print(f"Total de caracteres: {total_caracteres}")

# 2. Contar espacios
espacios = 0
for caracter in texto:
    if caracter == " ":
        espacios += 1
print(f"Total de espacios: {espacios}")

# 3. Contar vocales
vocales = "aeiouAEIOU"
contador_vocales = 0
for caracter in texto:
    if caracter in vocales:
        contador_vocales += 1
print(f"Total de vocales: {contador_vocales}")

# 4. Contar consonantes
consonantes = 0
for caracter in texto:
    if caracter.isalpha() and caracter not in vocales:
        consonantes += 1
print(f"Total de consonantes: {consonantes}")

# 5. Palabras que empiezan con vocal
palabras = texto.split()
palabras_vocal = []
for palabra in palabras:
    if palabra[0].lower() in vocales:
        palabras_vocal.append(palabra)
print(f"Palabras que empiezan con vocal: {palabras_vocal}")

# 6. Longitud de cada palabra
print("\n=== LONGITUD DE CADA PALABRA ===")
for palabra in palabras:
    print(f"'{palabra}': {len(palabra)} caracteres")
```

**Ejemplo de respuesta**:
```python
texto = "Python es un lenguaje de programación versátil y poderoso"

print("=== ANÁLISIS DE TEXTO ===")
print(f"Texto original: {texto}")

total_caracteres = len(texto)
print(f"Total de caracteres: {total_caracteres}")

espacios = 0
for caracter in texto:
    if caracter == " ":
        espacios += 1
print(f"Total de espacios: {espacios}")

vocales = "aeiouAEIOU"
contador_vocales = 0
for caracter in texto:
    if caracter in vocales:
        contador_vocales += 1
print(f"Total de vocales: {contador_vocales}")

consonantes = 0
for caracter in texto:
    if caracter.isalpha() and caracter not in vocales:
        consonantes += 1
print(f"Total de consonantes: {consonantes}")

palabras = texto.split()
palabras_vocal = []
for palabra in palabras:
    if palabra[0].lower() in vocales:
        palabras_vocal.append(palabra)
print(f"Palabras que empiezan con vocal: {palabras_vocal}")

print("\n=== LONGITUD DE CADA PALABRA ===")
for palabra in palabras:
    print(f"'{palabra}': {len(palabra)} caracteres")
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 6: Bucle While Avanzado**
**Objetivo**: Practicar el uso avanzado del bucle while

**Instrucciones**: Crea un programa que simule un cajero automático

**Tu código**:
```python
# Simulador de cajero automático
saldo = 1000.0
opciones_validas = ["1", "2", "3", "4"]

print("=== CAJERO AUTOMÁTICO ==="")
print("1. Consultar saldo")
print("2. Depositar dinero")
print("3. Retirar dinero")
print("4. Salir")

# Escribe aquí tu código usando while
while True:
    print(f"\nSaldo actual: ${saldo:.2f}")
    opcion = input("Selecciona una opción (1-4): ")
    
    if opcion == "1":
        print(f"Tu saldo es: ${saldo:.2f}")
    
    elif opcion == "2":
        try:
            monto = float(input("Ingresa el monto a depositar: $"))
            if monto > 0:
                saldo += monto
                print(f"Depósito exitoso. Nuevo saldo: ${saldo:.2f}")
            else:
                print("El monto debe ser mayor a 0")
        except ValueError:
            print("Por favor ingresa un monto válido")
    
    elif opcion == "3":
        try:
            monto = float(input("Ingresa el monto a retirar: $"))
            if monto > 0 and monto <= saldo:
                saldo -= monto
                print(f"Retiro exitoso. Nuevo saldo: ${saldo:.2f}")
            elif monto > saldo:
                print("Saldo insuficiente")
            else:
                print("El monto debe ser mayor a 0")
        except ValueError:
            print("Por favor ingresa un monto válido")
    
    elif opcion == "4":
        print("¡Gracias por usar el cajero automático!")
        break
    
    else:
        print("Opción no válida. Por favor selecciona 1-4")
```

**Ejemplo de respuesta**:
```python
saldo = 1000.0
opciones_validas = ["1", "2", "3", "4"]

print("=== CAJERO AUTOMÁTICO ===")
print("1. Consultar saldo")
print("2. Depositar dinero")
print("3. Retirar dinero")
print("4. Salir")

while True:
    print(f"\nSaldo actual: ${saldo:.2f}")
    opcion = input("Selecciona una opción (1-4): ")
    
    if opcion == "1":
        print(f"Tu saldo es: ${saldo:.2f}")
    
    elif opcion == "2":
        try:
            monto = float(input("Ingresa el monto a depositar: $"))
            if monto > 0:
                saldo += monto
                print(f"Depósito exitoso. Nuevo saldo: ${saldo:.2f}")
            else:
                print("El monto debe ser mayor a 0")
        except ValueError:
            print("Por favor ingresa un monto válido")
    
    elif opcion == "3":
        try:
            monto = float(input("Ingresa el monto a retirar: $"))
            if monto > 0 and monto <= saldo:
                saldo -= monto
                print(f"Retiro exitoso. Nuevo saldo: ${saldo:.2f}")
            elif monto > saldo:
                print("Saldo insuficiente")
            else:
                print("El monto debe ser mayor a 0")
        except ValueError:
            print("Por favor ingresa un monto válido")
    
    elif opcion == "4":
        print("¡Gracias por usar el cajero automático!")
        break
    
    else:
        print("Opción no válida. Por favor selecciona 1-4")
```

---

### **Ejercicio 7: Bucles con Diccionarios**
**Objetivo**: Practicar el uso de bucles con diccionarios

**Instrucciones**: Crea un programa que procese información de productos

**Tu código**:
```python
# Sistema de inventario de productos
productos = {
    "laptop": {"precio": 1200, "stock": 15, "categoria": "tecnologia"},
    "mouse": {"precio": 25, "stock": 50, "categoria": "tecnologia"},
    "libro": {"precio": 30, "stock": 100, "categoria": "educacion"},
    "cafe": {"precio": 8, "stock": 200, "categoria": "alimentos"},
    "camisa": {"precio": 45, "stock": 75, "categoria": "ropa"}
}

print("=== INVENTARIO DE PRODUCTOS ===")

# 1. Mostrar todos los productos
for nombre, info in productos.items():
    print(f"{nombre}: ${info['precio']} - Stock: {info['stock']} - Categoría: {info['categoria']}")

# 2. Productos de tecnología
print("\n=== PRODUCTOS DE TECNOLOGÍA ===")
for nombre, info in productos.items():
    if info['categoria'] == 'tecnologia':
        print(f"{nombre}: ${info['precio']}")

# 3. Productos con stock bajo (menos de 20)
print("\n=== PRODUCTOS CON STOCK BAJO ===")
for nombre, info in productos.items():
    if info['stock'] < 20:
        print(f"{nombre}: Stock crítico ({info['stock']} unidades)")

# 4. Valor total del inventario
print("\n=== VALOR TOTAL DEL INVENTARIO ===")
valor_total = 0
for nombre, info in productos.items():
    valor_producto = info['precio'] * info['stock']
    valor_total += valor_producto
    print(f"{nombre}: ${valor_producto:.2f}")

print(f"\nValor total del inventario: ${valor_total:.2f}")

# 5. Productos ordenados por precio
print("\n=== PRODUCTOS ORDENADOS POR PRECIO ===")
productos_ordenados = sorted(productos.items(), key=lambda x: x[1]['precio'])
for nombre, info in productos_ordenados:
    print(f"{nombre}: ${info['precio']}")
```

**Ejemplo de respuesta**:
```python
productos = {
    "laptop": {"precio": 1200, "stock": 15, "categoria": "tecnologia"},
    "mouse": {"precio": 25, "stock": 50, "categoria": "tecnologia"},
    "libro": {"precio": 30, "stock": 100, "categoria": "educacion"},
    "cafe": {"precio": 8, "stock": 200, "categoria": "alimentos"},
    "camisa": {"precio": 45, "stock": 75, "categoria": "ropa"}
}

print("=== INVENTARIO DE PRODUCTOS ===")

for nombre, info in productos.items():
    print(f"{nombre}: ${info['precio']} - Stock: {info['stock']} - Categoría: {info['categoria']}")

print("\n=== PRODUCTOS DE TECNOLOGÍA ===")
for nombre, info in productos.items():
    if info['categoria'] == 'tecnologia':
        print(f"{nombre}: ${info['precio']}")

print("\n=== PRODUCTOS CON STOCK BAJO ===")
for nombre, info in productos.items():
    if info['stock'] < 20:
        print(f"{nombre}: Stock crítico ({info['stock']} unidades)")

print("\n=== VALOR TOTAL DEL INVENTARIO ===")
valor_total = 0
for nombre, info in productos.items():
    valor_producto = info['precio'] * info['stock']
    valor_total += valor_producto
    print(f"{nombre}: ${valor_producto:.2f}")

print(f"\nValor total del inventario: ${valor_total:.2f}")

print("\n=== PRODUCTOS ORDENADOS POR PRECIO ===")
productos_ordenados = sorted(productos.items(), key=lambda x: x[1]['precio'])
for nombre, info in productos_ordenados:
    print(f"{nombre}: ${info['precio']}")
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 8: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores en bucles

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
numeros = [1, 2, 3, 4, 5]

# Error 1: Bucle for mal formado
for i in numeros
    print(i * 2)

# Error 2: Bucle while infinito
i = 0
while i < len(numeros):
    print(numeros[i])
    # Falta incrementar i

# Error 3: Comprensión de lista mal formada
cuadrados = [x**2 for x in numeros if x > 2]
print(cuadrados)

# Error 4: Bucle anidado mal indentado
for i in range(3):
for j in range(3):
    print(f"({i}, {j})")

# Error 5: Uso incorrecto de enumerate
for indice, valor in enumerate(numeros, 1):
print(f"{indice}: {valor}")
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
numeros = [1, 2, 3, 4, 5]

# Corregido: Agregar dos puntos
for i in numeros:
    print(i * 2)

# Corregido: Agregar incremento
i = 0
while i < len(numeros):
    print(numeros[i])
    i += 1

# Corregido: Comprensión de lista correcta
cuadrados = [x**2 for x in numeros if x > 2]
print(cuadrados)

# Corregido: Indentación correcta
for i in range(3):
    for j in range(3):
        print(f"({i}, {j})")

# Corregido: Indentación correcta
for indice, valor in enumerate(numeros, 1):
    print(f"{indice}: {valor}")
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Bucle for con enumerate completado
- [ ] Ejercicio 2: Bucle for con zip completado
- [ ] Ejercicio 3: Bucles anidados completado
- [ ] Ejercicio 4: Comprensión de listas completado
- [ ] Ejercicio 5: Bucles con strings completado
- [ ] Ejercicio 6: Bucle while avanzado completado
- [ ] Ejercicio 7: Bucles con diccionarios completado
- [ ] Ejercicio 8: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/2
- **Ejercicios avanzados**: ___/2
- **Depuración**: ___/1

**Puntuación Total**: ___/8

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Generador de Matrices**
Crea un programa que genere matrices usando bucles anidados y las muestre de manera visual.

### **Desafío 2: Analizador de Texto Avanzado**
Implementa un programa que analice un texto completo y genere estadísticas detalladas.

### **Desafío 3: Simulador de Juegos**
Crea un juego simple (como piedra, papel o tijeras) usando bucles y estructuras de control.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy sobre bucles?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué tipo de bucle te resultó más difícil de entender?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que usarás estos bucles en proyectos futuros?**
```
[Escribe aquí tus planes]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [05-Ejercicios-Integracion.md](./05-ejercicios-integracion.md)

**En la siguiente lección practicarás la integración de todos los conceptos aprendidos en ejercicios complejos.**

---

**¡Excelente trabajo! Has completado los ejercicios avanzados de bucles. Ahora estás listo para integrar todos los conceptos en ejercicios complejos.** 