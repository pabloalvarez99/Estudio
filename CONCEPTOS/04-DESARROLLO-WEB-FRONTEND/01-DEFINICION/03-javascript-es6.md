# ⚡ JavaScript ES6+: Programación Moderna y DOM

## 🎯 **OBJETIVO**
Dominar JavaScript ES6+ moderno, incluyendo módulos, clases, funciones avanzadas, async/await, manipulación del DOM y mejores prácticas para crear aplicaciones web interactivas y dinámicas.

---

## 📖 **¿QUÉ ES JAVASCRIPT ES6+?**

### **Definición**
JavaScript ES6+ (ECMAScript 2015+) es la versión moderna de JavaScript que introduce nuevas características sintácticas y funcionales para hacer el código más legible, mantenible y potente.

### **Características Principales**
- **Módulos**: Import/export, ES6 modules
- **Clases**: Herencia, métodos estáticos, getters/setters
- **Funciones avanzadas**: Arrow functions, destructuring, rest/spread
- **Async/Await**: Promesas, manejo de errores
- **DOM manipulation**: Event handling, DOM traversal

---

## 📦 **MÓDULOS ES6**

### **Sistema de Módulos**

#### **1. Export de Módulos**
```javascript
// math.js - Módulo de funciones matemáticas
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export function multiply(a, b) {
    return a * b;
}

// Export por defecto
export default class Calculator {
    constructor() {
        this.result = 0;
    }
    
    add(value) {
        this.result += value;
        return this;
    }
    
    getResult() {
        return this.result;
    }
}
```

#### **2. Import de Módulos**
```javascript
// main.js - Importando módulos
import Calculator, { add, subtract, PI } from './math.js';

// Uso de funciones importadas
console.log(PI); // 3.14159
console.log(add(5, 3)); // 8
console.log(subtract(10, 4)); // 6

// Uso de la clase por defecto
const calc = new Calculator();
calc.add(5).add(3);
console.log(calc.getResult()); // 8
```

#### **3. Import/Export con Alias**
```javascript
// utils.js
export const formatDate = (date) => {
    return date.toLocaleDateString();
};

export const validateEmail = (email) => {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email);
};

// main.js
import { formatDate as format, validateEmail as isValidEmail } from './utils.js';

const today = new Date();
console.log(format(today));
console.log(isValidEmail('user@example.com'));
```

---

## 🏗️ **CLASES ES6**

### **Definición de Clases**

#### **1. Clase Básica**
```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Método de instancia
    greet() {
        return `Hola, soy ${this.name} y tengo ${this.age} años.`;
    }
    
    // Método estático
    static create(name, age) {
        return new Person(name, age);
    }
    
    // Getter
    get isAdult() {
        return this.age >= 18;
    }
    
    // Setter
    set age(value) {
        if (value < 0) {
            throw new Error('La edad no puede ser negativa');
        }
        this._age = value;
    }
    
    get age() {
        return this._age;
    }
}

// Uso de la clase
const person = new Person('Juan', 25);
console.log(person.greet()); // "Hola, soy Juan y tengo 25 años."
console.log(person.isAdult); // true

// Método estático
const person2 = Person.create('María', 30);
```

#### **2. Herencia de Clases**
```javascript
class Employee extends Person {
    constructor(name, age, position, salary) {
        super(name, age); // Llamada al constructor de la clase padre
        this.position = position;
        this.salary = salary;
    }
    
    // Sobrescritura de método
    greet() {
        return `${super.greet()} Trabajo como ${this.position}.`;
    }
    
    // Método específico de Employee
    getSalary() {
        return `Mi salario es $${this.salary}`;
    }
    
    // Método estático
    static createManager(name, age) {
        return new Employee(name, age, 'Manager', 50000);
    }
}

const employee = new Employee('Ana', 28, 'Developer', 40000);
console.log(employee.greet()); // "Hola, soy Ana y tengo 28 años. Trabajo como Developer."
console.log(employee.getSalary()); // "Mi salario es $40000"

const manager = Employee.createManager('Carlos', 35);
```

#### **3. Clases Abstractas (Simuladas)**
```javascript
class Shape {
    constructor() {
        if (this.constructor === Shape) {
            throw new Error('Shape es una clase abstracta');
        }
    }
    
    // Método abstracto
    getArea() {
        throw new Error('getArea debe ser implementado');
    }
    
    // Método abstracto
    getPerimeter() {
        throw new Error('getPerimeter debe ser implementado');
    }
}

class Circle extends Shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }
    
    getArea() {
        return Math.PI * this.radius ** 2;
    }
    
    getPerimeter() {
        return 2 * Math.PI * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    
    getArea() {
        return this.width * this.height;
    }
    
    getPerimeter() {
        return 2 * (this.width + this.height);
    }
}

// Uso
const circle = new Circle(5);
const rectangle = new Rectangle(4, 6);

console.log(circle.getArea()); // 78.54...
console.log(rectangle.getArea()); // 24
```

