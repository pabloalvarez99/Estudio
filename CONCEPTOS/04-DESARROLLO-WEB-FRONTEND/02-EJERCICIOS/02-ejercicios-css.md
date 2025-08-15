# 🎨 Ejercicios CSS3: Layouts Modernos y Diseño Responsive

## 🎯 **OBJETIVO**
Practicar y reforzar los conceptos de CSS3 aprendidos, implementando layouts con Flexbox y Grid, animaciones, diseño responsive y CSS variables.

---

## 🎨 **EJERCICIO 1: LAYOUT CON FLEXBOX**

### **Descripción**
Crea un layout de página web usando Flexbox que incluya header, navegación, contenido principal y footer.

### **Requisitos**
- Header fijo en la parte superior
- Navegación horizontal centrada
- Contenido principal con sidebar
- Footer sticky en la parte inferior
- Diseño responsive

### **HTML Base**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Layout Flexbox</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header class="header">
        <div class="header-content">
            <h1 class="logo">Mi Sitio Web</h1>
            <nav class="nav">
                <ul class="nav-list">
                    <li><a href="#inicio">Inicio</a></li>
                    <li><a href="#servicios">Servicios</a></li>
                    <li><a href="#acerca">Acerca de</a></li>
                    <li><a href="#contacto">Contacto</a></li>
                </ul>
            </nav>
        </div>
    </header>
    
    <main class="main">
        <aside class="sidebar">
            <h3>Sidebar</h3>
            <ul>
                <li>Enlace 1</li>
                <li>Enlace 2</li>
                <li>Enlace 3</li>
            </ul>
        </aside>
        
        <section class="content">
            <h2>Contenido Principal</h2>
            <p>Este es el contenido principal de la página...</p>
        </section>
    </main>
    
    <footer class="footer">
        <p>&copy; 2024 Mi Sitio Web</p>
    </footer>
</body>
</html>
```

### **CSS Solución**
```css
/* Reset básico */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

/* Header */
.header {
    background: #333;
    color: white;
    padding: 1rem 0;
    position: sticky;
    top: 0;
    z-index: 1000;
}

.header-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.logo {
    font-size: 1.5rem;
    font-weight: bold;
}

.nav-list {
    display: flex;
    list-style: none;
    gap: 2rem;
}

.nav-list a {
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.nav-list a:hover {
    background-color: #555;
}

/* Main content */
.main {
    flex: 1;
    display: flex;
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    gap: 2rem;
}

.sidebar {
    flex: 0 0 250px;
    background: #f4f4f4;
    padding: 1.5rem;
    border-radius: 8px;
    height: fit-content;
}

.sidebar h3 {
    margin-bottom: 1rem;
    color: #333;
}

.sidebar ul {
    list-style: none;
}

.sidebar li {
    padding: 0.5rem 0;
    border-bottom: 1px solid #ddd;
}

.sidebar li:last-child {
    border-bottom: none;
}

.content {
    flex: 1;
    background: white;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.content h2 {
    margin-bottom: 1rem;
    color: #333;
}

/* Footer */
.footer {
    background: #333;
    color: white;
    text-align: center;
    padding: 1rem;
    margin-top: auto;
}

/* Responsive */
@media (max-width: 768px) {
    .header-content {
        flex-direction: column;
        gap: 1rem;
    }
    
    .nav-list {
        gap: 1rem;
    }
    
    .main {
        flex-direction: column;
        padding: 1rem;
    }
    
    .sidebar {
        flex: none;
        order: 2;
    }
    
    .content {
        order: 1;
    }
}
```

---

## 🎨 **EJERCICIO 2: GRID LAYOUT RESPONSIVE**

### **Descripción**
Crea una galería de productos usando CSS Grid con diseño responsive y diferentes layouts según el tamaño de pantalla.

### **Requisitos**
- Grid de productos responsive
- Diferentes columnas según breakpoint
- Imágenes con aspect ratio
- Hover effects
- Layout adaptativo

### **HTML Base**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galería Grid</title>
    <link rel="stylesheet" href="grid-styles.css">
</head>
<body>
    <div class="container">
        <h1>Galería de Productos</h1>
        
        <div class="products-grid">
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 1">
                </div>
                <div class="product-info">
                    <h3>Producto 1</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$99.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
            
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 2">
                </div>
                <div class="product-info">
                    <h3>Producto 2</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$149.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
            
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 3">
                </div>
                <div class="product-info">
                    <h3>Producto 3</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$79.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
            
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 4">
                </div>
                <div class="product-info">
                    <h3>Producto 4</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$199.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
            
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 5">
                </div>
                <div class="product-info">
                    <h3>Producto 5</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$129.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
            
            <div class="product-card">
                <div class="product-image">
                    <img src="https://via.placeholder.com/300x200" alt="Producto 6">
                </div>
                <div class="product-info">
                    <h3>Producto 6</h3>
                    <p>Descripción del producto</p>
                    <span class="price">$89.99</span>
                    <button class="btn">Comprar</button>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

### **CSS Solución**
```css
/* Reset y estilos base */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    line-height: 1.6;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
}

h1 {
    text-align: center;
    margin-bottom: 2rem;
    color: #333;
}

/* Grid de productos */
.products-grid {
    display: grid;
    gap: 2rem;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    grid-auto-rows: minmax(400px, auto);
}

/* Breakpoints específicos */
@media (min-width: 768px) {
    .products-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (min-width: 1024px) {
    .products-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (min-width: 1200px) {
    .products-grid {
        grid-template-columns: repeat(4, 1fr);
    }
}

/* Tarjetas de producto */
.product-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    display: flex;
    flex-direction: column;
}

.product-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,0,0,0.15);
}

