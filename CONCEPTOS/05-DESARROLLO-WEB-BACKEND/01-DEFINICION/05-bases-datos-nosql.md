# 🗄️ Bases de Datos NoSQL: MongoDB, Redis y Más

## 🎯 **OBJETIVO**
Dominar las bases de datos NoSQL, especialmente MongoDB y Redis, comprendiendo sus características, casos de uso y cómo integrarlas en aplicaciones Node.js.

---

## 📋 **¿QUÉ SON LAS BASES DE DATOS NOSQL?**

### **Definición**
NoSQL (Not Only SQL) se refiere a bases de datos que no siguen el modelo relacional tradicional. Están diseñadas para manejar grandes volúmenes de datos no estructurados y semi-estructurados.

### **Tipos de Bases de Datos NoSQL**
- **Document Stores**: MongoDB, CouchDB
- **Key-Value Stores**: Redis, DynamoDB
- **Column Stores**: Cassandra, HBase
- **Graph Databases**: Neo4j, ArangoDB

### **Ventajas de NoSQL**
- **Escalabilidad horizontal**: Fácil distribución en múltiples servidores
- **Flexibilidad de esquema**: Estructura de datos dinámica
- **Alto rendimiento**: Optimizadas para operaciones específicas
- **Disponibilidad**: Tolerancia a fallos y alta disponibilidad

---

## 🍃 **MONGODB**

### **¿Qué es MongoDB?**
MongoDB es una base de datos de documentos NoSQL que almacena datos en formato BSON (Binary JSON), permitiendo estructuras de datos flexibles y anidadas.

### **Características Principales**
- **Documentos BSON**: Estructura similar a JSON con tipos de datos adicionales
- **Colecciones**: Agrupación de documentos (equivalente a tablas)
- **Índices**: Optimización de consultas
- **Agregación**: Pipeline de procesamiento de datos
- **Sharding**: Distribución horizontal de datos

### **Instalación y Configuración**
```bash
# Instalar MongoDB
npm install mongodb mongoose

# Conectar a MongoDB
const { MongoClient } = require('mongodb');
const mongoose = require('mongoose');

// Conexión con MongoDB nativo
const client = new MongoClient('mongodb://localhost:27017');

// Conexión con Mongoose
mongoose.connect('mongodb://localhost:27017/myapp', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```

### **Estructura de Datos en MongoDB**
```javascript
// Documento de usuario
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30,
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zipCode": "10001"
  },
  "hobbies": ["reading", "gaming", "cooking"],
  "createdAt": ISODate("2024-01-15T10:30:00Z"),
  "isActive": true
}

// Documento de post
{
  "_id": ObjectId("507f1f77bcf86cd799439012"),
  "title": "Introduction to MongoDB",
  "content": "MongoDB is a NoSQL database...",
  "author": {
    "id": ObjectId("507f1f77bcf86cd799439011"),
    "name": "John Doe"
  },
  "tags": ["mongodb", "nosql", "database"],
  "comments": [
    {
      "id": ObjectId("507f1f77bcf86cd799439013"),
      "text": "Great article!",
      "author": "Jane Smith",
      "createdAt": ISODate("2024-01-15T11:00:00Z")
    }
  ],
  "createdAt": ISODate("2024-01-15T10:30:00Z"),
  "updatedAt": ISODate("2024-01-15T10:30:00Z")
}
```

