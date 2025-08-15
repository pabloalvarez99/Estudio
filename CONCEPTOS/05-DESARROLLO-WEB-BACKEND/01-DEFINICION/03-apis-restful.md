# 🌐 APIs RESTful: Diseño e Implementación de APIs Web

## 🎯 **OBJETIVO**
Dominar el diseño e implementación de APIs RESTful siguiendo las mejores prácticas, convenciones estándar y patrones de arquitectura para crear APIs escalables y mantenibles.

---

## 📋 **¿QUÉ SON LAS APIs RESTful?**

### **Definición**
REST (Representational State Transfer) es un estilo de arquitectura para sistemas distribuidos, especialmente aplicaciones web. Una API RESTful es una interfaz que sigue los principios REST para permitir la comunicación entre sistemas.

### **Características Principales**
- **Stateless**: Cada request contiene toda la información necesaria
- **Client-Server**: Separación clara entre cliente y servidor
- **Cacheable**: Las respuestas pueden ser cacheadas
- **Uniform Interface**: Interfaz consistente y predecible
- **Layered System**: Arquitectura en capas
- **Code on Demand**: Código ejecutable opcional

---

## 🏗️ **PRINCIPIOS FUNDAMENTALES DE REST**

### **1. Recursos como Entidades**
```javascript
// ✅ CORRECTO - Los recursos son sustantivos
GET /users           // Obtener usuarios
GET /users/123       // Obtener usuario específico
POST /users          // Crear usuario
PUT /users/123       // Actualizar usuario completo
PATCH /users/123     // Actualizar usuario parcialmente
DELETE /users/123    // Eliminar usuario

// ❌ INCORRECTO - Verbos en URLs
GET /getUsers
POST /createUser
PUT /updateUser
DELETE /removeUser
```

### **2. HTTP Methods Semánticos**
```javascript
// GET - Obtener recursos (no modifica estado)
GET /users                    // Lista de usuarios
GET /users/123               // Usuario específico
GET /users/123/posts         // Posts del usuario
GET /users?page=2&limit=10  // Paginación

// POST - Crear nuevos recursos
POST /users                  // Crear usuario
POST /users/123/posts       // Crear post para usuario

// PUT - Actualizar recurso completo
PUT /users/123              // Actualizar usuario completo

// PATCH - Actualizar recurso parcialmente
PATCH /users/123            // Actualizar campos específicos

// DELETE - Eliminar recursos
DELETE /users/123           // Eliminar usuario
DELETE /users/123/posts/456 // Eliminar post específico
```

### **3. URLs Jerárquicas y Anidadas**
```javascript
// Recursos anidados
GET /users/123/posts                    // Posts del usuario 123
GET /users/123/posts/456               // Post 456 del usuario 123
GET /users/123/posts/456/comments      // Comentarios del post 456
GET /users/123/posts/456/comments/789  // Comentario 789 del post 456

// Relaciones
GET /users/123/followers               // Seguidores del usuario 123
GET /users/123/following               // Usuarios que sigue el usuario 123

// Acciones específicas (cuando no hay recurso natural)
POST /users/123/follow                 // Seguir usuario
DELETE /users/123/follow               // Dejar de seguir usuario
POST /users/123/posts/456/like         // Dar like al post
```

---

## 📊 **HTTP STATUS CODES**

### **Códigos 2xx - Éxito**
```javascript
// 200 OK - Request exitoso
app.get('/users', (req, res) => {
  const users = getAllUsers();
  res.status(200).json(users);
});

// 201 Created - Recurso creado exitosamente
app.post('/users', (req, res) => {
  const user = createUser(req.body);
  res.status(201).json({
    message: 'User created successfully',
    user: user
  });
});

// 204 No Content - Éxito pero sin contenido
app.delete('/users/123', (req, res) => {
  deleteUser(123);
  res.status(204).send();
});
```

