# 🔄 Estado en React: Gestión y Patrones de Estado

## 🎯 **OBJETIVO**
Dominar la gestión de estado en React, comprendiendo useState, useReducer, Context API y los patrones para manejar estado local y global de manera eficiente.

---

## 📊 **ESTADO LOCAL CON USESTATE**

### **Conceptos Básicos de useState**

#### **1. Estado Simple**
```jsx
import React, { useState } from 'react';

function Counter() {
    // Estado básico
    const [count, setCount] = useState(0);
    
    const increment = () => {
        setCount(count + 1);
    };
    
    const decrement = () => {
        setCount(count - 1);
    };
    
    const reset = () => {
        setCount(0);
    };
    
    return (
        <div className="counter">
            <h2>Contador: {count}</h2>
            <div className="counter-controls">
                <button onClick={decrement}>-</button>
                <button onClick={reset}>Reset</button>
                <button onClick={increment}>+</button>
            </div>
        </div>
    );
}
```

#### **2. Estado con Objetos**
```jsx
function UserForm() {
    // Estado con objeto
    const [user, setUser] = useState({
        name: '',
        email: '',
        age: '',
        city: ''
    });
    
    // Actualización de propiedades individuales
    const updateField = (field, value) => {
        setUser(prevUser => ({
            ...prevUser,
            [field]: value
        }));
    };
    
    // Actualización de múltiples campos
    const updateMultipleFields = (updates) => {
        setUser(prevUser => ({
            ...prevUser,
            ...updates
        }));
    };
    
    const handleSubmit = (e) => {
        e.preventDefault();
        console.log('Usuario:', user);
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <div className="form-group">
                <label>Nombre:</label>
                <input
                    type="text"
                    value={user.name}
                    onChange={(e) => updateField('name', e.target.value)}
                />
            </div>
            
            <div className="form-group">
                <label>Email:</label>
                <input
                    type="email"
                    value={user.email}
                    onChange={(e) => updateField('email', e.target.value)}
                />
            </div>
            
            <div className="form-group">
                <label>Edad:</label>
                <input
                    type="number"
                    value={user.age}
                    onChange={(e) => updateField('age', e.target.value)}
                />
            </div>
            
            <div className="form-group">
                <label>Ciudad:</label>
                <input
                    type="text"
                    value={user.city}
                    onChange={(e) => updateField('city', e.target.value)}
                />
            </div>
            
            <button type="submit">Guardar</button>
            
            {/* Botón para actualizar múltiples campos */}
            <button 
                type="button"
                onClick={() => updateMultipleFields({
                    age: '25',
                    city: 'Madrid'
                })}
            >
                Rellenar Campos
            </button>
        </form>
    );
}
```