### **Operaciones CRUD con MongoDB Nativo**
```javascript
const { MongoClient, ObjectId } = require('mongodb');

class UserService {
  constructor() {
    this.client = new MongoClient('mongodb://localhost:27017');
    this.db = null;
    this.collection = null;
  }

  async connect() {
    await this.client.connect();
    this.db = this.client.db('myapp');
    this.collection = this.db.collection('users');
  }

  // CREATE - Crear usuario
  async createUser(userData) {
    const result = await this.collection.insertOne({
      ...userData,
      createdAt: new Date(),
      updatedAt: new Date()
    });
    
    return {
      id: result.insertedId,
      ...userData
    };
  }

  // READ - Obtener usuarios
  async getUsers(filter = {}, options = {}) {
    const { limit = 10, skip = 0, sort = { createdAt: -1 } } = options;
    
    const cursor = this.collection
      .find(filter)
      .sort(sort)
      .skip(skip)
      .limit(limit);
    
    return await cursor.toArray();
  }

  // READ - Obtener usuario por ID
  async getUserById(id) {
    return await this.collection.findOne({ _id: new ObjectId(id) });
  }

  // UPDATE - Actualizar usuario
  async updateUser(id, updateData) {
    const result = await this.collection.updateOne(
      { _id: new ObjectId(id) },
      { 
        $set: { 
          ...updateData, 
          updatedAt: new Date() 
        } 
      }
    );
    
    return result.modifiedCount > 0;
  }

  // DELETE - Eliminar usuario
  async deleteUser(id) {
    const result = await this.collection.deleteOne({ _id: new ObjectId(id) });
    return result.deletedCount > 0;
  }

  // Búsqueda avanzada
  async searchUsers(query) {
    const filter = {
      $or: [
        { name: { $regex: query, $options: 'i' } },
        { email: { $regex: query, $options: 'i' } }
      ]
    };
    
    return await this.collection.find(filter).toArray();
  }

  // Agregación
  async getUserStats() {
    const pipeline = [
      {
        $group: {
          _id: null,
          totalUsers: { $sum: 1 },
          activeUsers: { $sum: { $cond: ['$isActive', 1, 0] } },
          avgAge: { $avg: '$age' }
        }
      }
    ];
    
    const result = await this.collection.aggregate(pipeline).toArray();
    return result[0] || { totalUsers: 0, activeUsers: 0, avgAge: 0 };
  }
}
```

### **Mongoose ODM**
```javascript
const mongoose = require('mongoose');

// Definir esquema de usuario
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: [2, 'Name must be at least 2 characters'],
    maxlength: [50, 'Name cannot exceed 50 characters']
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    match: [/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/, 'Invalid email format']
  },
  age: {
    type: Number,
    min: [13, 'Age must be at least 13'],
    max: [120, 'Age cannot exceed 120']
  },
  address: {
    street: String,
    city: String,
    zipCode: String,
    country: {
      type: String,
      default: 'USA'
    }
  },
  hobbies: [{
    type: String,
    enum: ['reading', 'gaming', 'cooking', 'sports', 'music']
  }],
  isActive: {
    type: Boolean,
    default: true
  },
  role: {
    type: String,
    enum: ['user', 'moderator', 'admin'],
    default: 'user'
  }
}, {
  timestamps: true, // Añade createdAt y updatedAt automáticamente
  toJSON: { virtuals: true },
  toObject: { virtuals: true }
});

// Índices
userSchema.index({ email: 1 });
userSchema.index({ name: 'text', email: 'text' }); // Índice de texto
userSchema.index({ createdAt: -1 }); // Índice compuesto

// Virtuals (campos calculados)
userSchema.virtual('fullAddress').get(function() {
  if (this.address) {
    return `${this.address.street}, ${this.address.city}, ${this.address.zipCode}`;
  }
  return '';
});

// Middleware (hooks)
userSchema.pre('save', function(next) {
  // Capitalizar nombre antes de guardar
  if (this.name) {
    this.name = this.name.charAt(0).toUpperCase() + this.name.slice(1).toLowerCase();
  }
  next();
});

userSchema.post('save', function(doc) {
  console.log(`User ${doc.name} saved successfully`);
});

// Métodos de instancia
userSchema.methods.getAgeInYears = function() {
  return Math.floor((Date.now() - this.createdAt) / (1000 * 60 * 60 * 24 * 365));
};

userSchema.methods.isAdult = function() {
  return this.age >= 18;
};

// Métodos estáticos
userSchema.statics.findByRole = function(role) {
  return this.find({ role: role });
};

userSchema.statics.findActiveUsers = function() {
  return this.find({ isActive: true });
};

// Crear modelo
const User = mongoose.model('User', userSchema);

// Uso del modelo
class UserService {
  // Crear usuario
  async createUser(userData) {
    try {
      const user = new User(userData);
      await user.save();
      return user;
    } catch (error) {
      if (error.code === 11000) {
        throw new Error('Email already exists');
      }
      throw error;
    }
  }

  // Obtener usuarios con paginación y filtros
  async getUsers(options = {}) {
    const { page = 1, limit = 10, sort = '-createdAt', search, role, isActive } = options;
    
    const filter = {};
    
    if (search) {
      filter.$text = { $search: search };
    }
    
    if (role) {
      filter.role = role;
    }
    
    if (typeof isActive === 'boolean') {
      filter.isActive = isActive;
    }
    
    const skip = (page - 1) * limit;
    
    const [users, total] = await Promise.all([
      User.find(filter)
        .sort(sort)
        .skip(skip)
        .limit(limit)
        .select('-__v'),
      User.countDocuments(filter)
    ]);
    
    return {
      users,
      pagination: {
        page,
        limit,
        total,
        pages: Math.ceil(total / limit)
      }
    };
  }

  // Actualizar usuario
  async updateUser(id, updateData) {
    const user = await User.findByIdAndUpdate(
      id,
      updateData,
      { 
        new: true, // Retorna documento actualizado
        runValidators: true // Ejecuta validaciones
      }
    );
    
    if (!user) {
      throw new Error('User not found');
    }
    
    return user;
  }

  // Eliminar usuario
  async deleteUser(id) {
    const user = await User.findByIdAndDelete(id);
    
    if (!user) {
      throw new Error('User not found');
    }
    
    return user;
  }

  // Búsqueda por texto
  async searchUsers(query) {
    return await User.find(
      { $text: { $search: query } },
      { score: { $meta: 'textScore' } }
    ).sort({ score: { $meta: 'textScore' } });
  }

  // Agregación compleja
  async getUserStats() {
    const pipeline = [
      {
        $group: {
          _id: '$role',
          count: { $sum: 1 },
          avgAge: { $avg: '$age' },
          activeUsers: { $sum: { $cond: ['$isActive', 1, 0] } }
        }
      },
      {
        $sort: { count: -1 }
      }
    ];
    
    return await User.aggregate(pipeline);
  }
}
```

