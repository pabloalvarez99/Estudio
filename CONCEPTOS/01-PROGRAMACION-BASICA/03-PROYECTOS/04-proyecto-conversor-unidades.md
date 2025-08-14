# 🚀 Proyecto: Conversor de Unidades

## 📋 Descripción del Proyecto
Crear un sistema completo de conversión de unidades que permita convertir entre diferentes sistemas de medida: longitud, peso, temperatura, tiempo, área, volumen y más. El proyecto incluirá una interfaz intuitiva y validación de datos.

## 🎯 Objetivos del Proyecto
- **Aplicar conceptos**: Usar variables, operadores, estructuras de control y funciones
- **Implementar conversiones**: Crear algoritmos de conversión precisos
- **Manejar datos**: Validar entradas y manejar diferentes tipos de unidades
- **Crear interfaz**: Desarrollar un sistema de menús claro y funcional
- **Organizar código**: Estructurar el proyecto de manera modular y mantenible

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 🎮 Simulación del Mundo Real

### **Contexto del Proyecto**
Imagina que estás trabajando en una empresa de software que desarrolla aplicaciones para ingenieros y científicos. Tu jefe te pide crear un conversor de unidades que sea preciso, fácil de usar y que cubra las conversiones más comunes.

### **Requisitos del Cliente**
- Debe convertir entre múltiples sistemas de unidades
- Debe ser preciso en los cálculos
- Debe tener una interfaz clara y fácil de usar
- Debe incluir las unidades más utilizadas en cada categoría
- Debe validar las entradas del usuario

---

## 🛠️ Implementación Paso a Paso

### **Paso 1: Estructura Básica del Conversor**
Crea un archivo llamado `conversor_unidades.py` y comienza con esta estructura:

