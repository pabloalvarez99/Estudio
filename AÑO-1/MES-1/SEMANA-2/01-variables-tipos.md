# 🎯 Lección 2.1: Variables y Tipos de Datos

## 📚 Concepto Principal

### **¿Qué es una Variable?**
Una variable es como una caja donde guardamos información que puede cambiar. Es el concepto más fundamental en programación.

### **Analogía Simple**
Imagina que tienes diferentes tipos de cajas:
- **Caja de números**: Para guardar edades, precios, cantidades
- **Caja de texto**: Para guardar nombres, direcciones, mensajes
- **Caja de verdadero/falso**: Para guardar respuestas sí/no
- **Caja de decimales**: Para guardar medidas, pesos, temperaturas

### **Tipos de Datos en Python**

#### **1. Números Enteros (int)**
```python
edad = 25
año = 2024
temperatura = -5
```
- **Uso**: Edades, años, cantidades enteras
- **Ejemplos**: 1, 100, -50, 0

#### **2. Números Decimales (float)**
```python
altura = 1.75
precio = 19.99
temperatura = 23.5
```
- **Uso**: Medidas, precios, temperaturas
- **Ejemplos**: 3.14, 2.5, -0.5

#### **3. Texto (string)**
```python
nombre = "María"
ciudad = 'Madrid'
mensaje = "Hola mundo"
```
- **Uso**: Nombres, direcciones, mensajes
- **Nota**: Siempre entre comillas

#### **4. Valores Booleanos (bool)**
```python
es_estudiante = True
tiene_experiencia = False
```
- **Uso**: Condiciones verdadero/falso
- **Valores**: True o False

---

## 🛠️ Ejercicios Prácticos

### **Ejercicio 1: Identificar Tipos de Datos**
**Objetivo**: Reconocer el tipo de dato correcto para cada variable

**Instrucciones**: Para cada concepto, escribe qué tipo de dato usarías y un ejemplo

**Conceptos**:
1. **Nombre de usuario** → Tipo: ___ → Ejemplo: ___
2. **Edad** → Tipo: ___ → Ejemplo: ___
3. **Precio de un producto** → Tipo: ___ → Ejemplo: ___
4. **¿Está activo?** → Tipo: ___ → Ejemplo: ___
5. **Número de teléfono** → Tipo: ___ → Ejemplo: ___

**Respuestas correctas**:
1. **Nombre de usuario** → Tipo: string → Ejemplo: "juan123"
2. **Edad** → Tipo: int → Ejemplo: 25
3. **Precio de un producto** → Tipo: float → Ejemplo: 29.99
4. **¿Está activo?** → Tipo: bool → Ejemplo: True
5. **Número de teléfono** → Tipo: string → Ejemplo: "555-1234"

---

### **Ejercicio 2: Crear Variables en Python**
**Objetivo**: Practicar la creación de variables con diferentes tipos de datos

**Instrucciones**: Crea variables para almacenar información de un estudiante

**Información del estudiante**:
- Nombre: Ana García
- Edad: 20 años
- Altura: 1.68 metros
- ¿Está matriculado?: Sí
- Promedio: 8.5

**Tu código Python**:
```python
# Escribe aquí tu código
nombre = 
edad = 
altura = 
esta_matriculado = 
promedio = 

# Imprime las variables para verificar
print("Nombre:", nombre)
print("Edad:", edad)
print("Altura:", altura)
print("¿Está matriculado?:", esta_matriculado)
print("Promedio:", promedio)
```

**Código correcto**:
```python
nombre = "Ana García"
edad = 20
altura = 1.68
esta_matriculado = True
promedio = 8.5
```

---

### **Ejercicio 3: Operaciones con Variables**
**Objetivo**: Practicar operaciones matemáticas con variables

**Problema**: Calcular el área de un rectángulo

**Instrucciones**:
1. Crea variables para base y altura
2. Calcula el área (base × altura)
3. Muestra el resultado

**Tu código**:
```python
# Variables del rectángulo
base = 
altura = 

# Cálculo del área
area = 

# Mostrar resultado
print("El área del rectángulo es:", area)
```

