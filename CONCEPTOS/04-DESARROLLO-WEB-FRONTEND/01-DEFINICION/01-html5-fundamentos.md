# 📚 HTML5 Fundamentos: Estructura Semántica y Web Moderna

## 🎯 **OBJETIVO**
Comprender los fundamentos de HTML5, su estructura semántica, elementos de formularios avanzados, accesibilidad, multimedia y metadatos para crear páginas web modernas y accesibles.

---

## 📖 **¿QUÉ ES HTML5?**

### **Definición**
HTML5 (HyperText Markup Language 5) es la quinta versión del lenguaje de marcado estándar para crear páginas web. Es la evolución del HTML que introduce nuevas características semánticas, multimedia y de accesibilidad.

### **Características Principales**
- **Semántica mejorada**: Elementos con significado claro
- **Multimedia nativo**: `<video>`, `<audio>`, `<canvas>`
- **Formularios avanzados**: Validación HTML5, nuevos tipos de input
- **Accesibilidad integrada**: ARIA, roles, landmarks
- **Compatibilidad**: Funciona en navegadores modernos

---

## 🏗️ **ESTRUCTURA SEMÁNTICA HTML5**

### **Elementos de Estructura Principal**

#### **1. `<header>` - Encabezado de Sección**
```html
<header>
    <h1>Título Principal</h1>
    <nav>
        <ul>
            <li><a href="#inicio">Inicio</a></li>
            <li><a href="#servicios">Servicios</a></li>
            <li><a href="#contacto">Contacto</a></li>
        </ul>
    </nav>
</header>
```

#### **2. `<nav>` - Navegación**
```html
<nav aria-label="Navegación principal">
    <ul>
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#servicios">Servicios</a></li>
        <li><a href="#contacto">Contacto</a></li>
    </ul>
</nav>
```

#### **3. `<main>` - Contenido Principal**
```html
<main>
    <h1>Contenido Principal de la Página</h1>
    <p>Este es el contenido más importante de la página.</p>
</main>
```

#### **4. `<section>` - Sección de Contenido**
```html
<section>
    <h2>Nuestros Servicios</h2>
    <p>Descripción de los servicios ofrecidos.</p>
</section>
```

#### **5. `<article>` - Contenido Independiente**
```html
<article>
    <header>
        <h2>Título del Artículo</h2>
        <time datetime="2024-01-15">15 de Enero, 2024</time>
    </header>
    <p>Contenido del artículo...</p>
    <footer>
        <p>Autor: Nombre del Autor</p>
    </footer>
</article>
```

#### **6. `<aside>` - Contenido Relacionado**
```html
<aside>
    <h3>Artículos Relacionados</h3>
    <ul>
        <li><a href="#">Artículo 1</a></li>
        <li><a href="#">Artículo 2</a></li>
    </ul>
</aside>
```

#### **7. `<footer>` - Pie de Página**
```html
<footer>
    <p>&copy; 2024 Mi Empresa. Todos los derechos reservados.</p>
    <nav>
        <a href="/privacidad">Política de Privacidad</a>
        <a href="/terminos">Términos de Uso</a>
    </nav>
</footer>
```

### **Estructura Completa de una Página**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página Web</title>
</head>
<body>
    <header>
        <h1>Título Principal</h1>
        <nav>
            <!-- Navegación -->
        </nav>
    </header>
    
    <main>
        <section>
            <h2>Sección Principal</h2>
            <article>
                <h3>Artículo</h3>
                <p>Contenido...</p>
            </article>
        </section>
        
        <aside>
            <!-- Contenido relacionado -->
        </aside>
    </main>
    
    <footer>
        <!-- Pie de página -->
    </footer>
</body>
</html>
```

---

## 📝 **FORMULARIOS AVANZADOS HTML5**

### **Nuevos Tipos de Input**

#### **1. Inputs de Texto Especializados**
```html
<!-- Email -->
<input type="email" name="email" placeholder="tu@email.com" required>

<!-- URL -->
<input type="url" name="website" placeholder="https://ejemplo.com">

<!-- Teléfono -->
<input type="tel" name="phone" pattern="[0-9]{9}" placeholder="123456789">

