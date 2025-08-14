# 🚀 Proyecto: Juego del Ahorcado

## 🎯 Objetivo del Proyecto
Crear un juego del ahorcado completo que permita practicar algoritmos de búsqueda, estructuras de datos y lógica de programación, implementando un sistema de juego interactivo y educativo.

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 📋 Descripción del Proyecto

### **¿Qué es el Juego del Ahorcado?**
El juego del ahorcado es un juego de adivinanza de palabras donde:
- **Objetivo**: Adivinar una palabra oculta letra por letra
- **Mecánica**: Se revelan letras correctas y se penaliza con "vidas" por letras incorrectas
- **Finalización**: El jugador gana al adivinar la palabra o pierde al agotar las vidas

### **Funcionalidades Principales**
1. **Sistema de Juego**: Lógica principal del juego
2. **Gestión de Palabras**: Categorías, dificultad, selección aleatoria
3. **Interfaz Visual**: Representación del ahorcado y estado del juego
4. **Sistema de Puntuación**: Puntos, vidas, estadísticas
5. **Modo Multijugador**: Jugar contra la computadora o con amigos

---

## 🏗️ Estructura del Proyecto

### **1. Clase JuegoAhorcado**
```python
class JuegoAhorcado:
    def __init__(self):
        self.palabra_secreta = ""
        self.palabra_oculta = ""
        self.letras_adivinadas = set()
        self.letras_incorrectas = set()
        self.vidas_restantes = 6
        self.puntuacion = 0
        self.categoria = ""
        self.dificultad = "normal"
```

### **2. Funcionalidades del Juego**
- `iniciar_juego()`
- `adivinar_letra()`
- `verificar_victoria()`
- `dibujar_ahorcado()`
- `calcular_puntuacion()`
- `mostrar_estadisticas()`

### **3. Sistema de Palabras**
- `cargar_palabras()`
- `seleccionar_palabra()`
- `categorizar_palabras()`
- `ajustar_dificultad()`

---

## 🔧 Implementación Paso a Paso

### **Paso 1: Estructura Básica del Juego**
```python
import random
import json
from datetime import datetime

class JuegoAhorcado:
    def __init__(self):
        self.palabra_secreta = ""
        self.palabra_oculta = ""
        self.letras_adivinadas = set()
        self.letras_incorrectas = set()
        self.vidas_restantes = 6
        self.puntuacion = 0
        self.categoria = ""
        self.dificultad = "normal"
        self.palabras = self.cargar_palabras()
        self.estadisticas = {
            "partidas_jugadas": 0,
            "partidas_ganadas": 0,
            "partidas_perdidas": 0,
            "mejor_puntuacion": 0,
            "total_puntuacion": 0,
            "palabras_adivinadas": set(),
            "categorias_jugadas": set()
        }
    
    def cargar_palabras(self):
        """Carga las palabras del juego organizadas por categoría y dificultad"""
        return {
            "animales": {
                "facil": ["gato", "perro", "pato", "rata", "vaca", "cerdo", "lobo", "oso"],
                "normal": ["elefante", "jirafa", "leopardo", "cocodrilo", "hipopótamo", "rinoceronte"],
                "dificil": ["ornitorrinco", "quetzal", "axolotl", "narval", "okapi", "tarsero"]
            },
            "paises": {
                "facil": ["cuba", "peru", "chile", "brasil", "mexico", "canada"],
                "normal": ["argentina", "colombia", "venezuela", "ecuador", "paraguay", "uruguay"],
                "dificil": ["guatemala", "honduras", "nicaragua", "belice", "panama", "costa_rica"]
            },
            "profesiones": {
                "facil": ["medico", "abogado", "maestro", "cocinero", "pintor", "actor"],
                "normal": ["ingeniero", "arquitecto", "veterinario", "farmacéutico", "contador"],
                "dificil": ["neurocirujano", "astronauta", "paleontólogo", "etnógrafo", "bioquímico"]
            },
            "deportes": {
                "facil": ["futbol", "tenis", "golf", "boxeo", "karate", "judo"],
                "normal": ["baloncesto", "voleibol", "natación", "atletismo", "ciclismo"],
                "dificil": ["escalada", "parapente", "espeleología", "orienteering", "parkour"]
            },
            "tecnologia": {
                "facil": ["laptop", "mouse", "teclado", "monitor", "cable", "usb"],
                "normal": ["smartphone", "tablet", "router", "servidor", "firewall"],
                "dificil": ["microprocesador", "inteligencia_artificial", "blockchain", "machine_learning"]
            }
        }
    
    def seleccionar_palabra(self, categoria=None, dificultad=None):
        """Selecciona una palabra aleatoria del juego"""
        if categoria is None:
            categoria = random.choice(list(self.palabras.keys()))
        
        if dificultad is None:
            dificultad = random.choice(["facil", "normal", "dificil"])
        
        # Verificar que la categoría y dificultad existan
        if categoria not in self.palabras or dificultad not in self.palabras[categoria]:
            # Seleccionar aleatoriamente si no existe
            categoria = random.choice(list(self.palabras.keys()))
            dificultad = random.choice(["facil", "normal", "dificil"])
        
        palabra = random.choice(self.palabras[categoria][dificultad])
        
        self.palabra_secreta = palabra
        self.categoria = categoria
        self.dificultad = dificultad
        
        # Crear palabra oculta
        self.palabra_oculta = "_" * len(palabra)
        
        return palabra, categoria, dificultad
```