---

## 🚀 **FUNCIONES AVANZADAS**

### **Arrow Functions**

#### **1. Sintaxis Básica**
```javascript
// Función tradicional
function add(a, b) {
    return a + b;
}

// Arrow function equivalente
const add = (a, b) => a + b;

// Arrow function con múltiples líneas
const multiply = (a, b) => {
    const result = a * b;
    return result;
};

// Arrow function sin parámetros
const getRandomNumber = () => Math.random();

// Arrow function con un parámetro
const double = x => x * 2;
```

#### **2. Arrow Functions y Contexto**
```javascript
class Timer {
    constructor() {
        this.seconds = 0;
    }
    
    start() {
        // Arrow function mantiene el contexto de 'this'
        this.interval = setInterval(() => {
            this.seconds++;
            console.log(`Segundos: ${this.seconds}`);
        }, 1000);
    }
    
    stop() {
        clearInterval(this.interval);
    }
}

const timer = new Timer();
timer.start();
```

### **Destructuring**

#### **1. Destructuring de Arrays**
```javascript
// Destructuring básico
const numbers = [1, 2, 3, 4, 5];
const [first, second, ...rest] = numbers;

console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// Destructuring con valores por defecto
const [a = 0, b = 0, c = 0] = [1, 2];
console.log(a, b, c); // 1, 2, 0

// Intercambio de variables
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2, 1
```

#### **2. Destructuring de Objetos**
```javascript
const user = {
    name: 'Juan',
    age: 25,
    email: 'juan@example.com',
    address: {
        city: 'Madrid',
        country: 'España'
    }
};

// Destructuring básico
const { name, age, email } = user;
console.log(name, age, email);

// Destructuring con alias
const { name: userName, age: userAge } = user;
console.log(userName, userAge);

// Destructuring anidado
const { address: { city, country } } = user;
console.log(city, country);

// Destructuring con valores por defecto
const { phone = 'No disponible' } = user;
console.log(phone); // "No disponible"
```

### **Rest y Spread Operators**

#### **1. Rest Operator**
```javascript
// Rest en parámetros de función
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15

// Rest en destructuring
const [first, second, ...remaining] = [1, 2, 3, 4, 5];
console.log(remaining); // [3, 4, 5]

// Rest en objetos
const { name, ...otherProps } = user;
console.log(otherProps); // { age: 25, email: 'juan@example.com', address: {...} }
```

#### **2. Spread Operator**
```javascript
// Spread en arrays
const fruits = ['manzana', 'plátano'];
const moreFruits = [...fruits, 'naranja', 'uva'];
console.log(moreFruits); // ['manzana', 'plátano', 'naranja', 'uva']

// Spread en objetos
const baseUser = { name: 'Usuario', role: 'user' };
const adminUser = { ...baseUser, role: 'admin', permissions: ['read', 'write'] };
console.log(adminUser);

// Clonación de objetos
const userClone = { ...user };
console.log(userClone);

// Combinación de objetos
const config1 = { theme: 'dark', language: 'es' };
const config2 = { notifications: true, theme: 'light' };
const finalConfig = { ...config1, ...config2 }; // config2 sobrescribe config1
```

---

## ⏳ **ASYNC/AWAIT Y PROMESAS**

### **Promesas**

#### **1. Creación de Promesas**
```javascript
// Promesa básica
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = Math.random() > 0.5;
            
            if (success) {
                resolve({ data: 'Datos obtenidos exitosamente' });
            } else {
                reject(new Error('Error al obtener datos'));
            }
        }, 1000);
    });
};

// Uso con .then() y .catch()
fetchData()
    .then(result => {
        console.log('Éxito:', result.data);
    })
    .catch(error => {
        console.error('Error:', error.message);
    });
```

