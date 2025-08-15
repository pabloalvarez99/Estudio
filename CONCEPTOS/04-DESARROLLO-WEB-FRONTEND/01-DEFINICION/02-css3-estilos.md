# 🎨 CSS3 Estilos: Layout Moderno y Diseño Responsive

## 🎯 **OBJETIVO**
Dominar CSS3 avanzado, incluyendo Flexbox, CSS Grid, animaciones, diseño responsive, variables CSS y mejores prácticas para crear interfaces modernas y profesionales.

---

## 📖 **¿QUÉ ES CSS3?**

### **Definición**
CSS3 (Cascading Style Sheets 3) es la tercera versión del lenguaje de estilos CSS que introduce nuevas características para layout, animaciones, transiciones y diseño responsive.

### **Características Principales**
- **Layout moderno**: Flexbox y CSS Grid
- **Animaciones**: Transiciones, keyframes, transformaciones
- **Responsive design**: Media queries, mobile-first
- **Variables CSS**: Custom properties
- **Selectores avanzados**: Pseudo-clases y pseudo-elementos

---

## 🏗️ **LAYOUT MODERNO: FLEXBOX**

### **Conceptos Fundamentales de Flexbox**

#### **1. Contenedor Flex (Flex Container)**
```css
.flex-container {
    display: flex; /* o inline-flex */
    flex-direction: row; /* row | row-reverse | column | column-reverse */
    flex-wrap: wrap; /* nowrap | wrap | wrap-reverse */
    justify-content: center; /* flex-start | flex-end | center | space-between | space-around | space-evenly */
    align-items: center; /* stretch | flex-start | flex-end | center | baseline */
    align-content: space-between; /* stretch | flex-start | flex-end | center | space-between | space-around */
}
```

#### **2. Elementos Flex (Flex Items)**
```css
.flex-item {
    flex: 1 1 auto; /* shorthand: flex-grow flex-shrink flex-basis */
    flex-grow: 1; /* Factor de crecimiento */
    flex-shrink: 1; /* Factor de contracción */
    flex-basis: auto; /* Tamaño base */
    align-self: center; /* Alineación individual */
    order: 1; /* Orden de visualización */
}
```

### **Ejemplos Prácticos de Flexbox**

#### **1. Navegación Horizontal**
```css
.nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background-color: #333;
}

.nav-links {
    display: flex;
    gap: 2rem;
    list-style: none;
}

.nav-links a {
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.nav-links a:hover {
    background-color: #555;
}
```

#### **2. Tarjetas Responsive**
```css
.cards-container {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    padding: 2rem;
}

.card {
    flex: 1 1 300px; /* Mínimo 300px, crece y se contrae */
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    padding: 1.5rem;
    transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 20px rgba(0,0,0,0.15);
}
```

#### **3. Layout de Página Completo**
```css
.page-layout {
    display: flex;
    min-height: 100vh;
}

.sidebar {
    flex: 0 0 250px; /* No crece, no se contrae, 250px fijo */
    background: #f5f5f5;
    padding: 2rem;
}

.main-content {
    flex: 1; /* Toma el espacio restante */
    padding: 2rem;
}
```

---

## 🎯 **LAYOUT AVANZADO: CSS GRID**

### **Conceptos Fundamentales de Grid**

#### **1. Contenedor Grid**
```css
.grid-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* 3 columnas de igual tamaño */
    grid-template-rows: auto 1fr auto; /* Filas automáticas */
    grid-gap: 2rem; /* Espacio entre elementos */
    grid-template-areas: 
        "header header header"
        "sidebar main aside"
        "footer footer footer";
}
```

#### **2. Definición de Áreas**
```css
.header {
    grid-area: header;
    background: #333;
    color: white;
    padding: 1rem;
}

.sidebar {
    grid-area: sidebar;
    background: #f5f5f5;
    padding: 1rem;
}

.main {
    grid-area: main;
    padding: 1rem;
}

.aside {
    grid-area: aside;
    background: #f0f0f0;
    padding: 1rem;
}

.footer {
    grid-area: footer;
    background: #333;
    color: white;
    padding: 1rem;
}
```

### **Ejemplos Prácticos de Grid**

#### **1. Galería de Imágenes**
```css
.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    grid-auto-rows: 250px;
    gap: 1rem;
    padding: 2rem;
}

.gallery-item {
    overflow: hidden;
    border-radius: 8px;
    position: relative;
}

.gallery-item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s;
}

.gallery-item:hover img {
    transform: scale(1.1);
}
```

