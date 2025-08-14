# 🚀 Proyecto: Analizador de Texto

## 🎯 Objetivo del Proyecto
Crear un analizador de texto completo que permita analizar documentos, extraer estadísticas, identificar patrones y aplicar los conceptos aprendidos sobre estructuras de datos y algoritmos en Python.

## ⏱️ Tiempo Estimado: 2-3 horas

---

## 📋 Descripción del Proyecto

### **¿Qué es un Analizador de Texto?**
Un analizador de texto es una aplicación que procesa documentos y extrae información útil como:
- **Estadísticas básicas**: Número de palabras, caracteres, líneas
- **Análisis de frecuencia**: Palabras más comunes, distribución
- **Análisis de complejidad**: Longitud promedio, diversidad léxica
- **Identificación de patrones**: Frases repetidas, n-gramas
- **Análisis de sentimientos**: Emociones en el texto

### **Funcionalidades Principales**
1. **Análisis Básico**: Contar palabras, caracteres, líneas
2. **Análisis de Frecuencia**: Palabras más y menos comunes
3. **Análisis de Complejidad**: Índices de legibilidad
4. **Búsqueda de Patrones**: N-gramas, frases repetidas
5. **Exportación de Resultados**: Reportes en diferentes formatos

---

## 🏗️ Estructura del Proyecto

### **1. Clase AnalizadorTexto**
```python
class AnalizadorTexto:
    def __init__(self, texto=""):
        self.texto_original = texto
        self.texto_limpio = ""
        self.palabras = []
        self.estadisticas = {}
        self.frecuencias = {}
        self.ngramas = {}
```

### **2. Funcionalidades de Análisis**
- `analizar_texto_basico()`
- `analizar_frecuencias()`
- `analizar_complejidad()`
- `generar_ngramas()`
- `buscar_patrones()`
- `generar_reporte()`

### **3. Utilidades de Procesamiento**
- `limpiar_texto()`
- `tokenizar_palabras()`
- `calcular_indices_legibilidad()`
- `exportar_resultados()`

---

## 🔧 Implementación Paso a Paso

### **Paso 1: Estructura Básica y Limpieza**
```python
import re
from collections import Counter, defaultdict
from datetime import datetime
import json

class AnalizadorTexto:
    def __init__(self, texto=""):
        self.texto_original = texto
        self.texto_limpio = ""
        self.palabras = []
        self.estadisticas = {}
        self.frecuencias = {}
        self.ngramas = {}
        self.fecha_analisis = None
    
    def cargar_texto(self, texto):
        """Carga un nuevo texto para analizar"""
        self.texto_original = texto
        self.limpiar_texto()
        self.tokenizar_palabras()
        self.fecha_analisis = datetime.now()
    
    def limpiar_texto(self):
        """Limpia el texto eliminando caracteres especiales y normalizando"""
        if not self.texto_original:
            self.texto_limpio = ""
            return
        
        # Convertir a minúsculas
        texto = self.texto_original.lower()
        
        # Eliminar caracteres especiales pero mantener espacios y puntuación básica
        texto = re.sub(r'[^\w\s\.\,\!\?\;\:]', '', texto)
        
        # Normalizar espacios múltiples
        texto = re.sub(r'\s+', ' ', texto)
        
        # Eliminar espacios al inicio y final
        self.texto_limpio = texto.strip()
    
    def tokenizar_palabras(self):
        """Divide el texto en palabras individuales"""
        if not self.texto_limpio:
            self.palabras = []
            return
        
        # Dividir por espacios y filtrar palabras vacías
        palabras_raw = self.texto_limpio.split()
        self.palabras = [palabra for palabra in palabras_raw if len(palabra) > 0]
```

