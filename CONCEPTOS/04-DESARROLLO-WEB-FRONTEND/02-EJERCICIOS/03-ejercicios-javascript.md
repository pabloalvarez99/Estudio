# 🚀 Ejercicios JavaScript ES6+: Funciones Modernas y DOM

## 🎯 **OBJETIVO**
Practicar y reforzar los conceptos de JavaScript ES6+ aprendidos, implementando funciones modernas, manipulación del DOM, programación asíncrona y patrones de código.

---

## 🚀 **EJERCICIO 1: SISTEMA DE GESTIÓN DE TAREAS**

### **Descripción**
Crea un sistema completo de gestión de tareas usando JavaScript ES6+ con clases, módulos y localStorage.

### **Requisitos**
- Clases ES6 para Task y TaskManager
- Módulos ES6 (import/export)
- LocalStorage para persistencia
- Métodos de array modernos (map, filter, reduce)
- Destructuring y spread operator

### **Estructura de Archivos**
```
task-manager/
├── index.html
├── js/
│   ├── main.js
│   ├── Task.js
│   ├── TaskManager.js
│   └── utils.js
└── css/
    └── styles.css
```

### **Task.js**
```javascript
export class Task {
    constructor(title, description = '', priority = 'medium', dueDate = null) {
        this.id = Date.now() + Math.random().toString(36).substr(2, 9);
        this.title = title;
        this.description = description;
        this.priority = priority;
        this.dueDate = dueDate;
        this.completed = false;
        this.createdAt = new Date();
        this.updatedAt = new Date();
    }
    
    toggleComplete() {
        this.completed = !this.completed;
        this.updatedAt = new Date();
    }
    
    update(updates) {
        Object.assign(this, updates);
        this.updatedAt = new Date();
    }
    
    isOverdue() {
        if (!this.dueDate || this.completed) return false;
        return new Date() > new Date(this.dueDate);
    }
    
    getDaysUntilDue() {
        if (!this.dueDate) return null;
        const today = new Date();
        const due = new Date(this.dueDate);
        const diffTime = due - today;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        return diffDays;
    }
    
    toJSON() {
        return {
            id: this.id,
            title: this.title,
            description: this.description,
            priority: this.priority,
            dueDate: this.dueDate,
            completed: this.completed,
            createdAt: this.createdAt,
            updatedAt: this.updatedAt
        };
    }
    
    static fromJSON(data) {
        const task = new Task(data.title, data.description, data.priority, data.dueDate);
        task.id = data.id;
        task.completed = data.completed;
        task.createdAt = new Date(data.createdAt);
        task.updatedAt = new Date(data.updatedAt);
        return task;
    }
}
```