### **Códigos 4xx - Error del Cliente**
```javascript
// 400 Bad Request - Request mal formado
app.post('/users', (req, res) => {
  if (!req.body.email || !req.body.password) {
    return res.status(400).json({
      error: 'Missing required fields',
      required: ['email', 'password']
    });
  }
  // ... crear usuario
});

// 401 Unauthorized - No autenticado
app.get('/profile', authenticate, (req, res) => {
  // Si no hay token válido, middleware authenticate retorna 401
});

// 403 Forbidden - Autenticado pero no autorizado
app.delete('/users/123', authenticate, authorizeAdmin, (req, res) => {
  // Si no es admin, middleware authorizeAdmin retorna 403
});

// 404 Not Found - Recurso no encontrado
app.get('/users/999', (req, res) => {
  const user = findUser(999);
  if (!user) {
    return res.status(404).json({
      error: 'User not found',
      message: 'No user exists with ID 999'
    });
  }
  res.json(user);
});

// 409 Conflict - Conflicto (ej: email duplicado)
app.post('/users', (req, res) => {
  if (userExists(req.body.email)) {
    return res.status(409).json({
      error: 'User already exists',
      message: 'A user with this email already exists'
    });
  }
  // ... crear usuario
});

// 422 Unprocessable Entity - Validación fallida
app.post('/users', validateUser, (req, res) => {
  // Si validación falla, middleware validateUser retorna 422
});
```

### **Códigos 5xx - Error del Servidor**
```javascript
// 500 Internal Server Error - Error interno del servidor
app.get('/users', async (req, res, next) => {
  try {
    const users = await getAllUsers();
    res.json(users);
  } catch (error) {
    next(error); // Pasa al middleware de manejo de errores
  }
});

// 503 Service Unavailable - Servicio temporalmente no disponible
app.get('/users', (req, res) => {
  if (isDatabaseDown()) {
    return res.status(503).json({
      error: 'Service temporarily unavailable',
      message: 'Database is currently down, please try again later',
      retryAfter: 300 // segundos
    });
  }
  // ... obtener usuarios
});
```

---

## 🎨 **DISEÑO DE RESPONSES**

### **Estructura Consistente**
```javascript
// Response de éxito
{
  "success": true,
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "message": "User retrieved successfully",
  "timestamp": "2024-01-15T10:30:00Z"
}

// Response de error
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  },
  "timestamp": "2024-01-15T10:30:00Z"
}

// Response de lista con paginación
{
  "success": true,
  "data": [
    { "id": 1, "name": "User 1" },
    { "id": 2, "name": "User 2" }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25,
    "pages": 3
  },
  "message": "Users retrieved successfully"
}
```

### **Implementación en Express**
```javascript
// Middleware para responses consistentes
const successResponse = (res, data, message = 'Success', statusCode = 200) => {
  res.status(statusCode).json({
    success: true,
    data,
    message,
    timestamp: new Date().toISOString()
  });
};

const errorResponse = (res, error, statusCode = 500) => {
  res.status(statusCode).json({
    success: false,
    error: {
      code: error.code || 'INTERNAL_ERROR',
      message: error.message || 'Internal server error',
      details: error.details || []
    },
    timestamp: new Date().toISOString()
  });
};

// Uso en routes
app.get('/users', async (req, res, next) => {
  try {
    const users = await getAllUsers();
    successResponse(res, users, 'Users retrieved successfully');
  } catch (error) {
    next(error);
  }
});

app.post('/users', async (req, res, next) => {
  try {
    const user = await createUser(req.body);
    successResponse(res, user, 'User created successfully', 201);
  } catch (error) {
    if (error.name === 'ValidationError') {
      return errorResponse(res, {
        code: 'VALIDATION_ERROR',
        message: 'Validation failed',
        details: error.details
      }, 422);
    }
    next(error);
  }
});
```

---

## 🔍 **FILTRADO, ORDENAMIENTO Y PAGINACIÓN**

### **Query Parameters**
```javascript
// Filtrado
GET /users?role=admin&status=active
GET /users?age[gte]=18&age[lte]=65
GET /users?tags[]=javascript&tags[]=nodejs

// Ordenamiento
GET /users?sort=name&order=asc
GET /users?sort=-createdAt,name
GET /users?sort=age:desc,name:asc

// Paginación
GET /users?page=2&limit=10
GET /users?offset=20&limit=10

// Búsqueda
GET /users?search=john
GET /users?q=john+doe
```