### **Paso 2: Análisis Básico**
```python
    def analizar_texto_basico(self):
        """Realiza análisis básico del texto"""
        if not self.palabras:
            return {}
        
        # Estadísticas básicas
        total_palabras = len(self.palabras)
        total_caracteres = len(self.texto_limpio)
        total_caracteres_sin_espacios = len(self.texto_limpio.replace(" ", ""))
        total_lineas = self.texto_original.count('\n') + 1
        
        # Longitudes
        longitudes_palabras = [len(palabra) for palabra in self.palabras]
        longitud_promedio = sum(longitudes_palabras) / total_palabras if total_palabras > 0 else 0
        longitud_maxima = max(longitudes_palabras) if longitudes_palabras else 0
        longitud_minima = min(longitudes_palabras) if longitudes_palabras else 0
        
        # Palabras únicas
        palabras_unicas = len(set(self.palabras))
        diversidad_lexica = palabras_unicas / total_palabras if total_palabras > 0 else 0
        
        self.estadisticas = {
            "total_palabras": total_palabras,
            "total_caracteres": total_caracteres,
            "total_caracteres_sin_espacios": total_caracteres_sin_espacios,
            "total_lineas": total_lineas,
            "longitud_promedio_palabras": round(longitud_promedio, 2),
            "longitud_maxima_palabras": longitud_maxima,
            "longitud_minima_palabras": longitud_minima,
            "palabras_unicas": palabras_unicas,
            "diversidad_lexica": round(diversidad_lexica, 4)
        }
        
        return self.estadisticas
```

### **Paso 3: Análisis de Frecuencias**
```python
    def analizar_frecuencias(self):
        """Analiza la frecuencia de palabras en el texto"""
        if not self.palabras:
            return {}
        
        # Contar frecuencias
        contador = Counter(self.palabras)
        self.frecuencias = dict(contador)
        
        # Palabras más comunes
        palabras_mas_comunes = contador.most_common(10)
        
        # Palabras menos comunes (que aparecen solo una vez)
        palabras_unicas = [palabra for palabra, freq in contador.items() if freq == 1]
        
        # Distribución de frecuencias
        distribucion_freq = defaultdict(int)
        for freq in contador.values():
            if freq <= 5:
                distribucion_freq["1-5 veces"] += 1
            elif freq <= 10:
                distribucion_freq["6-10 veces"] += 1
            elif freq <= 20:
                distribucion_freq["11-20 veces"] += 1
            else:
                distribucion_freq["Más de 20 veces"] += 1
        
        return {
            "palabras_mas_comunes": palabras_mas_comunes,
            "palabras_unicas": palabras_unicas,
            "total_palabras_unicas": len(palabras_unicas),
            "distribucion_frecuencias": dict(distribucion_freq)
        }
```

### **Paso 4: Análisis de Complejidad**
```python
    def analizar_complejidad(self):
        """Analiza la complejidad y legibilidad del texto"""
        if not self.palabras:
            return {}
        
        # Estadísticas de oraciones
        oraciones = re.split(r'[.!?]+', self.texto_limpio)
        oraciones = [oracion.strip() for oracion in oraciones if oracion.strip()]
        total_oraciones = len(oraciones)
        
        # Palabras por oración
        palabras_por_oracion = [len(oracion.split()) for oracion in oraciones]
        promedio_palabras_oracion = sum(palabras_por_oracion) / total_oraciones if total_oraciones > 0 else 0
        
        # Caracteres por palabra
        caracteres_por_palabra = [len(palabra) for palabra in self.palabras]
        promedio_caracteres_palabra = sum(caracteres_por_palabra) / len(caracteres_por_palabra)
        
        # Índice de Flesch (legibilidad en inglés)
        # Fórmula: 206.835 - 1.015 × (total palabras / total oraciones) - 84.6 × (total sílabas / total palabras)
        # Simplificamos contando vocales como sílabas
        total_silabas = sum(self.contar_silabas(palabra) for palabra in self.palabras)
        
        if total_palabras > 0 and total_oraciones > 0:
            flesch_score = 206.835 - 1.015 * (len(self.palabras) / total_oraciones) - 84.6 * (total_silabas / len(self.palabras))
            flesch_score = max(0, min(100, flesch_score))  # Limitar entre 0 y 100
        else:
            flesch_score = 0
        
        # Interpretación del índice de Flesch
        if flesch_score >= 90:
            nivel_legibilidad = "Muy fácil"
        elif flesch_score >= 80:
            nivel_legibilidad = "Fácil"
        elif flesch_score >= 70:
            nivel_legibilidad = "Relativamente fácil"
        elif flesch_score >= 60:
            nivel_legibilidad = "Estándar"
        elif flesch_score >= 50:
            nivel_legibilidad = "Relativamente difícil"
        elif flesch_score >= 30:
            nivel_legibilidad = "Difícil"
        else:
            nivel_legibilidad = "Muy difícil"
        
        return {
            "total_oraciones": total_oraciones,
            "promedio_palabras_oracion": round(promedio_palabras_oracion, 2),
            "promedio_caracteres_palabra": round(promedio_caracteres_palabra, 2),
            "total_silabas": total_silabas,
            "indice_flesch": round(flesch_score, 2),
            "nivel_legibilidad": nivel_legibilidad
        }
    
    def contar_silabas(self, palabra):
        """Cuenta las sílabas de una palabra (aproximación)"""
        palabra = palabra.lower()
        vocales = 'aeiouy'
        silabas = 0
        anterior_vocal = False
        
        for letra in palabra:
            es_vocal = letra in vocales
            if es_vocal and not anterior_vocal:
                silabas += 1
            anterior_vocal = es_vocal
        
        # Asegurar al menos una sílaba
        return max(1, silabas)
```