### **TaskManager.js**
```javascript
import { Task } from './Task.js';

export class TaskManager {
    constructor() {
        this.tasks = new Map();
        this.loadFromStorage();
    }
    
    addTask(title, description, priority, dueDate) {
        const task = new Task(title, description, priority, dueDate);
        this.tasks.set(task.id, task);
        this.saveToStorage();
        return task;
    }
    
    getTask(id) {
        return this.tasks.get(id);
    }
    
    getAllTasks() {
        return Array.from(this.tasks.values());
    }
    
    getTasksByStatus(completed) {
        return this.getAllTasks().filter(task => task.completed === completed);
    }
    
    getTasksByPriority(priority) {
        return this.getAllTasks().filter(task => task.priority === priority);
    }
    
    getOverdueTasks() {
        return this.getAllTasks().filter(task => task.isOverdue());
    }
    
    updateTask(id, updates) {
        const task = this.tasks.get(id);
        if (task) {
            task.update(updates);
            this.saveToStorage();
            return true;
        }
        return false;
    }
    
    deleteTask(id) {
        const deleted = this.tasks.delete(id);
        if (deleted) {
            this.saveToStorage();
        }
        return deleted;
    }
    
    toggleTaskComplete(id) {
        const task = this.tasks.get(id);
        if (task) {
            task.toggleComplete();
            this.saveToStorage();
            return true;
        }
        return false;
    }
    
    getTaskStatistics() {
        const tasks = this.getAllTasks();
        const total = tasks.length;
        const completed = tasks.filter(task => task.completed).length;
        const pending = total - completed;
        const overdue = tasks.filter(task => task.isOverdue()).length;
        
        const priorityStats = tasks.reduce((acc, task) => {
            acc[task.priority] = (acc[task.priority] || 0) + 1;
            return acc;
        }, {});
        
        return {
            total,
            completed,
            pending,
            overdue,
            priorityStats,
            completionRate: total > 0 ? (completed / total * 100).toFixed(1) : 0
        };
    }
    
    searchTasks(query) {
        const searchTerm = query.toLowerCase();
        return this.getAllTasks().filter(task => 
            task.title.toLowerCase().includes(searchTerm) ||
            task.description.toLowerCase().includes(searchTerm)
        );
    }
    
    sortTasks(criteria = 'createdAt', order = 'desc') {
        const tasks = this.getAllTasks();
        const sortOrder = order === 'asc' ? 1 : -1;
        
        return tasks.sort((a, b) => {
            let aValue = a[criteria];
            let bValue = b[criteria];
            
            if (criteria === 'dueDate') {
                if (!aValue) aValue = new Date('9999-12-31');
                if (!bValue) bValue = new Date('9999-12-31');
            }
            
            if (aValue < bValue) return -1 * sortOrder;
            if (aValue > bValue) return 1 * sortOrder;
            return 0;
        });
    }
    
    saveToStorage() {
        try {
            const tasksData = Array.from(this.tasks.values()).map(task => task.toJSON());
            localStorage.setItem('tasks', JSON.stringify(tasksData));
        } catch (error) {
            console.error('Error saving to localStorage:', error);
        }
    }
    
    loadFromStorage() {
        try {
            const tasksData = JSON.parse(localStorage.getItem('tasks') || '[]');
            this.tasks.clear();
            tasksData.forEach(data => {
                const task = Task.fromJSON(data);
                this.tasks.set(task.id, task);
            });
        } catch (error) {
            console.error('Error loading from localStorage:', error);
        }
    }
    
    exportToJSON() {
        const tasksData = this.getAllTasks().map(task => task.toJSON());
        const dataStr = JSON.stringify(tasksData, null, 2);
        const dataBlob = new Blob([dataStr], { type: 'application/json' });
        
        const link = document.createElement('a');
        link.href = URL.createObjectURL(dataBlob);
        link.download = `tasks-${new Date().toISOString().split('T')[0]}.json`;
        link.click();
    }
    
    importFromJSON(file) {
        return new Promise((resolve, reject) => {
            const reader = new FileReader();
            
            reader.onload = (event) => {
                try {
                    const tasksData = JSON.parse(event.target.result);
                    const importedTasks = [];
                    
                    tasksData.forEach(data => {
                        try {
                            const task = Task.fromJSON(data);
                            this.tasks.set(task.id, task);
                            importedTasks.push(task);
                        } catch (error) {
                            console.warn('Error importing task:', error);
                        }
                    });
                    
                    this.saveToStorage();
                    resolve(importedTasks);
                } catch (error) {
                    reject(new Error('Invalid JSON file'));
                }
            };
            
            reader.onerror = () => reject(new Error('Error reading file'));
            reader.readAsText(file);
        });
    }
}
```

