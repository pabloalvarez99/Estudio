# 🪝 React Hooks: Hooks Personalizados y Patrones Avanzados

## 🎯 **OBJETIVO**
Dominar los hooks personalizados de React, comprendiendo useEffect, useCallback, useMemo y creando hooks reutilizables para lógica compleja.

---

## 🪝 **HOOKS PERSONALIZADOS**

### **Conceptos de Custom Hooks**

#### **1. Hook Básico**
```jsx
import { useState, useEffect } from 'react';

// Hook personalizado para manejo de formularios
function useForm(initialValues = {}, validationRules = {}) {
    const [values, setValues] = useState(initialValues);
    const [errors, setErrors] = useState({});
    const [touched, setTouched] = useState({});
    const [isSubmitting, setIsSubmitting] = useState(false);
    
    // Actualizar campo individual
    const setValue = (field, value) => {
        setValues(prev => ({ ...prev, [field]: value }));
        
        // Limpiar error del campo cuando se actualiza
        if (errors[field]) {
            setErrors(prev => ({ ...prev, [field]: '' }));
        }
    };
    
    // Actualizar múltiples campos
    const setValuesBatch = (updates) => {
        setValues(prev => ({ ...prev, ...updates }));
    };
    
    // Marcar campo como tocado
    const setFieldTouched = (field) => {
        setTouched(prev => ({ ...prev, [field]: true }));
    };
    
    // Validar campo individual
    const validateField = (field, value) => {
        const rule = validationRules[field];
        if (!rule) return '';
        
        const error = rule(value, values);
        setErrors(prev => ({ ...prev, [field]: error }));
        return error;
    };
    
    // Validar todo el formulario
    const validateForm = () => {
        const newErrors = {};
        
        Object.keys(validationRules).forEach(field => {
            const error = validationRules[field](values[field], values);
            if (error) {
                newErrors[field] = error;
            }
        });
        
        setErrors(newErrors);
        return Object.keys(newErrors).length === 0;
    };
    
    // Resetear formulario
    const resetForm = () => {
        setValues(initialValues);
        setErrors({});
        setTouched({});
        setIsSubmitting(false);
    };
    
    // Manejar envío del formulario
    const handleSubmit = async (onSubmit) => {
        if (validateForm()) {
            setIsSubmitting(true);
            try {
                await onSubmit(values);
                resetForm();
            } catch (error) {
                console.error('Error en submit:', error);
            } finally {
                setIsSubmitting(false);
            }
        }
    };
    
    return {
        values,
        errors,
        touched,
        isSubmitting,
        setValue,
        setValuesBatch,
        setFieldTouched,
        validateField,
        validateForm,
        resetForm,
        handleSubmit
    };
}

// Uso del hook personalizado
function UserForm() {
    const validationRules = {
        name: (value) => !value.trim() ? 'El nombre es requerido' : '',
        email: (value) => {
            if (!value.trim()) return 'El email es requerido';
            if (!value.includes('@')) return 'Email inválido';
            return '';
        },
        age: (value) => {
            if (!value) return 'La edad es requerida';
            if (value < 18) return 'Debe ser mayor de 18 años';
            return '';
        }
    };
    
    const {
        values,
        errors,
        touched,
        isSubmitting,
        setValue,
        setFieldTouched,
        handleSubmit
    } = useForm({
        name: '',
        email: '',
        age: ''
    }, validationRules);
    
    const onSubmit = async (formData) => {
        console.log('Formulario enviado:', formData);
        // Simular envío
        await new Promise(resolve => setTimeout(resolve, 1000));
    };
    
    const handleFieldChange = (field, value) => {
        setValue(field, value);
    };
    
    const handleFieldBlur = (field) => {
        setFieldTouched(field);
    };
    
    return (
        <form onSubmit={(e) => {
            e.preventDefault();
            handleSubmit(onSubmit);
        }}>
            <div className="form-group">
                <label>Nombre:</label>
                <input
                    type="text"
                    value={values.name}
                    onChange={(e) => handleFieldChange('name', e.target.value)}
                    onBlur={() => handleFieldBlur('name')}
                    className={touched.name && errors.name ? 'error' : ''}
                />
                {touched.name && errors.name && (
                    <span className="error-message">{errors.name}</span>
                )}
            </div>
            
            <div className="form-group">
                <label>Email:</label>
                <input
                    type="email"
                    value={values.email}
                    onChange={(e) => handleFieldChange('email', e.target.value)}
                    onBlur={() => handleFieldBlur('email')}
                    className={touched.email && errors.email ? 'error' : ''}
                />
                {touched.email && errors.email && (
                    <span className="error-message">{errors.email}</span>
                )}
            </div>
            
            <div className="form-group">
                <label>Edad:</label>
                <input
                    type="number"
                    value={values.age}
                    onChange={(e) => handleFieldChange('age', e.target.value)}
                    onBlur={() => handleFieldBlur('age')}
                    className={touched.age && errors.age ? 'error' : ''}
                />
                {touched.age && errors.age && (
                    <span className="error-message">{errors.age}</span>
                )}
            </div>
            
            <button type="submit" disabled={isSubmitting}>
                {isSubmitting ? 'Enviando...' : 'Enviar'}
            </button>
        </form>
    );
}
```