#### **3. Estado con Arrays**
```jsx
function TodoList() {
    const [todos, setTodos] = useState([]);
    const [newTodo, setNewTodo] = useState('');
    
    // Agregar nuevo todo
    const addTodo = () => {
        if (newTodo.trim()) {
            const todo = {
                id: Date.now(),
                text: newTodo.trim(),
                completed: false,
                createdAt: new Date()
            };
            
            setTodos(prevTodos => [todo, ...prevTodos]);
            setNewTodo('');
        }
    };
    
    // Cambiar estado de completado
    const toggleTodo = (id) => {
        setTodos(prevTodos =>
            prevTodos.map(todo =>
                todo.id === id
                    ? { ...todo, completed: !todo.completed }
                    : todo
            )
        );
    };
    
    // Eliminar todo
    const deleteTodo = (id) => {
        setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id));
    };
    
    // Editar todo
    const editTodo = (id, newText) => {
        setTodos(prevTodos =>
            prevTodos.map(todo =>
                todo.id === id
                    ? { ...todo, text: newText }
                    : todo
            )
        );
    };
    
    // Filtrar todos
    const [filter, setFilter] = useState('all');
    
    const filteredTodos = todos.filter(todo => {
        if (filter === 'active') return !todo.completed;
        if (filter === 'completed') return todo.completed;
        return true;
    });
    
    return (
        <div className="todo-list">
            <h2>Lista de Tareas</h2>
            
            {/* Formulario para agregar */}
            <div className="add-todo">
                <input
                    type="text"
                    value={newTodo}
                    onChange={(e) => setNewTodo(e.target.value)}
                    placeholder="Nueva tarea..."
                    onKeyPress={(e) => e.key === 'Enter' && addTodo()}
                />
                <button onClick={addTodo}>Agregar</button>
            </div>
            
            {/* Filtros */}
            <div className="filters">
                <button 
                    className={filter === 'all' ? 'active' : ''}
                    onClick={() => setFilter('all')}
                >
                    Todas
                </button>
                <button 
                    className={filter === 'active' ? 'active' : ''}
                    onClick={() => setFilter('active')}
                >
                    Activas
                </button>
                <button 
                    className={filter === 'completed' ? 'active' : ''}
                    onClick={() => setFilter('completed')}
                >
                    Completadas
                </button>
            </div>
            
            {/* Lista de todos */}
            <ul className="todos">
                {filteredTodos.map(todo => (
                    <TodoItem
                        key={todo.id}
                        todo={todo}
                        onToggle={toggleTodo}
                        onDelete={deleteTodo}
                        onEdit={editTodo}
                    />
                ))}
            </ul>
            
            {/* Estadísticas */}
            <div className="stats">
                <p>Total: {todos.length}</p>
                <p>Completadas: {todos.filter(t => t.completed).length}</p>
                <p>Pendientes: {todos.filter(t => !t.completed).length}</p>
            </div>
        </div>
    );
}

// Componente individual de todo
function TodoItem({ todo, onToggle, onDelete, onEdit }) {
    const [isEditing, setIsEditing] = useState(false);
    const [editText, setEditText] = useState(todo.text);
    
    const handleEdit = () => {
        if (editText.trim() && editText !== todo.text) {
            onEdit(todo.id, editText.trim());
        }
        setIsEditing(false);
    };
    
    const cancelEdit = () => {
        setEditText(todo.text);
        setIsEditing(false);
    };
    
    return (
        <li className={`todo-item ${todo.completed ? 'completed' : ''}`}>
            <input
                type="checkbox"
                checked={todo.completed}
                onChange={() => onToggle(todo.id)}
            />
            
            {isEditing ? (
                <div className="edit-mode">
                    <input
                        type="text"
                        value={editText}
                        onChange={(e) => setEditText(e.target.value)}
                        onKeyPress={(e) => e.key === 'Enter' && handleEdit()}
                        autoFocus
                    />
                    <button onClick={handleEdit}>✓</button>
                    <button onClick={cancelEdit}>✗</button>
                </div>
            ) : (
                <span 
                    className="todo-text"
                    onDoubleClick={() => setIsEditing(true)}
                >
                    {todo.text}
                </span>
            )}
            
            <div className="todo-actions">
                <button onClick={() => setIsEditing(true)}>✏️</button>
                <button onClick={() => onDelete(todo.id)}>🗑️</button>
            </div>
        </li>
    );
}
```

---

## 🔄 **ESTADO COMPLEJO CON USEREDUCER**

### **Conceptos de useReducer**

