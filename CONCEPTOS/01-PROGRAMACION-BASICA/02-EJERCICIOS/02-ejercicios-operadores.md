# 🛠️ Ejercicios Prácticos: Operadores en Python

## 🎯 Objetivo de la Lección
Practicar el uso de diferentes tipos de operadores en Python: aritméticos, de comparación, lógicos y de asignación. Comprender la precedencia de operadores y cómo combinarlos.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Tipos de Operadores en Python**
- **Aritméticos**: `+`, `-`, `*`, `/`, `//`, `%`, `**`
- **Comparación**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Lógicos**: `and`, `or`, `not`
- **Asignación**: `=`, `+=`, `-=`, `*=`, `/=` etc.

### **Precedencia de Operadores**
1. **Paréntesis** `()`
2. **Exponenciación** `**`
3. **Multiplicación/División** `*`, `/`, `//`, `%`
4. **Suma/Resta** `+`, `-`
5. **Comparación** `==`, `!=`, `<`, `>`, `<=`, `>=`
6. **Lógicos** `not`, `and`, `or`

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Operadores Aritméticos**
**Objetivo**: Practicar operaciones matemáticas básicas

**Instrucciones**: Completa las operaciones aritméticas

**Tu código**:
```python
# 1. Suma de dos números
a = 15
b = 7
suma = 
print(f"Suma: {a} + {b} = {suma}")

# 2. Resta de dos números
resta = 
print(f"Resta: {a} - {b} = {resta}")

# 3. Multiplicación de dos números
multiplicacion = 
print(f"Multiplicación: {a} × {b} = {multiplicacion}")

# 4. División de dos números
division = 
print(f"División: {a} ÷ {b} = {division}")

# 5. División entera (floor division)
division_entera = 
print(f"División entera: {a} // {b} = {division_entera}")

# 6. Módulo (resto de división)
modulo = 
print(f"Módulo: {a} % {b} = {modulo}")

# 7. Exponenciación
exponenciacion = 
print(f"Exponenciación: {a} ^ {b} = {exponenciacion}")
```

**Ejemplo de respuesta**:
```python
a = 15
b = 7
suma = a + b
resta = a - b
multiplicacion = a * b
division = a / b
division_entera = a // b
modulo = a % b
exponenciacion = a ** b

print(f"Suma: {a} + {b} = {suma}")
print(f"Resta: {a} - {b} = {resta}")
print(f"Multiplicación: {a} × {b} = {multiplicacion}")
print(f"División: {a} ÷ {b} = {division}")
print(f"División entera: {a} // {b} = {division_entera}")
print(f"Módulo: {a} % {b} = {modulo}")
print(f"Exponenciación: {a} ^ {b} = {exponenciacion}")
```

---

### **Ejercicio 2: Operadores de Comparación**
**Objetivo**: Practicar comparaciones entre valores

**Instrucciones**: Completa las comparaciones y predice el resultado

**Tu código**:
```python
# Variables para comparar
x = 10
y = 5
z = 10

# 1. Igualdad
igual = 
print(f"¿{x} == {y}? {igual}")

# 2. Desigualdad
diferente = 
print(f"¿{x} != {y}? {diferente}")

# 3. Mayor que
mayor_que = 
print(f"¿{x} > {y}? {mayor_que}")

# 4. Menor que
menor_que = 
print(f"¿{x} < {y}? {menor_que}")

# 5. Mayor o igual que
mayor_igual = 
print(f"¿{x} >= {z}? {mayor_igual}")

# 6. Menor o igual que
menor_igual = 
print(f"¿{x} <= {z}? {menor_igual}")

# 7. Comparación de strings
nombre1 = "Ana"
nombre2 = "ana"
comparacion_strings = 
print(f"¿'{nombre1}' == '{nombre2}'? {comparacion_strings}")
```

**Ejemplo de respuesta**:
```python
x = 10
y = 5
z = 10

igual = x == y
diferente = x != y
mayor_que = x > y
menor_que = x < y
mayor_igual = x >= z
menor_igual = x <= z
comparacion_strings = nombre1 == nombre2

print(f"¿{x} == {y}? {igual}")
print(f"¿{x} != {y}? {diferente}")
print(f"¿{x} > {y}? {mayor_que}")
print(f"¿{x} < {y}? {menor_que}")
print(f"¿{x} >= {z}? {mayor_igual}")
print(f"¿{x} <= {z}? {menor_igual}")
print(f"¿'{nombre1}' == '{nombre2}'? {comparacion_strings}")
```

