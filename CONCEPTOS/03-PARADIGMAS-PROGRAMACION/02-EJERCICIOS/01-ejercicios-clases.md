# 01. Ejercicios de Clases en POO

## 🎯 Ejercicios Básicos

### **Ejercicio 1: Clase Persona**
Crea una clase `Persona` con los siguientes atributos y métodos:

```python
# Atributos: nombre, edad, email
# Métodos: saludar(), cumplir_anios(), obtener_info()
```

**Solución:**
```python
class Persona:
    def __init__(self, nombre, edad, email):
        self.nombre = nombre
        self.edad = edad
        self.email = email
    
    def saludar(self):
        return f"¡Hola! Soy {self.nombre}"
    
    def cumplir_anios(self):
        self.edad += 1
        return f"¡Feliz cumpleaños! Ahora tengo {self.edad} años"
    
    def obtener_info(self):
        return f"Nombre: {self.nombre}, Edad: {self.edad}, Email: {self.email}"

# Uso
persona = Persona("Ana", 25, "ana@email.com")
print(persona.saludar())
print(persona.cumplir_anios())
print(persona.obtener_info())
```

### **Ejercicio 2: Clase Rectángulo**
Implementa una clase `Rectangulo` que calcule área y perímetro:

```python
# Atributos: ancho, alto
# Métodos: calcular_area(), calcular_perimetro(), es_cuadrado()
```

**Solución:**
```python
class Rectangulo:
    def __init__(self, ancho, alto):
        self.ancho = ancho
        self.alto = alto
    
    def calcular_area(self):
        return self.ancho * self.alto
    
    def calcular_perimetro(self):
        return 2 * (self.ancho + self.alto)
    
    def es_cuadrado(self):
        return self.ancho == self.alto
    
    def obtener_info(self):
        return f"Rectángulo {self.ancho}x{self.alto}, Área: {self.calcular_area()}, Perímetro: {self.calcular_perimetro()}"

# Uso
rect = Rectangulo(5, 3)
print(rect.calcular_area())      # 15
print(rect.calcular_perimetro()) # 16
print(rect.es_cuadrado())        # False

cuadrado = Rectangulo(4, 4)
print(cuadrado.es_cuadrado())    # True
```

### **Ejercicio 3: Clase Calculadora**
Crea una clase `Calculadora` con operaciones básicas:

```python
# Métodos: sumar(), restar(), multiplicar(), dividir(), obtener_historial()
```

**Solución:**
```python
class Calculadora:
    def __init__(self):
        self.historial = []
    
    def sumar(self, a, b):
        resultado = a + b
        self._registrar_operacion("suma", a, b, resultado)
        return resultado
    
    def restar(self, a, b):
        resultado = a - b
        self._registrar_operacion("resta", a, b, resultado)
        return resultado
    
    def multiplicar(self, a, b):
        resultado = a * b
        self._registrar_operacion("multiplicación", a, b, resultado)
        return resultado
    
    def dividir(self, a, b):
        if b == 0:
            raise ValueError("No se puede dividir por cero")
        resultado = a / b
        self._registrar_operacion("división", a, b, resultado)
        return resultado
    
    def _registrar_operacion(self, operacion, a, b, resultado):
        self.historial.append({
            "operacion": operacion,
            "a": a,
            "b": b,
            "resultado": resultado
        })
    
    def obtener_historial(self):
        return self.historial
    
    def limpiar_historial(self):
        self.historial.clear()

# Uso
calc = Calculadora()
print(calc.sumar(10, 5))        # 15
print(calc.multiplicar(4, 3))   # 12
print(calc.dividir(20, 4))      # 5.0

for op in calc.obtener_historial():
    print(f"{op['operacion']}: {op['a']} y {op['b']} = {op['resultado']}")
```

## 🔧 Ejercicios Intermedios

### **Ejercicio 4: Clase Libro con Validaciones**
Implementa una clase `Libro` con validaciones y propiedades:

```python
# Atributos: titulo, autor, isbn, precio, stock
# Validaciones: precio > 0, stock >= 0, isbn válido
# Métodos: vender(), reponer(), obtener_valor_total()
```