### **Paso 5: Generación de N-gramas**
```python
    def generar_ngramas(self, n=2):
        """Genera n-gramas del texto"""
        if not self.palabras or len(self.palabras) < n:
            return {}
        
        ngramas = []
        for i in range(len(self.palabras) - n + 1):
            ngrama = ' '.join(self.palabras[i:i+n])
            ngramas.append(ngrama)
        
        # Contar frecuencias de n-gramas
        contador_ngramas = Counter(ngramas)
        self.ngramas[n] = dict(contador_ngramas)
        
        # N-gramas más comunes
        ngramas_mas_comunes = contador_ngramas.most_common(10)
        
        return {
            "total_ngramas": len(ngramas),
            "ngramas_unicos": len(set(ngramas)),
            "ngramas_mas_comunes": ngramas_mas_comunes,
            "distribucion": dict(contador_ngramas)
        }
    
    def buscar_patrones(self, patron):
        """Busca un patrón específico en el texto"""
        if not self.texto_limpio:
            return []
        
        # Buscar todas las ocurrencias del patrón
        coincidencias = re.finditer(rf'\b{re.escape(patron.lower())}\b', self.texto_limpio)
        
        resultados = []
        for coincidencia in coincidencias:
            inicio = coincidencia.start()
            fin = coincidencia.end()
            
            # Obtener contexto (palabras antes y después)
            contexto_antes = self.texto_limpio[max(0, inicio-30):inicio].split()[-3:]
            contexto_despues = self.texto_limpio[fin:fin+30].split()[:3]
            
            contexto = ' '.join(contexto_antes) + ' [' + coincidencia.group() + '] ' + ' '.join(contexto_despues)
            
            resultados.append({
                "posicion": inicio,
                "patron": coincidencia.group(),
                "contexto": contexto.strip()
            })
        
        return resultados
```

### **Paso 6: Generación de Reportes**
```python
    def generar_reporte_completo(self):
        """Genera un reporte completo del análisis"""
        if not self.palabras:
            return "No hay texto para analizar"
        
        # Realizar todos los análisis
        estadisticas_basicas = self.analizar_texto_basico()
        frecuencias = self.analizar_frecuencias()
        complejidad = self.analizar_complejidad()
        bigramas = self.generar_ngramas(2)
        trigramas = self.generar_ngramas(3)
        
        # Generar reporte
        reporte = f"""
=== 📊 REPORTE DE ANÁLISIS DE TEXTO ===
Fecha de análisis: {self.fecha_analisis.strftime('%Y-%m-%d %H:%M:%S')}
Texto original: {len(self.texto_original)} caracteres

📈 ESTADÍSTICAS BÁSICAS:
- Total de palabras: {estadisticas_basicas['total_palabras']}
- Total de caracteres: {estadisticas_basicas['total_caracteres']}
- Total de líneas: {estadisticas_basicas['total_lineas']}
- Palabras únicas: {estadisticas_basicas['palabras_unicas']}
- Diversidad léxica: {estadisticas_basicas['diversidad_lexica']}

📝 ANÁLISIS DE COMPLEJIDAD:
- Total de oraciones: {complejidad['total_oraciones']}
- Promedio de palabras por oración: {complejidad['promedio_palabras_oracion']}
- Promedio de caracteres por palabra: {complejidad['promedio_caracteres_palabra']}
- Índice de Flesch: {complejidad['indice_flesch']}
- Nivel de legibilidad: {complejidad['nivel_legibilidad']}

🔝 PALABRAS MÁS COMUNES:
"""
        
        for palabra, frecuencia in frecuencias['palabras_mas_comunes'][:10]:
            reporte += f"- '{palabra}': {frecuencia} veces\n"
        
        reporte += f"""
📊 DISTRIBUCIÓN DE FRECUENCIAS:
"""
        for rango, cantidad in frecuencias['distribucion_frecuencias'].items():
            reporte += f"- {rango}: {cantidad} palabras\n"
        
        reporte += f"""
🔄 BIGRAMAS MÁS COMUNES:
"""
        for bigrama, frecuencia in bigramas['ngramas_mas_comunes'][:5]:
            reporte += f"- '{bigrama}': {frecuencia} veces\n"
        
        return reporte
```