---

### **Ejercicio 3: Operadores Lógicos**
**Objetivo**: Practicar operaciones lógicas y booleanas

**Instrucciones**: Completa las operaciones lógicas

**Tu código**:
```python
# Variables booleanas
es_estudiante = True
tiene_experiencia = False
edad = 20
es_mayor_edad = edad >= 18

# 1. Operador AND
puede_trabajar = 
print(f"¿Puede trabajar? {puede_trabajar}")

# 2. Operador OR
puede_aprender = 
print(f"¿Puede aprender? {puede_aprender}")

# 3. Operador NOT
no_tiene_experiencia = 
print(f"¿No tiene experiencia? {no_tiene_experiencia}")

# 4. Combinación de operadores
condicion_compleja = 
print(f"Condición compleja: {condicion_compleja}")

# 5. Evaluación de cortocircuito
resultado_cortocircuito = 
print(f"Resultado cortocircuito: {resultado_cortocircuito}")
```

**Ejemplo de respuesta**:
```python
es_estudiante = True
tiene_experiencia = False
edad = 20
es_mayor_edad = edad >= 18

puede_trabajar = es_mayor_edad and tiene_experiencia
puede_aprender = es_estudiante or tiene_experiencia
no_tiene_experiencia = not tiene_experiencia
condicion_compleja = es_estudiante and es_mayor_edad and not tiene_experiencia
resultado_cortocircuito = False and (print("Esto no se ejecuta") or True)

print(f"¿Puede trabajar? {puede_trabajar}")
print(f"¿Puede aprender? {puede_aprender}")
print(f"¿No tiene experiencia? {no_tiene_experiencia}")
print(f"Condición compleja: {condicion_compleja}")
print(f"Resultado cortocircuito: {resultado_cortocircuito}")
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Operadores de Asignación**
**Objetivo**: Practicar operadores de asignación compuesta

**Instrucciones**: Completa las operaciones de asignación

**Tu código**:
```python
# Variable inicial
contador = 10
print(f"Contador inicial: {contador}")

# 1. Incrementar en 5
contador += 5
print(f"Después de += 5: {contador}")

# 2. Decrementar en 3
contador -= 3
print(f"Después de -= 3: {contador}")

# 3. Multiplicar por 2
contador *= 2
print(f"Después de *= 2: {contador}")

# 4. Dividir por 4
contador /= 4
print(f"Después de /= 4: {contador}")

# 5. División entera por 2
contador //= 2
print(f"Después de //= 2: {contador}")

# 6. Módulo por 3
contador %= 3
print(f"Después de %= 3: {contador}")

# 7. Elevar al cuadrado
contador **= 2
print(f"Después de **= 2: {contador}")
```

**Ejemplo de respuesta**:
```python
contador = 10
print(f"Contador inicial: {contador}")

contador += 5
print(f"Después de += 5: {contador}")

contador -= 3
print(f"Después de -= 3: {contador}")

contador *= 2
print(f"Después de *= 2: {contador}")

contador /= 4
print(f"Después de /= 4: {contador}")

contador //= 2
print(f"Después de //= 2: {contador}")

contador %= 3
print(f"Después de %= 3: {contador}")

contador **= 2
print(f"Después de **= 2: {contador}")
```

---

### **Ejercicio 5: Precedencia de Operadores**
**Objetivo**: Entender el orden de evaluación de operadores

**Instrucciones**: Calcula el resultado de las expresiones

**Tu código**:
```python
# Expresiones para evaluar
a = 5
b = 3
c = 2

# 1. Expresión simple
resultado1 = a + b * c
print(f"a + b * c = {a} + {b} * {c} = {resultado1}")

# 2. Con paréntesis
resultado2 = (a + b) * c
print(f"(a + b) * c = ({a} + {b}) * {c} = {resultado2}")

# 3. Expresión compleja
resultado3 = a ** b + c * a
print(f"a ** b + c * a = {a} ** {b} + {c} * {a} = {resultado3}")

# 4. Con operadores lógicos
resultado4 = a > b and b < c
print(f"a > b and b < c = {a} > {b} and {b} < {c} = {resultado4}")

# 5. Expresión con múltiples operadores
resultado5 = a + b * c ** 2
print(f"a + b * c ** 2 = {a} + {b} * {c} ** 2 = {resultado5}")

