# 🏗️ Microservicios: Arquitectura Distribuida y Escalable

## 🎯 **OBJETIVO**
Dominar la arquitectura de microservicios, comprendiendo patrones de comunicación, deployment, testing y cómo diseñar sistemas distribuidos escalables y mantenibles.

---

## 📋 **¿QUÉ SON LOS MICROSERVICIOS?**

### **Definición**
Los microservicios son un patrón de arquitectura donde una aplicación se divide en servicios más pequeños, independientes y autónomos, cada uno ejecutándose en su propio proceso y comunicándose a través de mecanismos ligeros como HTTP/REST o mensajería.

### **Características Principales**
- **Independencia**: Cada servicio puede ser desarrollado, desplegado y escalado independientemente
- **Autonomía**: Los servicios manejan su propia base de datos y lógica de negocio
- **Tecnología diversa**: Diferentes servicios pueden usar diferentes tecnologías
- **Resiliencia**: Fallos en un servicio no afectan a otros
- **Escalabilidad**: Cada servicio puede escalar independientemente

### **Comparación con Monolito**
```
MONOLITO:
┌─────────────────────────────────────┐
│           Aplicación                │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│  │ Users   │ │ Posts   │ │ Orders  │ │
│  │ Module  │ │ Module  │ │ Module  │ │
│  └─────────┘ └─────────┘ └─────────┘ │
│           Base de Datos              │
└─────────────────────────────────────┘

MICROSERVICIOS:
┌─────────┐ ┌─────────┐ ┌─────────┐
│ Users   │ │ Posts   │ │ Orders  │
│ Service │ │ Service │ │ Service │
│         │ │         │ │         │
│   DB    │ │   DB    │ │   DB    │
└─────────┘ └─────────┘ └─────────┘
    │           │           │
    └───────────┼───────────┘
                │
        ┌───────▼───────┐
        │   Gateway     │
        │   / Router    │
        └───────────────┘
```

---

## 🏗️ **ARQUITECTURA DE MICROSERVICIOS**

### **Componentes Principales**
```
┌─────────────────────────────────────────────────────────────┐
│                    API Gateway                              │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │   Users     │ │   Posts     │ │   Orders    │          │
│  │  Service    │ │  Service    │ │  Service    │          │
│  │             │ │             │ │             │          │
│  │   Port:     │ │   Port:     │ │   Port:     │          │
│  │    3001     │ │    3002     │ │    3003     │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │   Users     │ │   Posts     │ │   Orders    │          │
│  │     DB      │ │     DB      │ │     DB      │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐          │
│  │  Service    │ │  Service    │ │  Service    │          │
│  │ Discovery   │ │ Registry    │ │  Config     │          │
│  └─────────────┘ └─────────────┘ └─────────────┘          │
└─────────────────────────────────────────────────────────────┘
```

### **Patrones de Arquitectura**

#### **1. API Gateway Pattern**
```javascript
// API Gateway con Express
const express = require('express');
const { createProxyMiddleware } = require('http-proxy-middleware');
const rateLimit = require('express-rate-limit');
const cors = require('cors');

const app = express();

// Middleware de seguridad
app.use(cors());
app.use(express.json());

// Rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // máximo 100 requests por ventana
});
app.use(limiter);

// Proxy a microservicios
app.use('/api/users', createProxyMiddleware({
  target: 'http://localhost:3001',
  changeOrigin: true,
  pathRewrite: {
    '^/api/users': '/'
  }
}));

app.use('/api/posts', createProxyMiddleware({
  target: 'http://localhost:3002',
  changeOrigin: true,
  pathRewrite: {
    '^/api/posts': '/'
  }
}));

app.use('/api/orders', createProxyMiddleware({
  target: 'http://localhost:3003',
  changeOrigin: true,
  pathRewrite: {
    '^/api/orders': '/'
  }
}));

// Middleware de logging
app.use((req, res, next) => {
  console.log(`${new Date().toISOString()} - ${req.method} ${req.path}`);
  next();
});

// Middleware de manejo de errores
app.use((err, req, res, next) => {
  console.error('Gateway Error:', err);
  res.status(500).json({
    error: 'Internal Gateway Error',
    message: err.message
  });
});

app.listen(3000, () => {
  console.log('API Gateway running on port 3000');
});
```

