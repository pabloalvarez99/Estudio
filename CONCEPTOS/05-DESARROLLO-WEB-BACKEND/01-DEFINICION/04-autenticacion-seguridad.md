# 🔐 Autenticación y Seguridad: Protección de APIs Web

## 🎯 **OBJETIVO**
Implementar sistemas robustos de autenticación y autorización, aplicar mejores prácticas de seguridad y proteger las APIs contra vulnerabilidades comunes.

---

## 📋 **¿QUÉ ES LA AUTENTICACIÓN Y AUTORIZACIÓN?**

### **Definición**
- **Autenticación**: Proceso de verificar la identidad de un usuario
- **Autorización**: Proceso de determinar qué recursos puede acceder un usuario autenticado

### **Diferencias Clave**
```javascript
// Autenticación: ¿Quién eres?
const user = authenticateUser(credentials);

// Autorización: ¿Qué puedes hacer?
if (authorizeUser(user, 'admin')) {
  // Acceso a funcionalidades de admin
}
```

---

## 🔑 **JWT (JSON WEB TOKENS)**

### **¿Qué son los JWT?**
JWT es un estándar para crear tokens de acceso que permiten la autenticación stateless entre aplicaciones.

### **Estructura de un JWT**
```
header.payload.signature
```

#### **Header**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### **Payload**
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "role": "admin",
  "iat": 1516239022,
  "exp": 1516242622
}
```

#### **Signature**
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

### **Implementación de JWT**
```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// Generar token
const generateToken = (userId, role) => {
  return jwt.sign(
    {
      sub: userId,
      role: role,
      iat: Date.now(),
      exp: Date.now() + (24 * 60 * 60 * 1000) // 24 horas
    },
    process.env.JWT_SECRET,
    { algorithm: 'HS256' }
  );
};

// Verificar token
const verifyToken = (token) => {
  try {
    return jwt.verify(token, process.env.JWT_SECRET);
  } catch (error) {
    throw new Error('Invalid token');
  }
};

// Login endpoint
app.post('/auth/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    
    // Buscar usuario
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    // Verificar password
    const isValidPassword = await bcrypt.compare(password, user.password);
    if (!isValidPassword) {
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    // Generar token
    const token = generateToken(user._id, user.role);
    
    // Response
    res.json({
      success: true,
      data: {
        user: {
          id: user._id,
          name: user.name,
          email: user.email,
          role: user.role
        },
        token: token
      },
      message: 'Login successful'
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      error: 'Internal server error'
    });
  }
});
```

### **Middleware de Autenticación JWT**
```javascript
const authenticateJWT = async (req, res, next) => {
  try {
    // Obtener token del header
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'NO_TOKEN',
          message: 'Access token is required'
        }
      });
    }
    
    const token = authHeader.substring(7); // Remover 'Bearer '
    
    // Verificar token
    const decoded = verifyToken(token);
    
    // Buscar usuario
    const user = await User.findById(decoded.sub);
    if (!user) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'USER_NOT_FOUND',
          message: 'User no longer exists'
        }
      });
    }
    
    // Verificar si el usuario está activo
    if (!user.isActive) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'USER_INACTIVE',
          message: 'User account is deactivated'
        }
      });
    }
    
    // Añadir usuario al request
    req.user = user;
    req.token = decoded;
    
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
```

---

## 🎭 **OAUTH 2.0**

### **¿Qué es OAuth 2.0?**
OAuth 2.0 es un protocolo de autorización que permite a aplicaciones de terceros acceder a recursos protegidos sin compartir credenciales.

### **Flujos de OAuth 2.0**

#### **Authorization Code Flow**
```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

// Configuración de Google OAuth
passport.use(new GoogleStrategy({
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: "/auth/google/callback"
}, async (accessToken, refreshToken, profile, done) => {
  try {
    // Buscar o crear usuario
    let user = await User.findOne({ googleId: profile.id });
    
    if (!user) {
      user = await User.create({
        googleId: profile.id,
        name: profile.displayName,
        email: profile.emails[0].value,
        avatar: profile.photos[0].value
      });
    }
    
    return done(null, user);
  } catch (error) {
    return done(error, null);
  }
}));