### **Paso 2: Lógica Principal del Juego**
```python
    def iniciar_juego(self, categoria=None, dificultad=None):
        """Inicia un nuevo juego"""
        # Reiniciar estado del juego
        self.letras_adivinadas.clear()
        self.letras_incorrectas.clear()
        self.vidas_restantes = 6
        self.puntuacion = 0
        
        # Seleccionar palabra
        palabra, categoria, dificultad = self.seleccionar_palabra(categoria, dificultad)
        
        # Ajustar vidas según dificultad
        if dificultad == "facil":
            self.vidas_restantes = 8
        elif dificultad == "normal":
            self.vidas_restantes = 6
        else:  # dificil
            self.vidas_restantes = 4
        
        print(f"\n🎯 Nueva partida iniciada!")
        print(f"📚 Categoría: {categoria.title()}")
        print(f"⚡ Dificultad: {dificultad.title()}")
        print(f"💀 Vidas: {self.vidas_restantes}")
        print(f"🔤 Palabra: {self.palabra_oculta}")
        
        return True
    
    def adivinar_letra(self, letra):
        """Procesa el intento de adivinar una letra"""
        letra = letra.lower().strip()
        
        # Validaciones
        if not letra.isalpha():
            return False, "❌ Por favor ingresa una letra válida"
        
        if len(letra) != 1:
            return False, "❌ Por favor ingresa solo una letra"
        
        if letra in self.letras_adivinadas or letra in self.letras_incorrectas:
            return False, f"❌ Ya intentaste la letra '{letra}'"
        
        # Verificar si la letra está en la palabra
        if letra in self.palabra_secreta:
            # Letra correcta
            self.letras_adivinadas.add(letra)
            
            # Actualizar palabra oculta
            nueva_oculta = ""
            for i, char in enumerate(self.palabra_secreta):
                if char == letra or char in self.letras_adivinadas:
                    nueva_oculta += char
                else:
                    nueva_oculta += "_"
            self.palabra_oculta = nueva_oculta
            
            # Calcular puntuación por letra correcta
            letras_correctas = self.palabra_secreta.count(letra)
            puntos_letra = self.calcular_puntos_letra(letra, letras_correctas)
            self.puntuacion += puntos_letra
            
            return True, f"✅ ¡Correcto! La letra '{letra}' aparece {letras_correctas} vez(es). +{puntos_letra} puntos"
        else:
            # Letra incorrecta
            self.letras_incorrectas.add(letra)
            self.vidas_restantes -= 1
            
            return False, f"❌ Incorrecto. La letra '{letra}' no está en la palabra. Vidas restantes: {self.vidas_restantes}"
    
    def calcular_puntos_letra(self, letra, cantidad):
        """Calcula los puntos por adivinar una letra correcta"""
        # Puntos base según dificultad
        puntos_base = {
            "facil": 10,
            "normal": 15,
            "dificil": 25
        }
        
        # Multiplicador por cantidad de veces que aparece
        multiplicador = 1 + (cantidad - 1) * 0.5
        
        # Bonus por letras poco comunes
        letras_comunes = "aeiou"
        if letra not in letras_comunes:
            multiplicador *= 1.2
        
        return int(puntos_base[self.dificultad] * multiplicador)
    
    def verificar_victoria(self):
        """Verifica si el jugador ha ganado o perdido"""
        if self.palabra_oculta == self.palabra_secreta:
            # Victoria
            self.puntuacion += self.vidas_restantes * 50  # Bonus por vidas restantes
            self.estadisticas["partidas_ganadas"] += 1
            self.estadisticas["palabras_adivinadas"].add(self.palabra_secreta)
            self.estadisticas["categorias_jugadas"].add(self.categoria)
            
            if self.puntuacion > self.estadisticas["mejor_puntuacion"]:
                self.estadisticas["mejor_puntuacion"] = self.puntuacion
            
            self.estadisticas["total_puntuacion"] += self.puntuacion
            
            return "victoria", f"🎉 ¡FELICIDADES! Has adivinado la palabra '{self.palabra_secreta}'"
        
        elif self.vidas_restantes <= 0:
            # Derrota
            self.estadisticas["partidas_perdidas"] += 1
            self.estadisticas["categorias_jugadas"].add(self.categoria)
            
            return "derrota", f"💀 ¡GAME OVER! La palabra era '{self.palabra_secreta}'"
        
        else:
            # Juego en curso
            return "en_curso", None
```