#### **2. Service Discovery Pattern**
```javascript
// Service Registry con Redis
const Redis = require('ioredis');
const express = require('express');

class ServiceRegistry {
  constructor() {
    this.redis = new Redis();
    this.app = express();
    this.setupRoutes();
  }

  setupRoutes() {
    // Registrar servicio
    this.app.post('/register', async (req, res) => {
      try {
        const { serviceName, serviceUrl, healthCheckUrl } = req.body;
        
        const serviceInfo = {
          name: serviceName,
          url: serviceUrl,
          healthCheckUrl,
          registeredAt: Date.now(),
          lastHeartbeat: Date.now()
        };
        
        await this.redis.hset(
          'services',
          serviceName,
          JSON.stringify(serviceInfo)
        );
        
        res.json({
          success: true,
          message: `Service ${serviceName} registered successfully`
        });
      } catch (error) {
        res.status(500).json({
          success: false,
          error: error.message
        });
      }
    });

    // Obtener servicios
    this.app.get('/services', async (req, res) => {
      try {
        const services = await this.redis.hgetall('services');
        const serviceList = Object.values(services).map(service => 
          JSON.parse(service)
        );
        
        res.json({
          success: true,
          services: serviceList
        });
      } catch (error) {
        res.status(500).json({
          success: false,
          error: error.message
        });
      }
    });

    // Heartbeat
    this.app.post('/heartbeat/:serviceName', async (req, res) => {
      try {
        const { serviceName } = req.params;
        const serviceData = await this.redis.hget('services', serviceName);
        
        if (serviceData) {
          const service = JSON.parse(serviceData);
          service.lastHeartbeat = Date.now();
          
          await this.redis.hset(
            'services',
            serviceName,
            JSON.stringify(service)
          );
          
          res.json({ success: true });
        } else {
          res.status(404).json({
            success: false,
            error: 'Service not found'
          });
        }
      } catch (error) {
        res.status(500).json({
          success: false,
          error: error.message
        });
      }
    });

    // Desregistrar servicio
    this.app.delete('/unregister/:serviceName', async (req, res) => {
      try {
        const { serviceName } = req.params;
        await this.redis.hdel('services', serviceName);
        
        res.json({
          success: true,
          message: `Service ${serviceName} unregistered successfully`
        });
      } catch (error) {
        res.status(500).json({
          success: false,
          error: error.message
        });
      }
    });
  }

  // Limpiar servicios inactivos
  async cleanupInactiveServices() {
    const services = await this.redis.hgetall('services');
    const now = Date.now();
    const timeout = 5 * 60 * 1000; // 5 minutos
    
    for (const [serviceName, serviceData] of Object.entries(services)) {
      const service = JSON.parse(serviceData);
      
      if (now - service.lastHeartbeat > timeout) {
        await this.redis.hdel('services', serviceName);
        console.log(`Service ${serviceName} removed due to inactivity`);
      }
    }
  }

  start() {
    this.app.listen(4000, () => {
      console.log('Service Registry running on port 4000');
    });
    
    // Limpiar servicios inactivos cada minuto
    setInterval(() => {
      this.cleanupInactiveServices();
    }, 60 * 1000);
  }
}

// Iniciar registry
const registry = new ServiceRegistry();
registry.start();
```

---

## 📡 **PATRONES DE COMUNICACIÓN**

### **1. Synchronous Communication (HTTP/REST)**
```javascript
// Cliente HTTP para comunicación síncrona
const axios = require('axios');

class UserServiceClient {
  constructor(baseURL) {
    this.client = axios.create({
      baseURL,
      timeout: 5000,
      headers: {
        'Content-Type': 'application/json'
      }
    });
  }

  async getUserById(userId) {
    try {
      const response = await this.client.get(`/users/${userId}`);
      return response.data;
    } catch (error) {
      if (error.response?.status === 404) {
        throw new Error('User not found');
      }
      throw new Error(`Error fetching user: ${error.message}`);
    }
  }

  async createUser(userData) {
    try {
      const response = await this.client.post('/users', userData);
      return response.data;
    } catch (error) {
      throw new Error(`Error creating user: ${error.message}`);
    }
  }

  async updateUser(userId, updateData) {
    try {
      const response = await this.client.put(`/users/${userId}`, updateData);
      return response.data;
    } catch (error) {
      throw new Error(`Error updating user: ${error.message}`);
    }
  }

  async deleteUser(userId) {
    try {
      const response = await this.client.delete(`/users/${userId}`);
      return response.data;
    } catch (error) {
      throw new Error(`Error deleting user: ${error.message}`);
    }
  }
}

// Uso en otro servicio
const userServiceClient = new UserServiceClient('http://localhost:3001');

// En el servicio de posts
app.get('/posts/:id', async (req, res) => {
  try {
    const post = await Post.findById(req.params.id);
    
    if (!post) {
      return res.status(404).json({ error: 'Post not found' });
    }
    
    // Obtener información del autor
    const author = await userServiceClient.getUserById(post.authorId);
    
    res.json({
      ...post.toObject(),
      author: {
        id: author.id,
        name: author.name,
        email: author.email
      }
    });
    
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

### **2. Asynchronous Communication (Message Queues)**
```javascript
// RabbitMQ para comunicación asíncrona
const amqp = require('amqplib');