.product-image {
    position: relative;
    overflow: hidden;
    aspect-ratio: 3/2;
}

.product-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.product-card:hover .product-image img {
    transform: scale(1.05);
}

.product-info {
    padding: 1.5rem;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.product-info h3 {
    color: #333;
    margin-bottom: 0.5rem;
    font-size: 1.2rem;
}

.product-info p {
    color: #666;
    margin-bottom: 1rem;
    flex: 1;
}

.price {
    font-size: 1.5rem;
    font-weight: bold;
    color: #2c5aa0;
    margin-bottom: 1rem;
}

.btn {
    background: #2c5aa0;
    color: white;
    border: none;
    padding: 0.75rem 1.5rem;
    border-radius: 6px;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.3s ease;
}

.btn:hover {
    background: #1e3f6b;
}

/* Grid areas para layout especial */
@media (min-width: 1200px) {
    .product-card:first-child {
        grid-column: span 2;
        grid-row: span 2;
    }
    
    .product-card:first-child .product-image {
        aspect-ratio: 16/9;
    }
    
    .product-card:first-child .product-info h3 {
        font-size: 1.5rem;
    }
    
    .product-card:first-child .price {
        font-size: 2rem;
    }
}
```

---

## 🎨 **EJERCICIO 3: ANIMACIONES Y TRANSICIONES**

### **Descripción**
Crea un dashboard interactivo con animaciones CSS, transiciones suaves y efectos hover avanzados.

### **Requisitos**
- Animaciones con keyframes
- Transiciones en elementos interactivos
- Efectos hover con transformaciones
- Animaciones de entrada
- Loading animations

### **HTML Base**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Animado</title>
    <link rel="stylesheet" href="animations.css">
</head>
<body>
    <div class="dashboard">
        <header class="dashboard-header">
            <h1 class="logo">Dashboard</h1>
            <nav class="nav">
                <a href="#" class="nav-link">Inicio</a>
                <a href="#" class="nav-link">Estadísticas</a>
                <a href="#" class="nav-link">Usuarios</a>
                <a href="#" class="nav-link">Configuración</a>
            </nav>
        </header>
        
        <main class="dashboard-content">
            <div class="stats-grid">
                <div class="stat-card">
                    <div class="stat-icon">📊</div>
                    <div class="stat-info">
                        <h3>Ventas</h3>
                        <p class="stat-number">$45,678</p>
                        <span class="stat-change positive">+12.5%</span>
                    </div>
                </div>
                
                <div class="stat-card">
                    <div class="stat-icon">👥</div>
                    <div class="stat-info">
                        <h3>Usuarios</h3>
                        <p class="stat-number">1,234</p>
                        <span class="stat-change positive">+8.2%</span>
                    </div>
                </div>
                
                <div class="stat-card">
                    <div class="stat-icon">📈</div>
                    <div class="stat-info">
                        <h3>Crecimiento</h3>
                        <p class="stat-number">23.4%</p>
                        <span class="stat-change negative">-2.1%</span>
                    </div>
                </div>
                
                <div class="stat-card">
                    <div class="stat-icon">🎯</div>
                    <div class="stat-info">
                        <h3>Objetivos</h3>
                        <p class="stat-number">87%</p>
                        <span class="stat-change positive">+5.3%</span>
                    </div>
                </div>
            </div>
            
            <div class="chart-container">
                <div class="chart">
                    <div class="chart-bar" style="--height: 60%"></div>
                    <div class="chart-bar" style="--height: 80%"></div>
                    <div class="chart-bar" style="--height: 45%"></div>
                    <div class="chart-bar" style="--height: 90%"></div>
                    <div class="chart-bar" style="--height: 70%"></div>
                </div>
            </div>
            
            <div class="action-buttons">
                <button class="btn btn-primary">Generar Reporte</button>
                <button class="btn btn-secondary">Exportar Datos</button>
                <button class="btn btn-success">Actualizar</button>
            </div>
        </main>
    </div>
</body>
</html>
```

### **CSS Solución**
```css
/* Variables CSS */
:root {
    --primary-color: #4f46e5;
    --secondary-color: #6b7280;
    --success-color: #10b981;
    --danger-color: #ef4444;
    --background: #f9fafb;
    --card-bg: #ffffff;
    --text-primary: #111827;
    --text-secondary: #6b7280;
    --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    --border-radius: 12px;
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Reset y estilos base */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    background: var(--background);
    color: var(--text-primary);
    line-height: 1.6;
}

/* Dashboard container */
.dashboard {
    min-height: 100vh;
    animation: fadeIn 0.8s ease-out;
}

/* Header */
.dashboard-header {
    background: var(--card-bg);
    padding: 1.5rem 2rem;
    box-shadow: var(--shadow);
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: sticky;
    top: 0;
    z-index: 100;
}

.logo {
    font-size: 1.8rem;
    font-weight: 700;
    background: linear-gradient(135deg, var(--primary-color), #7c3aed);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: slideInLeft 0.6s ease-out;
}

.nav {
    display: flex;
    gap: 2rem;
}

.nav-link {
    text-decoration: none;
    color: var(--text-secondary);
    font-weight: 500;
    padding: 0.5rem 1rem;
    border-radius: 8px;
    transition: var(--transition);
    position: relative;
}

.nav-link::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    width: 0;
    height: 2px;
    background: var(--primary-color);
    transition: var(--transition);
    transform: translateX(-50%);
}

.nav-link:hover {
    color: var(--primary-color);
    background: rgba(79, 70, 229, 0.1);
}

.nav-link:hover::after {
    width: 100%;
}

/* Contenido principal */
.dashboard-content {
    padding: 2rem;
    max-width: 1200px;
    margin: 0 auto;
}

/* Grid de estadísticas */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1.5rem;
    margin-bottom: 3rem;
}

.stat-card {
    background: var(--card-bg);
    padding: 1.5rem;
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    transition: var(--transition);
    animation: slideInUp 0.6s ease-out;
    animation-fill-mode: both;
}

.stat-card:nth-child(1) { animation-delay: 0.1s; }
.stat-card:nth-child(2) { animation-delay: 0.2s; }
.stat-card:nth-child(3) { animation-delay: 0.3s; }
.stat-card:nth-child(4) { animation-delay: 0.4s; }

.stat-card:hover {
    transform: translateY(-8px);
    box-shadow: var(--shadow-lg);
}

.stat-card {
    display: flex;
    align-items: center;
    gap: 1rem;
}

.stat-icon {
    font-size: 2.5rem;
    width: 60px;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, var(--primary-color), #7c3aed);
    color: white;
    border-radius: 50%;
    animation: pulse 2s infinite;
}

.stat-info h3 {
    color: var(--text-secondary);
    font-size: 0.875rem;
    font-weight: 500;
    margin-bottom: 0.25rem;
}

.stat-number {
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--text-primary);
    margin-bottom: 0.25rem;
}

.stat-change {
    font-size: 0.875rem;
    font-weight: 500;
    padding: 0.25rem 0.5rem;
    border-radius: 6px;
}

.stat-change.positive {
    background: rgba(16, 185, 129, 0.1);
    color: var(--success-color);
}

.stat-change.negative {
    background: rgba(239, 68, 68, 0.1);
    color: var(--danger-color);
}

/* Gráfico */
.chart-container {
    background: var(--card-bg);
    padding: 2rem;
    border-radius: var(--border-radius);
    box-shadow: var(--shadow);
    margin-bottom: 2rem;
    animation: slideInUp 0.8s ease-out 0.5s both;
}

.chart {
    display: flex;
    align-items: end;
    gap: 1rem;
    height: 200px;
    padding: 1rem 0;
}

.chart-bar {
    flex: 1;
    background: linear-gradient(to top, var(--primary-color), #7c3aed);
    border-radius: 4px 4px 0 0;
    height: var(--height);
    animation: growUp 1s ease-out 1s both;
    position: relative;
}

.chart-bar::after {
    content: attr(style);
    position: absolute;
    top: -25px;
    left: 50%;
    transform: translateX(-50%);
    background: var(--text-primary);
    color: white;
    padding: 0.25rem 0.5rem;
    border-radius: 4px;
    font-size: 0.75rem;
    opacity: 0;
    transition: var(--transition);
}

.chart-bar:hover::after {
    opacity: 1;
}

/* Botones de acción */
.action-buttons {
    display: flex;
    gap: 1rem;
    justify-content: center;
    animation: slideInUp 0.8s ease-out 0.7s both;
}

.btn {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: var(--transition);
    position: relative;
    overflow: hidden;
}

.btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
    transition: left 0.5s;
}

.btn:hover::before {
    left: 100%;
}

.btn-primary {
    background: var(--primary-color);
    color: white;
}

.btn-primary:hover {
    background: #4338ca;
    transform: translateY(-2px);
}

.btn-secondary {
    background: var(--secondary-color);
    color: white;
}

.btn-secondary:hover {
    background: #4b5563;
    transform: translateY(-2px);
}

.btn-success {
    background: var(--success-color);
    color: white;
}

.btn-success:hover {
    background: #059669;
    transform: translateY(-2px);
}

/* Keyframes */
@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@keyframes slideInLeft {
    from {
        opacity: 0;
        transform: translateX(-30px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}

@keyframes slideInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes growUp {
    from {
        height: 0;
    }
    to {
        height: var(--height);
    }
}

@keyframes pulse {
    0%, 100% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.05);
    }
}

/* Responsive */
@media (max-width: 768px) {
    .dashboard-header {
        flex-direction: column;
        gap: 1rem;
        padding: 1rem;
    }
    
    .nav {
        gap: 1rem;
    }
    
    .stats-grid {
        grid-template-columns: 1fr;
    }
    
    .action-buttons {
        flex-direction: column;
        align-items: center;
    }
    
    .btn {
        width: 100%;
        max-width: 300px;
    }
}
```

---

## 🎨 **EJERCICIO 4: CSS VARIABLES Y THEMES**

### **Descripción**
Crea un sistema de temas usando CSS variables que permita cambiar entre modo claro, oscuro y personalizado.

### **Requisitos**
- Sistema de variables CSS
- Múltiples temas
- Transiciones suaves entre temas
- Selector de temas interactivo
- Variables para colores, espaciado y tipografía

### **HTML Base**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Temas</title>
    <link rel="stylesheet" href="themes.css">
</head>
<body>
    <div class="theme-selector">
        <button class="theme-btn" data-theme="light">☀️ Claro</button>
        <button class="theme-btn" data-theme="dark">🌙 Oscuro</button>
        <button class="theme-btn" data-theme="custom">🎨 Personalizado</button>
    </div>
    
    <div class="container">
        <header class="header">
            <h1>Sistema de Temas CSS</h1>
            <p>Cambia entre diferentes temas usando CSS variables</p>
        </header>
        
        <main class="main">
            <section class="card">
                <h2>Tarjeta de Ejemplo</h2>
                <p>Esta es una tarjeta que se adapta al tema seleccionado.</p>
                <button class="btn">Botón de Acción</button>
            </section>
            
            <section class="card">
                <h2>Otra Tarjeta</h2>
                <p>Las variables CSS permiten cambios dinámicos de tema.</p>
                <div class="color-palette">
                    <div class="color-swatch primary"></div>
                    <div class="color-swatch secondary"></div>
                    <div class="color-swatch accent"></div>
                </div>
            </section>
        </main>
        
        <aside class="sidebar">
            <h3>Paleta de Colores</h3>
            <div class="color-info">
                <div class="color-item">
                    <span class="color-label">Primario</span>
                    <span class="color-value">#4f46e5</span>
                </div>
                <div class="color-item">
                    <span class="color-label">Secundario</span>
                    <span class="color-value">#6b7280</span>
                </div>
                <div class="color-item">
                    <span class="color-label">Acento</span>
                    <span class="color-value">#10b981</span>
                </div>
            </div>
        </aside>
    </div>
</body>
</html>
```

### **CSS Solución**
```css
/* Variables CSS para temas */
:root {
    /* Tema claro (por defecto) */
    --bg-primary: #ffffff;
    --bg-secondary: #f9fafb;
    --bg-tertiary: #f3f4f6;
    --text-primary: #111827;
    --text-secondary: #6b7280;
    --text-muted: #9ca3af;
    --border-color: #e5e7eb;
    --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    
    /* Colores de marca */
    --color-primary: #4f46e5;
    --color-primary-hover: #4338ca;
    --color-secondary: #6b7280;
    --color-secondary-hover: #4b5563;
    --color-accent: #10b981;
    --color-accent-hover: #059669;
    --color-danger: #ef4444;
    --color-warning: #f59e0b;
    --color-info: #3b82f6;
    
    /* Espaciado */
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 1.5rem;
    --spacing-xl: 2rem;
    --spacing-2xl: 3rem;
    
    /* Tipografía */
    --font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    --font-size-xs: 0.75rem;
    --font-size-sm: 0.875rem;
    --font-size-base: 1rem;
    --font-size-lg: 1.125rem;
    --font-size-xl: 1.25rem;
    --font-size-2xl: 1.5rem;
    --font-size-3xl: 1.875rem;
    
    /* Bordes */
    --border-radius-sm: 4px;
    --border-radius: 8px;
    --border-radius-lg: 12px;
    --border-radius-xl: 16px;
    
    /* Transiciones */
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Tema oscuro */
[data-theme="dark"] {
    --bg-primary: #111827;
    --bg-secondary: #1f2937;
    --bg-tertiary: #374151;
    --text-primary: #f9fafb;
    --text-secondary: #d1d5db;
    --text-muted: #9ca3af;
    --border-color: #4b5563;
    --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.3);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
}

/* Tema personalizado */
[data-theme="custom"] {
    --bg-primary: #fef3c7;
    --bg-secondary: #fde68a;
    --bg-tertiary: #fbbf24;
    --text-primary: #92400e;
    --text-secondary: #b45309;
    --text-muted: #d97706;
    --border-color: #f59e0b;
    --color-primary: #dc2626;
    --color-primary-hover: #b91c1c;
    --color-accent: #059669;
    --color-accent-hover: #047857;
}

/* Estilos base */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: var(--font-family);
    background: var(--bg-secondary);
    color: var(--text-primary);
    line-height: 1.6;
    transition: var(--transition);
}

