# ⚛️ React.js Fundamentos: Biblioteca para Interfaces de Usuario

## 🎯 **OBJETIVO**
Dominar los fundamentos de React.js, comprendiendo JSX, componentes, props, estado, ciclo de vida y las mejores prácticas para crear aplicaciones web modernas y eficientes.

---

## 📖 **¿QUÉ ES REACT.JS?**

### **Definición**
React.js es una biblioteca JavaScript de código abierto desarrollada por Facebook para construir interfaces de usuario interactivas y reutilizables. Permite crear aplicaciones web complejas de manera eficiente mediante componentes modulares.

### **Características Principales**
- **Componentes Reutilizables**: UI modular y mantenible
- **Virtual DOM**: Renderizado eficiente y rápido
- **JSX**: Sintaxis que combina JavaScript y HTML
- **Unidireccional**: Flujo de datos predecible
- **Ecosistema Rico**: Herramientas y librerías complementarias

---

## 🎨 **JSX: JAVASCRIPT XML**

### **Conceptos Básicos de JSX**

#### **1. Sintaxis JSX**
```jsx
// JSX básico
const element = <h1>Hola, React!</h1>;

// JSX con atributos
const element = <img src="logo.png" alt="Logo" />;

// JSX con expresiones JavaScript
const name = 'Juan';
const element = <h1>Hola, {name}!</h1>;

// JSX con múltiples líneas
const element = (
    <div className="container">
        <h1>Título Principal</h1>
        <p>Este es un párrafo de ejemplo</p>
    </div>
);
```

#### **2. JSX vs HTML**
```jsx
// ❌ HTML tradicional
<div class="container">
    <label for="username">Usuario:</label>
    <input type="text" id="username">
</div>

// ✅ JSX en React
<div className="container">
    <label htmlFor="username">Usuario:</label>
    <input type="text" id="username" />
</div>

// Diferencias clave:
// - class → className
// - for → htmlFor
// - Tags deben cerrarse: <input /> o <input></input>
```

#### **3. Expresiones en JSX**
```jsx
// Variables y propiedades
const user = { name: 'Ana', age: 25 };
const isLoggedIn = true;

const element = (
    <div>
        <h1>Bienvenido, {user.name}!</h1>
        <p>Edad: {user.age}</p>
        {isLoggedIn && <button>Cerrar Sesión</button>}
        {!isLoggedIn && <button>Iniciar Sesión</button>}
    </div>
);

// Expresiones condicionales
const status = 'loading';
const element = (
    <div>
        {status === 'loading' && <p>Cargando...</p>}
        {status === 'success' && <p>¡Éxito!</p>}
        {status === 'error' && <p>Error ocurrido</p>}
    </div>
);

// Arrays y listas
const fruits = ['manzana', 'plátano', 'naranja'];
const element = (
    <ul>
        {fruits.map((fruit, index) => (
            <li key={index}>{fruit}</li>
        ))}
    </ul>
);
```

---

## 🧩 **COMPONENTES REACT**

### **Componentes Funcionales**

#### **1. Componente Básico**
```jsx
// Componente funcional simple
function Welcome() {
    return <h1>¡Bienvenido a React!</h1>;
}

// Componente con arrow function
const Welcome = () => {
    return <h1>¡Bienvenido a React!</h1>;
};

// Componente con return implícito
const Welcome = () => <h1>¡Bienvenido a React!</h1>;

// Uso del componente
function App() {
    return (
        <div>
            <Welcome />
            <p>Esta es mi primera aplicación React</p>
        </div>
    );
}
```

#### **2. Componente con Props**
```jsx
// Componente que recibe props
function Greeting(props) {
    return <h1>Hola, {props.name}!</h1>;
}

// Uso con props
function App() {
    return (
        <div>
            <Greeting name="Juan" />
            <Greeting name="María" />
            <Greeting name="Carlos" />
        </div>
    );
}

// Destructuring de props
function Greeting({ name, age, city }) {
    return (
        <div>
            <h1>Hola, {name}!</h1>
            <p>Edad: {age}</p>
            <p>Ciudad: {city}</p>
        </div>
    );
}

// Props con valores por defecto
function Greeting({ name = 'Usuario', age = 18, city = 'Desconocida' }) {
    return (
        <div>
            <h1>Hola, {name}!</h1>
            <p>Edad: {age}</p>
            <p>Ciudad: {city}</p>
        </div>
    );
}
```