**Ejemplo con valores**:
```python
base = 5
altura = 3
area = base * altura
print("El área del rectángulo es:", area)
# Resultado: El área del rectángulo es: 15
```

**Prueba con diferentes valores**:
- base = 10, altura = 4 → área = ___
- base = 7.5, altura = 2 → área = ___

---

### **Ejercicio 4: Conversión de Tipos**
**Objetivo**: Aprender a convertir entre diferentes tipos de datos

**Instrucciones**: Completa las conversiones de tipos

**Ejercicios**:
```python
# 1. Convierte el número 42 a texto
numero = 42
texto = str(numero)
print("El número como texto:", texto)

# 2. Convierte el texto "15" a número
texto_numero = "15"
numero_convertido = int(texto_numero)
print("El texto como número:", numero_convertido)

# 3. Convierte 3.8 a entero
decimal = 3.8
entero = int(decimal)
print("El decimal como entero:", entero)
```

**Tu turno - Completa estas conversiones**:
```python
# Convierte "100" a número entero
texto = "100"
numero = 
print("Resultado:", numero)

# Convierte 25 a texto
edad = 25
edad_texto = 
print("Edad como texto:", edad_texto)

# Convierte 9.7 a entero
promedio = 9.7
promedio_entero = 
print("Promedio como entero:", promedio_entero)
```

---

### **Ejercicio 5: Aplicación Práctica**
**Objetivo**: Crear un programa que calcule el precio total de una compra

**Problema**: Calcular el precio total de una compra con impuestos

**Instrucciones**:
1. Crea variables para precio base, cantidad e impuesto
2. Calcula el subtotal (precio × cantidad)
3. Calcula el impuesto (subtotal × porcentaje de impuesto)
4. Calcula el total (subtotal + impuesto)
5. Muestra todos los valores

**Tu código**:
```python
# Variables de la compra
precio_base = 
cantidad = 
porcentaje_impuesto = 

# Cálculos
subtotal = 
impuesto = 
total = 

# Mostrar resultados
print("=== RESUMEN DE COMPRA ===")
print("Precio base:", precio_base)
print("Cantidad:", cantidad)
print("Subtotal:", subtotal)
print("Impuesto:", impuesto)
print("TOTAL:", total)
```

**Ejemplo con valores**:
```python
precio_base = 25.50
cantidad = 3
porcentaje_impuesto = 0.16  # 16%

subtotal = precio_base * cantidad
impuesto = subtotal * porcentaje_impuesto
total = subtotal + impuesto

print("=== RESUMEN DE COMPRA ===")
print("Precio base: $25.50")
print("Cantidad: 3")
print("Subtotal: $76.50")
print("Impuesto: $12.24")
print("TOTAL: $88.74")
```

---

## 🔍 Conceptos Clave a Recordar

- ✅ **Variable** = Caja para almacenar información
- ✅ **int** = Números enteros (1, 2, 3, -5)
- ✅ **float** = Números decimales (3.14, 2.5)
- ✅ **string** = Texto ("Hola", 'Mundo')
- ✅ **bool** = Verdadero o falso (True, False)
- ✅ **str()** = Convierte a texto
- ✅ **int()** = Convierte a entero
- ✅ **float()** = Convierte a decimal

---

## 📝 Tarea para la Próxima Lección

**Práctica**:
- Crea un programa que calcule tu edad en días
- Experimenta con diferentes tipos de variables
- Practica las conversiones de tipos

**Preparación**:
- Revisa los operadores aritméticos básicos
- Piensa en ejemplos de uso de cada tipo de dato

---

## 🎯 Evaluación de la Lección

**Autoevaluación**:
- [ ] Entiendo qué es una variable
- [ ] Puedo identificar tipos de datos
- [ ] Sé crear variables en Python
- [ ] Puedo convertir entre tipos de datos
- [ ] Estoy listo para operadores aritméticos

**Puntuación**: ___/10

---

**¡Excelente progreso! Ahora entiendes cómo almacenar información en variables. En la próxima lección aprenderemos a realizar operaciones matemáticas con estas variables.** 