#### **2. Promise.all() y Promise.race()**
```javascript
// Promise.all() - Todas las promesas deben resolverse
const fetchUser = (id) => Promise.resolve({ id, name: `Usuario ${id}` });
const fetchPosts = (userId) => Promise.resolve([`Post 1 de ${userId}`, `Post 2 de ${userId}`]);

Promise.all([
    fetchUser(1),
    fetchPosts(1)
])
.then(([user, posts]) => {
    console.log('Usuario:', user);
    console.log('Posts:', posts);
})
.catch(error => {
    console.error('Error:', error);
});

// Promise.race() - La primera promesa que se resuelva
const fastAPI = new Promise(resolve => setTimeout(() => resolve('API Rápida'), 1000));
const slowAPI = new Promise(resolve => setTimeout(() => resolve('API Lenta'), 3000));

Promise.race([fastAPI, slowAPI])
    .then(result => {
        console.log('Ganador:', result); // "API Rápida"
    });
```

### **Async/Await**

#### **1. Sintaxis Básica**
```javascript
// Función async básica
async function getUserData(userId) {
    try {
        const user = await fetchUser(userId);
        const posts = await fetchPosts(userId);
        
        return {
            user,
            posts,
            totalPosts: posts.length
        };
    } catch (error) {
        console.error('Error al obtener datos del usuario:', error);
        throw error;
    }
}

// Uso de la función async
getUserData(1)
    .then(data => {
        console.log('Datos completos:', data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

#### **2. Async/Await con Múltiples Promesas**
```javascript
async function fetchAllData() {
    try {
        // Ejecutar promesas en paralelo
        const [users, posts, comments] = await Promise.all([
            fetchUsers(),
            fetchPosts(),
            fetchComments()
        ]);
        
        return {
            users: users.length,
            posts: posts.length,
            comments: comments.length
        };
    } catch (error) {
        console.error('Error al obtener todos los datos:', error);
        throw error;
    }
}

// Función async con IIFE (Immediately Invoked Function Expression)
(async () => {
    try {
        const stats = await fetchAllData();
        console.log('Estadísticas:', stats);
    } catch (error) {
        console.error('Error:', error);
    }
})();
```

---

## 🎯 **MANIPULACIÓN DEL DOM**

### **Selección de Elementos**

#### **1. Métodos de Selección Modernos**
```javascript
// querySelector - Primer elemento que coincida
const firstButton = document.querySelector('.btn');
const firstInput = document.querySelector('input[type="text"]');

// querySelectorAll - Todos los elementos que coincidan
const allButtons = document.querySelectorAll('.btn');
const allInputs = document.querySelectorAll('input');

// getElementById (más rápido para IDs únicos)
const header = document.getElementById('header');

// getElementsByClassName (HTMLCollection)
const cards = document.getElementsByClassName('card');
```

#### **2. Navegación del DOM**
```javascript
// Navegación hacia arriba
const parent = element.parentElement;
const parentNode = element.parentNode;

// Navegación hacia abajo
const children = element.children; // HTMLCollection
const childNodes = element.childNodes; // NodeList

// Navegación horizontal
const nextSibling = element.nextElementSibling;
const previousSibling = element.previousElementSibling;

// Navegación específica
const firstChild = element.firstElementChild;
const lastChild = element.lastElementChild;
```

### **Manipulación de Elementos**

#### **1. Creación y Modificación**
```javascript
// Crear elementos
const newDiv = document.createElement('div');
const newParagraph = document.createElement('p');

// Modificar propiedades
newDiv.className = 'card';
newDiv.id = 'card-1';
newDiv.setAttribute('data-id', '1');

// Modificar contenido
newDiv.textContent = 'Contenido de texto';
newDiv.innerHTML = '<strong>Texto</strong> con <em>HTML</em>';

// Añadir elementos al DOM
document.body.appendChild(newDiv);
document.querySelector('.container').prepend(newParagraph);

// Insertar elementos en posiciones específicas
const referenceElement = document.querySelector('.reference');
referenceElement.insertAdjacentElement('beforebegin', newDiv);
referenceElement.insertAdjacentHTML('afterend', '<p>Nuevo párrafo</p>');
```

#### **2. Manipulación de Clases y Estilos**
```javascript
const element = document.querySelector('.my-element');

// Manipulación de clases
element.classList.add('active', 'highlighted');
element.classList.remove('inactive');
element.classList.toggle('visible');
element.classList.contains('active'); // true/false

// Manipulación de estilos
element.style.backgroundColor = '#ff0000';
element.style.fontSize = '16px';
element.style.cssText = 'color: blue; font-weight: bold;';

// Obtener estilos computados
const computedStyle = window.getComputedStyle(element);
const backgroundColor = computedStyle.backgroundColor;
```

### **Event Handling**

#### **1. Event Listeners**
```javascript
// Event listener básico
const button = document.querySelector('.btn');
button.addEventListener('click', function(event) {
    console.log('Botón clickeado!', event);
});

