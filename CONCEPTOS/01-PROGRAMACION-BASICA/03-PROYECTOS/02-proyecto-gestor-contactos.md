# 🚀 Proyecto: Gestor de Contactos

## 📋 Descripción del Proyecto
Crear un sistema de gestión de contactos que permita almacenar, buscar, modificar y eliminar información de contactos personales. Este proyecto te permitirá practicar con listas, diccionarios, funciones y estructuras de control.

## 🎯 Objetivos del Proyecto
- **Aplicar conceptos**: Usar variables, tipos de datos, operadores y estructuras de control
- **Manejar datos**: Trabajar con listas y diccionarios para almacenar información
- **Crear funciones**: Implementar funcionalidades modulares y reutilizables
- **Interfaz de usuario**: Desarrollar un menú interactivo y fácil de usar
- **Persistencia de datos**: Guardar y cargar contactos desde archivos

## ⏱️ Tiempo Estimado: 3-4 horas

---

## 🎮 Simulación del Mundo Real

### **Contexto del Proyecto**
Imagina que estás trabajando en una empresa de desarrollo de software y tu jefe te pide crear una aplicación de gestión de contactos para que los empleados puedan mantener organizada su información de contactos profesionales y personales.

### **Requisitos del Cliente**
- Debe permitir agregar, editar y eliminar contactos
- Debe incluir búsqueda por nombre, teléfono o email
- Debe mostrar todos los contactos de manera organizada
- Debe guardar la información en un archivo para persistencia
- Debe ser fácil de usar con un menú claro

---

## 🛠️ Implementación Paso a Paso

### **Paso 1: Estructura Básica del Programa**
Crea un archivo llamado `gestor_contactos.py` y comienza con esta estructura:

```python
# Gestor de Contactos
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

import json
import os

class Contacto:
    """Clase para representar un contacto"""
    def __init__(self, nombre, telefono, email, empresa=""):
        self.nombre = nombre
        self.telefono = telefono
        self.email = email
        self.empresa = empresa
    
    def to_dict(self):
        """Convierte el contacto a diccionario para guardar"""
        return {
            'nombre': self.nombre,
            'telefono': self.telefono,
            'email': self.email,
            'empresa': self.empresa
        }
    
    def __str__(self):
        """Representación en string del contacto"""
        return f"Nombre: {self.nombre} | Tel: {self.telefono} | Email: {self.email} | Empresa: {self.empresa}"

class GestorContactos:
    """Clase principal para gestionar contactos"""
    def __init__(self):
        self.contactos = []
        self.archivo_contactos = "contactos.json"
        self.cargar_contactos()
    
    def mostrar_menu(self):
        """Muestra el menú principal"""
        print("\n" + "="*50)
        print("        📱 GESTOR DE CONTACTOS 📱")
        print("="*50)
        print("1. ➕ Agregar contacto")
        print("2. 🔍 Buscar contacto")
        print("3. 📝 Editar contacto")
        print("4. ❌ Eliminar contacto")
        print("5. 📋 Mostrar todos los contactos")
        print("6. 💾 Guardar contactos")
        print("7. 📊 Estadísticas")
        print("8. ❌ Salir")
        print("="*50)

def main():
    """Función principal del programa"""
    gestor = GestorContactos()
    print("¡Bienvenido al Gestor de Contactos!")
    
    while True:
        gestor.mostrar_menu()
        opcion = input("\nSelecciona una opción (1-8): ")
        
        if opcion == "8":
            print("¡Gracias por usar el Gestor de Contactos! 👋")
            break
        
        # Aquí irá la lógica del menú
        print("Funcionalidad en desarrollo...")

if __name__ == "__main__":
    main()
```

### **Paso 2: Implementar Funcionalidades Básicas**
Agrega las funciones para gestionar contactos:

