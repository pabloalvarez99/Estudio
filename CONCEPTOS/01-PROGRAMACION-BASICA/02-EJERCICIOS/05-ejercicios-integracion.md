# 🛠️ Ejercicios Prácticos: Integración de Conceptos

## 🎯 Objetivo de la Lección
Integrar todos los conceptos aprendidos de programación básica en ejercicios complejos y proyectos reales. Aplicar variables, operadores, estructuras de control y bucles en situaciones prácticas.

## ⏱️ Tiempo Estimado: 60-90 minutos

---

## 📚 Repaso Rápido de Conceptos

### **Conceptos Integrados**
- **Variables y Tipos de Datos**: int, float, string, bool
- **Operadores**: Aritméticos, comparación, lógicos, asignación
- **Estructuras de Control**: if-elif-else, while, for
- **Bucles Avanzados**: enumerate, zip, comprensión de listas
- **Manejo de Errores**: try-except, validación de datos

---

## 🏋️ Ejercicios de Integración Básica

### **Ejercicio 1: Sistema de Gestión de Estudiantes**
**Objetivo**: Integrar variables, operadores y estructuras de control

**Instrucciones**: Crea un sistema que gestione información de estudiantes

**Tu código**:
```python
# Sistema de Gestión de Estudiantes
estudiantes = []
opciones = ["1", "2", "3", "4", "5"]

def mostrar_menu():
    print("\n=== SISTEMA DE GESTIÓN DE ESTUDIANTES ===")
    print("1. Agregar estudiante")
    print("2. Mostrar todos los estudiantes")
    print("3. Buscar estudiante")
    print("4. Calcular estadísticas")
    print("5. Salir")

def agregar_estudiante():
    print("\n--- AGREGAR ESTUDIANTE ---")
    
    # Obtener información del estudiante
    nombre = input("Nombre: ").strip()
    if not nombre:
        print("❌ El nombre es obligatorio")
        return
    
    try:
        edad = int(input("Edad: "))
        if edad < 0 or edad > 120:
            print("❌ La edad debe estar entre 0 y 120")
            return
    except ValueError:
        print("❌ La edad debe ser un número entero")
        return
    
    try:
        promedio = float(input("Promedio (0-10): "))
        if promedio < 0 or promedio > 10:
            print("❌ El promedio debe estar entre 0 y 10")
            return
    except ValueError:
        print("❌ El promedio debe ser un número")
        return
    
    carrera = input("Carrera: ").strip()
    
    # Crear diccionario del estudiante
    estudiante = {
        "nombre": nombre,
        "edad": edad,
        "promedio": promedio,
        "carrera": carrera
    }
    
    estudiantes.append(estudiante)
    print(f"✅ Estudiante '{nombre}' agregado exitosamente")

def mostrar_estudiantes():
    if not estudiantes:
        print("📭 No hay estudiantes registrados")
        return
    
    print(f"\n--- LISTA DE ESTUDIANTES ({len(estudiantes)}) ---")
    for i, estudiante in enumerate(estudiantes, 1):
        print(f"{i}. {estudiante['nombre']} - {estudiante['edad']} años")
        print(f"   Carrera: {estudiante['carrera']}")
        print(f"   Promedio: {estudiante['promedio']:.2f}")
        print()

def buscar_estudiante():
    if not estudiantes:
        print("📭 No hay estudiantes para buscar")
        return
    
    print("\n--- BUSCAR ESTUDIANTE ---")
    termino = input("Ingresa el nombre a buscar: ").strip().lower()
    
    if not termino:
        print("❌ Debes ingresar un término de búsqueda")
        return
    
    resultados = []
    for estudiante in estudiantes:
        if termino in estudiante['nombre'].lower():
            resultados.append(estudiante)
    
    if resultados:
        print(f"\n🔍 RESULTADOS ({len(resultados)} encontrados):")
        for estudiante in resultados:
            print(f"  {estudiante['nombre']} - {estudiante['carrera']} - Promedio: {estudiante['promedio']:.2f}")
    else:
        print("🔍 No se encontraron estudiantes con ese nombre")

def calcular_estadisticas():
    if not estudiantes:
        print("📭 No hay estudiantes para calcular estadísticas")
        return
    
    print("\n--- ESTADÍSTICAS GENERALES ---")
    
    # Estadísticas básicas
    total_estudiantes = len(estudiantes)
    edades = [e['edad'] for e in estudiantes]
    promedios = [e['promedio'] for e in estudiantes]
    
    # Edad promedio
    edad_promedio = sum(edades) / len(edades)
    print(f"Total de estudiantes: {total_estudiantes}")
    print(f"Edad promedio: {edad_promedio:.1f} años")
    
    # Promedio general
    promedio_general = sum(promedios) / len(promedios)
    print(f"Promedio general: {promedio_general:.2f}")
    
    # Estudiantes por carrera
    carreras = {}
    for estudiante in estudiantes:
        carrera = estudiante['carrera']
        carreras[carrera] = carreras.get(carrera, 0) + 1
    
    print("\nEstudiantes por carrera:")
    for carrera, cantidad in carreras.items():
        print(f"  {carrera}: {cantidad} estudiantes")
    
    # Mejores estudiantes (promedio >= 9.0)
    mejores = [e for e in estudiantes if e['promedio'] >= 9.0]
    if mejores:
        print(f"\n🏆 Mejores estudiantes ({len(mejores)}):")
        for estudiante in mejores:
            print(f"  {estudiante['nombre']}: {estudiante['promedio']:.2f}")

def main():
    print("¡Bienvenido al Sistema de Gestión de Estudiantes!")
    
    while True:
        mostrar_menu()
        opcion = input("\nSelecciona una opción (1-5): ")
        
        if opcion == "1":
            agregar_estudiante()
        elif opcion == "2":
            mostrar_estudiantes()
        elif opcion == "3":
            buscar_estudiante()
        elif opcion == "4":
            calcular_estadisticas()
        elif opcion == "5":
            print("👋 ¡Gracias por usar el sistema!")
            break
        else:
            print("❌ Opción no válida. Por favor selecciona 1-5")

if __name__ == "__main__":
    main()
```