<!-- Búsqueda -->
<input type="search" name="query" placeholder="Buscar...">

<!-- Contraseña -->
<input type="password" name="password" minlength="8" required>
```

#### **2. Inputs Numéricos**
```html
<!-- Número -->
<input type="number" name="age" min="18" max="100" step="1">

<!-- Rango -->
<input type="range" name="rating" min="1" max="5" value="3">

<!-- Fecha -->
<input type="date" name="birthdate">

<!-- Hora -->
<input type="time" name="meeting-time">

<!-- Fecha y hora -->
<input type="datetime-local" name="event-datetime">
```

#### **3. Inputs de Selección**
```html
<!-- Color -->
<input type="color" name="theme-color" value="#ff0000">

<!-- Archivo -->
<input type="file" name="document" accept=".pdf,.doc,.docx">

<!-- Checkbox -->
<input type="checkbox" name="newsletter" id="newsletter">
<label for="newsletter">Suscribirse al newsletter</label>

<!-- Radio -->
<input type="radio" name="gender" value="male" id="male">
<label for="male">Masculino</label>
<input type="radio" name="gender" value="female" id="female">
<label for="female">Femenino</label>
```

### **Validación HTML5**

#### **1. Atributos de Validación**
```html
<!-- Requerido -->
<input type="text" name="username" required>

<!-- Longitud mínima y máxima -->
<input type="text" name="description" minlength="10" maxlength="500">

<!-- Patrón personalizado -->
<input type="text" name="postal-code" pattern="[0-9]{5}" title="Código postal de 5 dígitos">

<!-- Valor mínimo y máximo -->
<input type="number" name="quantity" min="1" max="100">

<!-- Placeholder y título -->
<input type="text" name="search" placeholder="Buscar productos..." title="Ingresa tu búsqueda">
```

#### **2. Formulario Completo con Validación**
```html
<form action="/submit" method="POST" novalidate>
    <fieldset>
        <legend>Información Personal</legend>
        
        <div>
            <label for="name">Nombre completo:</label>
            <input type="text" id="name" name="name" required minlength="2" maxlength="50">
            <span class="error" id="name-error"></span>
        </div>
        
        <div>
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
            <span class="error" id="email-error"></span>
        </div>
        
        <div>
            <label for="phone">Teléfono:</label>
            <input type="tel" id="phone" name="phone" pattern="[0-9]{9}" title="9 dígitos">
            <span class="error" id="phone-error"></span>
        </div>
        
        <div>
            <label for="birthdate">Fecha de nacimiento:</label>
            <input type="date" id="birthdate" name="birthdate" max="2006-01-01">
            <span class="error" id="birthdate-error"></span>
        </div>
    </fieldset>
    
    <button type="submit">Enviar</button>
    <button type="reset">Limpiar</button>
</form>
```

---

## ♿ **ACCESIBILIDAD Y ARIA**

### **¿Qué es ARIA?**
ARIA (Accessible Rich Internet Applications) es un conjunto de atributos que mejoran la accesibilidad de las aplicaciones web para personas con discapacidades.

### **Roles ARIA Principales**

#### **1. Roles de Landmark**
```html
<!-- Navegación principal -->
<nav role="navigation" aria-label="Navegación principal">
    <!-- Enlaces de navegación -->
</nav>

<!-- Contenido principal -->
<main role="main">
    <!-- Contenido principal -->
</main>

<!-- Contenido complementario -->
<aside role="complementary">
    <!-- Contenido relacionado -->
</aside>

<!-- Información del sitio -->
<footer role="contentinfo">
    <!-- Información del sitio -->
</footer>
```

#### **2. Roles de Widget**
```html
<!-- Botón -->
<button role="button" aria-pressed="false">Toggle</button>

<!-- Checkbox -->
<div role="checkbox" aria-checked="false" tabindex="0">Acepto términos</div>

<!-- Radio button -->
<div role="radio" aria-checked="false" tabindex="0">Opción 1</div>

<!-- Slider -->
<div role="slider" aria-valuemin="0" aria-valuemax="100" aria-valuenow="50"></div>
```

#### **3. Estados y Propiedades ARIA**
```html
<!-- Expandible -->
<button aria-expanded="false" aria-controls="content">
    Mostrar contenido