```python
def agregar_contacto(self):
    """Agrega un nuevo contacto"""
    print("\n➕ AGREGAR NUEVO CONTACTO")
    
    # Obtener información del contacto
    nombre = input("Nombre: ").strip()
    if not nombre:
        print("❌ El nombre es obligatorio")
        return
    
    telefono = input("Teléfono: ").strip()
    if not telefono:
        print("❌ El teléfono es obligatorio")
        return
    
    email = input("Email: ").strip()
    empresa = input("Empresa (opcional): ").strip()
    
    # Crear y agregar el contacto
    nuevo_contacto = Contacto(nombre, telefono, email, empresa)
    self.contactos.append(nuevo_contacto)
    
    print(f"✅ Contacto '{nombre}' agregado exitosamente")

def mostrar_contactos(self):
    """Muestra todos los contactos"""
    if not self.contactos:
        print("📭 No hay contactos guardados")
        return
    
    print(f"\n📋 LISTA DE CONTACTOS ({len(self.contactos)} contactos)")
    print("-" * 60)
    
    for i, contacto in enumerate(self.contactos, 1):
        print(f"{i:2d}. {contacto}")
    
    print("-" * 60)

def buscar_contacto(self):
    """Busca contactos por diferentes criterios"""
    if not self.contactos:
        print("📭 No hay contactos para buscar")
        return
    
    print("\n🔍 BUSCAR CONTACTO")
    print("1. Por nombre")
    print("2. Por teléfono")
    print("3. Por email")
    print("4. Por empresa")
    
    opcion = input("Selecciona criterio de búsqueda (1-4): ")
    termino = input("Ingresa el término de búsqueda: ").strip().lower()
    
    if not termino:
        print("❌ Debes ingresar un término de búsqueda")
        return
    
    # Realizar búsqueda
    resultados = []
    for contacto in self.contactos:
        if opcion == "1" and termino in contacto.nombre.lower():
            resultados.append(contacto)
        elif opcion == "2" and termino in contacto.telefono:
            resultados.append(contacto)
        elif opcion == "3" and termino in contacto.email.lower():
            resultados.append(contacto)
        elif opcion == "4" and termino in contacto.empresa.lower():
            resultados.append(contacto)
    
    # Mostrar resultados
    if resultados:
        print(f"\n🔍 RESULTADOS DE BÚSQUEDA ({len(resultados)} encontrados)")
        print("-" * 60)
        for i, contacto in enumerate(resultados, 1):
            print(f"{i:2d}. {contacto}")
    else:
        print("🔍 No se encontraron contactos con ese criterio")
```

### **Paso 3: Implementar Funcionalidades Avanzadas**
Agrega funciones para editar, eliminar y gestionar archivos:

```python
def editar_contacto(self):
    """Edita un contacto existente"""
    if not self.contactos:
        print("📭 No hay contactos para editar")
        return
    
    self.mostrar_contactos()
    
    try:
        indice = int(input("\nIngresa el número del contacto a editar: ")) - 1
        if 0 <= indice < len(self.contactos):
            contacto = self.contactos[indice]
            print(f"\n📝 EDITANDO CONTACTO: {contacto.nombre}")
            
            # Obtener nueva información
            nuevo_nombre = input(f"Nuevo nombre ({contacto.nombre}): ").strip()
            if nuevo_nombre:
                contacto.nombre = nuevo_nombre
            
            nuevo_telefono = input(f"Nuevo teléfono ({contacto.telefono}): ").strip()
            if nuevo_telefono:
                contacto.telefono = nuevo_telefono
            
            nuevo_email = input(f"Nuevo email ({contacto.email}): ").strip()
            if nuevo_email:
                contacto.email = nuevo_email
            
            nueva_empresa = input(f"Nueva empresa ({contacto.empresa}): ").strip()
            if nueva_empresa:
                contacto.empresa = nueva_empresa
            
            print("✅ Contacto actualizado exitosamente")
        else:
            print("❌ Número de contacto inválido")
    except ValueError:
        print("❌ Debes ingresar un número válido")

def eliminar_contacto(self):
    """Elimina un contacto"""
    if not self.contactos:
        print("📭 No hay contactos para eliminar")
        return
    
    self.mostrar_contactos()
    
    try:
        indice = int(input("\nIngresa el número del contacto a eliminar: ")) - 1
        if 0 <= indice < len(self.contactos):
            contacto = self.contactos.pop(indice)
            print(f"✅ Contacto '{contacto.nombre}' eliminado exitosamente")
        else:
            print("❌ Número de contacto inválido")
    except ValueError:
        print("❌ Debes ingresar un número válido")

def guardar_contactos(self):
    """Guarda los contactos en un archivo JSON"""
    try:
        with open(self.archivo_contactos, 'w', encoding='utf-8') as archivo:
            contactos_dict = [contacto.to_dict() for contacto in self.contactos]
            json.dump(contactos_dict, archivo, indent=2, ensure_ascii=False)
        print("✅ Contactos guardados exitosamente")
    except Exception as e:
        print(f"❌ Error al guardar contactos: {e}")

def cargar_contactos(self):
    """Carga los contactos desde un archivo JSON"""
    if os.path.exists(self.archivo_contactos):
        try:
            with open(self.archivo_contactos, 'r', encoding='utf-8') as archivo:
                contactos_dict = json.load(archivo)
                self.contactos = [Contacto(**contacto) for contacto in contactos_dict]
            print(f"📂 {len(self.contactos)} contactos cargados")
        except Exception as e:
            print(f"❌ Error al cargar contactos: {e}")

def estadisticas(self):
    """Muestra estadísticas de los contactos"""
    if not self.contactos:
        print("📭 No hay contactos para mostrar estadísticas")
        return
    
    print("\n📊 ESTADÍSTICAS DE CONTACTOS")
    print("-" * 40)
    
    total_contactos = len(self.contactos)
    print(f"Total de contactos: {total_contactos}")
    
    # Contactos con empresa
    con_empresa = sum(1 for c in self.contactos if c.empresa)
    print(f"Con empresa: {con_empresa}")
    print(f"Sin empresa: {total_contactos - con_empresa}")
    
    # Dominios de email más comunes
    dominios = {}
    for contacto in self.contactos:
        if '@' in contacto.email:
            dominio = contacto.email.split('@')[1]
            dominios[dominio] = dominios.get(dominio, 0) + 1
    
    if dominios:
        print("\nDominios de email más comunes:")
        for dominio, cantidad in sorted(dominios.items(), key=lambda x: x[1], reverse=True):
            print(f"  {dominio}: {cantidad} contactos")
```

### **Paso 4: Integrar Funcionalidades en el Menú Principal**
Modifica la función main para incluir todas las funcionalidades:

```python
def main():
    """Función principal del programa"""
    gestor = GestorContactos()
    print("¡Bienvenido al Gestor de Contactos!")
    
    while True:
        gestor.mostrar_menu()
        opcion = input("\nSelecciona una opción (1-8): ")
        
        if opcion == "1":
            gestor.agregar_contacto()
        elif opcion == "2":
            gestor.buscar_contacto()
        elif opcion == "3":
            gestor.editar_contacto()
        elif opcion == "4":
            gestor.eliminar_contacto()
        elif opcion == "5":
            gestor.mostrar_contactos()
        elif opcion == "6":
            gestor.guardar_contactos()
        elif opcion == "7":
            gestor.estadisticas()
        elif opcion == "8":
            print("💾 Guardando contactos antes de salir...")
            gestor.guardar_contactos()
            print("¡Gracias por usar el Gestor de Contactos! 👋")
            break
        else:
            print("❌ Opción no válida. Por favor selecciona 1-8")
```

---

## 🔧 Código Completo del Proyecto

