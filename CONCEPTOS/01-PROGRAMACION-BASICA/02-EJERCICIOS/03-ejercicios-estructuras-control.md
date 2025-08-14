# 🛠️ Ejercicios Prácticos: Estructuras de Control

## 🎯 Objetivo de la Lección
Practicar el uso de estructuras de control en Python: condicionales (if-else), bucles (while, for) y control de flujo. Comprender cuándo y cómo usar cada estructura.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Estructuras de Control en Python**
- **Condicionales**: `if`, `elif`, `else`
- **Bucles**: `while`, `for`
- **Control de Flujo**: `break`, `continue`, `pass`
- **Operadores de Comparación**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Operadores Lógicos**: `and`, `or`, `not`

### **Sintaxis Básica**
```python
# Condicionales
if condicion:
    # código si es verdadero
elif otra_condicion:
    # código si es verdadero
else:
    # código por defecto

# Bucles
while condicion:
    # código que se repite

for elemento in secuencia:
    # código para cada elemento
```

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Condicionales Básicas**
**Objetivo**: Practicar el uso de if-elif-else

**Instrucciones**: Crea un programa que determine el estado de un estudiante según su calificación

**Tu código**:
```python
# Programa para determinar el estado del estudiante
calificacion = float(input("Ingresa la calificación del estudiante (0-10): "))

# Escribe aquí tu código con condicionales
if calificacion >= 9.0:
    print("Excelente - Aprobado")
elif calificacion >= 7.0:
    print("Bueno - Aprobado")
elif calificacion >= 6.0:
    print("Suficiente - Aprobado")
elif calificacion >= 0:
    print("Insuficiente - Reprobado")
else:
    print("Calificación inválida")
```

**Ejemplo de respuesta**:
```python
calificacion = float(input("Ingresa la calificación del estudiante (0-10): "))

if calificacion >= 9.0:
    print("Excelente - Aprobado")
elif calificacion >= 7.0:
    print("Bueno - Aprobado")
elif calificacion >= 6.0:
    print("Suficiente - Aprobado")
elif calificacion >= 0:
    print("Insuficiente - Reprobado")
else:
    print("Calificación inválida")
```

---

### **Ejercicio 2: Condicionales Anidadas**
**Objetivo**: Practicar condicionales dentro de condicionales

**Instrucciones**: Crea un programa que determine si una persona puede conducir y qué tipo de licencia necesita

**Tu código**:
```python
# Programa para determinar licencia de conducir
edad = int(input("Ingresa tu edad: "))
tiene_experiencia = input("¿Tienes experiencia conduciendo? (sí/no): ").lower()

# Escribe aquí tu código con condicionales anidadas
if edad >= 18:
    if tiene_experiencia == "sí":
        print("Puedes obtener licencia de conducir completa")
    else:
        print("Puedes obtener licencia de aprendiz")
elif edad >= 16:
    if tiene_experiencia == "sí":
        print("Puedes obtener licencia de aprendiz")
    else:
        print("Debes tomar clases de manejo primero")
else:
    print("No puedes conducir aún")
```

**Ejemplo de respuesta**:
```python
edad = int(input("Ingresa tu edad: "))
tiene_experiencia = input("¿Tienes experiencia conduciendo? (sí/no): ").lower()

if edad >= 18:
    if tiene_experiencia == "sí":
        print("Puedes obtener licencia de conducir completa")
    else:
        print("Puedes obtener licencia de aprendiz")
elif edad >= 16:
    if tiene_experiencia == "sí":
        print("Puedes obtener licencia de aprendiz")
    else:
        print("Debes tomar clases de manejo primero")
else:
    print("No puedes conducir aún")
```

---

### **Ejercicio 3: Bucle While**
**Objetivo**: Practicar el uso del bucle while

**Instrucciones**: Crea un programa que adivine un número usando el bucle while

