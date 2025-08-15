# ⚡ Node.js Fundamentos: Runtime de JavaScript

## 🎯 **OBJETIVO**
Comprender los fundamentos de Node.js, su arquitectura, el event loop, módulos, y cómo funciona como runtime de JavaScript en el servidor.

---

## 📋 **¿QUÉ ES NODE.JS?**

### **Definición**
Node.js es un runtime de JavaScript construido sobre el motor V8 de Chrome que permite ejecutar código JavaScript en el servidor. No es un lenguaje de programación, sino un entorno de ejecución.

### **Características Principales**
- **Single-threaded**: Un solo hilo principal (event loop)
- **Event-driven**: Basado en eventos y callbacks
- **Non-blocking I/O**: Operaciones de entrada/salida no bloqueantes
- **Cross-platform**: Funciona en Windows, macOS y Linux
- **Open Source**: Código abierto y mantenido por la comunidad

---

## 🏗️ **ARQUITECTURA DE NODE.JS**

### **Componentes Principales**
```
┌─────────────────────────────────────────┐
│                Aplicación               │
├─────────────────────────────────────────┤
│              Node.js Core              │
│  ┌─────────────┐ ┌──────────────────┐  │
│  │   V8 Engine │ │  Libuv (Event    │  │
│  │             │ │     Loop)        │  │
│  └─────────────┘ └──────────────────┘  │
├─────────────────────────────────────────┤
│              Sistema Operativo          │
└─────────────────────────────────────────┘
```

### **V8 Engine**
- **Motor de JavaScript** de Google Chrome
- **Compilación JIT** (Just-In-Time)
- **Optimizaciones automáticas** de código
- **Garbage collection** automático

### **Libuv**
- **Event loop** y manejo de operaciones asíncronas
- **Thread pool** para operaciones bloqueantes
- **I/O no bloqueante** para archivos y red
- **Cross-platform** abstractions

---

## 🔄 **EVENT LOOP**

### **¿Qué es el Event Loop?**
El event loop es el mecanismo que permite a Node.js realizar operaciones no bloqueantes, a pesar de ser single-threaded, mediante el uso de callbacks y la delegación de operaciones al thread pool.

### **Fases del Event Loop**
```
   ┌─────────────────────────────┐
   │           timers            │ ← setTimeout, setInterval
   ├─────────────────────────────┤
   │     pending callbacks       │ ← I/O callbacks diferidos
   ├─────────────────────────────┤
   │           idle              │ ← Interno de Node.js
   ├─────────────────────────────┤
   │           poll              │ ← Nuevos I/O events
   ├─────────────────────────────┤
   │           check             │ ← setImmediate
   ├─────────────────────────────┤
   │      close callbacks        │ ← close events
   └─────────────────────────────┘
```

### **Ejemplo de Event Loop**
```javascript
console.log('1. Inicio');

setTimeout(() => {
  console.log('2. Timeout 0ms');
}, 0);

setImmediate(() => {
  console.log('3. setImmediate');
});

process.nextTick(() => {
  console.log('4. nextTick');
});

console.log('5. Fin');

// Salida:
// 1. Inicio
// 5. Fin
// 4. nextTick
// 2. Timeout 0ms
// 3. setImmediate
```

---

## 📦 **SISTEMA DE MÓDULOS**

### **CommonJS (require/module.exports)**
```javascript
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

module.exports = {
  add,
  subtract
};

// app.js
const math = require('./math');
console.log(math.add(5, 3));      // 8
console.log(math.subtract(5, 3)); // 2
```

### **ES Modules (import/export)**
```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

export default {
  add,
  subtract
};

// app.js
import math, { add, subtract } from './math.js';
console.log(add(5, 3));      // 8
console.log(subtract(5, 3)); // 2
console.log(math.add(10, 5)); // 15
```

### **Módulos Nativos de Node.js**
```javascript
// Módulos principales
const fs = require('fs');           // Sistema de archivos
const path = require('path');       // Manejo de rutas
const http = require('http');       // Servidor HTTP
const crypto = require('crypto');   // Criptografía
const events = require('events');   // Sistema de eventos
const util = require('util');       // Utilidades
const os = require('os');           // Información del sistema
const url = require('url');         // Parsing de URLs
```

---

## 🌊 **STREAMS Y BUFFERS**

### **Buffers**
Los buffers son objetos que representan datos binarios de tamaño fijo.

```javascript
const buffer = Buffer.from('Hello World', 'utf8');
console.log(buffer);                    // <Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64>
console.log(buffer.toString());         // Hello World
console.log(buffer.length);             // 11
console.log(buffer[0]);                 // 72 (código ASCII de 'H')

// Crear buffer vacío
const emptyBuffer = Buffer.alloc(10);
const unsafeBuffer = Buffer.allocUnsafe(10);
```

### **Streams**
Los streams son objetos que permiten leer o escribir datos de manera continua.