#### **1. Reducer Básico**
```jsx
import React, { useReducer } from 'react';

// Estado inicial
const initialState = {
    count: 0,
    history: [],
    isLoading: false,
    error: null
};

// Tipos de acciones
const ACTIONS = {
    INCREMENT: 'INCREMENT',
    DECREMENT: 'DECREMENT',
    RESET: 'RESET',
    SET_LOADING: 'SET_LOADING',
    SET_ERROR: 'SET_ERROR',
    ADD_TO_HISTORY: 'ADD_TO_HISTORY'
};

// Función reducer
function counterReducer(state, action) {
    switch (action.type) {
        case ACTIONS.INCREMENT:
            return {
                ...state,
                count: state.count + 1,
                history: [...state.history, `Incrementado a ${state.count + 1}`]
            };
            
        case ACTIONS.DECREMENT:
            return {
                ...state,
                count: state.count - 1,
                history: [...state.history, `Decrementado a ${state.count - 1}`]
            };
            
        case ACTIONS.RESET:
            return {
                ...state,
                count: 0,
                history: [...state.history, 'Reset a 0']
            };
            
        case ACTIONS.SET_LOADING:
            return {
                ...state,
                isLoading: action.payload
            };
            
        case ACTIONS.SET_ERROR:
            return {
                ...state,
                error: action.payload
            };
            
        case ACTIONS.ADD_TO_HISTORY:
            return {
                ...state,
                history: [...state.history, action.payload]
            };
            
        default:
            return state;
    }
}

// Componente que usa useReducer
function AdvancedCounter() {
    const [state, dispatch] = useReducer(counterReducer, initialState);
    
    const increment = () => {
        dispatch({ type: ACTIONS.INCREMENT });
    };
    
    const decrement = () => {
        dispatch({ type: ACTIONS.DECREMENT });
    };
    
    const reset = () => {
        dispatch({ type: ACTIONS.RESET });
    };
    
    const addCustomValue = (value) => {
        dispatch({ 
            type: ACTIONS.ADD_TO_HISTORY, 
            payload: `Valor personalizado: ${value}` 
        });
    };
    
    const clearHistory = () => {
        dispatch({ 
            type: ACTIONS.ADD_TO_HISTORY, 
            payload: '--- Historial limpiado ---' 
        });
    };
    
    return (
        <div className="advanced-counter">
            <h2>Contador Avanzado: {state.count}</h2>
            
            <div className="counter-controls">
                <button onClick={decrement}>-</button>
                <button onClick={reset}>Reset</button>
                <button onClick={increment}>+</button>
            </div>
            
            <div className="custom-actions">
                <button onClick={() => addCustomValue(10)}>+10</button>
                <button onClick={() => addCustomValue(-5)}>-5</button>
                <button onClick={clearHistory}>Limpiar Historial</button>
            </div>
            
            <div className="history">
                <h3>Historial de Acciones:</h3>
                <ul>
                    {state.history.map((action, index) => (
                        <li key={index}>{action}</li>
                    ))}
                </ul>
            </div>
            
            {state.error && (
                <div className="error">
                    Error: {state.error}
                </div>
            )}
        </div>
    );
}
```