**Solución:**
```python
class Libro:
    def __init__(self, titulo, autor, isbn, precio, stock):
        self._titulo = self._validar_titulo(titulo)
        self._autor = self._validar_autor(autor)
        self._isbn = self._validar_isbn(isbn)
        self._precio = self._validar_precio(precio)
        self._stock = self._validar_stock(stock)
    
    def _validar_titulo(self, titulo):
        if not titulo or len(titulo.strip()) < 2:
            raise ValueError("El título debe tener al menos 2 caracteres")
        return titulo.strip()
    
    def _validar_autor(self, autor):
        if not autor or len(autor.strip()) < 2:
            raise ValueError("El autor debe tener al menos 2 caracteres")
        return autor.strip()
    
    def _validar_isbn(self, isbn):
        # Validación simple de ISBN (10 o 13 dígitos)
        isbn_limpio = str(isbn).replace("-", "").replace(" ", "")
        if not isbn_limpio.isdigit() or len(isbn_limpio) not in [10, 13]:
            raise ValueError("ISBN debe tener 10 o 13 dígitos")
        return isbn_limpio
    
    def _validar_precio(self, precio):
        if not isinstance(precio, (int, float)) or precio <= 0:
            raise ValueError("El precio debe ser un número positivo")
        return float(precio)
    
    def _validar_stock(self, stock):
        if not isinstance(stock, int) or stock < 0:
            raise ValueError("El stock debe ser un entero no negativo")
        return stock
    
    @property
    def titulo(self):
        return self._titulo
    
    @property
    def autor(self):
        return self._autor
    
    @property
    def isbn(self):
        return self._isbn
    
    @property
    def precio(self):
        return self._precio
    
    @precio.setter
    def precio(self, nuevo_precio):
        self._precio = self._validar_precio(nuevo_precio)
    
    @property
    def stock(self):
        return self._stock
    
    def vender(self, cantidad=1):
        if cantidad <= 0:
            raise ValueError("La cantidad debe ser positiva")
        if cantidad > self._stock:
            raise ValueError(f"No hay suficiente stock. Disponible: {self._stock}")
        
        self._stock -= cantidad
        return f"Vendidos {cantidad} ejemplares de '{self._titulo}'"
    
    def reponer(self, cantidad):
        if cantidad <= 0:
            raise ValueError("La cantidad a reponer debe ser positiva")
        
        self._stock += cantidad
        return f"Repuestos {cantidad} ejemplares de '{self._titulo}'"
    
    def obtener_valor_total(self):
        return self._precio * self._stock
    
    def obtener_info(self):
        return (f"'{self._titulo}' por {self._autor} "
                f"(ISBN: {self._isbn}) - ${self._precio:.2f} - Stock: {self._stock}")

# Uso
try:
    libro = Libro("Python Programming", "John Smith", "1234567890", 29.99, 10)
    print(libro.obtener_info())
    
    print(libro.vender(3))           # Vendidos 3 ejemplares
    print(libro.reponer(5))          # Repuestos 5 ejemplares
    print(f"Valor total: ${libro.obtener_valor_total():.2f}")
    
    # Cambiar precio
    libro.precio = 34.99
    print(f"Nuevo precio: ${libro.precio:.2f}")
    
except ValueError as e:
    print(f"Error: {e}")
```

### **Ejercicio 5: Clase Banco con Transacciones**
Crea una clase `Banco` que maneje cuentas y transacciones:

```python
# Clases: Banco, Cuenta, Transaccion
# Funcionalidades: crear_cuenta(), depositar(), retirar(), transferir()
```