</button>
<div id="content" aria-hidden="true">
    Contenido oculto
</div>

<!-- Live regions -->
<div aria-live="polite" aria-atomic="true">
    <!-- Contenido que se actualiza dinámicamente -->
</div>

<!-- Descriptions -->
<input type="text" aria-describedby="help-text">
<div id="help-text">Ingresa tu nombre completo</div>
```

### **Mejores Prácticas de Accesibilidad**

#### **1. Estructura Semántica**
```html
<!-- ✅ Correcto: Usar elementos semánticos -->
<main>
    <h1>Título Principal</h1>
    <section>
        <h2>Sección</h2>
        <p>Contenido...</p>
    </section>
</main>

<!-- ❌ Incorrecto: Usar divs genéricos -->
<div class="main">
    <div class="title">Título Principal</div>
    <div class="section">
        <div class="subtitle">Sección</div>
        <div class="content">Contenido...</div>
    </div>
</div>
```

#### **2. Etiquetas y Asociaciones**
```html
<!-- ✅ Correcto: Label asociado con input -->
<label for="username">Nombre de usuario:</label>
<input type="text" id="username" name="username">

<!-- ✅ Correcto: Label implícito -->
<label>
    Nombre de usuario:
    <input type="text" name="username">
</label>

<!-- ❌ Incorrecto: Sin label -->
<input type="text" name="username" placeholder="Usuario">
```

#### **3. Navegación por Teclado**
```html
<!-- ✅ Correcto: Elementos navegables por teclado -->
<button tabindex="0">Botón navegable</button>
<a href="#" tabindex="0">Enlace navegable</a>

<!-- ✅ Correcto: Saltar navegación -->
<a href="#main-content" class="skip-link">Saltar al contenido principal</a>
<main id="main-content">
    <!-- Contenido principal -->
</main>
```

---

## 🎥 **MULTIMEDIA HTML5**

### **Elemento `<video>`**

#### **1. Video Básico**
```html
<video width="640" height="360" controls>
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <p>Tu navegador no soporta el elemento video.</p>
</video>
```

#### **2. Video con Atributos Avanzados**
```html
<video 
    width="640" 
    height="360" 
    controls 
    preload="metadata"
    poster="thumbnail.jpg"
    autoplay
    muted
    loop>
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <track kind="subtitles" src="subtitles.vtt" srclang="es" label="Español">
    <p>Tu navegador no soporta el elemento video.</p>
</video>
```

### **Elemento `<audio>`**

#### **1. Audio Básico**
```html
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <p>Tu navegador no soporta el elemento audio.</p>
</audio>
```

#### **2. Audio con Atributos**
```html
<audio 
    controls 
    preload="auto"
    autoplay
    muted
    loop>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    <p>Tu navegador no soporta el elemento audio.</p>
</audio>
```

### **Elemento `<canvas>`**

#### **1. Canvas Básico**
```html
<canvas id="myCanvas" width="400" height="200">
    Tu navegador no soporta el elemento canvas.
</canvas>

<script>
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');
    
    // Dibujar un rectángulo
    ctx.fillStyle = '#ff0000';
    ctx.fillRect(10, 10, 100, 50);
    
    // Dibujar texto
    ctx.fillStyle = '#000000';
    ctx.font = '20px Arial';
    ctx.fillText('Hola Canvas!', 10, 100);
</script>
```

---

## 🔍 **METADATOS Y SEO**

### **Meta Tags Esenciales**

#### **1. Meta Tags Básicos**
```html
<head>
    <!-- Codificación de caracteres -->
    <meta charset="UTF-8">
    
    <!-- Viewport para responsive design -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Descripción para SEO -->
    <meta name="description" content="Descripción de tu página web para motores de búsqueda">
    
    <!-- Palabras clave -->
    <meta name="keywords" content="palabra1, palabra2, palabra3">
    
    <!-- Autor -->
    <meta name="author" content="Tu Nombre">
    
    <!-- Robots -->
    <meta name="robots" content="index, follow">