// Event listener con arrow function
button.addEventListener('mouseenter', (event) => {
    event.target.style.backgroundColor = '#ff0000';
});

// Event listener con opciones
button.addEventListener('click', handleClick, {
    once: true, // Solo se ejecuta una vez
    capture: false, // Fase de captura
    passive: true // No previene comportamiento por defecto
});

// Remover event listener
button.removeEventListener('click', handleClick);
```

#### **2. Event Delegation**
```javascript
// Event delegation para elementos dinámicos
document.addEventListener('click', (event) => {
    if (event.target.matches('.delete-btn')) {
        const itemId = event.target.dataset.id;
        deleteItem(itemId);
    }
    
    if (event.target.matches('.edit-btn')) {
        const itemId = event.target.dataset.id;
        editItem(itemId);
    }
});

// Event delegation con múltiples selectores
document.addEventListener('click', (event) => {
    const target = event.target;
    
    if (target.closest('.card')) {
        const card = target.closest('.card');
        const cardId = card.dataset.id;
        
        if (target.matches('.card-title')) {
            editCardTitle(cardId);
        } else if (target.matches('.card-delete')) {
            deleteCard(cardId);
        }
    }
});
```

---

## 🎨 **EJEMPLOS PRÁCTICOS**

### **1. Sistema de Tareas (Todo List)**
```javascript
class TodoList {
    constructor() {
        this.tasks = [];
        this.container = document.querySelector('.todo-container');
        this.init();
    }
    
    init() {
        this.render();
        this.bindEvents();
    }
    
    addTask(text) {
        const task = {
            id: Date.now(),
            text,
            completed: false,
            createdAt: new Date()
        };
        
        this.tasks.push(task);
        this.render();
        this.saveToLocalStorage();
    }
    
    toggleTask(id) {
        const task = this.tasks.find(t => t.id === id);
        if (task) {
            task.completed = !task.completed;
            this.render();
            this.saveToLocalStorage();
        }
    }
    
    deleteTask(id) {
        this.tasks = this.tasks.filter(t => t.id !== id);
        this.render();
        this.saveToLocalStorage();
    }
    
    render() {
        this.container.innerHTML = this.tasks.map(task => `
            <div class="task ${task.completed ? 'completed' : ''}" data-id="${task.id}">
                <input type="checkbox" ${task.completed ? 'checked' : ''}>
                <span class="task-text">${task.text}</span>
                <button class="delete-btn">×</button>
            </div>
        `).join('');
    }
    
    bindEvents() {
        this.container.addEventListener('click', (event) => {
            const target = event.target;
            const taskElement = target.closest('.task');
            
            if (!taskElement) return;
            
            const taskId = parseInt(taskElement.dataset.id);
            
            if (target.matches('input[type="checkbox"]')) {
                this.toggleTask(taskId);
            } else if (target.matches('.delete-btn')) {
                this.deleteTask(taskId);
            }
        });
    }
    
    saveToLocalStorage() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
    
    loadFromLocalStorage() {
        const saved = localStorage.getItem('tasks');
        if (saved) {
            this.tasks = JSON.parse(saved);
            this.render();
        }
    }
}

// Inicializar la aplicación
const todoList = new TodoList();
todoList.loadFromLocalStorage();
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Organización del Código**
- **Usa módulos ES6** para organizar el código
- **Implementa clases** para funcionalidades relacionadas
- **Separa responsabilidades** en diferentes módulos
- **Usa constantes** para valores mágicos

### **2. Performance**
- **Debounce/throttle** para eventos frecuentes
- **Event delegation** para elementos dinámicos
- **Lazy loading** para recursos pesados
- **Virtual scrolling** para listas largas

### **3. Mantenibilidad**
- **Escribe código legible** y bien documentado
- **Usa nombres descriptivos** para variables y funciones
- **Implementa manejo de errores** robusto
- **Escribe tests** para funcionalidades críticas

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- **MDN JavaScript**: [JavaScript Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- **ES6 Features**: [ES6 Features](https://github.com/lukehoban/es6features)
- **TC39**: [ECMAScript Proposals](https://github.com/tc39/proposals)

### **Herramientas de Desarrollo**
- **Babel**: [babeljs.io](https://babeljs.io/)
- **ESLint**: [eslint.org](https://eslint.org/)
- **Prettier**: [prettier.io](https://prettier.io/)

---

*JavaScript ES6+ moderno te permite escribir código más limpio, mantenible y potente. Dominar estas características es esencial para el desarrollo frontend profesional.* 