/* Selector de temas */
.theme-selector {
    position: fixed;
    top: var(--spacing-md);
    right: var(--spacing-md);
    display: flex;
    gap: var(--spacing-sm);
    z-index: 1000;
}

.theme-btn {
    padding: var(--spacing-sm) var(--spacing-md);
    border: 2px solid var(--border-color);
    background: var(--bg-primary);
    color: var(--text-primary);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: var(--font-size-sm);
    font-weight: 500;
    transition: var(--transition);
}

.theme-btn:hover {
    background: var(--color-primary);
    color: white;
    border-color: var(--color-primary);
    transform: translateY(-2px);
}

.theme-btn.active {
    background: var(--color-primary);
    color: white;
    border-color: var(--color-primary);
}

/* Container principal */
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: var(--spacing-2xl) var(--spacing-lg);
}

/* Header */
.header {
    text-align: center;
    margin-bottom: var(--spacing-2xl);
    animation: fadeInUp 0.8s ease-out;
}

.header h1 {
    font-size: var(--font-size-3xl);
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.header p {
    font-size: var(--font-size-lg);
    color: var(--text-secondary);
}

/* Layout principal */
.main {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: var(--spacing-xl);
    margin-bottom: var(--spacing-2xl);
}

/* Tarjetas */
.card {
    background: var(--bg-primary);
    padding: var(--spacing-xl);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
    transition: var(--transition);
    animation: fadeInUp 0.8s ease-out 0.2s both;
}

.card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}

