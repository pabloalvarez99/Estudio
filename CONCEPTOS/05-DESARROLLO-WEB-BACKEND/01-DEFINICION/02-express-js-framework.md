# 🚀 Express.js Framework: Framework Web para Node.js

## 🎯 **OBJETIVO**
Dominar Express.js, el framework web más popular para Node.js, incluyendo routing, middleware, manejo de errores y mejores prácticas para crear APIs robustas.

---

## 📋 **¿QUÉ ES EXPRESS.JS?**

### **Definición**
Express.js es un framework web minimalista y flexible para Node.js que simplifica el desarrollo de aplicaciones web y APIs. Proporciona una capa de abstracción sobre el módulo HTTP nativo de Node.js.

### **Características Principales**
- **Minimalista**: Framework ligero y no opinativo
- **Flexible**: Permite múltiples patrones de arquitectura
- **Middleware**: Sistema extensible de funciones intermedias
- **Routing**: Manejo elegante de rutas HTTP
- **Template Engines**: Soporte para múltiples motores de plantillas
- **Static Files**: Servir archivos estáticos fácilmente

---

## 🏗️ **ARQUITECTURA DE EXPRESS.JS**

### **Flujo de Request/Response**
```
Cliente → Request → Express App → Middleware → Route Handler → Response → Cliente
                ↓
            Error Handler (si hay error)
```

### **Componentes Principales**
```javascript
const express = require('express');
const app = express();

// 1. Middleware de aplicación
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// 2. Middleware de routing
app.use('/api', apiRoutes);
app.use('/auth', authRoutes);

// 3. Route handlers
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// 4. Error handling middleware
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

---

## 🛣️ **ROUTING EN EXPRESS.JS**

### **Métodos HTTP Básicos**
```javascript
const express = require('express');
const app = express();

// GET - Obtener recursos
app.get('/users', (req, res) => {
  res.json({ message: 'Get all users' });
});

// POST - Crear recursos
app.post('/users', (req, res) => {
  res.json({ message: 'Create user' });
});

// PUT - Actualizar recursos completos
app.put('/users/:id', (req, res) => {
  res.json({ message: `Update user ${req.params.id}` });
});

// PATCH - Actualizar recursos parcialmente
app.patch('/users/:id', (req, res) => {
  res.json({ message: `Partially update user ${req.params.id}` });
});

// DELETE - Eliminar recursos
app.delete('/users/:id', (req, res) => {
  res.json({ message: `Delete user ${req.params.id}` });
});

// ALL - Capturar todos los métodos
app.all('/info', (req, res) => {
  res.json({ 
    method: req.method,
    url: req.url,
    timestamp: new Date()
  });
});
```

### **Parámetros de Ruta**
```javascript
// Parámetros simples
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ userId, message: 'Get user by ID' });
});

// Múltiples parámetros
app.get('/users/:id/posts/:postId', (req, res) => {
  const { id, postId } = req.params;
  res.json({ userId: id, postId, message: 'Get specific post' });
});

// Parámetros opcionales
app.get('/users/:id?', (req, res) => {
  if (req.params.id) {
    res.json({ userId: req.params.id, message: 'Get specific user' });
  } else {
    res.json({ message: 'Get all users' });
  }
});

// Parámetros con regex
app.get('/users/:id([0-9]+)', (req, res) => {
  res.json({ userId: req.params.id, message: 'Get user with numeric ID' });
});
```

### **Query Strings**
```javascript
app.get('/search', (req, res) => {
  const { q, page = 1, limit = 10 } = req.query;
  
  res.json({
    query: q,
    page: parseInt(page),
    limit: parseInt(limit),
    message: 'Search results'
  });
});

// URL: /search?q=javascript&page=2&limit=20
```

### **Router Modular**
```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

// GET /users
router.get('/', (req, res) => {
  res.json({ message: 'Get all users' });
});

// GET /users/:id
router.get('/:id', (req, res) => {
  res.json({ userId: req.params.id, message: 'Get user by ID' });
});