#### **2. Hook para API Calls**
```jsx
import { useState, useEffect, useCallback } from 'react';

// Hook personalizado para llamadas a API
function useApi(endpoint, options = {}) {
    const {
        method = 'GET',
        body = null,
        headers = {},
        immediate = true,
        onSuccess = null,
        onError = null
    } = options;
    
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);
    
    // Función para ejecutar la llamada a la API
    const execute = useCallback(async (customBody = null, customHeaders = {}) => {
        setLoading(true);
        setError(null);
        
        try {
            const requestBody = customBody || body;
            const requestHeaders = {
                'Content-Type': 'application/json',
                ...headers,
                ...customHeaders
            };
            
            const config = {
                method,
                headers: requestHeaders
            };
            
            if (requestBody && method !== 'GET') {
                config.body = JSON.stringify(requestBody);
            }
            
            const response = await fetch(endpoint, config);
            
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            
            const result = await response.json();
            setData(result);
            
            if (onSuccess) {
                onSuccess(result);
            }
            
            return result;
        } catch (err) {
            const errorMessage = err.message || 'Error en la llamada a la API';
            setError(errorMessage);
            
            if (onError) {
                onError(errorMessage);
            }
            
            throw err;
        } finally {
            setLoading(false);
        }
    }, [endpoint, method, body, headers, onSuccess, onError]);
    
    // Función para resetear el estado
    const reset = useCallback(() => {
        setData(null);
        setLoading(false);
        setError(null);
    }, []);
    
    // Ejecutar inmediatamente si se especifica
    useEffect(() => {
        if (immediate && method === 'GET') {
            execute();
        }
    }, [immediate, method, execute]);
    
    return {
        data,
        loading,
        error,
        execute,
        reset
    };
}

// Uso del hook para API
function UserList() {
    const {
        data: users,
        loading,
        error,
        execute: fetchUsers,
        reset
    } = useApi('/api/users', {
        onSuccess: (data) => console.log('Usuarios cargados:', data),
        onError: (error) => console.error('Error al cargar usuarios:', error)
    });
    
    const {
        execute: createUser,
        loading: creating
    } = useApi('/api/users', {
        method: 'POST',
        immediate: false
    });
    
    const handleCreateUser = async (userData) => {
        try {
            await createUser(userData);
            // Recargar la lista después de crear
            fetchUsers();
        } catch (error) {
            console.error('Error al crear usuario:', error);
        }
    };
    
    if (loading) return <div>Cargando usuarios...</div>;
    if (error) return <div>Error: {error}</div>;
    
    return (
        <div className="user-list">
            <h2>Lista de Usuarios</h2>
            
            <button onClick={fetchUsers}>Recargar</button>
            <button onClick={reset}>Limpiar</button>
            
            {users && (
                <ul>
                    {users.map(user => (
                        <li key={user.id}>
                            {user.name} - {user.email}
                        </li>
                    ))}
                </ul>
            )}
            
            <CreateUserForm onSubmit={handleCreateUser} isSubmitting={creating} />
        </div>
    );
}

function CreateUserForm({ onSubmit, isSubmitting }) {
    const [formData, setFormData] = useState({
        name: '',
        email: ''
    });
    
    const handleSubmit = (e) => {
        e.preventDefault();
        onSubmit(formData);
        setFormData({ name: '', email: '' });
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                placeholder="Nombre"
                value={formData.name}
                onChange={(e) => setFormData(prev => ({
                    ...prev,
                    name: e.target.value
                }))}
                required
            />
            
            <input
                type="email"
                placeholder="Email"
                value={formData.email}
                onChange={(e) => setFormData(prev => ({
                    ...prev,
                    email: e.target.value
                }))}
                required
            />
            
            <button type="submit" disabled={isSubmitting}>
                {isSubmitting ? 'Creando...' : 'Crear Usuario'}
            </button>
        </form>
    );
}
```