### **utils.js**
```javascript
export const formatDate = (date) => {
    if (!date) return 'Sin fecha';
    
    const d = new Date(date);
    const now = new Date();
    const diffTime = d - now;
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    
    if (diffDays === 0) return 'Hoy';
    if (diffDays === 1) return 'Mañana';
    if (diffDays === -1) return 'Ayer';
    if (diffDays > 0) return `En ${diffDays} días`;
    if (diffDays < 0) return `Hace ${Math.abs(diffDays)} días`;
    
    return d.toLocaleDateString('es-ES', {
        year: 'numeric',
        month: 'short',
        day: 'numeric'
    });
};

export const getPriorityColor = (priority) => {
    const colors = {
        low: '#10b981',
        medium: '#f59e0b',
        high: '#ef4444',
        urgent: '#7c3aed'
    };
    return colors[priority] || colors.medium;
};

export const getPriorityLabel = (priority) => {
    const labels = {
        low: 'Baja',
        medium: 'Media',
        high: 'Alta',
        urgent: 'Urgente'
    };
    return labels[priority] || priority;
};

export const debounce = (func, wait) => {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
};

export const throttle = (func, limit) => {
    let inThrottle;
    return function() {
        const args = arguments;
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
};

export const generateId = () => {
    return Date.now().toString(36) + Math.random().toString(36).substr(2);
};
```

