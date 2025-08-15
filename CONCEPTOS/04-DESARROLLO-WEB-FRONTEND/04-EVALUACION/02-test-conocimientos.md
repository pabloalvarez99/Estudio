# 📝 Test de Conocimientos: Desarrollo Web Frontend

## 🎯 **OBJETIVO**
Evaluar el dominio teórico y práctico de los conceptos fundamentales de Desarrollo Web Frontend mediante preguntas específicas y casos de uso.

---

## 📋 **INSTRUCCIONES**
- **Tiempo**: 90 minutos
- **Preguntas**: 50 preguntas (25 teóricas + 25 prácticas)
- **Puntuación**: 2 puntos por pregunta correcta
- **Aprobado**: 70 puntos (70%)
- **Excelente**: 90 puntos (90%)

---

## 🧠 **PREGUNTAS TEÓRICAS**

### **HTML5 Fundamentos (5 preguntas)**

#### 1. **Elementos Semánticos**
¿Cuál de los siguientes elementos HTML5 es más apropiado para contener el contenido principal de una página web?

a) `<div class="main">`
b) `<main>`
c) `<section class="main">`
d) `<article class="main">`

**Respuesta correcta**: b) `<main>`

**Explicación**: El elemento `<main>` es específicamente para el contenido principal de la página, mientras que `<div>` es genérico y `<section>` y `<article>` tienen propósitos más específicos.

#### 2. **Validación de Formularios**
¿Qué atributo HTML5 se utiliza para especificar que un campo es obligatorio?

a) `mandatory`
b) `required`
c) `necessary`
d) `obligatory`

**Respuesta correcta**: b) `required`

**Explicación**: El atributo `required` es nativo de HTML5 y valida que el campo tenga un valor antes de enviar el formulario.

#### 3. **Accesibilidad ARIA**
¿Cuál es el propósito principal de los atributos ARIA en HTML5?

a) Mejorar el SEO
b) Añadir estilos CSS
c) Mejorar la accesibilidad para lectores de pantalla
d) Optimizar el rendimiento

**Respuesta correcta**: c) Mejorar la accesibilidad para lectores de pantalla

**Explicación**: ARIA (Accessible Rich Internet Applications) está diseñado específicamente para hacer el contenido web más accesible para personas con discapacidades.

#### 4. **Metadatos SEO**
¿Qué meta tag es más importante para el SEO de una página web?

a) `<meta name="keywords">`
b) `<meta name="description">`
c) `<meta name="author">`
d) `<meta name="robots">`

**Respuesta correcta**: b) `<meta name="description">`

**Explicación**: La descripción es lo que aparece en los resultados de búsqueda y es fundamental para el SEO, mientras que keywords tiene menos peso actualmente.

#### 5. **Multimedia HTML5**
¿Qué elemento HTML5 se utiliza para reproducir archivos de audio?

a) `<sound>`
b) `<audio>`
c) `<media>`
d) `<player>`

**Respuesta correcta**: b) `<audio>`

**Explicación**: El elemento `<audio>` es nativo de HTML5 y permite reproducir archivos de audio con controles integrados.

---

### **CSS3 Estilos (5 preguntas)**

#### 6. **Flexbox Layout**
¿Qué propiedad CSS se utiliza para cambiar la dirección del eje principal en Flexbox?

a) `flex-direction`
b) `flex-orientation`
c) `flex-flow`
d) `flex-axis`

**Respuesta correcta**: a) `flex-direction`

**Explicación**: `flex-direction` controla si los elementos se organizan en fila (`row`) o columna (`column`).

#### 7. **CSS Grid**
¿Qué propiedad CSS Grid se utiliza para definir el número de columnas?

a) `grid-columns`
b) `grid-template-columns`
c) `grid-layout-columns`
d) `grid-col`

**Respuesta correcta**: b) `grid-template-columns`

**Explicación**: `grid-template-columns` define el número y tamaño de las columnas en un grid.

#### 8. **Variables CSS**
¿Cómo se declara una variable CSS personalizada?

a) `$variable-name: value;`
b) `--variable-name: value;`
c) `@variable-name: value;`
d) `var(variable-name): value;`

**Respuesta correcta**: b) `--variable-name: value;`