### **Paso 3: Interfaz Visual del Ahorcado**
```python
    def dibujar_ahorcado(self):
        """Dibuja el estado actual del ahorcado"""
        estados = [
            # 0 vidas perdidas
            """
               +---+
                   |
                   |
                   |
                  ===
            """,
            # 1 vida perdida
            """
               +---+
               O   |
                   |
                   |
                  ===
            """,
            # 2 vidas perdidas
            """
               +---+
               O   |
               |   |
                   |
                  ===
            """,
            # 3 vidas perdidas
            """
               +---+
               O   |
              /|   |
                   |
                  ===
            """,
            # 4 vidas perdidas
            """
               +---+
               O   |
              /|\\  |
                   |
                  ===
            """,
            # 5 vidas perdidas
            """
               +---+
               O   |
              /|\\  |
              /    |
                  ===
            """,
            # 6 vidas perdidas
            """
               +---+
               O   |
              /|\\  |
              / \\  |
                  ===
            """
        ]
        
        vidas_perdidas = 6 - self.vidas_restantes
        return estados[min(vidas_perdidas, 6)]
    
    def mostrar_estado_juego(self):
        """Muestra el estado actual del juego"""
        print("\n" + "="*50)
        print("        🎮 ESTADO DEL JUEGO 🎮")
        print("="*50)
        
        # Dibujar ahorcado
        print(self.dibujar_ahorcado())
        
        # Información del juego
        print(f"🔤 Palabra: {' '.join(self.palabra_oculta)}")
        print(f"💀 Vidas: {self.vidas_restantes}")
        print(f"⭐ Puntuación: {self.puntuacion}")
        print(f"📚 Categoría: {self.categoria.title()}")
        print(f"⚡ Dificultad: {self.dificultad.title()}")
        
        # Letras adivinadas e incorrectas
        if self.letras_adivinadas:
            print(f"✅ Letras correctas: {', '.join(sorted(self.letras_adivinadas))}")
        if self.letras_incorrectas:
            print(f"❌ Letras incorrectas: {', '.join(sorted(self.letras_incorrectas))}")
        
        print("="*50)
```