---

## 🔴 **REDIS**

### **¿Qué es Redis?**
Redis es una base de datos en memoria de tipo clave-valor que se destaca por su velocidad y versatilidad. Es ideal para caché, sesiones, colas de mensajes y almacenamiento temporal.

### **Características Principales**
- **In-Memory**: Datos almacenados en RAM para máximo rendimiento
- **Persistencia**: Opciones para guardar datos en disco
- **Estructuras de Datos**: Strings, hashes, lists, sets, sorted sets
- **Pub/Sub**: Sistema de mensajería
- **Transacciones**: Operaciones atómicas
- **Lua Scripting**: Scripts personalizados

### **Instalación y Configuración**
```bash
# Instalar Redis
npm install redis ioredis

# Conectar a Redis
const Redis = require('ioredis');

const redis = new Redis({
  host: 'localhost',
  port: 6379,
  password: 'your-password', // si está configurado
  db: 0,
  retryDelayOnFailover: 100,
  maxRetriesPerRequest: 3
});
```

### **Operaciones Básicas con Redis**
```javascript
class RedisService {
  constructor() {
    this.redis = new Redis({
      host: 'localhost',
      port: 6379
    });
  }

  // Strings
  async setString(key, value, expireSeconds = null) {
    if (expireSeconds) {
      await this.redis.setex(key, expireSeconds, value);
    } else {
      await this.redis.set(key, value);
    }
  }

  async getString(key) {
    return await this.redis.get(key);
  }

  async deleteKey(key) {
    return await this.redis.del(key);
  }

  // Hashes
  async setHash(key, field, value) {
    await this.redis.hset(key, field, value);
  }

  async getHash(key, field) {
    return await this.redis.hget(key, field);
  }

  async getAllHash(key) {
    return await this.redis.hgetall(key);
  }

  async deleteHashField(key, field) {
    return await this.redis.hdel(key, field);
  }

  // Lists
  async pushToList(key, value, side = 'right') {
    if (side === 'left') {
      return await this.redis.lpush(key, value);
    } else {
      return await this.redis.rpush(key, value);
    }
  }

  async popFromList(key, side = 'right') {
    if (side === 'left') {
      return await this.redis.lpop(key);
    } else {
      return await this.redis.rpop(key);
    }
  }

  async getListRange(key, start = 0, stop = -1) {
    return await this.redis.lrange(key, start, stop);
  }

  // Sets
  async addToSet(key, member) {
    return await this.redis.sadd(key, member);
  }

  async removeFromSet(key, member) {
    return await this.redis.srem(key, member);
  }

  async isSetMember(key, member) {
    return await this.redis.sismember(key, member);
  }

  async getSetMembers(key) {
    return await this.redis.smembers(key);
  }

  // Sorted Sets
  async addToSortedSet(key, score, member) {
    return await this.redis.zadd(key, score, member);
  }

  async getSortedSetRange(key, start = 0, stop = -1, withScores = false) {
    if (withScores) {
      return await this.redis.zrange(key, start, stop, 'WITHSCORES');
    } else {
      return await this.redis.zrange(key, start, stop);
    }
  }

  async getSortedSetScore(key, member) {
    return await this.redis.zscore(key, member);
  }

  // Expiration
  async setExpiration(key, seconds) {
    return await this.redis.expire(key, seconds);
  }

  async getTimeToLive(key) {
    return await this.redis.ttl(key);
  }

  async removeExpiration(key) {
    return await this.redis.persist(key);
  }
}
```