---

## ⏰ **USEEFFECT AVANZADO**

### **Patrones de useEffect**

#### **1. useEffect con Cleanup**
```jsx
import { useState, useEffect } from 'react';

// Hook para suscripciones a eventos
function useEventListener(eventName, handler, element = window) {
    useEffect(() => {
        // Verificar que el elemento soporte addEventListener
        if (!(element && element.addEventListener)) {
            return;
        }
        
        // Agregar event listener
        element.addEventListener(eventName, handler);
        
        // Cleanup function
        return () => {
            element.removeEventListener(eventName, handler);
        };
    }, [eventName, handler, element]);
}

// Hook para intervalos
function useInterval(callback, delay) {
    useEffect(() => {
        if (delay !== null) {
            const id = setInterval(callback, delay);
            return () => clearInterval(id);
        }
    }, [callback, delay]);
}

// Hook para timeouts
function useTimeout(callback, delay) {
    useEffect(() => {
        if (delay !== null) {
            const id = setTimeout(callback, delay);
            return () => clearTimeout(id);
        }
    }, [callback, delay]);
}

// Hook para debounce
function useDebounce(value, delay) {
    const [debouncedValue, setDebouncedValue] = useState(value);
    
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay]);
    
    return debouncedValue;
}

// Hook para throttle
function useThrottle(value, delay) {
    const [throttledValue, setThrottledValue] = useState(value);
    const [lastRun, setLastRun] = useState(Date.now());
    
    useEffect(() => {
        const handler = setTimeout(() => {
            if (Date.now() - lastRun >= delay) {
                setThrottledValue(value);
                setLastRun(Date.now());
            }
        }, delay - (Date.now() - lastRun));
        
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay, lastRun]);
    
    return throttledValue;
}

// Uso de los hooks
function SearchComponent() {
    const [searchTerm, setSearchTerm] = useState('');
    const debouncedSearchTerm = useDebounce(searchTerm, 500);
    const [searchResults, setSearchResults] = useState([]);
    const [loading, setLoading] = useState(false);
    
    // Efecto que se ejecuta cuando cambia el término de búsqueda debounced
    useEffect(() => {
        if (debouncedSearchTerm) {
            setLoading(true);
            
            // Simular búsqueda
            const searchUsers = async () => {
                try {
                    const response = await fetch(`/api/users/search?q=${debouncedSearchTerm}`);
                    const results = await response.json();
                    setSearchResults(results);
                } catch (error) {
                    console.error('Error en búsqueda:', error);
                    setSearchResults([]);
                } finally {
                    setLoading(false);
                }
            };
            
            searchUsers();
        } else {
            setSearchResults([]);
        }
    }, [debouncedSearchTerm]);
    
    return (
        <div className="search-component">
            <input
                type="text"
                placeholder="Buscar usuarios..."
                value={searchTerm}
                onChange={(e) => setSearchTerm(e.target.value)}
            />
            
            {loading && <div>Buscando...</div>}
            
            {searchResults.length > 0 && (
                <ul className="search-results">
                    {searchResults.map(user => (
                        <li key={user.id}>
                            {user.name} - {user.email}
                        </li>
                    ))}
                </ul>
            )}
        </div>
    );
}
```

