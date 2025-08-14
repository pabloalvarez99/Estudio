# 🚀 Proyecto: Sistema de Inventario

## 🎯 Objetivo del Proyecto
Crear un sistema de inventario completo que permita gestionar productos, realizar operaciones CRUD, generar reportes y aplicar los conceptos aprendidos sobre estructuras de datos en Python.

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 📋 Descripción del Proyecto

### **¿Qué es un Sistema de Inventario?**
Un sistema de inventario es una aplicación que permite gestionar el stock de productos de una empresa, incluyendo:
- **Registro de productos** con información detallada
- **Control de stock** (entradas, salidas, inventario actual)
- **Búsqueda y filtrado** de productos
- **Reportes** de inventario
- **Alertas** de stock bajo

### **Funcionalidades Principales**
1. **Gestión de Productos**: Agregar, editar, eliminar y consultar productos
2. **Control de Stock**: Registrar entradas y salidas de inventario
3. **Búsqueda Avanzada**: Filtrar por categoría, precio, stock
4. **Reportes**: Generar estadísticas del inventario
5. **Alertas**: Notificar productos con stock bajo

---

## 🏗️ Estructura del Proyecto

### **1. Clase Producto**
```python
class Producto:
    def __init__(self, id, nombre, categoria, precio, stock, descripcion=""):
        self.id = id
        self.nombre = nombre
        self.categoria = categoria
        self.precio = precio
        self.stock = stock
        self.descripcion = descripcion
        self.fecha_creacion = datetime.now()
        self.historial_movimientos = []
```

### **2. Clase SistemaInventario**
```python
class SistemaInventario:
    def __init__(self):
        self.productos = {}  # Diccionario de productos
        self.categorias = set()  # Set de categorías únicas
        self.contador_id = 1  # Contador para IDs únicos
```

### **3. Funcionalidades Principales**
- `agregar_producto()`
- `editar_producto()`
- `eliminar_producto()`
- `buscar_producto()`
- `registrar_movimiento()`
- `generar_reporte()`

---

## 🔧 Implementación Paso a Paso

### **Paso 1: Estructura Básica**
```python
from datetime import datetime
import json

class Producto:
    def __init__(self, id, nombre, categoria, precio, stock, descripcion=""):
        self.id = id
        self.nombre = nombre
        self.categoria = categoria
        self.precio = precio
        self.stock = stock
        self.descripcion = descripcion
        self.fecha_creacion = datetime.now()
        self.historial_movimientos = []
    
    def to_dict(self):
        """Convierte el producto a diccionario para serialización"""
        return {
            "id": self.id,
            "nombre": self.nombre,
            "categoria": self.categoria,
            "precio": self.precio,
            "stock": self.stock,
            "descripcion": self.descripcion,
            "fecha_creacion": self.fecha_creacion.isoformat(),
            "historial_movimientos": self.historial_movimientos
        }
    
    def __str__(self):
        return f"Producto {self.id}: {self.nombre} - ${self.precio} - Stock: {self.stock}"
```

### **Paso 2: Sistema Principal**
```python
class SistemaInventario:
    def __init__(self):
        self.productos = {}
        self.categorias = set()
        self.contador_id = 1
    
    def agregar_producto(self, nombre, categoria, precio, stock, descripcion=""):
        """Agrega un nuevo producto al inventario"""
        if nombre.strip() == "" or precio <= 0 or stock < 0:
            return False, "Datos inválidos"
        
        producto = Producto(self.contador_id, nombre, categoria, precio, stock, descripcion)
        self.productos[self.contador_id] = producto
        self.categorias.add(categoria)
        self.contador_id += 1
        
        return True, f"Producto '{nombre}' agregado exitosamente"
    
    def editar_producto(self, id_producto, **kwargs):
        """Edita un producto existente"""
        if id_producto not in self.productos:
            return False, "Producto no encontrado"
        
        producto = self.productos[id_producto]
        
        # Actualizar campos permitidos
        if "nombre" in kwargs and kwargs["nombre"].strip() != "":
            producto.nombre = kwargs["nombre"]
        if "categoria" in kwargs:
            producto.categoria = kwargs["categoria"]
            self.categorias.add(kwargs["categoria"])
        if "precio" in kwargs and kwargs["precio"] > 0:
            producto.precio = kwargs["precio"]
        if "descripcion" in kwargs:
            producto.descripcion = kwargs["descripcion"]
        
        return True, f"Producto {id_producto} actualizado exitosamente"
    
    def eliminar_producto(self, id_producto):
        """Elimina un producto del inventario"""
        if id_producto not in self.productos:
            return False, "Producto no encontrado"
        
        nombre = self.productos[id_producto].nombre
        del self.productos[id_producto]
        
        return True, f"Producto '{nombre}' eliminado exitosamente"
```