**Tu código**:
```python
# Juego de adivinar número
import random

numero_secreto = random.randint(1, 100)
intentos = 0
max_intentos = 10

print("¡Bienvenido al juego de adivinar el número!")
print(f"Tienes {max_intentos} intentos para adivinar un número entre 1 y 100")

# Escribe aquí tu código con bucle while
while intentos < max_intentos:
    intentos += 1
    print(f"\nIntento {intentos} de {max_intentos}")
    
    try:
        adivinanza = int(input("Ingresa tu número: "))
        
        if adivinanza < numero_secreto:
            print("El número es mayor")
        elif adivinanza > numero_secreto:
            print("El número es menor")
        else:
            print(f"¡Felicidades! Adivinaste en {intentos} intentos")
            break
    except ValueError:
        print("Por favor ingresa un número válido")
        intentos -= 1

if intentos >= max_intentos:
    print(f"\n¡Se acabaron los intentos! El número era {numero_secreto}")
```

**Ejemplo de respuesta**:
```python
import random

numero_secreto = random.randint(1, 100)
intentos = 0
max_intentos = 10

print("¡Bienvenido al juego de adivinar el número!")
print(f"Tienes {max_intentos} intentos para adivinar un número entre 1 y 100")

while intentos < max_intentos:
    intentos += 1
    print(f"\nIntento {intentos} de {max_intentos}")
    
    try:
        adivinanza = int(input("Ingresa tu número: "))
        
        if adivinanza < numero_secreto:
            print("El número es mayor")
        elif adivinanza > numero_secreto:
            print("El número es menor")
        else:
            print(f"¡Felicidades! Adivinaste en {intentos} intentos")
            break
    except ValueError:
        print("Por favor ingresa un número válido")
        intentos -= 1

if intentos >= max_intentos:
    print(f"\n¡Se acabaron los intentos! El número era {numero_secreto}")
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Bucle For con Range**
**Objetivo**: Practicar el uso del bucle for con diferentes rangos

**Instrucciones**: Crea un programa que genere diferentes patrones numéricos

**Tu código**:
```python
# Generador de patrones numéricos
print("=== PATRÓN 1: Números del 1 al 10 ===")
# Escribe aquí tu código para mostrar números del 1 al 10

print("\n=== PATRÓN 2: Números pares del 2 al 20 ===")
# Escribe aquí tu código para mostrar números pares del 2 al 20

print("\n=== PATRÓN 3: Números del 10 al 1 (descendente) ===")
# Escribe aquí tu código para mostrar números del 10 al 1

print("\n=== PATRÓN 4: Múltiplos de 3 del 3 al 30 ===")
# Escribe aquí tu código para mostrar múltiplos de 3

print("\n=== PATRÓN 5: Suma de números del 1 al 100 ===")
# Escribe aquí tu código para calcular la suma
```

**Ejemplo de respuesta**:
```python
print("=== PATRÓN 1: Números del 1 al 10 ===")
for i in range(1, 11):
    print(i, end=" ")

print("\n=== PATRÓN 2: Números pares del 2 al 20 ===")
for i in range(2, 21, 2):
    print(i, end=" ")

print("\n=== PATRÓN 3: Números del 10 al 1 (descendente) ===")
for i in range(10, 0, -1):
    print(i, end=" ")

print("\n=== PATRÓN 4: Múltiplos de 3 del 3 al 30 ===")
for i in range(3, 31, 3):
    print(i, end=" ")

print("\n=== PATRÓN 5: Suma de números del 1 al 100 ===")
suma = 0
for i in range(1, 101):
    suma += i
print(f"La suma es: {suma}")
```

---

### **Ejercicio 5: Bucle For con Listas**
**Objetivo**: Practicar el uso del bucle for con listas y strings

**Instrucciones**: Crea un programa que procese listas y strings

**Tu código**:
```python
# Procesamiento de listas y strings
frutas = ["manzana", "banana", "naranja", "uva", "kiwi"]
numeros = [15, 23, 8, 42, 17, 9, 31, 5]

print("=== PROCESAMIENTO DE FRUTAS ===")
# Escribe aquí tu código para mostrar cada fruta con su longitud

print("\n=== FRUTAS QUE EMPIEZAN CON VOCAL ===")
# Escribe aquí tu código para mostrar frutas que empiecen con vocal

print("\n=== ANÁLISIS DE NÚMEROS ===")
# Escribe aquí tu código para contar pares e impares

print("\n=== NÚMEROS MAYORES A 20 ===")
# Escribe aquí tu código para mostrar números mayores a 20

