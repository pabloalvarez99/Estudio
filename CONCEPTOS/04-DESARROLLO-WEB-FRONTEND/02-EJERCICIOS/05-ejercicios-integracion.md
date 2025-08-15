# 🔗 Ejercicios de Integración: Frontend Completo

## 🎯 **OBJETIVO**
Practicar la integración completa de HTML5, CSS3, JavaScript ES6+ y React, creando aplicaciones web funcionales y realistas.

---

## 🔗 **EJERCICIO 1: APLICACIÓN DE GESTIÓN DE PROYECTOS**

### **Descripción**
Crea una aplicación completa de gestión de proyectos que integre todas las tecnologías frontend aprendidas.

### **Requisitos**
- Interfaz responsive con HTML5 semántico
- Estilos modernos con CSS3 (Grid, Flexbox, Variables)
- Funcionalidad con JavaScript ES6+
- Componentes React para funcionalidades avanzadas
- Integración con API REST
- LocalStorage para persistencia local

### **Estructura del Proyecto**
```
project-manager/
├── public/
│   ├── index.html
│   ├── manifest.json
│   └── icons/
├── src/
│   ├── components/
│   │   ├── ProjectCard.jsx
│   │   ├── TaskList.jsx
│   │   ├── ProjectForm.jsx
│   │   ├── Dashboard.jsx
│   │   └── Navigation.jsx
│   ├── hooks/
│   │   ├── useProjects.js
│   │   ├── useTasks.js
│   │   └── useAuth.js
│   ├── services/
│   │   ├── api.js
│   │   └── storage.js
│   ├── utils/
│   │   ├── validators.js
│   │   └── helpers.js
│   ├── styles/
│   │   ├── components/
│   │   ├── layout/
│   │   └── main.css
│   ├── App.jsx
│   └── index.js
└── package.json
```

### **index.html (HTML5 Semántico)**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Gestor de proyectos profesional con interfaz moderna">
    <meta name="keywords" content="proyectos, gestión, tareas, organización">
    <meta name="author" content="Tu Nombre">
    
    <!-- Open Graph -->
    <meta property="og:title" content="Gestor de Proyectos">
    <meta property="og:description" content="Organiza y gestiona tus proyectos de manera eficiente">
    <meta property="og:type" content="website">
    
    <!-- Favicon -->
    <link rel="icon" type="image/x-icon" href="/favicon.ico">
    <link rel="apple-touch-icon" href="/icons/apple-touch-icon.png">
    
    <!-- Manifest -->
    <link rel="manifest" href="/manifest.json">
    
    <!-- Preload crítico -->
    <link rel="preload" href="/fonts/inter-var.woff2" as="font" type="font/woff2" crossorigin>
    
    <title>Gestor de Proyectos - Organiza tu trabajo</title>
</head>
<body>
    <div id="root"></div>
    
    <!-- Loading inicial -->
    <div id="loading" class="initial-loading">
        <div class="loading-spinner"></div>
        <p>Cargando aplicación...</p>
    </div>
    
    <!-- Scripts -->
    <script type="module" src="/src/index.js"></script>
    
    <!-- Fallback para JavaScript deshabilitado -->
    <noscript>
        <div class="no-js-message">
            <h1>JavaScript Requerido</h1>
            <p>Esta aplicación requiere JavaScript para funcionar correctamente.</p>
            <p>Por favor, habilita JavaScript en tu navegador.</p>
        </div>
    </noscript>
</body>
</html>
```

### **main.css (CSS3 Moderno con Variables)**
```css
/* Variables CSS para el tema */
:root {
    /* Colores principales */
    --primary-color: #4f46e5;
    --primary-hover: #4338ca;
    --secondary-color: #6b7280;
    --accent-color: #10b981;
    --danger-color: #ef4444;
    --warning-color: #f59e0b;
    --info-color: #3b82f6;
    
    /* Colores de fondo */
    --bg-primary: #ffffff;
    --bg-secondary: #f9fafb;
    --bg-tertiary: #f3f4f6;
    --bg-dark: #111827;
    
    /* Colores de texto */
    --text-primary: #111827;
    --text-secondary: #6b7280;
    --text-muted: #9ca3af;
    --text-light: #ffffff;
    
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
    
    /* Bordes y sombras */
    --border-radius-sm: 4px;
    --border-radius: 8px;
    --border-radius-lg: 12px;
    --border-radius-xl: 16px;
    --border-color: #e5e7eb;
    --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    
    /* Transiciones */
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    --transition-fast: all 0.15s ease;
    
    /* Breakpoints */
    --breakpoint-sm: 640px;
    --breakpoint-md: 768px;
    --breakpoint-lg: 1024px;
    --breakpoint-xl: 1280px;
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

/* Reset y estilos base */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    font-size: 16px;
    scroll-behavior: smooth;
}