### **Paso 3: Gestión de Stock**
```python
    def registrar_movimiento(self, id_producto, tipo, cantidad, motivo=""):
        """Registra un movimiento de stock (entrada o salida)"""
        if id_producto not in self.productos:
            return False, "Producto no encontrado"
        
        producto = self.productos[id_producto]
        
        if tipo == "entrada":
            producto.stock += cantidad
            mensaje = f"Entrada de {cantidad} unidades"
        elif tipo == "salida":
            if producto.stock < cantidad:
                return False, "Stock insuficiente"
            producto.stock -= cantidad
            mensaje = f"Salida de {cantidad} unidades"
        else:
            return False, "Tipo de movimiento inválido"
        
        # Registrar en historial
        movimiento = {
            "fecha": datetime.now().isoformat(),
            "tipo": tipo,
            "cantidad": cantidad,
            "stock_anterior": producto.stock - (cantidad if tipo == "entrada" else -cantidad),
            "stock_nuevo": producto.stock,
            "motivo": motivo
        }
        producto.historial_movimientos.append(movimiento)
        
        return True, f"{mensaje} registrada para '{producto.nombre}'"
    
    def obtener_stock_bajo(self, limite=10):
        """Obtiene productos con stock bajo"""
        return {id_prod: prod for id_prod, prod in self.productos.items() 
                if prod.stock <= limite}
```

### **Paso 4: Búsqueda y Filtrado**
```python
    def buscar_producto(self, criterio, valor):
        """Busca productos por diferentes criterios"""
        resultados = {}
        
        if criterio == "id":
            if valor in self.productos:
                resultados[valor] = self.productos[valor]
        elif criterio == "nombre":
            for id_prod, producto in self.productos.items():
                if valor.lower() in producto.nombre.lower():
                    resultados[id_prod] = producto
        elif criterio == "categoria":
            for id_prod, producto in self.productos.items():
                if producto.categoria.lower() == valor.lower():
                    resultados[id_prod] = producto
        elif criterio == "precio_max":
            for id_prod, producto in self.productos.items():
                if producto.precio <= valor:
                    resultados[id_prod] = producto
        elif criterio == "stock_min":
            for id_prod, producto in self.productos.items():
                if producto.stock >= valor:
                    resultados[id_prod] = producto
        
        return resultados
    
    def filtrar_por_categoria(self, categoria):
        """Filtra productos por categoría"""
        return self.buscar_producto("categoria", categoria)
    
    def obtener_productos_caros(self, precio_minimo):
        """Obtiene productos con precio superior al mínimo"""
        return {id_prod: prod for id_prod, prod in self.productos.items() 
                if prod.precio >= precio_minimo}
```

