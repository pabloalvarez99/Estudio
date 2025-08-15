# 🧩 Componentes React: Composición y Patrones Avanzados

## 🎯 **OBJETIVO**
Dominar la composición de componentes React, patrones avanzados como Higher-Order Components, Render Props, y las mejores prácticas para crear componentes reutilizables y mantenibles.

---

## 🏗️ **COMPOSICIÓN DE COMPONENTES**

### **Composición Básica**

#### **1. Composición por Children**
```jsx
// Componente contenedor que renderiza children
function Card({ title, children, className = '' }) {
    return (
        <div className={`card ${className}`}>
            <div className="card-header">
                <h3 className="card-title">{title}</h3>
            </div>
            <div className="card-body">
                {children}
            </div>
        </div>
    );
}

// Uso del componente
function App() {
    return (
        <div className="app">
            <Card title="Información del Usuario">
                <p>Nombre: Juan Pérez</p>
                <p>Email: juan@example.com</p>
                <button>Editar</button>
            </Card>
            
            <Card title="Estadísticas" className="stats-card">
                <div className="stats-grid">
                    <div>Posts: 25</div>
                    <div>Seguidores: 150</div>
                    <div>Seguidos: 120</div>
                </div>
            </Card>
        </div>
    );
}
```

#### **2. Composición por Props**
```jsx
// Componente que acepta múltiples props para composición
function Layout({ header, sidebar, main, footer }) {
    return (
        <div className="layout">
            <header className="layout-header">
                {header}
            </header>
            
            <div className="layout-content">
                <aside className="layout-sidebar">
                    {sidebar}
                </aside>
                
                <main className="layout-main">
                    {main}
                </main>
            </div>
            
            <footer className="layout-footer">
                {footer}
            </footer>
        </div>
    );
}

// Uso del layout
function App() {
    return (
        <Layout
            header={<Header />}
            sidebar={<Sidebar />}
            main={<MainContent />}
            footer={<Footer />}
        />
    );
}

// Componentes individuales
function Header() {
    return (
        <nav className="header-nav">
            <h1>Mi Aplicación</h1>
            <ul>
                <li><a href="#home">Inicio</a></li>
                <li><a href="#about">Acerca de</a></li>
                <li><a href="#contact">Contacto</a></li>
            </ul>
        </nav>
    );
}

function Sidebar() {
    return (
        <div className="sidebar">
            <h3>Navegación</h3>
            <ul>
                <li>Dashboard</li>
                <li>Usuarios</li>
                <li>Configuración</li>
            </ul>
        </div>
    );
}
```

### **Composición Avanzada**

#### **1. Componente con Slots Nombrados**
```jsx
// Componente con slots nombrados usando props
function PageLayout({ 
    header, 
    sidebar, 
    main, 
    footer,
    headerProps = {},
    sidebarProps = {},
    mainProps = {},
    footerProps = {}
}) {
    return (
        <div className="page-layout">
            <header className="page-header" {...headerProps}>
                {header}
            </header>
            
            <div className="page-body">
                <aside className="page-sidebar" {...sidebarProps}>
                    {sidebar}
                </aside>
                
                <main className="page-main" {...mainProps}>
                    {main}
                </main>
            </div>
            
            <footer className="page-footer" {...footerProps}>
                {footer}
            </footer>
        </div>
    );
}

// Uso con slots nombrados
function App() {
    return (
        <PageLayout
            header={<AppHeader />}
            sidebar={<AppSidebar />}
            main={<AppMain />}
            footer={<AppFooter />}
            headerProps={{ className: 'app-header' }}
            sidebarProps={{ className: 'app-sidebar' }}
            mainProps={{ className: 'app-main' }}
            footerProps={{ className: 'app-footer' }}
        />
    );
}
```

#### **2. Componente con Render Props**
```jsx
// Componente que usa render props para máxima flexibilidad
function DataFetcher({ url, children }) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        const fetchData = async () => {
            try {
                setLoading(true);
                const response = await fetch(url);
                const result = await response.json();
                setData(result);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };
        
        fetchData();
    }, [url]);
    
    return children({ data, loading, error });
}

// Uso del componente
function UserList() {
    return (
        <DataFetcher url="/api/users">
            {({ data, loading, error }) => {
                if (loading) return <div>Cargando usuarios...</div>;
                if (error) return <div>Error: {error}</div>;
                if (!data) return <div>No hay usuarios</div>;
                
                return (
                    <ul className="user-list">
                        {data.map(user => (
                            <li key={user.id}>
                                {user.name} - {user.email}
                            </li>
                        ))}
                    </ul>
                );
            }}
        </DataFetcher>
    );
}
```

---

## 🔄 **HIGHER-ORDER COMPONENTS (HOC)**

