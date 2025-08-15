# 📝 Test de Conocimientos: Paradigmas de Programación

## 🎯 **OBJETIVO**
Este test evalúa tu comprensión teórica y práctica de los Paradigmas de Programación. Incluye preguntas teóricas, ejercicios de código y problemas de lógica.

---

## ⏱️ **INFORMACIÓN DEL TEST**
- **Duración estimada**: 90 minutos
- **Puntuación total**: 100 puntos
- **Puntuación mínima para aprobar**: 70 puntos
- **Nivel**: Intermedio-Avanzado

---

## 📚 **SECCIÓN 1: PREGUNTAS TEÓRICAS (40 puntos)**

### **1. Conceptos Fundamentales (10 puntos)**

#### **1.1 ¿Qué es un paradigma de programación? (2 puntos)**
- [ ] A) Un lenguaje de programación específico
- [ ] B) Un estilo o enfoque para resolver problemas computacionales
- [ ] C) Un patrón de diseño específico
- [ ] D) Una herramienta de desarrollo

#### **1.2 ¿Cuál de los siguientes NO es un paradigma de programación? (2 puntos)**
- [ ] A) Imperativo
- [ ] B) Orientado a Objetos
- [ ] C) Funcional
- [ ] D) Estructurado

#### **1.3 ¿Qué significa programación multiparadigma? (3 puntos)**
- [ ] A) Usar múltiples lenguajes de programación
- [ ] B) Combinar diferentes estilos de programación en un mismo proyecto
- [ ] C) Programar en diferentes plataformas
- [ ] D) Usar múltiples frameworks

#### **1.4 ¿Cuál es la principal ventaja de la programación funcional? (3 puntos)**
- [ ] A) Mayor velocidad de ejecución
- [ ] B) Código más predecible y fácil de probar
- [ ] C) Mejor rendimiento en memoria
- [ ] D) Sintaxis más simple

### **2. Programación Orientada a Objetos (15 puntos)**

#### **2.1 ¿Cuál es la diferencia entre una clase y un objeto? (3 puntos)**
- [ ] A) No hay diferencia, son lo mismo
- [ ] B) Una clase es una plantilla, un objeto es una instancia
- [ ] C) Una clase es un objeto con métodos
- [ ] D) Un objeto es una clase abstracta

#### **2.2 ¿Qué principio de POO se refiere a ocultar datos internos? (2 puntos)**
- [ ] A) Herencia
- [ ] B) Polimorfismo
- [ ] C) Encapsulamiento
- [ ] D) Abstracción

#### **2.3 ¿Cuál es el propósito de la herencia? (3 puntos)**
- [ ] A) Reutilizar código y establecer relaciones jerárquicas
- [ ] B) Mejorar el rendimiento del programa
- [ ] C) Reducir el tamaño del código
- [ ] D) Facilitar la depuración

#### **2.4 ¿Qué es el polimorfismo? (3 puntos)**
- [ ] A) La capacidad de una clase de heredar de múltiples clases
- [ ] B) La capacidad de diferentes objetos de responder al mismo mensaje de diferentes maneras
- [ ] C) La capacidad de ocultar implementaciones
- [ ] D) La capacidad de crear múltiples instancias

#### **2.5 ¿Cuándo se usa un método estático? (4 puntos)**
- [ ] A) Cuando el método no necesita acceso a la instancia
- [ ] B) Cuando el método es muy rápido
- [ ] C) Cuando el método es privado
- [ ] D) Cuando el método es abstracto

### **3. Principios SOLID (15 puntos)**

#### **3.1 ¿Qué establece el Principio de Responsabilidad Única? (3 puntos)**
- [ ] A) Una clase debe tener solo un método
- [ ] B) Una clase debe tener solo una razón para cambiar
- [ ] C) Una clase debe tener solo un atributo
- [ ] D) Una clase debe tener solo una instancia

#### **3.2 ¿Qué significa el Principio Abierto/Cerrado? (3 puntos)**
- [ ] A) El código debe estar abierto para lectura y cerrado para modificación
- [ ] B) Las entidades deben estar abiertas para extensión pero cerradas para modificación
- [ ] C) El código debe estar abierto para todos los desarrolladores
- [ ] D) Las clases deben estar abiertas para herencia pero cerradas para composición