print("\n=== SUMA DE NÚMEROS PARES ===")
# Escribe aquí tu código para sumar solo números pares
```

**Ejemplo de respuesta**:
```python
frutas = ["manzana", "banana", "naranja", "uva", "kiwi"]
numeros = [15, 23, 8, 42, 17, 9, 31, 5]

print("=== PROCESAMIENTO DE FRUTAS ===")
for fruta in frutas:
    print(f"{fruta}: {len(fruta)} letras")

print("\n=== FRUTAS QUE EMPIEZAN CON VOCAL ===")
vocales = "aeiou"
for fruta in frutas:
    if fruta[0].lower() in vocales:
        print(f"{fruta} empieza con vocal")

print("\n=== ANÁLISIS DE NÚMEROS ===")
pares = 0
impares = 0
for num in numeros:
    if num % 2 == 0:
        pares += 1
    else:
        impares += 1
print(f"Pares: {pares}, Impares: {impares}")

print("\n=== NÚMEROS MAYORES A 20 ===")
for num in numeros:
    if num > 20:
        print(num, end=" ")

print("\n=== SUMA DE NÚMEROS PARES ===")
suma_pares = 0
for num in numeros:
    if num % 2 == 0:
        suma_pares += num
print(f"Suma de pares: {suma_pares}")
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 6: Control de Flujo**
**Objetivo**: Practicar el uso de break, continue y pass

**Instrucciones**: Crea un programa que demuestre el uso de estas instrucciones

**Tu código**:
```python
# Demostración de control de flujo
print("=== DEMOSTRACIÓN DE BREAK ===")
# Escribe aquí tu código que use break para salir de un bucle

print("\n=== DEMOSTRACIÓN DE CONTINUE ===")
# Escribe aquí tu código que use continue para saltar iteraciones

print("\n=== DEMOSTRACIÓN DE PASS ===")
# Escribe aquí tu código que use pass como placeholder
```

**Ejemplo de respuesta**:
```python
print("=== DEMOSTRACIÓN DE BREAK ===")
for i in range(1, 11):
    if i == 7:
        print("¡Encontré el 7! Saliendo del bucle...")
        break
    print(f"Número: {i}")

print("\n=== DEMOSTRACIÓN DE CONTINUE ===")
for i in range(1, 11):
    if i % 2 == 0:
        continue  # Salta números pares
    print(f"Número impar: {i}")

print("\n=== DEMOSTRACIÓN DE PASS ===")
for i in range(1, 6):
    if i < 3:
        pass  # No hacer nada por ahora
    else:
        print(f"Procesando: {i}")
```

---

### **Ejercicio 7: Estructuras de Control Combinadas**
**Objetivo**: Crear un programa complejo que use múltiples estructuras de control

**Instrucciones**: Crea un sistema de calificaciones que procese múltiples estudiantes

**Tu código**:
```python
# Sistema de calificaciones
estudiantes = [
    {"nombre": "Ana", "calificaciones": [8.5, 9.0, 7.5, 8.0]},
    {"nombre": "Carlos", "calificaciones": [6.5, 7.0, 8.5, 9.0]},
    {"nombre": "María", "calificaciones": [9.5, 9.0, 9.5, 10.0]},
    {"nombre": "Luis", "calificaciones": [5.5, 6.0, 7.5, 8.0]}
]

print("=== SISTEMA DE CALIFICACIONES ===")

# Escribe aquí tu código para procesar cada estudiante
for estudiante in estudiantes:
    nombre = estudiante["nombre"]
    calificaciones = estudiante["calificaciones"]
    
    # Calcular promedio
    promedio = sum(calificaciones) / len(calificaciones)
    
    # Determinar estado
    if promedio >= 9.0:
        estado = "Excelente"
    elif promedio >= 8.0:
        estado = "Muy Bueno"
    elif promedio >= 7.0:
        estado = "Bueno"
    elif promedio >= 6.0:
        estado = "Suficiente"
    else:
        estado = "Insuficiente"
    
    # Mostrar resultados
    print(f"\n{nombre}:")
    print(f"  Calificaciones: {calificaciones}")
    print(f"  Promedio: {promedio:.2f}")
    print(f"  Estado: {estado}")
    
    # Análisis adicional
    aprobadas = 0
    for cal in calificaciones:
        if cal >= 6.0:
            aprobadas += 1
    
    print(f"  Materias aprobadas: {aprobadas}/{len(calificaciones)}")
```

