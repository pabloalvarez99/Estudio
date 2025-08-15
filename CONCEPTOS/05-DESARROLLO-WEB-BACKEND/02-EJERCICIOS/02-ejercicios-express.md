# 🚀 Ejercicios Prácticos: Express.js Framework

## 🎯 **OBJETIVO**
Aplicar los conceptos de Express.js mediante ejercicios prácticos que desarrollen habilidades de creación de APIs web robustas y escalables.

---

## 📋 **EJERCICIO 1: MIDDLEWARE PERSONALIZADO**

### **Descripción**
Crear un sistema de middleware personalizado para logging, autenticación, validación y rate limiting.

### **Requisitos**
- Implementar middleware de logging estructurado
- Crear middleware de autenticación JWT
- Implementar middleware de validación de datos
- Crear middleware de rate limiting personalizado

### **Implementación**

#### **middleware/logger.js**
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

const loggerMiddleware = (req, res, next) => {
  const startTime = Date.now();
  
  // Log del request
  logger.info('HTTP Request', {
    method: req.method,
    url: req.url,
    ip: req.ip,
    userAgent: req.get('User-Agent'),
    userId: req.user?.id,
    requestId: req.headers['x-request-id'] || generateRequestId()
  });
  
  // Interceptar response
  const originalSend = res.send;
  res.send = function(data) {
    const duration = Date.now() - startTime;
    
    // Log del response
    logger.info('HTTP Response', {
      method: req.method,
      url: req.url,
      statusCode: res.statusCode,
      duration,
      requestId: req.headers['x-request-id'] || generateRequestId()
    });
    
    originalSend.call(this, data);
  };
  
  next();
};

function generateRequestId() {
  return Date.now().toString(36) + Math.random().toString(36).substr(2);
}

module.exports = { logger, loggerMiddleware };
```

#### **middleware/auth.js**
```javascript
const jwt = require('jsonwebtoken');

const authenticateToken = (req, res, next) => {
  const authHeader = req.headers.authorization;
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({
      success: false,
      error: {
        code: 'NO_TOKEN',
        message: 'Access token is required'
      }
    });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      if (err.name === 'TokenExpiredError') {
        return res.status(401).json({
          success: false,
          error: {
            code: 'TOKEN_EXPIRED',
            message: 'Access token has expired'
          }
        });
      }
      
      return res.status(401).json({
        success: false,
        error: {
          code: 'INVALID_TOKEN',
          message: 'Invalid access token'
        }
      });
    }
    
    req.user = user;
    next();
  });
};

const authorizeRole = (...roles) => {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'UNAUTHORIZED',
          message: 'Authentication required'
        }
      });
    }
    
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        success: false,
        error: {
          code: 'INSUFFICIENT_PERMISSIONS',
          message: 'You do not have permission to perform this action'
        }
      });
    }
    
    next();
  };
};

module.exports = { authenticateToken, authorizeRole };
```

#### **middleware/validation.js**
```javascript
const { body, validationResult } = require('express-validator');

const validateUser = [
  body('username')
    .trim()
    .isLength({ min: 3, max: 30 })
    .withMessage('Username must be between 3 and 30 characters')
    .matches(/^[a-zA-Z0-9_]+$/)
    .withMessage('Username can only contain letters, numbers and underscores'),
  
  body('email')
    .isEmail()
    .normalizeEmail()
    .withMessage('Must be a valid email address'),
  
  body('password')
    .isLength({ min: 8 })
    .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/)
    .withMessage('Password must be at least 8 characters with uppercase, lowercase, number and special character'),
  
  body('age')
    .optional()
    .isInt({ min: 13, max: 120 })
    .withMessage('Age must be between 13 and 120'),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(422).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Validation failed',
          details: errors.array().map(err => ({
            field: err.path,
            message: err.msg,
            value: err.value
          }))
        }
      });
    }
    next();
  }
];

const validatePost = [
  body('title')
    .trim()
    .isLength({ min: 1, max: 200 })
    .withMessage('Title must be between 1 and 200 characters'),
  
  body('content')
    .trim()
    .isLength({ min: 10 })
    .withMessage('Content must be at least 10 characters'),
  
  body('category')
    .optional()
    .isIn(['technology', 'science', 'arts', 'sports', 'politics'])
    .withMessage('Invalid category'),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(422).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Validation failed',
          details: errors.array()
        }
      });
    }
    next();
  }
];

