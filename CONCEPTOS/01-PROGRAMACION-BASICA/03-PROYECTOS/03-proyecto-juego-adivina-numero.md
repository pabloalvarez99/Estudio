# 🚀 Proyecto: Juego "Adivina el Número"

## 📋 Descripción del Proyecto
Crear un juego interactivo de adivinanza de números que permita al usuario intentar adivinar un número secreto generado aleatoriamente. El juego incluirá diferentes niveles de dificultad, sistema de puntuación y estadísticas de juego.

## 🎯 Objetivos del Proyecto
- **Aplicar conceptos**: Usar variables, tipos de datos, operadores y estructuras de control
- **Implementar lógica de juego**: Crear un sistema de juego completo y funcional
- **Manejar entrada del usuario**: Validar y procesar entradas del usuario
- **Crear interfaz interactiva**: Desarrollar una experiencia de usuario atractiva
- **Implementar sistema de puntuación**: Crear un sistema de recompensas y seguimiento

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 🎮 Simulación del Mundo Real

### **Contexto del Proyecto**
Imagina que estás trabajando en una empresa de desarrollo de juegos móviles. Tu jefe te pide crear un prototipo de un juego simple pero adictivo que pueda ser la base para futuros juegos más complejos.

### **Requisitos del Cliente**
- Debe ser fácil de jugar pero desafiante
- Debe incluir diferentes niveles de dificultad
- Debe mantener estadísticas del jugador
- Debe ser adictivo y rejugable
- Debe tener una interfaz clara y atractiva

---

## 🛠️ Implementación Paso a Paso

### **Paso 1: Estructura Básica del Juego**
Crea un archivo llamado `juego_adivina_numero.py` y comienza con esta estructura:

```python
# Juego "Adivina el Número"
# Autor: [Tu nombre]
# Fecha: [Fecha actual]
# Versión: 1.0

import random
import time
import json
import os

class JuegoAdivinaNumero:
    def __init__(self):
        self.numero_secreto = 0
        self.intentos_maximos = 10
        self.intentos_usados = 0
        self.nivel_dificultad = "normal"
        self.puntuacion_actual = 0
        self.mejor_puntuacion = 0
        self.estadisticas = {
            "partidas_jugadas": 0,
            "partidas_ganadas": 0,
            "mejor_racha": 0,
            "racha_actual": 0,
            "total_intentos": 0
        }
        self.archivo_estadisticas = "estadisticas_juego.json"
        self.cargar_estadisticas()
    
    def mostrar_menu_principal(self):
        """Muestra el menú principal del juego"""
        print("\n" + "="*50)
        print("        🎯 ADIVINA EL NÚMERO 🎯")
        print("="*50)
        print("1. 🎮 Jugar")
        print("2. ⚙️  Configurar dificultad")
        print("3. 📊 Ver estadísticas")
        print("4. 🏆 Ver mejores puntuaciones")
        print("5. ❌ Salir")
        print("="*50)
    
    def configurar_dificultad(self):
        """Permite al usuario configurar la dificultad del juego"""
        print("\n--- CONFIGURAR DIFICULTAD ---")
        print("1. 🟢 Fácil (1-50, 15 intentos)")
        print("2. 🟡 Normal (1-100, 10 intentos)")
        print("3. 🔴 Difícil (1-200, 7 intentos)")
        print("4. ⚫ Extremo (1-500, 5 intentos)")
        
        opcion = input("\nSelecciona dificultad (1-4): ").strip()
        
        if opcion == "1":
            self.nivel_dificultad = "facil"
            self.intentos_maximos = 15
            print("✅ Dificultad configurada: FÁCIL")
        elif opcion == "2":
            self.nivel_dificultad = "normal"
            self.intentos_maximos = 10
            print("✅ Dificultad configurada: NORMAL")
        elif opcion == "3":
            self.nivel_dificultad = "dificil"
            self.intentos_maximos = 7
            print("✅ Dificultad configurada: DIFÍCIL")
        elif opcion == "4":
            self.nivel_dificultad = "extremo"
            self.intentos_maximos = 5
            print("✅ Dificultad configurada: EXTREMO")
        else:
            print("❌ Opción no válida. Manteniendo dificultad actual.")
    
    def generar_numero_secreto(self):
        """Genera un número secreto según la dificultad"""
        if self.nivel_dificultad == "facil":
            self.numero_secreto = random.randint(1, 50)
        elif self.nivel_dificultad == "normal":
            self.numero_secreto = random.randint(1, 100)
        elif self.nivel_dificultad == "dificil":
            self.numero_secreto = random.randint(1, 200)
        elif self.nivel_dificultad == "extremo":
            self.numero_secreto = random.randint(1, 500)
        
        print(f"\n🎲 Número secreto generado (1-{self.numero_secreto if self.numero_secreto < 100 else '...'})")
        print(f"🎯 Tienes {self.intentos_maximos} intentos para adivinarlo")
    
    def obtener_rango_dificultad(self):
        """Retorna el rango de números según la dificultad"""
        if self.nivel_dificultad == "facil":
            return 50
        elif self.nivel_dificultad == "normal":
            return 100
        elif self.nivel_dificultad == "dificil":
            return 200
        elif self.nivel_dificultad == "extremo":
            return 500
        return 100
    
    def calcular_puntuacion(self):
        """Calcula la puntuación basada en intentos y dificultad"""
        rango = self.obtener_rango_dificultad()
        base = 1000
        
        # Multiplicador por dificultad
        multiplicadores = {"facil": 1, "normal": 1.5, "dificil": 2, "extremo": 3}
        multiplicador = multiplicadores.get(self.nivel_dificultad, 1)
        
        # Penalización por intentos
        penalizacion = (self.intentos_usados - 1) * 50
        
        # Bonus por adivinar rápido
        bonus_rapidez = max(0, (self.intentos_maximos - self.intentos_usados) * 100)
        
        puntuacion = int((base * multiplicador) - penalizacion + bonus_rapidez)
        return max(100, puntuacion)  # Mínimo 100 puntos
    
    def mostrar_estadisticas(self):
        """Muestra las estadísticas del jugador"""
        print("\n--- 📊 ESTADÍSTICAS DEL JUGADOR ---")
        print(f"🎮 Partidas jugadas: {self.estadisticas['partidas_jugadas']}")
        print(f"🏆 Partidas ganadas: {self.estadisticas['partidas_ganadas']}")
        
        if self.estadisticas['partidas_jugadas'] > 0:
            porcentaje_victoria = (self.estadisticas['partidas_ganadas'] / self.estadisticas['partidas_jugadas']) * 100
            print(f"📈 Porcentaje de victoria: {porcentaje_victoria:.1f}%")
        
        print(f"🔥 Mejor racha: {self.estadisticas['mejor_racha']}")
        print(f"🔥 Racha actual: {self.estadisticas['racha_actual']}")
        print(f"🎯 Total de intentos: {self.estadisticas['total_intentos']}")
        
        if self.estadisticas['partidas_jugadas'] > 0:
            promedio_intentos = self.estadisticas['total_intentos'] / self.estadisticas['partidas_jugadas']
            print(f"📊 Promedio de intentos por partida: {promedio_intentos:.1f}")
        
        print(f"🏆 Mejor puntuación: {self.mejor_puntuacion}")
    
    def jugar(self):
        """Función principal del juego"""
        print(f"\n🎮 INICIANDO NUEVA PARTIDA - DIFICULTAD: {self.nivel_dificultad.upper()}")
        
        # Reiniciar variables de la partida
        self.intentos_usados = 0
        self.puntuacion_actual = 0
        
        # Generar número secreto
        self.generar_numero_secreto()
        
        # Bucle principal del juego
        while self.intentos_usados < self.intentos_maximos:
            self.intentos_usados += 1
            intentos_restantes = self.intentos_maximos - self.intentos_usados
            
            print(f"\n🔄 Intento {self.intentos_usados} de {self.intentos_maximos}")
            if intentos_restantes > 0:
                print(f"⏰ Intentos restantes: {intentos_restantes}")
            
            # Obtener entrada del usuario
            try:
                adivinanza = input("🎯 Ingresa tu número: ").strip()
                
                # Validar entrada
                if not adivinanza:
                    print("❌ Debes ingresar un número")
                    self.intentos_usados -= 1
                    continue
                
                numero_usuario = int(adivinanza)
                
                # Verificar rango
                rango_max = self.obtener_rango_dificultad()
                if numero_usuario < 1 or numero_usuario > rango_max:
                    print(f"❌ El número debe estar entre 1 y {rango_max}")
                    self.intentos_usados -= 1
                    continue
                
            except ValueError:
                print("❌ Por favor ingresa un número válido")
                self.intentos_usados -= 1
                continue
            
            # Verificar si adivinó
            if numero_usuario == self.numero_secreto:
                self.puntuacion_actual = self.calcular_puntuacion()
                self.ganar_partida()
                return True
            
            # Dar pistas
            elif numero_usuario < self.numero_secreto:
                print("📈 El número secreto es MAYOR")
                if numero_usuario < self.numero_secreto - 20:
                    print("💡 Pista: Estás muy lejos, el número es mucho mayor")
                elif numero_usuario < self.numero_secreto - 10:
                    print("💡 Pista: Estás cerca, pero aún es mayor")
            else:
                print("📉 El número secreto es MENOR")
                if numero_usuario > self.numero_secreto + 20:
                    print("💡 Pista: Estás muy lejos, el número es mucho menor")
                elif numero_usuario > self.numero_secreto + 10:
                    print("💡 Pista: Estás cerca, pero aún es menor")
            
            # Mostrar progreso
            if intentos_restantes <= 3:
                print("⚠️  ¡Cuidado! Te quedan pocos intentos")
        
        # Si se acabaron los intentos
        self.perder_partida()
        return False
    
    def ganar_partida(self):
        """Maneja la victoria del jugador"""
        print("\n" + "🎉" * 20)
        print("🎉 ¡FELICIDADES! ¡HAS ADIVINADO EL NÚMERO! 🎉")
        print("🎉" * 20)
        
        print(f"\n🎯 Número secreto: {self.numero_secreto}")
        print(f"🔄 Intentos utilizados: {self.intentos_usados}")
        print(f"🏆 Puntuación: {self.puntuacion_actual}")
        
        # Actualizar estadísticas
        self.estadisticas['partidas_jugadas'] += 1
        self.estadisticas['partidas_ganadas'] += 1
        self.estadisticas['racha_actual'] += 1
        self.estadisticas['total_intentos'] += self.intentos_usados
        
        # Actualizar mejor racha
        if self.estadisticas['racha_actual'] > self.estadisticas['mejor_racha']:
            self.estadisticas['mejor_racha'] = self.estadisticas['racha_actual']
            print("🔥 ¡Nueva mejor racha!")
        
        # Actualizar mejor puntuación
        if self.puntuacion_actual > self.mejor_puntuacion:
            self.mejor_puntuacion = self.puntuacion_actual
            print("🏆 ¡Nueva mejor puntuación!")
        
        # Mostrar mensaje motivacional
        mensajes_victoria = [
            "🌟 ¡Eres increíble!",
            "🚀 ¡Nada te detiene!",
            "💪 ¡Sigues mejorando!",
            "🎯 ¡Precisión perfecta!",
            "🔥 ¡Estás en llamas!"
        ]
        print(f"\n{random.choice(mensajes_victoria)}")
        
        self.guardar_estadisticas()
    
    def perder_partida(self):
        """Maneja la derrota del jugador"""
        print("\n" + "💔" * 20)
        print("💔 ¡GAME OVER! Se acabaron los intentos 💔")
        print("💔" * 20)
        
        print(f"\n🎯 El número secreto era: {self.numero_secreto}")
        print(f"🔄 Intentos utilizados: {self.intentos_usados}")
        
        # Actualizar estadísticas
        self.estadisticas['partidas_jugadas'] += 1
        self.estadisticas['racha_actual'] = 0
        self.estadisticas['total_intentos'] += self.intentos_usados
        
        # Mostrar mensaje motivacional
        mensajes_derrota = [
            "💪 ¡No te rindas! La próxima vez será mejor",
            "🌟 Cada intento te hace más fuerte",
            "🎯 La práctica hace al maestro",
            "🔥 ¡Mantén la determinación!",
            "🚀 ¡El éxito está a la vuelta de la esquina!"
        ]
        print(f"\n{random.choice(mensajes_derrota)}")
        
        self.guardar_estadisticas()
    
    def cargar_estadisticas(self):
        """Carga las estadísticas desde un archivo"""
        if os.path.exists(self.archivo_estadisticas):
            try:
                with open(self.archivo_estadisticas, 'r', encoding='utf-8') as archivo:
                    datos = json.load(archivo)
                    self.estadisticas = datos.get('estadisticas', self.estadisticas)
                    self.mejor_puntuacion = datos.get('mejor_puntuacion', 0)
                print("📂 Estadísticas cargadas")
            except Exception as e:
                print(f"❌ Error al cargar estadísticas: {e}")
    
    def guardar_estadisticas(self):
        """Guarda las estadísticas en un archivo"""
        try:
            datos = {
                'estadisticas': self.estadisticas,
                'mejor_puntuacion': self.mejor_puntuacion
            }
            with open(self.archivo_estadisticas, 'w', encoding='utf-8') as archivo:
                json.dump(datos, archivo, indent=2, ensure_ascii=False)
        except Exception as e:
            print(f"❌ Error al guardar estadísticas: {e}")
    
    def ejecutar(self):
        """Función principal que ejecuta el juego"""
        print("🎮 ¡Bienvenido al Juego 'Adivina el Número'!")
        print("🎯 El objetivo es adivinar un número secreto en el menor número de intentos posible")
        
        while True:
            self.mostrar_menu_principal()
            opcion = input("\nSelecciona una opción (1-5): ").strip()
            
            if opcion == "1":
                self.jugar()
                
                # Preguntar si quiere jugar otra vez
                jugar_otra_vez = input("\n¿Quieres jugar otra vez? (sí/no): ").lower().strip()
                if jugar_otra_vez not in ['sí', 'si', 's', 'y', 'yes']:
                    print("👋 ¡Gracias por jugar! ¡Hasta la próxima!")
                    break
            
            elif opcion == "2":
                self.configurar_dificultad()
            
            elif opcion == "3":
                self.mostrar_estadisticas()
            
            elif opcion == "4":
                print("\n--- 🏆 MEJORES PUNTUACIONES ---")
                if self.mejor_puntuacion > 0:
                    print(f"🥇 Mejor puntuación: {self.mejor_puntuacion}")
                    print(f"🔥 Mejor racha: {self.estadisticas['mejor_racha']}")
                else:
                    print("📭 Aún no tienes puntuaciones")
            
            elif opcion == "5":
                print("💾 Guardando estadísticas...")
                self.guardar_estadisticas()
                print("👋 ¡Gracias por jugar! ¡Hasta la próxima!")
                break
            
            else:
                print("❌ Opción no válida. Por favor selecciona 1-5")

def main():
    """Función principal del programa"""
    juego = JuegoAdivinaNumero()
    juego.ejecutar()

if __name__ == "__main__":
    main()
```