**Solución:**
```python
from datetime import datetime
from typing import List, Dict

class Transaccion:
    def __init__(self, tipo: str, monto: float, cuenta_origen: str, cuenta_destino: str = None):
        self.tipo = tipo
        self.monto = monto
        self.cuenta_origen = cuenta_origen
        self.cuenta_destino = cuenta_destino
        self.fecha = datetime.now()
        self.id = f"TXN_{self.fecha.strftime('%Y%m%d_%H%M%S')}_{hash(self)}"
    
    def obtener_info(self):
        if self.cuenta_destino:
            return f"{self.id}: {self.tipo} ${self.monto:.2f} de {self.cuenta_origen} a {self.cuenta_destino}"
        else:
            return f"{self.id}: {self.tipo} ${self.monto:.2f} en {self.cuenta_origen}"

class Cuenta:
    def __init__(self, numero: str, titular: str, saldo_inicial: float = 0):
        self.numero = numero
        self.titular = titular
        self.saldo = saldo_inicial
        self.transacciones: List[Transaccion] = []
        
        if saldo_inicial > 0:
            self._registrar_transaccion("deposito_inicial", saldo_inicial, numero)
    
    def _registrar_transaccion(self, tipo: str, monto: float, cuenta_destino: str = None):
        transaccion = Transaccion(tipo, monto, self.numero, cuenta_destino)
        self.transacciones.append(transaccion)
    
    def depositar(self, monto: float) -> bool:
        if monto <= 0:
            return False
        
        self.saldo += monto
        self._registrar_transaccion("deposito", monto)
        return True
    
    def retirar(self, monto: float) -> bool:
        if monto <= 0 or monto > self.saldo:
            return False
        
        self.saldo -= monto
        self._registrar_transaccion("retiro", monto)
        return True
    
    def transferir(self, cuenta_destino: 'Cuenta', monto: float) -> bool:
        if self.retirar(monto):
            cuenta_destino.depositar(monto)
            self._registrar_transaccion("transferencia_salida", monto, cuenta_destino.numero)
            cuenta_destino._registrar_transaccion("transferencia_entrada", monto, self.numero)
            return True
        return False
    
    def obtener_historial(self) -> List[Transaccion]:
        return self.transacciones.copy()
    
    def obtener_info(self) -> str:
        return f"Cuenta {self.numero} - {self.titular} - Saldo: ${self.saldo:.2f}"

class Banco:
    def __init__(self, nombre: str):
        self.nombre = nombre
        self.cuentas: Dict[str, Cuenta] = {}
        self.contador_cuentas = 0
    
    def crear_cuenta(self, titular: str, saldo_inicial: float = 0) -> str:
        self.contador_cuentas += 1
        numero_cuenta = f"CUENTA_{self.contador_cuentas:06d}"
        
        cuenta = Cuenta(numero_cuenta, titular, saldo_inicial)
        self.cuentas[numero_cuenta] = cuenta
        
        return numero_cuenta
    
    def obtener_cuenta(self, numero: str) -> Cuenta:
        return self.cuentas.get(numero)
    
    def depositar(self, numero_cuenta: str, monto: float) -> bool:
        cuenta = self.obtener_cuenta(numero_cuenta)
        if cuenta:
            return cuenta.depositar(monto)
        return False
    
    def retirar(self, numero_cuenta: str, monto: float) -> bool:
        cuenta = self.obtener_cuenta(numero_cuenta)
        if cuenta:
            return cuenta.retirar(monto)
        return False
    
    def transferir(self, cuenta_origen: str, cuenta_destino: str, monto: float) -> bool:
        cuenta_orig = self.obtener_cuenta(cuenta_origen)
        cuenta_dest = self.obtener_cuenta(cuenta_destino)
        
        if cuenta_orig and cuenta_dest:
            return cuenta_orig.transferir(cuenta_dest, monto)
        return False
    
    def obtener_resumen(self) -> str:
        total_cuentas = len(self.cuentas)
        total_saldo = sum(cuenta.saldo for cuenta in self.cuentas.values())
        
        return (f"Banco: {self.nombre}\n"
                f"Total de cuentas: {total_cuentas}\n"
                f"Saldo total: ${total_saldo:.2f}")
    
    def listar_cuentas(self) -> List[str]:
        return [cuenta.obtener_info() for cuenta in self.cuentas.values()]

# Uso
banco = Banco("Banco Ejemplo")

# Crear cuentas
cuenta1 = banco.crear_cuenta("Ana García", 1000)
cuenta2 = banco.crear_cuenta("Bob Smith", 500)

print(f"Cuenta creada: {cuenta1}")
print(f"Cuenta creada: {cuenta2}")

# Operaciones
banco.depositar(cuenta1, 200)
banco.retirar(cuenta1, 100)
banco.transferir(cuenta1, cuenta2, 150)

# Información
print("\n" + "="*50)
print(banco.obtener_resumen())
print("\nCuentas:")
for info in banco.listar_cuentas():
    print(f"- {info}")

# Historial de transacciones
print(f"\nHistorial de {cuenta1}:")
for trans in banco.obtener_cuenta(cuenta1).obtener_historial():
    print(f"  {trans.obtener_info()}")
```

## 🚀 Ejercicios Avanzados

### **Ejercicio 6: Sistema de Gestión de Empleados**
Implementa un sistema completo de gestión de empleados:

```python
# Clases: Empleado, EmpleadoPorHora, EmpleadoComision, Departamento
# Funcionalidades: calcular_salario(), agregar_empleado(), generar_nomina()
```