#### **2. Reducer para Formularios Complejos**
```jsx
// Estado inicial del formulario
const formInitialState = {
    fields: {
        name: '',
        email: '',
        age: '',
        city: '',
        interests: []
    },
    errors: {},
    touched: {},
    isSubmitting: false,
    isSubmitted: false
};

// Tipos de acciones del formulario
const FORM_ACTIONS = {
    UPDATE_FIELD: 'UPDATE_FIELD',
    SET_ERROR: 'SET_ERROR',
    CLEAR_ERROR: 'CLEAR_ERROR',
    SET_TOUCHED: 'SET_TOUCHED',
    SET_SUBMITTING: 'SET_SUBMITTING',
    SET_SUBMITTED: 'SET_SUBMITTED',
    RESET_FORM: 'RESET_FORM'
};

// Reducer del formulario
function formReducer(state, action) {
    switch (action.type) {
        case FORM_ACTIONS.UPDATE_FIELD:
            return {
                ...state,
                fields: {
                    ...state.fields,
                    [action.payload.field]: action.payload.value
                },
                // Limpiar error del campo cuando se actualiza
                errors: {
                    ...state.errors,
                    [action.payload.field]: ''
                }
            };
            
        case FORM_ACTIONS.SET_ERROR:
            return {
                ...state,
                errors: {
                    ...state.errors,
                    [action.payload.field]: action.payload.message
                }
            };
            
        case FORM_ACTIONS.CLEAR_ERROR:
            const newErrors = { ...state.errors };
            delete newErrors[action.payload.field];
            return {
                ...state,
                errors: newErrors
            };
            
        case FORM_ACTIONS.SET_TOUCHED:
            return {
                ...state,
                touched: {
                    ...state.touched,
                    [action.payload.field]: true
                }
            };
            
        case FORM_ACTIONS.SET_SUBMITTING:
            return {
                ...state,
                isSubmitting: action.payload
            };
            
        case FORM_ACTIONS.SET_SUBMITTED:
            return {
                ...state,
                isSubmitted: action.payload
            };
            
        case FORM_ACTIONS.RESET_FORM:
            return formInitialState;
            
        default:
            return state;
    }
}

// Componente de formulario con useReducer
function AdvancedForm() {
    const [state, dispatch] = useReducer(formReducer, formInitialState);
    
    const updateField = (field, value) => {
        dispatch({
            type: FORM_ACTIONS.UPDATE_FIELD,
            payload: { field, value }
        });
    };
    
    const setFieldTouched = (field) => {
        dispatch({
            type: FORM_ACTIONS.SET_TOUCHED,
            payload: { field }
        });
    };
    
    const validateField = (field, value) => {
        let error = '';
        
        switch (field) {
            case 'name':
                if (!value.trim()) error = 'El nombre es requerido';
                else if (value.length < 2) error = 'El nombre debe tener al menos 2 caracteres';
                break;
                
            case 'email':
                if (!value.trim()) error = 'El email es requerido';
                else if (!value.includes('@')) error = 'Email inválido';
                break;
                
            case 'age':
                if (!value) error = 'La edad es requerida';
                else if (value < 18) error = 'Debe ser mayor de 18 años';
                break;
                
            case 'city':
                if (!value.trim()) error = 'La ciudad es requerida';
                break;
        }
        
        if (error) {
            dispatch({
                type: FORM_ACTIONS.SET_ERROR,
                payload: { field, message: error }
            });
        } else {
            dispatch({
                type: FORM_ACTIONS.CLEAR_ERROR,
                payload: { field }
            });
        }
        
        return !error;
    };
    
    const handleSubmit = async (e) => {
        e.preventDefault();
        
        // Validar todos los campos
        const fields = Object.keys(state.fields);
        const validations = fields.map(field => 
            validateField(field, state.fields[field])
        );
        
        if (validations.every(Boolean)) {
            dispatch({ type: FORM_ACTIONS.SET_SUBMITTING, payload: true });
            
            try {
                // Simular envío de formulario
                await new Promise(resolve => setTimeout(resolve, 1000));
                
                console.log('Formulario enviado:', state.fields);
                dispatch({ type: FORM_ACTIONS.SET_SUBMITTED, payload: true });
                
                // Resetear formulario después de 2 segundos
                setTimeout(() => {
                    dispatch({ type: FORM_ACTIONS.RESET_FORM });
                }, 2000);
                
            } catch (error) {
                console.error('Error al enviar:', error);
            } finally {
                dispatch({ type: FORM_ACTIONS.SET_SUBMITTING, payload: false });
            }
        }
    };
    
    const handleBlur = (field) => {
        setFieldTouched(field);
        validateField(field, state.fields[field]);
    };
    
    if (state.isSubmitted) {
        return (
            <div className="success-message">
                <h2>¡Formulario enviado exitosamente!</h2>
                <p>Gracias por tu información.</p>
            </div>
        );
    }
    
    return (
        <form onSubmit={handleSubmit} className="advanced-form">
            <h2>Formulario Avanzado</h2>
            
            <div className="form-group">
                <label>Nombre:</label>
                <input
                    type="text"
                    value={state.fields.name}
                    onChange={(e) => updateField('name', e.target.value)}
                    onBlur={() => handleBlur('name')}
                    className={state.touched.name && state.errors.name ? 'error' : ''}
                />
                {state.touched.name && state.errors.name && (
                    <span className="error-message">{state.errors.name}</span>
                )}
            </div>
            
            <div className="form-group">
                <label>Email:</label>
                <input
                    type="email"
                    value={state.fields.email}
                    onChange={(e) => updateField('email', e.target.value)}
                    onBlur={() => handleBlur('email')}
                    className={state.touched.email && state.errors.email ? 'error' : ''}
                />
                {state.touched.email && state.errors.email && (
                    <span className="error-message">{state.errors.email}</span>
                )}
            </div>
            
            <div className="form-group">
                <label>Edad:</label>
                <input
                    type="number"
                    value={state.fields.age}
                    onChange={(e) => updateField('age', e.target.value)}
                    onBlur={() => handleBlur('age')}
                    className={state.touched.age && state.errors.age ? 'error' : ''}
                />
                {state.touched.age && state.errors.age && (
                    <span className="error-message">{state.errors.age}</span>
                )}
            </div>
            
            <div className="form-group">
                <label>Ciudad:</label>
                <input
                    type="text"
                    value={state.fields.city}
                    onChange={(e) => updateField('city', e.target.value)}
                    onBlur={() => handleBlur('city')}
                    className={state.touched.city && state.errors.city ? 'error' : ''}
                />
                {state.touched.city && state.errors.city && (
                    <span className="error-message">{state.errors.city}</span>
                )}
            </div>
            
            <button 
                type="submit" 
                disabled={state.isSubmitting}
                className="submit-button"
            >
                {state.isSubmitting ? 'Enviando...' : 'Enviar'}
            </button>
        </form>
    );
}
```