### **Paso 5: Reportes y Estadísticas**
```python
    def generar_reporte_inventario(self):
        """Genera un reporte completo del inventario"""
        if not self.productos:
            return "No hay productos en el inventario"
        
        total_productos = len(self.productos)
        total_stock = sum(prod.stock for prod in self.productos.values())
        valor_total = sum(prod.precio * prod.stock for prod in self.productos.values())
        
        # Productos por categoría
        productos_por_categoria = {}
        for producto in self.productos.values():
            if producto.categoria not in productos_por_categoria:
                productos_por_categoria[producto.categoria] = []
            productos_por_categoria[producto.categoria].append(producto.nombre)
        
        # Stock bajo
        stock_bajo = self.obtener_stock_bajo()
        
        reporte = f"""
=== REPORTE DE INVENTARIO ===
Fecha: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

RESUMEN GENERAL:
- Total de productos: {total_productos}
- Total de unidades en stock: {total_stock}
- Valor total del inventario: ${valor_total:.2f}

PRODUCTOS POR CATEGORÍA:"""
        
        for categoria, productos in productos_por_categoria.items():
            reporte += f"\n- {categoria}: {len(productos)} productos"
        
        if stock_bajo:
            reporte += f"\n\nALERTAS - STOCK BAJO (≤10 unidades):"
            for id_prod, producto in stock_bajo.items():
                reporte += f"\n- {producto.nombre}: {producto.stock} unidades"
        
        return reporte
    
    def obtener_estadisticas_categoria(self, categoria):
        """Obtiene estadísticas de una categoría específica"""
        productos_categoria = self.filtrar_por_categoria(categoria)
        
        if not productos_categoria:
            return f"No hay productos en la categoría '{categoria}'"
        
        total_productos = len(productos_categoria)
        total_stock = sum(prod.stock for prod in productos_categoria.values())
        precio_promedio = sum(prod.precio for prod in productos_categoria.values()) / total_productos
        valor_total = sum(prod.precio * prod.stock for prod in productos_categoria.values())
        
        return f"""
=== ESTADÍSTICAS DE CATEGORÍA: {categoria.upper()} ===
- Total de productos: {total_productos}
- Total de unidades en stock: {total_stock}
- Precio promedio: ${precio_promedio:.2f}
- Valor total: ${valor_total:.2f}
"""
```

---

## 🎮 Interfaz de Usuario

### **Menú Principal**
```python
def mostrar_menu_principal():
    """Muestra el menú principal del sistema"""
    print("\n" + "="*50)
    print("        🏪 SISTEMA DE INVENTARIO 🏪")
    print("="*50)
    print("1. 📦 Gestionar Productos")
    print("2. 📊 Gestionar Stock")
    print("3. 🔍 Buscar Productos")
    print("4. 📈 Generar Reportes")
    print("5. ⚠️  Ver Alertas")
    print("6. 💾 Guardar/Cargar Datos")
    print("7. ❌ Salir")
    print("="*50)

def mostrar_menu_productos():
    """Muestra el menú de gestión de productos"""
    print("\n--- GESTIÓN DE PRODUCTOS ---")
    print("1. ➕ Agregar Producto")
    print("2. ✏️  Editar Producto")
    print("3. 🗑️  Eliminar Producto")
    print("4. 👁️  Ver Producto")
    print("5. 📋 Listar Todos los Productos")
    print("6. ↩️  Volver al Menú Principal")

def mostrar_menu_stock():
    """Muestra el menú de gestión de stock"""
    print("\n--- GESTIÓN DE STOCK ---")
    print("1. ➕ Registrar Entrada")
    print("2. ➖ Registrar Salida")
    print("3. 📊 Ver Historial de Movimientos")
    print("4. ⚠️  Ver Stock Bajo")
    print("5. ↩️  Volver al Menú Principal")
```

### **Funciones de Entrada de Datos**
```python
def obtener_datos_producto():
    """Obtiene los datos de un nuevo producto del usuario"""
    print("\n--- DATOS DEL NUEVO PRODUCTO ---")
    
    nombre = input("Nombre del producto: ").strip()
    if not nombre:
        return None
    
    categoria = input("Categoría: ").strip()
    if not categoria:
        return None
    
    try:
        precio = float(input("Precio: $"))
        if precio <= 0:
            print("❌ El precio debe ser mayor a 0")
            return None
    except ValueError:
        print("❌ Precio inválido")
        return None
    
    try:
        stock = int(input("Stock inicial: "))
        if stock < 0:
            print("❌ El stock no puede ser negativo")
            return None
    except ValueError:
        print("❌ Stock inválido")
        return None
    
    descripcion = input("Descripción (opcional): ").strip()
    
    return {
        "nombre": nombre,
        "categoria": categoria,
        "precio": precio,
        "stock": stock,
        "descripcion": descripcion
    }

def obtener_datos_movimiento():
    """Obtiene los datos de un movimiento de stock"""
    print("\n--- DATOS DEL MOVIMIENTO ---")
    
    try:
        id_producto = int(input("ID del producto: "))
    except ValueError:
        print("❌ ID inválido")
        return None
    
    tipo = input("Tipo (entrada/salida): ").strip().lower()
    if tipo not in ["entrada", "salida"]:
        print("❌ Tipo inválido")
        return None
    
    try:
        cantidad = int(input("Cantidad: "))
        if cantidad <= 0:
            print("❌ La cantidad debe ser mayor a 0")
            return None
    except ValueError:
        print("❌ Cantidad inválida")
        return None
    
    motivo = input("Motivo (opcional): ").strip()
    
    return {
        "id_producto": id_producto,
        "tipo": tipo,
        "cantidad": cantidad,
        "motivo": motivo
    }
```

