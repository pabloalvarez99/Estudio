# ⚛️ Proyecto: Aplicación React Completa

## 🎯 **OBJETIVO**
Crear una aplicación React completa y funcional que demuestre el dominio de componentes, hooks, estado, routing y patrones modernos de React.

---

## 📋 **DESCRIPCIÓN DEL PROYECTO**

### **Visión General**
Desarrollar una aplicación de gestión de tareas (Task Manager) con funcionalidades avanzadas, demostrando arquitectura React moderna, manejo de estado complejo y componentes reutilizables.

### **Características Principales**
- **Arquitectura Modular**: Componentes bien estructurados y reutilizables
- **Estado Global**: Context API y hooks personalizados
- **Routing Avanzado**: Navegación con React Router
- **Formularios Inteligentes**: Validación y manejo de estado
- **Persistencia Local**: LocalStorage y sincronización
- **Temas Personalizables**: Sistema de temas claro/oscuro
- **Responsive Design**: Adaptable a todos los dispositivos

---

## 🏗️ **ESTRUCTURA DEL PROYECTO**

### **Organización de Archivos**
```
task-manager-app/
├── public/
│   ├── index.html
│   ├── manifest.json
│   ├── favicon.ico
│   └── images/
├── src/
│   ├── components/
│   │   ├── layout/
│   │   │   ├── AppLayout.jsx
│   │   │   ├── Header.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   └── Footer.jsx
│   │   ├── tasks/
│   │   │   ├── TaskList.jsx
│   │   │   ├── TaskCard.jsx
│   │   │   ├── TaskForm.jsx
│   │   │   ├── TaskFilters.jsx
│   │   │   └── TaskStats.jsx
│   │   ├── projects/
│   │   │   ├── ProjectList.jsx
│   │   │   ├── ProjectCard.jsx
│   │   │   ├── ProjectForm.jsx
│   │   │   └── ProjectDashboard.jsx
│   │   ├── common/
│   │   │   ├── Button.jsx
│   │   │   ├── Modal.jsx
│   │   │   ├── Input.jsx
│   │   │   ├── Select.jsx
│   │   │   ├── Loading.jsx
│   │   │   └── ErrorBoundary.jsx
│   │   └── ui/
│   │       ├── ThemeToggle.jsx
│   │       ├── Notification.jsx
│   │       ├── ProgressBar.jsx
│   │       └── Calendar.jsx
│   ├── hooks/
│   │   ├── useTasks.js
│   │   ├── useProjects.js
│   │   ├── useLocalStorage.js
│   │   ├── useDebounce.js
│   │   ├── useIntersection.js
│   │   └── useTheme.js
│   ├── context/
│   │   ├── AppContext.jsx
│   │   ├── TaskContext.jsx
│   │   ├── ProjectContext.jsx
│   │   └── ThemeContext.jsx
│   ├── pages/
│   │   ├── Dashboard.jsx
│   │   ├── Tasks.jsx
│   │   ├── Projects.jsx
│   │   ├── Calendar.jsx
│   │   └── Settings.jsx
│   ├── utils/
│   │   ├── validators.js
│   │   ├── helpers.js
│   │   ├── constants.js
│   │   └── storage.js
│   ├── styles/
│   │   ├── components/
│   │   ├── layout/
│   │   ├── themes/
│   │   └── main.css
│   ├── App.jsx
│   ├── index.js
│   └── routes.jsx
├── package.json
└── README.md
```

---

## ⚛️ **ARQUITECTURA REACT**

### **1. App.jsx (Componente Principal)**
```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import { ThemeProvider } from './context/ThemeContext';
import { AppProvider } from './context/AppContext';
import { TaskProvider } from './context/TaskContext';
import { ProjectProvider } from './context/ProjectContext';
import AppLayout from './components/layout/AppLayout';
import Loading from './components/common/Loading';
import ErrorBoundary from './components/common/ErrorBoundary';
import './styles/main.css';

// Lazy loading de páginas
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Tasks = lazy(() => import('./pages/Tasks'));
const Projects = lazy(() => import('./pages/Projects'));
const Calendar = lazy(() => import('./pages/Calendar'));
const Settings = lazy(() => import('./pages/Settings'));

function App() {
  return (
    <ErrorBoundary>
      <ThemeProvider>
        <AppProvider>
          <TaskProvider>
            <ProjectProvider>
              <Router>
                <AppLayout>
                  <Suspense fallback={<Loading />}>
                    <Routes>
                      <Route path="/" element={<Dashboard />} />
                      <Route path="/tasks" element={<Tasks />} />
                      <Route path="/projects" element={<Projects />} />
                      <Route path="/calendar" element={<Calendar />} />
                      <Route path="/settings" element={<Settings />} />
                    </Routes>
                  </Suspense>
                </AppLayout>
              </Router>
            </ProjectProvider>
          </TaskProvider>
        </AppProvider>
      </ThemeProvider>
    </ErrorBoundary>
  );
}

export default App;
```