body {
    font-family: var(--font-family);
    background: var(--bg-secondary);
    color: var(--text-primary);
    line-height: 1.6;
    transition: var(--transition);
}

/* Layout principal */
.app {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

.main-content {
    flex: 1;
    padding: var(--spacing-lg);
    max-width: var(--breakpoint-xl);
    margin: 0 auto;
    width: 100%;
}

/* Componentes de navegación */
.navigation {
    background: var(--bg-primary);
    border-bottom: 1px solid var(--border-color);
    box-shadow: var(--shadow);
    position: sticky;
    top: 0;
    z-index: 100;
}

.nav-container {
    max-width: var(--breakpoint-xl);
    margin: 0 auto;
    padding: 0 var(--spacing-lg);
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 64px;
}

.nav-brand {
    font-size: var(--font-size-xl);
    font-weight: 700;
    color: var(--primary-color);
    text-decoration: none;
}

.nav-menu {
    display: flex;
    gap: var(--spacing-lg);
    list-style: none;
}

.nav-link {
    color: var(--text-secondary);
    text-decoration: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    transition: var(--transition);
    font-weight: 500;
}

.nav-link:hover,
.nav-link.active {
    color: var(--primary-color);
    background: rgba(79, 70, 229, 0.1);
}

/* Dashboard */
.dashboard {
    display: grid;
    gap: var(--spacing-xl);
}

.dashboard-header {
    text-align: center;
    margin-bottom: var(--spacing-2xl);
}

.dashboard-title {
    font-size: var(--font-size-3xl);
    font-weight: 700;
    margin-bottom: var(--spacing-md);
    color: var(--text-primary);
}

.dashboard-subtitle {
    font-size: var(--font-size-lg);
    color: var(--text-secondary);
}

/* Grid de estadísticas */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: var(--spacing-lg);
    margin-bottom: var(--spacing-2xl);
}

.stat-card {
    background: var(--bg-primary);
    padding: var(--spacing-xl);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
    text-align: center;
    transition: var(--transition);
}

.stat-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-lg);
}

.stat-number {
    font-size: var(--font-size-3xl);
    font-weight: 700;
    color: var(--primary-color);
    margin-bottom: var(--spacing-sm);
}

.stat-label {
    font-size: var(--font-size-sm);
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.05em;
}

/* Grid de proyectos */
.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: var(--spacing-lg);
}

.project-card {
    background: var(--bg-primary);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
    overflow: hidden;
    transition: var(--transition);
}

.project-card:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.project-header {
    padding: var(--spacing-lg);
    border-bottom: 1px solid var(--border-color);
}

.project-title {
    font-size: var(--font-size-lg);
    font-weight: 600;
    margin-bottom: var(--spacing-sm);
    color: var(--text-primary);
}

.project-description {
    color: var(--text-secondary);
    font-size: var(--font-size-sm);
    line-height: 1.5;
}