**Solución:**
```python
from abc import ABC, abstractmethod
from typing import List, Dict
from datetime import datetime

class Empleado(ABC):
    def __init__(self, id: str, nombre: str, email: str):
        self.id = id
        self.nombre = nombre
        self.email = email
        self.fecha_contratacion = datetime.now()
        self.activo = True
    
    @abstractmethod
    def calcular_salario(self) -> float:
        pass
    
    def obtener_info(self) -> str:
        return f"ID: {self.id}, Nombre: {self.nombre}, Email: {self.email}"
    
    def desactivar(self):
        self.activo = False
    
    def activar(self):
        self.activo = True

class EmpleadoPorHora(Empleado):
    def __init__(self, id: str, nombre: str, email: str, tarifa_hora: float, horas_trabajadas: float):
        super().__init__(id, nombre, email)
        self.tarifa_hora = tarifa_hora
        self.horas_trabajadas = horas_trabajadas
    
    def calcular_salario(self) -> float:
        # Horas extra (más de 40 horas) se pagan 1.5x
        if self.horas_trabajadas <= 40:
            return self.horas_trabajadas * self.tarifa_hora
        else:
            horas_normales = 40
            horas_extra = self.horas_trabajadas - 40
            return (horas_normales * self.tarifa_hora) + (horas_extra * self.tarifa_hora * 1.5)
    
    def agregar_horas(self, horas: float):
        if horas > 0:
            self.horas_trabajadas += horas
    
    def obtener_info(self) -> str:
        return (f"{super().obtener_info()}, "
                f"Tarifa: ${self.tarifa_hora}/h, "
                f"Horas: {self.horas_trabajadas}")

class EmpleadoComision(Empleado):
    def __init__(self, id: str, nombre: str, email: str, salario_base: float, comision_porcentaje: float):
        super().__init__(id, nombre, email)
        self.salario_base = salario_base
        self.comision_porcentaje = comision_porcentaje
        self.ventas_mes = 0.0
    
    def calcular_salario(self) -> float:
        comision = self.ventas_mes * (self.comision_porcentaje / 100)
        return self.salario_base + comision
    
    def agregar_venta(self, monto: float):
        if monto > 0:
            self.ventas_mes += monto
    
    def resetear_ventas_mes(self):
        self.ventas_mes = 0.0
    
    def obtener_info(self) -> str:
        return (f"{super().obtener_info()}, "
                f"Salario base: ${self.salario_base:.2f}, "
                f"Comisión: {self.comision_porcentaje}%, "
                f"Ventas del mes: ${self.ventas_mes:.2f}")

class Departamento:
    def __init__(self, nombre: str, presupuesto: float):
        self.nombre = nombre
        self.presupuesto = presupuesto
        self.empleados: List[Empleado] = []
    
    def agregar_empleado(self, empleado: Empleado) -> bool:
        if empleado not in self.empleados:
            self.empleados.append(empleado)
            return True
        return False
    
    def remover_empleado(self, empleado: Empleado) -> bool:
        if empleado in self.empleados:
            self.empleados.remove(empleado)
            return True
        return False
    
    def obtener_empleados_activos(self) -> List[Empleado]:
        return [emp for emp in self.empleados if emp.activo]
    
    def calcular_total_salarios(self) -> float:
        return sum(emp.calcular_salario() for emp in self.obtener_empleados_activos())
    
    def verificar_presupuesto(self) -> Dict[str, any]:
        total_salarios = self.calcular_total_salarios()
        disponible = self.presupuesto - total_salarios
        
        return {
            "presupuesto": self.presupuesto,
            "total_salarios": total_salarios,
            "disponible": disponible,
            "dentro_presupuesto": disponible >= 0
        }
    
    def obtener_info(self) -> str:
        empleados_activos = len(self.obtener_empleados_activos())
        total_salarios = self.calcular_total_salarios()
        
        return (f"Departamento: {self.nombre}\n"
                f"Empleados activos: {empleados_activos}\n"
                f"Presupuesto: ${self.presupuesto:.2f}\n"
                f"Total salarios: ${total_salarios:.2f}")

class SistemaGestionEmpleados:
    def __init__(self):
        self.departamentos: Dict[str, Departamento] = {}
        self.empleados: Dict[str, Empleado] = {}
        self.contador_empleados = 0
    
    def crear_empleado_por_hora(self, nombre: str, email: str, tarifa_hora: float, 
                                horas_trabajadas: float, departamento: str) -> str:
        self.contador_empleados += 1
        id_empleado = f"EMP_{self.contador_empleados:06d}"
        
        empleado = EmpleadoPorHora(id_empleado, nombre, email, tarifa_hora, horas_trabajadas)
        self.empleados[id_empleado] = empleado
        
        # Agregar al departamento
        if departamento in self.departamentos:
            self.departamentos[departamento].agregar_empleado(empleado)
        
        return id_empleado
    
    def crear_empleado_comision(self, nombre: str, email: str, salario_base: float,
                               comision_porcentaje: float, departamento: str) -> str:
        self.contador_empleados += 1
        id_empleado = f"EMP_{self.contador_empleados:06d}"
        
        empleado = EmpleadoComision(id_empleado, nombre, email, salario_base, comision_porcentaje)
        self.empleados[id_empleado] = empleado
        
        # Agregar al departamento
        if departamento in self.departamentos:
            self.departamentos[departamento].agregar_empleado(empleado)
        
        return id_empleado
    
    def crear_departamento(self, nombre: str, presupuesto: float):
        self.departamentos[nombre] = Departamento(nombre, presupuesto)
    
    def generar_nomina(self) -> str:
        nomina = "=== NÓMINA MENSUAL ===\n\n"
        
        for dept_nombre, departamento in self.departamentos.items():
            nomina += f"{departamento.obtener_info()}\n"
            
            empleados_activos = departamento.obtener_empleados_activos()
            if empleados_activos:
                nomina += "Empleados:\n"
                for empleado in empleados_activos:
                    nomina += f"  - {empleado.obtener_info()}\n"
                    nomina += f"    Salario: ${empleado.calcular_salario():.2f}\n"
            else:
                nomina += "  Sin empleados activos\n"
            
            # Verificar presupuesto
            presupuesto_info = departamento.verificar_presupuesto()
            nomina += f"Estado presupuesto: {'✅' if presupuesto_info['dentro_presupuesto'] else '❌'}\n"
            nomina += f"Disponible: ${presupuesto_info['disponible']:.2f}\n"
            nomina += "-" * 50 + "\n\n"
        
        return nomina

# Uso del sistema
sistema = SistemaGestionEmpleados()

# Crear departamentos
sistema.crear_departamento("Ventas", 50000)
sistema.crear_departamento("Desarrollo", 80000)
sistema.crear_departamento("Marketing", 30000)

# Crear empleados
emp1 = sistema.crear_empleado_por_hora("Ana García", "ana@empresa.com", 25.0, 160, "Desarrollo")
emp2 = sistema.crear_empleado_comision("Bob Smith", "bob@empresa.com", 2000, 5, "Ventas")
emp3 = sistema.crear_empleado_por_hora("Carlos López", "carlos@empresa.com", 20.0, 180, "Desarrollo")

# Agregar horas y ventas
sistema.empleados[emp1].agregar_horas(10)  # Ana trabajó 10 horas extra
sistema.empleados[emp2].agregar_venta(50000)  # Bob vendió $50,000

# Generar nómina
print(sistema.generar_nomina())
```