// POST /users
router.post('/', (req, res) => {
  res.json({ message: 'Create user', data: req.body });
});

// PUT /users/:id
router.put('/:id', (req, res) => {
  res.json({ 
    userId: req.params.id, 
    message: 'Update user',
    data: req.body 
  });
});

// DELETE /users/:id
router.delete('/:id', (req, res) => {
  res.json({ userId: req.params.id, message: 'Delete user' });
});

module.exports = router;

// app.js
const usersRouter = require('./routes/users');
app.use('/users', usersRouter);
```

---

## 🔌 **MIDDLEWARE EN EXPRESS.JS**

### **¿Qué es el Middleware?**
Middleware son funciones que tienen acceso al objeto request (req), al objeto response (res) y a la siguiente función middleware en el ciclo request-response. Pueden ejecutar código, modificar request/response, terminar el ciclo o llamar al siguiente middleware.

### **Tipos de Middleware**

#### **1. Middleware de Aplicación**
```javascript
const express = require('express');
const app = express();

// Middleware que se ejecuta en todas las rutas
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date()}`);
  next();
});

// Middleware para parsing de JSON
app.use(express.json());

// Middleware para parsing de formularios
app.use(express.urlencoded({ extended: true }));

// Middleware para archivos estáticos
app.use(express.static('public'));

// Middleware con path específico
app.use('/api', (req, res, next) => {
  req.apiVersion = 'v1';
  next();
});
```

#### **2. Middleware de Router**
```javascript
const router = express.Router();

router.use((req, res, next) => {
  console.log('Router middleware');
  next();
});

router.get('/', (req, res) => {
  res.send('Router home');
});
```

#### **3. Middleware de Error**
```javascript
// Debe tener 4 parámetros (err, req, res, next)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ 
    error: 'Something went wrong!',
    message: err.message 
  });
});
```

### **Middleware Personalizado**
```javascript
// Logger middleware
const logger = (req, res, next) => {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] ${req.method} ${req.url}`);
  next();
};

// Authentication middleware
const authenticate = (req, res, next) => {
  const token = req.headers.authorization;
  
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  
  try {
    // Verificar token (ejemplo simplificado)
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid token' });
  }
};

// Rate limiting middleware
const rateLimit = (limit, windowMs) => {
  const requests = new Map();
  
  return (req, res, next) => {
    const ip = req.ip;
    const now = Date.now();
    
    if (!requests.has(ip)) {
      requests.set(ip, []);
    }
    
    const userRequests = requests.get(ip);
    const validRequests = userRequests.filter(time => now - time < windowMs);
    
    if (validRequests.length >= limit) {
      return res.status(429).json({ error: 'Too many requests' });
    }
    
    validRequests.push(now);
    requests.set(ip, validRequests);
    next();
  };
};

// Uso
app.use(logger);
app.use('/api', authenticate);
app.use('/api', rateLimit(100, 60000)); // 100 requests per minute
```

### **Middleware de Terceros**
```javascript
const helmet = require('helmet');
const cors = require('cors');
const morgan = require('morgan');
const compression = require('compression');

// Seguridad
app.use(helmet());

// CORS
app.use(cors({
  origin: ['http://localhost:3000', 'https://myapp.com'],
  credentials: true
}));

// Logging
app.use(morgan('combined'));