#### **2. useEffect con Dependencias Complejas**
```jsx
// Hook para comparar objetos profundamente
function useDeepCompareEffect(effect, dependencies) {
    const ref = useRef();
    
    if (!ref.current || !isEqual(dependencies, ref.current)) {
        ref.current = dependencies;
    }
    
    useEffect(effect, ref.current);
}

// Función helper para comparación profunda
function isEqual(a, b) {
    if (a === b) return true;
    if (a == null || b == null) return false;
    if (a.length !== b.length) return false;
    
    for (let i = 0; i < a.length; i++) {
        if (a[i] !== b[i]) return false;
    }
    
    return true;
}

// Hook para manejar scroll con throttling
function useScrollPosition() {
    const [scrollPosition, setScrollPosition] = useState(0);
    
    useEffect(() => {
        let ticking = false;
        
        const updatePosition = () => {
            setScrollPosition(window.pageYOffset);
            ticking = false;
        };
        
        const onScroll = () => {
            if (!ticking) {
                requestAnimationFrame(updatePosition);
                ticking = true;
            }
        };
        
        window.addEventListener('scroll', onScroll);
        
        return () => window.removeEventListener('scroll', onScroll);
    }, []);
    
    return scrollPosition;
}

// Hook para manejar dimensiones de ventana
function useWindowSize() {
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        let timeoutId = null;
        
        const handleResize = () => {
            clearTimeout(timeoutId);
            timeoutId = setTimeout(() => {
                setWindowSize({
                    width: window.innerWidth,
                    height: window.innerHeight
                });
            }, 100);
        };
        
        window.addEventListener('resize', handleResize);
        
        return () => {
            window.removeEventListener('resize', handleResize);
            if (timeoutId) clearTimeout(timeoutId);
        };
    }, []);
    
    return windowSize;
}
```

---

## 🚀 **USECALLBACK Y USEMEMO**

### **Optimización de Performance**

#### **1. useCallback para Funciones**
```jsx
import { useState, useCallback, memo } from 'react';

// Componente memoizado que recibe funciones como props
const ExpensiveComponent = memo(({ onUpdate, data }) => {
    console.log('ExpensiveComponent renderizado');
    
    return (
        <div className="expensive-component">
            <h3>Componente Costoso</h3>
            <p>Datos: {JSON.stringify(data)}</p>
            <button onClick={onUpdate}>Actualizar</button>
        </div>
    );
});

// Componente padre que optimiza las funciones
function ParentComponent() {
    const [count, setCount] = useState(0);
    const [data, setData] = useState({ value: 'inicial' });
    
    // Función optimizada con useCallback
    const handleUpdate = useCallback(() => {
        setData(prev => ({
            ...prev,
            value: `actualizado-${Date.now()}`
        }));
    }, []); // Dependencias vacías = función nunca cambia
    
    // Función que depende de count
    const handleCountUpdate = useCallback(() => {
        setCount(prev => prev + 1);
    }, []); // No depende de count, pero se ejecuta cuando count cambia
    
    // Función que depende de data
    const handleDataUpdate = useCallback((newValue) => {
        setData(prev => ({
            ...prev,
            value: newValue
        }));
    }, []); // No depende de data, pero se ejecuta cuando data cambia
    
    return (
        <div className="parent-component">
            <h2>Componente Padre</h2>
            <p>Contador: {count}</p>
            <p>Datos: {data.value}</p>
            
            <button onClick={handleCountUpdate}>Incrementar Contador</button>
            <button onClick={() => handleDataUpdate('nuevo-valor')}>
                Actualizar Datos
            </button>
            
            <ExpensiveComponent
                onUpdate={handleUpdate}
                data={data}
            />
        </div>
    );
}
```