```python
# Gestor de Contactos
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

import json
import os

class Contacto:
    """Clase para representar un contacto"""
    def __init__(self, nombre, telefono, email, empresa=""):
        self.nombre = nombre
        self.telefono = telefono
        self.email = email
        self.empresa = empresa
    
    def to_dict(self):
        """Convierte el contacto a diccionario para guardar"""
        return {
            'nombre': self.nombre,
            'telefono': self.telefono,
            'email': self.email,
            'empresa': self.empresa
        }
    
    def __str__(self):
        """Representación en string del contacto"""
        return f"Nombre: {self.nombre} | Tel: {self.telefono} | Email: {self.email} | Empresa: {self.empresa}"

class GestorContactos:
    """Clase principal para gestionar contactos"""
    def __init__(self):
        self.contactos = []
        self.archivo_contactos = "contactos.json"
        self.cargar_contactos()
    
    def mostrar_menu(self):
        """Muestra el menú principal"""
        print("\n" + "="*50)
        print("        📱 GESTOR DE CONTACTOS 📱")
        print("="*50)
        print("1. ➕ Agregar contacto")
        print("2. 🔍 Buscar contacto")
        print("3. 📝 Editar contacto")
        print("4. ❌ Eliminar contacto")
        print("5. 📋 Mostrar todos los contactos")
        print("6. 💾 Guardar contactos")
        print("7. 📊 Estadísticas")
        print("8. ❌ Salir")
        print("="*50)
    
    def agregar_contacto(self):
        """Agrega un nuevo contacto"""
        print("\n➕ AGREGAR NUEVO CONTACTO")
        
        # Obtener información del contacto
        nombre = input("Nombre: ").strip()
        if not nombre:
            print("❌ El nombre es obligatorio")
            return
        
        telefono = input("Teléfono: ").strip()
        if not telefono:
            print("❌ El teléfono es obligatorio")
            return
        
        email = input("Email: ").strip()
        empresa = input("Empresa (opcional): ").strip()
        
        # Crear y agregar el contacto
        nuevo_contacto = Contacto(nombre, telefono, email, empresa)
        self.contactos.append(nuevo_contacto)
        
        print(f"✅ Contacto '{nombre}' agregado exitosamente")
    
    def mostrar_contactos(self):
        """Muestra todos los contactos"""
        if not self.contactos:
            print("📭 No hay contactos guardados")
            return
        
        print(f"\n📋 LISTA DE CONTACTOS ({len(self.contactos)} contactos)")
        print("-" * 60)
        
        for i, contacto in enumerate(self.contactos, 1):
            print(f"{i:2d}. {contacto}")
        
        print("-" * 60)
    
    def buscar_contacto(self):
        """Busca contactos por diferentes criterios"""
        if not self.contactos:
            print("📭 No hay contactos para buscar")
            return
        
        print("\n🔍 BUSCAR CONTACTO")
        print("1. Por nombre")
        print("2. Por teléfono")
        print("3. Por email")
        print("4. Por empresa")
        
        opcion = input("Selecciona criterio de búsqueda (1-4): ")
        termino = input("Ingresa el término de búsqueda: ").strip().lower()
        
        if not termino:
            print("❌ Debes ingresar un término de búsqueda")
            return
        
        # Realizar búsqueda
        resultados = []
        for contacto in self.contactos:
            if opcion == "1" and termino in contacto.nombre.lower():
                resultados.append(contacto)
            elif opcion == "2" and termino in contacto.telefono:
                resultados.append(contacto)
            elif opcion == "3" and termino in contacto.email.lower():
                resultados.append(contacto)
            elif opcion == "4" and termino in contacto.empresa.lower():
                resultados.append(contacto)
        
        # Mostrar resultados
        if resultados:
            print(f"\n🔍 RESULTADOS DE BÚSQUEDA ({len(resultados)} encontrados)")
            print("-" * 60)
            for i, contacto in enumerate(resultados, 1):
                print(f"{i:2d}. {contacto}")
        else:
            print("🔍 No se encontraron contactos con ese criterio")
    
    def editar_contacto(self):
        """Edita un contacto existente"""
        if not self.contactos:
            print("📭 No hay contactos para editar")
            return
        
        self.mostrar_contactos()
        
        try:
            indice = int(input("\nIngresa el número del contacto a editar: ")) - 1
            if 0 <= indice < len(self.contactos):
                contacto = self.contactos[indice]
                print(f"\n📝 EDITANDO CONTACTO: {contacto.nombre}")
                
                # Obtener nueva información
                nuevo_nombre = input(f"Nuevo nombre ({contacto.nombre}): ").strip()
                if nuevo_nombre:
                    contacto.nombre = nuevo_nombre
                
                nuevo_telefono = input(f"Nuevo teléfono ({contacto.telefono}): ").strip()
                if nuevo_telefono:
                    contacto.telefono = nuevo_telefono
                
                nuevo_email = input(f"Nuevo email ({contacto.email}): ").strip()
                if nuevo_email:
                    contacto.email = nuevo_email
                
                nueva_empresa = input(f"Nueva empresa ({contacto.empresa}): ").strip()
                if nueva_empresa:
                    contacto.empresa = nueva_empresa
                
                print("✅ Contacto actualizado exitosamente")
            else:
                print("❌ Número de contacto inválido")
        except ValueError:
            print("❌ Debes ingresar un número válido")
    
    def eliminar_contacto(self):
        """Elimina un contacto"""
        if not self.contactos:
            print("📭 No hay contactos para eliminar")
            return
        
        self.mostrar_contactos()
        
        try:
            indice = int(input("\nIngresa el número del contacto a eliminar: ")) - 1
            if 0 <= indice < len(self.contactos):
                contacto = self.contactos.pop(indice)
                print(f"✅ Contacto '{contacto.nombre}' eliminado exitosamente")
            else:
                print("❌ Número de contacto inválido")
        except ValueError:
            print("❌ Debes ingresar un número válido")
    
    def guardar_contactos(self):
        """Guarda los contactos en un archivo JSON"""
        try:
            with open(self.archivo_contactos, 'w', encoding='utf-8') as archivo:
                contactos_dict = [contacto.to_dict() for contacto in self.contactos]
                json.dump(contactos_dict, archivo, indent=2, ensure_ascii=False)
            print("✅ Contactos guardados exitosamente")
        except Exception as e:
            print(f"❌ Error al guardar contactos: {e}")
    
    def cargar_contactos(self):
        """Carga los contactos desde un archivo JSON"""
        if os.path.exists(self.archivo_contactos):
            try:
                with open(self.archivo_contactos, 'r', encoding='utf-8') as archivo:
                    contactos_dict = json.load(archivo)
                    self.contactos = [Contacto(**contacto) for contacto in contactos_dict]
                print(f"📂 {len(self.contactos)} contactos cargados")
            except Exception as e:
                print(f"❌ Error al cargar contactos: {e}")
    
    def estadisticas(self):
        """Muestra estadísticas de los contactos"""
        if not self.contactos:
            print("📭 No hay contactos para mostrar estadísticas")
            return
        
        print("\n📊 ESTADÍSTICAS DE CONTACTOS")
        print("-" * 40)
        
        total_contactos = len(self.contactos)
        print(f"Total de contactos: {total_contactos}")
        
        # Contactos con empresa
        con_empresa = sum(1 for c in self.contactos if c.empresa)
        print(f"Con empresa: {con_empresa}")
        print(f"Sin empresa: {total_contactos - con_empresa}")
        
        # Dominios de email más comunes
        dominios = {}
        for contacto in self.contactos:
            if '@' in contacto.email:
                dominio = contacto.email.split('@')[1]
                dominios[dominio] = dominios.get(dominio, 0) + 1
        
        if dominios:
            print("\nDominios de email más comunes:")
            for dominio, cantidad in sorted(dominios.items(), key=lambda x: x[1], reverse=True):
                print(f"  {dominio}: {cantidad} contactos")

def main():
    """Función principal del programa"""
    gestor = GestorContactos()
    print("¡Bienvenido al Gestor de Contactos!")
    
    while True:
        gestor.mostrar_menu()
        opcion = input("\nSelecciona una opción (1-8): ")
        
        if opcion == "1":
            gestor.agregar_contacto()
        elif opcion == "2":
            gestor.buscar_contacto()
        elif opcion == "3":
            gestor.editar_contacto()
        elif opcion == "4":
            gestor.eliminar_contacto()
        elif opcion == "5":
            gestor.mostrar_contactos()
        elif opcion == "6":
            gestor.guardar_contactos()
        elif opcion == "7":
            gestor.estadisticas()
        elif opcion == "8":
            print("💾 Guardando contactos antes de salir...")
            gestor.guardar_contactos()
            print("¡Gracias por usar el Gestor de Contactos! 👋")
            break
        else:
            print("❌ Opción no válida. Por favor selecciona 1-8")

if __name__ == "__main__":
    main()
```