// Serialización del usuario
passport.serializeUser((user, done) => {
  done(null, user.id);
});

passport.deserializeUser(async (id, done) => {
  try {
    const user = await User.findById(id);
    done(null, user);
  } catch (error) {
    done(error, null);
  }
});

// Rutas de OAuth
app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    // Autenticación exitosa
    const token = generateToken(req.user._id, req.user.role);
    res.redirect(`/dashboard?token=${token}`);
  }
);
```

#### **Client Credentials Flow**
```javascript
// Para APIs que se comunican entre sí
app.post('/auth/client-credentials', async (req, res) => {
  try {
    const { clientId, clientSecret } = req.body;
    
    // Verificar credenciales del cliente
    const client = await Client.findOne({ 
      clientId, 
      clientSecret,
      isActive: true 
    });
    
    if (!client) {
      return res.status(401).json({
        success: false,
        error: 'Invalid client credentials'
      });
    }
    
    // Generar token de acceso
    const token = generateClientToken(client.clientId, client.scopes);
    
    res.json({
      success: true,
      data: {
        access_token: token,
        token_type: 'Bearer',
        expires_in: 3600,
        scope: client.scopes.join(' ')
      }
    });
    
  } catch (error) {
    res.status(500).json({
      success: false,
      error: 'Internal server error'
    });
  }
});
```

---

## 🔒 **AUTORIZACIÓN Y ROLES**

### **Sistema de Roles y Permisos**
```javascript
// Definir roles y permisos
const ROLES = {
  USER: 'user',
  MODERATOR: 'moderator',
  ADMIN: 'admin',
  SUPER_ADMIN: 'super_admin'
};

const PERMISSIONS = {
  READ_USERS: 'read:users',
  WRITE_USERS: 'write:users',
  DELETE_USERS: 'delete:users',
  READ_POSTS: 'read:posts',
  WRITE_POSTS: 'write:posts',
  DELETE_POSTS: 'delete:posts',
  MANAGE_SYSTEM: 'manage:system'
};

// Mapeo de roles a permisos
const ROLE_PERMISSIONS = {
  [ROLES.USER]: [
    PERMISSIONS.READ_POSTS,
    PERMISSIONS.WRITE_POSTS
  ],
  [ROLES.MODERATOR]: [
    PERMISSIONS.READ_USERS,
    PERMISSIONS.READ_POSTS,
    PERMISSIONS.WRITE_POSTS,
    PERMISSIONS.DELETE_POSTS
  ],
  [ROLES.ADMIN]: [
    PERMISSIONS.READ_USERS,
    PERMISSIONS.WRITE_USERS,
    PERMISSIONS.READ_POSTS,
    PERMISSIONS.WRITE_POSTS,
    PERMISSIONS.DELETE_POSTS
  ],
  [ROLES.SUPER_ADMIN]: [
    ...Object.values(PERMISSIONS)
  ]
};

// Middleware de autorización
const authorize = (...requiredPermissions) => {
  return (req, res, next) => {
    try {
      const userRole = req.user.role;
      const userPermissions = ROLE_PERMISSIONS[userRole] || [];
      
      // Verificar si el usuario tiene todos los permisos requeridos
      const hasAllPermissions = requiredPermissions.every(permission =>
        userPermissions.includes(permission)
      );
      
      if (!hasAllPermissions) {
        return res.status(403).json({
          success: false,
          error: {
            code: 'INSUFFICIENT_PERMISSIONS',
            message: 'You do not have permission to perform this action',
            required: requiredPermissions,
            current: userPermissions
          }
        });
      }
      
      next();
    } catch (error) {
      next(error);
    }
  };
};