---

## 🔧 Código Completo del Proyecto

El código completo está en la sección anterior. Este proyecto incluye:

### **Características Principales**
- ✅ **Sistema de dificultad** con 4 niveles
- ✅ **Generación aleatoria** de números según dificultad
- ✅ **Sistema de puntuación** basado en intentos y dificultad
- ✅ **Estadísticas completas** del jugador
- ✅ **Persistencia de datos** en archivo JSON
- ✅ **Interfaz atractiva** con emojis y colores
- ✅ **Validación de entrada** robusta
- ✅ **Sistema de pistas** inteligente
- ✅ **Mensajes motivacionales** aleatorios

---

## 🧪 Pruebas del Proyecto

### **Casos de Prueba Básicos**
1. **Jugar partida fácil**: Verificar que funcione correctamente
2. **Cambiar dificultad**: Probar todos los niveles
3. **Ganar partida**: Verificar estadísticas y puntuación
4. **Perder partida**: Verificar que se actualicen las estadísticas

### **Casos de Prueba Avanzados**
1. **Entrada inválida**: Probar con letras, números fuera de rango
2. **Persistencia**: Verificar que se guarden las estadísticas
3. **Múltiples partidas**: Jugar varias veces y verificar estadísticas
4. **Diferentes dificultades**: Probar todos los niveles de dificultad