**Ejemplo de respuesta**:
```python
estudiantes = [
    {"nombre": "Ana", "calificaciones": [8.5, 9.0, 7.5, 8.0]},
    {"nombre": "Carlos", "calificaciones": [6.5, 7.0, 8.5, 9.0]},
    {"nombre": "María", "calificaciones": [9.5, 9.0, 9.5, 10.0]},
    {"nombre": "Luis", "calificaciones": [5.5, 6.0, 7.5, 8.0]}
]

print("=== SISTEMA DE CALIFICACIONES ===")

for estudiante in estudiantes:
    nombre = estudiante["nombre"]
    calificaciones = estudiante["calificaciones"]
    
    # Calcular promedio
    promedio = sum(calificaciones) / len(calificaciones)
    
    # Determinar estado
    if promedio >= 9.0:
        estado = "Excelente"
    elif promedio >= 8.0:
        estado = "Muy Bueno"
    elif promedio >= 7.0:
        estado = "Bueno"
    elif promedio >= 6.0:
        estado = "Suficiente"
    else:
        estado = "Insuficiente"
    
    # Mostrar resultados
    print(f"\n{nombre}:")
    print(f"  Calificaciones: {calificaciones}")
    print(f"  Promedio: {promedio:.2f}")
    print(f"  Estado: {estado}")
    
    # Análisis adicional
    aprobadas = 0
    for cal in calificaciones:
        if cal >= 6.0:
            aprobadas += 1
    
    print(f"  Materias aprobadas: {aprobadas}/{len(calificaciones)}")
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 8: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores en estructuras de control

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
numero = 15

# Error 1: Falta dos puntos
if numero > 10
    print("El número es mayor a 10")

# Error 2: Indentación incorrecta
elif numero == 10:
print("El número es igual a 10")

# Error 3: Bucle infinito
i = 1
while i < 5:
    print(i)
    # Falta incrementar i

# Error 4: Bucle for mal formado
for j in range(5)
    print(j * 2)

# Error 5: Condición siempre verdadera
if True:
    pass
else:
    print("Esto nunca se ejecutará")
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
numero = 15

# Corregido: Agregar dos puntos
if numero > 10:
    print("El número es mayor a 10")

# Corregido: Indentación correcta
elif numero == 10:
    print("El número es igual a 10")

# Corregido: Agregar incremento
i = 1
while i < 5:
    print(i)
    i += 1

# Corregido: Agregar dos puntos
for j in range(5):
    print(j * 2)

# Corregido: Condición lógica
if numero > 10:
    pass
else:
    print("El número no es mayor a 10")
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Condicionales básicas completado
- [ ] Ejercicio 2: Condicionales anidadas completado
- [ ] Ejercicio 3: Bucle while completado
- [ ] Ejercicio 4: Bucle for con range completado
- [ ] Ejercicio 5: Bucle for con listas completado
- [ ] Ejercicio 6: Control de flujo completado
- [ ] Ejercicio 7: Estructuras combinadas completado
- [ ] Ejercicio 8: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/2
- **Ejercicios avanzados**: ___/2
- **Depuración**: ___/1

**Puntuación Total**: ___/8

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Generador de Patrones**
Crea un programa que genere patrones visuales usando bucles (triángulos, pirámides, etc.).

### **Desafío 2: Calculadora de Estadísticas**
Implementa un programa que calcule estadísticas de un conjunto de datos usando bucles.

### **Desafío 3: Validador de Formularios**
Crea un validador que verifique múltiples campos usando condicionales anidadas.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy sobre estructuras de control?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué estructura te resultó más difícil de entender?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que usarás estas estructuras en proyectos futuros?**
```
[Escribe aquí tus planes]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [04-Ejercicios-Bucles.md](./04-ejercicios-bucles.md)

**En la siguiente lección practicarás específicamente con bucles y sus aplicaciones avanzadas.**

---

**¡Excelente trabajo! Has completado los ejercicios de estructuras de control. Ahora estás listo para profundizar en el uso de bucles.** 