class MessageBroker {
  constructor() {
    this.connection = null;
    this.channel = null;
  }

  async connect() {
    try {
      this.connection = await amqp.connect('amqp://localhost');
      this.channel = await this.connection.createChannel();
      
      // Declarar exchanges
      await this.channel.assertExchange('user_events', 'topic', { durable: true });
      await this.channel.assertExchange('post_events', 'topic', { durable: true });
      
      console.log('Connected to RabbitMQ');
    } catch (error) {
      console.error('Error connecting to RabbitMQ:', error);
      throw error;
    }
  }

  async publishUserEvent(eventType, userData) {
    try {
      const message = {
        eventType,
        timestamp: new Date().toISOString(),
        data: userData
      };
      
      await this.channel.publish(
        'user_events',
        eventType,
        Buffer.from(JSON.stringify(message)),
        { persistent: true }
      );
      
      console.log(`Published user event: ${eventType}`);
    } catch (error) {
      console.error('Error publishing user event:', error);
      throw error;
    }
  }

  async publishPostEvent(eventType, postData) {
    try {
      const message = {
        eventType,
        timestamp: new Date().toISOString(),
        data: postData
      };
      
      await this.channel.publish(
        'post_events',
        eventType,
        Buffer.from(JSON.stringify(message)),
        { persistent: true }
      );
      
      console.log(`Published post event: ${eventType}`);
    } catch (error) {
      console.error('Error publishing post event:', error);
      throw error;
    }
  }

  async subscribeToUserEvents(serviceName, eventTypes, handler) {
    try {
      const queue = await this.channel.assertQueue('', { exclusive: true });
      
      // Bind a eventos específicos
      for (const eventType of eventTypes) {
        await this.channel.bindQueue(queue.queue, 'user_events', eventType);
      }
      
      // Consumir mensajes
      await this.channel.consume(queue.queue, async (msg) => {
        if (msg) {
          try {
            const message = JSON.parse(msg.content.toString());
            console.log(`${serviceName} received user event: ${message.eventType}`);
            
            await handler(message);
            
            this.channel.ack(msg);
          } catch (error) {
            console.error('Error processing message:', error);
            this.channel.nack(msg, false, true); // Requeue message
          }
        }
      });
      
      console.log(`${serviceName} subscribed to user events: ${eventTypes.join(', ')}`);
    } catch (error) {
      console.error('Error subscribing to user events:', error);
      throw error;
    }
  }

  async close() {
    if (this.channel) {
      await this.channel.close();
    }
    if (this.connection) {
      await this.connection.close();
    }
  }
}

// Uso en el servicio de usuarios
const messageBroker = new MessageBroker();

// Conectar al broker
await messageBroker.connect();