### **main.js**
```javascript
import { TaskManager } from './TaskManager.js';
import { formatDate, getPriorityColor, getPriorityLabel, debounce } from './utils.js';

class TaskApp {
    constructor() {
        this.taskManager = new TaskManager();
        this.currentFilter = 'all';
        this.currentSort = 'createdAt';
        this.currentSortOrder = 'desc';
        
        this.init();
    }
    
    init() {
        this.bindEvents();
        this.renderTasks();
        this.renderStatistics();
        this.setupSearch();
    }
    
    bindEvents() {
        // Form de nueva tarea
        document.getElementById('taskForm').addEventListener('submit', (e) => {
            e.preventDefault();
            this.handleAddTask();
        });
        
        // Filtros
        document.getElementById('filterAll').addEventListener('click', () => this.setFilter('all'));
        document.getElementById('filterPending').addEventListener('click', () => this.setFilter('pending'));
        document.getElementById('filterCompleted').addEventListener('click', () => this.setFilter('completed'));
        document.getElementById('filterOverdue').addEventListener('click', () => this.setFilter('overdue'));
        
        // Ordenamiento
        document.getElementById('sortSelect').addEventListener('change', (e) => {
            this.setSort(e.target.value);
        });
        
        // Botones de acción
        document.getElementById('exportBtn').addEventListener('click', () => this.exportTasks());
        document.getElementById('importBtn').addEventListener('click', () => this.importTasks());
    }
    
    handleAddTask() {
        const formData = new FormData(document.getElementById('taskForm'));
        const title = formData.get('title').trim();
        const description = formData.get('description').trim();
        const priority = formData.get('priority');
        const dueDate = formData.get('dueDate') || null;
        
        if (!title) {
            this.showNotification('El título es requerido', 'error');
            return;
        }
        
        const task = this.taskManager.addTask(title, description, priority, dueDate);
        this.showNotification('Tarea creada exitosamente', 'success');
        
        document.getElementById('taskForm').reset();
        this.renderTasks();
        this.renderStatistics();
    }
    
    renderTasks() {
        const container = document.getElementById('tasksContainer');
        let tasks = this.getFilteredTasks();
        tasks = this.taskManager.sortTasks(this.currentSort, this.currentSortOrder);
        
        if (tasks.length === 0) {
            container.innerHTML = '<p class="no-tasks">No hay tareas que mostrar</p>';
            return;
        }
        
        container.innerHTML = tasks.map(task => this.renderTaskCard(task)).join('');
        this.bindTaskEvents();
    }
    
    renderTaskCard(task) {
        const overdueClass = task.isOverdue() ? 'overdue' : '';
        const completedClass = task.completed ? 'completed' : '';
        const priorityColor = getPriorityColor(task.priority);
        
        return `
            <div class="task-card ${overdueClass} ${completedClass}" data-task-id="${task.id}">
                <div class="task-header">
                    <div class="task-priority" style="background-color: ${priorityColor}">
                        ${getPriorityLabel(task.priority)}
                    </div>
                    <div class="task-actions">
                        <button class="btn-icon toggle-btn" title="Marcar como completada">
                            ${task.completed ? '✓' : '○'}
                        </button>
                        <button class="btn-icon edit-btn" title="Editar tarea">✏️</button>
                        <button class="btn-icon delete-btn" title="Eliminar tarea">🗑️</button>
                    </div>
                </div>
                
                <div class="task-content">
                    <h3 class="task-title">${task.title}</h3>
                    ${task.description ? `<p class="task-description">${task.description}</p>` : ''}
                    
                    <div class="task-meta">
                        <span class="task-date">
                            📅 ${formatDate(task.dueDate)}
                        </span>
                        <span class="task-created">
                            Creada: ${formatDate(task.createdAt)}
                        </span>
                    </div>
                </div>
            </div>
        `;
    }
    
    bindTaskEvents() {
        // Toggle completado
        document.querySelectorAll('.toggle-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const taskId = e.target.closest('.task-card').dataset.taskId;
                this.taskManager.toggleTaskComplete(taskId);
                this.renderTasks();
                this.renderStatistics();
            });
        });
        
        // Editar tarea
        document.querySelectorAll('.edit-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const taskId = e.target.closest('.task-card').dataset.taskId;
                this.editTask(taskId);
            });
        });
        
        // Eliminar tarea
        document.querySelectorAll('.delete-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const taskId = e.target.closest('.task-card').dataset.taskId;
                this.deleteTask(taskId);
            });
        });
    }
    
    getFilteredTasks() {
        switch (this.currentFilter) {
            case 'pending':
                return this.taskManager.getTasksByStatus(false);
            case 'completed':
                return this.taskManager.getTasksByStatus(true);
            case 'overdue':
                return this.taskManager.getTasksByStatus(false).filter(task => task.isOverdue());
            default:
                return this.taskManager.getAllTasks();
        }
    }
    
    setFilter(filter) {
        this.currentFilter = filter;
        document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
        document.getElementById(`filter${filter.charAt(0).toUpperCase() + filter.slice(1)}`).classList.add('active');
        this.renderTasks();
    }
    
    setSort(sort) {
        this.currentSort = sort;
        this.renderTasks();
    }
    
    renderStatistics() {
        const stats = this.taskManager.getTaskStatistics();
        const container = document.getElementById('statistics');
        
        container.innerHTML = `
            <div class="stat-card">
                <h3>Total</h3>
                <p class="stat-number">${stats.total}</p>
            </div>
            <div class="stat-card">
                <h3>Completadas</h3>
                <p class="stat-number">${stats.completed}</p>
            </div>
            <div class="stat-card">
                <h3>Pendientes</h3>
                <p class="stat-number">${stats.pending}</p>
            </div>
            <div class="stat-card">
                <h3>Vencidas</h3>
                <p class="stat-number">${stats.overdue}</p>
            </div>
            <div class="stat-card">
                <h3>Completado</h3>
                <p class="stat-number">${stats.completionRate}%</p>
            </div>
        `;
    }
    
    setupSearch() {
        const searchInput = document.getElementById('searchInput');
        const debouncedSearch = debounce((query) => {
            this.performSearch(query);
        }, 300);
        
        searchInput.addEventListener('input', (e) => {
            debouncedSearch(e.target.value);
        });
    }
    
    performSearch(query) {
        if (!query.trim()) {
            this.renderTasks();
            return;
        }
        
        const results = this.taskManager.searchTasks(query);
        const container = document.getElementById('tasksContainer');
        
        if (results.length === 0) {
            container.innerHTML = '<p class="no-tasks">No se encontraron tareas</p>';
            return;
        }
        
        container.innerHTML = results.map(task => this.renderTaskCard(task)).join('');
        this.bindTaskEvents();
    }
    
    editTask(taskId) {
        const task = this.taskManager.getTask(taskId);
        if (!task) return;
        
        // Implementar modal de edición
        this.showEditModal(task);
    }
    
    deleteTask(taskId) {
        if (confirm('¿Estás seguro de que quieres eliminar esta tarea?')) {
            this.taskManager.deleteTask(taskId);
            this.renderTasks();
            this.renderStatistics();
            this.showNotification('Tarea eliminada', 'success');
        }
    }
    
    exportTasks() {
        this.taskManager.exportToJSON();
        this.showNotification('Tareas exportadas exitosamente', 'success');
    }
    
    async importTasks() {
        const input = document.createElement('input');
        input.type = 'file';
        input.accept = '.json';
        
        input.onchange = async (e) => {
            const file = e.target.files[0];
            if (!file) return;
            
            try {
                const importedTasks = await this.taskManager.importFromJSON(file);
                this.showNotification(`${importedTasks.length} tareas importadas`, 'success');
                this.renderTasks();
                this.renderStatistics();
            } catch (error) {
                this.showNotification('Error al importar tareas', 'error');
            }
        };
        
        input.click();
    }
    
    showNotification(message, type = 'info') {
        const notification = document.createElement('div');
        notification.className = `notification ${type}`;
        notification.textContent = message;
        
        document.body.appendChild(notification);
        
        setTimeout(() => {
            notification.remove();
        }, 3000);
    }
    
    showEditModal(task) {
        // Implementar modal de edición
        console.log('Editar tarea:', task);
    }
}

// Inicializar la aplicación
document.addEventListener('DOMContentLoaded', () => {
    new TaskApp();
});
```