#### **3.3 ¿Qué establece el Principio de Sustitución de Liskov? (3 puntos)**
- [ ] A) Las clases derivadas deben poder sustituir a sus clases base
- [ ] B) Las clases base deben poder sustituir a sus clases derivadas
- [ ] C) Solo las clases abstractas pueden ser sustituidas
- [ ] D) Las interfaces no pueden ser sustituidas

#### **3.4 ¿Qué significa el Principio de Segregación de Interfaces? (3 puntos)**
- [ ] A) Las interfaces deben ser pequeñas y específicas
- [ ] B) Las interfaces deben ser grandes y generales
- [ ] C) Solo debe haber una interfaz por clase
- [ ] D) Las interfaces deben ser privadas

#### **3.5 ¿Qué establece el Principio de Inversión de Dependencias? (3 puntos)**
- [ ] A) Los módulos de alto nivel no deben depender de módulos de bajo nivel
- [ ] B) Los módulos de bajo nivel deben depender de módulos de alto nivel
- [ ] C) Todos los módulos deben ser independientes
- [ ] D) Solo los módulos principales pueden tener dependencias

---

## 🛠️ **SECCIÓN 2: EJERCICIOS DE CÓDIGO (40 puntos)**

### **Ejercicio 1: Implementación de Clases (15 puntos)**

#### **1.1 Crea una clase `Empleado` con los siguientes requisitos:**
- Atributos: `nombre`, `salario`, `departamento`
- Método `calcular_bonificacion()` que retorne el 10% del salario
- Método `obtener_info()` que retorne un string con toda la información
- Validación en el constructor: salario debe ser positivo

```python
# Tu código aquí:

```

#### **1.2 Crea una clase `Gerente` que herede de `Empleado` con:**
- Atributo adicional: `equipo` (lista de empleados)
- Sobrescribe `calcular_bonificacion()` para retornar 15% del salario
- Método `agregar_empleado(empleado)` para añadir empleados al equipo
- Método `obtener_tamaño_equipo()` que retorne el número de empleados

```python
# Tu código aquí:

```

### **Ejercicio 2: Programación Funcional (15 puntos)**

#### **2.1 Implementa las siguientes funciones funcionales:**

```python
# 1. Función que retorne el cuadrado de todos los números pares de una lista
def cuadrados_pares(numeros):
    # Tu código aquí:
    pass

# 2. Función que use reduce para encontrar el máximo de una lista
def maximo_lista(numeros):
    # Tu código aquí:
    pass

# 3. Función que retorne solo las palabras que empiecen con vocal
def palabras_vocal(palabras):
    # Tu código aquí:
    pass
```

#### **2.2 Escribe una comprensión de lista que:**
- Genere los primeros 20 números de Fibonacci
- Solo incluya los números pares
- Cada número sea elevado al cuadrado

```python
# Tu código aquí:

```

### **Ejercicio 3: Patrones de Diseño (10 puntos)**

#### **3.1 Implementa el patrón Singleton para una clase `Configuracion`:**
- Debe tener un atributo `configuraciones` (diccionario)
- Método `obtener_configuracion(clave)` para obtener valores
- Método `establecer_configuracion(clave, valor)` para establecer valores
- Solo debe permitir una instancia

```python
# Tu código aquí:

```

---

## 🧠 **SECCIÓN 3: PROBLEMAS DE LÓGICA (20 puntos)**

### **Problema 1: Diseño de Sistema (10 puntos)**

Diseña un sistema de gestión de biblioteca que incluya:
- Clases principales necesarias
- Relaciones entre clases
- Principios SOLID aplicados
- Patrones de diseño utilizados

**Describe tu diseño:**

### **Problema 2: Análisis de Código (10 puntos)**

Analiza el siguiente código y responde:

```python
class Usuario:
    def __init__(self, nombre, email):
        self.nombre = nombre
        self.email = email
        self.contraseña = None
    
    def establecer_contraseña(self, contraseña):
        if len(contraseña) < 8:
            raise ValueError("La contraseña debe tener al menos 8 caracteres")
        self.contraseña = contraseña
    
    def autenticar(self, contraseña):
        return self.contraseña == contraseña

class Admin(Usuario):
    def __init__(self, nombre, email, nivel_acceso):
        super().__init__(nombre, email)
        self.nivel_acceso = nivel_acceso
    
    def establecer_contraseña(self, contraseña):
        if len(contraseña) < 12:
            raise ValueError("La contraseña de admin debe tener al menos 12 caracteres")
        super().establecer_contraseña(contraseña)
    
    def puede_acceder(self, recurso):
        return self.nivel_acceso >= recurso.nivel_requerido
```

