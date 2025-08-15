# 🛠️ Ejercicios HTML5: Estructura Semántica y Formularios

## 🎯 **OBJETIVO**
Practicar y reforzar los conceptos de HTML5 aprendidos, implementando estructuras semánticas, formularios avanzados, accesibilidad y elementos multimedia.

---

## 📝 **EJERCICIO 1: ESTRUCTURA SEMÁNTICA DE BLOG**

### **Descripción**
Crea la estructura HTML5 semántica para un blog personal que incluya header, navegación, contenido principal, sidebar y footer.

### **Requisitos**
- Usa elementos semánticos HTML5 (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`)
- Implementa navegación accesible con ARIA labels
- Incluye metadatos para SEO
- Estructura jerárquica de encabezados correcta

### **Código Base**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <!-- Meta tags para SEO -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Blog personal sobre tecnología y desarrollo web">
    <meta name="keywords" content="tecnología, desarrollo web, programación, blog">
    <meta name="author" content="Tu Nombre">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Mi Blog Personal">
    <meta property="og:description" content="Blog sobre tecnología y desarrollo web">
    <meta property="og:type" content="website">
    
    <title>Mi Blog Personal - Tecnología y Desarrollo Web</title>
</head>
<body>
    <!-- Tu código aquí -->
</body>
</html>
```

### **Solución Esperada**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Blog personal sobre tecnología y desarrollo web">
    <meta name="keywords" content="tecnología, desarrollo web, programación, blog">
    <meta name="author" content="Tu Nombre">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Mi Blog Personal">
    <meta property="og:description" content="Blog sobre tecnología y desarrollo web">
    <meta property="og:type" content="website">
    
    <title>Mi Blog Personal - Tecnología y Desarrollo Web</title>
</head>
<body>
    <!-- Header con navegación -->
    <header>
        <h1>Mi Blog Personal</h1>
        <nav aria-label="Navegación principal">
            <ul>
                <li><a href="#inicio">Inicio</a></li>
                <li><a href="#articulos">Artículos</a></li>
                <li><a href="#sobre-mi">Sobre Mí</a></li>
                <li><a href="#contacto">Contacto</a></li>
            </ul>
        </nav>
    </header>
    
    <!-- Contenido principal -->
    <main>
        <!-- Sección de artículos -->
        <section id="articulos">
            <h2>Artículos Recientes</h2>
            
            <article>
                <header>
                    <h3>Introducción a HTML5 Semántico</h3>
                    <time datetime="2024-01-15">15 de Enero, 2024</time>
                    <p class="author">Por <a href="#autor">Tu Nombre</a></p>
                </header>
                
                <p>HTML5 semántico nos permite crear páginas web más accesibles y mantenibles...</p>
                
                <footer>
                    <a href="#leer-mas">Leer más</a>
                    <span class="tags">Tags: HTML5, Semántica, Accesibilidad</span>
                </footer>
            </article>
            
            <article>
                <header>
                    <h3>CSS Grid vs Flexbox</h3>
                    <time datetime="2024-01-10">10 de Enero, 2024</time>
                    <p class="author">Por <a href="#autor">Tu Nombre</a></p>
                </header>
                
                <p>Comparación entre CSS Grid y Flexbox para layouts modernos...</p>
                
                <footer>
                    <a href="#leer-mas">Leer más</a>
                    <span class="tags">Tags: CSS, Grid, Flexbox</span>
                </footer>
            </article>
        </section>
        
        <!-- Sidebar -->
        <aside>
            <h3>Artículos Populares</h3>
            <ul>
                <li><a href="#art1">JavaScript Moderno</a></li>
                <li><a href="#art2">React.js Básico</a></li>
                <li><a href="#art3">Optimización Web</a></li>
            </ul>
            
            <h3>Categorías</h3>
            <ul>
                <li><a href="#cat1">Frontend</a></li>
                <li><a href="#cat2">Backend</a></li>
                <li><a href="#cat3">DevOps</a></li>
            </ul>
        </aside>
    </main>
    
    <!-- Footer -->
    <footer>
        <p>&copy; 2024 Mi Blog Personal. Todos los derechos reservados.</p>
        <nav aria-label="Enlaces del footer">
            <a href="/privacidad">Política de Privacidad</a>
            <a href="/terminos">Términos de Uso</a>
        </nav>
    </footer>
</body>
</html>
```

---

## 📝 **EJERCICIO 2: FORMULARIO DE CONTACTO AVANZADO**

### **Descripción**
Crea un formulario de contacto completo con validación HTML5, campos especializados y accesibilidad.

### **Requisitos**
- Usa tipos de input HTML5 apropiados
- Implementa validación nativa HTML5
- Incluye campos para nombre, email, teléfono, asunto, mensaje
- Agrega un campo de archivo para adjuntos
- Implementa accesibilidad completa

### **Código Base**
```html
<form class="contact-form">
    <!-- Tu código aquí -->
</form>
```

### **Solución Esperada**
```html
<form class="contact-form" action="/submit-contact" method="POST" enctype="multipart/form-data" novalidate>
    <fieldset>
        <legend>Información de Contacto</legend>
        
        <div class="form-group">
            <label for="name">Nombre completo *</label>
            <input 
                type="text" 
                id="name" 
                name="name" 
                required 
                minlength="2" 
                maxlength="50"
                placeholder="Tu nombre completo"
                aria-describedby="name-help"
            >
            <div id="name-help" class="help-text">
                Ingresa tu nombre completo (mínimo 2 caracteres)
            </div>
        </div>
        
        <div class="form-group">
            <label for="email">Email *</label>
            <input 
                type="email" 
                id="email" 
                name="email" 
                required
                placeholder="tu@email.com"
                aria-describedby="email-help"
            >
            <div id="email-help" class="help-text">
                Ingresa un email válido
            </div>
        </div>
        
        <div class="form-group">
            <label for="phone">Teléfono</label>
            <input 
                type="tel" 
                id="phone" 
                name="phone"
                pattern="[0-9]{9}"
                placeholder="123456789"
                aria-describedby="phone-help"
            >
            <div id="phone-help" class="help-text">
                Formato: 9 dígitos sin espacios
            </div>
        </div>
        
        <div class="form-group">
            <label for="subject">Asunto *</label>
            <select id="subject" name="subject" required>
                <option value="">Selecciona un asunto</option>
                <option value="consulta">Consulta general</option>
                <option value="soporte">Soporte técnico</option>
                <option value="colaboracion">Colaboración</option>
                <option value="otro">Otro</option>
            </select>
        </div>
        
        <div class="form-group">
            <label for="priority">Prioridad</label>
            <input 
                type="range" 
                id="priority" 
                name="priority" 
                min="1" 
                max="5" 
                value="3"
                aria-describedby="priority-help"
            >
            <div id="priority-help" class="help-text">
                Prioridad: <span id="priority-value">3</span>/5
            </div>
        </div>
        
        <div class="form-group">
            <label for="message">Mensaje *</label>
            <textarea 
                id="message" 
                name="message" 
                required 
                minlength="10" 
                maxlength="1000"
                rows="5"
                placeholder="Escribe tu mensaje aquí..."
                aria-describedby="message-help"
            ></textarea>
            <div id="message-help" class="help-text">
                Mínimo 10 caracteres, máximo 1000
            </div>
        </div>
        
        <div class="form-group">
            <label for="attachment">Archivo adjunto</label>
            <input 
                type="file" 
                id="attachment" 
                name="attachment"
                accept=".pdf,.doc,.docx,.txt,.jpg,.png"
                aria-describedby="attachment-help"
            >
            <div id="attachment-help" class="help-text">
                Formatos permitidos: PDF, DOC, DOCX, TXT, JPG, PNG (máx. 5MB)
            </div>
        </div>
        
        <div class="form-group">
            <label>
                <input type="checkbox" name="newsletter" value="yes">
                Suscribirme al newsletter
            </label>
        </div>
        
        <div class="form-group">
            <label>
                <input type="checkbox" name="terms" value="accepted" required>
                Acepto los <a href="/terminos" target="_blank">términos y condiciones</a> *
            </label>
        </div>
    </fieldset>
    
    <div class="form-actions">
        <button type="submit">Enviar Mensaje</button>
        <button type="reset">Limpiar Formulario</button>
    </div>
</form>

<script>
// Actualizar valor de prioridad en tiempo real
document.getElementById('priority').addEventListener('input', function() {
    document.getElementById('priority-value').textContent = this.value;
});
</script>
```

---

## 📝 **EJERCICIO 3: GALERÍA MULTIMEDIA RESPONSIVE**

### **Descripción**
Crea una galería multimedia que incluya imágenes, videos y audio con elementos HTML5 nativos.

### **Requisitos**
- Usa elementos `<figure>` y `<figcaption>`
- Implementa `<video>` y `<audio>` con controles
- Incluye atributos de accesibilidad
- Estructura semántica para galería

### **Código Base**
```html
<section class="multimedia-gallery">
    <!-- Tu código aquí -->
</section>
```

### **Solución Esperada**
```html
<section class="multimedia-gallery" aria-labelledby="gallery-title">
    <h2 id="gallery-title">Galería Multimedia</h2>
    
    <div class="gallery-grid">
        <!-- Imagen con figura -->
        <figure class="gallery-item">
            <img 
                src="images/landscape.jpg" 
                alt="Paisaje montañoso con lago al atardecer"
                loading="lazy"
                width="400"
                height="300"
            >
            <figcaption>
                <h3>Paisaje Montañoso</h3>
                <p>Hermoso paisaje natural capturado al atardecer</p>
                <time datetime="2024-01-15">15 de Enero, 2024</time>
            </figcaption>
        </figure>
        
        <!-- Video con figura -->
        <figure class="gallery-item">
            <video 
                controls 
                preload="metadata"
                width="400"
                height="300"
                poster="images/video-poster.jpg"
                aria-describedby="video-desc"
            >
                <source src="videos/demo.mp4" type="video/mp4">
                <source src="videos/demo.webm" type="video/webm">
                <track 
                    kind="subtitles" 
                    src="videos/demo.vtt" 
                    srclang="es" 
                    label="Español"
                >
                Tu navegador no soporta el elemento video.
            </video>
            <figcaption>
                <h3>Demo de Producto</h3>
                <p id="video-desc">Video demostrativo de las características principales</p>
                <time datetime="2024-01-10">10 de Enero, 2024</time>
            </figcaption>
        </figure>
        
        <!-- Audio con figura -->
        <figure class="gallery-item">
            <audio 
                controls
                preload="metadata"
                aria-describedby="audio-desc"
            >
                <source src="audio/podcast.mp3" type="audio/mpeg">
                <source src="audio/podcast.ogg" type="audio/ogg">
                Tu navegador no soporta el elemento audio.
            </audio>
            <figcaption>
                <h3>Podcast Semanal</h3>
                <p id="audio-desc">Episodio sobre las últimas tendencias en tecnología</p>
                <time datetime="2024-01-12">12 de Enero, 2024</time>
            </figcaption>
        </figure>
        
        <!-- Imagen con mapa de áreas -->
        <figure class="gallery-item">
            <img 
                src="images/world-map.jpg" 
                alt="Mapa del mundo interactivo"
                usemap="#world-map"
                loading="lazy"
                width="400"
                height="300"
            >
            <map name="world-map">
                <area 
                    shape="rect" 
                    coords="0,0,100,100" 
                    href="#europa" 
                    alt="Europa"
                    title="Haz click para ver información de Europa"
                >
                <area 
                    shape="rect" 
                    coords="100,0,200,100" 
                    href="#asia" 
                    alt="Asia"
                    title="Haz click para ver información de Asia"
                >
                <area 
                    shape="rect" 
                    coords="0,100,100,200" 
                    href="#africa" 
                    alt="África"
                    title="Haz click para ver información de África"
                >
            </map>
            <figcaption>
                <h3>Mapa Interactivo</h3>
                <p>Haz click en las regiones para obtener más información</p>
            </figcaption>
        </figure>
    </div>
    
    <!-- Navegación de galería -->
    <nav class="gallery-nav" aria-label="Navegación de galería">
        <button type="button" aria-label="Anterior">← Anterior</button>
        <span class="gallery-counter">1 de 4</span>
        <button type="button" aria-label="Siguiente">Siguiente →</button>
    </nav>
</section>
```

---

## 📝 **EJERCICIO 4: TABLA DE DATOS ACCESIBLE**

### **Descripción**
Crea una tabla de datos compleja con encabezados, agrupación y accesibilidad completa.

### **Requisitos**
- Usa `<table>`, `<thead>`, `<tbody>`, `<tfoot>`
- Implementa `<caption>` y `<colgroup>`
- Agrega atributos de accesibilidad
- Incluye agrupación de datos

### **Código Base**
```html
<table class="data-table">
    <!-- Tu código aquí -->
</table>
```

### **Solución Esperada**
```html
<table class="data-table" aria-labelledby="table-title">
    <caption id="table-title">Estadísticas de Ventas por Región - 2024</caption>
    
    <colgroup>
        <col style="width: 20%">
        <col style="width: 15%">
        <col style="width: 15%">
        <col style="width: 15%">
        <col style="width: 15%">
        <col style="width: 20%">
    </colgroup>
    
    <thead>
        <tr>
            <th scope="col" rowspan="2">Región</th>
            <th scope="col" rowspan="2">Vendedor</th>
            <th scope="col" colspan="3">Ventas por Trimestre</th>
            <th scope="col" rowspan="2">Total Anual</th>
        </tr>
        <tr>
            <th scope="col">Q1</th>
            <th scope="col">Q2</th>
            <th scope="col">Q3</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <th scope="row" rowspan="3">Norte</th>
            <td>Ana García</td>
            <td>$15,000</td>
            <td>$18,000</td>
            <td>$22,000</td>
            <td>$55,000</td>
        </tr>
        <tr>
            <td>Carlos López</td>
            <td>$12,000</td>
            <td>$14,000</td>
            <td>$16,000</td>
            <td>$42,000</td>
        </tr>
        <tr>
            <td>María Rodríguez</td>
            <td>$20,000</td>
            <td>$25,000</td>
            <td>$28,000</td>
            <td>$73,000</td>
        </tr>
        
        <tr>
            <th scope="row" rowspan="2">Sur</th>
            <td>Juan Pérez</td>
            <td>$18,000</td>
            <td>$21,000</td>
            <td>$24,000</td>
            <td>$63,000</td>
        </tr>
        <tr>
            <td>Laura Martínez</td>
            <td>$16,000</td>
            <td>$19,000</td>
            <td>$23,000</td>
            <td>$58,000</td>
        </tr>
        
        <tr>
            <th scope="row">Este</th>
            <td>Roberto Silva</td>
            <td>$22,000</td>
            <td>$26,000</td>
            <td>$30,000</td>
            <td>$78,000</td>
        </tr>
    </tbody>
    
    <tfoot>
        <tr>
            <th scope="row" colspan="2">Total General</th>
            <td>$103,000</td>
            <td>$123,000</td>
            <td>$143,000</td>
            <td><strong>$369,000</strong></td>
        </tr>
        <tr>
            <th scope="row" colspan="2">Promedio por Vendedor</th>
            <td>$17,167</td>
            <td>$20,500</td>
            <td>$23,833</td>
            <td><strong>$61,500</strong></td>
        </tr>
    </tfoot>
</table>
```

---

## 📝 **EJERCICIO 5: FORMULARIO DE REGISTRO CON VALIDACIÓN**

### **Descripción**
Crea un formulario de registro de usuario con validación HTML5 avanzada y campos especializados.

### **Requisitos**
- Campos para información personal y credenciales
- Validación HTML5 nativa
- Campos de fecha, color, rango
- Accesibilidad completa
- Mensajes de ayuda contextual

### **Código Base**
```html
<form class="registration-form">
    <!-- Tu código aquí -->
</form>
```

### **Solución Esperada**
```html
<form class="registration-form" action="/register" method="POST" novalidate>
    <h2>Registro de Usuario</h2>
    
    <!-- Información Personal -->
    <fieldset>
        <legend>Información Personal</legend>
        
        <div class="form-group">
            <label for="firstName">Nombre *</label>
            <input 
                type="text" 
                id="firstName" 
                name="firstName" 
                required 
                minlength="2" 
                maxlength="30"
                pattern="[A-Za-záéíóúÁÉÍÓÚñÑ\s]+"
                placeholder="Tu nombre"
                aria-describedby="firstName-help"
            >
            <div id="firstName-help" class="help-text">
                Solo letras y espacios, mínimo 2 caracteres
            </div>
        </div>
        
        <div class="form-group">
            <label for="lastName">Apellidos *</label>
            <input 
                type="text" 
                id="lastName" 
                name="lastName" 
                required 
                minlength="2" 
                maxlength="50"
                pattern="[A-Za-záéíóúÁÉÍÓÚñÑ\s]+"
                placeholder="Tus apellidos"
                aria-describedby="lastName-help"
            >
            <div id="lastName-help" class="help-text">
                Solo letras y espacios, mínimo 2 caracteres
            </div>
        </div>
        
        <div class="form-group">
            <label for="birthDate">Fecha de Nacimiento *</label>
            <input 
                type="date" 
                id="birthDate" 
                name="birthDate" 
                required
                max="2006-01-01"
                aria-describedby="birthDate-help"
            >
            <div id="birthDate-help" class="help-text">
                Debes ser mayor de 18 años
            </div>
        </div>
        
        <div class="form-group">
            <label for="phone">Teléfono</label>
            <input 
                type="tel" 
                id="phone" 
                name="phone"
                pattern="[0-9]{9}"
                placeholder="123456789"
                aria-describedby="phone-help"
            >
            <div id="phone-help" class="help-text">
                9 dígitos sin espacios ni guiones
            </div>
        </div>
        
        <div class="form-group">
            <label for="website">Sitio Web</label>
            <input 
                type="url" 
                id="website" 
                name="website"
                placeholder="https://ejemplo.com"
                aria-describedby="website-help"
            >
            <div id="website-help" class="help-text">
                Incluye http:// o https://
            </div>
        </div>
    </fieldset>
    
    <!-- Credenciales -->
    <fieldset>
        <legend>Credenciales de Acceso</legend>
        
        <div class="form-group">
            <label for="email">Email *</label>
            <input 
                type="email" 
                id="email" 
                name="email" 
                required
                placeholder="tu@email.com"
                aria-describedby="email-help"
            >
            <div id="email-help" class="help-text">
                Usa un email válido y activo
            </div>
        </div>
        
        <div class="form-group">
            <label for="password">Contraseña *</label>
            <input 
                type="password" 
                id="password" 
                name="password" 
                required
                minlength="8"
                pattern="^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]"
                aria-describedby="password-help"
            >
            <div id="password-help" class="help-text">
                Mínimo 8 caracteres, incluye mayúscula, minúscula, número y símbolo
            </div>
        </div>
        
        <div class="form-group">
            <label for="confirmPassword">Confirmar Contraseña *</label>
            <input 
                type="password" 
                id="confirmPassword" 
                name="confirmPassword" 
                required
                aria-describedby="confirmPassword-help"
            >
            <div id="confirmPassword-help" class="help-text">
                Debe coincidir con la contraseña anterior
            </div>
        </div>
    </fieldset>
    
    <!-- Preferencias -->
    <fieldset>
        <legend>Preferencias</legend>
        
        <div class="form-group">
            <label for="themeColor">Color del Tema</label>
            <input 
                type="color" 
                id="themeColor" 
                name="themeColor"
                value="#007bff"
                aria-describedby="themeColor-help"
            >
            <div id="themeColor-help" class="help-text">
                Selecciona tu color preferido para la interfaz
            </div>
        </div>
        
        <div class="form-group">
            <label for="notifications">Frecuencia de Notificaciones</label>
            <input 
                type="range" 
                id="notifications" 
                name="notifications"
                min="0" 
                max="3" 
                value="1"
                step="1"
                aria-describedby="notifications-help"
            >
            <div id="notifications-help" class="help-text">
                <span id="notifications-value">Ocasional</span> 
                (0: Nunca, 1: Ocasional, 2: Frecuente, 3: Siempre)
            </div>
        </div>
        
        <div class="form-group">
            <label for="timezone">Zona Horaria</label>
            <select id="timezone" name="timezone">
                <option value="">Selecciona tu zona horaria</option>
                <optgroup label="España">
                    <option value="Europe/Madrid">Madrid (GMT+1)</option>
                    <option value="Europe/Las_Palmas">Canarias (GMT+0)</option>
                </optgroup>
                <optgroup label="América">
                    <option value="America/New_York">Nueva York (GMT-5)</option>
                    <option value="America/Los_Angeles">Los Ángeles (GMT-8)</option>
                </optgroup>
            </select>
        </div>
    </fieldset>
    
    <!-- Términos -->
    <fieldset>
        <legend>Términos y Condiciones</legend>
        
        <div class="form-group">
            <label>
                <input 
                    type="checkbox" 
                    name="terms" 
                    value="accepted" 
                    required
                    aria-describedby="terms-help"
                >
                Acepto los <a href="/terminos" target="_blank">términos y condiciones</a> *
            </label>
            <div id="terms-help" class="help-text">
                Debes aceptar los términos para continuar
            </div>
        </div>
        
        <div class="form-group">
            <label>
                <input 
                    type="checkbox" 
                    name="marketing" 
                    value="accepted"
                >
                Acepto recibir comunicaciones de marketing
            </label>
        </div>
    </fieldset>
    
    <!-- Botones -->
    <div class="form-actions">
        <button type="submit">Crear Cuenta</button>
        <button type="reset">Limpiar Formulario</button>
        <button type="button" onclick="window.history.back()">Cancelar</button>
    </div>
</form>

<script>
// Actualizar valor de notificaciones en tiempo real
document.getElementById('notifications').addEventListener('input', function() {
    const values = ['Nunca', 'Ocasional', 'Frecuente', 'Siempre'];
    document.getElementById('notifications-value').textContent = values[this.value];
});

// Validar confirmación de contraseña
document.getElementById('confirmPassword').addEventListener('input', function() {
    const password = document.getElementById('password').value;
    const confirmPassword = this.value;
    
    if (password !== confirmPassword) {
        this.setCustomValidity('Las contraseñas no coinciden');
    } else {
        this.setCustomValidity('');
    }
});
</script>
```

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 6: Canvas Interactivo**
Crea un elemento `<canvas>` con JavaScript que dibuje formas geométricas básicas y responda a eventos del mouse.

### **Ejercicio 7: Formulario de Búsqueda Avanzada**
Implementa un formulario de búsqueda con filtros múltiples, búsqueda por texto y opciones de ordenamiento.

### **Ejercicio 8: Galería de Imágenes con Lightbox**
Crea una galería de imágenes que se abra en un lightbox modal al hacer click, usando solo HTML5 y CSS.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Estructura Semántica (25%)**
- Uso correcto de elementos HTML5 semánticos
- Jerarquía de encabezados apropiada
- Estructura lógica y organizada

### **Formularios y Validación (30%)**
- Tipos de input apropiados
- Validación HTML5 nativa
- Mensajes de ayuda y error

### **Accesibilidad (25%)**
- Labels y asociaciones correctas
- Atributos ARIA apropiados
- Navegación por teclado

### **Multimedia y Contenido (20%)**
- Elementos multimedia apropiados
- Atributos de accesibilidad
- Optimización de contenido

---

## 💡 **CONSEJOS PARA LA PRÁCTICA**

1. **Empieza simple**: Construye la estructura básica primero
2. **Valida tu HTML**: Usa el validador W3C
3. **Testea accesibilidad**: Usa lectores de pantalla o herramientas de testing
4. **Optimiza para SEO**: Incluye metadatos apropiados
5. **Practica responsive**: Testea en diferentes tamaños de pantalla

---

*Estos ejercicios te ayudarán a dominar HTML5 y crear páginas web semánticas, accesibles y optimizadas para SEO.* 