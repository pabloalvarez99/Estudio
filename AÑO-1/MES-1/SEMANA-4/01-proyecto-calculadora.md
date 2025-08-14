# 🎯 Proyecto Integrador: Calculadora Simple

## 📋 Descripción del Proyecto
Crear una calculadora que pueda realizar operaciones básicas (suma, resta, multiplicación, división) con una interfaz de consola.

## 🎯 Objetivos del Proyecto
- Aplicar todos los conceptos aprendidos en el mes
- Crear un programa funcional y completo
- Practicar la entrada de datos del usuario
- Implementar lógica de decisión
- Manejar diferentes tipos de datos

## 🛠️ Requisitos del Proyecto

### **Funcionalidades Mínimas:**
1. **Suma** de dos números
2. **Resta** de dos números
3. **Multiplicación** de dos números
4. **División** de dos números
5. **Interfaz de usuario** clara

### **Funcionalidades Extras (Opcionales):**
- Validación de entrada
- Manejo de errores
- Historial de operaciones
- Operaciones con más de dos números

---

## 📚 Conceptos Aplicados

### **Del MES 1:**
- ✅ Variables y tipos de datos
- ✅ Operadores aritméticos
- ✅ Estructuras de control (if-else)
- ✅ Bucles (while)
- ✅ Entrada y salida de datos

---

## 🚀 Implementación Paso a Paso

### **Paso 1: Estructura Básica**
Crea un archivo llamado `calculadora.py` y comienza con esta estructura:

```python
# Calculadora Simple
# Autor: [Tu nombre]
# Fecha: [Fecha actual]

print("=== CALCULADORA SIMPLE ===")
print("Operaciones disponibles:")
print("1. Suma")
print("2. Resta")
print("3. Multiplicación")
print("4. División")
print("5. Salir")
```

### **Paso 2: Bucle Principal**
Agrega el bucle principal del programa:

```python
while True:
    print("\n" + "="*30)
    
    # Mostrar menú
    print("=== CALCULADORA SIMPLE ===")
    print("Operaciones disponibles:")
    print("1. Suma")
    print("2. Resta")
    print("3. Multiplicación")
    print("4. División")
    print("5. Salir")
    
    # Obtener opción del usuario
    opcion = input("\nSelecciona una operación (1-5): ")
    
    # Salir del programa
    if opcion == "5":
        print("¡Gracias por usar la calculadora!")
        break
```

### **Paso 3: Obtener Números**
Después del menú, agrega la entrada de números:

```python
    # Obtener números del usuario
    try:
        num1 = float(input("Ingresa el primer número: "))
        num2 = float(input("Ingresa el segundo número: "))
    except ValueError:
        print("Error: Por favor ingresa números válidos")
        continue
```

### **Paso 4: Realizar Operaciones**
Implementa la lógica de las operaciones:

```python
    # Realizar operación seleccionada
    if opcion == "1":
        resultado = num1 + num2
        operacion = "suma"
    elif opcion == "2":
        resultado = num1 - num2
        operacion = "resta"
    elif opcion == "3":
        resultado = num1 * num2
        operacion = "multiplicación"
    elif opcion == "4":
        if num2 == 0:
            print("Error: No se puede dividir por cero")
            continue
        resultado = num1 / num2
        operacion = "división"
    else:
        print("Opción no válida. Por favor selecciona 1-5")
        continue
    
    # Mostrar resultado
    print(f"\nResultado de la {operacion}:")
    print(f"{num1} {operacion} {num2} = {resultado}")
```

---

## 🔧 Código Completo del Proyecto

```python
# Calculadora Simple
# Autor: [Tu nombre]
# Fecha: [Fecha actual]

def mostrar_menu():
    """Función para mostrar el menú de opciones"""
    print("\n" + "="*30)
    print("=== CALCULADORA SIMPLE ===")
    print("Operaciones disponibles:")
    print("1. Suma")
    print("2. Resta")
    print("3. Multiplicación")
    print("4. División")
    print("5. Salir")

def realizar_operacion(num1, num2, operacion):
    """Función para realizar la operación matemática"""
    if operacion == "suma":
        return num1 + num2
    elif operacion == "resta":
        return num1 - num2
    elif operacion == "multiplicación":
        return num1 * num2
    elif operacion == "división":
        if num2 == 0:
            return None  # Error: división por cero
        return num1 / num2

def main():
    """Función principal del programa"""
    print("¡Bienvenido a la Calculadora Simple!")
    
    while True:
        mostrar_menu()
        
        # Obtener opción del usuario
        opcion = input("\nSelecciona una operación (1-5): ")
        
        # Salir del programa
        if opcion == "5":
            print("¡Gracias por usar la calculadora!")
            break
        
        # Validar opción
        if opcion not in ["1", "2", "3", "4"]:
            print("Opción no válida. Por favor selecciona 1-5")
            continue
        
        # Obtener números del usuario
        try:
            num1 = float(input("Ingresa el primer número: "))
            num2 = float(input("Ingresa el segundo número: "))
        except ValueError:
            print("Error: Por favor ingresa números válidos")
            continue
        
        # Mapear opción a operación
        operaciones = {
            "1": "suma",
            "2": "resta", 
            "3": "multiplicación",
            "4": "división"
        }
        
        operacion = operaciones[opcion]
        
        # Realizar operación
        resultado = realizar_operacion(num1, num2, operacion)
        
        # Verificar si hubo error
        if resultado is None:
            print("Error: No se puede dividir por cero")
            continue
        
        # Mostrar resultado
        print(f"\nResultado de la {operacion}:")
        print(f"{num1} {operacion} {num2} = {resultado}")

# Ejecutar el programa
if __name__ == "__main__":
    main()
```