### **Paso 4: Sistema de Puntuación y Estadísticas**
```python
    def mostrar_estadisticas(self):
        """Muestra las estadísticas del jugador"""
        if self.estadisticas["partidas_jugadas"] == 0:
            print("\n📊 No hay estadísticas disponibles. ¡Juega tu primera partida!")
            return
        
        total_partidas = self.estadisticas["partidas_jugadas"]
        partidas_ganadas = self.estadisticas["partidas_ganadas"]
        partidas_perdidas = self.estadisticas["partidas_perdidas"]
        porcentaje_victoria = (partidas_ganadas / total_partidas) * 100 if total_partidas > 0 else 0
        
        print("\n" + "="*50)
        print("        📊 ESTADÍSTICAS DEL JUGADOR 📊")
        print("="*50)
        print(f"🎮 Total de partidas: {total_partidas}")
        print(f"🏆 Partidas ganadas: {partidas_ganadas}")
        print(f"💀 Partidas perdidas: {partidas_perdidas}")
        print(f"📈 Porcentaje de victoria: {porcentaje_victoria:.1f}%")
        print(f"⭐ Mejor puntuación: {self.estadisticas['mejor_puntuacion']}")
        print(f"💰 Puntuación total: {self.estadisticas['total_puntuacion']}")
        print(f"📚 Categorías jugadas: {len(self.estadisticas['categorias_jugadas'])}")
        print(f"🔤 Palabras adivinadas: {len(self.estadisticas['palabras_adivinadas'])}")
        
        if self.estadisticas['categorias_jugadas']:
            print(f"\n📚 Categorías jugadas:")
            for categoria in sorted(self.estadisticas['categorias_jugadas']):
                print(f"  - {categoria.title()}")
        
        if self.estadisticas['palabras_adivinadas']:
            print(f"\n🔤 Últimas palabras adivinadas:")
            palabras_recientes = list(self.estadisticas['palabras_adivinadas'])[-5:]
            for palabra in palabras_recientes:
                print(f"  - {palabra}")
        
        print("="*50)
    
    def reiniciar_estadisticas(self):
        """Reinicia todas las estadísticas del jugador"""
        confirmacion = input("⚠️ ¿Estás seguro de que quieres reiniciar todas las estadísticas? (sí/no): ").lower()
        if confirmacion in ['sí', 'si', 's', 'y', 'yes']:
            self.estadisticas = {
                "partidas_jugadas": 0,
                "partidas_ganadas": 0,
                "partidas_perdidas": 0,
                "mejor_puntuacion": 0,
                "total_puntuacion": 0,
                "palabras_adivinadas": set(),
                "categorias_jugadas": set()
            }
            print("✅ Estadísticas reiniciadas")
        else:
            print("❌ Operación cancelada")
```

### **Paso 5: Funciones de Ayuda y Pistas**
```python
    def obtener_pista(self):
        """Proporciona una pista al jugador (cuesta puntos)"""
        if self.puntuacion < 50:
            return False, "❌ Necesitas al menos 50 puntos para obtener una pista"
        
        # Encontrar letras no adivinadas
        letras_no_adivinadas = [char for char in self.palabra_secreta if char not in self.letras_adivinadas]
        
        if not letras_no_adivinadas:
            return False, "❌ Ya has adivinado todas las letras"
        
        # Seleccionar una letra aleatoria como pista
        letra_pista = random.choice(letras_no_adivinadas)
        
        # Costo de la pista
        costo_pista = 50
        self.puntuacion -= costo_pista
        
        return True, f"💡 Pista: La letra '{letra_pista}' está en la palabra. Costo: -{costo_pista} puntos"
    
    def mostrar_ayuda(self):
        """Muestra información de ayuda para el jugador"""
        print("\n" + "="*50)
        print("        ❓ AYUDA DEL JUEGO ❓")
        print("="*50)
        print("🎯 OBJETIVO: Adivinar la palabra oculta letra por letra")
        print("💀 VIDAS: Pierdes una vida por cada letra incorrecta")
        print("⭐ PUNTUACIÓN: Ganas puntos por letras correctas")
        print("💡 PISTAS: Puedes comprar pistas por 50 puntos")
        print("📚 CATEGORÍAS: Animales, Países, Profesiones, Deportes, Tecnología")
        print("⚡ DIFICULTADES:")
        print("  - Fácil: 8 vidas, palabras cortas")
        print("  - Normal: 6 vidas, palabras medianas")
        print("  - Difícil: 4 vidas, palabras largas")
        print("🏆 CONSEJOS:")
        print("  - Empieza con vocales comunes (a, e, i, o, u)")
        print("  - Observa el patrón de la palabra")
        print("  - Usa pistas cuando estés atascado")
        print("="*50)
```

---

## 🎮 Interfaz de Usuario

### **Menú Principal**
```python
def mostrar_menu_principal():
    """Muestra el menú principal del juego"""
    print("\n" + "="*50)
    print("        🎮 JUEGO DEL AHORCADO 🎮")
    print("="*50)
    print("1. 🎯 Jugar")
    print("2. 📊 Ver Estadísticas")
    print("3. ⚙️  Configuración")
    print("4. ❓ Ayuda")
    print("5. 🔄 Reiniciar Estadísticas")
    print("6. ❌ Salir")
    print("="*50)

def mostrar_menu_juego():
    """Muestra el menú durante el juego"""
    print("\n--- OPCIONES DEL JUEGO ---")
    print("1. 🔤 Adivinar letra")
    print("2. 💡 Obtener pista")
    print("3. 📊 Ver estadísticas")
    print("4. 🆘 Rendirse")
    print("5. ↩️ Volver al menú principal")

def mostrar_menu_configuracion():
    """Muestra el menú de configuración"""
    print("\n--- CONFIGURACIÓN ---")
    print("1. 📚 Seleccionar categoría")
    print("2. ⚡ Seleccionar dificultad")
    print("3. 🎲 Modo aleatorio")
    print("4. ↩️ Volver al menú principal")
```