```python
# Conversor de Unidades
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

import math

class ConversorUnidades:
    def __init__(self):
        self.categorias = {
            "1": "Longitud",
            "2": "Peso/Masa",
            "3": "Temperatura",
            "4": "Tiempo",
            "5": "Área",
            "6": "Volumen",
            "7": "Velocidad",
            "8": "Energía"
        }
        
        # Definir unidades para cada categoría
        self.unidades = {
            "Longitud": {
                "metros": 1.0,
                "kilómetros": 1000.0,
                "centímetros": 0.01,
                "milímetros": 0.001,
                "millas": 1609.344,
                "yardas": 0.9144,
                "pies": 0.3048,
                "pulgadas": 0.0254
            },
            "Peso/Masa": {
                "kilogramos": 1.0,
                "gramos": 0.001,
                "miligramos": 0.000001,
                "toneladas": 1000.0,
                "libras": 0.45359237,
                "onzas": 0.028349523125
            },
            "Temperatura": {
                "celsius": "celsius",
                "fahrenheit": "fahrenheit",
                "kelvin": "kelvin"
            },
            "Tiempo": {
                "segundos": 1.0,
                "minutos": 60.0,
                "horas": 3600.0,
                "días": 86400.0,
                "semanas": 604800.0,
                "años": 31536000.0
            },
            "Área": {
                "metros_cuadrados": 1.0,
                "kilómetros_cuadrados": 1000000.0,
                "centímetros_cuadrados": 0.0001,
                "millas_cuadradas": 2589988.110336,
                "acres": 4046.8564224,
                "hectáreas": 10000.0
            },
            "Volumen": {
                "metros_cúbicos": 1.0,
                "litros": 0.001,
                "mililitros": 0.000001,
                "galones": 0.003785411784,
                "pies_cúbicos": 0.028316846592
            },
            "Velocidad": {
                "metros_por_segundo": 1.0,
                "kilómetros_por_hora": 0.2777777778,
                "millas_por_hora": 0.44704,
                "nudos": 0.5144444444,
                "pies_por_segundo": 0.3048
            },
            "Energía": {
                "julios": 1.0,
                "kilojulios": 1000.0,
                "calorías": 4.184,
                "kilocalorías": 4184.0,
                "kilovatios_hora": 3600000.0,
                "electronvoltios": 1.602176634e-19
            }
        }
    
    def mostrar_menu_principal(self):
        """Muestra el menú principal del conversor"""
        print("\n" + "="*60)
        print("        🔄 CONVERSOR DE UNIDADES 🔄")
        print("="*60)
        
        for codigo, categoria in self.categorias.items():
            print(f"{codigo}. {categoria}")
        
        print("9. 📊 Ver historial de conversiones")
        print("0. ❌ Salir")
        print("="*60)
    
    def seleccionar_categoria(self):
        """Permite al usuario seleccionar una categoría"""
        while True:
            try:
                opcion = input("\nSelecciona una categoría (0-9): ").strip()
                
                if opcion == "0":
                    return None
                elif opcion == "9":
                    self.mostrar_historial()
                    continue
                elif opcion in self.categorias:
                    return self.categorias[opcion]
                else:
                    print("❌ Opción no válida. Por favor selecciona 0-9")
            except KeyboardInterrupt:
                print("\n\n👋 ¡Hasta luego!")
                return None
    
    def mostrar_unidades_categoria(self, categoria):
        """Muestra las unidades disponibles para una categoría"""
        print(f"\n--- UNIDADES DE {categoria.upper()} ---")
        
        unidades = list(self.unidades[categoria].keys())
        for i, unidad in enumerate(unidades, 1):
            print(f"{i}. {unidad}")
        
        return unidades
    
    def seleccionar_unidades(self, categoria):
        """Permite al usuario seleccionar unidades de origen y destino"""
        unidades = self.mostrar_unidades_categoria(categoria)
        
        try:
            # Seleccionar unidad de origen
            print(f"\nSelecciona la unidad de ORIGEN (1-{len(unidades)}):")
            origen_idx = int(input("Unidad de origen: ")) - 1
            if origen_idx < 0 or origen_idx >= len(unidades):
                print("❌ Índice de unidad de origen inválido")
                return None, None
            
            unidad_origen = unidades[origen_idx]
            
            # Seleccionar unidad de destino
            print(f"\nSelecciona la unidad de DESTINO (1-{len(unidades)}):")
            destino_idx = int(input("Unidad de destino: ")) - 1
            if destino_idx < 0 or destino_idx >= len(unidades):
                print("❌ Índice de unidad de destino inválido")
                return None, None
            
            unidad_destino = unidades[destino_idx]
            
            if unidad_origen == unidad_destino:
                print("❌ No puedes convertir a la misma unidad")
                return None, None
            
            return unidad_origen, unidad_destino
            
        except ValueError:
            print("❌ Por favor ingresa números válidos")
            return None, None
    
    def obtener_valor(self):
        """Obtiene el valor a convertir del usuario"""
        while True:
            try:
                valor_str = input("\nIngresa el valor a convertir: ").strip()
                
                if not valor_str:
                    print("❌ Debes ingresar un valor")
                    continue
                
                # Permitir notación científica
                if 'e' in valor_str.lower():
                    valor = float(valor_str)
                else:
                    valor = float(valor_str)
                
                if math.isnan(valor) or math.isinf(valor):
                    print("❌ El valor no es válido")
                    continue
                
                return valor
                
            except ValueError:
                print("❌ Por favor ingresa un número válido")
            except KeyboardInterrupt:
                return None
    
    def convertir_longitud(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de longitud"""
        # Convertir a metros primero
        metros = valor * self.unidades["Longitud"][unidad_origen]
        # Convertir de metros a unidad destino
        resultado = metros / self.unidades["Longitud"][unidad_destino]
        return resultado
    
    def convertir_peso(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de peso/masa"""
        # Convertir a kilogramos primero
        kilogramos = valor * self.unidades["Peso/Masa"][unidad_origen]
        # Convertir de kilogramos a unidad destino
        resultado = kilogramos / self.unidades["Peso/Masa"][unidad_destino]
        return resultado
    
    def convertir_temperatura(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de temperatura"""
        # Convertir a Celsius primero
        if unidad_origen == "celsius":
            celsius = valor
        elif unidad_origen == "fahrenheit":
            celsius = (valor - 32) * 5/9
        elif unidad_origen == "kelvin":
            celsius = valor - 273.15
        
        # Convertir de Celsius a unidad destino
        if unidad_destino == "celsius":
            return celsius
        elif unidad_destino == "fahrenheit":
            return celsius * 9/5 + 32
        elif unidad_destino == "kelvin":
            return celsius + 273.15
    
    def convertir_tiempo(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de tiempo"""
        # Convertir a segundos primero
        segundos = valor * self.unidades["Tiempo"][unidad_origen]
        # Convertir de segundos a unidad destino
        resultado = segundos / self.unidades["Tiempo"][unidad_destino]
        return resultado
    
    def convertir_area(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de área"""
        # Convertir a metros cuadrados primero
        metros_cuadrados = valor * self.unidades["Área"][unidad_origen]
        # Convertir de metros cuadrados a unidad destino
        resultado = metros_cuadrados / self.unidades["Área"][unidad_destino]
        return resultado
    
    def convertir_volumen(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de volumen"""
        # Convertir a metros cúbicos primero
        metros_cubicos = valor * self.unidades["Volumen"][unidad_origen]
        # Convertir de metros cúbicos a unidad destino
        resultado = metros_cubicos / self.unidades["Volumen"][unidad_destino]
        return resultado
    
    def convertir_velocidad(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de velocidad"""
        # Convertir a metros por segundo primero
        mps = valor * self.unidades["Velocidad"][unidad_origen]
        # Convertir de metros por segundo a unidad destino
        resultado = mps / self.unidades["Velocidad"][unidad_destino]
        return resultado
    
    def convertir_energia(self, valor, unidad_origen, unidad_destino):
        """Convierte unidades de energía"""
        # Convertir a julios primero
        julios = valor * self.unidades["Energía"][unidad_origen]
        # Convertir de julios a unidad destino
        resultado = julios / self.unidades["Energía"][unidad_destino]
        return resultado
    
    def realizar_conversion(self, categoria, unidad_origen, unidad_destino, valor):
        """Realiza la conversión según la categoría"""
        try:
            if categoria == "Longitud":
                resultado = self.convertir_longitud(valor, unidad_origen, unidad_destino)
            elif categoria == "Peso/Masa":
                resultado = self.convertir_peso(valor, unidad_origen, unidad_destino)
            elif categoria == "Temperatura":
                resultado = self.convertir_temperatura(valor, unidad_origen, unidad_destino)
            elif categoria == "Tiempo":
                resultado = self.convertir_tiempo(valor, unidad_origen, unidad_destino)
            elif categoria == "Área":
                resultado = self.convertir_area(valor, unidad_origen, unidad_destino)
            elif categoria == "Volumen":
                resultado = self.convertir_volumen(valor, unidad_origen, unidad_destino)
            elif categoria == "Velocidad":
                resultado = self.convertir_velocidad(valor, unidad_origen, unidad_destino)
            elif categoria == "Energía":
                resultado = self.convertir_energia(valor, unidad_origen, unidad_destino)
            else:
                return None
            
            return resultado
            
        except Exception as e:
            print(f"❌ Error en la conversión: {e}")
            return None
    
    def mostrar_resultado(self, valor, unidad_origen, unidad_destino, resultado, categoria):
        """Muestra el resultado de la conversión"""
        print("\n" + "🔄" * 30)
        print("🔄 RESULTADO DE LA CONVERSIÓN 🔄")
        print("🔄" * 30)
        
        print(f"📊 Categoría: {categoria}")
        print(f"📥 Valor de entrada: {valor} {unidad_origen}")
        print(f"📤 Resultado: {resultado:.6f} {unidad_destino}")
        
        # Formatear resultado según la magnitud
        if abs(resultado) >= 1000000:
            print(f"📊 Resultado en notación científica: {resultado:.2e} {unidad_destino}")
        elif abs(resultado) < 0.000001 and resultado != 0:
            print(f"📊 Resultado en notación científica: {resultado:.2e} {unidad_destino}")
        
        print("🔄" * 30)
        
        # Guardar en historial
        self.guardar_conversion(valor, unidad_origen, unidad_destino, resultado, categoria)
    
    def guardar_conversion(self, valor, unidad_origen, unidad_destino, resultado, categoria):
        """Guarda la conversión en el historial"""
        # Esta función se puede expandir para guardar en archivo
        pass
    
    def mostrar_historial(self):
        """Muestra el historial de conversiones"""
        print("\n--- 📊 HISTORIAL DE CONVERSIONES ---")
        print("🔄 Funcionalidad en desarrollo...")
        print("📝 Las conversiones se mostrarán aquí en futuras versiones")
    
    def ejecutar(self):
        """Función principal que ejecuta el conversor"""
        print("🔄 ¡Bienvenido al Conversor de Unidades!")
        print("📊 Convierte entre diferentes sistemas de unidades de manera precisa")
        
        while True:
            self.mostrar_menu_principal()
            
            # Seleccionar categoría
            categoria = self.seleccionar_categoria()
            if categoria is None:
                break
            
            print(f"\n🎯 Categoría seleccionada: {categoria}")
            
            # Seleccionar unidades
            unidades = self.seleccionar_unidades(categoria)
            if unidades[0] is None:
                continue
            
            unidad_origen, unidad_destino = unidades
            
            # Obtener valor
            valor = self.obtener_valor()
            if valor is None:
                continue
            
            # Realizar conversión
            resultado = self.realizar_conversion(categoria, unidad_origen, unidad_destino, valor)
            
            if resultado is not None:
                self.mostrar_resultado(valor, unidad_origen, unidad_destino, resultado, categoria)
                
                # Preguntar si quiere hacer otra conversión
                otra_conversion = input("\n¿Quieres hacer otra conversión? (sí/no): ").lower().strip()
                if otra_conversion not in ['sí', 'si', 's', 'y', 'yes']:
                    break
            else:
                print("❌ No se pudo realizar la conversión")
                break
        
        print("\n👋 ¡Gracias por usar el Conversor de Unidades!")

def main():
    """Función principal del programa"""
    conversor = ConversorUnidades()
    conversor.ejecutar()

if __name__ == "__main__":
    main()
```