// Middleware de roles específicos
const requireRole = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({
        success: false,
        error: {
          code: 'INSUFFICIENT_ROLE',
          message: 'You do not have the required role for this action',
          required: roles,
          current: req.user.role
        }
      });
    }
    next();
  };
};

// Uso en routes
app.get('/admin/users', 
  authenticateJWT, 
  requireRole(ROLES.ADMIN, ROLES.SUPER_ADMIN),
  async (req, res) => {
    // Solo admins pueden acceder
  }
);

app.delete('/users/:id',
  authenticateJWT,
  authorize(PERMISSIONS.DELETE_USERS),
  async (req, res) => {
    // Solo usuarios con permiso de eliminar usuarios
  }
);
```

---

## 🛡️ **VALIDACIÓN Y SANITIZACIÓN**

### **Validación de Inputs**
```javascript
const { body, param, query, validationResult } = require('express-validator');
const xss = require('xss');

// Middleware de sanitización
const sanitizeInput = (req, res, next) => {
  // Sanitizar body
  if (req.body) {
    Object.keys(req.body).forEach(key => {
      if (typeof req.body[key] === 'string') {
        req.body[key] = xss(req.body[key]);
      }
    });
  }
  
  // Sanitizar query params
  if (req.query) {
    Object.keys(req.query).forEach(key => {
      if (typeof req.query[key] === 'string') {
        req.query[key] = xss(req.query[key]);
      }
    });
  }
  
  next();
};

// Validación de creación de usuario
const validateCreateUser = [
  body('name')
    .trim()
    .isLength({ min: 2, max: 50 })
    .withMessage('Name must be between 2 and 50 characters')
    .matches(/^[a-zA-Z\s]+$/)
    .withMessage('Name can only contain letters and spaces'),
  
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
  
  body('phone')
    .optional()
    .matches(/^\+?[\d\s\-\(\)]+$/)
    .withMessage('Invalid phone number format'),
  
  // Validación personalizada
  body('email').custom(async (email) => {
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      throw new Error('Email already exists');
    }
    return true;
  }),
  
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
            value: err.value,
            location: err.location
          }))
        }
      });
    }
    next();
  }
];

// Aplicar middleware
app.use(sanitizeInput);
app.post('/users', validateCreateUser, async (req, res) => {
  // Usuario validado y sanitizado
});
```

### **Validación de Archivos**
```javascript
const multer = require('multer');
const path = require('path');

// Configuración de multer
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
    cb(null, file.fieldname + '-' + uniqueSuffix + path.extname(file.originalname));
  }
});

// Filtro de archivos
const fileFilter = (req, file, cb) => {
  // Tipos de archivo permitidos
  const allowedTypes = ['image/jpeg', 'image/png', 'image/gif', 'application/pdf'];
  
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Invalid file type. Only JPEG, PNG, GIF and PDF are allowed.'), false);
  }
};

// Configuración de multer
const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB
    files: 5 // máximo 5 archivos
  },
  fileFilter: fileFilter
});

// Middleware de validación de archivos
const validateFileUpload = (req, res, next) => {
  upload.array('files', 5)(req, res, (err) => {
    if (err instanceof multer.MulterError) {
      if (err.code === 'LIMIT_FILE_SIZE') {
        return res.status(400).json({
          success: false,
          error: {
            code: 'FILE_TOO_LARGE',
            message: 'File size too large. Maximum size is 5MB.'
          }
        });
      }
      if (err.code === 'LIMIT_FILE_COUNT') {
        return res.status(400).json({
          success: false,
          error: {
            code: 'TOO_MANY_FILES',
            message: 'Too many files. Maximum is 5 files.'
          }
        });
      }
    } else if (err) {
      return res.status(400).json({
        success: false,
        error: {
          code: 'INVALID_FILE',
          message: err.message
        }
      });
    }
    
    next();
  });
};