---

## 🚀 Funcionalidades Avanzadas

### **Persistencia de Datos**
```python
def guardar_datos(self, archivo="inventario.json"):
    """Guarda el inventario en un archivo JSON"""
    try:
        datos = {
            "contador_id": self.contador_id,
            "productos": {str(id_prod): prod.to_dict() 
                         for id_prod, prod in self.productos.items()},
            "categorias": list(self.categorias)
        }
        
        with open(archivo, 'w', encoding='utf-8') as f:
            json.dump(datos, f, indent=2, ensure_ascii=False)
        
        return True, f"Datos guardados en '{archivo}'"
    except Exception as e:
        return False, f"Error al guardar: {str(e)}"

def cargar_datos(self, archivo="inventario.json"):
    """Carga el inventario desde un archivo JSON"""
    try:
        with open(archivo, 'r', encoding='utf-8') as f:
            datos = json.load(f)
        
        self.contador_id = datos["contador_id"]
        self.categorias = set(datos["categorias"])
        
        # Reconstruir productos
        self.productos = {}
        for id_str, prod_dict in datos["productos"].items():
            id_prod = int(id_str)
            producto = Producto(
                id_prod,
                prod_dict["nombre"],
                prod_dict["categoria"],
                prod_dict["precio"],
                prod_dict["stock"],
                prod_dict["descripcion"]
            )
            producto.fecha_creacion = datetime.fromisoformat(prod_dict["fecha_creacion"])
            producto.historial_movimientos = prod_dict["historial_movimientos"]
            self.productos[id_prod] = producto
        
        return True, f"Datos cargados desde '{archivo}'"
    except FileNotFoundError:
        return False, f"Archivo '{archivo}' no encontrado"
    except Exception as e:
        return False, f"Error al cargar: {str(e)}"
```

### **Exportación de Reportes**
```python
def exportar_reporte_csv(self, archivo="reporte_inventario.csv"):
    """Exporta el reporte de inventario a CSV"""
    try:
        import csv
        
        with open(archivo, 'w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            
            # Encabezados
            writer.writerow(['ID', 'Nombre', 'Categoría', 'Precio', 'Stock', 'Valor Total'])
            
            # Datos
            for id_prod, producto in self.productos.items():
                valor_total = producto.precio * producto.stock
                writer.writerow([
                    id_prod,
                    producto.nombre,
                    producto.categoria,
                    f"${producto.precio:.2f}",
                    producto.stock,
                    f"${valor_total:.2f}"
                ])
        
        return True, f"Reporte exportado a '{archivo}'"
    except Exception as e:
        return False, f"Error al exportar: {str(e)}"
```

---

## 🧪 Casos de Prueba

### **Prueba 1: Agregar Productos**
```python
# Crear sistema
sistema = SistemaInventario()

# Agregar productos de prueba
productos_prueba = [
    ("Laptop Gaming", "Electrónicos", 1299.99, 15, "Laptop para gaming de alto rendimiento"),
    ("Mouse Gaming", "Accesorios", 49.99, 50, "Mouse óptico para gaming"),
    ("Teclado Mecánico", "Accesorios", 89.99, 25, "Teclado mecánico con switches Cherry MX"),
    ("Monitor 4K", "Electrónicos", 399.99, 10, "Monitor 4K de 27 pulgadas"),
    ("Auriculares", "Accesorios", 79.99, 30, "Auriculares con cancelación de ruido")
]

for nombre, categoria, precio, stock, descripcion in productos_prueba:
    exito, mensaje = sistema.agregar_producto(nombre, categoria, precio, stock, descripcion)
    print(f"✅ {mensaje}")

print(f"\nTotal de productos: {len(sistema.productos)}")
print(f"Categorías: {', '.join(sistema.categorias)}")
```