// Compresión
app.use(compression());
```

---

## 📝 **MANEJO DE REQUEST Y RESPONSE**

### **Request Object (req)**
```javascript
app.get('/example/:id', (req, res) => {
  // Parámetros de ruta
  console.log('Params:', req.params);
  
  // Query strings
  console.log('Query:', req.query);
  
  // Headers
  console.log('Headers:', req.headers);
  
  // Body (para POST, PUT, PATCH)
  console.log('Body:', req.body);
  
  // Método HTTP
  console.log('Method:', req.method);
  
  // URL
  console.log('URL:', req.url);
  
  // IP del cliente
  console.log('IP:', req.ip);
  
  // Cookies
  console.log('Cookies:', req.cookies);
  
  // Archivos (si se usa multer)
  console.log('Files:', req.files);
});
```

### **Response Object (res)**
```javascript
app.get('/response-example', (req, res) => {
  // JSON response
  res.json({ message: 'JSON response' });
  
  // HTML response
  res.send('<h1>HTML response</h1>');
  
  // Status code + JSON
  res.status(201).json({ message: 'Created' });
  
  // Redirect
  res.redirect('/new-url');
  
  // Download file
  res.download('/path/to/file.pdf');
  
  // Send file
  res.sendFile('/path/to/file.html');
  
  // Set headers
  res.set('Content-Type', 'text/plain');
  res.set({
    'X-Custom-Header': 'value',
    'Cache-Control': 'no-cache'
  });
  
  // Set cookies
  res.cookie('name', 'value', { 
    maxAge: 900000, 
    httpOnly: true 
  });
  
  // Clear cookies
  res.clearCookie('name');
});
```

---

## 🚨 **MANEJO DE ERRORES**

### **Error Handling Básico**
```javascript
// Middleware de manejo de errores
app.use((err, req, res, next) => {
  console.error(err.stack);
  
  // Error personalizado
  if (err.name === 'ValidationError') {
    return res.status(400).json({
      error: 'Validation Error',
      details: err.message
    });
  }
  
  // Error de autenticación
  if (err.name === 'UnauthorizedError') {
    return res.status(401).json({
      error: 'Unauthorized',
      message: 'Invalid token'
    });
  }
  
  // Error por defecto
  res.status(err.status || 500).json({
    error: err.message || 'Internal Server Error'
  });
});
```

### **Async Error Handling**
```javascript
// Wrapper para async functions
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Uso en routes
app.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    throw new Error('User not found');
  }
  res.json(user);
}));

// O usar try-catch
app.post('/users', async (req, res, next) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (error) {
    next(error);
  }
});
```

### **Custom Error Classes**
```javascript
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
    
    Error.captureStackTrace(this, this.constructor);
  }
}

class ValidationError extends AppError {
  constructor(message) {
    super(message, 400);
    this.name = 'ValidationError';
  }
}

class NotFoundError extends AppError {
  constructor(resource) {
    super(`${resource} not found`, 404);
    this.name = 'NotFoundError';
  }
}

// Uso
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      throw new NotFoundError('User');
    }
    res.json(user);
  } catch (error) {
    next(error);
  }
});
```

---

## 🔒 **SEGURIDAD EN EXPRESS.JS**

### **Helmet.js para Headers de Seguridad**
```javascript
const helmet = require('helmet');

app.use(helmet());
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    styleSrc: ["'self'", "'unsafe-inline'"],
    scriptSrc: ["'self'"],
    imgSrc: ["'self'", "data:", "https:"],
  },
}));
```

### **CORS Configuration**
```javascript
const cors = require('cors');