// Uso
app.post('/upload', validateFileUpload, async (req, res) => {
  // Archivos validados y subidos
  const files = req.files;
  res.json({
    success: true,
    data: {
      files: files.map(file => ({
        filename: file.filename,
        originalname: file.originalname,
        size: file.size,
        mimetype: file.mimetype
      }))
    },
    message: 'Files uploaded successfully'
  });
});
```

---

## 🚨 **PROTECCIÓN CONTRA VULNERABILIDADES**

### **OWASP Top 10**

#### **1. Injection (SQL, NoSQL, Command)**
```javascript
// ❌ VULNERABLE - SQL Injection
app.get('/users', async (req, res) => {
  const { search } = req.query;
  const query = `SELECT * FROM users WHERE name LIKE '%${search}%'`;
  const users = await db.query(query);
  res.json(users);
});

// ✅ SEGURO - Usar parámetros preparados
app.get('/users', async (req, res) => {
  const { search } = req.query;
  const query = 'SELECT * FROM users WHERE name LIKE ?';
  const users = await db.query(query, [`%${search}%`]);
  res.json(users);
});

// ✅ SEGURO - Con Mongoose
app.get('/users', async (req, res) => {
  const { search } = req.query;
  const users = await User.find({
    name: { $regex: search, $options: 'i' }
  });
  res.json(users);
});
```

#### **2. Broken Authentication**
```javascript
// ✅ SEGURO - Implementar rate limiting
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5, // máximo 5 intentos
  message: {
    success: false,
    error: {
      code: 'TOO_MANY_ATTEMPTS',
      message: 'Too many login attempts, please try again later.'
    }
  },
  skipSuccessfulRequests: true
});

app.use('/auth/login', loginLimiter);

// ✅ SEGURO - Logging de intentos fallidos
app.post('/auth/login', async (req, res) => {
  try {
    const { email, password } = req.body;
    
    const user = await User.findOne({ email });
    if (!user) {
      await logFailedAttempt(req.ip, email, 'User not found');
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    const isValidPassword = await bcrypt.compare(password, user.password);
    if (!isValidPassword) {
      await logFailedAttempt(req.ip, email, 'Invalid password');
      return res.status(401).json({
        success: false,
        error: 'Invalid credentials'
      });
    }
    
    // Login exitoso
    await logSuccessfulLogin(req.ip, user._id);
    
    const token = generateToken(user._id, user.role);
    res.json({
      success: true,
      data: { user, token }
    });
    
  } catch (error) {
    next(error);
  }
});
```

#### **3. Sensitive Data Exposure**
```javascript
// ❌ VULNERABLE - Exponer datos sensibles
app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  res.json(user); // Incluye password hash, tokens, etc.
});

// ✅ SEGURO - Filtrar datos sensibles
app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id)
    .select('-password -resetToken -resetTokenExpiry');
  
  if (!user) {
    return res.status(404).json({
      success: false,
      error: 'User not found'
    });
  }
  
  res.json({
    success: true,
    data: user
  });
});

// ✅ SEGURO - Usar transform en Mongoose
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String,
  role: String
});

userSchema.methods.toJSON = function() {
  const user = this.toObject();
  delete user.password;
  delete user.resetToken;
  delete user.resetTokenExpiry;
  return user;
};
```

#### **4. XML External Entity (XXE)**
```javascript
// ❌ VULNERABLE - Parsear XML sin restricciones
app.post('/parse-xml', express.raw({ type: 'application/xml' }), (req, res) => {
  const xml = req.body.toString();
  const result = xml2js.parseString(xml); // Vulnerable a XXE
  res.json(result);
});

// ✅ SEGURO - Configurar parser XML de forma segura
const xml2js = require('xml2js');

const secureParser = new xml2js.Parser({
  explicitArray: false,
  explicitRoot: false,
  ignoreAttrs: true,
  tagNameProcessors: [xml2js.processors.stripPrefix],
  valueProcessors: [xml2js.processors.parseBooleans, xml2js.processors.parseNumbers]
});