.card h2 {
    font-size: var(--font-size-xl);
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.card p {
    color: var(--text-secondary);
    margin-bottom: var(--spacing-lg);
}

/* Botones */
.btn {
    background: var(--color-primary);
    color: white;
    border: none;
    padding: var(--spacing-sm) var(--spacing-lg);
    border-radius: var(--border-radius);
    font-size: var(--font-size-base);
    font-weight: 500;
    cursor: pointer;
    transition: var(--transition);
}

.btn:hover {
    background: var(--color-primary-hover);
    transform: translateY(-2px);
}

/* Paleta de colores */
.color-palette {
    display: flex;
    gap: var(--spacing-md);
    margin-top: var(--spacing-lg);
}

.color-swatch {
    width: 40px;
    height: 40px;
    border-radius: var(--border-radius);
    border: 2px solid var(--border-color);
}

.color-swatch.primary {
    background: var(--color-primary);
}

.color-swatch.secondary {
    background: var(--color-secondary);
}

.color-swatch.accent {
    background: var(--color-accent);
}

/* Sidebar */
.sidebar {
    background: var(--bg-primary);
    padding: var(--spacing-xl);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
    animation: fadeInUp 0.8s ease-out 0.4s both;
}

.sidebar h3 {
    font-size: var(--font-size-lg);
    margin-bottom: var(--spacing-lg);
    color: var(--text-primary);
}

.color-info {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-md);
}