#### **3. Componente con Children**
```jsx
// Componente que renderiza children
function Card({ title, children }) {
    return (
        <div className="card">
            <h2 className="card-title">{title}</h2>
            <div className="card-content">
                {children}
            </div>
        </div>
    );
}

// Uso con children
function App() {
    return (
        <div>
            <Card title="Mi Primera Tarjeta">
                <p>Este es el contenido de la tarjeta.</p>
                <button>Click aquí</button>
            </Card>
            
            <Card title="Otra Tarjeta">
                <ul>
                    <li>Elemento 1</li>
                    <li>Elemento 2</li>
                    <li>Elemento 3</li>
                </ul>
            </Card>
        </div>
    );
}
```

### **Componentes de Clase**

#### **1. Componente de Clase Básico**
```jsx
import React, { Component } from 'react';

class Welcome extends Component {
    render() {
        return <h1>¡Bienvenido a React!</h1>;
    }
}

// Componente de clase con props
class Greeting extends Component {
    render() {
        const { name, age } = this.props;
        return (
            <div>
                <h1>Hola, {name}!</h1>
                <p>Edad: {age}</p>
            </div>
        );
    }
}

// Uso
function App() {
    return (
        <div>
            <Welcome />
            <Greeting name="Ana" age={25} />
        </div>
    );
}
```

#### **2. Componente de Clase con Estado**
```jsx
class Counter extends Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }
    
    increment = () => {
        this.setState(prevState => ({
            count: prevState.count + 1
        }));
    };
    
    decrement = () => {
        this.setState(prevState => ({
            count: prevState.count - 1
        }));
    };
    
    reset = () => {
        this.setState({ count: 0 });
    };
    
    render() {
        return (
            <div>
                <h2>Contador: {this.state.count}</h2>
                <button onClick={this.increment}>+</button>
                <button onClick={this.decrement}>-</button>
                <button onClick={this.reset}>Reset</button>
            </div>
        );
    }
}
```

---

## 🔄 **ESTADO Y PROPS**

### **Estado Local**

#### **1. useState Hook**
```jsx
import React, { useState } from 'react';

function Counter() {
    // Estado básico
    const [count, setCount] = useState(0);
    
    // Estado con objeto
    const [user, setUser] = useState({
        name: 'Juan',
        age: 25,
        email: 'juan@example.com'
    });
    
    // Estado con array
    const [items, setItems] = useState([]);
    
    const increment = () => {
        setCount(prevCount => prevCount + 1);
    };
    
    const updateUser = () => {
        setUser(prevUser => ({
            ...prevUser,
            age: prevUser.age + 1
        }));
    };
    
    const addItem = () => {
        setItems(prevItems => [...prevItems, `Item ${prevItems.length + 1}`]);
    };
    
    return (
        <div>
            <h2>Contador: {count}</h2>
            <button onClick={increment}>Incrementar</button>
            
            <h3>Usuario: {user.name}</h3>
            <p>Edad: {user.age}</p>
            <button onClick={updateUser}>Cumplir Años</button>
            
            <h3>Items: {items.length}</h3>
            <button onClick={addItem}>Agregar Item</button>
            <ul>
                {items.map((item, index) => (
                    <li key={index}>{item}</li>
                ))}
            </ul>
        </div>
    );
}
```

#### **2. Múltiples Estados**
```jsx
function UserForm() {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [age, setAge] = useState('');
    const [errors, setErrors] = useState({});
    
    const handleSubmit = (e) => {
        e.preventDefault();
        
        // Validación
        const newErrors = {};
        if (!name.trim()) newErrors.name = 'El nombre es requerido';
        if (!email.trim()) newErrors.email = 'El email es requerido';
        if (!age || age < 18) newErrors.age = 'Debe ser mayor de 18 años';
        
        if (Object.keys(newErrors).length === 0) {
            // Enviar formulario
            console.log('Formulario enviado:', { name, email, age });
            // Limpiar formulario
            setName('');
            setEmail('');
            setAge('');
        } else {
            setErrors(newErrors);
        }
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <div>
                <label>Nombre:</label>
                <input
                    type="text"
                    value={name}
                    onChange={(e) => setName(e.target.value)}
                />
                {errors.name && <span className="error">{errors.name}</span>}
            </div>
            
            <div>
                <label>Email:</label>
                <input
                    type="email"
                    value={email}
                    onChange={(e) => setEmail(e.target.value)}
                />
                {errors.email && <span className="error">{errors.email}</span>}
            </div>
            
            <div>
                <label>Edad:</label>
                <input
                    type="number"
                    value={age}
                    onChange={(e) => setAge(e.target.value)}
                />
                {errors.age && <span className="error">{errors.age}</span>}
            </div>
            
            <button type="submit">Enviar</button>
        </form>
    );
}
```