module.exports = { validateUser, validatePost };
```

#### **middleware/rateLimit.js**
```javascript
class RateLimiter {
  constructor(windowMs = 15 * 60 * 1000, maxRequests = 100) {
    this.windowMs = windowMs;
    this.maxRequests = maxRequests;
    this.requests = new Map();
  }
  
  middleware() {
    return (req, res, next) => {
      const key = req.ip;
      const now = Date.now();
      
      if (!this.requests.has(key)) {
        this.requests.set(key, []);
      }
      
      const userRequests = this.requests.get(key);
      const validRequests = userRequests.filter(time => now - time < this.windowMs);
      
      if (validRequests.length >= this.maxRequests) {
        return res.status(429).json({
          success: false,
          error: {
            code: 'RATE_LIMIT_EXCEEDED',
            message: 'Too many requests, please try again later',
            retryAfter: Math.ceil(this.windowMs / 1000)
          }
        });
      }
      
      validRequests.push(now);
      this.requests.set(key, validRequests);
      
      // Añadir headers de rate limit
      res.set({
        'X-RateLimit-Limit': this.maxRequests,
        'X-RateLimit-Remaining': this.maxRequests - validRequests.length,
        'X-RateLimit-Reset': new Date(now + this.windowMs).toISOString()
      });
      
      next();
    };
  }
  
  cleanup() {
    const now = Date.now();
    for (const [key, requests] of this.requests.entries()) {
      const validRequests = requests.filter(time => now - time < this.windowMs);
      if (validRequests.length === 0) {
        this.requests.delete(key);
      } else {
        this.requests.set(key, validRequests);
      }
    }
  }
}

// Limpiar requests expirados cada minuto
const rateLimiter = new RateLimiter();
setInterval(() => rateLimiter.cleanup(), 60 * 1000);

module.exports = rateLimiter;
```

### **Ejercicio para el Estudiante**
1. Implementa middleware de compresión de respuestas
2. Crea middleware de cache para respuestas
3. Implementa middleware de CORS personalizado
4. Añade middleware de sanitización de inputs

---

## 📋 **EJERCICIO 2: ROUTING AVANZADO**

### **Descripción**
Crear un sistema de routing modular con middleware específico, validación y manejo de errores.

### **Requisitos**
- Implementar routing modular por entidades
- Crear middleware específico para cada ruta
- Implementar manejo de errores por ruta
- Crear sistema de versionado de APIs

### **Implementación**

#### **routes/users.js**
```javascript
const express = require('express');
const { authenticateToken, authorizeRole } = require('../middleware/auth');
const { validateUser } = require('../middleware/validation');
const UserController = require('../controllers/UserController');

const router = express.Router();

// Middleware específico para rutas de usuarios
router.use((req, res, next) => {
  req.resourceType = 'user';
  next();
});

// GET /users - Listar usuarios
router.get('/', 
  authenticateToken,
  authorizeRole('admin', 'moderator'),
  async (req, res, next) => {
    try {
      const { page = 1, limit = 10, search, role } = req.query;
      const users = await UserController.getUsers({ page, limit, search, role });
      
      res.json({
        success: true,
        data: users
      });
    } catch (error) {
      next(error);
    }
  }
);

// GET /users/:id - Obtener usuario específico
router.get('/:id',
  authenticateToken,
  async (req, res, next) => {
    try {
      const user = await UserController.getUserById(req.params.id);
      
      // Verificar permisos
      if (req.user.role !== 'admin' && req.user.id !== user.id) {
        return res.status(403).json({
          success: false,
          error: {
            code: 'ACCESS_DENIED',
            message: 'You can only view your own profile'
          }
        });
      }
      
      res.json({
        success: true,
        data: user
      });
    } catch (error) {
      next(error);
    }
  }
);

// POST /users - Crear usuario
router.post('/',
  validateUser,
  async (req, res, next) => {
    try {
      const user = await UserController.createUser(req.body);
      
      res.status(201).json({
        success: true,
        data: user,
        message: 'User created successfully'
      });
    } catch (error) {
      next(error);
    }
  }
);

// PUT /users/:id - Actualizar usuario
router.put('/:id',
  authenticateToken,
  validateUser,
  async (req, res, next) => {
    try {
      // Verificar permisos
      if (req.user.role !== 'admin' && req.user.id !== req.params.id) {
        return res.status(403).json({
          success: false,
          error: {
            code: 'ACCESS_DENIED',
            message: 'You can only update your own profile'
          }
        });
      }
      
      const user = await UserController.updateUser(req.params.id, req.body);
      
      res.json({
        success: true,
        data: user,
        message: 'User updated successfully'
      });
    } catch (error) {
      next(error);
    }
  }
);