### **Concepto de HOC**

#### **1. HOC Básico**
```jsx
// HOC que añade funcionalidad de loading
function withLoading(WrappedComponent) {
    return function WithLoadingComponent(props) {
        const [loading, setLoading] = useState(false);
        
        const wrappedProps = {
            ...props,
            loading,
            setLoading
        };
        
        return <WrappedComponent {...wrappedProps} />;
    };
}

// HOC que añade funcionalidad de error handling
function withErrorHandling(WrappedComponent) {
    return function WithErrorHandlingComponent(props) {
        const [error, setError] = useState(null);
        
        const wrappedProps = {
            ...props,
            error,
            setError,
            clearError: () => setError(null)
        };
        
        return <WrappedComponent {...wrappedProps} />;
    };
}

// Uso de HOCs
const UserFormWithLoading = withLoading(UserForm);
const UserFormWithErrorHandling = withErrorHandling(UserForm);
const UserFormWithBoth = withErrorHandling(withLoading(UserForm));

// Componente base
function UserForm({ loading, setLoading, error, setError, clearError }) {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        age: ''
    });
    
    const handleSubmit = async (e) => {
        e.preventDefault();
        setLoading(true);
        setError(null);
        
        try {
            // Simular envío de formulario
            await new Promise(resolve => setTimeout(resolve, 1000));
            console.log('Formulario enviado:', formData);
            setFormData({ name: '', email: '', age: '' });
        } catch (err) {
            setError('Error al enviar el formulario');
        } finally {
            setLoading(false);
        }
    };
    
    return (
        <form onSubmit={handleSubmit}>
            {error && (
                <div className="error-message">
                    {error}
                    <button type="button" onClick={clearError}>
                        ×
                    </button>
                </div>
            )}
            
            <div>
                <label>Nombre:</label>
                <input
                    type="text"
                    value={formData.name}
                    onChange={(e) => setFormData(prev => ({
                        ...prev,
                        name: e.target.value
                    }))}
                    required
                />
            </div>
            
            <div>
                <label>Email:</label>
                <input
                    type="email"
                    value={formData.email}
                    onChange={(e) => setFormData(prev => ({
                        ...prev,
                        email: e.target.value
                    }))}
                    required
                />
            </div>
            
            <div>
                <label>Edad:</label>
                <input
                    type="number"
                    value={formData.age}
                    onChange={(e) => setFormData(prev => ({
                        ...prev,
                        age: e.target.value
                    }))}
                    required
                />
            </div>
            
            <button type="submit" disabled={loading}>
                {loading ? 'Enviando...' : 'Enviar'}
            </button>
        </form>
    );
}
```

#### **2. HOC con Configuración**
```jsx
// HOC configurable que añade funcionalidad de autenticación
function withAuth(WrappedComponent, options = {}) {
    const {
        redirectTo = '/login',
        requiredRole = null,
        fallback = null
    } = options;
    
    return function WithAuthComponent(props) {
        const [user, setUser] = useState(null);
        const [loading, setLoading] = useState(true);
        
        useEffect(() => {
            // Simular verificación de autenticación
            const checkAuth = async () => {
                try {
                    const response = await fetch('/api/auth/me');
                    if (response.ok) {
                        const userData = await response.json();
                        setUser(userData);
                    } else {
                        // Redirigir si no está autenticado
                        window.location.href = redirectTo;
                    }
                } catch (error) {
                    console.error('Error de autenticación:', error);
                    window.location.href = redirectTo;
                } finally {
                    setLoading(false);
                }
            };
            
            checkAuth();
        }, [redirectTo]);
        
        if (loading) {
            return fallback || <div>Verificando autenticación...</div>;
        }
        
        if (!user) {
            return null;
        }
        
        // Verificar rol si es requerido
        if (requiredRole && user.role !== requiredRole) {
            return <div>Acceso denegado. Rol requerido: {requiredRole}</div>;
        }
        
        return <WrappedComponent {...props} user={user} />;
    };
}

// Uso del HOC con diferentes configuraciones
const AdminPanel = withAuth(AdminPanelComponent, {
    requiredRole: 'admin',
    redirectTo: '/admin/login',
    fallback: <div>Verificando permisos de administrador...</div>
});

const UserProfile = withAuth(UserProfileComponent, {
    redirectTo: '/login'
});

// Componentes protegidos
function AdminPanelComponent({ user }) {
    return (
        <div>
            <h1>Panel de Administración</h1>
            <p>Bienvenido, {user.name} (Admin)</p>
            <div className="admin-controls">
                <button>Gestionar Usuarios</button>
                <button>Configuración del Sistema</button>
                <button>Reportes</button>
            </div>
        </div>
    );
}

function UserProfileComponent({ user }) {
    return (
        <div>
            <h1>Perfil de Usuario</h1>
            <p>Nombre: {user.name}</p>
            <p>Email: {user.email}</p>
            <p>Rol: {user.role}</p>
        </div>
    );
}
```

