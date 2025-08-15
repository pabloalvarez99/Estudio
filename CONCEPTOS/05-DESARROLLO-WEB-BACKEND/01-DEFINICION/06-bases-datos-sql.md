# 🗃️ Bases de Datos SQL: PostgreSQL, MySQL y ORMs

## 🎯 **OBJETIVO**
Dominar las bases de datos relacionales SQL, especialmente PostgreSQL y MySQL, incluyendo ORMs, transacciones, optimización de queries y mejores prácticas de diseño.

---

## 📋 **¿QUÉ SON LAS BASES DE DATOS SQL?**

### **Definición**
SQL (Structured Query Language) es un lenguaje estándar para acceder y manipular bases de datos relacionales. Las bases de datos SQL almacenan datos en tablas relacionadas entre sí mediante claves foráneas.

### **Características Principales**
- **ACID**: Atomicity, Consistency, Isolation, Durability
- **Relaciones**: Claves primarias y foráneas
- **Normalización**: Eliminación de redundancia
- **Transacciones**: Operaciones atómicas
- **Integridad**: Constraints y validaciones
- **Escalabilidad vertical**: Mejora de hardware del servidor

---

## 🐘 **POSTGRESQL**

### **¿Qué es PostgreSQL?**
PostgreSQL es una base de datos relacional de código abierto, orientada a objetos, que extiende el estándar SQL con características avanzadas como tipos personalizados, funciones, triggers y soporte para JSON.

### **Características Avanzadas**
- **Tipos personalizados**: Creación de tipos de datos específicos
- **Funciones y procedimientos**: PL/pgSQL, PL/Python, PL/JavaScript
- **Triggers**: Automatización de acciones en la base de datos
- **Soporte JSON**: Campos JSON y JSONB nativos
- **Full-text search**: Búsqueda de texto completo
- **Particionamiento**: División de tablas grandes
- **Replicación**: Master-slave y streaming

### **Instalación y Configuración**
```bash
# Instalar dependencias
npm install pg pg-pool sequelize

# Conectar a PostgreSQL
const { Pool } = require('pg');

const pool = new Pool({
  user: 'postgres',
  host: 'localhost',
  database: 'myapp',
  password: 'your-password',
  port: 5432,
  max: 20, // máximo de conexiones en el pool
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});
```

### **Estructura de Base de Datos**
```sql
-- Crear base de datos
CREATE DATABASE myapp;

-- Crear usuario
CREATE USER myapp_user WITH PASSWORD 'secure_password';

-- Otorgar permisos
GRANT ALL PRIVILEGES ON DATABASE myapp TO myapp_user;

-- Conectar a la base de datos
\c myapp;

-- Crear esquema
CREATE SCHEMA IF NOT EXISTS public;
```

### **Diseño de Tablas**
```sql
-- Tabla de usuarios
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  date_of_birth DATE,
  is_active BOOLEAN DEFAULT true,
  role VARCHAR(20) DEFAULT 'user' CHECK (role IN ('user', 'moderator', 'admin')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de perfiles
CREATE TABLE user_profiles (
  id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  bio TEXT,
  avatar_url VARCHAR(500),
  website VARCHAR(255),
  location VARCHAR(100),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de posts
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  excerpt VARCHAR(500),
  author_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  status VARCHAR(20) DEFAULT 'draft' CHECK (status IN ('draft', 'published', 'archived')),
  published_at TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de categorías
CREATE TABLE categories (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) UNIQUE NOT NULL,
  slug VARCHAR(100) UNIQUE NOT NULL,
  description TEXT,
  parent_id INTEGER REFERENCES categories(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de relación posts-categorías (many-to-many)
CREATE TABLE post_categories (
  post_id INTEGER REFERENCES posts(id) ON DELETE CASCADE,
  category_id INTEGER REFERENCES categories(id) ON DELETE CASCADE,
  PRIMARY KEY (post_id, category_id)
);

-- Tabla de tags
CREATE TABLE tags (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50) UNIQUE NOT NULL,
  slug VARCHAR(50) UNIQUE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de relación posts-tags (many-to-many)
CREATE TABLE post_tags (
  post_id INTEGER REFERENCES posts(id) ON DELETE CASCADE,
  tag_id INTEGER REFERENCES tags(id) ON DELETE CASCADE,
  PRIMARY KEY (post_id, tag_id)
);

-- Tabla de comentarios
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  content TEXT NOT NULL,
  author_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  post_id INTEGER REFERENCES posts(id) ON DELETE CASCADE,
  parent_id INTEGER REFERENCES comments(id) ON DELETE CASCADE,
  is_approved BOOLEAN DEFAULT false,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

### **Índices y Optimización**
```sql
-- Índices para mejorar performance
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);
CREATE INDEX idx_users_role ON users(role);
CREATE INDEX idx_users_created_at ON users(created_at);