---

## 🚀 Ejercicios de Integración Intermedia

### **Ejercicio 2: Calculadora Avanzada con Historial**
**Objetivo**: Integrar operadores, estructuras de control y manejo de datos

**Instrucciones**: Crea una calculadora que mantenga historial de operaciones

**Tu código**:
```python
# Calculadora Avanzada con Historial
import math
import time

class Calculadora:
    def __init__(self):
        self.historial = []
        self.resultado_actual = 0
    
    def mostrar_menu(self):
        print("\n=== CALCULADORA AVANZADA ===")
        print("1. Operación básica")
        print("2. Operación científica")
        print("3. Ver historial")
        print("4. Limpiar historial")
        print("5. Salir")
    
    def operacion_basica(self):
        print("\n--- OPERACIÓN BÁSICA ---")
        print("Operadores disponibles: +, -, *, /, //, %, **")
        
        try:
            num1 = float(input("Primer número: "))
            operador = input("Operador: ").strip()
            num2 = float(input("Segundo número: "))
            
            if operador == "+":
                resultado = num1 + num2
            elif operador == "-":
                resultado = num1 - num2
            elif operador == "*":
                resultado = num1 * num2
            elif operador == "/":
                if num2 == 0:
                    print("❌ Error: División por cero")
                    return
                resultado = num1 / num2
            elif operador == "//":
                if num2 == 0:
                    print("❌ Error: División por cero")
                    return
                resultado = num1 // num2
            elif operador == "%":
                if num2 == 0:
                    print("❌ Error: División por cero")
                    return
                resultado = num1 % num2
            elif operador == "**":
                resultado = num1 ** num2
            else:
                print("❌ Operador no válido")
                return
            
            # Guardar en historial
            operacion = f"{num1} {operador} {num2} = {resultado}"
            self.historial.append({
                "operacion": operacion,
                "resultado": resultado,
                "tipo": "básica",
                "timestamp": time.strftime("%H:%M:%S")
            })
            
            self.resultado_actual = resultado
            print(f"✅ Resultado: {resultado}")
            
        except ValueError:
            print("❌ Error: Ingresa números válidos")
    
    def operacion_cientifica(self):
        print("\n--- OPERACIÓN CIENTÍFICA ---")
        print("1. Raíz cuadrada")
        print("2. Potencia")
        print("3. Factorial")
        print("4. Seno")
        print("5. Coseno")
        
        try:
            opcion = input("Selecciona operación (1-5): ")
            
            if opcion == "1":
                num = float(input("Número: "))
                if num < 0:
                    print("❌ Error: No se puede calcular raíz de número negativo")
                    return
                resultado = math.sqrt(num)
                operacion = f"√{num} = {resultado}"
                
            elif opcion == "2":
                base = float(input("Base: "))
                exponente = float(input("Exponente: "))
                resultado = base ** exponente
                operacion = f"{base}^{exponente} = {resultado}"
                
            elif opcion == "3":
                num = int(input("Número entero: "))
                if num < 0:
                    print("❌ Error: No se puede calcular factorial de número negativo")
                    return
                resultado = math.factorial(num)
                operacion = f"{num}! = {resultado}"
                
            elif opcion == "4":
                num = float(input("Ángulo en radianes: "))
                resultado = math.sin(num)
                operacion = f"sin({num}) = {resultado}"
                
            elif opcion == "5":
                num = float(input("Ángulo en radianes: "))
                resultado = math.cos(num)
                operacion = f"cos({num}) = {resultado}"
                
            else:
                print("❌ Opción no válida")
                return
            
            # Guardar en historial
            self.historial.append({
                "operacion": operacion,
                "resultado": resultado,
                "tipo": "científica",
                "timestamp": time.strftime("%H:%M:%S")
            })
            
            self.resultado_actual = resultado
            print(f"✅ Resultado: {resultado}")
            
        except ValueError:
            print("❌ Error: Ingresa números válidos")
    
    def ver_historial(self):
        if not self.historial:
            print("📭 No hay operaciones en el historial")
            return
        
        print(f"\n--- HISTORIAL DE OPERACIONES ({len(self.historial)}) ---")
        for i, item in enumerate(self.historial, 1):
            print(f"{i}. [{item['timestamp']}] {item['operacion']}")
            print(f"   Tipo: {item['tipo']}")
            print()
    
    def limpiar_historial(self):
        if not self.historial:
            print("📭 El historial ya está vacío")
            return
        
        confirmacion = input("¿Estás seguro de limpiar el historial? (sí/no): ").lower()
        if confirmacion == "sí":
            self.historial.clear()
            print("✅ Historial limpiado")
        else:
            print("❌ Operación cancelada")
    
    def ejecutar(self):
        print("¡Bienvenido a la Calculadora Avanzada!")
        
        while True:
            self.mostrar_menu()
            opcion = input("\nSelecciona una opción (1-5): ")
            
            if opcion == "1":
                self.operacion_basica()
            elif opcion == "2":
                self.operacion_cientifica()
            elif opcion == "3":
                self.ver_historial()
            elif opcion == "4":
                self.limpiar_historial()
            elif opcion == "5":
                print("👋 ¡Gracias por usar la calculadora!")
                break
            else:
                print("❌ Opción no válida. Por favor selecciona 1-5")

# Ejecutar la calculadora
if __name__ == "__main__":
    calc = Calculadora()
    calc.ejecutar()
```

