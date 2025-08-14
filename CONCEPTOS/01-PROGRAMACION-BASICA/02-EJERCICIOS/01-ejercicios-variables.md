# 🛠️ Ejercicios Prácticos: Variables y Tipos de Datos

## 🎯 Objetivo de la Lección
Practicar la creación y manipulación de variables en Python, comprendiendo los diferentes tipos de datos y cómo usarlos correctamente.

## ⏱️ Tiempo Estimado: 45-60 minutos

---

## 📚 Repaso Rápido

### **Tipos de Datos en Python**
- **int**: Números enteros (1, 2, 3, -5, 0)
- **float**: Números decimales (3.14, 2.5, -0.5)
- **str**: Texto ("Hola", 'Mundo', "123")
- **bool**: Valores lógicos (True, False)

### **Creación de Variables**
```python
# Sintaxis: nombre_variable = valor
nombre = "Juan"
edad = 25
altura = 1.75
es_estudiante = True
```

---

## 🏋️ Ejercicios Básicos

### **Ejercicio 1: Crear Variables Básicas**
**Objetivo**: Practicar la creación de variables con diferentes tipos de datos

**Instrucciones**: Crea variables para almacenar información personal

**Tu código**:
```python
# 1. Crea una variable para tu nombre
nombre = 

# 2. Crea una variable para tu edad
edad = 

# 3. Crea una variable para tu altura en metros
altura = 

# 4. Crea una variable para indicar si eres estudiante
es_estudiante = 

# 5. Imprime todas las variables
print("Nombre:", nombre)
print("Edad:", edad)
print("Altura:", altura)
print("¿Es estudiante?:", es_estudiante)
```

**Ejemplo de respuesta**:
```python
nombre = "María"
edad = 22
altura = 1.68
es_estudiante = True

print("Nombre:", nombre)
print("Edad:", edad)
print("Altura:", altura)
print("¿Es estudiante?:", es_estudiante)
```

---

### **Ejercicio 2: Operaciones con Variables Numéricas**
**Objetivo**: Practicar operaciones matemáticas con variables

**Instrucciones**: Crea un programa que calcule el área de un rectángulo

**Tu código**:
```python
# 1. Define la base del rectángulo
base = 

# 2. Define la altura del rectángulo
altura = 

# 3. Calcula el área (base × altura)
area = 

# 4. Calcula el perímetro (2 × base + 2 × altura)
perimetro = 

# 5. Muestra los resultados
print("=== RECTÁNGULO ===")
print(f"Base: {base}")
print(f"Altura: {altura}")
print(f"Área: {area}")
print(f"Perímetro: {perimetro}")
```

**Ejemplo de respuesta**:
```python
base = 5
altura = 3
area = base * altura
perimetro = 2 * base + 2 * altura

print("=== RECTÁNGULO ===")
print(f"Base: {base}")
print(f"Altura: {altura}")
print(f"Área: {area}")
print(f"Perímetro: {perimetro}")
```

---

### **Ejercicio 3: Conversión de Tipos**
**Objetivo**: Aprender a convertir entre diferentes tipos de datos

**Instrucciones**: Practica las conversiones de tipos

**Tu código**:
```python
# 1. Convierte el número 42 a texto
numero = 42
texto = 
print(f"El número {numero} como texto: '{texto}'")

# 2. Convierte el texto "15" a número
texto_numero = "15"
numero_convertido = 
print(f"El texto '{texto_numero}' como número: {numero_convertido}")

# 3. Convierte 3.8 a entero
decimal = 3.8
entero = 
print(f"El decimal {decimal} como entero: {entero}")

# 4. Convierte 0 a booleano
cero = 0
booleano = 
print(f"El número {cero} como booleano: {booleano}")
```