---

## 🧪 Pruebas del Proyecto

### **Casos de Prueba Básicos:**
1. **Suma**: 5 + 3 = 8
2. **Resta**: 10 - 4 = 6
3. **Multiplicación**: 6 × 7 = 42
4. **División**: 15 ÷ 3 = 5

### **Casos de Prueba Avanzados:**
1. **División por cero**: 10 ÷ 0 → Error
2. **Números decimales**: 3.5 + 2.7 = 6.2
3. **Números negativos**: -5 + 8 = 3
4. **Entrada inválida**: "abc" → Error

---

## 🎨 Mejoras Opcionales

### **Nivel 1: Validación Mejorada**
```python
def validar_numero(texto):
    """Función para validar entrada numérica"""
    while True:
        try:
            return float(input(texto))
        except ValueError:
            print("Error: Por favor ingresa un número válido")
```

### **Nivel 2: Historial de Operaciones**
```python
historial = []

# Después de cada operación exitosa:
historial.append(f"{num1} {operacion} {num2} = {resultado}")

# Mostrar historial:
if historial:
    print("\n=== HISTORIAL ===")
    for i, operacion in enumerate(historial, 1):
        print(f"{i}. {operacion}")
```

### **Nivel 3: Operaciones Avanzadas**
```python
print("6. Potencia")
print("7. Raíz cuadrada")
print("8. Módulo (resto)")

# Implementar nuevas operaciones
elif opcion == "6":
    resultado = num1 ** num2
    operacion = "potencia"
elif opcion == "7":
    resultado = num1 ** 0.5
    operacion = "raíz cuadrada"
elif opcion == "8":
    resultado = num1 % num2
    operacion = "módulo"
```

---

## 📊 Criterios de Evaluación

### **Funcionalidad (40 puntos):**
- [ ] Suma funciona correctamente
- [ ] Resta funciona correctamente
- [ ] Multiplicación funciona correctamente
- [ ] División funciona correctamente
- [ ] Manejo de división por cero

### **Código (30 puntos):**
- [ ] Código está bien estructurado
- [ ] Variables tienen nombres descriptivos
- [ ] Comentarios explicativos
- [ ] Manejo de errores

### **Interfaz (20 puntos):**
- [ ] Menú claro y fácil de usar
- [ ] Mensajes informativos
- [ ] Formato de salida consistente

### **Extras (10 puntos):**
- [ ] Funcionalidades adicionales implementadas
- [ ] Código optimizado y eficiente

**Puntuación Total**: ___/100

---

## 🎯 Desafíos Adicionales

### **Desafío 1: Calculadora Científica**
Implementa funciones trigonométricas, logaritmos y exponenciales.

### **Desafío 2: Interfaz Gráfica**
Usa tkinter para crear una calculadora con botones.

### **Desafío 3: Calculadora de Conversión**
Convierte entre diferentes unidades (temperatura, longitud, peso).

---

## 📝 Reflexión del Proyecto

**Preguntas para reflexionar:**
1. ¿Qué fue lo más difícil de implementar?
2. ¿Cómo podrías mejorar el código?
3. ¿Qué nuevas funcionalidades te gustaría agregar?
4. ¿Cómo aplicaste los conceptos aprendidos?

**Tus respuestas:**
```
1. [Escribe aquí]
2. [Escribe aquí]
3. [Escribe aquí]
4. [Escribe aquí]
```

---

## 🚀 Próximos Pasos

**Después de completar este proyecto:**
1. **Revisa el código** y busca mejoras
2. **Experimenta** con nuevas funcionalidades
3. **Comparte** tu proyecto con otros
4. **Preparate** para el MES 2: Estructuras de Datos

---

**¡Felicitaciones! Has completado tu primer proyecto de programación. Este es un gran logro que demuestra tu comprensión de los conceptos fundamentales.** 