**Explicación**: Las variables CSS se declaran con dos guiones (`--`) y se usan con la función `var()`.

#### 9. **Media Queries**
¿Cuál es la sintaxis correcta para una media query que se aplique solo a pantallas grandes?

a) `@media (min-width: 1024px)`
b) `@media screen and (width > 1024px)`
c) `@media (max-width: 1024px)`
d) `@media (width >= 1024px)`

**Respuesta correcta**: a) `@media (min-width: 1024px)`

**Explicación**: `min-width` se aplica cuando la pantalla es mayor o igual al valor especificado.

#### 10. **Animaciones CSS**
¿Qué propiedad CSS se utiliza para definir la duración de una transición?

a) `transition-time`
b) `transition-duration`
c) `animation-duration`
d) `duration`

**Respuesta correcta**: b) `transition-duration`

**Explicación**: `transition-duration` especifica cuánto tiempo dura la transición.

---

### **JavaScript ES6+ (5 preguntas)**

#### 11. **Arrow Functions**
¿Cuál es la sintaxis correcta para una arrow function que retorna un objeto literal?

a) `() => { return {} }`
b) `() => ({})`
c) `() => return {}`
d) `() => {}`

**Respuesta correcta**: b) `() => ({})`

**Explicación**: Los paréntesis son necesarios para retornar un objeto literal directamente.

#### 12. **Destructuring**
¿Qué resultado produce el siguiente código?
```javascript
const [a, b, ...rest] = [1, 2, 3, 4, 5];
console.log(rest);
```

a) `[3, 4, 5]`
b) `[1, 2, 3, 4, 5]`
c) `[2, 3, 4, 5]`
d) `[1, 2]`

**Respuesta correcta**: a) `[3, 4, 5]`

**Explicación**: El operador rest (`...rest`) captura todos los elementos restantes después de asignar `a=1` y `b=2`.

#### 13. **Template Literals**
¿Cuál es la sintaxis correcta para incluir una variable en un template literal?

a) `'Hello ${name}'`
b) `"Hello ${name}"`
c) `` `Hello ${name}` ``
d) `'Hello ' + name`

**Respuesta correcta**: c) `` `Hello ${name}` ``