### **Funciones de Entrada de Datos**
```python
def seleccionar_categoria():
    """Permite al usuario seleccionar una categoría"""
    categorias = ["animales", "paises", "profesiones", "deportes", "tecnologia", "aleatorio"]
    
    print("\n📚 Selecciona una categoría:")
    for i, categoria in enumerate(categorias, 1):
        print(f"{i}. {categoria.title()}")
    
    try:
        opcion = int(input("\nOpción: "))
        if 1 <= opcion <= len(categorias):
            categoria_seleccionada = categorias[opcion - 1]
            if categoria_seleccionada == "aleatorio":
                return None
            return categoria_seleccionada
        else:
            print("❌ Opción inválida")
            return None
    except ValueError:
        print("❌ Por favor ingresa un número")
        return None

def seleccionar_dificultad():
    """Permite al usuario seleccionar una dificultad"""
    dificultades = ["facil", "normal", "dificil", "aleatorio"]
    
    print("\n⚡ Selecciona una dificultad:")
    print("1. Fácil (8 vidas, palabras cortas)")
    print("2. Normal (6 vidas, palabras medianas)")
    print("3. Difícil (4 vidas, palabras largas)")
    print("4. Aleatorio")
    
    try:
        opcion = int(input("\nOpción: "))
        if 1 <= opcion <= len(dificultades):
            dificultad_seleccionada = dificultades[opcion - 1]
            if dificultad_seleccionada == "aleatorio":
                return None
            return dificultad_seleccionada
        else:
            print("❌ Opción inválida")
            return None
    except ValueError:
        print("❌ Por favor ingresa un número")
        return None
```

---

## 🚀 Funcionalidades Avanzadas

### **Modo Multijugador Simple**
```python
def modo_multijugador(self):
    """Permite a dos jugadores jugar turnos alternados"""
    print("\n🎮 MODO MULTIJUGADOR")
    print("Los jugadores se turnan para adivinar la misma palabra")
    
    # Obtener nombres de jugadores
    jugador1 = input("👤 Nombre del Jugador 1: ").strip() or "Jugador 1"
    jugador2 = input("👤 Nombre del Jugador 2: ").strip() or "Jugador 2"
    
    # Iniciar juego
    self.iniciar_juego()
    
    jugador_actual = jugador1
    turno = 1
    
    while True:
        print(f"\n🔄 Turno {turno} - {jugador_actual}")
        self.mostrar_estado_juego()
        
        # Opciones del turno
        print(f"\n--- TURNO DE {jugador_actual.upper()} ---")
        print("1. 🔤 Adivinar letra")
        print("2. 💡 Obtener pista")
        print("3. 🆘 Rendirse")
        
        opcion = input("Opción: ").strip()
        
        if opcion == "1":
            letra = input("🔤 Ingresa una letra: ").strip()
            exito, mensaje = self.adivinar_letra(letra)
            print(mensaje)
            
            # Verificar fin del juego
            estado, mensaje_final = self.verificar_victoria()
            if estado != "en_curso":
                print(f"\n{mensaje_final}")
                print(f"🏆 Ganador: {jugador_actual}")
                break
        
        elif opcion == "2":
            exito, mensaje = self.obtener_pista()
            print(mensaje)
        
        elif opcion == "3":
            print(f"🏳️ {jugador_actual} se rinde")
            print(f"💀 La palabra era: {self.palabra_secreta}")
            break
        
        else:
            print("❌ Opción inválida")
            continue
        
        # Cambiar jugador
        jugador_actual = jugador2 if jugador_actual == jugador1 else jugador1
        if jugador_actual == jugador1:
            turno += 1
```