### **2. AppContext.jsx (Estado Global)**
```jsx
import React, { createContext, useContext, useReducer, useEffect } from 'react';
import { useLocalStorage } from '../hooks/useLocalStorage';

// Estado inicial
const initialState = {
  user: null,
  notifications: [],
  sidebarCollapsed: false,
  currentView: 'dashboard',
  searchQuery: '',
  filters: {
    status: 'all',
    priority: 'all',
    category: 'all'
  },
  sortBy: 'createdAt',
  sortOrder: 'desc'
};

// Tipos de acciones
const ACTIONS = {
  SET_USER: 'SET_USER',
  ADD_NOTIFICATION: 'ADD_NOTIFICATION',
  REMOVE_NOTIFICATION: 'REMOVE_NOTIFICATION',
  TOGGLE_SIDEBAR: 'TOGGLE_SIDEBAR',
  SET_CURRENT_VIEW: 'SET_CURRENT_VIEW',
  SET_SEARCH_QUERY: 'SET_SEARCH_QUERY',
  SET_FILTERS: 'SET_FILTERS',
  SET_SORT: 'SET_SORT',
  RESET_STATE: 'RESET_STATE'
};

// Reducer
function appReducer(state, action) {
  switch (action.type) {
    case ACTIONS.SET_USER:
      return { ...state, user: action.payload };
      
    case ACTIONS.ADD_NOTIFICATION:
      return {
        ...state,
        notifications: [...state.notifications, {
          id: Date.now(),
          ...action.payload
        }]
      };
      
    case ACTIONS.REMOVE_NOTIFICATION:
      return {
        ...state,
        notifications: state.notifications.filter(
          notif => notif.id !== action.payload
        )
      };
      
    case ACTIONS.TOGGLE_SIDEBAR:
      return { ...state, sidebarCollapsed: !state.sidebarCollapsed };
      
    case ACTIONS.SET_CURRENT_VIEW:
      return { ...state, currentView: action.payload };
      
    case ACTIONS.SET_SEARCH_QUERY:
      return { ...state, searchQuery: action.payload };
      
    case ACTIONS.SET_FILTERS:
      return { ...state, filters: { ...state.filters, ...action.payload } };
      
    case ACTIONS.SET_SORT:
      return { 
        ...state, 
        sortBy: action.payload.field, 
        sortOrder: action.payload.order 
      };
      
    case ACTIONS.RESET_STATE:
      return initialState;
      
    default:
      return state;
  }
}

// Context
const AppContext = createContext();

// Provider
export function AppProvider({ children }) {
  const [savedState, setSavedState] = useLocalStorage('appState', initialState);
  const [state, dispatch] = useReducer(appReducer, savedState);

  // Persistir estado en localStorage
  useEffect(() => {
    setSavedState(state);
  }, [state, setSavedState]);

  // Acciones
  const actions = {
    setUser: (user) => dispatch({ type: ACTIONS.SET_USER, payload: user }),
    
    addNotification: (notification) => 
      dispatch({ type: ACTIONS.ADD_NOTIFICATION, payload: notification }),
    
    removeNotification: (id) => 
      dispatch({ type: ACTIONS.REMOVE_NOTIFICATION, payload: id }),
    
    toggleSidebar: () => dispatch({ type: ACTIONS.TOGGLE_SIDEBAR }),
    
    setCurrentView: (view) => 
      dispatch({ type: ACTIONS.SET_CURRENT_VIEW, payload: view }),
    
    setSearchQuery: (query) => 
      dispatch({ type: ACTIONS.SET_SEARCH_QUERY, payload: query }),
    
    setFilters: (filters) => 
      dispatch({ type: ACTIONS.SET_FILTERS, payload: filters }),
    
    setSort: (field, order) => 
      dispatch({ type: ACTIONS.SET_SORT, payload: { field, order } }),
    
    resetState: () => dispatch({ type: ACTIONS.RESET_STATE })
  };

  const value = {
    state,
    actions
  };

  return (
    <AppContext.Provider value={value}>
      {children}
    </AppContext.Provider>
  );
}

// Hook personalizado
export function useApp() {
  const context = useContext(AppContext);
  if (!context) {
    throw new Error('useApp debe usarse dentro de AppProvider');
  }
  return context;
}
```