**Explicación**: Los template literals usan backticks (`` ` ``) y la interpolación `${}`.

#### 14. **Promesas**
¿Qué método de Promise se ejecuta cuando la promesa se rechaza?

a) `.then()`
b) `.catch()`
c) `.finally()`
d) `.reject()`

**Respuesta correcta**: b) `.catch()`

**Explicación**: `.catch()` maneja los errores y rechazos de una promesa.

#### 15. **Async/Await**
¿Cuál es la diferencia principal entre `async/await` y las promesas tradicionales?

a) `async/await` es más rápido
b) `async/await` permite código más legible y síncrono
c) `async/await` no maneja errores
d) `async/await` solo funciona en navegadores modernos

**Respuesta correcta**: b) `async/await` permite código más legible y síncrono

**Explicación**: `async/await` hace que el código asíncrono se vea y se comporte como código síncrono, mejorando la legibilidad.

---

### **Responsive Design (5 preguntas)**

#### 16. **Mobile-First**
¿Cuál es el principio fundamental del diseño mobile-first?

a) Diseñar primero para móvil y luego escalar hacia arriba
b) Diseñar primero para desktop y luego adaptar a móvil
c) Diseñar para todos los dispositivos simultáneamente
d) Usar solo CSS para responsive design

**Respuesta correcta**: a) Diseñar primero para móvil y luego escalar hacia arriba

**Explicación**: Mobile-first significa comenzar con el diseño más simple (móvil) y agregar complejidad progresivamente.

#### 17. **Breakpoints**
¿Cuál es un breakpoint típico para tablets?

a) `768px`
b) `480px`
c) `1024px`
d) `1200px`

**Respuesta correcta**: a) `768px`

**Explicación**: `768px` es un breakpoint común para tablets, entre móvil y desktop.

#### 18. **Unidades Responsive**
¿Qué unidad CSS es más apropiada para tipografía responsive?

a) `px`
b) `em`
c) `rem`
d) `pt`

**Respuesta correcta**: c) `rem`

**Explicación**: `rem` es relativa al tamaño de fuente del elemento raíz y es ideal para tipografía responsive.

#### 19. **Imágenes Responsive**
¿Qué atributo HTML se utiliza para especificar diferentes tamaños de imagen?

a) `srcset`
b) `sizes`
c) `responsive`
d) `adaptive`

**Respuesta correcta**: a) `srcset`

**Explicación**: `srcset` permite especificar múltiples fuentes de imagen para diferentes resoluciones.

#### 20. **CSS Grid Responsive**
¿Qué función CSS se utiliza para crear columnas que se ajusten automáticamente?

a) `auto-fit`
b) `auto-fill`
c) `auto-grid`
d) `responsive-grid`

**Respuesta correcta**: a) `auto-fit`

**Explicación**: `auto-fit` en CSS Grid crea columnas que se ajustan automáticamente al espacio disponible.

---

### **React.js Fundamentos (5 preguntas)**

#### 21. **Componentes Funcionales**
¿Cuál es la diferencia principal entre componentes funcionales y de clase en React?

a) Los funcionales son más rápidos
b) Los funcionales usan hooks en lugar de lifecycle methods
c) Los funcionales no pueden tener estado
d) Los funcionales solo pueden recibir props

**Respuesta correcta**: b) Los funcionales usan hooks en lugar de lifecycle methods

**Explicación**: Los componentes funcionales modernos usan hooks como `useState` y `useEffect` en lugar de métodos de ciclo de vida.

#### 22. **JSX**
¿Qué característica de JSX permite usar expresiones JavaScript?

a) `{{}}`
b) `{}`
c) `()`
d) `[]`

**Respuesta correcta**: b) `{}`

**Explicación**: Las llaves `{}` en JSX permiten incluir expresiones JavaScript.

#### 23. **Props**
¿Cómo se pasan props a un componente hijo en React?

a) Como atributos HTML
b) Como parámetros de función
c) Como variables globales
d) Como estado del componente padre

**Respuesta correcta**: a) Como atributos HTML

**Explicación**: Las props se pasan como atributos HTML y se reciben como parámetros de la función del componente.

#### 24. **Estado Local**
¿Qué hook se utiliza para manejar estado local en componentes funcionales?

a) `useState`
b) `useEffect`
c) `useContext`
d) `useReducer`

**Respuesta correcta**: a) `useState`

**Explicación**: `useState` es el hook principal para manejar estado local en componentes funcionales.

#### 25. **useEffect**
¿Cuál es el propósito principal del hook `useEffect`?

a) Manejar eventos del usuario
b) Ejecutar efectos secundarios
c) Optimizar el rendimiento
d) Manejar el estado global

**Respuesta correcta**: b) Ejecutar efectos secundarios

**Explicación**: `useEffect` se utiliza para ejecutar efectos secundarios como llamadas a APIs, suscripciones, o manipulación del DOM.

---

## 💻 **PREGUNTAS PRÁCTICAS**

### **HTML5 y CSS3 (5 preguntas)**

#### 26. **Formulario Semántico**
Crea un formulario de contacto semántico con los siguientes campos:
- Nombre (requerido)
- Email (requerido, validación HTML5)
- Mensaje (textarea, requerido)
- Botón de envío

**Respuesta esperada**:
```html
<form action="/contact" method="POST" novalidate>
  <div class="form-group">
    <label for="name">Nombre *</label>
    <input 
      type="text" 
      id="name" 
      name="name" 
      required 
      aria-describedby="name-error"
    />
    <div id="name-error" class="error-message" role="alert"></div>
  </div>
  
  <div class="form-group">
    <label for="email">Email *</label>
    <input 
      type="email" 
      id="email" 
      name="email" 
      required 
      aria-describedby="email-error"
    />
    <div id="email-error" class="error-message" role="alert"></div>
  </div>
  
  <div class="form-group">
    <label for="message">Mensaje *</label>
    <textarea 
      id="message" 
      name="message" 
      required 
      rows="4"
      aria-describedby="message-error"
    ></textarea>
    <div id="message-error" class="error-message" role="alert"></div>
  </div>
  
  <button type="submit">Enviar Mensaje</button>
</form>
```

#### 27. **Layout con Flexbox**
Crea un layout de 3 columnas usando Flexbox que se adapte a móvil (1 columna), tablet (2 columnas) y desktop (3 columnas).

**Respuesta esperada**:
```css
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.column {
  flex: 1 1 100%;
  min-width: 0;
}

@media (min-width: 768px) {
  .column {
    flex: 1 1 calc(50% - 0.5rem);
  }
}

@media (min-width: 1024px) {
  .column {
    flex: 1 1 calc(33.333% - 0.667rem);
  }
}
```

#### 28. **Grid Responsive**
Crea un grid de productos que muestre 1 columna en móvil, 2 en tablet, 3 en desktop y 4 en pantallas grandes.

**Respuesta esperada**:
```css
.products-grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
}

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