// DELETE /users/:id - Eliminar usuario
router.delete('/:id',
  authenticateToken,
  authorizeRole('admin'),
  async (req, res, next) => {
    try {
      await UserController.deleteUser(req.params.id);
      
      res.json({
        success: true,
        message: 'User deleted successfully'
      });
    } catch (error) {
      next(error);
    }
  }
);

// Rutas anidadas
router.use('/:userId/posts', require('./userPosts'));
router.use('/:userId/profile', require('./userProfile'));

module.exports = router;
```

#### **routes/userPosts.js**
```javascript
const express = require('express');
const { authenticateToken } = require('../middleware/auth');
const PostController = require('../controllers/PostController');

const router = express.Router({ mergeParams: true });

// Middleware para verificar que el usuario existe
router.use(async (req, res, next) => {
  try {
    const user = await UserController.getUserById(req.params.userId);
    if (!user) {
      return res.status(404).json({
        success: false,
        error: {
          code: 'USER_NOT_FOUND',
          message: 'User not found'
        }
      });
    }
    req.targetUser = user;
    next();
  } catch (error) {
    next(error);
  }
});

// GET /users/:userId/posts - Obtener posts del usuario
router.get('/', async (req, res, next) => {
  try {
    const posts = await PostController.getPostsByUser(req.params.userId, req.query);
    
    res.json({
      success: true,
      data: posts
    });
  } catch (error) {
    next(error);
  }
});

// POST /users/:userId/posts - Crear post para el usuario
router.post('/',
  authenticateToken,
  (req, res, next) => {
    // Verificar que el usuario autenticado es el dueño
    if (req.user.id !== req.params.userId) {
      return res.status(403).json({
        success: false,
        error: {
          code: 'ACCESS_DENIED',
          message: 'You can only create posts for your own account'
        }
      });
    }
    next();
  },
  async (req, res, next) => {
    try {
      const post = await PostController.createPost({
        ...req.body,
        authorId: req.params.userId
      });
      
      res.status(201).json({
        success: true,
        data: post,
        message: 'Post created successfully'
      });
    } catch (error) {
      next(error);
    }
  }
);

module.exports = router;
```

### **Ejercicio para el Estudiante**
1. Implementa rutas para posts y comentarios
2. Crea sistema de versionado de API (v1, v2)
3. Implementa rutas con parámetros dinámicos
4. Añade middleware de logging específico por ruta

---

## 📋 **EJERCICIO 3: MANEJO DE ERRORES**

### **Descripción**
Implementar un sistema robusto de manejo de errores con clases personalizadas y middleware especializado.

### **Requisitos**
- Crear clases de error personalizadas
- Implementar middleware de manejo de errores
- Crear sistema de logging de errores
- Implementar respuestas de error consistentes

### **Implementación**

#### **errors/AppError.js**
```javascript
class AppError extends Error {
  constructor(message, statusCode, code = null) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
    
    Error.captureStackTrace(this, this.constructor);
  }
}

class ValidationError extends AppError {
  constructor(message, details = []) {
    super(message, 422, 'VALIDATION_ERROR');
    this.details = details;
  }
}

class AuthenticationError extends AppError {
  constructor(message = 'Authentication failed') {
    super(message, 401, 'AUTHENTICATION_ERROR');
  }
}

class AuthorizationError extends AppError {
  constructor(message = 'Insufficient permissions') {
    super(message, 403, 'AUTHORIZATION_ERROR');
  }
}

class NotFoundError extends AppError {
  constructor(resource = 'Resource') {
    super(`${resource} not found`, 404, 'NOT_FOUND_ERROR');
  }
}

class ConflictError extends AppError {
  constructor(message = 'Resource conflict') {
    super(message, 409, 'CONFLICT_ERROR');
  }
}

class RateLimitError extends AppError {
  constructor(message = 'Rate limit exceeded') {
    super(message, 429, 'RATE_LIMIT_ERROR');
  }
}

module.exports = {
  AppError,
  ValidationError,
  AuthenticationError,
  AuthorizationError,
  NotFoundError,
  ConflictError,
  RateLimitError
};
```

#### **middleware/errorHandler.js**
```javascript
const { logger } = require('./logger');