#### **2. useMemo para Cálculos Costosos**
```jsx
import { useState, useMemo } from 'react';

// Función de cálculo costoso
function expensiveCalculation(items) {
    console.log('Ejecutando cálculo costoso...');
    
    return items.reduce((acc, item) => {
        // Simular cálculo complejo
        const processed = item.value * Math.sqrt(item.weight) + Math.log(item.price);
        return acc + processed;
    }, 0);
}

// Componente que usa useMemo
function DataProcessor({ items, filterValue }) {
    // Memoizar el cálculo costoso
    const processedData = useMemo(() => {
        return expensiveCalculation(items);
    }, [items]); // Solo se recalcula cuando items cambia
    
    // Memoizar datos filtrados
    const filteredItems = useMemo(() => {
        return items.filter(item => 
            item.name.toLowerCase().includes(filterValue.toLowerCase())
        );
    }, [items, filterValue]);
    
    // Memoizar estadísticas
    const statistics = useMemo(() => {
        if (filteredItems.length === 0) return null;
        
        const total = filteredItems.reduce((sum, item) => sum + item.value, 0);
        const average = total / filteredItems.length;
        const max = Math.max(...filteredItems.map(item => item.value));
        const min = Math.min(...filteredItems.map(item => item.value));
        
        return { total, average, max, min };
    }, [filteredItems]);
    
    return (
        <div className="data-processor">
            <h3>Procesador de Datos</h3>
            
            <div className="results">
                <h4>Resultados del Cálculo:</h4>
                <p>Valor procesado: {processedData.toFixed(2)}</p>
                
                <h4>Estadísticas de Items Filtrados:</h4>
                {statistics ? (
                    <ul>
                        <li>Total: {statistics.total.toFixed(2)}</li>
                        <li>Promedio: {statistics.average.toFixed(2)}</li>
                        <li>Máximo: {statistics.max.toFixed(2)}</li>
                        <li>Mínimo: {statistics.min.toFixed(2)}</li>
                    </ul>
                ) : (
                    <p>No hay items que coincidan con el filtro</p>
                )}
            </div>
            
            <div className="filtered-items">
                <h4>Items Filtrados ({filteredItems.length}):</h4>
                <ul>
                    {filteredItems.map(item => (
                        <li key={item.id}>
                            {item.name}: {item.value} (${item.price})
                        </li>
                    ))}
                </ul>
            </div>
        </div>
    );
}

// Componente de ejemplo
function App() {
    const [items, setItems] = useState([
        { id: 1, name: 'Item A', value: 100, weight: 10, price: 50 },
        { id: 2, name: 'Item B', value: 200, weight: 15, price: 75 },
        { id: 3, name: 'Item C', value: 150, weight: 12, price: 60 }
    ]);
    
    const [filterValue, setFilterValue] = useState('');
    
    const addItem = () => {
        const newItem = {
            id: Date.now(),
            name: `Item ${items.length + 1}`,
            value: Math.floor(Math.random() * 300) + 50,
            weight: Math.floor(Math.random() * 20) + 5,
            price: Math.floor(Math.random() * 100) + 25
        };
        setItems(prev => [...prev, newItem]);
    };
    
    return (
        <div className="app">
            <h1>Optimización con useMemo</h1>
            
            <div className="controls">
                <input
                    type="text"
                    placeholder="Filtrar items..."
                    value={filterValue}
                    onChange={(e) => setFilterValue(e.target.value)}
                />
                
                <button onClick={addItem}>Agregar Item</button>
            </div>
            
            <DataProcessor items={items} filterValue={filterValue} />
        </div>
    );
}
```

---

## 🎨 **HOOKS PERSONALIZADOS AVANZADOS**

### **Patrones de Hooks Compuestos**