---

## 🎯 Ejercicios de Integración Avanzada

### **Ejercicio 3: Sistema de Gestión de Biblioteca**
**Objetivo**: Integrar todos los conceptos en un sistema complejo

**Instrucciones**: Crea un sistema completo de gestión de biblioteca

**Tu código**:
```python
# Sistema de Gestión de Biblioteca
import json
import time
from datetime import datetime, timedelta

class Biblioteca:
    def __init__(self):
        self.libros = {}
        self.usuarios = {}
        self.prestamos = {}
        self.cargar_datos()
    
    def cargar_datos(self):
        # Datos de ejemplo
        self.libros = {
            "001": {"titulo": "Python para Principiantes", "autor": "Juan Pérez", "genero": "Programación", "disponible": True},
            "002": {"titulo": "El Señor de los Anillos", "autor": "J.R.R. Tolkien", "genero": "Fantasía", "disponible": True},
            "003": {"titulo": "Cien Años de Soledad", "autor": "Gabriel García Márquez", "genero": "Literatura", "disponible": True}
        }
        
        self.usuarios = {
            "U001": {"nombre": "Ana García", "email": "ana@email.com", "telefono": "123-456-7890"},
            "U002": {"nombre": "Carlos López", "email": "carlos@email.com", "telefono": "098-765-4321"}
        }
    
    def mostrar_menu_principal(self):
        print("\n" + "="*50)
        print("        📚 SISTEMA DE GESTIÓN DE BIBLIOTECA 📚")
        print("="*50)
        print("1. 📖 Gestión de Libros")
        print("2. 👥 Gestión de Usuarios")
        print("3. 🔄 Gestión de Préstamos")
        print("4. 📊 Reportes y Estadísticas")
        print("5. 💾 Guardar y Salir")
        print("="*50)
    
    def gestion_libros(self):
        while True:
            print("\n--- GESTIÓN DE LIBROS ---")
            print("1. Agregar libro")
            print("2. Mostrar todos los libros")
            print("3. Buscar libro")
            print("4. Modificar libro")
            print("5. Eliminar libro")
            print("6. Volver al menú principal")
            
            opcion = input("Selecciona opción (1-6): ")
            
            if opcion == "1":
                self.agregar_libro()
            elif opcion == "2":
                self.mostrar_libros()
            elif opcion == "3":
                self.buscar_libro()
            elif opcion == "4":
                self.modificar_libro()
            elif opcion == "5":
                self.eliminar_libro()
            elif opcion == "6":
                break
            else:
                print("❌ Opción no válida")
    
    def agregar_libro(self):
        print("\n--- AGREGAR NUEVO LIBRO ---")
        
        codigo = input("Código del libro: ").strip()
        if codigo in self.libros:
            print("❌ Ya existe un libro con ese código")
            return
        
        titulo = input("Título: ").strip()
        if not titulo:
            print("❌ El título es obligatorio")
            return
        
        autor = input("Autor: ").strip()
        genero = input("Género: ").strip()
        
        self.libros[codigo] = {
            "titulo": titulo,
            "autor": autor,
            "genero": genero,
            "disponible": True
        }
        
        print(f"✅ Libro '{titulo}' agregado exitosamente")
    
    def mostrar_libros(self):
        if not self.libros:
            print("📭 No hay libros registrados")
            return
        
        print(f"\n--- CATÁLOGO DE LIBROS ({len(self.libros)}) ---")
        for codigo, libro in self.libros.items():
            estado = "✅ Disponible" if libro['disponible'] else "❌ Prestado"
            print(f"Código: {codigo}")
            print(f"Título: {libro['titulo']}")
            print(f"Autor: {libro['autor']}")
            print(f"Género: {libro['genero']}")
            print(f"Estado: {estado}")
            print("-" * 40)
    
    def buscar_libro(self):
        if not self.libros:
            print("📭 No hay libros para buscar")
            return
        
        print("\n--- BUSCAR LIBRO ---")
        termino = input("Ingresa término de búsqueda: ").strip().lower()
        
        if not termino:
            print("❌ Debes ingresar un término de búsqueda")
            return
        
        resultados = []
        for codigo, libro in self.libros.items():
            if (termino in libro['titulo'].lower() or 
                termino in libro['autor'].lower() or 
                termino in libro['genero'].lower()):
                resultados.append((codigo, libro))
        
        if resultados:
            print(f"\n🔍 RESULTADOS ({len(resultados)} encontrados):")
            for codigo, libro in resultados:
                estado = "✅ Disponible" if libro['disponible'] else "❌ Prestado"
                print(f"  {codigo}: {libro['titulo']} - {libro['autor']} - {estado}")
        else:
            print("🔍 No se encontraron libros con ese término")
    
    def gestion_prestamos(self):
        while True:
            print("\n--- GESTIÓN DE PRÉSTAMOS ---")
            print("1. Prestar libro")
            print("2. Devolver libro")
            print("3. Ver préstamos activos")
            print("4. Historial de préstamos")
            print("5. Volver al menú principal")
            
            opcion = input("Selecciona opción (1-5): ")
            
            if opcion == "1":
                self.prestar_libro()
            elif opcion == "2":
                self.devolver_libro()
            elif opcion == "3":
                self.ver_prestamos_activos()
            elif opcion == "4":
                self.historial_prestamos()
            elif opcion == "5":
                break
            else:
                print("❌ Opción no válida")
    
    def prestar_libro(self):
        print("\n--- PRESTAR LIBRO ---")
        
        # Mostrar libros disponibles
        libros_disponibles = {k: v for k, v in self.libros.items() if v['disponible']}
        if not libros_disponibles:
            print("📭 No hay libros disponibles para prestar")
            return
        
        print("Libros disponibles:")
        for codigo, libro in libros_disponibles.items():
            print(f"  {codigo}: {libro['titulo']}")
        
        codigo_libro = input("Código del libro: ").strip()
        if codigo_libro not in libros_disponibles:
            print("❌ Código de libro no válido o no disponible")
            return
        
        # Mostrar usuarios
        if not self.usuarios:
            print("📭 No hay usuarios registrados")
            return
        
        print("\nUsuarios disponibles:")
        for codigo, usuario in self.usuarios.items():
            print(f"  {codigo}: {usuario['nombre']}")
        
        codigo_usuario = input("Código del usuario: ").strip()
        if codigo_usuario not in self.usuarios:
            print("❌ Código de usuario no válido")
            return
        
        # Crear préstamo
        fecha_prestamo = datetime.now()
        fecha_devolucion = fecha_prestamo + timedelta(days=14)
        
        prestamo_id = f"P{len(self.prestamos) + 1:03d}"
        self.prestamos[prestamo_id] = {
            "libro": codigo_libro,
            "usuario": codigo_usuario,
            "fecha_prestamo": fecha_prestamo.strftime("%Y-%m-%d"),
            "fecha_devolucion": fecha_devolucion.strftime("%Y-%m-%d"),
            "devuelto": False
        }
        
        # Marcar libro como prestado
        self.libros[codigo_libro]['disponible'] = False
        
        print(f"✅ Libro prestado exitosamente")
        print(f"   ID de préstamo: {prestamo_id}")
        print(f"   Fecha de devolución: {fecha_devolucion.strftime('%Y-%m-%d')}")
    
    def ejecutar(self):
        print("¡Bienvenido al Sistema de Gestión de Biblioteca!")
        
        while True:
            self.mostrar_menu_principal()
            opcion = input("\nSelecciona una opción (1-5): ")
            
            if opcion == "1":
                self.gestion_libros()
            elif opcion == "2":
                print("🔄 Funcionalidad en desarrollo...")
            elif opcion == "3":
                self.gestion_prestamos()
            elif opcion == "4":
                print("🔄 Funcionalidad en desarrollo...")
            elif opcion == "5":
                print("💾 Guardando datos...")
                print("👋 ¡Gracias por usar el sistema!")
                break
            else:
                print("❌ Opción no válida. Por favor selecciona 1-5")

# Ejecutar la biblioteca
if __name__ == "__main__":
    biblio = Biblioteca()
    biblio.ejecutar()
```