**Ejemplo de respuesta**:
```python
numero = 42
texto = str(numero)
print(f"El número {numero} como texto: '{texto}'")

texto_numero = "15"
numero_convertido = int(texto_numero)
print(f"El texto '{texto_numero}' como número: {numero_convertido}")

decimal = 3.8
entero = int(decimal)
print(f"El decimal {decimal} como entero: {entero}")

cero = 0
booleano = bool(cero)
print(f"El número {cero} como booleano: {booleano}")
```

---

## 🚀 Ejercicios Intermedios

### **Ejercicio 4: Calculadora de Precios**
**Objetivo**: Crear un programa que calcule precios con descuentos

**Instrucciones**: Calcula el precio final de un producto con descuento

**Tu código**:
```python
# 1. Define el precio original del producto
precio_original = 

# 2. Define el porcentaje de descuento
porcentaje_descuento = 

# 3. Calcula el monto del descuento
monto_descuento = 

# 4. Calcula el precio final
precio_final = 

# 5. Muestra el resumen
print("=== CALCULADORA DE DESCUENTOS ===")
print(f"Precio original: ${precio_original}")
print(f"Descuento: {porcentaje_descuento}%")
print(f"Monto del descuento: ${monto_descuento}")
print(f"Precio final: ${precio_final}")
```

**Ejemplo de respuesta**:
```python
precio_original = 100.00
porcentaje_descuento = 15
monto_descuento = precio_original * (porcentaje_descuento / 100)
precio_final = precio_original - monto_descuento

print("=== CALCULADORA DE DESCUENTOS ===")
print(f"Precio original: ${precio_original}")
print(f"Descuento: {porcentaje_descuento}%")
print(f"Monto del descuento: ${monto_descuento}")
print(f"Precio final: ${precio_final}")
```

---

### **Ejercicio 5: Gestor de Información Personal**
**Objetivo**: Crear un sistema que gestione información personal

**Instrucciones**: Crea un programa que almacene y muestre información personal

**Tu código**:
```python
# Información personal
nombre = 
apellido = 
edad = 
ciudad = 
profesion = 
experiencia_anios = 

# Cálculos
nombre_completo = 
año_nacimiento = 2024 - edad
experiencia_meses = experiencia_anios * 12

# Mostrar información
print("=== INFORMACIÓN PERSONAL ===")
print(f"Nombre completo: {nombre_completo}")
print(f"Edad: {edad} años")
print(f"Año de nacimiento: {año_nacimiento}")
print(f"Ciudad: {ciudad}")
print(f"Profesión: {profesion}")
print(f"Experiencia: {experiencia_anios} años ({experiencia_meses} meses)")
```

**Ejemplo de respuesta**:
```python
nombre = "Ana"
apellido = "García"
edad = 28
ciudad = "Madrid"
profesion = "Desarrolladora"
experiencia_anios = 5

nombre_completo = nombre + " " + apellido
año_nacimiento = 2024 - edad
experiencia_meses = experiencia_anios * 12

print("=== INFORMACIÓN PERSONAL ===")
print(f"Nombre completo: {nombre_completo}")
print(f"Edad: {edad} años")
print(f"Año de nacimiento: {año_nacimiento}")
print(f"Ciudad: {ciudad}")
print(f"Profesión: {profesion}")
print(f"Experiencia: {experiencia_anios} años ({experiencia_meses} meses)")
```

---

## 🎯 Ejercicios Avanzados

### **Ejercicio 6: Calculadora de Estadísticas**
**Objetivo**: Crear un programa que calcule estadísticas básicas

**Instrucciones**: Calcula estadísticas de un conjunto de números

**Tu código**:
```python
# Conjunto de números
numero1 = 15
numero2 = 23
numero3 = 8
numero4 = 42
numero5 = 17

# Cálculos estadísticos
suma_total = 
cantidad_numeros = 5
promedio = 
numero_maximo = 
numero_minimo = 
rango = 

# Mostrar resultados
print("=== ESTADÍSTICAS BÁSICAS ===")
print(f"Números: {numero1}, {numero2}, {numero3}, {numero4}, {numero5}")
print(f"Suma total: {suma_total}")
print(f"Cantidad de números: {cantidad_numeros}")
print(f"Promedio: {promedio}")
print(f"Número máximo: {numero_maximo}")
print(f"Número mínimo: {numero_minimo}")
print(f"Rango: {rango}")
```