### **Casos de Prueba de Error**
1. **Archivo corrupto**: Manejar archivos JSON malformados
2. **Entrada vacía**: Manejar entradas sin contenido
3. **Números negativos**: Validar rangos correctos
4. **Caracteres especiales**: Manejar entradas inesperadas

---

## 🎨 Mejoras y Extensiones

### **Nivel 1: Funcionalidades Básicas**
- Modo multijugador local
- Tiempo límite por intento
- Sonidos y efectos visuales
- Diferentes modos de juego

### **Nivel 2: Funcionalidades Avanzadas**
- Ranking online de jugadores
- Logros y medallas desbloqueables
- Modo historia con narrativa
- Personalización de avatar

### **Nivel 3: Interfaz Gráfica**
- Interfaz con tkinter o PyQt
- Gráficos y animaciones
- Efectos de partículas
- Modo oscuro/claro

---

## 📊 Criterios de Evaluación

### **Funcionalidad (40 puntos)**
- [ ] Juego funciona correctamente (15 puntos)
- [ ] Sistema de dificultad implementado (10 puntos)
- [ ] Sistema de puntuación funcional (10 puntos)
- [ ] Estadísticas se actualizan correctamente (5 puntos)

### **Código (30 puntos)**
- [ ] Código bien estructurado con clases (10 puntos)
- [ ] Manejo de errores implementado (10 puntos)
- [ ] Funciones tienen nombres descriptivos (5 puntos)
- [ ] Comentarios explicativos presentes (5 puntos)