---

## 🎭 **RENDER PROPS PATTERN**

### **Implementación de Render Props**

#### **1. Render Props Básico**
```jsx
// Componente que usa render props para renderizar contenido
function MouseTracker({ render }) {
    const [position, setPosition] = useState({ x: 0, y: 0 });
    
    useEffect(() => {
        const handleMouseMove = (event) => {
            setPosition({
                x: event.clientX,
                y: event.clientY
            });
        };
        
        window.addEventListener('mousemove', handleMouseMove);
        
        return () => {
            window.removeEventListener('mousemove', handleMouseMove);
        };
    }, []);
    
    return render(position);
}

// Uso del componente
function App() {
    return (
        <div>
            <h1>Rastreador del Mouse</h1>
            <MouseTracker
                render={({ x, y }) => (
                    <p>
                        Posición del mouse: ({x}, {y})
                    </p>
                )}
            />
            
            <MouseTracker
                render={({ x, y }) => (
                    <div
                        style={{
                            position: 'absolute',
                            left: x,
                            top: y,
                            width: 20,
                            height: 20,
                            backgroundColor: 'red',
                            borderRadius: '50%',
                            pointerEvents: 'none'
                        }}
                    />
                )}
            />
        </div>
    );
}
```

#### **2. Render Props con Estado Compartido**
```jsx
// Componente que comparte estado a través de render props
function Counter({ initialValue = 0, render }) {
    const [count, setCount] = useState(initialValue);
    
    const increment = () => setCount(prev => prev + 1);
    const decrement = () => setCount(prev => prev - 1);
    const reset = () => setCount(initialValue);
    
    const counterState = {
        count,
        increment,
        decrement,
        reset,
        isEven: count % 2 === 0,
        isPositive: count > 0
    };
    
    return render(counterState);
}

// Uso del componente
function App() {
    return (
        <div>
            <h1>Contadores con Render Props</h1>
            
            {/* Contador simple */}
            <Counter
                initialValue={5}
                render={({ count, increment, decrement, reset }) => (
                    <div className="counter">
                        <h3>Contador: {count}</h3>
                        <button onClick={increment}>+</button>
                        <button onClick={decrement}>-</button>
                        <button onClick={reset}>Reset</button>
                    </div>
                )}
            />
            
            {/* Contador con información adicional */}
            <Counter
                render={({ count, increment, decrement, isEven, isPositive }) => (
                    <div className="counter-advanced">
                        <h3>Contador Avanzado: {count}</h3>
                        <p>Es par: {isEven ? 'Sí' : 'No'}</p>
                        <p>Es positivo: {isPositive ? 'Sí' : 'No'}</p>
                        <button onClick={increment}>Incrementar</button>
                        <button onClick={decrement}>Decrementar</button>
                    </div>
                )}
            />
            
            {/* Contador con estilos condicionales */}
            <Counter
                render={({ count, increment, decrement, isEven }) => (
                    <div 
                        className="counter-styled"
                        style={{
                            backgroundColor: isEven ? '#e8f5e8' : '#ffe8e8',
                            padding: '1rem',
                            borderRadius: '8px',
                            border: `2px solid ${isEven ? '#4caf50' : '#f44336'}`
                        }}
                    >
                        <h3>Contador Estilizado: {count}</h3>
                        <p>Color: {isEven ? 'Verde' : 'Rojo'}</p>
                        <button onClick={increment}>+</button>
                        <button onClick={decrement}>-</button>
                    </div>
                )}
            />
        </div>
    );
}
```

---

## 🎨 **PATRONES DE COMPONENTES**

### **Compound Components**

#### **1. Componente Compuesto Básico**
```jsx
// Componente compuesto para formularios
const Form = ({ children, onSubmit, ...props }) => {
    return (
        <form onSubmit={onSubmit} {...props}>
            {children}
        </form>
    );
};

Form.Input = ({ label, ...props }) => (
    <div className="form-group">
        <label>{label}</label>
        <input {...props} />
    </div>
);

Form.TextArea = ({ label, ...props }) => (
    <div className="form-group">
        <label>{label}</label>
        <textarea {...props} />
    </div>
);

Form.Select = ({ label, options, ...props }) => (
    <div className="form-group">
        <label>{label}</label>
        <select {...props}>
            {options.map(option => (
                <option key={option.value} value={option.value}>
                    {option.label}
                </option>
            ))}
        </select>
    </div>
);

Form.Button = ({ children, ...props }) => (
    <button type="submit" {...props}>
        {children}
    </button>
);

// Uso del componente compuesto
function App() {
    const handleSubmit = (e) => {
        e.preventDefault();
        const formData = new FormData(e.target);
        console.log('Formulario enviado:', Object.fromEntries(formData));
    };
    
    return (
        <Form onSubmit={handleSubmit}>
            <Form.Input
                label="Nombre"
                name="name"
                type="text"
                required
            />
            
            <Form.TextArea
                label="Descripción"
                name="description"
                rows="4"
            />
            
            <Form.Select
                label="Categoría"
                name="category"
                options={[
                    { value: 'tech', label: 'Tecnología' },
                    { value: 'design', label: 'Diseño' },
                    { value: 'business', label: 'Negocios' }
                ]}
            />
            
            <Form.Button>Enviar</Form.Button>
        </Form>
    );
}
```