### **Props y Comunicación entre Componentes**

#### **1. Props de Padre a Hijo**
```jsx
// Componente padre
function App() {
    const [users, setUsers] = useState([
        { id: 1, name: 'Juan', email: 'juan@example.com' },
        { id: 2, name: 'María', email: 'maria@example.com' },
        { id: 3, name: 'Carlos', email: 'carlos@example.com' }
    ]);
    
    const deleteUser = (userId) => {
        setUsers(prevUsers => prevUsers.filter(user => user.id !== userId));
    };
    
    return (
        <div>
            <h1>Lista de Usuarios</h1>
            {users.map(user => (
                <UserCard
                    key={user.id}
                    user={user}
                    onDelete={deleteUser}
                />
            ))}
        </div>
    );
}

// Componente hijo
function UserCard({ user, onDelete }) {
    return (
        <div className="user-card">
            <h3>{user.name}</h3>
            <p>{user.email}</p>
            <button onClick={() => onDelete(user.id)}>
                Eliminar
            </button>
        </div>
    );
}
```

#### **2. Props de Hijo a Padre (Callback)**
```jsx
// Componente padre
function TodoApp() {
    const [todos, setTodos] = useState([]);
    
    const addTodo = (text) => {
        const newTodo = {
            id: Date.now(),
            text,
            completed: false
        };
        setTodos(prevTodos => [...prevTodos, newTodo]);
    };
    
    const toggleTodo = (id) => {
        setTodos(prevTodos =>
            prevTodos.map(todo =>
                todo.id === id
                    ? { ...todo, completed: !todo.completed }
                    : todo
            )
        );
    };
    
    const deleteTodo = (id) => {
        setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id));
    };
    
    return (
        <div>
            <h1>Lista de Tareas</h1>
            <AddTodoForm onAdd={addTodo} />
            <TodoList
                todos={todos}
                onToggle={toggleTodo}
                onDelete={deleteTodo}
            />
        </div>
    );
}

// Componente hijo para agregar tareas
function AddTodoForm({ onAdd }) {
    const [text, setText] = useState('');
    
    const handleSubmit = (e) => {
        e.preventDefault();
        if (text.trim()) {
            onAdd(text.trim());
            setText('');
        }
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={text}
                onChange={(e) => setText(e.target.value)}
                placeholder="Nueva tarea..."
            />
            <button type="submit">Agregar</button>
        </form>
    );
}
```

---

## ⏰ **CICLO DE VIDA Y USEEFFECT**

### **useEffect Hook**

#### **1. useEffect Básico**
```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    // useEffect que se ejecuta cuando cambia userId
    useEffect(() => {
        const fetchUser = async () => {
            try {
                setLoading(true);
                setError(null);
                
                const response = await fetch(`/api/users/${userId}`);
                if (!response.ok) {
                    throw new Error('Usuario no encontrado');
                }
                
                const userData = await response.json();
                setUser(userData);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchUser();
    }, [userId]); // Dependencia: se ejecuta cuando userId cambia
    
    if (loading) return <div>Cargando...</div>;
    if (error) return <div>Error: {error}</div>;
    if (!user) return <div>Usuario no encontrado</div>;
    
    return (
        <div>
            <h2>{user.name}</h2>
            <p>Email: {user.email}</p>
            <p>Edad: {user.age}</p>
        </div>
    );
}
```

#### **2. useEffect con Cleanup**
```jsx
function Timer() {
    const [count, setCount] = useState(0);
    const [isRunning, setIsRunning] = useState(false);
    
    useEffect(() => {
        let intervalId;
        
        if (isRunning) {
            intervalId = setInterval(() => {
                setCount(prevCount => prevCount + 1);
            }, 1000);
        }
        
        // Cleanup function
        return () => {
            if (intervalId) {
                clearInterval(intervalId);
            }
        };
    }, [isRunning]); // Se ejecuta cuando isRunning cambia
    
    const startTimer = () => setIsRunning(true);
    const stopTimer = () => setIsRunning(false);
    const resetTimer = () => setCount(0);
    
    return (
        <div>
            <h2>Contador: {count}</h2>
            <button onClick={startTimer} disabled={isRunning}>
                Iniciar
            </button>
            <button onClick={stopTimer} disabled={!isRunning}>
                Detener
            </button>
            <button onClick={resetTimer}>
                Reset
            </button>
        </div>
    );
}
```