@media (min-width: 1280px) {
  .products-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

#### 29. **Animación CSS**
Crea una animación que haga que un botón se agrande ligeramente al hacer hover y cambie de color.

**Respuesta esperada**:
```css
.button {
  padding: 0.75rem 1.5rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
```

#### 30. **Variables CSS**
Crea un sistema de variables CSS para colores, espaciado y tipografía, y aplícalo a un componente.

**Respuesta esperada**:
```css
:root {
  /* Colores */
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --success-color: #28a745;
  --danger-color: #dc3545;
  
  /* Espaciado */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  
  /* Tipografía */
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
}

.card {
  padding: var(--spacing-lg);
  background-color: var(--primary-color);
  font-size: var(--font-size-lg);
  margin-bottom: var(--spacing-md);
}
```

---

### **JavaScript ES6+ (5 preguntas)**

#### 31. **Función con Arrow Functions**
Convierte la siguiente función tradicional a una arrow function:
```javascript
function multiply(a, b) {
  return a * b;
}
```

**Respuesta esperada**:
```javascript
const multiply = (a, b) => a * b;
```

#### 32. **Destructuring de Objetos**
Extrae las propiedades `name`, `age` y `city` del siguiente objeto:
```javascript
const person = {
  name: 'Juan',
  age: 30,
  city: 'Madrid',
  country: 'España'
};
```

**Respuesta esperada**:
```javascript
const { name, age, city } = person;
```

#### 33. **Template Literals**
Convierte la siguiente concatenación de strings a template literals:
```javascript
const firstName = 'Ana';
const lastName = 'García';
const greeting = 'Hola ' + firstName + ' ' + lastName + ', bienvenida!';
```

**Respuesta esperada**:
```javascript
const firstName = 'Ana';
const lastName = 'García';
const greeting = `Hola ${firstName} ${lastName}, bienvenida!`;
```

#### 34. **Array Methods**
Usa métodos de array modernos para filtrar números pares y duplicarlos:
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
```

**Respuesta esperada**:
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const evenDoubled = numbers
  .filter(num => num % 2 === 0)
  .map(num => num * 2);
// Resultado: [4, 8, 12, 16, 20]
```

#### 35. **Async/Await**
Convierte la siguiente promesa a async/await:
```javascript
fetch('/api/users')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

**Respuesta esperada**:
```javascript
async function fetchUsers() {
  try {
    const response = await fetch('/api/users');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

---

### **React.js (5 preguntas)**

#### 36. **Componente Funcional**
Crea un componente funcional que muestre un contador con botones para incrementar y decrementar.

**Respuesta esperada**:
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  
  return (
    <div className="counter">
      <h2>Contador: {count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}

export default Counter;
```

#### 37. **Hook Personalizado**
Crea un hook personalizado `useLocalStorage` que permita guardar y recuperar datos del localStorage.

**Respuesta esperada**:
```jsx
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };

  return [storedValue, setValue];
}

export default useLocalStorage;
```

#### 38. **useEffect con Cleanup**
Crea un componente que se suscriba a un evento del navegador y limpie la suscripción al desmontar.

**Respuesta esperada**:
```jsx
import React, { useState, useEffect } from 'react';

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

    window.addEventListener('resize', handleResize);
    
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return (
    <div>
      <p>Ancho: {windowSize.width}px</p>
      <p>Alto: {windowSize.height}px</p>
    </div>
  );
}

export default WindowSize;
```

#### 39. **Context API**
Crea un contexto para el tema (claro/oscuro) y un componente que lo consuma.

**Respuesta esperada**:
```jsx
// ThemeContext.jsx
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme debe usarse dentro de ThemeProvider');
  }
  return context;
}