---

## 🧪 Pruebas del Proyecto

### **Casos de Prueba Básicos**
1. **Agregar contacto**: Crear un nuevo contacto con información completa
2. **Mostrar contactos**: Ver la lista de todos los contactos
3. **Buscar contacto**: Encontrar contactos por diferentes criterios
4. **Editar contacto**: Modificar información de un contacto existente

### **Casos de Prueba Avanzados**
1. **Eliminar contacto**: Remover un contacto de la lista
2. **Guardar contactos**: Persistir datos en archivo JSON
3. **Cargar contactos**: Recuperar datos del archivo al reiniciar
4. **Estadísticas**: Ver resumen de información de contactos

### **Casos de Prueba de Error**
1. **Entrada inválida**: Manejar entradas incorrectas del usuario
2. **Archivo corrupto**: Recuperarse de archivos JSON malformados
3. **Campos vacíos**: Validar información obligatoria
4. **Índices inválidos**: Manejar selecciones fuera de rango

---

## 🎨 Mejoras y Extensiones

### **Nivel 1: Funcionalidades Básicas**
- Validación de formato de email
- Validación de formato de teléfono
- Categorización de contactos
- Fotos de perfil (URLs)

### **Nivel 2: Funcionalidades Avanzadas**
- Búsqueda avanzada con filtros múltiples
- Exportar contactos a CSV/Excel
- Importar contactos desde archivos
- Backup automático