**Ejemplo de respuesta**:
```python
numero1 = 15
numero2 = 23
numero3 = 8
numero4 = 42
numero5 = 17

suma_total = numero1 + numero2 + numero3 + numero4 + numero5
cantidad_numeros = 5
promedio = suma_total / cantidad_numeros
numero_maximo = max(numero1, numero2, numero3, numero4, numero5)
numero_minimo = min(numero1, numero2, numero3, numero4, numero5)
rango = numero_maximo - numero_minimo

print("=== ESTADÍSTICAS BÁSICAS ===")
print(f"Números: {numero1}, {numero2}, {numero3}, {numero4}, {numero5}")
print(f"Suma total: {suma_total}")
print(f"Cantidad de números: {cantidad_numeros}")
print(f"Promedio: {promedio}")
print(f"Número máximo: {numero_maximo}")
print(f"Número mínimo: {numero_minimo}")
print(f"Rango: {rango}")
```

---

## 🔍 Ejercicios de Depuración

### **Ejercicio 7: Encontrar y Corregir Errores**
**Objetivo**: Practicar la identificación y corrección de errores

**Instrucciones**: El siguiente código tiene errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
nombre = "Juan"
edad = "25"  # Error: debería ser número
altura = 1.75
es_estudiante = "True"  # Error: debería ser booleano

# Cálculos
edad_futura = edad + 5  # Error: no se puede sumar string + número
altura_cm = altura * 100

# Mostrar información
print("Nombre:", nombre)
print("Edad actual:", edad)
print("Edad en 5 años:", edad_futura)
print("Altura en cm:", altura_cm)
print("¿Es estudiante?:", es_estudiante)
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
nombre = "Juan"
edad = 25  # Corregido: ahora es número
altura = 1.75
es_estudiante = True  # Corregido: ahora es booleano

# Cálculos
edad_futura = edad + 5  # Corregido: ahora funciona
altura_cm = altura * 100

# Mostrar información
print("Nombre:", nombre)
print("Edad actual:", edad)
print("Edad en 5 años:", edad_futura)
print("Altura en cm:", altura_cm)
print("¿Es estudiante?:", es_estudiante)
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Variables básicas completado
- [ ] Ejercicio 2: Operaciones numéricas completado
- [ ] Ejercicio 3: Conversión de tipos completado
- [ ] Ejercicio 4: Calculadora de descuentos completado
- [ ] Ejercicio 5: Gestor de información personal completado
- [ ] Ejercicio 6: Calculadora de estadísticas completado
- [ ] Ejercicio 7: Depuración de errores completado

### **Puntuación**
- **Ejercicios básicos**: ___/3
- **Ejercicios intermedios**: ___/2
- **Ejercicios avanzados**: ___/1
- **Depuración**: ___/1

**Puntuación Total**: ___/7

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Calculadora de Interés Compuesto**
Crea un programa que calcule el interés compuesto de una inversión.

### **Desafío 2: Conversor de Unidades**
Crea un programa que convierta entre diferentes unidades (metros a pies, kilogramos a libras, etc.).

### **Desafío 3: Generador de Contraseñas**
Crea un programa que genere contraseñas seguras basadas en criterios específicos.

---

## 📝 Notas y Reflexiones

**¿Qué aprendiste hoy?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué fue más difícil?**
```
[Escribe aquí las dificultades]
```

**¿Qué te gustaría practicar más?**
```
[Escribe aquí tus intereses]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [02-Ejercicios-Operadores.md](./02-ejercicios-operadores.md)

**En la siguiente lección practicarás con operadores aritméticos, lógicos y de comparación.**

---

**¡Excelente trabajo! Has completado los ejercicios de variables y tipos de datos. Ahora estás listo para aprender sobre operadores.** 