---

## 🔍 Ejercicios de Depuración Avanzada

### **Ejercicio 4: Encontrar y Corregir Errores Complejos**
**Objetivo**: Practicar la identificación y corrección de errores en código complejo

**Instrucciones**: El siguiente código tiene múltiples errores. Identifícalos y corrígelos

**Código con errores**:
```python
# Código con errores - ¡Corrígelo!
class GestorProductos:
    def __init__(self):
        self.productos = []
        self.categorias = ["tecnologia", "ropa", "alimentos"]
    
    def agregar_producto(self):
        nombre = input("Nombre del producto: ")
        precio = input("Precio: ")
        categoria = input("Categoría: ")
        
        if precio > 0 and categoria in self.categorias:
            producto = {
                "nombre": nombre,
                "precio": precio,
                "categoria": categoria
            }
            self.productos.append(producto)
            print("Producto agregado")
        else:
            print("Datos inválidos")
    
    def mostrar_productos(self):
        for i in range(len(self.productos)):
            print(f"{i}: {self.productos[i]['nombre']} - ${self.productos[i]['precio']}")
    
    def buscar_por_categoria(self, cat):
        resultados = []
        for producto in self.productos:
            if producto['categoria'] == cat:
                resultados.append(producto)
        return resultados
    
    def calcular_total(self):
        total = 0
        for producto in self.productos:
            total = total + producto['precio']
        return total

def main():
    gestor = GestorProductos()
    
    while True:
        print("1. Agregar producto")
        print("2. Mostrar productos")
        print("3. Buscar por categoría")
        print("4. Calcular total")
        print("5. Salir")
        
        opcion = input("Opción: ")
        
        if opcion == "1":
            gestor.agregar_producto()
        elif opcion == "2":
            gestor.mostrar_productos()
        elif opcion == "3":
            categoria = input("Categoría a buscar: ")
            resultados = gestor.buscar_por_categoria(categoria)
            print(f"Encontrados: {len(resultados)}")
        elif opcion == "4":
            total = gestor.calcular_total()
            print(f"Total: ${total}")
        elif opcion == "5":
            break
        else:
            print("Opción inválida")

if __name__ == "__main__":
    main()
```