### **Prueba 2: Operaciones de Stock**
```python
# Registrar movimientos de stock
movimientos_prueba = [
    (1, "entrada", 5, "Compra de proveedor"),
    (2, "salida", 10, "Venta a cliente"),
    (3, "entrada", 15, "Reposición de inventario"),
    (4, "salida", 3, "Venta online"),
    (5, "entrada", 20, "Compra mayorista")
]

for id_prod, tipo, cantidad, motivo in movimientos_prueba:
    exito, mensaje = sistema.registrar_movimiento(id_prod, tipo, cantidad, motivo)
    print(f"✅ {mensaje}")

# Ver stock bajo
stock_bajo = sistema.obtener_stock_bajo(20)
print(f"\n⚠️ Productos con stock bajo (≤20): {len(stock_bajo)}")
for id_prod, producto in stock_bajo.items():
    print(f"  - {producto.nombre}: {producto.stock} unidades")
```

### **Prueba 3: Búsqueda y Filtrado**
```python
# Búsqueda por nombre
resultados_nombre = sistema.buscar_producto("nombre", "gaming")
print(f"\n🔍 Productos con 'gaming' en el nombre: {len(resultados_nombre)}")
for id_prod, producto in resultados_nombre.items():
    print(f"  - {producto}")

# Filtrado por categoría
productos_electronicos = sistema.filtrar_por_categoria("Electrónicos")
print(f"\n📱 Productos de Electrónicos: {len(productos_electronicos)}")
for id_prod, producto in productos_electronicos.items():
    print(f"  - {producto}")

# Productos caros
productos_caros = sistema.obtener_productos_caros(100)
print(f"\n💰 Productos con precio ≥$100: {len(productos_caros)}")
for id_prod, producto in productos_caros.items():
    print(f"  - {producto}")
```

### **Prueba 4: Reportes**
```python
# Generar reporte general
reporte_general = sistema.generar_reporte_inventario()
print("\n" + reporte_general)

# Estadísticas por categoría
for categoria in sistema.categorias:
    estadisticas = sistema.obtener_estadisticas_categoria(categoria)
    print(estadisticas)

# Exportar a CSV
exito, mensaje = sistema.exportar_reporte_csv()
print(f"\n💾 {mensaje}")
```

---

## 📊 Evaluación del Proyecto

### **Criterios de Evaluación**
- [ ] **Funcionalidad Básica** (40%): CRUD de productos funciona correctamente
- [ ] **Gestión de Stock** (25%): Movimientos de entrada/salida funcionan
- [ ] **Búsqueda y Filtrado** (20%): Búsquedas y filtros funcionan correctamente
- [ ] **Reportes** (10%): Generación de reportes y estadísticas
- [ ] **Interfaz de Usuario** (5%): Menús claros y fáciles de usar

### **Puntuación Total**: ___/100

---

## 🚀 Mejoras Futuras

### **Funcionalidades Adicionales**
1. **Sistema de Usuarios**: Diferentes niveles de acceso
2. **Historial de Cambios**: Auditoría de modificaciones
3. **Notificaciones**: Alertas por email/SMS
4. **Interfaz Web**: Dashboard en navegador
5. **Base de Datos**: Persistencia en PostgreSQL/MySQL
6. **API REST**: Endpoints para integración con otros sistemas

### **Optimizaciones**
1. **Índices**: Mejorar búsquedas con índices
2. **Caché**: Implementar sistema de caché para consultas frecuentes
3. **Validaciones**: Validaciones más robustas de datos
4. **Logging**: Sistema de logs para debugging
5. **Testing**: Pruebas unitarias y de integración

---

## 📝 Reflexiones del Proyecto

**¿Qué aprendiste implementando este sistema de inventario?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué funcionalidad te resultó más difícil de implementar?**
```
[Escribe aquí las dificultades]
```

**¿Cómo podrías mejorar este sistema en el futuro?**
```
[Escribe aquí tus ideas de mejora]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [02-Proyecto-Analizador-Texto.md](./02-proyecto-analizador-texto.md)

**En el siguiente proyecto implementarás un analizador de texto que te permitirá practicar con diferentes estructuras de datos y algoritmos de procesamiento.**

---

**¡Excelente trabajo! Has implementado un sistema de inventario completo que demuestra tu dominio de diccionarios, sets y otras estructuras de datos en Python. Este proyecto te ha preparado para crear aplicaciones más complejas y reales.** 