app.post('/parse-xml', express.raw({ type: 'application/xml' }), (req, res) => {
  const xml = req.body.toString();
  
  secureParser.parseString(xml, (err, result) => {
    if (err) {
      return res.status(400).json({
        success: false,
        error: 'Invalid XML format'
      });
    }
    res.json(result);
  });
});
```

#### **5. Broken Access Control**
```javascript
// ❌ VULNERABLE - Sin verificación de propiedad
app.put('/users/:id', authenticateJWT, async (req, res) => {
  const user = await User.findByIdAndUpdate(req.params.id, req.body);
  res.json(user); // Cualquier usuario puede modificar cualquier otro
});

// ✅ SEGURO - Verificar propiedad o permisos
app.put('/users/:id', authenticateJWT, async (req, res) => {
  const userId = req.params.id;
  const currentUser = req.user;
  
  // Verificar si es el propio usuario o tiene permisos de admin
  if (userId !== currentUser._id.toString() && currentUser.role !== 'admin') {
    return res.status(403).json({
      success: false,
      error: {
        code: 'ACCESS_DENIED',
        message: 'You can only modify your own profile'
      }
    });
  }
  
  const user = await User.findByIdAndUpdate(userId, req.body, { new: true });
  res.json({
    success: true,
    data: user
  });
});
```

---

## 🔍 **MONITOREO Y AUDITORÍA**

### **Logging de Seguridad**
```javascript
const winston = require('winston');

// Logger de seguridad
const securityLogger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'security.log' })
  ]
});

// Middleware de logging de seguridad
const securityLogging = (req, res, next) => {
  const startTime = Date.now();
  
  // Log del request
  securityLogger.info('API Request', {
    timestamp: new Date().toISOString(),
    method: req.method,
    url: req.url,
    ip: req.ip,
    userAgent: req.get('User-Agent'),
    userId: req.user ? req.user._id : 'anonymous'
  });
  
  // Interceptar response
  const originalSend = res.send;
  res.send = function(data) {
    const duration = Date.now() - startTime;
    
    // Log del response
    securityLogger.info('API Response', {
      timestamp: new Date().toISOString(),
      method: req.method,
      url: req.url,
      statusCode: res.statusCode,
      duration: duration,
      userId: req.user ? req.user._id : 'anonymous'
    });
    
    originalSend.call(this, data);
  };
  
  next();
};

// Logging de eventos de seguridad
const logSecurityEvent = (event, details) => {
  securityLogger.warn('Security Event', {
    timestamp: new Date().toISOString(),
    event: event,
    details: details
  });
};

// Uso
app.use(securityLogging);

// En middleware de autenticación
if (!user.isActive) {
  logSecurityEvent('LOGIN_ATTEMPT_INACTIVE_ACCOUNT', {
    email: req.body.email,
    ip: req.ip
  });
  // ... manejar error
}
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Sistema de Autenticación JWT**
Implementa un sistema completo de login/registro con JWT.

### **Ejercicio 2: OAuth con Google**
Integra autenticación OAuth usando Google.

### **Ejercicio 3: Sistema de Roles y Permisos**
Crea un sistema granular de autorización.

### **Ejercicio 4: Validación y Sanitización**
Implementa validación robusta de todos los inputs.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [JWT.io](https://jwt.io/) - JWT Debugger y documentación
- [OAuth 2.0 RFC](https://tools.ietf.org/html/rfc6749) - Especificación oficial
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) - Vulnerabilidades web

### **Herramientas de Seguridad**
- [Helmet.js](https://helmetjs.github.io/) - Headers de seguridad
- [Express Rate Limit](https://github.com/nfriedly/express-rate-limit) - Rate limiting
- [Express Validator](https://express-validator.github.io/) - Validación de inputs

### **Libros y Cursos**
- "Web Application Security" - Andrew Hoffman
- "OAuth 2 in Action" - Justin Richer
- Web Security Course (OWASP)

---

*La seguridad en las aplicaciones web es crítica. Implementar autenticación robusta y seguir las mejores prácticas de seguridad te protegerá contra ataques comunes y mantendrá tus aplicaciones seguras.* 