.project-meta {
    padding: var(--spacing-lg);
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.project-status {
    padding: var(--spacing-xs) var(--spacing-sm);
    border-radius: var(--border-radius);
    font-size: var(--font-size-xs);
    font-weight: 500;
    text-transform: uppercase;
}

.project-status.active {
    background: rgba(16, 185, 129, 0.1);
    color: var(--accent-color);
}

.project-status.completed {
    background: rgba(79, 70, 229, 0.1);
    color: var(--primary-color);
}

.project-status.paused {
    background: rgba(245, 158, 11, 0.1);
    color: var(--warning-color);
}

/* Formularios */
.form-container {
    background: var(--bg-primary);
    padding: var(--spacing-2xl);
    border-radius: var(--border-radius-lg);
    box-shadow: var(--shadow);
    border: 1px solid var(--border-color);
    max-width: 600px;
    margin: 0 auto;
}

.form-title {
    font-size: var(--font-size-2xl);
    font-weight: 600;
    margin-bottom: var(--spacing-xl);
    text-align: center;
    color: var(--text-primary);
}

.form-group {
    margin-bottom: var(--spacing-lg);
}

.form-label {
    display: block;
    margin-bottom: var(--spacing-sm);
    font-weight: 500;
    color: var(--text-primary);
}

.form-input,
.form-textarea,
.form-select {
    width: 100%;
    padding: var(--spacing-md);
    border: 1px solid var(--border-color);
    border-radius: var(--border-radius);
    font-size: var(--font-size-base);
    background: var(--bg-primary);
    color: var(--text-primary);
    transition: var(--transition);
}

.form-input:focus,
.form-textarea:focus,
.form-select:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
}

.form-textarea {
    resize: vertical;
    min-height: 100px;
}

.form-error {
    color: var(--danger-color);
    font-size: var(--font-size-sm);
    margin-top: var(--spacing-xs);
}

/* Botones */
.btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: var(--spacing-md) var(--spacing-lg);
    border: none;
    border-radius: var(--border-radius);
    font-size: var(--font-size-base);
    font-weight: 500;
    text-decoration: none;
    cursor: pointer;
    transition: var(--transition);
    gap: var(--spacing-sm);
}

.btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.btn-primary {
    background: var(--primary-color);
    color: var(--text-light);
}

.btn-primary:hover:not(:disabled) {
    background: var(--primary-hover);
    transform: translateY(-1px);
}

.btn-secondary {
    background: var(--secondary-color);
    color: var(--text-light);
}

.btn-secondary:hover:not(:disabled) {
    background: #4b5563;
    transform: translateY(-1px);
}

.btn-success {
    background: var(--accent-color);
    color: var(--text-light);
}

.btn-danger {
    background: var(--danger-color);
    color: var(--text-light);
}

/* Responsive */
@media (max-width: 768px) {
    .main-content {
        padding: var(--spacing-md);
    }
    
    .nav-container {
        padding: 0 var(--spacing-md);
    }
    
    .nav-menu {
        display: none;
    }
    
    .stats-grid {
        grid-template-columns: repeat(2, 1fr);
    }
    
    .projects-grid {
        grid-template-columns: 1fr;
    }
    
    .form-container {
        padding: var(--spacing-lg);
        margin: 0 var(--spacing-md);
    }
}

/* Loading inicial */
.initial-loading {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--bg-primary);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 9999;
}