### **Sistema de Logros**
```python
def verificar_logros(self):
    """Verifica y otorga logros al jugador"""
    logros = []
    
    # Logros por partidas
    if self.estadisticas["partidas_ganadas"] >= 1:
        logros.append("🥉 Primer Triunfo - Gana tu primera partida")
    
    if self.estadisticas["partidas_ganadas"] >= 5:
        logros.append("🥈 Ganador Experto - Gana 5 partidas")
    
    if self.estadisticas["partidas_ganadas"] >= 10:
        logros.append("🥇 Maestro del Ahorcado - Gana 10 partidas")
    
    # Logros por puntuación
    if self.estadisticas["mejor_puntuacion"] >= 100:
        logros.append("⭐ Puntuador - Alcanza 100 puntos en una partida")
    
    if self.estadisticas["mejor_puntuacion"] >= 200:
        logros.append("⭐⭐ Puntuador Experto - Alcanza 200 puntos en una partida")
    
    # Logros por categorías
    if len(self.estadisticas["categorias_jugadas"]) >= 3:
        logros.append("📚 Explorador - Juega en 3 categorías diferentes")
    
    if len(self.estadisticas["categorias_jugadas"]) >= 5:
        logros.append("📚 Maestro del Conocimiento - Juega en todas las categorías")
    
    # Logros por palabras
    if len(self.estadisticas["palabras_adivinadas"]) >= 10:
        logros.append("🔤 Vocabulario Rico - Adivina 10 palabras diferentes")
    
    if len(self.estadisticas["palabras_adivinadas"]) >= 25:
        logros.append("🔤 Lexicógrafo - Adivina 25 palabras diferentes")
    
    return logros

def mostrar_logros(self):
    """Muestra los logros del jugador"""
    logros = self.verificar_logros()
    
    if not logros:
        print("\n🏆 No hay logros disponibles aún. ¡Sigue jugando!")
        return
    
    print("\n" + "="*50)
    print("        🏆 LOGROS DESBLOQUEADOS 🏆")
    print("="*50)
    
    for i, logro in enumerate(logros, 1):
        print(f"{i}. {logro}")
    
    print("="*50)
```

### **Persistencia de Datos**
```python
def guardar_estadisticas(self, archivo="estadisticas_ahorcado.json"):
    """Guarda las estadísticas del jugador en un archivo"""
    try:
        # Convertir sets a listas para serialización JSON
        datos_guardar = self.estadisticas.copy()
        datos_guardar["palabras_adivinadas"] = list(self.estadisticas["palabras_adivinadas"])
        datos_guardar["categorias_jugadas"] = list(self.estadisticas["categorias_jugadas"])
        
        with open(archivo, 'w', encoding='utf-8') as f:
            json.dump(datos_guardar, f, indent=2, ensure_ascii=False)
        
        return True, f"Estadísticas guardadas en '{archivo}'"
    except Exception as e:
        return False, f"Error al guardar: {str(e)}"

def cargar_estadisticas(self, archivo="estadisticas_ahorcado.json"):
    """Carga las estadísticas del jugador desde un archivo"""
    try:
        with open(archivo, 'r', encoding='utf-8') as f:
            datos = json.load(f)
        
        # Convertir listas de vuelta a sets
        self.estadisticas = datos
        self.estadisticas["palabras_adivinadas"] = set(datos["palabras_adivinadas"])
        self.estadisticas["categorias_jugadas"] = set(datos["categorias_jugadas"])
        
        return True, f"Estadísticas cargadas desde '{archivo}'"
    except FileNotFoundError:
        return False, f"Archivo '{archivo}' no encontrado"
    except Exception as e:
        return False, f"Error al cargar: {str(e)}"
```

---

## 🧪 Casos de Prueba

### **Prueba 1: Juego Básico**
```python
# Crear juego
juego = JuegoAhorcado()

# Iniciar partida
juego.iniciar_juego("animales", "facil")
print(f"Palabra seleccionada: {juego.palabra_secreta}")

# Simular juego
letras_prueba = ["a", "e", "i", "o", "u", "t", "r", "s"]
for letra in letras_prueba:
    exito, mensaje = juego.adivinar_letra(letra)
    print(f"Letra '{letra}': {mensaje}")
    
    if exito:
        print(f"Palabra actual: {juego.palabra_oculta}")
    
    estado, mensaje_final = juego.verificar_victoria()
    if estado != "en_curso":
        print(f"\n{mensaje_final}")
        break

# Mostrar estado final
juego.mostrar_estado_juego()
```