// ThemeToggle.jsx
import React from 'react';
import { useTheme } from './ThemeContext';

function ThemeToggle() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <button onClick={toggleTheme}>
      Tema actual: {theme}
    </button>
  );
}

export default ThemeToggle;
```

#### 40. **Componente con Props**
Crea un componente `Button` reutilizable que acepte diferentes variantes y tamaños.

**Respuesta esperada**:
```jsx
import React from 'react';

function Button({ 
  children, 
  variant = 'primary', 
  size = 'medium', 
  onClick, 
  disabled = false,
  className = ''
}) {
  const baseClass = 'btn';
  const variantClass = `btn--${variant}`;
  const sizeClass = `btn--${size}`;
  
  const buttonClass = `${baseClass} ${variantClass} ${sizeClass} ${className}`.trim();
  
  return (
    <button
      className={buttonClass}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
}

export default Button;
```

---

### **Integración Frontend (5 preguntas)**

#### 41. **Formulario React con Validación**
Crea un formulario de registro en React con validación en tiempo real.

**Respuesta esperada**:
```jsx
import React, { useState } from 'react';

function RegisterForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    confirmPassword: ''
  });
  
  const [errors, setErrors] = useState({});
  
  const validateField = (name, value) => {
    switch (name) {
      case 'username':
        if (value.length < 3) return 'El usuario debe tener al menos 3 caracteres';
        break;
      case 'email':
        if (!/\S+@\S+\.\S+/.test(value)) return 'Email inválido';
        break;
      case 'password':
        if (value.length < 6) return 'La contraseña debe tener al menos 6 caracteres';
        break;
      case 'confirmPassword':
        if (value !== formData.password) return 'Las contraseñas no coinciden';
        break;
      default:
        return '';
    }
    return '';
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    const error = validateField(name, value);
    setErrors(prev => ({ ...prev, [name]: error }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    // Lógica de envío
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="username">Usuario:</label>
        <input
          type="text"
          id="username"
          name="username"
          value={formData.username}
          onChange={handleChange}
        />
        {errors.username && <span className="error">{errors.username}</span>}
      </div>
      
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      
      <div>
        <label htmlFor="password">Contraseña:</label>
        <input
          type="password"
          id="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      
      <div>
        <label htmlFor="confirmPassword">Confirmar Contraseña:</label>
        <input
          type="password"
          id="confirmPassword"
          name="confirmPassword"
          value={formData.confirmPassword}
          onChange={handleChange}
        />
        {errors.confirmPassword && <span className="error">{errors.confirmPassword}</span>}
      </div>
      
      <button type="submit">Registrarse</button>
    </form>
  );
}

export default RegisterForm;
```

#### 42. **Lista Responsive con React**
Crea una lista de productos que se adapte a diferentes tamaños de pantalla.

**Respuesta esperada**:
```jsx
import React from 'react';
import './ProductList.css';

function ProductList({ products }) {
  return (
    <div className="product-list">
      {products.map(product => (
        <div key={product.id} className="product-item">
          <img src={product.image} alt={product.name} />
          <h3>{product.name}</h3>
          <p>{product.description}</p>
          <span className="price">${product.price}</span>
        </div>
      ))}
    </div>
  );
}

// CSS correspondiente
/*
.product-list {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  .product-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .product-list {
    grid-template-columns: repeat(3, 1fr);
  }
}
*/

export default ProductList;
```

#### 43. **Hook Personalizado para API**
Crea un hook personalizado que maneje llamadas a una API con loading y error states.

**Respuesta esperada**:
```jsx
import { useState, useEffect } from 'react';

function useApi(endpoint) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(endpoint);
        if (!response.ok) {
          throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [endpoint]);

  const refetch = () => {
    setLoading(true);
    setError(null);
    fetchData();
  };

  return { data, loading, error, refetch };
}