---

## 🔧 Código Completo del Proyecto

El código completo está en la sección anterior. Este proyecto incluye:

### **Características Principales**
- ✅ **8 categorías** de conversión diferentes
- ✅ **Múltiples unidades** por categoría
- ✅ **Conversiones precisas** usando factores de conversión
- ✅ **Validación robusta** de entradas del usuario
- ✅ **Interfaz clara** con menús organizados
- ✅ **Manejo de errores** completo
- ✅ **Notación científica** para valores muy grandes o pequeños
- ✅ **Estructura modular** y fácil de mantener

---

## 🧪 Pruebas del Proyecto

### **Casos de Prueba Básicos**
1. **Conversión de longitud**: metros a pies, kilómetros a millas
2. **Conversión de peso**: kilogramos a libras, gramos a onzas
3. **Conversión de temperatura**: Celsius a Fahrenheit, Kelvin a Celsius
4. **Conversión de tiempo**: horas a minutos, días a segundos

### **Casos de Prueba Avanzados**
1. **Conversiones complejas**: área, volumen, velocidad, energía
2. **Valores extremos**: números muy grandes o muy pequeños
3. **Notación científica**: manejo de valores en formato científico
4. **Validación de entrada**: letras, números negativos, valores vacíos

### **Casos de Prueba de Error**
1. **Entrada inválida**: manejo de caracteres no numéricos
2. **Valores fuera de rango**: números muy extremos
3. **Mismas unidades**: conversión a la misma unidad
4. **Interrupciones**: manejo de Ctrl+C