.color-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spacing-sm);
    background: var(--bg-secondary);
    border-radius: var(--border-radius);
}

.color-label {
    font-weight: 500;
    color: var(--text-primary);
}

.color-value {
    font-family: monospace;
    color: var(--text-secondary);
    font-size: var(--font-size-sm);
}

/* Animaciones */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Responsive */
@media (max-width: 768px) {
    .main {
        grid-template-columns: 1fr;
        gap: var(--spacing-lg);
    }
    
    .theme-selector {
        position: static;
        justify-content: center;
        margin-bottom: var(--spacing-lg);
    }
    
    .container {
        padding: var(--spacing-lg) var(--spacing-md);
    }
}
```

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 5: Layout de Dashboard Complejo**
Crea un dashboard con múltiples secciones usando CSS Grid y Flexbox combinados.

### **Ejercicio 6: Galería de Imágenes con Lightbox**
Implementa una galería de imágenes con efectos hover y modal lightbox usando solo CSS.

### **Ejercicio 7: Formulario de Búsqueda Avanzada**
Crea un formulario de búsqueda con filtros y resultados usando CSS Grid y Flexbox.

### **Ejercicio 8: Sistema de Notificaciones**
Implementa un sistema de notificaciones con diferentes tipos y animaciones CSS.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Layout y Estructura (30%)**
- Uso correcto de Flexbox y Grid
- Diseño responsive
- Estructura semántica

### **Animaciones y Transiciones (25%)**
- Keyframes apropiados
- Transiciones suaves
- Efectos hover

### **CSS Variables y Temas (25%)**
- Sistema de variables robusto
- Múltiples temas
- Transiciones entre temas

### **Responsive Design (20%)**
- Breakpoints apropiados
- Adaptabilidad móvil
- Performance optimizado

---

## 💡 **CONSEJOS PARA LA PRÁCTICA**

1. **Empieza con la estructura**: Define el layout antes de los estilos
2. **Usa CSS variables**: Para consistencia y mantenibilidad
3. **Testea en diferentes dispositivos**: Asegura responsive design
4. **Optimiza animaciones**: Usa transform y opacity para performance
5. **Valida tu CSS**: Usa herramientas de validación

---

*Estos ejercicios te ayudarán a dominar CSS3 moderno, creando layouts flexibles, animaciones suaves y sistemas de temas dinámicos.* 