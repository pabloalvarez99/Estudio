# ⚙️ Módulo 3: Desarrollo Web Backend

## 📚 Descripción del Módulo
Este módulo te prepara para construir la lógica del servidor, APIs robustas y sistemas de bases de datos. Aprenderás a crear aplicaciones web completas con Node.js y tecnologías modernas del backend.

## 🎯 Objetivos de Aprendizaje
- Desarrollar APIs RESTful y GraphQL con Node.js
- Diseñar y gestionar bases de datos relacionales y NoSQL
- Implementar autenticación y autorización seguras
- Crear arquitecturas escalables y mantenibles
- Desplegar aplicaciones en la nube

## ⏱️ Duración Estimada: 3 meses

---

## 📖 Contenido del Módulo

### **Semana 1-2: Fundamentos de Backend y Node.js**
#### **Arquitectura Web**
- Cliente-Servidor
- Modelo MVC
- API vs Server-Side Rendering
- Microservicios conceptos básicos
- REST vs GraphQL

#### **Node.js y JavaScript del Servidor**
- Runtime de Node.js
- Event loop y asincronía
- Módulos CommonJS y ES6
- NPM y gestión de dependencias
- Variables de entorno

#### **HTTP y APIs**
- Protocolo HTTP (métodos, headers, status codes)
- REST API principles
- JSON y serialización
- CORS y políticas de seguridad
- Rate limiting básico

#### **Proyecto Práctico**
- **API de Tareas**: Endpoints CRUD básicos
- **Servidor HTTP Simple**: Con routing manual

---

### **Semana 3-4: Express.js y Middleware**
#### **Framework Express.js**
- Configuración y estructura
- Routing y parámetros
- Middleware conceptos
- Error handling
- Template engines

#### **Middleware Personalizado**
- Authentication middleware
- Logging y monitoring
- Validation middleware
- Compression y caching
- Security headers

#### **Validación y Sanitización**
- Joi para validación
- Sanitización de inputs
- XSS prevention
- SQL injection protection
- Input validation patterns

#### **Proyecto Práctico**
- **Blog API**: Con middleware de autenticación
- **File Upload Service**: Con validación y sanitización

---

### **Semana 5-6: Bases de Datos Relacionales**
#### **SQL y PostgreSQL**
- Diseño de bases de datos
- Normalización básica
- Queries complejas (JOINs, subqueries)
- Índices y performance
- Transacciones y ACID

#### **ORM con Prisma/Sequelize**
- Modelos y migraciones
- Relaciones (1:1, 1:N, N:N)
- Queries optimizadas
- Seeders y factories
- Database versioning

#### **Database Design Patterns**
- Repository pattern
- Data Access Layer
- Connection pooling
- Query optimization
- Backup strategies

#### **Proyecto Práctico**
- **E-commerce Database**: Diseño completo con relaciones
- **User Management System**: Con roles y permisos

---

### **Semana 7-8: Bases de Datos NoSQL y Caching**
#### **MongoDB y Mongoose**
- Document databases
- Schemas y validación
- Aggregation pipeline
- Indexing strategies
- Replica sets conceptos

#### **Redis para Caching**
- In-memory caching
- Session storage
- Cache invalidation
- Redis patterns
- Performance optimization

#### **Database Selection**
- Cuándo usar SQL vs NoSQL
- Polyglot persistence
- Data modeling strategies
- Migration strategies
- Backup y recovery

#### **Proyecto Práctico**
- **Hybrid App**: SQL + NoSQL + Redis
- **Real-time Chat**: Con WebSockets y Redis

---

### **Semana 9-10: Autenticación y Seguridad**
#### **JWT y Session Management**
- JWT tokens (access/refresh)
- Session-based auth
- Password hashing (bcrypt)
- OAuth 2.0 básico
- Social login

#### **Seguridad Web**
- OWASP Top 10
- Helmet.js security headers
- Rate limiting avanzado
- CORS configuration
- Content Security Policy

#### **Testing de Seguridad**
- Penetration testing básico
- Vulnerability scanning
- Security headers testing
- Input validation testing
- Authentication testing

#### **Proyecto Práctico**
- **Secure API**: Con JWT, rate limiting y validación
- **OAuth Integration**: Con Google/GitHub login

---

### **Semana 11-12: Testing y Deployment**
#### **Testing del Backend**
- Jest para unit testing
- Supertest para integration testing
- Mocking y stubbing
- Test coverage
- CI/CD básico

#### **Deployment y DevOps**
- Environment management
- Docker containers
- PM2 process manager
- Nginx reverse proxy
- SSL certificates

#### **Monitoring y Logging**
- Winston para logging
- Error tracking (Sentry)
- Performance monitoring
- Health checks
- Metrics collection

#### **Proyecto Final del Módulo**
- **Full-Stack App**: Frontend + Backend + Database
- **Deployment**: En VPS o cloud platform

---

## 🛠️ Tecnologías y Herramientas

### **Stack Principal**
1. **Node.js**: Runtime de JavaScript
2. **Express.js**: Framework web
3. **PostgreSQL**: Base de datos relacional
4. **MongoDB**: Base de datos NoSQL
5. **Redis**: Caching y sesiones