---

## 🎨 Mejoras y Extensiones

### **Nivel 1: Funcionalidades Básicas**
- Historial de conversiones persistente
- Conversiones inversas automáticas
- Redondeo configurable de resultados
- Exportar resultados a archivo

### **Nivel 2: Funcionalidades Avanzadas**
- Conversiones personalizadas
- Gráficos de comparación de unidades
- Calculadora de costos por unidad
- Modo batch para múltiples conversiones

### **Nivel 3: Interfaz Gráfica**
- Interfaz con tkinter o PyQt
- Gráficos interactivos
- Base de datos de conversiones
- API para conversiones online

---

## 📊 Criterios de Evaluación

### **Funcionalidad (40 puntos)**
- [ ] Conversiones funcionan correctamente (20 puntos)
- [ ] Múltiples categorías implementadas (10 puntos)
- [ ] Validación de entrada robusta (10 puntos)

### **Código (30 puntos)**
- [ ] Código bien estructurado con clases (10 puntos)
- [ ] Funciones modulares y reutilizables (10 puntos)
- [ ] Manejo de errores implementado (10 puntos)

### **Interfaz (20 puntos)**
- [ ] Menú claro y fácil de usar (10 puntos)
- [ ] Mensajes informativos y claros (5 puntos)
- [ ] Formato de salida consistente (5 puntos)

### **Extras (10 puntos)**
- [ ] Funcionalidades adicionales implementadas (5 puntos)
- [ ] Código optimizado y eficiente (5 puntos)

**Puntuación Total**: ___/100

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Conversiones Personalizadas**
Implementa un sistema que permita al usuario crear sus propias conversiones personalizadas.

### **Desafío 2: Calculadora de Costos**
Crea un sistema que calcule costos basados en diferentes unidades (precio por metro, por kilogramo, etc.).

### **Desafío 3: Conversor de Monedas**
Extiende el proyecto para incluir conversión de monedas con tasas de cambio en tiempo real.

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

## 🎯 Próximo Paso

**Archivo siguiente**: [01-Checklist-Conceptos.md](../04-EVALUACION/01-checklist-conceptos.md)

**En la siguiente lección evaluarás tu progreso y completarás la autoevaluación del concepto.**

---

## 🏆 Logros del Proyecto

**¡Felicitaciones! Has completado tu Conversor de Unidades funcional. Este proyecto demuestra que:**

- ✅ Puedes crear aplicaciones complejas y precisas
- ✅ Sabes implementar algoritmos de conversión
- ✅ Puedes manejar múltiples tipos de datos y validaciones
- ✅ Sabes estructurar código de manera modular
- ✅ Puedes crear interfaces de usuario funcionales
- ✅ Estás listo para proyectos más avanzados

**¡Continúa con la evaluación para medir tu progreso!** 