const corsOptions = {
  origin: function (origin, callback) {
    const allowedOrigins = [
      'http://localhost:3000',
      'https://myapp.com'
    ];
    
    if (!origin || allowedOrigins.indexOf(origin) !== -1) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization']
};

app.use(cors(corsOptions));
```

### **Rate Limiting**
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100, // máximo 100 requests por ventana
  message: {
    error: 'Too many requests from this IP, please try again later.'
  },
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', limiter);
```

### **Input Validation**
```javascript
const { body, validationResult } = require('express-validator');

const validateUser = [
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 6 }),
  body('name').trim().isLength({ min: 2 }),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ 
        errors: errors.array() 
      });
    }
    next();
  }
];

app.post('/users', validateUser, (req, res) => {
  // Usuario validado
  res.json({ message: 'User created successfully' });
});
```

---

## 🧪 **TESTING DE APLICACIONES EXPRESS**

### **Setup de Testing**
```javascript
// test/app.test.js
const request = require('supertest');
const app = require('../app');
const mongoose = require('mongoose');

describe('User API', () => {
  beforeAll(async () => {
    await mongoose.connect(process.env.MONGODB_TEST_URI);
  });
  
  afterAll(async () => {
    await mongoose.connection.close();
  });
  
  beforeEach(async () => {
    await User.deleteMany({});
  });
  
  describe('GET /users', () => {
    it('should return empty array when no users exist', async () => {
      const response = await request(app)
        .get('/users')
        .expect(200);
      
      expect(response.body).toEqual([]);
    });
  });
  
  describe('POST /users', () => {
    it('should create a new user', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };
      
      const response = await request(app)
        .post('/users')
        .send(userData)
        .expect(201);
      
      expect(response.body.name).toBe(userData.name);
      expect(response.body.email).toBe(userData.email);
    });
  });
});
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **Estructura de Proyecto**
```
project/
├── src/
│   ├── controllers/
│   │   ├── userController.js
│   │   └── authController.js
│   ├── models/
│   │   ├── User.js
│   │   └── Post.js
│   ├── routes/
│   │   ├── userRoutes.js
│   │   └── authRoutes.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── validation.js
│   ├── utils/
│   │   ├── database.js
│   │   └── helpers.js
│   ├── config/
│   │   ├── database.js
│   │   └── environment.js
│   └── app.js
├── tests/
├── docs/
├── package.json
└── README.md
```

### **Separación de Responsabilidades**
```javascript
// controllers/userController.js
const User = require('../models/User');

exports.getAllUsers = async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    next(error);
  }
};

exports.createUser = async (req, res, next) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (error) {
    next(error);
  }
};

// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const { authenticate } = require('../middleware/auth');

router.get('/', authenticate, userController.getAllUsers);
router.post('/', userController.createUser);

module.exports = router;

// app.js
const userRoutes = require('./routes/userRoutes');
app.use('/api/users', userRoutes);
```

### **Environment Configuration**
```javascript
// config/environment.js
require('dotenv').config();

const config = {
  port: process.env.PORT || 3000,
  nodeEnv: process.env.NODE_ENV || 'development',
  database: {
    uri: process.env.MONGODB_URI,
    options: {
      useNewUrlParser: true,
      useUnifiedTopology: true
    }
  },
  jwt: {
    secret: process.env.JWT_SECRET,
    expiresIn: process.env.JWT_EXPIRES_IN || '1d'
  }
};

module.exports = config;
```

---

## 📊 **MONITOREO Y LOGGING**

### **Morgan para Logging**
```javascript
const morgan = require('morgan');

// Logging personalizado
morgan.token('body', (req) => JSON.stringify(req.body));
morgan.token('user', (req) => req.user ? req.user.id : 'anonymous');

app.use(morgan(':method :url :status :response-time ms - :user - :body'));
```

### **Winston para Logging Avanzado**
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

// Middleware de logging
app.use((req, res, next) => {
  logger.info(`${req.method} ${req.url}`, {
    ip: req.ip,
    userAgent: req.get('User-Agent'),
    timestamp: new Date().toISOString()
  });
  next();
});
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: API REST Básica**
Crea una API REST para gestionar una lista de tareas (CRUD completo).

### **Ejercicio 2: Middleware Personalizado**
Implementa middleware para logging, autenticación y validación.

### **Ejercicio 3: Manejo de Errores**
Crea un sistema robusto de manejo de errores con clases personalizadas.

### **Ejercicio 4: Testing Completo**
Escribe tests unitarios e integración para tu API.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [Express.js Documentation](https://expressjs.com/)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [Express.js API Reference](https://expressjs.com/en/4x/api.html)

### **Middleware Recomendados**
- [helmet](https://helmetjs.github.io/) - Seguridad
- [cors](https://github.com/expressjs/cors) - CORS
- [morgan](https://github.com/expressjs/morgan) - Logging
- [compression](https://github.com/expressjs/compression) - Compresión
- [express-validator](https://express-validator.github.io/) - Validación

### **Libros y Cursos**
- "Express in Action" - Evan Hahn
- "Node.js in Action" - Mike Cantelon
- Express.js Crash Course (YouTube)

---

*Express.js es el framework web más popular para Node.js. Dominar Express te permitirá crear APIs robustas y aplicaciones web escalables de manera eficiente.* 