### **Prueba 2: Sistema de Puntuación**
```python
# Probar sistema de puntuación
juego.iniciar_juego("tecnologia", "normal")
print(f"Palabra: {juego.palabra_secreta}")

# Adivinar letras correctas
letras_correctas = ["a", "e", "i", "o"]
for letra in letras_correctas:
    if letra in juego.palabra_secreta:
        exito, mensaje = juego.adivinar_letra(letra)
        print(f"{mensaje}")
        print(f"Puntuación actual: {juego.puntuacion}")

# Obtener pista
if juego.puntuacion >= 50:
    exito, mensaje = juego.obtener_pista()
    print(mensaje)
    print(f"Puntuación después de pista: {juego.puntuacion}")
```

### **Prueba 3: Estadísticas y Logros**
```python
# Simular múltiples partidas
for i in range(3):
    print(f"\n--- PARTIDA {i+1} ---")
    juego.iniciar_juego()
    
    # Simular juego rápido
    letras_comunes = ["a", "e", "i", "o", "u", "n", "r", "s", "t"]
    for letra in letras_comunes:
        if letra in juego.palabra_secreta:
            juego.adivinar_letra(letra)
            estado, mensaje = juego.verificar_victoria()
            if estado != "en_curso":
                break
    
    # Verificar resultado
    if juego.palabra_oculta == juego.palabra_secreta:
        print("✅ Victoria")
    else:
        print("❌ Derrota")

# Mostrar estadísticas
juego.mostrar_estadisticas()

# Verificar logros
logros = juego.verificar_logros()
print(f"\n🏆 Logros desbloqueados: {len(logros)}")
for logro in logros:
    print(f"  - {logro}")
```

### **Prueba 4: Funcionalidades Avanzadas**
```python
# Probar modo multijugador
juego.modo_multijugador()

# Probar guardado/carga de estadísticas
exito, mensaje = juego.guardar_estadisticas()
print(f"💾 {mensaje}")

# Crear nuevo juego y cargar estadísticas
nuevo_juego = JuegoAhorcado()
exito, mensaje = nuevo_juego.cargar_estadisticas()
print(f"📂 {mensaje}")

# Verificar que se cargaron las estadísticas
nuevo_juego.mostrar_estadisticas()
```

---

## 📊 Evaluación del Proyecto

### **Criterios de Evaluación**
- [ ] **Funcionalidad Básica** (30%): Juego funciona correctamente
- [ ] **Sistema de Puntuación** (20%): Puntos se calculan y asignan correctamente
- [ ] **Gestión de Palabras** (15%): Categorías y dificultades funcionan
- [ ] **Interfaz de Usuario** (15%): Menús claros y fáciles de usar
- [ ] **Estadísticas** (10%): Sistema de seguimiento funciona
- [ ] **Funcionalidades Avanzadas** (10%): Pistas, multijugador, logros

### **Puntuación Total**: ___/100

---

## 🚀 Mejoras Futuras

### **Funcionalidades Adicionales**
1. **Interfaz Gráfica**: Usar Tkinter o Pygame para interfaz visual
2. **Sonidos**: Efectos de sonido para victoria/derrota
3. **Modo Online**: Jugar contra otros jugadores en internet
4. **Editor de Palabras**: Permitir agregar palabras personalizadas
5. **Modo Historia**: Jugar con palabras de un tema específico
6. **Sistema de Niveles**: Desbloquear categorías por progreso

### **Optimizaciones**
1. **Base de Datos**: Usar SQLite para almacenar palabras y estadísticas
2. **Algoritmos de IA**: Sugerencias inteligentes de letras
3. **Múltiples Idiomas**: Soporte para diferentes idiomas
4. **Modo Sin Conexión**: Juego completamente offline
5. **Backup en la Nube**: Sincronización de estadísticas

---

## 📝 Reflexiones del Proyecto

**¿Qué aprendiste implementando este juego del ahorcado?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué funcionalidad te resultó más difícil de implementar?**
```
[Escribe aquí las dificultades]
```

**¿Cómo podrías mejorar este juego en el futuro?**
```
[Escribe aquí tus ideas de mejora]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [01-Checklist-Conceptos.md](./../04-EVALUACION/01-checklist-conceptos.md)

**En la siguiente lección evaluarás tu progreso en el segundo concepto del curso, completando el checklist de autoevaluación.**

---

**¡Excelente trabajo! Has implementado un juego del ahorcado completo que demuestra tu dominio de algoritmos, estructuras de datos y lógica de programación. Este proyecto te ha preparado para crear aplicaciones más complejas y entretenidas.** 