// Publicar eventos cuando ocurren cambios
app.post('/users', async (req, res) => {
  try {
    const user = await User.create(req.body);
    
    // Publicar evento de usuario creado
    await messageBroker.publishUserEvent('user.created', {
      userId: user._id,
      username: user.username,
      email: user.email
    });
    
    res.status(201).json(user);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Uso en el servicio de posts (subscriber)
const postServiceBroker = new MessageBroker();
await postServiceBroker.connect();

// Suscribirse a eventos de usuario
await postServiceBroker.subscribeToUserEvents(
  'PostService',
  ['user.created', 'user.updated', 'user.deleted'],
  async (message) => {
    switch (message.eventType) {
      case 'user.created':
        console.log('New user created, updating post service cache');
        // Actualizar cache local
        break;
        
      case 'user.updated':
        console.log('User updated, refreshing post service cache');
        // Refrescar cache local
        break;
        
      case 'user.deleted':
        console.log('User deleted, cleaning up related posts');
        // Limpiar posts del usuario
        await Post.deleteMany({ authorId: message.data.userId });
        break;
    }
  }
);
```

---

## 🐳 **CONTAINERIZACIÓN Y DEPLOYMENT**

### **Docker para Microservicios**
```dockerfile
# Dockerfile para el servicio de usuarios
FROM node:18-alpine

WORKDIR /app

# Copiar package files
COPY package*.json ./

# Instalar dependencias
RUN npm ci --only=production

# Copiar código fuente
COPY . .

# Exponer puerto
EXPOSE 3001

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3001/health || exit 1

# Comando de inicio
CMD ["npm", "start"]
```

### **Docker Compose para Desarrollo**
```yaml
# docker-compose.yml
version: '3.8'

services:
  # API Gateway
  api-gateway:
    build: ./api-gateway
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - USERS_SERVICE_URL=http://users-service:3001
      - POSTS_SERVICE_URL=http://posts-service:3002
      - ORDERS_SERVICE_URL=http://orders-service:3003
    depends_on:
      - users-service
      - posts-service
      - orders-service
    networks:
      - microservices-network

  # Users Service
  users-service:
    build: ./users-service
    ports:
      - "3001:3001"
    environment:
      - NODE_ENV=development
      - PORT=3001
      - MONGODB_URI=mongodb://users-db:27017/users
      - REDIS_URL=redis://redis:6379
    depends_on:
      - users-db
      - redis
    networks:
      - microservices-network

  # Posts Service
  posts-service:
    build: ./posts-service
    ports:
      - "3002:3002"
    environment:
      - NODE_ENV=development
      - PORT=3002
      - MONGODB_URI=mongodb://posts-db:27017/posts
      - REDIS_URL=redis://redis:6379
      - USERS_SERVICE_URL=http://users-service:3001
    depends_on:
      - posts-db
      - redis
      - users-service
    networks:
      - microservices-network

  # Orders Service
  orders-service:
    build: ./orders-service
    ports:
      - "3003:3003"
    environment:
      - NODE_ENV=development
      - PORT=3003
      - MONGODB_URI=mongodb://orders-db:27017/orders
      - REDIS_URL=redis://redis:6379
      - USERS_SERVICE_URL=http://users-service:3001
      - POSTS_SERVICE_URL=http://posts-service:3002
    depends_on:
      - orders-db
      - redis
      - users-service
      - posts-service
    networks:
      - microservices-network

  # Service Registry
  service-registry:
    build: ./service-registry
    ports:
      - "4000:4000"
    environment:
      - NODE_ENV=development
      - PORT=4000
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis
    networks:
      - microservices-network

  # Message Broker
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      - RABBITMQ_DEFAULT_USER=admin
      - RABBITMQ_DEFAULT_PASS=admin
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq
    networks:
      - microservices-network

  # Databases
  users-db:
    image: mongo:6
    ports:
      - "27017:27017"
    volumes:
      - users_data:/data/db
    networks:
      - microservices-network

  posts-db:
    image: mongo:6
    ports:
      - "27018:27017"
    volumes:
      - posts_data:/data/db
    networks:
      - microservices-network

  orders-db:
    image: mongo:6
    ports:
      - "27019:27017"
    volumes:
      - orders_data:/data/db
    networks:
      - microservices-network

  # Redis
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - microservices-network

volumes:
  users_data:
  posts_data:
  orders_data:
  redis_data:
  rabbitmq_data:

networks:
  microservices-network:
    driver: bridge
```

---

## 🧪 **TESTING DE MICROSERVICIOS**

### **Testing Estratégico**
```javascript
// Testing unitario para el servicio de usuarios
const { expect } = require('chai');
const sinon = require('sinon');
const UserService = require('../services/UserService');
const User = require('../models/User');

describe('UserService', () => {
  let userService;
  let userStub;

  beforeEach(() => {
    userService = new UserService();
    userStub = sinon.stub(User);
  });

  afterEach(() => {
    sinon.restore();
  });

  describe('createUser', () => {
    it('should create a user successfully', async () => {
      const userData = {
        username: 'testuser',
        email: 'test@example.com',
        password: 'password123'
      };

      const expectedUser = {
        _id: 'user123',
        ...userData,
        createdAt: new Date()
      };

      userStub.create.resolves(expectedUser);

      const result = await userService.createUser(userData);

      expect(result).to.deep.equal(expectedUser);
      expect(userStub.create.calledOnceWith(userData)).to.be.true;
    });

    it('should throw error if user already exists', async () => {
      const userData = {
        username: 'existinguser',
        email: 'existing@example.com',
        password: 'password123'
      };

      userStub.findOne.resolves({ username: 'existinguser' });

      try {
        await userService.createUser(userData);
        expect.fail('Should have thrown an error');
      } catch (error) {
        expect(error.message).to.equal('User already exists');
      }
    });
  });

  describe('getUserById', () => {
    it('should return user if found', async () => {
      const userId = 'user123';
      const expectedUser = {
        _id: userId,
        username: 'testuser',
        email: 'test@example.com'
      };

      userStub.findById.resolves(expectedUser);

      const result = await userService.getUserById(userId);

      expect(result).to.deep.equal(expectedUser);
      expect(userStub.findById.calledOnceWith(userId)).to.be.true;
    });

    it('should return null if user not found', async () => {
      const userId = 'nonexistent';

      userStub.findById.resolves(null);

      const result = await userService.getUserById(userId);

      expect(result).to.be.null;
    });
  });
});

// Testing de integración
const request = require('supertest');
const app = require('../app');
const mongoose = require('mongoose');

describe('User API Integration', () => {
  before(async () => {
    await mongoose.connect(process.env.MONGODB_TEST_URI);
  });

  after(async () => {
    await mongoose.connection.close();
  });

  beforeEach(async () => {
    await User.deleteMany({});
  });

  describe('POST /users', () => {
    it('should create a new user', async () => {
      const userData = {
        username: 'testuser',
        email: 'test@example.com',
        password: 'password123'
      };

      const response = await request(app)
        .post('/users')
        .send(userData)
        .expect(201);

      expect(response.body).to.have.property('_id');
      expect(response.body.username).to.equal(userData.username);
      expect(response.body.email).to.equal(userData.email);
      expect(response.body).to.not.have.property('password');
    });

    it('should return 400 for invalid data', async () => {
      const invalidData = {
        username: 'test',
        email: 'invalid-email'
      };

      const response = await request(app)
        .post('/users')
        .send(invalidData)
        .expect(400);

      expect(response.body).to.have.property('errors');
    });
  });
});

// Testing de contratos (Consumer-Driven Contracts)
const pact = require('@pact-foundation/pact');
const { Consumer } = pact;

describe('User Service Contract', () => {
  const provider = new Consumer({
    consumer: 'PostsService',
    provider: 'UsersService'
  });

  describe('get user by id', () => {
    it('should return user data', async () => {
      await provider
        .given('a user exists')
        .uponReceiving('a request for user data')
        .withRequest({
          method: 'GET',
          path: '/users/123',
          headers: {
            'Accept': 'application/json'
          }
        })
        .willRespondWith({
          status: 200,
          headers: {
            'Content-Type': 'application/json'
          },
          body: {
            id: '123',
            username: 'testuser',
            email: 'test@example.com'
          }
        });

      await provider.verify();
    });
  });
});
```

---

## 📊 **MONITOREO Y OBSERVABILIDAD**

### **Logging Centralizado**
```javascript
// Winston logger con formato estructurado
const winston = require('winston');
const { format } = winston;

const logger = winston.createLogger({
  level: 'info',
  format: format.combine(
    format.timestamp(),
    format.errors({ stack: true }),
    format.json()
  ),
  defaultMeta: {
    service: process.env.SERVICE_NAME || 'unknown',
    version: process.env.SERVICE_VERSION || '1.0.0'
  },
  transports: [
    new winston.transports.Console({
      format: format.combine(
        format.colorize(),
        format.simple()
      )
    }),
    new winston.transports.File({
      filename: 'logs/error.log',
      level: 'error'
    }),
    new winston.transports.File({
      filename: 'logs/combined.log'
    })
  ]
});

// Middleware de logging para Express
const loggingMiddleware = (req, res, next) => {
  const startTime = Date.now();
  
  // Log del request
  logger.info('HTTP Request', {
    method: req.method,
    url: req.url,
    ip: req.ip,
    userAgent: req.get('User-Agent'),
    userId: req.user?.id,
    requestId: req.headers['x-request-id']
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
      requestId: req.headers['x-request-id']
    });
    
    originalSend.call(this, data);
  };
  
  next();
};

// Uso
app.use(loggingMiddleware);
```

### **Métricas y Health Checks**
```javascript
// Health check endpoint
app.get('/health', async (req, res) => {
  try {
    // Verificar base de datos
    const dbStatus = await checkDatabaseConnection();
    
    // Verificar dependencias externas
    const externalServicesStatus = await checkExternalServices();
    
    // Verificar recursos del sistema
    const systemStatus = checkSystemResources();
    
    const overallStatus = dbStatus && externalServicesStatus && systemStatus;
    
    res.status(overallStatus ? 200 : 503).json({
      status: overallStatus ? 'healthy' : 'unhealthy',
      timestamp: new Date().toISOString(),
      service: process.env.SERVICE_NAME,
      version: process.env.SERVICE_VERSION,
      checks: {
        database: dbStatus,
        externalServices: externalServicesStatus,
        system: systemStatus
      }
    });
  } catch (error) {
    res.status(503).json({
      status: 'unhealthy',
      error: error.message,
      timestamp: new Date().toISOString()
    });
  }
});

// Función para verificar base de datos
async function checkDatabaseConnection() {
  try {
    await mongoose.connection.db.admin().ping();
    return true;
  } catch (error) {
    logger.error('Database health check failed:', error);
    return false;
  }
}

// Función para verificar servicios externos
async function checkExternalServices() {
  try {
    const checks = await Promise.allSettled([
      checkService('http://users-service:3001/health'),
      checkService('http://posts-service:3002/health'),
      checkService('http://orders-service:3003/health')
    ]);
    
    return checks.every(check => check.status === 'fulfilled' && check.value);
  } catch (error) {
    logger.error('External services health check failed:', error);
    return false;
  }
}

async function checkService(url) {
  try {
    const response = await axios.get(url, { timeout: 5000 });
    return response.status === 200;
  } catch (error) {
    return false;
  }
}

// Función para verificar recursos del sistema
function checkSystemResources() {
  const memUsage = process.memoryUsage();
  const cpuUsage = process.cpuUsage();
  
  // Verificar uso de memoria (máximo 90%)
  const memoryUsagePercent = (memUsage.heapUsed / memUsage.heapTotal) * 100;
  if (memoryUsagePercent > 90) {
    logger.warn('High memory usage:', memoryUsagePercent);
    return false;
  }
  
  return true;
}
```

---

## 🚀 **DEPLOYMENT Y CI/CD**

### **GitHub Actions para CI/CD**
```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Run linting
      run: npm run lint
    
    - name: Run security audit
      run: npm audit --audit-level moderate

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    
    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: |
          ${{ secrets.DOCKER_USERNAME }}/users-service:latest
          ${{ secrets.DOCKER_USERNAME }}/users-service:${{ github.sha }}
        cache-from: type=gha
        cache-to: type=gha,mode=max

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Deploy to production
      run: |
        echo "Deploying to production..."
        # Aquí irían los comandos de deployment
        # Por ejemplo, kubectl apply, docker-compose up, etc.
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Diseño de Microservicios**
Diseña la arquitectura de microservicios para un sistema de e-commerce.

### **Ejercicio 2: Implementación de Servicios**
Implementa dos microservicios básicos con comunicación HTTP.

### **Ejercicio 3: Message Queue**
Integra RabbitMQ para comunicación asíncrona entre servicios.

### **Ejercicio 4: Deployment**
Configura Docker y CI/CD para tus microservicios.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [Microservices.io](https://microservices.io/) - Patrones y mejores prácticas
- [Docker Documentation](https://docs.docker.com/) - Containerización
- [Kubernetes Documentation](https://kubernetes.io/docs/) - Orquestación

### **Herramientas Recomendadas**
- [Docker Compose](https://docs.docker.com/compose/) - Orquestación local
- [Kubernetes](https://kubernetes.io/) - Orquestación en producción
- [Helm](https://helm.sh/) - Gestión de charts de Kubernetes

### **Libros y Cursos**
- "Building Microservices" - Sam Newman
- "Microservices Patterns" - Chris Richardson
- "Docker in Action" - Jeff Nickoloff

---

*Los microservicios representan una evolución natural en la arquitectura de software, permitiendo sistemas más flexibles, escalables y mantenibles. Dominar esta arquitectura te preparará para los desafíos de las aplicaciones modernas a gran escala.* 