#### **2. Dashboard Responsive**
```css
.dashboard {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-auto-rows: minmax(200px, auto);
    gap: 1.5rem;
    padding: 2rem;
}

.widget {
    background: white;
    border-radius: 8px;
    padding: 1.5rem;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.widget.large {
    grid-column: span 2;
    grid-row: span 2;
}

.widget.wide {
    grid-column: span 2;
}
```

---

## 🎭 **ANIMACIONES Y TRANSICIONES**

### **Transiciones CSS**

#### **1. Transiciones Básicas**
```css
.button {
    background: #007bff;
    color: white;
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s ease;
}

.button:hover {
    background: #0056b3;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,123,255,0.3);
}
```

#### **2. Transiciones Múltiples**
```css
.card {
    background: white;
    padding: 1rem;
    border-radius: 8px;
    transition: 
        transform 0.3s ease,
        box-shadow 0.3s ease,
        background-color 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,0,0,0.15);
    background: #f8f9fa;
}
```

### **Animaciones con Keyframes**

#### **1. Animación de Entrada**
```css
@keyframes slideInFromLeft {
    0% {
        transform: translateX(-100%);
        opacity: 0;
    }
    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

.slide-in {
    animation: slideInFromLeft 0.8s ease-out;
}
```

#### **2. Animación de Loading**
```css
@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}

.loading-spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #3498db;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}
```

#### **3. Animación de Fade In**
```css
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.fade-in {
    animation: fadeIn 0.6s ease-out;
}

/* Animación con delay para elementos múltiples */
.fade-in:nth-child(1) { animation-delay: 0.1s; }
.fade-in:nth-child(2) { animation-delay: 0.2s; }
.fade-in:nth-child(3) { animation-delay: 0.3s; }
```

### **Transformaciones CSS**

#### **1. Transformaciones 2D**
```css
.transform-examples {
    /* Rotación */
    transform: rotate(45deg);
    
    /* Escala */
    transform: scale(1.2);
    
    /* Traslación */
    transform: translate(20px, 10px);
    
    /* Sesgado */
    transform: skew(10deg, 5deg);
    
    /* Múltiples transformaciones */
    transform: rotate(45deg) scale(1.2) translate(20px, 10px);
}
```

#### **2. Transformaciones 3D**
```css
.card-3d {
    perspective: 1000px;
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.card-3d:hover {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
}

.card-back {
    transform: rotateY(180deg);
}
```

---

## 📱 **DISEÑO RESPONSIVE**

### **Media Queries**

#### **1. Breakpoints Comunes**
```css
/* Mobile First - Base styles for mobile */
.container {
    padding: 1rem;
    margin: 0 auto;
    max-width: 100%;
}

/* Tablet */
@media (min-width: 768px) {
    .container {
        padding: 2rem;
        max-width: 750px;
    }
}

/* Desktop */
@media (min-width: 1024px) {
    .container {
        padding: 3rem;
        max-width: 1000px;
    }
}

/* Large Desktop */
@media (min-width: 1200px) {
    .container {
        max-width: 1200px;
    }
}
```

#### **2. Orientación y Características**
```css
/* Orientación landscape */
@media (orientation: landscape) {
    .header {
        height: 60px;
    }
}

/* Orientación portrait */
@media (orientation: portrait) {
    .header {
        height: 80px;
    }
}

/* Pantalla de alta resolución */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
    .logo {
        background-image: url('logo@2x.png');
    }
}

/* Modo oscuro del sistema */
@media (prefers-color-scheme: dark) {
    body {
        background: #1a1a1a;
        color: #ffffff;
    }
}
```

### **Grid Responsive**

#### **1. Grid Adaptativo**
```css
.responsive-grid {
    display: grid;
    gap: 1rem;
    padding: 1rem;
}

/* Mobile: 1 columna */
@media (max-width: 767px) {
    .responsive-grid {
        grid-template-columns: 1fr;
    }
}

/* Tablet: 2 columnas */
@media (min-width: 768px) and (max-width: 1023px) {
    .responsive-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

/* Desktop: 3 columnas */
@media (min-width: 1024px) {
    .responsive-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}
```

#### **2. Flexbox Responsive**
```css
.flex-responsive {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.flex-item {
    flex: 1 1 100%; /* Mobile: 100% */
}

@media (min-width: 768px) {
    .flex-item {
        flex: 1 1 calc(50% - 0.5rem); /* Tablet: 50% */
    }
}

@media (min-width: 1024px) {
    .flex-item {
        flex: 1 1 calc(33.333% - 0.667rem); /* Desktop: 33.333% */
    }
}
```