### **Implementación en Express**
```javascript
// Middleware para parsing de queries
const parseQuery = (req, res, next) => {
  const { page = 1, limit = 10, sort, search, ...filters } = req.query;
  
  req.queryOptions = {
    page: parseInt(page),
    limit: parseInt(limit),
    skip: (parseInt(page) - 1) * parseInt(limit),
    sort: parseSort(sort),
    search: search,
    filters: parseFilters(filters)
  };
  
  next();
};

const parseSort = (sort) => {
  if (!sort) return { createdAt: -1 };
  
  const sortObj = {};
  const fields = sort.split(',');
  
  fields.forEach(field => {
    if (field.startsWith('-')) {
      sortObj[field.slice(1)] = -1;
    } else {
      sortObj[field] = 1;
    }
  });
  
  return sortObj;
};

const parseFilters = (filters) => {
  const filterObj = {};
  
  Object.keys(filters).forEach(key => {
    const value = filters[key];
    
    if (key.includes('[') && key.includes(']')) {
      // Filtros de rango: age[gte]=18
      const [field, operator] = key.split(/[\[\]]/);
      if (!filterObj[field]) filterObj[field] = {};
      filterObj[field][`$${operator}`] = value;
    } else if (Array.isArray(value)) {
      // Filtros de array: tags[]=javascript
      filterObj[key] = { $in: value };
    } else {
      // Filtros simples
      filterObj[key] = value;
    }
  });
  
  return filterObj;
};

// Uso en routes
app.get('/users', parseQuery, async (req, res, next) => {
  try {
    const { page, limit, skip, sort, search, filters } = req.queryOptions;
    
    let query = { ...filters };
    
    if (search) {
      query.$or = [
        { name: { $regex: search, $options: 'i' } },
        { email: { $regex: search, $options: 'i' } }
      ];
    }
    
    const users = await User.find(query)
      .sort(sort)
      .skip(skip)
      .limit(limit);
    
    const total = await User.countDocuments(query);
    
    successResponse(res, {
      users,
      pagination: {
        page,
        limit,
        total,
        pages: Math.ceil(total / limit)
      }
    });
  } catch (error) {
    next(error);
  }
});
```

---

## 🔐 **AUTENTICACIÓN Y AUTORIZACIÓN**

### **JWT Authentication**
```javascript
const jwt = require('jsonwebtoken');

// Middleware de autenticación
const authenticate = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (!token) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'NO_TOKEN',
          message: 'Access token is required'
        }
      });
    }
    
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    const user = await User.findById(decoded.userId);
    
    if (!user) {
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
  } catch (error) {
    if (error.name === 'JsonWebTokenError') {
      return res.status(401).json({
        success: false,
        error: {
          code: 'INVALID_TOKEN',
          message: 'Invalid access token'
        }
      });
    }
    
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({
        success: false,
        error: {
          code: 'TOKEN_EXPIRED',
          message: 'Access token has expired'
        }
      });
    }
    
    next(error);
  }
};

// Middleware de autorización
const authorize = (...roles) => {
  return (req, res, next) => {
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

// Uso en routes
app.get('/admin/users', authenticate, authorize('admin'), async (req, res) => {
  // Solo admins pueden acceder
});
```

---

## 📝 **VALIDACIÓN Y SANITIZACIÓN**

### **Express Validator**
```javascript
const { body, param, query, validationResult } = require('express-validator');

// Validación de creación de usuario
const validateCreateUser = [
  body('name')
    .trim()
    .isLength({ min: 2, max: 50 })
    .withMessage('Name must be between 2 and 50 characters'),
  
  body('email')
    .isEmail()
    .normalizeEmail()
    .withMessage('Must be a valid email address'),
  
  body('password')
    .isLength({ min: 6 })
    .matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/)
    .withMessage('Password must be at least 6 characters with uppercase, lowercase and number'),
  
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

// Uso
app.post('/users', validateCreateUser, async (req, res) => {
  // Usuario validado
});
```

---

## 🧪 **TESTING DE APIs**

### **Supertest para Testing HTTP**
```javascript
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
  
  describe('POST /users', () => {
    it('should create a new user with valid data', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'Password123'
      };
      
      const response = await request(app)
        .post('/users')
        .send(userData)
        .expect(201);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data.name).toBe(userData.name);
      expect(response.body.data.email).toBe(userData.email);
      expect(response.body.data.password).toBeUndefined(); // No debe incluir password
    });
    
    it('should return validation error for invalid email', async () => {
      const userData = {
        name: 'John Doe',
        email: 'invalid-email',
        password: 'Password123'
      };
      
      const response = await request(app)
        .post('/users')
        .send(userData)
        .expect(422);
      
      expect(response.body.success).toBe(false);
      expect(response.body.error.code).toBe('VALIDATION_ERROR');
      expect(response.body.error.details).toHaveLength(1);
      expect(response.body.error.details[0].field).toBe('email');
    });
  });
  
  describe('GET /users', () => {
    it('should return paginated users', async () => {
      // Crear usuarios de prueba
      await User.create([
        { name: 'User 1', email: 'user1@test.com', password: 'pass123' },
        { name: 'User 2', email: 'user2@test.com', password: 'pass123' }
      ]);
      
      const response = await request(app)
        .get('/users?page=1&limit=10')
        .expect(200);
      
      expect(response.body.success).toBe(true);
      expect(response.body.data.users).toHaveLength(2);
      expect(response.body.data.pagination.page).toBe(1);
      expect(response.body.data.pagination.total).toBe(2);
    });
  });
});
```