const errorHandler = (err, req, res, next) => {
  let error = { ...err };
  error.message = err.message;
  
  // Log del error
  logger.error('Error occurred', {
    error: {
      message: err.message,
      stack: err.stack,
      name: err.name
    },
    request: {
      method: req.method,
      url: req.url,
      ip: req.ip,
      userId: req.user?.id
    }
  });
  
  // Error de validación de Mongoose
  if (err.name === 'ValidationError') {
    const message = 'Validation Error';
    const details = Object.values(err.errors).map(val => ({
      field: val.path,
      message: val.message,
      value: val.value
    }));
    
    error = {
      statusCode: 422,
      code: 'VALIDATION_ERROR',
      message,
      details
    };
  }
  
  // Error de duplicación de Mongoose
  if (err.code === 11000) {
    const field = Object.keys(err.keyValue)[0];
    const value = err.keyValue[field];
    
    error = {
      statusCode: 409,
      code: 'DUPLICATE_ERROR',
      message: `${field} '${value}' already exists`,
      details: [{ field, value }]
    };
  }
  
  // Error de JWT
  if (err.name === 'JsonWebTokenError') {
    error = {
      statusCode: 401,
      code: 'INVALID_TOKEN',
      message: 'Invalid access token'
    };
  }
  
  if (err.name === 'TokenExpiredError') {
    error = {
      statusCode: 401,
      code: 'TOKEN_EXPIRED',
      message: 'Access token has expired'
    };
  }
  
  // Error de cast de MongoDB
  if (err.name === 'CastError') {
    error = {
      statusCode: 400,
      code: 'INVALID_ID',
      message: 'Invalid ID format'
    };
  }
  
  // Respuesta de error
  res.status(error.statusCode || 500).json({
    success: false,
    error: {
      code: error.code || 'INTERNAL_SERVER_ERROR',
      message: error.message || 'Internal server error',
      details: error.details || [],
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    },
    timestamp: new Date().toISOString(),
    path: req.url,
    method: req.method
  });
};

// Middleware para manejar rutas no encontradas
const notFoundHandler = (req, res, next) => {
  const error = new Error(`Route ${req.originalUrl} not found`);
  error.statusCode = 404;
  next(error);
};

module.exports = { errorHandler, notFoundHandler };
```

### **Ejercicio para el Estudiante**
1. Implementa manejo de errores específicos por tipo
2. Crea sistema de notificación de errores críticos
3. Implementa retry logic para errores transitorios
4. Añade métricas de errores y alertas

---

## 📋 **EJERCICIO 4: TESTING DE APIS**

### **Descripción**
Implementar tests completos para las APIs creadas usando Supertest y Jest.

### **Requisitos**
- Tests unitarios para controladores
- Tests de integración para rutas
- Tests de middleware
- Tests de manejo de errores

### **Implementación**

#### **tests/integration/users.test.js**
```javascript
const request = require('supertest');
const app = require('../../app');
const mongoose = require('mongoose');
const User = require('../../models/User');
const { generateToken } = require('../../utils/auth');