## 📝 Ejercicios de Práctica

### **Ejercicio 7: Implementa una Clase Coche**
Crea una clase `Coche` con las siguientes características:

- Atributos: marca, modelo, año, color, velocidad_actual, combustible
- Métodos: arrancar(), parar(), acelerar(), frenar(), repostar()
- Validaciones: velocidad no puede ser negativa, combustible entre 0 y 100

### **Ejercicio 8: Clase Biblioteca**
Implementa una clase `Biblioteca` que gestione libros y préstamos:

- Clases: Biblioteca, Libro, Usuario, Prestamo
- Funcionalidades: agregar_libro(), prestar_libro(), devolver_libro(), buscar_libro()
- Validaciones: libro disponible, usuario sin multas, fecha de devolución

### **Ejercicio 9: Sistema de Reservas**
Crea un sistema de reservas para un restaurante:

- Clases: Restaurante, Mesa, Reserva, Cliente
- Funcionalidades: hacer_reserva(), cancelar_reserva(), verificar_disponibilidad()
- Lógica: mesas disponibles, horarios, capacidad

## 🔍 Preguntas de Reflexión

1. **¿Cuándo usarías una clase abstracta vs una interfaz?**
2. **¿Cómo manejas la validación de datos en las clases?**
3. **¿Qué ventajas tiene usar propiedades (@property) en lugar de atributos públicos?**
4. **¿Cómo implementarías un sistema de logging en las clases?**
5. **¿Qué patrones de diseño aplicarías para mejorar la arquitectura?**

## 📚 Recursos Adicionales

- **Documentación Python**: Clases y POO
- **Libros**: "Python Object-Oriented Programming" - Steven F. Lott
- **Práctica**: Implementar un juego simple con múltiples clases
- **Proyectos**: Sistema de gestión de inventario, API REST con modelos

---

**Siguiente**: [02. Ejercicios de Herencia](./02-ejercicios-herencia.md) 