---

## 🚀 **EJERCICIO 2: API CLIENT CON FETCH Y ASYNC/AWAIT**

### **Descripción**
Crea un cliente de API completo usando fetch, async/await y manejo de errores moderno.

### **Requisitos**
- Clase ApiClient con métodos HTTP
- Interceptores de request/response
- Manejo de errores con try/catch
- Cache de respuestas
- Retry automático en fallos

### **ApiClient.js**
```javascript
export class ApiClient {
    constructor(baseURL, options = {}) {
        this.baseURL = baseURL;
        this.options = {
            timeout: 10000,
            retries: 3,
            retryDelay: 1000,
            enableCache: true,
            cacheTTL: 5 * 60 * 1000, // 5 minutos
            ...options
        };
        
        this.cache = new Map();
        this.interceptors = {
            request: [],
            response: []
        };
    }
    
    // Métodos HTTP principales
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
    
    // Método principal de request
    async request(endpoint, options = {}) {
        const url = this.buildUrl(endpoint);
        const config = this.buildConfig(options);
        
        // Aplicar interceptores de request
        const interceptedConfig = await this.applyRequestInterceptors(config);
        
        // Verificar cache para GET requests
        if (config.method === 'GET' && this.options.enableCache) {
            const cached = this.getFromCache(url);
            if (cached) return cached;
        }
        
        // Intentar request con retry
        let lastError;
        for (let attempt = 1; attempt <= this.options.retries; attempt++) {
            try {
                const response = await this.makeRequest(url, interceptedConfig);
                const result = await this.processResponse(response);
                
                // Cachear respuesta exitosa
                if (config.method === 'GET' && this.options.enableCache) {
                    this.setCache(url, result);
                }
                
                return result;
            } catch (error) {
                lastError = error;
                
                if (attempt < this.options.retries) {
                    await this.delay(this.options.retryDelay * attempt);
                    continue;
                }
            }
        }
        
        throw lastError;
    }
    
    async makeRequest(url, config) {
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), this.options.timeout);
        
        try {
            const response = await fetch(url, {
                ...config,
                signal: controller.signal
            });
            
            clearTimeout(timeoutId);
            return response;
        } catch (error) {
            clearTimeout(timeoutId);
            if (error.name === 'AbortError') {
                throw new Error('Request timeout');
            }
            throw error;
        }
    }
    
    async processResponse(response) {
        // Aplicar interceptores de response
        const interceptedResponse = await this.applyResponseInterceptors(response);
        
        if (!interceptedResponse.ok) {
            const error = await this.createError(interceptedResponse);
            throw error;
        }
        
        const contentType = interceptedResponse.headers.get('content-type');
        if (contentType && contentType.includes('application/json')) {
            return await interceptedResponse.json();
        }
        
        return await interceptedResponse.text();
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
    
    buildUrl(endpoint) {
        if (endpoint.startsWith('http')) {
            return endpoint;
        }
        
        const url = new URL(endpoint, this.baseURL);
        return url.toString();
    }
    
    buildConfig(options) {
        const defaultConfig = {
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            }
        };
        
        return { ...defaultConfig, ...options };
    }
    
    // Interceptores
    addRequestInterceptor(interceptor) {
        this.interceptors.request.push(interceptor);
    }
    
    addResponseInterceptor(interceptor) {
        this.interceptors.response.push(interceptor);
    }
    
    async applyRequestInterceptors(config) {
        let interceptedConfig = config;
        
        for (const interceptor of this.interceptors.request) {
            interceptedConfig = await interceptor(interceptedConfig);
        }
        
        return interceptedConfig;
    }
    
    async applyResponseInterceptors(response) {
        let interceptedResponse = response;
        
        for (const interceptor of this.interceptors.response) {
            interceptedResponse = await interceptor(interceptedResponse);
        }
        
        return interceptedResponse;
    }
    
    // Cache
    getFromCache(key) {
        const cached = this.cache.get(key);
        if (!cached) return null;
        
        if (Date.now() > cached.expiry) {
            this.cache.delete(key);
            return null;
        }
        
        return cached.data;
    }
    
    setCache(key, data) {
        this.cache.set(key, {
            data,
            expiry: Date.now() + this.options.cacheTTL
        });
    }
    
    clearCache() {
        this.cache.clear();
    }
    
    // Utilidades
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    // Métodos de conveniencia para APIs comunes
    async getUsers() {
        return this.get('/users');
    }
    
    async getUser(id) {
        return this.get(`/users/${id}`);
    }
    
    async createUser(userData) {
        return this.post('/users', userData);
    }
    
    async updateUser(id, userData) {
        return this.put(`/users/${id}`, userData);
    }
    
    async deleteUser(id) {
        return this.delete(`/users/${id}`);
    }
    
    async searchUsers(query, filters = {}) {
        const params = new URLSearchParams({ q: query, ...filters });
        return this.get(`/users/search?${params}`);
    }
}

// Uso del cliente
const apiClient = new ApiClient('https://api.example.com', {
    timeout: 15000,
    retries: 3,
    enableCache: true
});

// Agregar interceptor para autenticación
apiClient.addRequestInterceptor(async (config) => {
    const token = localStorage.getItem('authToken');
    if (token) {
        config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
});

// Agregar interceptor para logging
apiClient.addResponseInterceptor(async (response) => {
    console.log(`${response.method} ${response.url} - ${response.status}`);
    return response;
});

// Ejemplo de uso
async function example() {
    try {
        // Obtener usuarios
        const users = await apiClient.getUsers();
        console.log('Usuarios:', users);
        
        // Crear usuario
        const newUser = await apiClient.createUser({
            name: 'Juan Pérez',
            email: 'juan@example.com'
        });
        console.log('Usuario creado:', newUser);
        
        // Buscar usuarios
        const searchResults = await apiClient.searchUsers('Juan', { role: 'admin' });
        console.log('Resultados de búsqueda:', searchResults);
        
    } catch (error) {
        console.error('Error en API:', error);
    }
}
```