### **3. TaskContext.jsx (Gestión de Tareas)**
```jsx
import React, { createContext, useContext, useReducer, useEffect } from 'react';
import { useLocalStorage } from '../hooks/useLocalStorage';
import { generateId, formatDate } from '../utils/helpers';

// Estado inicial
const initialState = {
  tasks: [],
  loading: false,
  error: null,
  selectedTask: null,
  filters: {
    status: 'all',
    priority: 'all',
    category: 'all',
    assignee: 'all'
  },
  sortBy: 'dueDate',
  sortOrder: 'asc'
};

// Tipos de acciones
const TASK_ACTIONS = {
  SET_LOADING: 'SET_LOADING',
  SET_ERROR: 'SET_ERROR',
  SET_TASKS: 'SET_TASKS',
  ADD_TASK: 'ADD_TASK',
  UPDATE_TASK: 'UPDATE_TASK',
  DELETE_TASK: 'DELETE_TASK',
  SELECT_TASK: 'SELECT_TASK',
  SET_FILTERS: 'SET_FILTERS',
  SET_SORT: 'SET_SORT',
  BULK_UPDATE: 'BULK_UPDATE',
  CLEAR_COMPLETED: 'CLEAR_COMPLETED'
};

// Reducer
function taskReducer(state, action) {
  switch (action.type) {
    case TASK_ACTIONS.SET_LOADING:
      return { ...state, loading: action.payload };
      
    case TASK_ACTIONS.SET_ERROR:
      return { ...state, error: action.payload };
      
    case TASK_ACTIONS.SET_TASKS:
      return { ...state, tasks: action.payload };
      
    case TASK_ACTIONS.ADD_TASK:
      const newTask = {
        id: generateId(),
        createdAt: formatDate(new Date()),
        updatedAt: formatDate(new Date()),
        completed: false,
        ...action.payload
      };
      return { ...state, tasks: [newTask, ...state.tasks] };
      
    case TASK_ACTIONS.UPDATE_TASK:
      return {
        ...state,
        tasks: state.tasks.map(task =>
          task.id === action.payload.id
            ? { ...task, ...action.payload, updatedAt: formatDate(new Date()) }
            : task
        )
      };
      
    case TASK_ACTIONS.DELETE_TASK:
      return {
        ...state,
        tasks: state.tasks.filter(task => task.id !== action.payload)
      };
      
    case TASK_ACTIONS.SELECT_TASK:
      return { ...state, selectedTask: action.payload };
      
    case TASK_ACTIONS.SET_FILTERS:
      return {
        ...state,
        filters: { ...state.filters, ...action.payload }
      };
      
    case TASK_ACTIONS.SET_SORT:
      return {
        ...state,
        sortBy: action.payload.field,
        sortOrder: action.payload.order
      };
      
    case TASK_ACTIONS.BULK_UPDATE:
      return {
        ...state,
        tasks: state.tasks.map(task =>
          action.payload.ids.includes(task.id)
            ? { ...task, ...action.payload.updates, updatedAt: formatDate(new Date()) }
            : task
        )
      };
      
    case TASK_ACTIONS.CLEAR_COMPLETED:
      return {
        ...state,
        tasks: state.tasks.filter(task => !task.completed)
      };
      
    default:
      return state;
  }
}

// Context
const TaskContext = createContext();

// Provider
export function TaskProvider({ children }) {
  const [savedTasks, setSavedTasks] = useLocalStorage('tasks', []);
  const [state, dispatch] = useReducer(taskReducer, {
    ...initialState,
    tasks: savedTasks
  });

  // Persistir tareas en localStorage
  useEffect(() => {
    setSavedTasks(state.tasks);
  }, [state.tasks, setSavedTasks]);

  // Acciones
  const actions = {
    setLoading: (loading) => 
      dispatch({ type: TASK_ACTIONS.SET_LOADING, payload: loading }),
    
    setError: (error) => 
      dispatch({ type: TASK_ACTIONS.SET_ERROR, payload: error }),
    
    addTask: (taskData) => 
      dispatch({ type: TASK_ACTIONS.ADD_TASK, payload: taskData }),
    
    updateTask: (id, updates) => 
      dispatch({ type: TASK_ACTIONS.UPDATE_TASK, payload: { id, ...updates } }),
    
    deleteTask: (id) => 
      dispatch({ type: TASK_ACTIONS.DELETE_TASK, payload: id }),
    
    selectTask: (task) => 
      dispatch({ type: TASK_ACTIONS.SELECT_TASK, payload: task }),
    
    setFilters: (filters) => 
      dispatch({ type: TASK_ACTIONS.SET_FILTERS, payload: filters }),
    
    setSort: (field, order) => 
      dispatch({ type: TASK_ACTIONS.SET_SORT, payload: { field, order } }),
    
    bulkUpdate: (ids, updates) => 
      dispatch({ type: TASK_ACTIONS.BULK_UPDATE, payload: { ids, updates } }),
    
    clearCompleted: () => 
      dispatch({ type: TASK_ACTIONS.CLEAR_COMPLETED }),
    
    toggleTaskComplete: (id) => {
      const task = state.tasks.find(t => t.id === id);
      if (task) {
        dispatch({
          type: TASK_ACTIONS.UPDATE_TASK,
          payload: { id, completed: !task.completed }
        });
      }
    },
    
    duplicateTask: (id) => {
      const task = state.tasks.find(t => t.id === id);
      if (task) {
        const { id: _, createdAt, updatedAt, ...taskData } = task;
        dispatch({
          type: TASK_ACTIONS.ADD_TASK,
          payload: { ...taskData, title: `${taskData.title} (copia)` }
        });
      }
    }
  };

  // Computed values
  const computed = {
    filteredTasks: () => {
      let filtered = [...state.tasks];
      
      // Aplicar filtros
      if (state.filters.status !== 'all') {
        filtered = filtered.filter(task => 
          state.filters.status === 'completed' ? task.completed : !task.completed
        );
      }
      
      if (state.filters.priority !== 'all') {
        filtered = filtered.filter(task => task.priority === state.filters.priority);
      }
      
      if (state.filters.category !== 'all') {
        filtered = filtered.filter(task => task.category === state.filters.category);
      }
      
      if (state.filters.assignee !== 'all') {
        filtered = filtered.filter(task => task.assignee === state.filters.assignee);
      }
      
      // Aplicar ordenamiento
      filtered.sort((a, b) => {
        let aValue = a[state.sortBy];
        let bValue = b[state.sortBy];
        
        if (state.sortBy === 'dueDate') {
          aValue = aValue ? new Date(aValue) : new Date('9999-12-31');
          bValue = bValue ? new Date(bValue) : new Date('9999-12-31');
        }
        
        if (aValue < bValue) return state.sortOrder === 'asc' ? -1 : 1;
        if (aValue > bValue) return state.sortOrder === 'asc' ? 1 : -1;
        return 0;
      });
      
      return filtered;
    },
    
    taskStats: () => {
      const total = state.tasks.length;
      const completed = state.tasks.filter(t => t.completed).length;
      const pending = total - completed;
      const overdue = state.tasks.filter(t => 
        !t.completed && t.dueDate && new Date(t.dueDate) < new Date()
      ).length;
      
      return { total, completed, pending, overdue };
    }
  };

  const value = {
    state,
    actions,
    computed
  };

  return (
    <TaskContext.Provider value={value}>
      {children}
    </TaskContext.Provider>
  );
}

// Hook personalizado
export function useTasks() {
  const context = useContext(TaskContext);
  if (!context) {
    throw new Error('useTasks debe usarse dentro de TaskProvider');
  }
  return context;
}
```