**Tu código corregido**:
```python
# Escribe aquí el código corregido
```

**Código correcto**:
```python
class GestorProductos:
    def __init__(self):
        self.productos = []
        self.categorias = ["tecnologia", "ropa", "alimentos"]
    
    def agregar_producto(self):
        nombre = input("Nombre del producto: ").strip()
        if not nombre:
            print("❌ El nombre es obligatorio")
            return
        
        try:
            precio = float(input("Precio: "))
            if precio <= 0:
                print("❌ El precio debe ser mayor a 0")
                return
        except ValueError:
            print("❌ El precio debe ser un número válido")
            return
        
        categoria = input("Categoría: ").strip().lower()
        if categoria not in self.categorias:
            print(f"❌ Categoría no válida. Opciones: {', '.join(self.categorias)}")
            return
        
        producto = {
            "nombre": nombre,
            "precio": precio,
            "categoria": categoria
        }
        self.productos.append(producto)
        print("✅ Producto agregado exitosamente")
    
    def mostrar_productos(self):
        if not self.productos:
            print("📭 No hay productos registrados")
            return
        
        print(f"\n--- PRODUCTOS ({len(self.productos)}) ---")
        for i, producto in enumerate(self.productos):
            print(f"{i+1}: {producto['nombre']} - ${producto['precio']:.2f} - {producto['categoria']}")
    
    def buscar_por_categoria(self, cat):
        resultados = []
        for producto in self.productos:
            if producto['categoria'] == cat.lower():
                resultados.append(producto)
        return resultados
    
    def calcular_total(self):
        total = 0
        for producto in self.productos:
            total += producto['precio']
        return total

def main():
    gestor = GestorProductos()
    
    while True:
        print("\n=== GESTOR DE PRODUCTOS ===")
        print("1. Agregar producto")
        print("2. Mostrar productos")
        print("3. Buscar por categoría")
        print("4. Calcular total")
        print("5. Salir")
        
        opcion = input("\nOpción: ").strip()
        
        if opcion == "1":
            gestor.agregar_producto()
        elif opcion == "2":
            gestor.mostrar_productos()
        elif opcion == "3":
            categoria = input("Categoría a buscar: ").strip()
            resultados = gestor.buscar_por_categoria(categoria)
            if resultados:
                print(f"\n🔍 Encontrados: {len(resultados)} productos")
                for producto in resultados:
                    print(f"  {producto['nombre']} - ${producto['precio']:.2f}")
            else:
                print("🔍 No se encontraron productos en esa categoría")
        elif opcion == "4":
            total = gestor.calcular_total()
            print(f"\n💰 Total: ${total:.2f}")
        elif opcion == "5":
            print("👋 ¡Hasta luego!")
            break
        else:
            print("❌ Opción inválida")

if __name__ == "__main__":
    main()
```