---

## 🚀 **EJERCICIO 3: SISTEMA DE EVENTOS PERSONALIZADO**

### **Descripción**
Crea un sistema de eventos personalizado similar a EventEmitter de Node.js usando JavaScript moderno.

### **Requisitos**
- Clase EventEmitter con métodos on, off, emit
- Soporte para eventos únicos (once)
- Prioridades de eventos
- Eventos con namespace
- Limpieza automática de listeners

### **EventEmitter.js**
```javascript
export class EventEmitter {
    constructor() {
        this.events = new Map();
        this.onceEvents = new Set();
        this.maxListeners = 10;
    }
    
    on(eventName, listener, options = {}) {
        const { priority = 0, namespace = null } = options;
        
        if (!this.events.has(eventName)) {
            this.events.set(eventName, []);
        }
        
        const listeners = this.events.get(eventName);
        
        // Verificar límite de listeners
        if (listeners.length >= this.maxListeners) {
            console.warn(`MaxListenersExceededWarning: ${listeners.length} listeners added for event '${eventName}'`);
        }
        
        const listenerObj = {
            fn: listener,
            priority,
            namespace,
            id: this.generateListenerId()
        };
        
        listeners.push(listenerObj);
        
        // Ordenar por prioridad (mayor prioridad primero)
        listeners.sort((a, b) => b.priority - a.priority);
        
        return listenerObj.id;
    }
    
    once(eventName, listener, options = {}) {
        const wrappedListener = (...args) => {
            this.off(eventName, wrappedListener);
            listener.apply(this, args);
        };
        
        const listenerId = this.on(eventName, wrappedListener, options);
        this.onceEvents.add(listenerId);
        
        return listenerId;
    }
    
    off(eventName, listenerOrId) {
        if (!this.events.has(eventName)) return false;
        
        const listeners = this.events.get(eventName);
        
        if (typeof listenerOrId === 'string') {
            // Remover por ID
            const index = listeners.findIndex(l => l.id === listenerOrId);
            if (index !== -1) {
                const removed = listeners.splice(index, 1)[0];
                this.onceEvents.delete(removed.id);
                return true;
            }
        } else {
            // Remover por función
            const index = listeners.findIndex(l => l.fn === listenerOrId);
            if (index !== -1) {
                const removed = listeners.splice(index, 1)[0];
                this.onceEvents.delete(removed.id);
                return true;
            }
        }
        
        return false;
    }
    
    emit(eventName, ...args) {
        if (!this.events.has(eventName)) return false;
        
        const listeners = this.events.get(eventName);
        const results = [];
        
        for (const listener of listeners) {
            try {
                const result = listener.fn.apply(this, args);
                results.push(result);
            } catch (error) {
                console.error(`Error in event listener for '${eventName}':`, error);
                this.emit('error', error, eventName);
            }
        }
        
        return results;
    }
    
    emitAsync(eventName, ...args) {
        if (!this.events.has(eventName)) return Promise.resolve([]);
        
        const listeners = this.events.get(eventName);
        const promises = listeners.map(listener => {
            return Promise.resolve().then(() => {
                try {
                    return listener.fn.apply(this, args);
                } catch (error) {
                    console.error(`Error in async event listener for '${eventName}':`, error);
                    this.emit('error', error, eventName);
                    throw error;
                }
            });
        });
        
        return Promise.all(promises);
    }
    
    // Eventos con namespace
    onNamespace(namespace, eventName, listener, options = {}) {
        return this.on(eventName, listener, { ...options, namespace });
    }
    
    offNamespace(namespace, eventName) {
        if (!this.events.has(eventName)) return 0;
        
        const listeners = this.events.get(eventName);
        const initialLength = listeners.length;
        
        for (let i = listeners.length - 1; i >= 0; i--) {
            if (listeners[i].namespace === namespace) {
                const removed = listeners.splice(i, 1)[0];
                this.onceEvents.delete(removed.id);
            }
        }
        
        return initialLength - listeners.length;
    }
    
    // Métodos de utilidad
    listenerCount(eventName) {
        return this.events.has(eventName) ? this.events.get(eventName).length : 0;
    }
    
    eventNames() {
        return Array.from(this.events.keys());
    }
    
    listeners(eventName) {
        if (!this.events.has(eventName)) return [];
        return this.events.get(eventName).map(l => l.fn);
    }
    
    removeAllListeners(eventName) {
        if (eventName) {
            if (this.events.has(eventName)) {
                const listeners = this.events.get(eventName);
                listeners.forEach(l => this.onceEvents.delete(l.id));
                this.events.delete(eventName);
            }
        } else {
            this.events.clear();
            this.onceEvents.clear();
        }
    }
    
    setMaxListeners(n) {
        this.maxListeners = n;
        return this;
    }
    
    getMaxListeners() {
        return this.maxListeners;
    }
    
    // Eventos especiales
    prependListener(eventName, listener, options = {}) {
        const { priority = 0, namespace = null } = options;
        
        if (!this.events.has(eventName)) {
            this.events.set(eventName, []);
        }
        
        const listeners = this.events.get(eventName);
        
        const listenerObj = {
            fn: listener,
            priority,
            namespace,
            id: this.generateListenerId()
        };
        
        listeners.unshift(listenerObj);
        return listenerObj.id;
    }
    
    // Limpieza automática
    cleanup() {
        const now = Date.now();
        
        for (const [eventName, listeners] of this.events) {
            const activeListeners = listeners.filter(l => {
                if (l.expiry && now > l.expiry) {
                    this.onceEvents.delete(l.id);
                    return false;
                }
                return true;
            });
            
            if (activeListeners.length === 0) {
                this.events.delete(eventName);
            } else {
                this.events.set(eventName, activeListeners);
            }
        }
    }
    
    // Utilidades privadas
    generateListenerId() {
        return `listener_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    }
}