#### **Tipos de Streams**
```javascript
const fs = require('fs');

// Readable Stream (Lectura)
const readStream = fs.createReadStream('input.txt');

// Writable Stream (Escritura)
const writeStream = fs.createWriteStream('output.txt');

// Transform Stream (Transformación)
const { Transform } = require('stream');
const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

// Pipe streams
readStream
  .pipe(upperCaseTransform)
  .pipe(writeStream);
```

#### **Ejemplo Práctico de Streams**
```javascript
const fs = require('fs');
const http = require('http');

const server = http.createServer((req, res) => {
  // Stream de lectura para archivos grandes
  const fileStream = fs.createReadStream('./large-file.txt');
  
  // Configurar headers
  res.setHeader('Content-Type', 'text/plain');
  
  // Pipe del archivo a la respuesta HTTP
  fileStream.pipe(res);
  
  // Manejo de errores
  fileStream.on('error', (error) => {
    res.statusCode = 500;
    res.end('Error reading file');
  });
});

server.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

---

## 🎭 **SISTEMA DE EVENTOS**

### **EventEmitter**
Node.js implementa el patrón Observer a través de la clase EventEmitter.

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

// Escuchar eventos
myEmitter.on('event', (arg1, arg2) => {
  console.log('Evento recibido:', arg1, arg2);
});

// Emitir eventos
myEmitter.emit('event', 'arg1', 'arg2');

// Eventos una sola vez
myEmitter.once('once', () => {
  console.log('Este evento solo se ejecuta una vez');
});

// Remover listeners
const listener = () => console.log('Listener');
myEmitter.on('test', listener);
myEmitter.removeListener('test', listener);
```

### **Ejemplo Práctico: Chat Server**
```javascript
const EventEmitter = require('events');
const net = require('net');

class ChatServer extends EventEmitter {
  constructor() {
    super();
    this.clients = new Set();
  }
  
  addClient(client) {
    this.clients.add(client);
    this.emit('clientJoined', client);
    
    client.on('data', (data) => {
      this.emit('message', client, data.toString());
    });
    
    client.on('end', () => {
      this.clients.delete(client);
      this.emit('clientLeft', client);
    });
  }
  
  broadcast(message, sender) {
    this.clients.forEach(client => {
      if (client !== sender) {
        client.write(message);
      }
    });
  }
}

const chatServer = new ChatServer();

chatServer.on('clientJoined', (client) => {
  console.log('Nuevo cliente conectado');
  chatServer.broadcast('Nuevo usuario se unió al chat\n', client);
});

chatServer.on('message', (client, message) => {
  console.log('Mensaje recibido:', message.trim());
  chatServer.broadcast(`Usuario: ${message}`, client);
});

chatServer.on('clientLeft', (client) => {
  console.log('Cliente desconectado');
  chatServer.broadcast('Un usuario dejó el chat\n', client);
});
```

---

## 🔧 **MÓDULOS Y PAQUETES NPM**

### **package.json**
```json
{
  "name": "mi-aplicacion",
  "version": "1.0.0",
  "description": "Descripción de la aplicación",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "build": "webpack"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0"
  },
  "devDependencies": {
    "nodemon": "^2.0.22",
    "jest": "^29.0.0"
  },
  "keywords": ["node", "express", "api"],
  "author": "Tu Nombre",
  "license": "MIT"
}
```

### **Comandos NPM Principales**
```bash
# Inicializar proyecto
npm init
npm init -y

# Instalar dependencias
npm install express
npm install --save-dev nodemon

# Instalar globalmente
npm install -g nodemon

# Ejecutar scripts
npm start
npm run dev
npm test

# Actualizar paquetes
npm update
npm outdated

# Limpiar cache
npm cache clean --force
```

### **Gestión de Dependencias**
```javascript
// Instalación específica
npm install express@4.18.2

// Instalación de desarrollo
npm install --save-dev jest nodemon

// Instalación opcional
npm install --save-optional debug

// Instalación peer
npm install --save-peer react
```

---

## 🚀 **PROCESOS Y CLUSTERS**

### **Proceso Principal**
```javascript
console.log('PID del proceso:', process.pid);
console.log('Versión de Node:', process.version);
console.log('Plataforma:', process.platform);
console.log('Directorio de trabajo:', process.cwd());

// Variables de entorno
console.log('NODE_ENV:', process.env.NODE_ENV);

// Argumentos de línea de comandos
console.log('Argumentos:', process.argv);

// Señales del sistema
process.on('SIGINT', () => {
  console.log('Recibida señal SIGINT');
  process.exit(0);
});

process.on('SIGTERM', () => {
  console.log('Recibida señal SIGTERM');
  process.exit(0);
});
```

### **Clusters para Multi-Core**
```javascript
const cluster = require('cluster');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`Proceso maestro ${process.pid} está ejecutándose`);
  
  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
  
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} murió`);
    // Reiniciar worker
    cluster.fork();
  });
  
} else {
  // Workers pueden compartir conexiones TCP
  require('./server.js');
  console.log(`Worker ${process.pid} iniciado`);
}
```

---

## 🧪 **TESTING Y DEBUGGING**

### **Debugging con Node.js**
```javascript
// Debugger nativo
debugger;