---

## 🎮 Interfaz de Usuario

### **Menú Principal**
```python
def mostrar_menu_principal():
    """Muestra el menú principal del analizador"""
    print("\n" + "="*50)
    print("        📝 ANALIZADOR DE TEXTO 📝")
    print("="*50)
    print("1. 📄 Cargar Texto")
    print("2. 📊 Análisis Básico")
    print("3. 🔍 Análisis de Frecuencias")
    print("4. 🧠 Análisis de Complejidad")
    print("5. 🔄 Generar N-gramas")
    print("6. 🔎 Buscar Patrones")
    print("7. 📈 Reporte Completo")
    print("8. 💾 Exportar Resultados")
    print("9. ❌ Salir")
    print("="*50)

def mostrar_menu_carga():
    """Muestra opciones para cargar texto"""
    print("\n--- OPCIONES DE CARGA ---")
    print("1. 📝 Escribir texto manualmente")
    print("2. 📁 Cargar desde archivo")
    print("3. 🌐 Cargar desde URL")
    print("4. ↩️ Volver al menú principal")
```

### **Funciones de Entrada de Datos**
```python
def cargar_texto_manual():
    """Permite al usuario escribir texto manualmente"""
    print("\n📝 Escribe tu texto (presiona Enter dos veces para terminar):")
    print("(Puedes pegar texto largo desde el portapapeles)")
    
    lineas = []
    while True:
        linea = input()
        if linea == "" and lineas and lineas[-1] == "":
            break
        lineas.append(linea)
    
    # Eliminar la línea vacía final
    if lineas and lineas[-1] == "":
        lineas.pop()
    
    return '\n'.join(lineas)

def cargar_desde_archivo():
    """Carga texto desde un archivo"""
    try:
        ruta = input("📁 Ruta del archivo: ").strip()
        if not ruta:
            return None
        
        with open(ruta, 'r', encoding='utf-8') as f:
            contenido = f.read()
        
        print(f"✅ Archivo cargado: {len(contenido)} caracteres")
        return contenido
    except FileNotFoundError:
        print("❌ Archivo no encontrado")
        return None
    except Exception as e:
        print(f"❌ Error al leer archivo: {str(e)}")
        return None

def cargar_desde_url():
    """Carga texto desde una URL (requiere requests)"""
    try:
        import requests
        url = input("🌐 URL: ").strip()
        if not url:
            return None
        
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        
        # Extraer texto del HTML (simplificado)
        contenido = response.text
        # Eliminar etiquetas HTML básicas
        contenido = re.sub(r'<[^>]+>', '', contenido)
        contenido = re.sub(r'\s+', ' ', contenido).strip()
        
        print(f"✅ URL cargada: {len(contenido)} caracteres")
        return contenido
    except ImportError:
        print("❌ Módulo 'requests' no disponible. Instala con: pip install requests")
        return None
    except Exception as e:
        print(f"❌ Error al cargar URL: {str(e)}")
        return None
```

---

## 🚀 Funcionalidades Avanzadas