// Ejemplo de uso
const emitter = new EventEmitter();

// Listener con prioridad alta
emitter.on('user:created', (user) => {
    console.log('High priority: User created:', user);
}, { priority: 10 });

// Listener normal
emitter.on('user:created', (user) => {
    console.log('Normal priority: User created:', user);
});

// Listener con namespace
emitter.onNamespace('auth', 'user:created', (user) => {
    console.log('Auth namespace: User created:', user);
});

// Evento único
emitter.once('app:ready', () => {
    console.log('App is ready!');
});

// Emitir eventos
emitter.emit('user:created', { id: 1, name: 'Juan' });
emitter.emit('app:ready');

// Limpiar listeners por namespace
emitter.offNamespace('auth', 'user:created');
```

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 4: Sistema de Validación de Formularios**
Crea un sistema de validación robusto usando clases ES6 y validadores personalizables.

### **Ejercicio 5: Router SPA Simple**
Implementa un router de página única (SPA) con navegación y manejo de rutas.

### **Ejercicio 6: Sistema de Estado Global**
Crea un store de estado global similar a Redux usando JavaScript moderno.

### **Ejercicio 7: WebSocket Client**
Implementa un cliente WebSocket con reconexión automática y manejo de eventos.

### **Ejercicio 8: Sistema de Caché Inteligente**
Crea un sistema de caché con expiración, prioridades y limpieza automática.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Uso de ES6+ (30%)**
- Clases y módulos
- Destructuring y spread
- Arrow functions
- Async/await

### **Arquitectura y Patrones (25%)**
- Separación de responsabilidades
- Patrones de diseño
- Manejo de errores
- Testing

### **Funcionalidad (25%)**
- Características implementadas
- Robustez del código
- Manejo de casos edge
- Performance

### **Calidad del Código (20%)**
- Legibilidad
- Documentación
- Consistencia
- Reutilización

---

## 💡 **CONSEJOS PARA LA PRÁCTICA**

1. **Usa ES6+ features**: Aprovecha las características modernas de JavaScript
2. **Maneja errores**: Implementa try/catch y manejo de errores robusto
3. **Testea tu código**: Escribe tests para validar funcionalidad
4. **Optimiza performance**: Usa técnicas como debouncing y caching
5. **Documenta bien**: Comenta funciones complejas y APIs

---

*Estos ejercicios te ayudarán a dominar JavaScript ES6+ moderno, creando aplicaciones robustas, mantenibles y escalables.* 