</head>
```

#### **2. Open Graph (Facebook)**
```html
<head>
    <!-- Open Graph -->
    <meta property="og:title" content="Título de la página">
    <meta property="og:description" content="Descripción de la página">
    <meta property="og:image" content="https://ejemplo.com/imagen.jpg">
    <meta property="og:url" content="https://ejemplo.com/pagina">
    <meta property="og:type" content="website">
    <meta property="og:site_name" content="Nombre del Sitio">
</head>
```

#### **3. Twitter Cards**
```html
<head>
    <!-- Twitter Cards -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="Título de la página">
    <meta name="twitter:description" content="Descripción de la página">
    <meta name="twitter:image" content="https://ejemplo.com/imagen.jpg">
    <meta name="twitter:site" content="@tucuenta">
    <meta name="twitter:creator" content="@tucuenta">
</head>
```

### **Estructura de Head Completa**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <!-- Meta tags básicos -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Descripción de la página">
    <meta name="keywords" content="palabras, clave">
    <meta name="author" content="Tu Nombre">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Título">
    <meta property="og:description" content="Descripción">
    <meta property="og:image" content="imagen.jpg">
    <meta property="og:url" content="https://ejemplo.com">
    
    <!-- Twitter Cards -->
    <meta name="twitter:card" content="summary">
    <meta name="twitter:title" content="Título">
    <meta name="twitter:description" content="Descripción">
    
    <!-- Favicon -->
    <link rel="icon" type="image/x-icon" href="/favicon.ico">
    
    <!-- CSS -->
    <link rel="stylesheet" href="styles.css">
    
    <title>Título de la Página</title>
</head>
<body>
    <!-- Contenido de la página -->
</body>
</html>
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Estructura Semántica**
- **Usa elementos HTML5** en lugar de divs genéricos
- **Mantén una jerarquía** clara de encabezados (h1, h2, h3...)
- **Agrupa contenido relacionado** en secciones lógicas
- **Usa landmarks** para navegación por lectores de pantalla

### **2. Formularios**
- **Siempre incluye labels** para todos los inputs
- **Usa validación HTML5** nativa cuando sea posible
- **Proporciona mensajes de error** claros y útiles
- **Considera la accesibilidad** en el diseño de formularios

### **3. Multimedia**
- **Proporciona alternativas** para contenido multimedia
- **Optimiza archivos** para web (compresión, formatos apropiados)
- **Usa preload** para contenido importante
- **Incluye subtítulos** para videos

### **4. SEO y Metadatos**
- **Escribe títulos únicos** y descriptivos para cada página
- **Incluye descripciones** meta atractivas
- **Optimiza para Open Graph** y Twitter Cards
- **Usa URLs semánticas** y descriptivas

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- **MDN Web Docs**: [HTML5 Reference](https://developer.mozilla.org/en-US/docs/Web/HTML)
- **W3C HTML5**: [Specification](https://www.w3.org/TR/html5/)
- **HTML Living Standard**: [WhatWG](https://html.spec.whatwg.org/)

### **Herramientas de Validación**
- **W3C Validator**: [validator.w3.org](https://validator.w3.org/)
- **HTML5 Outliner**: [gsnedders.html5.org](https://gsnedders.html5.org/outliner/)
- **Lighthouse**: Auditoría de accesibilidad y SEO

### **Práctica Online**
- **CodePen**: [codepen.io](https://codepen.io/)
- **JSFiddle**: [jsfiddle.net](https://jsfiddle.net/)
- **HTML5 Test**: [html5test.com](https://html5test.com/)

---

## 🎯 **EJERCICIOS PRÁCTICOS RECOMENDADOS**

1. **Crear una página de blog** con estructura semántica completa
2. **Implementar un formulario de contacto** con validación HTML5
3. **Crear una galería de imágenes** con elementos multimedia
4. **Desarrollar una página de portfolio** optimizada para SEO
5. **Implementar navegación accesible** con ARIA landmarks

---

*HTML5 es la base fundamental del desarrollo web moderno. Dominar su estructura semántica, formularios avanzados y características de accesibilidad te permitirá crear páginas web profesionales y accesibles.* 