#### **1. Hook para Gestión de Listas**
```jsx
// Hook para gestión de listas con operaciones CRUD
function useList(initialItems = []) {
    const [items, setItems] = useState(initialItems);
    const [selectedItems, setSelectedItems] = useState(new Set());
    
    // Agregar item
    const addItem = useCallback((item) => {
        setItems(prev => [...prev, { ...item, id: Date.now() }]);
    }, []);
    
    // Actualizar item
    const updateItem = useCallback((id, updates) => {
        setItems(prev => prev.map(item =>
            item.id === id ? { ...item, ...updates } : item
        ));
    }, []);
    
    // Eliminar item
    const removeItem = useCallback((id) => {
        setItems(prev => prev.filter(item => item.id !== id));
        setSelectedItems(prev => {
            const newSelected = new Set(prev);
            newSelected.delete(id);
            return newSelected;
        });
    }, []);
    
    // Eliminar múltiples items
    const removeSelected = useCallback(() => {
        setItems(prev => prev.filter(item => !selectedItems.has(item.id)));
        setSelectedItems(new Set());
    }, [selectedItems]);
    
    // Seleccionar/deseleccionar item
    const toggleSelection = useCallback((id) => {
        setSelectedItems(prev => {
            const newSelected = new Set(prev);
            if (newSelected.has(id)) {
                newSelected.delete(id);
            } else {
                newSelected.add(id);
            }
            return newSelected;
        });
    }, []);
    
    // Seleccionar todos
    const selectAll = useCallback(() => {
        setSelectedItems(new Set(items.map(item => item.id)));
    }, [items]);
    
    // Deseleccionar todos
    const deselectAll = useCallback(() => {
        setSelectedItems(new Set());
    }, []);
    
    // Filtrar items
    const filterItems = useCallback((predicate) => {
        return items.filter(predicate);
    }, [items]);
    
    // Ordenar items
    const sortItems = useCallback((comparator) => {
        return [...items].sort(comparator);
    }, [items]);
    
    // Estadísticas
    const stats = useMemo(() => ({
        total: items.length,
        selected: selectedItems.size,
        unselected: items.length - selectedItems.size
    }), [items.length, selectedItems.size]);
    
    return {
        items,
        selectedItems,
        addItem,
        updateItem,
        removeItem,
        removeSelected,
        toggleSelection,
        selectAll,
        deselectAll,
        filterItems,
        sortItems,
        stats
    };
}

// Uso del hook
function TaskManager() {
    const {
        items: tasks,
        selectedItems,
        addItem,
        updateItem,
        removeItem,
        removeSelected,
        toggleSelection,
        selectAll,
        deselectAll,
        stats
    } = useList([
        { id: 1, title: 'Tarea 1', completed: false, priority: 'high' },
        { id: 2, title: 'Tarea 2', completed: true, priority: 'medium' },
        { id: 3, title: 'Tarea 3', completed: false, priority: 'low' }
    ]);
    
    const [newTaskTitle, setNewTaskTitle] = useState('');
    
    const handleAddTask = () => {
        if (newTaskTitle.trim()) {
            addItem({
                title: newTaskTitle.trim(),
                completed: false,
                priority: 'medium'
            });
            setNewTaskTitle('');
        }
    };
    
    const handleToggleTask = (id) => {
        const task = tasks.find(t => t.id === id);
        updateItem(id, { completed: !task.completed });
    };
    
    return (
        <div className="task-manager">
            <h2>Gestor de Tareas</h2>
            
            <div className="task-controls">
                <input
                    type="text"
                    placeholder="Nueva tarea..."
                    value={newTaskTitle}
                    onChange={(e) => setNewTaskTitle(e.target.value)}
                    onKeyPress={(e) => e.key === 'Enter' && handleAddTask()}
                />
                <button onClick={handleAddTask}>Agregar</button>
            </div>
            
            <div className="selection-controls">
                <button onClick={selectAll}>Seleccionar Todos</button>
                <button onClick={deselectAll}>Deseleccionar Todos</button>
                {selectedItems.size > 0 && (
                    <button onClick={removeSelected}>
                        Eliminar Seleccionados ({selectedItems.size})
                    </button>
                )}
            </div>
            
            <div className="stats">
                <p>Total: {stats.total} | Seleccionadas: {stats.selected}</p>
            </div>
            
            <ul className="task-list">
                {tasks.map(task => (
                    <li key={task.id} className="task-item">
                        <input
                            type="checkbox"
                            checked={selectedItems.has(task.id)}
                            onChange={() => toggleSelection(task.id)}
                        />
                        
                        <input
                            type="checkbox"
                            checked={task.completed}
                            onChange={() => handleToggleTask(task.id)}
                        />
                        
                        <span className={task.completed ? 'completed' : ''}>
                            {task.title}
                        </span>
                        
                        <span className={`priority priority-${task.priority}`}>
                            {task.priority}
                        </span>
                        
                        <button onClick={() => removeItem(task.id)}>
                            Eliminar
                        </button>
                    </li>
                ))}
            </ul>
        </div>
    );
}
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Reglas de los Hooks**
- **Siempre en el nivel superior** de componentes o hooks
- **No dentro de bucles, condiciones o funciones anidadas**
- **Solo en componentes funcionales** o hooks personalizados

### **2. Performance**
- **useCallback** para funciones que se pasan como props
- **useMemo** para cálculos costosos
- **Dependencias correctas** en useEffect
- **Evitar recrear objetos** en cada render

### **3. Organización**
- **Un hook por archivo** cuando sea complejo
- **Nombres descriptivos** para hooks
- **Documentación clara** de la API del hook
- **Tests** para hooks complejos

---

## 📚 **RECURSOS ADICIONALES**

### **Hooks Avanzados**
- **useReducer** para estado complejo
- **useContext** para estado global
- **useImperativeHandle** para refs personalizados
- **useLayoutEffect** para mediciones del DOM

---

*Los hooks personalizados son una de las características más poderosas de React. Te permiten extraer y reutilizar lógica compleja de manera elegante y eficiente.* 