---

## 🎨 **COMPONENTES REACT AVANZADOS**

### **1. TaskForm.jsx (Formulario Inteligente)**
```jsx
import React, { useState, useEffect } from 'react';
import { useForm } from '../../hooks/useForm';
import { taskValidationRules } from '../../utils/validators';
import { Button, Input, Select, Modal } from '../common';
import './TaskForm.css';

const TaskForm = ({ task = null, onSubmit, onCancel, isOpen }) => {
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const {
    values,
    errors,
    touched,
    setValue,
    setFieldTouched,
    validateForm,
    resetForm
  } = useForm(
    {
      title: task?.title || '',
      description: task?.description || '',
      priority: task?.priority || 'medium',
      category: task?.category || 'personal',
      dueDate: task?.dueDate || '',
      assignee: task?.assignee || '',
      tags: task?.tags || []
    },
    taskValidationRules
  );

  // Resetear formulario cuando cambia la tarea
  useEffect(() => {
    if (task) {
      Object.entries(task).forEach(([key, value]) => {
        if (values.hasOwnProperty(key)) {
          setValue(key, value);
        }
      });
    }
  }, [task, setValue]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (await validateForm()) {
      setIsSubmitting(true);
      try {
        await onSubmit(values);
        resetForm();
        onCancel();
      } catch (error) {
        console.error('Error al guardar tarea:', error);
      } finally {
        setIsSubmitting(false);
      }
    }
  };

  const handleCancel = () => {
    resetForm();
    onCancel();
  };

  const priorityOptions = [
    { value: 'low', label: 'Baja', color: '#10b981' },
    { value: 'medium', label: 'Media', color: '#f59e0b' },
    { value: 'high', label: 'Alta', color: '#ef4444' },
    { value: 'urgent', label: 'Urgente', color: '#dc2626' }
  ];

  const categoryOptions = [
    { value: 'personal', label: 'Personal' },
    { value: 'work', label: 'Trabajo' },
    { value: 'study', label: 'Estudio' },
    { value: 'health', label: 'Salud' },
    { value: 'finance', label: 'Finanzas' }
  ];

  return (
    <Modal isOpen={isOpen} onClose={handleCancel} title="Nueva Tarea">
      <form onSubmit={handleSubmit} className="task-form">
        {/* Título */}
        <div className="form-group">
          <label htmlFor="title" className="form-label">
            Título *
          </label>
          <Input
            id="title"
            type="text"
            value={values.title}
            onChange={(e) => setValue('title', e.target.value)}
            onBlur={() => setFieldTouched('title')}
            error={touched.title && errors.title}
            placeholder="¿Qué necesitas hacer?"
            required
          />
        </div>

        {/* Descripción */}
        <div className="form-group">
          <label htmlFor="description" className="form-label">
            Descripción
          </label>
          <textarea
            id="description"
            value={values.description}
            onChange={(e) => setValue('description', e.target.value)}
            onBlur={() => setFieldTouched('description')}
            className={`form-textarea ${touched.description && errors.description ? 'error' : ''}`}
            placeholder="Detalles adicionales..."
            rows="3"
          />
          {touched.description && errors.description && (
            <span className="form-error">{errors.description}</span>
          )}
        </div>

        {/* Fila de controles */}
        <div className="form-row">
          {/* Prioridad */}
          <div className="form-group">
            <label htmlFor="priority" className="form-label">
              Prioridad
            </label>
            <Select
              id="priority"
              value={values.priority}
              onChange={(e) => setValue('priority', e.target.value)}
              options={priorityOptions}
              renderOption={(option) => (
                <span style={{ color: option.color }}>
                  {option.label}
                </span>
              )}
            />
          </div>

          {/* Categoría */}
          <div className="form-group">
            <label htmlFor="category" className="form-label">
              Categoría
            </label>
            <Select
              id="category"
              value={values.category}
              onChange={(e) => setValue('category', e.target.value)}
              options={categoryOptions}
            />
          </div>
        </div>

        {/* Fila de controles */}
        <div className="form-row">
          {/* Fecha de vencimiento */}
          <div className="form-group">
            <label htmlFor="dueDate" className="form-label">
              Fecha de vencimiento
            </label>
            <Input
              id="dueDate"
              type="datetime-local"
              value={values.dueDate}
              onChange={(e) => setValue('dueDate', e.target.value)}
              min={new Date().toISOString().slice(0, 16)}
            />
          </div>

          {/* Asignado a */}
          <div className="form-group">
            <label htmlFor="assignee" className="form-label">
              Asignado a
            </label>
            <Input
              id="assignee"
              type="text"
              value={values.assignee}
              onChange={(e) => setValue('assignee', e.target.value)}
              placeholder="Tu nombre"
            />
          </div>
        </div>

        {/* Tags */}
        <div className="form-group">
          <label htmlFor="tags" className="form-label">
            Etiquetas
          </label>
          <Input
            id="tags"
            type="text"
            value={values.tags.join(', ')}
            onChange={(e) => {
              const tags = e.target.value.split(',').map(tag => tag.trim()).filter(Boolean);
              setValue('tags', tags);
            }}
            placeholder="tag1, tag2, tag3"
          />
          <small className="form-help">
            Separa las etiquetas con comas
          </small>
        </div>

        {/* Acciones del formulario */}
        <div className="form-actions">
          <Button
            type="button"
            variant="secondary"
            onClick={handleCancel}
            disabled={isSubmitting}
          >
            Cancelar
          </Button>
          
          <Button
            type="submit"
            variant="primary"
            loading={isSubmitting}
            disabled={isSubmitting}
          >
            {task ? 'Actualizar Tarea' : 'Crear Tarea'}
          </Button>
        </div>
      </form>
    </Modal>
  );
};

export default TaskForm;
```