### **Interfaz (20 puntos)**
- [ ] Menú claro y fácil de usar (10 puntos)
- [ ] Mensajes informativos y atractivos (5 puntos)
- [ ] Emojis y formato consistente (5 puntos)

### **Extras (10 puntos)**
- [ ] Funcionalidades adicionales implementadas (5 puntos)
- [ ] Código optimizado y eficiente (5 puntos)

**Puntuación Total**: ___/100

---

## 🚀 Desafíos Adicionales

### **Desafío 1: Modo Multijugador**
Implementa un sistema donde dos jugadores compitan por adivinar el número en menos intentos.

### **Desafío 2: Sistema de Logros**
Crea un sistema de logros desbloqueables basado en el rendimiento del jugador.

### **Desafío 3: Modo Historia**
Implementa una narrativa donde el jugador progrese a través de diferentes niveles con historia.

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

**Archivo siguiente**: [04-Proyecto-Conversor-Unidades.md](./04-proyecto-conversor-unidades.md)

**En el siguiente proyecto crearás un conversor de unidades que te permitirá practicar con funciones y manejo de datos.**

---

## 🏆 Logros del Proyecto

**¡Felicitaciones! Has completado tu juego "Adivina el Número" funcional. Este proyecto demuestra que:**

- ✅ Puedes crear aplicaciones complejas y funcionales
- ✅ Sabes implementar lógica de juego avanzada
- ✅ Puedes manejar estadísticas y persistencia de datos
- ✅ Sabes crear interfaces de usuario atractivas
- ✅ Puedes manejar errores y validaciones
- ✅ Estás listo para proyectos más complejos

**¡Continúa con el siguiente proyecto para seguir desarrollando tus habilidades!** 