### **Análisis de Sentimientos Básico**
```python
def analizar_sentimientos_basico(self):
    """Análisis básico de sentimientos (positivo/negativo/neutral)"""
    if not self.palabras:
        return {}
    
    # Diccionarios de palabras (simplificados)
    palabras_positivas = {
        'bueno', 'excelente', 'fantástico', 'maravilloso', 'genial', 'perfecto',
        'hermoso', 'increíble', 'asombroso', 'brillante', 'espléndido', 'magnífico'
    }
    
    palabras_negativas = {
        'malo', 'terrible', 'horrible', 'pésimo', 'dreadful', 'awful',
        'terrible', 'espantoso', 'atroz', 'deplorable', 'lamentable', 'pobre'
    }
    
    # Contar palabras
    positivas = sum(1 for palabra in self.palabras if palabra in palabras_positivas)
    negativas = sum(1 for palabra in self.palabras if palabra in palabras_negativas)
    total_analizadas = len(self.palabras)
    
    # Calcular puntuación
    if total_analizadas > 0:
        puntuacion_positiva = positivas / total_analizadas
        puntuacion_negativa = negativas / total_analizadas
        puntuacion_neutral = 1 - puntuacion_positiva - puntuacion_negativa
    else:
        puntuacion_positiva = puntuacion_negativa = puntuacion_neutral = 0
    
    # Determinar sentimiento predominante
    if puntuacion_positiva > puntuacion_negativa and puntuacion_positiva > puntuacion_neutral:
        sentimiento = "Positivo"
    elif puntuacion_negativa > puntuacion_positiva and puntuacion_negativa > puntuacion_neutral:
        sentimiento = "Negativo"
    else:
        sentimiento = "Neutral"
    
    return {
        "palabras_positivas": positivas,
        "palabras_negativas": negativas,
        "puntuacion_positiva": round(puntuacion_positiva, 4),
        "puntuacion_negativa": round(puntuacion_negativa, 4),
        "puntuacion_neutral": round(puntuacion_neutral, 4),
        "sentimiento_predominante": sentimiento
    }
```

### **Exportación de Resultados**
```python
def exportar_resultados_json(self, archivo="analisis_texto.json"):
    """Exporta los resultados del análisis a JSON"""
    try:
        # Recopilar todos los datos
        datos = {
            "fecha_analisis": self.fecha_analisis.isoformat() if self.fecha_analisis else None,
            "texto_original": self.texto_original[:1000] + "..." if len(self.texto_original) > 1000 else self.texto_original,
            "estadisticas": self.estadisticas,
            "frecuencias": self.frecuencias,
            "ngramas": self.ngramas
        }
        
        with open(archivo, 'w', encoding='utf-8') as f:
            json.dump(datos, f, indent=2, ensure_ascii=False)
        
        return True, f"Resultados exportados a '{archivo}'"
    except Exception as e:
        return False, f"Error al exportar: {str(e)}"

def exportar_reporte_txt(self, archivo="reporte_analisis.txt"):
    """Exporta el reporte completo a archivo de texto"""
    try:
        reporte = self.generar_reporte_completo()
        
        with open(archivo, 'w', encoding='utf-8') as f:
            f.write(reporte)
        
        return True, f"Reporte exportado a '{archivo}'"
    except Exception as e:
        return False, f"Error al exportar: {str(e)}"
```

---

## 🧪 Casos de Prueba

### **Prueba 1: Texto Simple**
```python
# Crear analizador
analizador = AnalizadorTexto()

# Texto de prueba
texto_prueba = """
Python es un lenguaje de programación de alto nivel, interpretado y orientado a objetos. 
Fue creado por Guido van Rossum y lanzado por primera vez en 1991. Python tiene una 
sintaxis simple y legible que enfatiza la legibilidad del código.

Python es ampliamente utilizado en desarrollo web, análisis de datos, inteligencia 
artificial, automatización y muchas otras áreas. Su filosofía se resume en el Zen de 
Python, que incluye principios como "Simple es mejor que complejo" y "Legibilidad cuenta".

El lenguaje es conocido por su gran biblioteca estándar y su ecosistema de paquetes 
de terceros, que incluye frameworks populares como Django, Flask, NumPy, Pandas y 
muchos más.
"""

# Cargar y analizar
analizador.cargar_texto(texto_prueba)

# Análisis básico
estadisticas = analizador.analizar_texto_basico()
print("📊 Estadísticas básicas:")
for clave, valor in estadisticas.items():
    print(f"  {clave}: {valor}")

# Análisis de frecuencias
frecuencias = analizador.analizar_frecuencias()
print(f"\n🔍 Palabras más comunes:")
for palabra, freq in frecuencias['palabras_mas_comunes'][:5]:
    print(f"  '{palabra}': {freq} veces")
```