### **Herramientas de Desarrollo**
- **VS Code**: Editor principal
- **Postman**: Testing de APIs
- **pgAdmin**: Gestión de PostgreSQL
- **MongoDB Compass**: Gestión de MongoDB
- **Redis Desktop Manager**: Gestión de Redis

### **Librerías y Frameworks**
- **Prisma/Sequelize**: ORM para SQL
- **Mongoose**: ODM para MongoDB
- **Joi**: Validación de datos
- **bcrypt**: Hashing de passwords
- **jsonwebtoken**: JWT tokens

---

## 📋 Checklist de Evaluación

### **Habilidades Técnicas**
- [ ] Crear APIs RESTful con Express.js
- [ ] Diseñar bases de datos relacionales
- [ ] Implementar autenticación JWT
- [ ] Gestionar bases de datos NoSQL
- [ ] Desplegar aplicaciones en producción

### **Proyectos Completados**
- [ ] API REST completa con autenticación
- [ ] Sistema de bases de datos híbrido
- [ ] Aplicación con WebSockets
- [ ] Deploy en VPS o cloud

### **Conocimientos Teóricos**
- [ ] Entender arquitectura cliente-servidor
- [ ] Comprender principios REST
- [ ] Saber cuándo usar SQL vs NoSQL
- [ ] Entender conceptos de seguridad web

---

## 🎯 Proyectos Prácticos

### **Proyecto 1: Blog API Completa**
```javascript
// Ejemplo de estructura del proyecto
const express = require('express');
const { PrismaClient } = require('@prisma/client');
const jwt = require('jsonwebtoken');

const app = express();
const prisma = new PrismaClient();

// Middleware
app.use(express.json());
app.use(authMiddleware);

// Routes
app.post('/posts', createPost);
app.get('/posts', getPosts);
app.put('/posts/:id', updatePost);
app.delete('/posts/:id', deletePost);

// Authentication
app.post('/auth/login', login);
app.post('/auth/register', register);
```

### **Proyecto 2: E-commerce Backend**
- Product management API
- User authentication
- Order processing
- Payment integration
- Inventory management

### **Proyecto 3: Real-time Chat System**
- WebSocket connections
- User presence
- Message persistence
- File sharing
- Group chats

---

## 📚 Recursos Adicionales

### **Documentación Oficial**
- [Node.js Documentation](https://nodejs.org/docs/)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/current/tutorial.html)
- [MongoDB Manual](https://docs.mongodb.com/manual/)

### **Cursos Online**
- [Node.js Course](https://www.udemy.com/course/nodejs-the-complete-guide/) - Udemy
- [Express.js Tutorial](https://expressjs.com/en/starter/installing.html) - Oficial
- [PostgreSQL Course](https://www.postgresqltutorial.com/) - PostgreSQL Tutorial
- [MongoDB University](https://university.mongodb.com/) - MongoDB

### **Herramientas de Testing**
- [Postman](https://www.postman.com/) - API testing
- [Insomnia](https://insomnia.rest/) - API client
- [Thunder Client](https://www.thunderclient.com/) - VS Code extension
- [Jest](https://jestjs.io/) - Testing framework

### **Comunidades y Foros**
- [Node.js Discord](https://discord.gg/nodejs)
- [Express.js Community](https://expressjs.com/en/resources/community.html)
- [Stack Overflow Backend](https://stackoverflow.com/questions/tagged/backend)
- [Reddit r/node](https://www.reddit.com/r/node/)

---

## 🚀 Próximos Pasos

Una vez completado este módulo, estarás listo para:
1. **Módulo 4**: DevOps y Herramientas de Desarrollo
2. **Especialización Backend**: Microservicios, GraphQL, o serverless
3. **Full-Stack Development**: Combinar frontend y backend
4. **Backend Architect**: Diseñar sistemas escalables

---

## 💡 Consejos de Estudio

### **Desarrollo Backend Efectivo**
- **API First**: Diseña APIs antes de implementar
- **Security First**: Implementa seguridad desde el inicio
- **Testing**: Escribe tests para cada endpoint
- **Documentation**: Documenta tus APIs con Swagger/OpenAPI

### **Bases de Datos**
- **Normalize**: Diseña bases de datos normalizadas
- **Index**: Usa índices para performance
- **Backup**: Implementa estrategias de backup
- **Monitor**: Monitorea performance de queries

### **Deployment y DevOps**
- **Environment**: Separa configuraciones por ambiente
- **Logging**: Implementa logging estructurado
- **Monitoring**: Monitorea tu aplicación en producción
- **CI/CD**: Automatiza deployment

---

## 🔍 Evaluación Continua

### **Métricas de Performance**
- API response time
- Database query performance
- Memory usage
- CPU utilization
- Error rates

### **Seguridad y Testing**
- Security scan results
- Test coverage percentage
- Vulnerability assessments
- Penetration test results
- OWASP compliance

---

**¡El backend es el cerebro de la aplicación! Una API bien diseñada y segura es la base de cualquier aplicación web exitosa.**

**Próximo módulo**: [DevOps y Herramientas de Desarrollo](./04-DevOps-Tools.md) 