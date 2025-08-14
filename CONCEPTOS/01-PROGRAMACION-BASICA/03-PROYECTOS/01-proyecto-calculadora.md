# 🚀 Proyecto: Calculadora Simple

## 📋 Descripción del Proyecto
Crear una calculadora funcional que simule las operaciones básicas de una calculadora real. Este proyecto te permitirá aplicar todos los conceptos aprendidos sobre variables, tipos de datos y operadores.

## 🎯 Objetivos del Proyecto
- **Aplicar conceptos**: Usar variables, tipos de datos y operadores
- **Crear interfaz**: Desarrollar una interfaz de usuario clara
- **Implementar lógica**: Crear algoritmos para operaciones matemáticas
- **Manejar errores**: Implementar validación básica de entrada
- **Simular realidad**: Crear una herramienta útil y funcional

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 🎮 Simulación del Mundo Real

### **Contexto del Proyecto**
Imagina que estás trabajando en una empresa de desarrollo de software y tu jefe te pide crear una calculadora simple para que los empleados puedan hacer cálculos rápidos sin abrir Excel o usar la calculadora del sistema operativo.

### **Requisitos del Cliente**
- Debe realizar operaciones básicas (suma, resta, multiplicación, división)
- Interfaz clara y fácil de usar
- Manejo de errores (división por cero, entrada inválida)
- Debe ser rápida y eficiente

---

## 🛠️ Implementación Paso a Paso

### **Paso 1: Estructura Básica del Programa**
Crea un archivo llamado `calculadora.py` y comienza con esta estructura:

```python
# Calculadora Simple
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

def mostrar_menu():
    """Muestra el menú principal de la calculadora"""
    print("\n" + "="*40)
    print("        🧮 CALCULADORA SIMPLE 🧮")
    print("="*40)
    print("Operaciones disponibles:")
    print("1. ➕ Suma")
    print("2. ➖ Resta")
    print("3. ✖️  Multiplicación")
    print("4. ➗ División")
    print("5. 🧮 Calculadora científica básica")
    print("6. ❌ Salir")
    print("="*40)

def main():
    """Función principal del programa"""
    print("¡Bienvenido a la Calculadora Simple!")
    
    while True:
        mostrar_menu()
        opcion = input("\nSelecciona una operación (1-6): ")
        
        if opcion == "6":
            print("¡Gracias por usar la calculadora! 👋")
            break
        
        # Aquí irá la lógica de las operaciones
        print("Funcionalidad en desarrollo...")

if __name__ == "__main__":
    main()
```

### **Paso 2: Implementar Operaciones Básicas**
Agrega las funciones para las operaciones matemáticas:

```python
def obtener_numeros():
    """Obtiene dos números del usuario con validación"""
    while True:
        try:
            num1 = float(input("Ingresa el primer número: "))
            num2 = float(input("Ingresa el segundo número: "))
            return num1, num2
        except ValueError:
            print("❌ Error: Por favor ingresa números válidos")

def suma(num1, num2):
    """Realiza la suma de dos números"""
    return num1 + num2

def resta(num1, num2):
    """Realiza la resta de dos números"""
    return num1 - num2

def multiplicacion(num1, num2):
    """Realiza la multiplicación de dos números"""
    return num1 * num2

def division(num1, num2):
    """Realiza la división de dos números con validación"""
    if num2 == 0:
        return None, "❌ Error: No se puede dividir por cero"
    return num1 / num2, None

def mostrar_resultado(operacion, num1, num2, resultado):
    """Muestra el resultado de la operación de forma clara"""
    print(f"\n📊 RESULTADO:")
    print(f"{num1} {operacion} {num2} = {resultado}")
```

### **Paso 3: Integrar Operaciones en el Menú Principal**
Modifica la función main para incluir las operaciones:

```python
def main():
    """Función principal del programa"""
    print("¡Bienvenido a la Calculadora Simple!")
    
    while True:
        mostrar_menu()
        opcion = input("\nSelecciona una operación (1-6): ")
        
        if opcion == "6":
            print("¡Gracias por usar la calculadora! 👋")
            break
        
        if opcion in ["1", "2", "3", "4"]:
            num1, num2 = obtener_numeros()
            
            if opcion == "1":
                resultado = suma(num1, num2)
                mostrar_resultado("+", num1, num2, resultado)
            elif opcion == "2":
                resultado = resta(num1, num2)
                mostrar_resultado("-", num1, num2, resultado)
            elif opcion == "3":
                resultado = multiplicacion(num1, num2)
                mostrar_resultado("×", num1, num2, resultado)
            elif opcion == "4":
                resultado, error = division(num1, num2)
                if error:
                    print(error)
                else:
                    mostrar_resultado("÷", num1, num2, resultado)
        elif opcion == "5":
            calculadora_cientifica()
        else:
            print("❌ Opción no válida. Por favor selecciona 1-6")
```