---

## 📊 Evaluación del Ejercicio

### **Checklist de Completado**
- [ ] Ejercicio 1: Sistema de Gestión de Estudiantes completado
- [ ] Ejercicio 2: Calculadora Avanzada con Historial completado
- [ ] Ejercicio 3: Sistema de Gestión de Biblioteca completado
- [ ] Ejercicio 4: Depuración de errores complejos completado

### **Puntuación**
- **Ejercicios básicos**: ___/1
- **Ejercicios intermedios**: ___/1
- **Ejercicios avanzados**: ___/1
- **Depuración**: ___/1

**Puntuación Total**: ___/4

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Sistema de Gestión de Restaurante**
Crea un sistema completo que gestione menús, pedidos, mesas y facturación.

### **Desafío 2: Plataforma de Gestión de Tareas**
Implementa un sistema que permita crear, asignar, priorizar y dar seguimiento a tareas.

### **Desafío 3: Sistema de Gestión de Inventario**
Desarrolla un sistema completo de control de inventario con alertas de stock bajo.

---

## 📝 Notas y Reflexiones

**¿Qué conceptos te resultaron más fáciles de integrar?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué conceptos te resultaron más difíciles de integrar?**
```
[Escribe aquí las dificultades]
```

**¿Cómo crees que esta integración te prepara para proyectos reales?**
```
[Escribe aquí tus reflexiones]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [01-Proyecto-Calculadora.md](../03-PROYECTOS/01-proyecto-calculadora.md)

**En la siguiente lección comenzarás a trabajar en proyectos prácticos que integren todos los conceptos aprendidos.**

---

**¡Excelente trabajo! Has completado la integración de todos los conceptos de programación básica. Ahora estás listo para crear proyectos complejos y funcionales.** 