### **Casos de Uso Comunes**

#### **1. Caché de Datos**
```javascript
class CacheService {
  constructor(redisService) {
    this.redis = redisService;
    this.defaultTTL = 3600; // 1 hora
  }

  // Caché simple
  async get(key) {
    try {
      const cached = await this.redis.getString(key);
      return cached ? JSON.parse(cached) : null;
    } catch (error) {
      console.error('Cache get error:', error);
      return null;
    }
  }

  async set(key, value, ttl = this.defaultTTL) {
    try {
      await this.redis.setString(key, JSON.stringify(value), ttl);
      return true;
    } catch (error) {
      console.error('Cache set error:', error);
      return false;
    }
  }

  // Caché con tags para invalidación
  async setWithTags(key, value, tags = [], ttl = this.defaultTTL) {
    try {
      const cacheData = {
        value: value,
        tags: tags,
        timestamp: Date.now()
      };
      
      await this.redis.setString(key, JSON.stringify(cacheData), ttl);
      
      // Guardar tags para invalidación
      for (const tag of tags) {
        await this.redis.addToSet(`tag:${tag}`, key);
      }
      
      return true;
    } catch (error) {
      console.error('Cache set with tags error:', error);
      return false;
    }
  }

  async invalidateByTag(tag) {
    try {
      const keys = await this.redis.getSetMembers(`tag:${tag}`);
      
      if (keys.length > 0) {
        await this.redis.redis.del(...keys);
        await this.redis.redis.del(`tag:${tag}`);
      }
      
      return keys.length;
    } catch (error) {
      console.error('Cache invalidate by tag error:', error);
      return 0;
    }
  }
}
```

#### **2. Sesiones de Usuario**
```javascript
class SessionService {
  constructor(redisService) {
    this.redis = redisService;
    this.sessionTTL = 86400; // 24 horas
  }

  async createSession(userId, userData) {
    const sessionId = this.generateSessionId();
    const sessionData = {
      userId: userId,
      userData: userData,
      createdAt: Date.now(),
      lastActivity: Date.now()
    };
    
    await this.redis.setString(
      `session:${sessionId}`,
      JSON.stringify(sessionData),
      this.sessionTTL
    );
    
    return sessionId;
  }

  async getSession(sessionId) {
    try {
      const sessionData = await this.redis.getString(`session:${sessionId}`);
      
      if (!sessionData) {
        return null;
      }
      
      const session = JSON.parse(sessionData);
      
      // Actualizar última actividad
      session.lastActivity = Date.now();
      await this.redis.setString(
        `session:${sessionId}`,
        JSON.stringify(session),
        this.sessionTTL
      );
      
      return session;
    } catch (error) {
      console.error('Session get error:', error);
      return null;
    }
  }

  async destroySession(sessionId) {
    try {
      await this.redis.deleteKey(`session:${sessionId}`);
      return true;
    } catch (error) {
      console.error('Session destroy error:', error);
      return false;
    }
  }

  async refreshSession(sessionId) {
    try {
      const session = await this.getSession(sessionId);
      
      if (session) {
        await this.redis.setString(
          `session:${sessionId}`,
          JSON.stringify(session),
          this.sessionTTL
        );
        return true;
      }
      
      return false;
    } catch (error) {
      console.error('Session refresh error:', error);
      return false;
    }
  }

  generateSessionId() {
    return require('crypto').randomBytes(32).toString('hex');
  }
}
```