---

## 🌐 **CONTEXT API PARA ESTADO GLOBAL**

### **Implementación de Context**

#### **1. Context Básico**
```jsx
import React, { createContext, useContext, useState } from 'react';

// Crear el contexto
const UserContext = createContext();

// Provider del contexto
function UserProvider({ children }) {
    const [user, setUser] = useState(null);
    const [isAuthenticated, setIsAuthenticated] = useState(false);
    
    const login = async (credentials) => {
        try {
            // Simular login
            const response = await fetch('/api/login', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(credentials)
            });
            
            if (response.ok) {
                const userData = await response.json();
                setUser(userData);
                setIsAuthenticated(true);
                return { success: true };
            } else {
                return { success: false, error: 'Credenciales inválidas' };
            }
        } catch (error) {
            return { success: false, error: 'Error de conexión' };
        }
    };
    
    const logout = () => {
        setUser(null);
        setIsAuthenticated(false);
    };
    
    const updateProfile = (updates) => {
        setUser(prevUser => ({
            ...prevUser,
            ...updates
        }));
    };
    
    const value = {
        user,
        isAuthenticated,
        login,
        logout,
        updateProfile
    };
    
    return (
        <UserContext.Provider value={value}>
            {children}
        </UserContext.Provider>
    );
}

// Hook personalizado para usar el contexto
function useUser() {
    const context = useContext(UserContext);
    if (!context) {
        throw new Error('useUser debe usarse dentro de UserProvider');
    }
    return context;
}

// Componente de login
function LoginForm() {
    const { login } = useUser();
    const [credentials, setCredentials] = useState({
        email: '',
        password: ''
    });
    const [isLoading, setIsLoading] = useState(false);
    const [error, setError] = useState('');
    
    const handleSubmit = async (e) => {
        e.preventDefault();
        setIsLoading(true);
        setError('');
        
        const result = await login(credentials);
        
        if (!result.success) {
            setError(result.error);
        }
        
        setIsLoading(false);
    };
    
    return (
        <form onSubmit={handleSubmit} className="login-form">
            <h2>Iniciar Sesión</h2>
            
            {error && (
                <div className="error-message">{error}</div>
            )}
            
            <div className="form-group">
                <label>Email:</label>
                <input
                    type="email"
                    value={credentials.email}
                    onChange={(e) => setCredentials(prev => ({
                        ...prev,
                        email: e.target.value
                    }))}
                    required
                />
            </div>
            
            <div className="form-group">
                <label>Contraseña:</label>
                <input
                    type="password"
                    value={credentials.password}
                    onChange={(e) => setCredentials(prev => ({
                        ...prev,
                        password: e.target.value
                    }))}
                    required
                />
            </div>
            
            <button type="submit" disabled={isLoading}>
                {isLoading ? 'Iniciando...' : 'Iniciar Sesión'}
            </button>
        </form>
    );
}

// Componente de perfil
function UserProfile() {
    const { user, logout, updateProfile } = useUser();
    const [isEditing, setIsEditing] = useState(false);
    const [editData, setEditData] = useState({
        name: user?.name || '',
        email: user?.email || '',
        bio: user?.bio || ''
    });
    
    const handleSave = () => {
        updateProfile(editData);
        setIsEditing(false);
    };
    
    const handleCancel = () => {
        setEditData({
            name: user?.name || '',
            email: user?.email || '',
            bio: user?.bio || ''
        });
        setIsEditing(false);
    };
    
    if (!user) {
        return <div>No hay usuario autenticado</div>;
    }
    
    return (
        <div className="user-profile">
            <h2>Perfil de Usuario</h2>
            
            {isEditing ? (
                <div className="edit-mode">
                    <div className="form-group">
                        <label>Nombre:</label>
                        <input
                            type="text"
                            value={editData.name}
                            onChange={(e) => setEditData(prev => ({
                                ...prev,
                                name: e.target.value
                            }))}
                        />
                    </div>
                    
                    <div className="form-group">
                        <label>Email:</label>
                        <input
                            type="email"
                            value={editData.email}
                            onChange={(e) => setEditData(prev => ({
                                ...prev,
                                email: e.target.value
                            }))}
                        />
                    </div>
                    
                    <div className="form-group">
                        <label>Biografía:</label>
                        <textarea
                            value={editData.bio}
                            onChange={(e) => setEditData(prev => ({
                                ...prev,
                                bio: e.target.value
                            }))}
                        />
                    </div>
                    
                    <div className="edit-actions">
                        <button onClick={handleSave}>Guardar</button>
                        <button onClick={handleCancel}>Cancelar</button>
                    </div>
                </div>
            ) : (
                <div className="view-mode">
                    <p><strong>Nombre:</strong> {user.name}</p>
                    <p><strong>Email:</strong> {user.email}</p>
                    <p><strong>Biografía:</strong> {user.bio || 'Sin biografía'}</p>
                    
                    <div className="profile-actions">
                        <button onClick={() => setIsEditing(true)}>
                            Editar Perfil
                        </button>
                        <button onClick={logout} className="logout-btn">
                            Cerrar Sesión
                        </button>
                    </div>
                </div>
            )}
        </div>
    );
}

// Componente principal de la aplicación
function App() {
    return (
        <UserProvider>
            <div className="app">
                <Header />
                <MainContent />
            </div>
        </UserProvider>
    );
}

function Header() {
    const { isAuthenticated, user } = useUser();
    
    return (
        <header className="app-header">
            <h1>Mi Aplicación</h1>
            {isAuthenticated && (
                <span>Bienvenido, {user.name}</span>
            )}
        </header>
    );
}

function MainContent() {
    const { isAuthenticated } = useUser();
    
    return (
        <main className="app-main">
            {isAuthenticated ? <UserProfile /> : <LoginForm />}
        </main>
    );
}
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Cuándo Usar Cada Hook**
- **useState**: Estado simple y local
- **useReducer**: Estado complejo con lógica de transiciones
- **Context**: Estado que necesita ser compartido entre componentes

### **2. Performance**
- **useCallback** para funciones que se pasan como props
- **useMemo** para cálculos costosos
- **React.memo** para componentes que no cambian
- **Evitar recrear objetos** en cada render

### **3. Organización del Estado**
- **Estado local** cuando sea posible
- **Lifting state up** cuando sea necesario
- **Context** para estado global
- **Estado inmutable** siempre

---

## 📚 **RECURSOS ADICIONALES**

### **Patrones Avanzados**
- **State Machines**: Para flujos complejos
- **Custom Hooks**: Para lógica reutilizable
- **State Persistence**: Para persistir estado

---

*La gestión de estado es fundamental en React. Dominar useState, useReducer y Context API te permitirá crear aplicaciones robustas y mantenibles.* 