CREATE INDEX idx_posts_author_id ON posts(author_id);
CREATE INDEX idx_posts_status ON posts(status);
CREATE INDEX idx_posts_published_at ON posts(published_at);
CREATE INDEX idx_posts_title_content ON posts USING gin(to_tsvector('english', title || ' ' || content));

CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_author_id ON comments(author_id);
CREATE INDEX idx_comments_created_at ON comments(created_at);

-- Índices compuestos
CREATE INDEX idx_posts_author_status ON posts(author_id, status);
CREATE INDEX idx_posts_category_status ON posts(id) WHERE status = 'published';

-- Índices parciales
CREATE INDEX idx_active_users ON users(id) WHERE is_active = true;
CREATE INDEX idx_published_posts ON posts(id) WHERE status = 'published';
```

### **Triggers y Funciones**
```sql
-- Función para actualizar updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = CURRENT_TIMESTAMP;
  RETURN NEW;
END;
$$ language 'plpgsql';

-- Triggers para actualizar updated_at
CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_posts_updated_at BEFORE UPDATE ON posts
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_comments_updated_at BEFORE UPDATE ON comments
  FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Función para generar slug
CREATE OR REPLACE FUNCTION generate_slug(input_text TEXT)
RETURNS TEXT AS $$
BEGIN
  RETURN lower(regexp_replace(input_text, '[^a-zA-Z0-9\s-]', '', 'g'));
END;
$$ language 'plpgsql';

-- Trigger para generar slug automáticamente
CREATE OR REPLACE FUNCTION generate_category_slug()
RETURNS TRIGGER AS $$
BEGIN
  NEW.slug := generate_slug(NEW.name);
  RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER generate_category_slug_trigger BEFORE INSERT OR UPDATE ON categories
  FOR EACH ROW EXECUTE FUNCTION generate_category_slug();
```

---

## 🐬 **MYSQL**

### **¿Qué es MySQL?**
MySQL es una base de datos relacional de código abierto, propiedad de Oracle, que es ampliamente utilizada en aplicaciones web debido a su facilidad de uso, rendimiento y compatibilidad.

### **Características Principales**
- **Almacenamiento múltiple**: InnoDB, MyISAM, Memory
- **Replicación**: Master-slave y master-master
- **Particionamiento**: División de tablas por rangos, hash o listas
- **Full-text search**: Búsqueda de texto completo
- **Triggers y stored procedures**: Automatización de la base de datos
- **Views**: Vistas virtuales de datos

### **Instalación y Configuración**
```bash
# Instalar dependencias
npm install mysql2 sequelize

# Conectar a MySQL
const mysql = require('mysql2/promise');

const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'your-password',
  database: 'myapp',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
  acquireTimeout: 60000,
  timeout: 60000,
  reconnect: true
});
```

### **Estructura de Base de Datos MySQL**
```sql
-- Crear base de datos
CREATE DATABASE myapp CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Usar la base de datos
USE myapp;

-- Crear tabla de usuarios
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  date_of_birth DATE,
  is_active BOOLEAN DEFAULT TRUE,
  role ENUM('user', 'moderator', 'admin') DEFAULT 'user',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  INDEX idx_email (email),
  INDEX idx_username (username),
  INDEX idx_role (role),
  INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Crear tabla de perfiles