#### **3. Rate Limiting**
```javascript
class RateLimitService {
  constructor(redisService) {
    this.redis = redisService;
  }

  async checkRateLimit(key, maxRequests, windowSeconds) {
    try {
      const current = await this.redis.redis.incr(key);
      
      if (current === 1) {
        await this.redis.setExpiration(key, windowSeconds);
      }
      
      if (current > maxRequests) {
        return {
          allowed: false,
          remaining: 0,
          resetTime: await this.redis.getTimeToLive(key)
        };
      }
      
      return {
        allowed: true,
        remaining: maxRequests - current,
        resetTime: await this.redis.getTimeToLive(key)
      };
    } catch (error) {
      console.error('Rate limit check error:', error);
      return { allowed: true, remaining: maxRequests, resetTime: 0 };
    }
  }

  async getRateLimitInfo(key) {
    try {
      const current = await this.redis.redis.get(key);
      const ttl = await this.redis.getTimeToLive(key);
      
      return {
        current: parseInt(current) || 0,
        resetTime: ttl
      };
    } catch (error) {
      console.error('Rate limit info error:', error);
      return { current: 0, resetTime: 0 };
    }
  }
}
```

---

## 🔄 **INTEGRACIÓN CON EXPRESS.JS**

### **Middleware de Caché**
```javascript
const cacheMiddleware = (ttl = 300) => {
  return async (req, res, next) => {
    try {
      const cacheKey = `cache:${req.originalUrl}`;
      const cached = await cacheService.get(cacheKey);
      
      if (cached) {
        return res.json(cached);
      }
      
      // Interceptar response para cachear
      const originalSend = res.send;
      res.send = function(data) {
        cacheService.set(cacheKey, data, ttl);
        originalSend.call(this, data);
      };
      
      next();
    } catch (error) {
      next(error);
    }
  };
};

// Uso
app.get('/users', cacheMiddleware(600), async (req, res) => {
  // Response será cacheada por 10 minutos
});
```

### **Middleware de Rate Limiting**
```javascript
const rateLimitMiddleware = (maxRequests = 100, windowSeconds = 900) => {
  return async (req, res, next) => {
    try {
      const key = `rate_limit:${req.ip}`;
      const result = await rateLimitService.checkRateLimit(key, maxRequests, windowSeconds);
      
      if (!result.allowed) {
        return res.status(429).json({
          success: false,
          error: {
            code: 'RATE_LIMIT_EXCEEDED',
            message: 'Too many requests',
            resetTime: result.resetTime
          }
        });
      }
      
      // Añadir headers de rate limit
      res.set({
        'X-RateLimit-Limit': maxRequests,
        'X-RateLimit-Remaining': result.remaining,
        'X-RateLimit-Reset': result.resetTime
      });
      
      next();
    } catch (error) {
      next(error);
    }
  };
};

// Uso
app.use('/api/', rateLimitMiddleware(100, 900)); // 100 requests por 15 minutos
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: CRUD con MongoDB**
Implementa operaciones CRUD completas para una entidad de tu elección.

### **Ejercicio 2: Sistema de Caché con Redis**
Crea un sistema de caché para mejorar el rendimiento de tu API.

### **Ejercicio 3: Sesiones con Redis**
Implementa un sistema de sesiones de usuario usando Redis.

### **Ejercicio 4: Rate Limiting**
Crea un sistema de rate limiting para proteger tu API.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [MongoDB Documentation](https://docs.mongodb.com/)
- [Redis Documentation](https://redis.io/documentation)
- [Mongoose Documentation](https://mongoosejs.com/docs/)

### **Herramientas Recomendadas**
- [MongoDB Compass](https://www.mongodb.com/products/compass) - GUI para MongoDB
- [Redis Commander](https://github.com/joeferner/redis-commander) - GUI para Redis
- [Studio 3T](https://studio3t.com/) - MongoDB IDE

### **Libros y Cursos**
- "MongoDB: The Definitive Guide" - Shannon Bradshaw
- "Redis in Action" - Josiah L. Carlson
- MongoDB University (Gratuito)

---

*Las bases de datos NoSQL son esenciales para aplicaciones modernas que requieren flexibilidad, escalabilidad y alto rendimiento. MongoDB y Redis son herramientas poderosas que complementan perfectamente el stack de Node.js.* 