#### **Preguntas:**
1. **¿Qué principio SOLID se está violando y por qué? (3 puntos)**
2. **¿Cómo mejorarías el diseño para cumplir mejor los principios SOLID? (4 puntos)**
3. **¿Qué patrones de diseño podrías aplicar aquí? (3 puntos)**

---

## 📊 **SOLUCIONES Y PUNTUACIÓN**

### **Respuestas Correctas:**

#### **Sección 1: Preguntas Teóricas**
- **1.1**: B (2 puntos)
- **1.2**: D (2 puntos)
- **1.3**: B (3 puntos)
- **1.4**: B (3 puntos)
- **2.1**: B (3 puntos)
- **2.2**: C (2 puntos)
- **2.3**: A (3 puntos)
- **2.4**: B (3 puntos)
- **2.5**: A (4 puntos)
- **3.1**: B (3 puntos)
- **3.2**: B (3 puntos)
- **3.3**: A (3 puntos)
- **3.4**: A (3 puntos)
- **3.5**: A (3 puntos)

### **Criterios de Evaluación:**

#### **Ejercicio 1: Implementación de Clases (15 puntos)**
- **Clase Empleado**: 5 puntos
  - Constructor y atributos: 2 puntos
  - Métodos básicos: 2 puntos
  - Validaciones: 1 punto
- **Clase Gerente**: 10 puntos
  - Herencia correcta: 3 puntos
  - Sobrescritura de métodos: 3 puntos
  - Métodos adicionales: 4 puntos

#### **Ejercicio 2: Programación Funcional (15 puntos)**
- **Funciones funcionales**: 10 puntos
  - `cuadrados_pares`: 3 puntos
  - `maximo_lista`: 3 puntos
  - `palabras_vocal`: 4 puntos
- **Comprensión de lista**: 5 puntos

#### **Ejercicio 3: Patrones de Diseño (10 puntos)**
- **Implementación Singleton**: 10 puntos
  - Una sola instancia: 4 puntos
  - Métodos de configuración: 4 puntos
  - Funcionamiento correcto: 2 puntos

#### **Problemas de Lógica (20 puntos)**
- **Diseño de Sistema**: 10 puntos
  - Clases y relaciones: 4 puntos
  - Principios SOLID: 3 puntos
  - Patrones de diseño: 3 puntos
- **Análisis de Código**: 10 puntos
  - Identificación de violación SOLID: 3 puntos
  - Propuesta de mejora: 4 puntos
  - Patrones aplicables: 3 puntos

---

## 🎯 **INTERPRETACIÓN DE RESULTADOS**

### **Puntuación por Nivel:**
- **90-100 puntos**: 🏆 **Excelente** - Dominio completo del tema
- **80-89 puntos**: 🥇 **Muy Bueno** - Comprensión sólida
- **70-79 puntos**: 🥈 **Bueno** - Comprensión satisfactoria
- **60-69 puntos**: 🥉 **Aceptable** - Necesita refuerzo
- **Menos de 60**: 📚 **Necesita Estudiar** - Requiere revisión completa

### **Áreas de Mejora por Sección:**
- **Sección 1 < 30 puntos**: Revisar conceptos teóricos fundamentales
- **Sección 2 < 30 puntos**: Practicar más implementación de código
- **Sección 3 < 15 puntos**: Trabajar en diseño y análisis de sistemas

---

## 💡 **RECOMENDACIONES POST-TEST**

### **Si obtuviste menos de 70 puntos:**
1. **Revisa** los conceptos teóricos marcados incorrectamente
2. **Practica** implementando los ejercicios de código
3. **Estudia** los principios SOLID y patrones de diseño
4. **Implementa** proyectos personales aplicando los conceptos

### **Si obtuviste 70-89 puntos:**
1. **Refuerza** las áreas con menor puntuación
2. **Practica** con ejercicios más avanzados
3. **Implementa** proyectos que combinen múltiples conceptos
4. **Participa** en code reviews para mejorar

### **Si obtuviste 90+ puntos:**
1. **Mantén** tu nivel con práctica constante
2. **Ayuda** a otros desarrolladores
3. **Contribuye** a proyectos open source
4. **Explora** conceptos más avanzados

---

*Este test debe realizarse después de completar todo el contenido del concepto para evaluar tu comprensión integral.* 