### **Paso 4: Agregar Funcionalidad Científica Básica**
Implementa operaciones adicionales:

```python
def calculadora_cientifica():
    """Calculadora científica con operaciones adicionales"""
    print("\n🧮 CALCULADORA CIENTÍFICA")
    print("1. Potencia (a^b)")
    print("2. Raíz cuadrada")
    print("3. Módulo (resto de división)")
    print("4. Volver al menú principal")
    
    opcion = input("Selecciona operación (1-4): ")
    
    if opcion == "1":
        num1, num2 = obtener_numeros()
        resultado = num1 ** num2
        print(f"{num1} ^ {num2} = {resultado}")
    elif opcion == "2":
        try:
            num = float(input("Ingresa un número: "))
            if num < 0:
                print("❌ Error: No se puede calcular raíz de número negativo")
            else:
                resultado = num ** 0.5
                print(f"√{num} = {resultado}")
        except ValueError:
            print("❌ Error: Ingresa un número válido")
    elif opcion == "3":
        num1, num2 = obtener_numeros()
        if num2 == 0:
            print("❌ Error: No se puede calcular módulo con divisor 0")
        else:
            resultado = num1 % num2
            print(f"{num1} % {num2} = {resultado}")
    elif opcion == "4":
        return
    else:
        print("❌ Opción no válida")
```

---

## 🔧 Código Completo del Proyecto

```python
# Calculadora Simple
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

def mostrar_menu():
    """Muestra el menú principal de la calculadora"""
    print("\n" + "="*40)
    print("        🧮 CALCULADORA SIMPLE 🧮")
    print("="*40)
    print("Operaciones disponibles:")
    print("1. ➕ Suma")
    print("2. ➖ Resta")
    print("3. ✖️  Multiplicación")
    print("4. ➗ División")
    print("5. 🧮 Calculadora científica básica")
    print("6. ❌ Salir")
    print("="*40)

def obtener_numeros():
    """Obtiene dos números del usuario con validación"""
    while True:
        try:
            num1 = float(input("Ingresa el primer número: "))
            num2 = float(input("Ingresa el segundo número: "))
            return num1, num2
        except ValueError:
            print("❌ Error: Por favor ingresa números válidos")

def suma(num1, num2):
    """Realiza la suma de dos números"""
    return num1 + num2

def resta(num1, num2):
    """Realiza la resta de dos números"""
    return num1 - num2

def multiplicacion(num1, num2):
    """Realiza la multiplicación de dos números"""
    return num1 * num2

def division(num1, num2):
    """Realiza la división de dos números con validación"""
    if num2 == 0:
        return None, "❌ Error: No se puede dividir por cero"
    return num1 / num2, None

def mostrar_resultado(operacion, num1, num2, resultado):
    """Muestra el resultado de la operación de forma clara"""
    print(f"\n📊 RESULTADO:")
    print(f"{num1} {operacion} {num2} = {resultado}")

def calculadora_cientifica():
    """Calculadora científica con operaciones adicionales"""
    print("\n🧮 CALCULADORA CIENTÍFICA")
    print("1. Potencia (a^b)")
    print("2. Raíz cuadrada")
    print("3. Módulo (resto de división)")
    print("4. Volver al menú principal")
    
    opcion = input("Selecciona operación (1-4): ")
    
    if opcion == "1":
        num1, num2 = obtener_numeros()
        resultado = num1 ** num2
        print(f"{num1} ^ {num2} = {resultado}")
    elif opcion == "2":
        try:
            num = float(input("Ingresa un número: "))
            if num < 0:
                print("❌ Error: No se puede calcular raíz de número negativo")
            else:
                resultado = num ** 0.5
                print(f"√{num} = {resultado}")
        except ValueError:
            print("❌ Error: Ingresa un número válido")
    elif opcion == "3":
        num1, num2 = obtener_numeros()
        if num2 == 0:
            print("❌ Error: No se puede calcular módulo con divisor 0")
        else:
            resultado = num1 % num2
            print(f"{num1} % {num2} = {resultado}")
    elif opcion == "4":
        return
    else:
        print("❌ Opción no válida")

def main():
    """Función principal del programa"""
    print("¡Bienvenido a la Calculadora Simple!")
    
    while True:
        mostrar_menu()
        opcion = input("\nSelecciona una operación (1-6): ")
        
        if opcion == "6":
            print("¡Gracias por usar la calculadora! 👋")
            break
        
        if opcion in ["1", "2", "3", "4"]:
            num1, num2 = obtener_numeros()
            
            if opcion == "1":
                resultado = suma(num1, num2)
                mostrar_resultado("+", num1, num2, resultado)
            elif opcion == "2":
                resultado = resta(num1, num2)
                mostrar_resultado("-", num1, num2, resultado)
            elif opcion == "3":
                resultado = multiplicacion(num1, num2)
                mostrar_resultado("×", num1, num2, resultado)
            elif opcion == "4":
                resultado, error = division(num1, num2)
                if error:
                    print(error)
                else:
                    mostrar_resultado("÷", num1, num2, resultado)
        elif opcion == "5":
            calculadora_cientifica()
        else:
            print("❌ Opción no válida. Por favor selecciona 1-6")

if __name__ == "__main__":
    main()
```