// Console methods
console.log('Log normal');
console.info('Información');
console.warn('Advertencia');
console.error('Error');
console.table([{ name: 'John', age: 30 }]);

// Timing
console.time('operacion');
// ... código ...
console.timeEnd('operacion');

// Trace
console.trace('Stack trace');
```

### **Testing Básico**
```javascript
// test.js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

// Tests simples
console.log('Testing add function...');
console.assert(add(2, 3) === 5, 'add(2, 3) should equal 5');
console.assert(add(-1, 1) === 0, 'add(-1, 1) should equal 0');

console.log('Testing subtract function...');
console.assert(subtract(5, 3) === 2, 'subtract(5, 3) should equal 2');
console.assert(subtract(1, 1) === 0, 'subtract(1, 1) should equal 0');

console.log('All tests passed!');
```

---

## 📊 **MONITOREO Y PERFORMANCE**

### **Process Monitoring**
```javascript
const startUsage = process.cpuUsage();
const startMemory = process.memoryUsage();

// Simular trabajo
setTimeout(() => {
  const endUsage = process.cpuUsage(startUsage);
  const endMemory = process.memoryUsage();
  
  console.log('CPU Usage:', endUsage);
  console.log('Memory Usage:', endMemory);
  
  console.log('RSS:', endMemory.rss / 1024 / 1024, 'MB');
  console.log('Heap Used:', endMemory.heapUsed / 1024 / 1024, 'MB');
  console.log('Heap Total:', endMemory.heapTotal / 1024 / 1024, 'MB');
}, 1000);
```

### **Performance Hooks**
```javascript
const { performance, PerformanceObserver } = require('perf_hooks');

const obs = new PerformanceObserver((list) => {
  const entries = list.getEntries();
  entries.forEach((entry) => {
    console.log(`${entry.name}: ${entry.duration}ms`);
  });
});

obs.observe({ entryTypes: ['measure'] });

performance.mark('start');
// ... código a medir ...
performance.mark('end');
performance.measure('operacion', 'start', 'end');
```

---

## 🔒 **SEGURIDAD BÁSICA**

### **Variables de Entorno**
```javascript
require('dotenv').config();

const config = {
  port: process.env.PORT || 3000,
  database: process.env.DATABASE_URL,
  jwtSecret: process.env.JWT_SECRET,
  nodeEnv: process.env.NODE_ENV || 'development'
};

// Validar variables críticas
if (!config.jwtSecret) {
  throw new Error('JWT_SECRET is required');
}
```

### **Validación de Inputs**
```javascript
const validateEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

const validatePassword = (password) => {
  // Mínimo 8 caracteres, al menos una letra y un número
  const passwordRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/;
  return passwordRegex.test(password);
};

// Uso
if (!validateEmail(userEmail)) {
  throw new Error('Invalid email format');
}

if (!validatePassword(userPassword)) {
  throw new Error('Password must be at least 8 characters with letters and numbers');
}
```

---

## 💡 **MEJORES PRÁCTICAS**

### **Estructura de Proyecto**
```
proyecto/
├── src/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── utils/
│   └── app.js
├── tests/
├── docs/
├── package.json
├── .env
├── .gitignore
└── README.md
```

### **Manejo de Errores**
```javascript
process.on('uncaughtException', (error) => {
  console.error('Uncaught Exception:', error);
  process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection at:', promise, 'reason:', reason);
  process.exit(1);
});

// Async error handling
async function handleAsyncOperation() {
  try {
    const result = await someAsyncOperation();
    return result;
  } catch (error) {
    console.error('Error in async operation:', error);
    throw error;
  }
}
```

### **Logging**
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

// Uso
logger.info('Application started');
logger.error('An error occurred', { error: 'details' });
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Módulo de Utilidades**
Crea un módulo de utilidades con funciones matemáticas y de string.

### **Ejercicio 2: Event Emitter Personalizado**
Implementa un sistema de notificaciones usando EventEmitter.

### **Ejercicio 3: Stream de Transformación**
Crea un stream que transforme texto a mayúsculas.

### **Ejercicio 4: Servidor HTTP Básico**
Implementa un servidor HTTP que sirva archivos estáticos.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [Node.js Documentation](https://nodejs.org/docs/)
- [Node.js API Reference](https://nodejs.org/api/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

### **Libros Recomendados**
- "Node.js in Action" - Mike Cantelon
- "Node.js Design Patterns" - Mario Casciaro
- "Node.js the Right Way" - Jim Wilson

### **Cursos Online**
- Node.js: The Complete Guide (Udemy)
- Node.js Crash Course (YouTube)
- Node.js Fundamentals (Pluralsight)

---

*Node.js es la base fundamental para el desarrollo backend moderno con JavaScript. Dominar estos conceptos te permitirá crear aplicaciones robustas y escalables.* 