---

## 🎯 **HOOKS PERSONALIZADOS AVANZADOS**

### **1. useDebounce.js (Hook de Debounce)**
```jsx
import { useState, useEffect, useRef } from 'react';

export function useDebounce(value, delay = 500) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  const timeoutRef = useRef(null);

  useEffect(() => {
    // Limpiar timeout anterior
    if (timeoutRef.current) {
      clearTimeout(timeoutRef.current);
    }

    // Crear nuevo timeout
    timeoutRef.current = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // Cleanup
    return () => {
      if (timeoutRef.current) {
        clearTimeout(timeoutRef.current);
      }
    };
  }, [value, delay]);

  return debouncedValue;
}
```

### **2. useIntersection.js (Hook de Intersection Observer)**
```jsx
import { useState, useEffect, useRef } from 'react';

export function useIntersection(options = {}) {
  const [isIntersecting, setIsIntersecting] = useState(false);
  const [hasIntersected, setHasIntersected] = useState(false);
  const elementRef = useRef(null);

  useEffect(() => {
    const element = elementRef.current;
    if (!element) return;

    const observer = new IntersectionObserver(
      ([entry]) => {
        const intersecting = entry.isIntersecting;
        setIsIntersecting(intersecting);
        
        if (intersecting && !hasIntersected) {
          setHasIntersected(true);
        }
      },
      {
        threshold: options.threshold || 0.1,
        rootMargin: options.rootMargin || '0px',
        ...options
      }
    );

    observer.observe(element);

    return () => {
      observer.unobserve(element);
    };
  }, [options, hasIntersected]);

  return [elementRef, isIntersecting, hasIntersected];
}
```

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Arquitectura React (30%)**
- Estructura de componentes
- Manejo de estado
- Context API
- Hooks personalizados

### **Funcionalidad (25%)**
- CRUD completo
- Filtros y búsqueda
- Validaciones
- Persistencia

### **Componentes (25%)**
- Reutilizabilidad
- Props y estado
- Eventos y callbacks
- Estilos y CSS

### **Performance (20%)**
- Lazy loading
- Memoización
- Optimizaciones
- Bundle size

---

## 💡 **CONSEJOS PARA LA IMPLEMENTACIÓN**

1. **Planifica la arquitectura**: Define la estructura antes de empezar
2. **Usa hooks personalizados**: Para lógica reutilizable
3. **Optimiza re-renders**: Con useMemo y useCallback
4. **Maneja errores**: Implementa error boundaries
5. **Testea componentes**: Escribe tests para funcionalidad crítica

---

*Este proyecto te permitirá demostrar el dominio completo de React, creando una aplicación robusta y escalable.* 