export default useApi;
```

#### 44. **Componente con Estado Global**
Crea un componente que use Context API para manejar un carrito de compras.

**Respuesta esperada**:
```jsx
import React, { useContext } from 'react';
import { CartContext } from './CartContext';

function CartItem({ item }) {
  const { removeFromCart, updateQuantity } = useContext(CartContext);
  
  const handleQuantityChange = (newQuantity) => {
    if (newQuantity <= 0) {
      removeFromCart(item.id);
    } else {
      updateQuantity(item.id, newQuantity);
    }
  };
  
  return (
    <div className="cart-item">
      <img src={item.image} alt={item.name} />
      <div className="item-details">
        <h3>{item.name}</h3>
        <p>${item.price}</p>
      </div>
      <div className="quantity-controls">
        <button onClick={() => handleQuantityChange(item.quantity - 1)}>-</button>
        <span>{item.quantity}</span>
        <button onClick={() => handleQuantityChange(item.quantity + 1)}>+</button>
      </div>
      <button onClick={() => removeFromCart(item.id)}>Eliminar</button>
    </div>
  );
}

export default CartItem;
```

#### 45. **Optimización de Performance**
Implementa `React.memo` y `useCallback` para optimizar un componente que recibe funciones como props.

**Respuesta esperada**:
```jsx
import React, { useCallback, useState } from 'react';

const ExpensiveComponent = React.memo(({ onItemClick, items }) => {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id} onClick={() => onItemClick(item.id)}>
          {item.name}
        </li>
      ))}
    </ul>
  );
});

function ParentComponent() {
  const [items, setItems] = useState([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ]);
  
  const [selectedId, setSelectedId] = useState(null);
  
  const handleItemClick = useCallback((id) => {
    setSelectedId(id);
  }, []);
  
  return (
    <div>
      <ExpensiveComponent 
        items={items} 
        onItemClick={handleItemClick} 
      />
      {selectedId && <p>Seleccionado: {selectedId}</p>}
    </div>
  );
}

export default ParentComponent;
```

---

## 📊 **SISTEMA DE PUNTUACIÓN**

### **Preguntas Teóricas (50 puntos)**
- **HTML5**: 10 puntos
- **CSS3**: 10 puntos  
- **JavaScript ES6+**: 10 puntos
- **Responsive Design**: 10 puntos
- **React.js**: 10 puntos

### **Preguntas Prácticas (50 puntos)**
- **HTML5 y CSS3**: 10 puntos
- **JavaScript ES6+**: 10 puntos
- **React.js**: 10 puntos
- **Integración Frontend**: 20 puntos

### **Total: 100 puntos**

---

## 🎯 **CRITERIOS DE EVALUACIÓN**

### **Nivel Principiante (0-50 puntos)**
- Conceptos básicos conocidos
- Necesita más práctica y estudio
- Recomendación: Revisar fundamentos

### **Nivel Intermedio (51-70 puntos)**
- Conocimiento sólido de conceptos básicos
- Algunas áreas requieren refuerzo
- Recomendación: Practicar proyectos

### **Nivel Avanzado (71-85 puntos)**
- Dominio sólido de la mayoría de conceptos
- Pocas áreas de mejora
- Recomendación: Especialización

### **Nivel Experto (86-100 puntos)**
- Maestría completa de todos los conceptos
- Listo para roles senior
- Recomendación: Liderazgo técnico

---

## 💡 **RECOMENDACIONES POST-TEST**

### **Si obtuviste menos de 50 puntos:**
- Revisa los fundamentos de HTML5, CSS3 y JavaScript
- Practica con ejercicios básicos
- Usa recursos como MDN y freeCodeCamp

### **Si obtuviste 51-70 puntos:**
- Enfócate en las áreas con menor puntuación
- Practica con proyectos pequeños
- Profundiza en React y patrones avanzados

### **Si obtuviste 71-85 puntos:**
- Refuerza las áreas específicas de mejora
- Construye proyectos complejos
- Considera especializarte en áreas específicas

### **Si obtuviste 86-100 puntos:**
- Comparte tu conocimiento
- Contribuye a proyectos open source
- Considera roles de liderazgo técnico

---

*Este test te ayudará a identificar tu nivel actual y planificar tu desarrollo profesional en Frontend Development.* 