#### **3. useEffect para Event Listeners**
```jsx
function WindowSize() {
    const [windowSize, setWindowSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setWindowSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        // Agregar event listener
        window.addEventListener('resize', handleResize);
        
        // Cleanup: remover event listener
        return () => {
            window.removeEventListener('resize', handleResize);
        };
    }, []); // Array vacío: solo se ejecuta una vez al montar
    
    return (
        <div>
            <h2>Tamaño de la Ventana</h2>
            <p>Ancho: {windowSize.width}px</p>
            <p>Alto: {windowSize.height}px</p>
        </div>
    );
}
```

---

## 🎨 **ESTILOS EN REACT**

### **CSS Modules**

#### **1. CSS Module Básico**
```css
/* UserCard.module.css */
.userCard {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 1rem;
    margin: 1rem 0;
    background: white;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.userCard:hover {
    box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    transform: translateY(-2px);
    transition: all 0.3s ease;
}

.title {
    color: #333;
    margin: 0 0 0.5rem 0;
    font-size: 1.25rem;
}

.email {
    color: #666;
    margin: 0 0 1rem 0;
}

.deleteButton {
    background: #dc3545;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.deleteButton:hover {
    background: #c82333;
}
```

```jsx
// Uso del CSS Module
import styles from './UserCard.module.css';

function UserCard({ user, onDelete }) {
    return (
        <div className={styles.userCard}>
            <h3 className={styles.title}>{user.name}</h3>
            <p className={styles.email}>{user.email}</p>
            <button 
                className={styles.deleteButton}
                onClick={() => onDelete(user.id)}
            >
                Eliminar
            </button>
        </div>
    );
}
```

#### **2. CSS Modules con Composición**
```css
/* Button.module.css */
.button {
    padding: 0.75rem 1.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
    transition: all 0.3s ease;
}

.primary {
    background: #007bff;
    color: white;
}

.primary:hover {
    background: #0056b3;
}

.secondary {
    background: #6c757d;
    color: white;
}

.secondary:hover {
    background: #545b62;
}

.danger {
    background: #dc3545;
    color: white;
}

.danger:hover {
    background: #c82333;
}

.large {
    padding: 1rem 2rem;
    font-size: 1.125rem;
}

.small {
    padding: 0.5rem 1rem;
    font-size: 0.875rem;
}
```

```jsx
// Uso con composición de clases
import styles from './Button.module.css';

function Button({ 
    children, 
    variant = 'primary', 
    size = 'medium', 
    onClick 
}) {
    const buttonClasses = [
        styles.button,
        styles[variant],
        size !== 'medium' && styles[size]
    ].filter(Boolean).join(' ');
    
    return (
        <button className={buttonClasses} onClick={onClick}>
            {children}
        </button>
    );
}

// Uso
function App() {
    return (
        <div>
            <Button variant="primary" size="large">
                Botón Grande
            </Button>
            <Button variant="secondary">
                Botón Secundario
            </Button>
            <Button variant="danger" size="small">
                Eliminar
            </Button>
        </div>
    );
}
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Organización de Componentes**
- **Un componente por archivo**
- **Nombres descriptivos** para componentes
- **Separación de responsabilidades**
- **Reutilización** de componentes comunes

### **2. Performance**
- **React.memo** para componentes que no cambian
- **useCallback** para funciones que se pasan como props
- **useMemo** para cálculos costosos
- **Lazy loading** para componentes grandes

### **3. Estado**
- **Estado local** cuando sea posible
- **Lifting state up** cuando sea necesario
- **Inmutabilidad** en el estado
- **Estructura plana** del estado

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- **React Docs**: [react.dev](https://react.dev/)
- **Create React App**: [create-react-app.dev](https://create-react-app.dev/)

### **Herramientas de Desarrollo**
- **React Developer Tools**: Extensión del navegador
- **Create React App**: Herramienta de configuración
- **Vite**: Build tool moderno para React

---

*React.js es la base fundamental del desarrollo frontend moderno. Dominar sus conceptos básicos te permitirá crear aplicaciones web interactivas, mantenibles y escalables.* 