describe('Users API Integration', () => {
  let authToken;
  let adminToken;
  
  beforeAll(async () => {
    await mongoose.connect(process.env.MONGODB_TEST_URI);
  });
  
  afterAll(async () => {
    await mongoose.connection.close();
  });
  
  beforeEach(async () => {
    await User.deleteMany({});
    
    // Crear usuario de prueba
    const user = await User.create({
      username: 'testuser',
      email: 'test@example.com',
      password: 'Password123!',
      role: 'user'
    });
    
    authToken = generateToken(user._id, user.role);
    
    // Crear admin
    const admin = await User.create({
      username: 'admin',
      email: 'admin@example.com',
      password: 'Admin123!',
      role: 'admin'
    });
    
    adminToken = generateToken(admin._id, admin.role);
  });
  
  describe('POST /api/users', () => {
    it('should create a new user with valid data', async () => {
      const userData = {
        username: 'newuser',
        email: 'new@example.com',
        password: 'NewPass123!'
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(201);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data.username).toBe(userData.username);
      expect(response.body.data.email).toBe(userData.email);
      expect(response.body.data).to.not.have.property('password');
    });
    
    it('should return 422 for invalid data', async () => {
      const invalidData = {
        username: 'te', // muy corto
        email: 'invalid-email',
        password: 'weak' // muy débil
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(invalidData)
        .expect(422);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('VALIDATION_ERROR');
      expect(response.body.error.details).to.have.length(3);
    });
    
    it('should return 409 for duplicate username', async () => {
      const duplicateData = {
        username: 'testuser', // ya existe
        email: 'different@example.com',
        password: 'Password123!'
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(duplicateData)
        .expect(409);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('DUPLICATE_ERROR');
    });
  });
  
  describe('GET /api/users', () => {
    it('should return users list for admin', async () => {
      const response = await request(app)
        .get('/api/users')
        .set('Authorization', `Bearer ${adminToken}`)
        .expect(200);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data).to.be.an('array');
      expect(response.body.data).to.have.length(2);
    });
    
    it('should return 403 for non-admin users', async () => {
      const response = await request(app)
        .get('/api/users')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(403);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('INSUFFICIENT_PERMISSIONS');
    });
    
    it('should return 401 without token', async () => {
      const response = await request(app)
        .get('/api/users')
        .expect(401);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('NO_TOKEN');
    });
  });
  
  describe('GET /api/users/:id', () => {
    it('should return user profile for owner', async () => {
      const user = await User.findOne({ username: 'testuser' });
      
      const response = await request(app)
        .get(`/api/users/${user._id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data.username).toBe('testuser');
    });
    
    it('should return 403 for other users', async () => {
      const otherUser = await User.findOne({ username: 'testuser' });
      
      const response = await request(app)
        .get(`/api/users/${otherUser._id}`)
        .set('Authorization', `Bearer ${adminToken}`)
        .expect(403);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('ACCESS_DENIED');
    });
  });
  
  describe('PUT /api/users/:id', () => {
    it('should update user profile for owner', async () => {
      const user = await User.findOne({ username: 'testuser' });
      const updateData = { firstName: 'John', lastName: 'Doe' };
      
      const response = await request(app)
        .put(`/api/users/${user._id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .send(updateData)
        .expect(200);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data.firstName).toBe(updateData.firstName);
      expect(response.body.data.lastName).toBe(updateData.lastName);
    });
    
    it('should return 403 for other users', async () => {
      const otherUser = await User.findOne({ username: 'testuser' });
      const updateData = { firstName: 'Hacked' };
      
      const response = await request(app)
        .put(`/api/users/${otherUser._id}`)
        .set('Authorization', `Bearer ${adminToken}`)
        .send(updateData)
        .expect(403);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('ACCESS_DENIED');
    });
  });
  
  describe('DELETE /api/users/:id', () => {
    it('should delete user for admin', async () => {
      const user = await User.findOne({ username: 'testuser' });
      
      const response = await request(app)
        .delete(`/api/users/${user._id}`)
        .set('Authorization', `Bearer ${adminToken}`)
        .expect(200);
      
      expect(response.body.success).toBe(true);
      
      // Verificar que el usuario fue eliminado
      const deletedUser = await User.findById(user._id);
      expect(deletedUser).to.be.null;
    });
    
    it('should return 403 for non-admin users', async () => {
      const user = await User.findOne({ username: 'testuser' });
      
      const response = await request(app)
        .delete(`/api/users/${user._id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .expect(403);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('INSUFFICIENT_PERMISSIONS');
    });
  });
});
```

### **Ejercicio para el Estudiante**
1. Implementa tests para posts y comentarios
2. Crea tests de performance y carga
3. Implementa tests de seguridad
4. Añade tests de edge cases

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 5: API Versioning**
Implementa un sistema de versionado de APIs que permita mantener compatibilidad.

### **Ejercicio 6: Caching Strategy**
Crea un sistema de cache con Redis para mejorar el rendimiento de las APIs.

### **Ejercicio 7: File Upload**
Implementa un sistema de subida de archivos con validación y almacenamiento.

### **Ejercicio 8: Real-time Features**
Añade funcionalidades en tiempo real usando WebSockets.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Funcionalidad (35%)**
- ✅ APIs funcionando correctamente
- ✅ Middleware implementado
- ✅ Manejo de errores robusto
- ✅ Validación de datos

### **Arquitectura (25%)**
- ✅ Estructura modular
- ✅ Separación de responsabilidades
- ✅ Patrones de diseño apropiados
- ✅ Código reutilizable

### **Testing (25%)**
- ✅ Cobertura de tests
- ✅ Tests de integración
- ✅ Tests de edge cases
- ✅ Mocks y stubs

### **Documentación (15%)**
- ✅ README claro
- ✅ Documentación de APIs
- ✅ Ejemplos de uso
- ✅ Instrucciones de deployment

---

## 🚀 **PRÓXIMOS PASOS**

Una vez completados estos ejercicios, estarás listo para:
1. **Integración con Bases de Datos**: MongoDB, PostgreSQL
2. **Autenticación Avanzada**: OAuth, JWT refresh
3. **Performance**: Optimización, caching, load balancing
4. **Deployment**: Docker, CI/CD, cloud platforms

---

*Estos ejercicios te proporcionarán experiencia práctica en Express.js y te prepararán para crear APIs robustas y escalables.* 