# 6. Comparación compleja
resultado6 = a == b or a > b and c < a
print(f"a == b or a > b and c < a = {a} == {b} or {a} > {b} and {c} < {a} = {resultado6}")
```

**Ejemplo de respuesta**:
```python
a = 5
b = 3
c = 2

resultado1 = a + b * c
resultado2 = (a + b) * c
resultado3 = a ** b + c * a
resultado4 = a > b and b < c
resultado5 = a + b * c ** 2
resultado6 = a == b or a > b and c < a

print(f"a + b * c = {a} + {b} * {c} = {resultado1}")
print(f"(a + b) * c = ({a} + {b}) * {c} = {resultado2}")
print(f"a ** b + c * a = {a} ** {b} + {c} * {a} = {resultado3}")
print(f"a > b and b < c = {a} > {b} and {b} < {c} = {resultado4}")
print(f"a + b * c ** 2 = {a} + {b} * {c} ** 2 = {resultado5}")
print(f"a == b or a > b and c < a = {a} == {b} or {a} > {b} and {c} < {a} = {resultado6}")
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 6: Calculadora de Expresiones**
**Objetivo**: Crear un programa que evalúe expresiones matemáticas

**Instrucciones**: Completa el código de la calculadora

**Tu código**:
```python
def calcular_expresion(expresion):
    """Evalúa una expresión matemática simple"""
    try:
        # Reemplazar operadores por operadores de Python
        expresion = expresion.replace('×', '*')
        expresion = expresion.replace('÷', '/')
        expresion = expresion.replace('^', '**')
        
        # Evaluar la expresión
        resultado = eval(expresion)
        return resultado
    except:
        return "Error en la expresión"

# Pruebas de la calculadora
expresiones = [
    "5 + 3 × 2",
    "10 ÷ 2 + 3",
    "2 ^ 3 × 4",
    "(5 + 3) × 2",
    "10 - 3 × 2"
]

print("=== CALCULADORA DE EXPRESIONES ===")
for expresion in expresiones:
    resultado = calcular_expresion(expresion)
    print(f"{expresion} = {resultado}")
```

**Ejemplo de respuesta**:
```python
def calcular_expresion(expresion):
    """Evalúa una expresión matemática simple"""
    try:
        # Reemplazar operadores por operadores de Python
        expresion = expresion.replace('×', '*')
        expresion = expresion.replace('÷', '/')
        expresion = expresion.replace('^', '**')
        
        # Evaluar la expresión
        resultado = eval(expresion)
        return resultado
    except:
        return "Error en la expresión"

# Pruebas de la calculadora
expresiones = [
    "5 + 3 × 2",
    "10 ÷ 2 + 3",
    "2 ^ 3 × 4",
    "(5 + 3) × 2",
    "10 - 3 × 2"
]

print("=== CALCULADORA DE EXPRESIONES ===")
for expresion in expresiones:
    resultado = calcular_expresion(expresion)
    print(f"{expresion} = {resultado}")
```

---

### **Ejercicio 7: Validador de Contraseñas**
**Objetivo**: Crear un validador que use operadores lógicos

**Instrucciones**: Completa el validador de contraseñas

**Tu código**:
```python
def validar_password(password):
    """Valida una contraseña según criterios específicos"""
    
    # Criterios de validación
    tiene_longitud_minima = len(password) >= 8
    tiene_mayuscula = any(c.isupper() for c in password)
    tiene_minuscula = any(c.islower() for c in password)
    tiene_numero = any(c.isdigit() for c in password)
    tiene_caracter_especial = any(c in "!@#$%^&*" for c in password)
    
    # Validación completa
    es_valida = (
        tiene_longitud_minima and
        tiene_mayuscula and
        tiene_minuscula and
        tiene_numero and
        tiene_caracter_especial
    )
    
    # Mensajes de error
    errores = []
    if not tiene_longitud_minima:
        errores.append("Debe tener al menos 8 caracteres")
    if not tiene_mayuscula:
        errores.append("Debe tener al menos una mayúscula")
    if not tiene_minuscula:
        errores.append("Debe tener al menos una minúscula")
    if not tiene_numero:
        errores.append("Debe tener al menos un número")
    if not tiene_caracter_especial:
        errores.append("Debe tener al menos un carácter especial")
    
    return es_valida, errores

# Pruebas del validador
passwords = [
    "abc123",
    "Password123",
    "MySecurePass!",
    "weak",
    "STRONG123!"
]

print("=== VALIDADOR DE CONTRASEÑAS ===")
for password in passwords:
    es_valida, errores = validar_password(password)
    print(f"\nContraseña: {password}")
    print(f"¿Es válida? {es_valida}")
    if not es_valida:
        print("Errores:")
        for error in errores:
            print(f"  - {error}")
```