.loading-spinner {
    width: 40px;
    height: 40px;
    border: 3px solid var(--border-color);
    border-top: 3px solid var(--primary-color);
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: var(--spacing-lg);
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Mensaje sin JavaScript */
.no-js-message {
    text-align: center;
    padding: var(--spacing-2xl);
    background: var(--bg-primary);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

.no-js-message h1 {
    color: var(--danger-color);
    margin-bottom: var(--spacing-lg);
}
```

### **App.jsx (Componente Principal)**
```jsx
import React, { useState, useEffect } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Navigation from './components/Navigation';
import Dashboard from './components/Dashboard';
import ProjectForm from './components/ProjectForm';
import ProjectDetail from './components/ProjectDetail';
import { useAuth } from './hooks/useAuth';
import { useProjects } from './hooks/useProjects';
import './styles/main.css';

function App() {
  const { user, login, logout, isAuthenticated } = useAuth();
  const { projects, loading, error } = useProjects();
  const [theme, setTheme] = useState('light');

  // Cambiar tema
  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    document.documentElement.setAttribute('data-theme', newTheme);
    localStorage.setItem('theme', newTheme);
  };

  // Cargar tema guardado
  useEffect(() => {
    const savedTheme = localStorage.getItem('theme') || 'light';
    setTheme(savedTheme);
    document.documentElement.setAttribute('data-theme', savedTheme);
  }, []);

  // Ocultar loading inicial
  useEffect(() => {
    const loadingElement = document.getElementById('loading');
    if (loadingElement) {
      setTimeout(() => {
        loadingElement.style.opacity = '0';
        setTimeout(() => {
          loadingElement.remove();
        }, 300);
      }, 1000);
    }
  }, []);

  if (!isAuthenticated) {
    return (
      <div className="auth-container">
        <div className="auth-card">
          <h1>Bienvenido al Gestor de Proyectos</h1>
          <p>Inicia sesión para continuar</p>
          <button 
            className="btn btn-primary"
            onClick={() => login('demo@example.com', 'password')}
          >
            Iniciar Sesión Demo
          </button>
        </div>
      </div>
    );
  }

  return (
    <Router>
      <div className="app">
        <Navigation 
          user={user}
          onLogout={logout}
          onThemeToggle={toggleTheme}
          theme={theme}
        />
        
        <main className="main-content">
          <Routes>
            <Route 
              path="/" 
              element={
                <Dashboard 
                  projects={projects}
                  loading={loading}
                  error={error}
                />
              } 
            />
            <Route 
              path="/create" 
              element={<ProjectForm />} 
            />
            <Route 
              path="/project/:id" 
              element={<ProjectDetail />} 
            />
            <Route 
              path="/edit/:id" 
              element={<ProjectForm />} 
            />
          </Routes>
        </main>
      </div>
    </Router>
  );
}

export default App;
```

---

## 🔗 **EJERCICIO 2: INTEGRACIÓN CON API REST**

### **Descripción**
Crea un sistema completo de integración con API REST que incluya autenticación, manejo de errores y sincronización de datos.

### **Requisitos**
- Cliente HTTP con interceptores
- Manejo de estado de autenticación
- Cache y sincronización offline
- Manejo de errores robusto
- Optimistic updates

### **api.js (Servicio de API)**
```javascript
class ApiService {
  constructor(baseURL) {
    this.baseURL = baseURL;
    this.token = localStorage.getItem('authToken');
    this.refreshToken = localStorage.getItem('refreshToken');
    this.isRefreshing = false;
    this.failedQueue = [];
    
    this.setupInterceptors();
  }

  setupInterceptors() {
    // Interceptor para agregar token
    this.requestInterceptor = (config) => {
      if (this.token) {
        config.headers.Authorization = `Bearer ${this.token}`;
      }
      return config;
    };

    // Interceptor para manejar respuestas
    this.responseInterceptor = async (response) => {
      if (response.status === 401 && this.refreshToken) {
        return this.handleTokenRefresh(response);
      }
      return response;
    };
  }

  async handleTokenRefresh(originalResponse) {
    if (this.isRefreshing) {
      return new Promise((resolve) => {
        this.failedQueue.push({ resolve, reject: () => resolve(originalResponse) });
      });
    }

    this.isRefreshing = true;

    try {
      const response = await fetch(`${this.baseURL}/auth/refresh`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ refreshToken: this.refreshToken })
      });

      if (response.ok) {
        const { accessToken, refreshToken } = await response.json();
        this.token = accessToken;
        this.refreshToken = refreshToken;
        
        localStorage.setItem('authToken', accessToken);
        localStorage.setItem('refreshToken', refreshToken);

        // Procesar cola de requests fallidos
        this.failedQueue.forEach(({ resolve }) => resolve());
        this.failedQueue = [];

        return originalResponse;
      } else {
        // Token de refresh expirado
        this.logout();
        throw new Error('Sesión expirada');
      }
    } catch (error) {
      this.failedQueue.forEach(({ reject }) => reject(error));
      this.failedQueue = [];
      throw error;
    } finally {
      this.isRefreshing = false;
    }
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = this.requestInterceptor({
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options.headers
      }
    });

    try {
      const response = await fetch(url, config);
      const processedResponse = await this.responseInterceptor(response);
      
      if (!processedResponse.ok) {
        throw await this.createError(processedResponse);
      }

      return await processedResponse.json();
    } catch (error) {
      if (error.name === 'AbortError') {
        throw new Error('Request cancelled');
      }
      throw error;
    }
  }

  async createError(response) {
    let errorData;
    try {
      errorData = await response.json();
    } catch {
      errorData = { message: 'Unknown error' };
    }

    const error = new Error(errorData.message || `HTTP ${response.status}`);
    error.status = response.status;
    error.statusText = response.statusText;
    error.data = errorData;
    error.response = response;

    return error;
  }

  // Métodos HTTP
  async get(endpoint, options = {}) {
    return this.request(endpoint, { ...options, method: 'GET' });
  }

  async post(endpoint, data, options = {}) {
    return this.request(endpoint, {
      ...options,
      method: 'POST',
      body: JSON.stringify(data)
    });
  }

  async put(endpoint, data, options = {}) {
    return this.request(endpoint, {
      ...options,
      method: 'PUT',
      body: JSON.stringify(data)
    });
  }

  async delete(endpoint, options = {}) {
    return this.request(endpoint, { ...options, method: 'DELETE' });
  }

  async patch(endpoint, data, options = {}) {
    return this.request(endpoint, {
      ...options,
      method: 'PATCH',
      body: JSON.stringify(data)
    });
  }

  // Métodos específicos de la API
  async login(credentials) {
    const response = await this.post('/auth/login', credentials);
    this.token = response.accessToken;
    this.refreshToken = response.refreshToken;
    
    localStorage.setItem('authToken', this.token);
    localStorage.setItem('refreshToken', this.refreshToken);
    
    return response;
  }

  async logout() {
    try {
      await this.post('/auth/logout');
    } catch (error) {
      console.error('Error en logout:', error);
    } finally {
      this.token = null;
      this.refreshToken = null;
      localStorage.removeItem('authToken');
      localStorage.removeItem('refreshToken');
    }
  }

  // Proyectos
  async getProjects(filters = {}) {
    const params = new URLSearchParams(filters);
    return this.get(`/projects?${params}`);
  }

  async getProject(id) {
    return this.get(`/projects/${id}`);
  }

  async createProject(projectData) {
    return this.post('/projects', projectData);
  }

  async updateProject(id, projectData) {
    return this.put(`/projects/${id}`, projectData);
  }

  async deleteProject(id) {
    return this.delete(`/projects/${id}`);
  }

  // Tareas
  async getTasks(projectId, filters = {}) {
    const params = new URLSearchParams(filters);
    return this.get(`/projects/${projectId}/tasks?${params}`);
  }

  async createTask(projectId, taskData) {
    return this.post(`/projects/${projectId}/tasks`, taskData);
  }

  async updateTask(projectId, taskId, taskData) {
    return this.put(`/projects/${projectId}/tasks/${taskId}`, taskData);
  }

  async deleteTask(projectId, taskId) {
    return this.delete(`/projects/${projectId}/tasks/${taskId}`);
  }

  // Usuarios
  async getProfile() {
    return this.get('/auth/profile');
  }

  async updateProfile(profileData) {
    return this.put('/auth/profile', profileData);
  }

  async changePassword(passwordData) {
    return this.post('/auth/change-password', passwordData);
  }
}