---

## 🎨 **VARIABLES CSS (CUSTOM PROPERTIES)**

### **Definición y Uso**

#### **1. Variables Globales**
```css
:root {
    /* Colores */
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --success-color: #28a745;
    --danger-color: #dc3545;
    --warning-color: #ffc107;
    --info-color: #17a2b8;
    
    /* Tipografía */
    --font-family-base: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    --font-size-base: 16px;
    --font-size-lg: 1.25rem;
    --font-size-sm: 0.875rem;
    
    /* Espaciado */
    --spacing-unit: 1rem;
    --spacing-sm: calc(var(--spacing-unit) * 0.5);
    --spacing-md: var(--spacing-unit);
    --spacing-lg: calc(var(--spacing-unit) * 1.5);
    --spacing-xl: calc(var(--spacing-unit) * 2);
    
    /* Sombras */
    --shadow-sm: 0 2px 4px rgba(0,0,0,0.1);
    --shadow-md: 0 4px 8px rgba(0,0,0,0.15);
    --shadow-lg: 0 8px 16px rgba(0,0,0,0.2);
    
    /* Bordes */
    --border-radius: 4px;
    --border-radius-lg: 8px;
    --border-radius-sm: 2px;
}
```

#### **2. Uso de Variables**
```css
.button {
    background: var(--primary-color);
    color: white;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    font-family: var(--font-family-base);
    font-size: var(--font-size-base);
    box-shadow: var(--shadow-sm);
    transition: all 0.3s ease;
}

.button:hover {
    background: var(--danger-color);
    box-shadow: var(--shadow-md);
    transform: translateY(-2px);
}

.card {
    background: white;
    padding: var(--spacing-md);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow-sm);
    margin-bottom: var(--spacing-md);
}
```

#### **3. Variables Locales**
```css
.card {
    --card-bg: #ffffff;
    --card-border: #e9ecef;
    --card-shadow: var(--shadow-sm);
    
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    box-shadow: var(--card-shadow);
}

.card.dark {
    --card-bg: #343a40;
    --card-border: #495057;
    --card-shadow: var(--shadow-md);
}
```

---

## 🎯 **MEJORES PRÁCTICAS**

### **1. Organización del Código**
- **Usa metodologías** como BEM, SMACSS o ITCSS
- **Organiza por componentes** en lugar de por propiedades
- **Mantén especificidad baja** para facilitar mantenimiento
- **Usa variables CSS** para valores reutilizables

### **2. Performance**
- **Minimiza el uso de selectores** complejos
- **Evita animaciones** en propiedades que causan reflow
- **Usa transform y opacity** para animaciones
- **Optimiza media queries** para evitar duplicación

### **3. Accesibilidad**
- **Mantén contraste** adecuado entre colores
- **No dependas solo del color** para transmitir información
- **Usa focus visible** para navegación por teclado
- **Considera usuarios** con preferencias de movimiento reducido

### **4. Compatibilidad**
- **Usa fallbacks** para navegadores antiguos
- **Progressive enhancement** para nuevas características
- **Testea** en múltiples navegadores
- **Usa autoprefixer** para vendor prefixes

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- **MDN CSS**: [CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS)
- **CSS Grid**: [Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
- **Flexbox**: [Flexbox Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout)

### **Herramientas de Desarrollo**
- **CSS Grid Generator**: [cssgrid-generator.netlify.app](https://cssgrid-generator.netlify.app/)
- **Flexbox Froggy**: [flexboxfroggy.com](https://flexboxfroggy.com/)
- **CSS Grid Garden**: [cssgridgarden.com](https://cssgridgarden.com/)

### **Práctica Online**
- **CodePen**: [codepen.io](https://codepen.io/)
- **CSS-Tricks**: [css-tricks.com](https://css-tricks.com/)
- **Awwwards**: [awwwards.com](https://awwwards.com/)

---

## 🎯 **EJERCICIOS PRÁCTICOS RECOMENDADOS**

1. **Crear un layout responsive** usando Flexbox y Grid
2. **Implementar animaciones** para elementos de interfaz
3. **Desarrollar un sistema de componentes** con variables CSS
4. **Crear una galería de imágenes** responsive
5. **Implementar un dashboard** con Grid y Flexbox

---

*CSS3 moderno con Flexbox, Grid y animaciones te permite crear interfaces web profesionales, responsive y atractivas. Dominar estas técnicas es esencial para el desarrollo frontend actual.* 