**Ejemplo de respuesta**:
```python
def validar_password(password):
    """Valida una contraseña según criterios específicos"""
    
    # Criterios de validación
    tiene_longitud_minima = len(password) >= 8
    tiene_mayuscula = any(c.isupper() for c in password)
    tiene_minuscula = any(c.islower() for c in password)
    tiene_numero = any(c.isdigit() for c in password)
    tiene_caracter_especial = any(c in "!@#$%^&*" for c in password)
    
    # Validación completa
    es_valida = (
        tiene_longitud_minima and
        tiene_mayuscula and
        tiene_minuscula and
        tiene_numero and
        tiene_caracter_especial
    )
    
    # Mensajes de error
    errores = []
    if not tiene_longitud_minima:
        errores.append("Debe tener al menos 8 caracteres")
    if not tiene_mayuscula:
        errores.append("Debe tener al menos una mayúscula")
    if not tiene_minuscula:
        errores.append("Debe tener al menos una minúscula")
    if not tiene_numero:
        errores.append("Debe tener al menos un número")
    if not tiene_caracter_especial:
        errores.append("Debe tener al menos un carácter especial")
    
    return es_valida, errores

# Pruebas del validador
passwords = [
    "abc123",
    "Password123",
    "MySecurePass!",
    "weak",
    "STRONG123!"
]

print("=== VALIDADOR DE CONTRASEÑAS ===")
for password in passwords:
    es_valida, errores = validar_password(password)
    print(f"\nContraseña: {password}")
    print(f"¿Es válida? {es_valida}")
    if not es_valida:
        print("Errores:")
        for error in errores:
            print(f"  - {error}")
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 8: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores en operadores

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
a = 10
b = 5
c = 2

# Error 1: Operador incorrecto
resultado1 = a + b * c
print("Resultado 1:", resultado1)

# Error 2: Comparación incorrecta
if a = b:
    print("a es igual a b")

# Error 3: Operador lógico incorrecto
if a > b & c < a:
    print("Condición compleja verdadera")

# Error 4: División por cero
resultado2 = a / (b - b)
print("Resultado 2:", resultado2)

# Error 5: Operador de asignación incorrecto
a =+ 5
print("a después de incremento:", a)
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
a = 10
b = 5
c = 2

# Corregido: Operador correcto
resultado1 = a + b * c
print("Resultado 1:", resultado1)

# Corregido: Operador de comparación
if a == b:
    print("a es igual a b")

# Corregido: Operador lógico correcto
if a > b and c < a:
    print("Condición compleja verdadera")

# Corregido: Evitar división por cero
if b - b != 0:
    resultado2 = a / (b - b)
    print("Resultado 2:", resultado2)
else:
    print("Error: División por cero")

# Corregido: Operador de asignación correcto
a += 5
print("a después de incremento:", a)
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Operadores aritméticos completado
- [ ] Ejercicio 2: Operadores de comparación completado
- [ ] Ejercicio 3: Operadores lógicos completado
- [ ] Ejercicio 4: Operadores de asignación completado
- [ ] Ejercicio 5: Precedencia de operadores completado
- [ ] Ejercicio 6: Calculadora de expresiones completado
- [ ] Ejercicio 7: Validador de contraseñas completado
- [ ] Ejercicio 8: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/2
- **Ejercicios avanzados**: ___/2
- **Depuración**: ___/1

**Puntuación Total**: ___/8

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Calculadora de Notas**
Crea un programa que calcule el promedio de notas usando operadores aritméticos y determine si el estudiante aprueba o no.

### **Desafío 2: Sistema de Descuentos**
Implementa un sistema que calcule descuentos basados en múltiples criterios usando operadores lógicos.

### **Desafío 3: Validador de Fechas**
Crea un validador que verifique si una fecha es válida usando operadores de comparación y lógicos.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy sobre operadores?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué operadores te resultaron más difíciles de entender?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que usarás estos operadores en proyectos futuros?**
```
[Escribe aquí tus planes]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [03-Ejercicios-Estructuras-Control.md](./03-ejercicios-estructuras-control.md)

**En la siguiente lección practicarás con estructuras de control como condicionales y bucles.**

---

**¡Excelente trabajo! Has completado los ejercicios de operadores. Ahora estás listo para aprender sobre estructuras de control en Python.** 