// Instancia singleton
export const apiService = new ApiService(process.env.REACT_APP_API_URL || 'http://localhost:3001/api');

export default apiService;
```

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 3: Sistema de Notificaciones en Tiempo Real**
Implementa notificaciones push y WebSocket para actualizaciones en tiempo real.

### **Ejercicio 4: Sincronización Offline**
Crea un sistema de sincronización que funcione sin conexión a internet.

### **Ejercicio 5: Optimización de Performance**
Implementa lazy loading, code splitting y optimización de bundles.

### **Ejercicio 6: Testing Completo**
Escribe tests unitarios, de integración y end-to-end para toda la aplicación.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Integración de Tecnologías (30%)**
- Uso correcto de HTML5, CSS3, JavaScript y React
- Consistencia entre tecnologías
- Arquitectura coherente

### **Funcionalidad (25%)**
- Características implementadas
- Integración con APIs
- Manejo de estado global

### **Experiencia de Usuario (25%)**
- Interfaz responsive
- Accesibilidad
- Performance

### **Calidad del Código (20%)**
- Organización y estructura
- Manejo de errores
- Documentación

---

## 💡 **CONSEJOS PARA LA PRÁCTICA**

1. **Planifica la arquitectura**: Define la estructura antes de empezar
2. **Integra gradualmente**: Construye capa por capa
3. **Testea la integración**: Verifica que todas las partes funcionen juntas
4. **Optimiza el performance**: Usa herramientas de profiling
5. **Documenta las APIs**: Especialmente para integración

---

*Estos ejercicios te ayudarán a dominar la integración completa de tecnologías frontend, creando aplicaciones web robustas y profesionales.* 