### **Nivel 3: Interfaz Gráfica**
- Interfaz con tkinter
- Gráficos de estadísticas
- Drag & drop para fotos
- Temas personalizables

---

## 📊 Criterios de Evaluación

### **Funcionalidad (40 puntos)**
- [ ] Agregar contactos funciona correctamente
- [ ] Buscar contactos funciona correctamente
- [ ] Editar contactos funciona correctamente
- [ ] Eliminar contactos funciona correctamente
- [ ] Guardar/cargar contactos funciona correctamente
- [ ] Estadísticas funcionan correctamente

### **Código (30 puntos)**
- [ ] Código está bien estructurado con clases
- [ ] Funciones tienen nombres descriptivos
- [ ] Manejo de errores implementado
- [ ] Comentarios explicativos presentes

### **Interfaz (20 puntos)**
- [ ] Menú claro y fácil de usar
- [ ] Mensajes informativos y claros
- [ ] Formato de salida consistente
- [ ] Emojis y formato atractivo

### **Extras (10 puntos)**
- [ ] Funcionalidades adicionales implementadas
- [ ] Código optimizado y eficiente

**Puntuación Total**: ___/100

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Sistema de Etiquetas**
Implementa un sistema de etiquetas para categorizar contactos (familia, trabajo, amigos, etc.).

### **Desafío 2: Historial de Cambios**
Agrega un sistema que registre todos los cambios realizados en los contactos.

### **Desafío 3: Sincronización en la Nube**
Implementa sincronización de contactos con servicios en la nube como Google Contacts.

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

**Archivo siguiente**: [03-Proyecto-Juego-Adivina-Numero.md](./03-proyecto-juego-adivina-numero.md)

**En el siguiente proyecto crearás un juego interactivo que te permitirá practicar con bucles y estructuras de control.**

---

## 🏆 Logros del Proyecto

**¡Felicitaciones! Has completado tu gestor de contactos funcional. Este proyecto demuestra que:**

- ✅ Puedes trabajar con clases y objetos en Python
- ✅ Sabes manejar listas y diccionarios
- ✅ Puedes implementar persistencia de datos
- ✅ Sabes crear interfaces de usuario interactivas
- ✅ Puedes manejar errores y validaciones
- ✅ Estás listo para proyectos más complejos

**¡Continúa con el siguiente proyecto para seguir desarrollando tus habilidades!** 