#### **2. Componente Compuesto con Context**
```jsx
// Context para el componente compuesto
const FormContext = React.createContext();

// Componente compuesto con estado compartido
const Form = ({ children, onSubmit, initialValues = {} }) => {
    const [values, setValues] = useState(initialValues);
    const [errors, setErrors] = useState({});
    
    const setValue = (name, value) => {
        setValues(prev => ({ ...prev, [name]: value }));
        // Limpiar error cuando se cambia el valor
        if (errors[name]) {
            setErrors(prev => ({ ...prev, [name]: '' }));
        }
    };
    
    const setError = (name, error) => {
        setErrors(prev => ({ ...prev, [name]: error }));
    };
    
    const validate = () => {
        const newErrors = {};
        
        // Validación básica
        if (!values.name) newErrors.name = 'El nombre es requerido';
        if (!values.email) newErrors.email = 'El email es requerido';
        if (values.email && !values.email.includes('@')) {
            newErrors.email = 'Email inválido';
        }
        
        setErrors(newErrors);
        return Object.keys(newErrors).length === 0;
    };
    
    const handleSubmit = (e) => {
        e.preventDefault();
        if (validate()) {
            onSubmit(values);
        }
    };
    
    const contextValue = {
        values,
        setValue,
        errors,
        setError
    };
    
    return (
        <FormContext.Provider value={contextValue}>
            <form onSubmit={handleSubmit}>
                {children}
            </form>
        </FormContext.Provider>
    );
};

// Hook personalizado para usar el contexto
const useFormContext = () => {
    const context = useContext(FormContext);
    if (!context) {
        throw new Error('useFormContext debe usarse dentro de Form');
    }
    return context;
};

// Componentes del formulario
Form.Input = ({ name, label, type = 'text', ...props }) => {
    const { values, setValue, errors } = useFormContext();
    
    return (
        <div className="form-group">
            <label>{label}</label>
            <input
                type={type}
                name={name}
                value={values[name] || ''}
                onChange={(e) => setValue(name, e.target.value)}
                {...props}
            />
            {errors[name] && (
                <span className="error">{errors[name]}</span>
            )}
        </div>
    );
};

Form.Button = ({ children, ...props }) => {
    const { errors } = useFormContext();
    const hasErrors = Object.values(errors).some(error => error);
    
    return (
        <button 
            type="submit" 
            disabled={hasErrors}
            {...props}
        >
            {children}
        </button>
    );
};

// Uso del componente compuesto
function App() {
    const handleSubmit = (formData) => {
        console.log('Formulario válido:', formData);
    };
    
    return (
        <Form onSubmit={handleSubmit}>
            <Form.Input
                name="name"
                label="Nombre"
                placeholder="Tu nombre"
            />
            
            <Form.Input
                name="email"
                label="Email"
                type="email"
                placeholder="tu@email.com"
            />
            
            <Form.Button>Enviar</Form.Button>
        </Form>
    );
}
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Composición vs Herencia**
- **Usa composición** en lugar de herencia
- **Props para configuración** y personalización
- **Children para contenido** flexible
- **Render props para lógica** compleja

### **2. Performance**
- **React.memo** para componentes que no cambian
- **useCallback** para funciones que se pasan como props
- **useMemo** para cálculos costosos
- **Evita recrear objetos** en cada render

### **3. Mantenibilidad**
- **Un componente por archivo**
- **Nombres descriptivos** y consistentes
- **Props tipadas** con PropTypes o TypeScript
- **Documentación clara** de la API del componente

---

## 📚 **RECURSOS ADICIONALES**

### **Patrones Avanzados**
- **Compound Components**: Para componentes relacionados
- **Render Props**: Para máxima flexibilidad
- **Higher-Order Components**: Para funcionalidad reutilizable
- **Custom Hooks**: Para lógica reutilizable

---

*La composición de componentes es fundamental en React. Dominar estos patrones te permitirá crear componentes más flexibles, reutilizables y mantenibles.* 