CREATE TABLE user_profiles (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  bio TEXT,
  avatar_url VARCHAR(500),
  website VARCHAR(255),
  location VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
  INDEX idx_user_id (user_id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Crear tabla de posts
CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  content LONGTEXT NOT NULL,
  excerpt VARCHAR(500),
  author_id INT NOT NULL,
  status ENUM('draft', 'published', 'archived') DEFAULT 'draft',
  published_at TIMESTAMP NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  FOREIGN KEY (author_id) REFERENCES users(id) ON DELETE CASCADE,
  INDEX idx_author_id (author_id),
  INDEX idx_status (status),
  INDEX idx_published_at (published_at),
  FULLTEXT idx_title_content (title, content)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

---

## 🔧 **ORMS (OBJECT RELATIONAL MAPPERS)**

### **¿Qué son los ORMs?**
Los ORMs son herramientas que permiten interactuar con bases de datos usando objetos y métodos en lugar de escribir SQL directamente. Proporcionan una capa de abstracción que simplifica el desarrollo.

### **Sequelize ORM**

#### **Configuración Básica**
```javascript
const { Sequelize, DataTypes, Model } = require('sequelize');

// Configuración de conexión
const sequelize = new Sequelize('myapp', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres', // o 'mysql'
  logging: false, // desactivar logging SQL
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  }
});

// Probar conexión
async function testConnection() {
  try {
    await sequelize.authenticate();
    console.log('Connection has been established successfully.');
  } catch (error) {
    console.error('Unable to connect to the database:', error);
  }
}

testConnection();
```

#### **Definición de Modelos**
```javascript
// Modelo de Usuario
class User extends Model {}

User.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  username: {
    type: DataTypes.STRING(50),
    allowNull: false,
    unique: true,
    validate: {
      len: [3, 50],
      is: /^[a-zA-Z0-9_]+$/
    }
  },
  email: {
    type: DataTypes.STRING(255),
    allowNull: false,
    unique: true,
    validate: {
      isEmail: true
    }
  },
  passwordHash: {
    type: DataTypes.STRING(255),
    allowNull: false
  },
  firstName: {
    type: DataTypes.STRING(100),
    allowNull: true
  },
  lastName: {
    type: DataTypes.STRING(100),
    allowNull: true
  },
  dateOfBirth: {
    type: DataTypes.DATEONLY,
    allowNull: true
  },
  isActive: {
    type: DataTypes.BOOLEAN,
    defaultValue: true
  },
  role: {
    type: DataTypes.ENUM('user', 'moderator', 'admin'),
    defaultValue: 'user'
  }
}, {
  sequelize,
  modelName: 'User',
  tableName: 'users',
  timestamps: true,
  underscored: true, // usar snake_case para columnas
  indexes: [
    {
      fields: ['email']
    },
    {
      fields: ['username']
    },
    {
      fields: ['role']
    },
    {
      fields: ['createdAt']
    }
  ]
});

// Modelo de Perfil
class UserProfile extends Model {}

UserProfile.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  bio: {
    type: DataTypes.TEXT,
    allowNull: true
  },
  avatarUrl: {
    type: DataTypes.STRING(500),
    allowNull: true,
    validate: {
      isUrl: true
    }
  },
  website: {
    type: DataTypes.STRING(255),
    allowNull: true,
    validate: {
      isUrl: true
    }
  },
  location: {
    type: DataTypes.STRING(100),
    allowNull: true
  }
}, {
  sequelize,
  modelName: 'UserProfile',
  tableName: 'user_profiles',
  timestamps: true,
  underscored: true
});

// Modelo de Post
class Post extends Model {}

Post.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  title: {
    type: DataTypes.STRING(255),
    allowNull: false,
    validate: {
      len: [1, 255]
    }
  },
  content: {
    type: DataTypes.TEXT,
    allowNull: false
  },
  excerpt: {
    type: DataTypes.STRING(500),
    allowNull: true
  },
  status: {
    type: DataTypes.ENUM('draft', 'published', 'archived'),
    defaultValue: 'draft'
  },
  publishedAt: {
    type: DataTypes.DATE,
    allowNull: true
  }
}, {
  sequelize,
  modelName: 'Post',
  tableName: 'posts',
  timestamps: true,
  underscored: true,
  indexes: [
    {
      fields: ['authorId']
    },
    {
      fields: ['status']
    },
    {
      fields: ['publishedAt']
    }
  ]
});

// Modelo de Categoría
class Category extends Model {}

Category.init({
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  name: {
    type: DataTypes.STRING(100),
    allowNull: false,
    unique: true
  },
  slug: {
    type: DataTypes.STRING(100),
    allowNull: false,
    unique: true
  },
  description: {
    type: DataTypes.TEXT,
    allowNull: true
  },
  parentId: {
    type: DataTypes.INTEGER,
    allowNull: true,
    references: {
      model: 'categories',
      key: 'id'
    }
  }
}, {
  sequelize,
  modelName: 'Category',
  tableName: 'categories',
  timestamps: true,
  underscored: true
});
```

#### **Definición de Relaciones**
```javascript
// Relaciones uno a uno
User.hasOne(UserProfile, {
  foreignKey: 'userId',
  as: 'profile'
});

UserProfile.belongsTo(User, {
  foreignKey: 'userId',
  as: 'user'
});

// Relaciones uno a muchos
User.hasMany(Post, {
  foreignKey: 'authorId',
  as: 'posts'
});

Post.belongsTo(User, {
  foreignKey: 'authorId',
  as: 'author'
});

// Relaciones muchos a muchos
Post.belongsToMany(Category, {
  through: 'post_categories',
  foreignKey: 'postId',
  otherKey: 'categoryId',
  as: 'categories'
});

Category.belongsToMany(Post, {
  through: 'post_categories',
  foreignKey: 'categoryId',
  otherKey: 'postId',
  as: 'posts'
});

// Relaciones jerárquicas (self-referencing)
Category.hasMany(Category, {
  foreignKey: 'parentId',
  as: 'children'
});

Category.belongsTo(Category, {
  foreignKey: 'parentId',
  as: 'parent'
});
```

#### **Operaciones CRUD con Sequelize**
```javascript
class UserService {
  // CREATE - Crear usuario
  async createUser(userData) {
    try {
      const user = await User.create(userData);
      return user;
    } catch (error) {
      if (error.name === 'SequelizeUniqueConstraintError') {
        throw new Error('Username or email already exists');
      }
      throw error;
    }
  }

  // READ - Obtener usuarios con paginación y filtros
  async getUsers(options = {}) {
    const {
      page = 1,
      limit = 10,
      search,
      role,
      isActive,
      includeProfile = false
    } = options;

    const where = {};
    
    if (search) {
      where[Op.or] = [
        { username: { [Op.iLike]: `%${search}%` } },
        { email: { [Op.iLike]: `%${search}%` } },
        { firstName: { [Op.iLike]: `%${search}%` } },
        { lastName: { [Op.iLike]: `%${search}%` } }
      ];
    }
    
    if (role) {
      where.role = role;
    }
    
    if (typeof isActive === 'boolean') {
      where.isActive = isActive;
    }

    const include = [];
    if (includeProfile) {
      include.push({
        model: UserProfile,
        as: 'profile',
        attributes: ['bio', 'avatarUrl', 'website', 'location']
      });
    }

    const { count, rows } = await User.findAndCountAll({
      where,
      include,
      attributes: { exclude: ['passwordHash'] },
      order: [['createdAt', 'DESC']],
      limit,
      offset: (page - 1) * limit
    });

    return {
      users: rows,
      pagination: {
        page,
        limit,
        total: count,
        pages: Math.ceil(count / limit)
      }
    };
  }

  // READ - Obtener usuario por ID con relaciones
  async getUserById(id, includeRelations = false) {
    const include = [];
    
    if (includeRelations) {
      include.push(
        {
          model: UserProfile,
          as: 'profile'
        },
        {
          model: Post,
          as: 'posts',
          include: [
            {
              model: Category,
              as: 'categories'
            }
          ]
        }
      );
    }

    const user = await User.findByPk(id, {
      include,
      attributes: { exclude: ['passwordHash'] }
    });

    if (!user) {
      throw new Error('User not found');
    }

    return user;
  }

  // UPDATE - Actualizar usuario
  async updateUser(id, updateData) {
    const user = await User.findByPk(id);
    
    if (!user) {
      throw new Error('User not found');
    }

    // Actualizar campos permitidos
    const allowedFields = [
      'firstName', 'lastName', 'dateOfBirth', 
      'isActive', 'role'
    ];
    
    const filteredData = {};
    allowedFields.forEach(field => {
      if (updateData[field] !== undefined) {
        filteredData[field] = updateData[field];
      }
    });

    await user.update(filteredData);
    return user;
  }

  // DELETE - Eliminar usuario
  async deleteUser(id) {
    const user = await User.findByPk(id);
    
    if (!user) {
      throw new Error('User not found');
    }

    await user.destroy();
    return { message: 'User deleted successfully' };
  }

  // Búsqueda avanzada con agregaciones
  async getUserStats() {
    const stats = await User.findAll({
      attributes: [
        'role',
        [sequelize.fn('COUNT', sequelize.col('id')), 'count'],
        [sequelize.fn('AVG', sequelize.fn('EXTRACT', sequelize.literal('YEAR FROM AGE(NOW(), "createdAt")'))), 'avgAge'],
        [sequelize.fn('COUNT', sequelize.literal('CASE WHEN "isActive" = true THEN 1 END')), 'activeUsers']
      ],
      group: ['role'],
      order: [[sequelize.literal('count'), 'DESC']]
    });

    return stats;
  }
}
```

---

## 💰 **TRANSACCIONES Y CONCURRENCIA**

### **¿Qué son las Transacciones?**
Las transacciones son grupos de operaciones que se ejecutan como una unidad atómica. Garantizan que todas las operaciones se completen exitosamente o ninguna se aplique (ACID).

### **Implementación de Transacciones**
```javascript
class TransactionService {
  // Transacción simple
  async createUserWithProfile(userData, profileData) {
    const transaction = await sequelize.transaction();
    
    try {
      // Crear usuario
      const user = await User.create(userData, { transaction });
      
      // Crear perfil
      const profile = await UserProfile.create({
        ...profileData,
        userId: user.id
      }, { transaction });
      
      // Confirmar transacción
      await transaction.commit();
      
      return { user, profile };
      
    } catch (error) {
      // Revertir transacción en caso de error
      await transaction.rollback();
      throw error;
    }
  }

  // Transacción con rollback automático
  async createUserWithProfileAuto(userData, profileData) {
    return await sequelize.transaction(async (transaction) => {
      // Crear usuario
      const user = await User.create(userData, { transaction });
      
      // Crear perfil
      const profile = await UserProfile.create({
        ...profileData,
        userId: user.id
      }, { transaction });
      
      return { user, profile };
    });
  }

  // Transacción compleja con múltiples operaciones
  async transferPosts(fromUserId, toUserId, postIds) {
    return await sequelize.transaction(async (transaction) => {
      // Verificar que los posts pertenezcan al usuario origen
      const posts = await Post.findAll({
        where: {
          id: postIds,
          authorId: fromUserId
        },
        transaction
      });

      if (posts.length !== postIds.length) {
        throw new Error('Some posts do not belong to the source user');
      }

      // Transferir posts
      await Post.update(
        { authorId: toUserId },
        {
          where: { id: postIds },
          transaction
        }
      );

      // Actualizar contadores de posts
      await User.increment('postCount', {
        by: posts.length,
        where: { id: toUserId },
        transaction
      });

      await User.decrement('postCount', {
        by: posts.length,
        where: { id: fromUserId },
        transaction
      });

      return { transferredPosts: posts.length };
    });
  }

  // Transacción con savepoints
  async complexOperation() {
    const transaction = await sequelize.transaction();
    
    try {
      // Operación 1
      const user = await User.create(userData, { transaction });
      
      // Crear savepoint
      await transaction.commit();
      const savepoint = await sequelize.transaction();
      
      try {
        // Operación 2
        const profile = await UserProfile.create({
          ...profileData,
          userId: user.id
        }, { savepoint });
        
        // Operación 3
        const post = await Post.create({
          ...postData,
          authorId: user.id
        }, { savepoint });
        
        await savepoint.commit();
        await transaction.commit();
        
        return { user, profile, post };
        
      } catch (error) {
        await savepoint.rollback();
        throw error;
      }
      
    } catch (error) {
      await transaction.rollback();
      throw error;
    }
  }
}
```

### **Manejo de Concurrencia**
```javascript
class ConcurrencyService {
  // Optimistic locking con versión
  async updateUserWithVersion(id, updateData) {
    const user = await User.findByPk(id);
    
    if (!user) {
      throw new Error('User not found');
    }

    try {
      await user.update(updateData, {
        where: {
          id: id,
          version: user.version // Verificar versión actual
        }
      });
      
      return user;
      
    } catch (error) {
      if (error.name === 'SequelizeEmptyResultError') {
        throw new Error('User was modified by another user. Please refresh and try again.');
      }
      throw error;
    }
  }

  // Pessimistic locking
  async updateUserWithLock(id, updateData) {
    const transaction = await sequelize.transaction({
      isolationLevel: Sequelize.Transaction.ISOLATION_LEVELS.READ_COMMITTED
    });
    
    try {
      const user = await User.findByPk(id, {
        lock: transaction.LOCK.UPDATE,
        transaction
      });
      
      if (!user) {
        throw new Error('User not found');
      }

      await user.update(updateData, { transaction });
      await transaction.commit();
      
      return user;
      
    } catch (error) {
      await transaction.rollback();
      throw error;
    }
  }

  // Deadlock handling
  async safeUpdateUser(id, updateData, maxRetries = 3) {
    let retries = 0;
    
    while (retries < maxRetries) {
      try {
        return await this.updateUserWithLock(id, updateData);
        
      } catch (error) {
        if (error.message.includes('deadlock') && retries < maxRetries - 1) {
          retries++;
          await new Promise(resolve => setTimeout(resolve, 100 * retries)); // Backoff exponencial
          continue;
        }
        throw error;
      }
    }
    
    throw new Error('Maximum retries exceeded');
  }
}
```

---

## 🔍 **OPTIMIZACIÓN DE QUERIES**

### **Análisis de Performance**
```javascript
class QueryOptimizationService {
  // Habilitar logging de queries
  async enableQueryLogging() {
    sequelize.options.logging = console.log;
  }

  // Analizar query con EXPLAIN
  async explainQuery(sql, params = []) {
    const result = await sequelize.query(`EXPLAIN (ANALYZE, BUFFERS) ${sql}`, {
      replacements: params,
      type: Sequelize.QueryTypes.SELECT
    });
    
    return result;
  }

  // Query con eager loading optimizado
  async getUsersWithPostsOptimized(options = {}) {
    const { page = 1, limit = 10 } = options;
    
    // Usar include separado para evitar N+1 queries
    const users = await User.findAll({
      attributes: ['id', 'username', 'email', 'firstName', 'lastName'],
      include: [
        {
          model: Post,
          as: 'posts',
          attributes: ['id', 'title', 'status', 'createdAt'],
          separate: true, // Query separado para posts
          limit: 5 // Limitar posts por usuario
        }
      ],
      order: [['createdAt', 'DESC']],
      limit,
      offset: (page - 1) * limit
    });

    return users;
  }

  // Query con subqueries optimizadas
  async getUsersWithPostCount() {
    const users = await User.findAll({
      attributes: [
        'id',
        'username',
        'email',
        [
          sequelize.literal(`(
            SELECT COUNT(*)
            FROM posts
            WHERE posts.author_id = User.id
            AND posts.status = 'published'
          )`),
          'publishedPostCount'
        ]
      ],
      order: [[sequelize.literal('publishedPostCount'), 'DESC']]
    });

    return users;
  }

  // Query con raw SQL para casos complejos
  async getComplexUserStats() {
    const sql = `
      SELECT 
        u.id,
        u.username,
        u.email,
        COUNT(p.id) as total_posts,
        COUNT(CASE WHEN p.status = 'published' THEN 1 END) as published_posts,
        COUNT(c.id) as total_comments,
        AVG(p.rating) as avg_post_rating,
        MAX(p.created_at) as last_post_date
      FROM users u
      LEFT JOIN posts p ON u.id = p.author_id
      LEFT JOIN comments c ON u.id = c.author_id
      WHERE u.is_active = true
      GROUP BY u.id, u.username, u.email
      HAVING COUNT(p.id) > 0
      ORDER BY total_posts DESC, avg_post_rating DESC
    `;

    const results = await sequelize.query(sql, {
      type: Sequelize.QueryTypes.SELECT
    });

    return results;
  }
}
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Diseño de Base de Datos**
Diseña una base de datos para un sistema de e-commerce con usuarios, productos, categorías, pedidos y pagos.

### **Ejercicio 2: ORM con Sequelize**
Implementa operaciones CRUD completas usando Sequelize con relaciones complejas.

### **Ejercicio 3: Transacciones**
Crea un sistema de transferencia de fondos entre usuarios con transacciones.

### **Ejercicio 4: Optimización**
Analiza y optimiza queries complejos usando índices y técnicas de optimización.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [Sequelize Documentation](https://sequelize.org/)

### **Herramientas Recomendadas**
- [pgAdmin](https://www.pgadmin.org/) - GUI para PostgreSQL
- [MySQL Workbench](https://www.mysql.com/products/workbench/) - GUI para MySQL
- [Sequelize CLI](https://github.com/sequelize/cli) - CLI para Sequelize

### **Libros y Cursos**
- "PostgreSQL: Up and Running" - Regina Obe
- "MySQL Cookbook" - Paul DuBois
- "Sequelize: The Definitive Guide" - Fernando Doglio

---

*Las bases de datos SQL son fundamentales para aplicaciones que requieren consistencia de datos, relaciones complejas y transacciones. PostgreSQL y MySQL son excelentes opciones para diferentes casos de uso.* 