### **Prueba 2: Análisis de Complejidad**
```python
# Análisis de complejidad
complejidad = analizador.analizar_complejidad()
print(f"\n🧠 Análisis de complejidad:")
for clave, valor in complejidad.items():
    print(f"  {clave}: {valor}")

# Generar n-gramas
bigramas = analizador.generar_ngramas(2)
print(f"\n🔄 Bigramas más comunes:")
for bigrama, freq in bigramas['ngramas_mas_comunes'][:5]:
    print(f"  '{bigrama}': {freq} veces")

trigramas = analizador.generar_ngramas(3)
print(f"\n🔄 Trigramas más comunes:")
for trigrama, freq in trigramas['ngramas_mas_comunes'][:3]:
    print(f"  '{trigrama}': {freq} veces")
```

### **Prueba 3: Búsqueda de Patrones**
```python
# Buscar patrones específicos
patrones_buscar = ["python", "lenguaje", "programación"]
for patron in patrones_buscar:
    resultados = analizador.buscar_patrones(patron)
    print(f"\n🔎 Patrón '{patron}': {len(resultados)} coincidencias")
    
    for i, resultado in enumerate(resultados[:3]):  # Mostrar solo las primeras 3
        print(f"  {i+1}. Posición {resultado['posicion']}: {resultado['contexto']}")

# Análisis de sentimientos
sentimientos = analizador.analizar_sentimientos_basico()
print(f"\n😊 Análisis de sentimientos:")
for clave, valor in sentimientos.items():
    print(f"  {clave}: {valor}")
```

### **Prueba 4: Reporte Completo**
```python
# Generar reporte completo
reporte = analizador.generar_reporte_completo()
print("\n" + reporte)

# Exportar resultados
exito_json, mensaje_json = analizador.exportar_resultados_json()
print(f"\n💾 {mensaje_json}")

exito_txt, mensaje_txt = analizador.exportar_reporte_txt()
print(f"💾 {mensaje_txt}")
```

---

## 📊 Evaluación del Proyecto

### **Criterios de Evaluación**
- [ ] **Análisis Básico** (25%): Conteo de palabras, caracteres, líneas funciona
- [ ] **Análisis de Frecuencias** (25%): Conteo y distribución de palabras funciona
- [ ] **Análisis de Complejidad** (20%): Índices de legibilidad funcionan
- [ ] **Generación de N-gramas** (15%): Bigramas y trigramas se generan correctamente
- [ ] **Interfaz de Usuario** (10%): Menús claros y fáciles de usar
- [ ] **Exportación** (5%): Funciones de exportación funcionan

### **Puntuación Total**: ___/100

---

## 🚀 Mejoras Futuras

### **Funcionalidades Adicionales**
1. **Análisis de Sentimientos Avanzado**: Usar modelos de ML pre-entrenados
2. **Análisis de Temas**: Identificación automática de temas principales
3. **Comparación de Textos**: Análisis comparativo entre documentos
4. **Análisis de Estilo**: Detección de autoría, género literario
5. **Interfaz Web**: Dashboard interactivo en navegador
6. **API REST**: Endpoints para integración con otros sistemas

### **Optimizaciones**
1. **Procesamiento Paralelo**: Análisis de textos muy largos
2. **Caché de Resultados**: Evitar re-análisis del mismo texto
3. **Validaciones Avanzadas**: Mejor manejo de diferentes formatos
4. **Logging**: Sistema de logs para debugging
5. **Testing**: Pruebas unitarias y de integración

---

## 📝 Reflexiones del Proyecto

**¿Qué aprendiste implementando este analizador de texto?**
```
[Escribe aquí tus aprendizajes]
```

**¿Qué funcionalidad te resultó más difícil de implementar?**
```
[Escribe aquí las dificultades]
```

**¿Cómo podrías mejorar este analizador en el futuro?**
```
[Escribe aquí tus ideas de mejora]
```

---

## 🎯 Próximo Paso

**Archivo siguiente**: [03-Proyecto-Juego-Ahorcado.md](./03-proyecto-juego-ahorcado.md)

**En el siguiente proyecto implementarás un juego del ahorcado que te permitirá practicar con algoritmos de búsqueda y estructuras de datos más avanzadas.**

---

**¡Excelente trabajo! Has implementado un analizador de texto completo que demuestra tu dominio de diferentes estructuras de datos, algoritmos de procesamiento y análisis de información. Este proyecto te ha preparado para crear herramientas de análisis más sofisticadas.** 