---

## 🧪 Pruebas del Proyecto

### **Casos de Prueba Básicos**
1. **Suma**: 5 + 3 = 8
2. **Resta**: 10 - 4 = 6
3. **Multiplicación**: 6 × 7 = 42
4. **División**: 15 ÷ 3 = 5

### **Casos de Prueba Avanzados**
1. **División por cero**: 10 ÷ 0 → Error
2. **Números decimales**: 3.5 + 2.7 = 6.2
3. **Números negativos**: -5 + 8 = 3
4. **Entrada inválida**: "abc" → Error

### **Casos de Prueba Científicos**
1. **Potencia**: 2^3 = 8
2. **Raíz cuadrada**: √16 = 4
3. **Módulo**: 17 % 5 = 2

---

## 🎨 Mejoras y Extensiones

### **Nivel 1: Funcionalidades Básicas**
- Historial de operaciones
- Memoria (M+, M-, MR, MC)
- Cambio de signo (+/-)

### **Nivel 2: Funcionalidades Avanzadas**
- Funciones trigonométricas
- Logaritmos
- Conversión de unidades

### **Nivel 3: Interfaz Gráfica**
- Botones con tkinter
- Interfaz más intuitiva
- Gráficos de funciones

---

## 📊 Criterios de Evaluación

### **Funcionalidad (40 puntos)**
- [ ] Suma funciona correctamente
- [ ] Resta funciona correctamente
- [ ] Multiplicación funciona correctamente
- [ ] División funciona correctamente
- [ ] Manejo de división por cero
- [ ] Funciones científicas funcionan

### **Código (30 puntos)**
- [ ] Código está bien estructurado
- [ ] Funciones tienen nombres descriptivos
- [ ] Comentarios explicativos
- [ ] Manejo de errores implementado

### **Interfaz (20 puntos)**
- [ ] Menú claro y fácil de usar
- [ ] Mensajes informativos
- [ ] Formato de salida consistente
- [ ] Emojis y formato atractivo

### **Extras (10 puntos)**
- [ ] Funcionalidades adicionales implementadas
- [ ] Código optimizado y eficiente

**Puntuación Total**: ___/100

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Calculadora de Impuestos**
Agrega funcionalidad para calcular impuestos, descuentos y totales.

### **Desafío 2: Calculadora de Conversión**
Implementa conversión entre diferentes unidades (temperatura, longitud, peso).

### **Desafío 3: Calculadora de Estadísticas**
Agrega funciones para calcular media, mediana, moda y desviación estándar.

---

## 📝 Reflexión del Proyecto

**¿Qué aprendiste implementando este proyecto?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué fue más difícil de implementar?**
```
[Escribe aquí las dificultades]
```

**¿Cómo podrías mejorar el proyecto?**
```
[Escribe aquí las mejoras]
```

**¿Qué nuevas funcionalidades te gustaría agregar?**
```
[Escribe aquí las funcionalidades]
```

---

## 🎯 Próximo Proyecto

**Archivo siguiente**: [02-Proyecto-Gestor-Contactos.md](./02-proyecto-gestor-contactos.md)

**En el siguiente proyecto crearás un gestor de contactos que te permitirá practicar con listas y estructuras de datos.**

---

## 🏆 Logros del Proyecto

**¡Felicitaciones! Has completado tu primera calculadora funcional. Este proyecto demuestra que:**

- ✅ Entiendes variables y tipos de datos
- ✅ Puedes implementar operaciones matemáticas
- ✅ Sabes manejar errores básicos
- ✅ Puedes crear programas útiles y funcionales
- ✅ Estás listo para proyectos más complejos

**¡Continúa con el siguiente proyecto para seguir desarrollando tus habilidades!** 