---

## 📚 **DOCUMENTACIÓN DE APIs**

### **OpenAPI/Swagger**
```javascript
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'User Management API',
      version: '1.0.0',
      description: 'API para gestión de usuarios',
    },
    servers: [
      {
        url: 'http://localhost:3000',
        description: 'Development server',
      },
    ],
    components: {
      securitySchemes: {
        bearerAuth: {
          type: 'http',
          scheme: 'bearer',
          bearerFormat: 'JWT',
        },
      },
    },
  },
  apis: ['./routes/*.js'], // Archivos que contienen anotaciones
};

const specs = swaggerJsdoc(options);

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(specs));

// Ejemplo de documentación en route
/**
 * @swagger
 * /users:
 *   get:
 *     summary: Obtener lista de usuarios
 *     tags: [Users]
 *     security:
 *       - bearerAuth: []
 *     parameters:
 *       - in: query
 *         name: page
 *         schema:
 *           type: integer
 *           default: 1
 *         description: Número de página
 *       - in: query
 *         name: limit
 *         schema:
 *           type: integer
 *           default: 10
 *         description: Usuarios por página
 *     responses:
 *       200:
 *         description: Lista de usuarios obtenida exitosamente
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 *               properties:
 *                 success:
 *                   type: boolean
 *                 data:
 *                   type: object
 *                   properties:
 *                     users:
 *                       type: array
 *                       items:
 *                         $ref: '#/components/schemas/User'
 *       401:
 *         description: No autorizado
 *       500:
 *         description: Error interno del servidor
 */
app.get('/users', authenticate, async (req, res) => {
  // Implementación
});
```

---

## 🚀 **MEJORES PRÁCTICAS**

### **1. Versionado de APIs**
```javascript
// Versionado en URL
app.use('/api/v1', v1Routes);
app.use('/api/v2', v2Routes);

// Versionado en headers
app.use('/api', (req, res, next) => {
  const version = req.headers['api-version'] || 'v1';
  req.apiVersion = version;
  next();
});
```

### **2. Rate Limiting**
```javascript
const rateLimit = require('express-rate-limit');

const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100, // máximo 100 requests por ventana
  message: {
    success: false,
    error: {
      code: 'RATE_LIMIT_EXCEEDED',
      message: 'Too many requests, please try again later.'
    }
  },
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', apiLimiter);
```

### **3. Caching**
```javascript
const mcache = require('memory-cache');

const cache = (duration) => {
  return (req, res, next) => {
    const key = `__express__${req.originalUrl || req.url}`;
    const cachedBody = mcache.get(key);
    
    if (cachedBody) {
      res.send(cachedBody);
      return;
    }
    
    res.sendResponse = res.send;
    res.send = (body) => {
      mcache.put(key, body, duration * 1000);
      res.sendResponse(body);
    };
    next();
  };
};

// Cache por 5 minutos
app.get('/users', cache(300), async (req, res) => {
  // Response será cacheada
});
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: API REST Básica**
Implementa una API REST completa para gestionar una biblioteca de libros.

### **Ejercicio 2: Filtrado y Paginación**
Añade filtros, ordenamiento y paginación a tu API.

### **Ejercicio 3: Autenticación JWT**
Implementa sistema de autenticación con JWT.

### **Ejercicio 4: Validación y Testing**
Añade validación robusta y tests completos.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [REST API Tutorial](https://restfulapi.net/)
- [HTTP Status Codes](https://httpstatuses.com/)
- [OpenAPI Specification](https://swagger.io/specification/)

### **Herramientas Recomendadas**
- [Postman](https://www.postman.com/) - Testing de APIs
- [Insomnia](https://insomnia.rest/) - Cliente REST alternativo
- [Swagger UI](https://swagger.io/tools/swagger-ui/) - Documentación interactiva

### **Libros y Cursos**
- "RESTful Web Services" - Leonard Richardson
- "API Design Patterns" - JJ Geewax
- REST API Design Course (Udemy)

---

*Las APIs RESTful son el estándar de facto para la comunicación entre sistemas web. Dominar su diseño e implementación te permitirá crear interfaces robustas y escalables.* 