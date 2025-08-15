# 🔮 GraphQL: Query Language Moderno para APIs

## 🎯 **OBJETIVO**
Dominar GraphQL como alternativa moderna a REST, comprendiendo su schema, resolvers, queries, mutations, subscriptions y cómo implementarlo en aplicaciones Node.js.

---

## 📋 **¿QUÉ ES GRAPHQL?**

### **Definición**
GraphQL es un lenguaje de consulta y runtime para APIs desarrollado por Facebook en 2015. Permite a los clientes solicitar exactamente los datos que necesitan, ni más ni menos, en una sola request.

### **Ventajas sobre REST**
- **Over-fetching**: Evita obtener datos innecesarios
- **Under-fetching**: Obtiene todos los datos relacionados en una sola request
- **Schema fuerte**: Documentación automática y validación
- **Evolución**: Cambios sin versionado de API
- **Introspection**: Exploración automática de la API
- **Real-time**: Soporte nativo para subscriptions

### **Comparación con REST**
```javascript
// REST - Múltiples requests
GET /users/123          // Datos básicos del usuario
GET /users/123/posts    // Posts del usuario
GET /users/123/followers // Seguidores del usuario

// GraphQL - Una sola request
query {
  user(id: 123) {
    id
    name
    email
    posts {
      id
      title
      content
    }
    followers {
      id
      name
    }
  }
}
```

---

## 🏗️ **ARQUITECTURA DE GRAPHQL**

### **Componentes Principales**
```
Cliente → GraphQL Request → GraphQL Server → Resolvers → Data Sources
                ↓
            Schema (Type Definitions)
                ↓
            Validation & Execution
                ↓
            Response
```

### **Flujo de Request**
1. **Parse**: El servidor parsea el query GraphQL
2. **Validate**: Valida contra el schema
3. **Execute**: Ejecuta los resolvers correspondientes
4. **Response**: Retorna los datos solicitados

---

## 📝 **SCHEMA Y TYPE DEFINITIONS**

### **Tipos Básicos**
```graphql
# Tipos escalares
type User {
  id: ID!
  username: String!
  email: String!
  age: Int
  isActive: Boolean!
  createdAt: DateTime!
  updatedAt: DateTime!
}

# Tipos personalizados
scalar DateTime

# Enums
enum UserRole {
  USER
  MODERATOR
  ADMIN
}

enum PostStatus {
  DRAFT
  PUBLISHED
  ARCHIVED
}

# Input types
input CreateUserInput {
  username: String!
  email: String!
  password: String!
  firstName: String
  lastName: String
  age: Int
}

input UpdateUserInput {
  username: String
  email: String
  firstName: String
  lastName: String
  age: Int
  isActive: Boolean
}
```

### **Schema Completo**
```graphql
# Schema principal
schema {
  query: Query
  mutation: Mutation
  subscription: Subscription
}

# Query type (solo lectura)
type Query {
  # Usuarios
  users(
    first: Int
    after: String
    search: String
    role: UserRole
    isActive: Boolean
  ): UserConnection!
  
  user(id: ID!): User
  
  # Posts
  posts(
    first: Int
    after: String
    authorId: ID
    status: PostStatus
    categoryId: ID
    search: String
  ): PostConnection!
  
  post(id: ID!): Post
  
  # Categorías
  categories: [Category!]!
  category(id: ID!): Category
  
  # Tags
  tags: [Tag!]!
  tag(id: ID!): Tag
  
  # Búsqueda global
  search(query: String!, first: Int = 10): SearchResult!
}

# Mutation type (modificaciones)
type Mutation {
  # Usuarios
  createUser(input: CreateUserInput!): CreateUserPayload!
  updateUser(id: ID!, input: UpdateUserInput!): UpdateUserPayload!
  deleteUser(id: ID!): DeleteUserPayload!
  
  # Posts
  createPost(input: CreatePostInput!): CreatePostPayload!
  updatePost(id: ID!, input: UpdatePostInput!): UpdatePostPayload!
  deletePost(id: ID!): DeletePostPayload!
  
  # Comentarios
  createComment(input: CreateCommentInput!): CreateCommentPayload!
  updateComment(id: ID!, input: UpdateCommentInput!): UpdateCommentPayload!
  deleteComment(id: ID!): DeleteCommentPayload!
  
  # Autenticación
  login(email: String!, password: String!): LoginPayload!
  register(input: CreateUserInput!): RegisterPayload!
  logout: LogoutPayload!
}

# Subscription type (tiempo real)
type Subscription {
  postCreated: Post!
  postUpdated: Post!
  postDeleted: ID!
  commentAdded(postId: ID!): Comment!
  userStatusChanged: User!
}

# Tipos de entidades
type User {
  id: ID!
  username: String!
  email: String!
  firstName: String
  lastName: String
  fullName: String!
  age: Int
  isActive: Boolean!
  role: UserRole!
  profile: UserProfile
  posts(
    first: Int
    after: String
    status: PostStatus
  ): PostConnection!
  followers: [User!]!
  following: [User!]!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type UserProfile {
  id: ID!
  bio: String
  avatarUrl: String
  website: String
  location: String
  socialLinks: SocialLinks
}

type SocialLinks {
  twitter: String
  linkedin: String
  github: String
  instagram: String
}

type Post {
  id: ID!
  title: String!
  content: String!
  excerpt: String
  status: PostStatus!
  author: User!
  categories: [Category!]!
  tags: [Tag!]!
  comments(
    first: Int
    after: String
  ): CommentConnection!
  likes: Int!
  isLikedByUser(userId: ID!): Boolean!
  publishedAt: DateTime
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Category {
  id: ID!
  name: String!
  slug: String!
  description: String
  parent: Category
  children: [Category!]!
  posts(
    first: Int
    after: String
  ): PostConnection!
  postCount: Int!
  createdAt: DateTime!
}

type Tag {
  id: ID!
  name: String!
  slug: String!
  posts(
    first: Int
    after: String
  ): PostConnection!
  postCount: Int!
  createdAt: DateTime!
}

type Comment {
  id: ID!
  content: String!
  author: User!
  post: Post!
  parent: Comment
  replies: [Comment!]!
  likes: Int!
  createdAt: DateTime!
  updatedAt: DateTime!
}

# Tipos de conexión para paginación
type UserConnection {
  edges: [UserEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type UserEdge {
  node: User!
  cursor: String!
}

type PostConnection {
  edges: [PostEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type PostEdge {
  node: Post!
  cursor: String!
}

type CommentConnection {
  edges: [CommentEdge!]!
  pageInfo: PageInfo!
  totalCount: Int!
}

type CommentEdge {
  node: Comment!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  hasPreviousPage: Boolean!
  startCursor: String
  endCursor: String
}

# Tipos de respuesta para mutations
type CreateUserPayload {
  user: User
  errors: [UserError!]
  success: Boolean!
}

type UpdateUserPayload {
  user: User
  errors: [UserError!]
  success: Boolean!
}

type DeleteUserPayload {
  success: Boolean!
  errors: [UserError!]
}

type CreatePostPayload {
  post: Post
  errors: [PostError!]
  success: Boolean!
}

type UpdatePostPayload {
  post: Post
  errors: [PostError!]
  success: Boolean!
}

type DeletePostPayload {
  success: Boolean!
  errors: [PostError!]
}

type CreateCommentPayload {
  comment: Comment
  errors: [CommentError!]
  success: Boolean!
}

type UpdateCommentPayload {
  comment: Comment
  errors: [CommentError!]
  success: Boolean!
}

type DeleteCommentPayload {
  success: Boolean!
  errors: [CommentError!]
}

# Tipos de autenticación
type LoginPayload {
  user: User
  token: String
  errors: [AuthError!]
  success: Boolean!
}

type RegisterPayload {
  user: User
  token: String
  errors: [AuthError!]
  success: Boolean!
}

type LogoutPayload {
  success: Boolean!
  errors: [AuthError!]
}

# Tipos de búsqueda
type SearchResult {
  users: [User!]!
  posts: [Post!]!
  categories: [Category!]!
  tags: [Tag!]!
  totalCount: Int!
}

# Tipos de error
type UserError {
  field: String!
  message: String!
}

type PostError {
  field: String!
  message: String!
}

type CommentError {
  field: String!
  message: String!
}

type AuthError {
  field: String!
  message: String!
}
```

---

## 🔧 **IMPLEMENTACIÓN CON APOLLO SERVER**

### **Configuración Básica**
```javascript
const { ApolloServer } = require('apollo-server-express');
const { gql } = require('apollo-server-express');
const express = require('express');

const app = express();

// Schema GraphQL
const typeDefs = gql`
  type Query {
    hello: String
    users: [User!]!
    user(id: ID!): User
  }
  
  type User {
    id: ID!
    name: String!
    email: String!
  }
`;

// Resolvers
const resolvers = {
  Query: {
    hello: () => 'Hello World!',
    users: () => User.find(),
    user: (_, { id }) => User.findById(id)
  }
};

// Crear servidor Apollo
const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => ({
    user: req.user // Usuario autenticado
  }),
  formatError: (error) => {
    // Personalizar formato de errores
    return {
      message: error.message,
      code: error.extensions?.code || 'INTERNAL_SERVER_ERROR',
      path: error.path
    };
  }
});

// Aplicar middleware
server.applyMiddleware({ app });

app.listen(4000, () => {
  console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`);
});
```

### **Resolvers Avanzados**
```javascript
const resolvers = {
  Query: {
    // Resolver para usuarios con paginación
    users: async (_, { first = 10, after, search, role, isActive }) => {
      try {
        let query = {};
        
        // Filtros
        if (search) {
          query.$or = [
            { username: { $regex: search, $options: 'i' } },
            { email: { $regex: search, $options: 'i' } },
            { firstName: { $regex: search, $options: 'i' } },
            { lastName: { $regex: search, $options: 'i' } }
          ];
        }
        
        if (role) {
          query.role = role;
        }
        
        if (typeof isActive === 'boolean') {
          query.isActive = isActive;
        }
        
        // Paginación con cursor
        if (after) {
          const decodedCursor = Buffer.from(after, 'base64').toString('utf-8');
          const { id, createdAt } = JSON.parse(decodedCursor);
          
          query.$or = [
            { createdAt: { $lt: new Date(createdAt) } },
            { createdAt: new Date(createdAt), _id: { $lt: id } }
          ];
        }
        
        // Obtener usuarios
        const users = await User.find(query)
          .sort({ createdAt: -1, _id: -1 })
          .limit(first + 1); // +1 para saber si hay siguiente página
        
        const hasNextPage = users.length > first;
        const edges = users.slice(0, first).map(user => ({
          node: user,
          cursor: Buffer.from(JSON.stringify({
            id: user._id.toString(),
            createdAt: user.createdAt.toISOString()
          })).toString('base64')
        }));
        
        return {
          edges,
          pageInfo: {
            hasNextPage,
            hasPreviousPage: !!after,
            startCursor: edges[0]?.cursor,
            endCursor: edges[edges.length - 1]?.cursor
          },
          totalCount: await User.countDocuments(query)
        };
        
      } catch (error) {
        throw new Error(`Error fetching users: ${error.message}`);
      }
    },
    
    // Resolver para usuario específico
    user: async (_, { id }) => {
      try {
        const user = await User.findById(id);
        if (!user) {
          throw new Error('User not found');
        }
        return user;
      } catch (error) {
        throw new Error(`Error fetching user: ${error.message}`);
      }
    },
    
    // Resolver para posts
    posts: async (_, { first = 10, after, authorId, status, categoryId, search }) => {
      try {
        let query = {};
        
        if (authorId) query.authorId = authorId;
        if (status) query.status = status;
        if (categoryId) query.categories = categoryId;
        if (search) {
          query.$or = [
            { title: { $regex: search, $options: 'i' } },
            { content: { $regex: search, $options: 'i' } }
          ];
        }
        
        const posts = await Post.find(query)
          .sort({ createdAt: -1 })
          .limit(first + 1);
        
        const hasNextPage = posts.length > first;
        const edges = posts.slice(0, first).map(post => ({
          node: post,
          cursor: Buffer.from(JSON.stringify({
            id: post._id.toString(),
            createdAt: post.createdAt.toISOString()
          })).toString('base64')
        }));
        
        return {
          edges,
          pageInfo: {
            hasNextPage,
            hasPreviousPage: !!after,
            startCursor: edges[0]?.cursor,
            endCursor: edges[edges.length - 1]?.cursor
          },
          totalCount: await Post.countDocuments(query)
        };
        
      } catch (error) {
        throw new Error(`Error fetching posts: ${error.message}`);
      }
    }
  },
  
  // Resolvers de tipos
  User: {
    // Resolver para fullName (campo calculado)
    fullName: (parent) => {
      if (parent.firstName && parent.lastName) {
        return `${parent.firstName} ${parent.lastName}`;
      }
      return parent.username;
    },
    
    // Resolver para posts del usuario
    posts: async (parent, { first = 10, after, status }) => {
      try {
        let query = { authorId: parent._id };
        if (status) query.status = status;
        
        const posts = await Post.find(query)
          .sort({ createdAt: -1 })
          .limit(first);
        
        return posts;
      } catch (error) {
        throw new Error(`Error fetching user posts: ${error.message}`);
      }
    },
    
    // Resolver para seguidores
    followers: async (parent) => {
      try {
        const user = await User.findById(parent._id).populate('followers');
        return user.followers || [];
      } catch (error) {
        throw new Error(`Error fetching followers: ${error.message}`);
      }
    },
    
    // Resolver para usuarios que sigue
    following: async (parent) => {
      try {
        const user = await User.findById(parent._id).populate('following');
        return user.following || [];
      } catch (error) {
        throw new Error(`Error fetching following: ${error.message}`);
      }
    }
  },
  
  Post: {
    // Resolver para autor del post
    author: async (parent) => {
      try {
        return await User.findById(parent.authorId);
      } catch (error) {
        throw new Error(`Error fetching post author: ${error.message}`);
      }
    },
    
    // Resolver para categorías del post
    categories: async (parent) => {
      try {
        return await Category.find({ _id: { $in: parent.categories } });
      } catch (error) {
        throw new Error(`Error fetching post categories: ${error.message}`);
      }
    },
    
    // Resolver para tags del post
    tags: async (parent) => {
      try {
        return await Tag.find({ _id: { $in: parent.tags } });
      } catch (error) {
        throw new Error(`Error fetching post tags: ${error.message}`);
      }
    },
    
    // Resolver para comentarios del post
    comments: async (parent, { first = 10, after }) => {
      try {
        const comments = await Comment.find({ postId: parent._id })
          .sort({ createdAt: -1 })
          .limit(first);
        
        return comments;
      } catch (error) {
        throw new Error(`Error fetching post comments: ${error.message}`);
      }
    },
    
    // Resolver para likes del post
    likes: async (parent) => {
      try {
        return await Like.countDocuments({ postId: parent._id });
      } catch (error) {
        return 0;
      }
    },
    
    // Resolver para verificar si el usuario actual dio like
    isLikedByUser: async (parent, { userId }) => {
      try {
        if (!userId) return false;
        const like = await Like.findOne({ postId: parent._id, userId });
        return !!like;
      } catch (error) {
        return false;
      }
    }
  },
  
  Category: {
    // Resolver para posts de la categoría
    posts: async (parent, { first = 10, after }) => {
      try {
        const posts = await Post.find({ categories: parent._id })
          .sort({ createdAt: -1 })
          .limit(first);
        
        return posts;
      } catch (error) {
        throw new Error(`Error fetching category posts: ${error.message}`);
      }
    },
    
    // Resolver para contar posts de la categoría
    postCount: async (parent) => {
      try {
        return await Post.countDocuments({ categories: parent._id });
      } catch (error) {
        return 0;
      }
    }
  }
};
```

---

## ✏️ **MUTATIONS**

### **Resolvers de Mutations**
```javascript
const mutations = {
  Mutation: {
    // Crear usuario
    createUser: async (_, { input }) => {
      try {
        // Validar input
        if (!input.username || !input.email || !input.password) {
          return {
            user: null,
            errors: [
              {
                field: 'general',
                message: 'Username, email and password are required'
              }
            ],
            success: false
          };
        }
        
        // Verificar si el usuario ya existe
        const existingUser = await User.findOne({
          $or: [{ username: input.username }, { email: input.email }]
        });
        
        if (existingUser) {
          return {
            user: null,
            errors: [
              {
                field: existingUser.username === input.username ? 'username' : 'email',
                message: 'User already exists'
              }
            ],
            success: false
          };
        }
        
        // Hash de la contraseña
        const hashedPassword = await bcrypt.hash(input.password, 12);
        
        // Crear usuario
        const user = new User({
          ...input,
          passwordHash: hashedPassword
        });
        
        await user.save();
        
        return {
          user,
          errors: [],
          success: true
        };
        
      } catch (error) {
        return {
          user: null,
          errors: [
            {
              field: 'general',
              message: `Error creating user: ${error.message}`
            }
          ],
          success: false
        };
      }
    },
    
    // Actualizar usuario
    updateUser: async (_, { id, input }, { user }) => {
      try {
        // Verificar autenticación
        if (!user) {
          return {
            user: null,
            errors: [{ field: 'auth', message: 'Authentication required' }],
            success: false
          };
        }
        
        // Verificar autorización
        if (user._id.toString() !== id && user.role !== 'ADMIN') {
          return {
            user: null,
            errors: [{ field: 'auth', message: 'Not authorized' }],
            success: false
          };
        }
        
        // Actualizar usuario
        const updatedUser = await User.findByIdAndUpdate(
          id,
          input,
          { new: true, runValidators: true }
        );
        
        if (!updatedUser) {
          return {
            user: null,
            errors: [{ field: 'id', message: 'User not found' }],
            success: false
          };
        }
        
        return {
          user: updatedUser,
          errors: [],
          success: true
        };
        
      } catch (error) {
        return {
          user: null,
          errors: [
            {
              field: 'general',
              message: `Error updating user: ${error.message}`
            }
          ],
          success: false
        };
      }
    },
    
    // Crear post
    createPost: async (_, { input }, { user }) => {
      try {
        if (!user) {
          return {
            post: null,
            errors: [{ field: 'auth', message: 'Authentication required' }],
            success: false
          };
        }
        
        const post = new Post({
          ...input,
          authorId: user._id,
          status: input.status || 'DRAFT'
        });
        
        await post.save();
        
        return {
          post,
          errors: [],
          success: true
        };
        
      } catch (error) {
        return {
          post: null,
          errors: [
            {
              field: 'general',
              message: `Error creating post: ${error.message}`
            }
          ],
          success: false
        };
      }
    },
    
    // Login
    login: async (_, { email, password }) => {
      try {
        // Buscar usuario
        const user = await User.findOne({ email });
        if (!user) {
          return {
            user: null,
            token: null,
            errors: [{ field: 'email', message: 'Invalid credentials' }],
            success: false
          };
        }
        
        // Verificar contraseña
        const isValidPassword = await bcrypt.compare(password, user.passwordHash);
        if (!isValidPassword) {
          return {
            user: null,
            token: null,
            errors: [{ field: 'password', message: 'Invalid credentials' }],
            success: false
          };
        }
        
        // Generar token
        const token = jwt.sign(
          { userId: user._id, role: user.role },
          process.env.JWT_SECRET,
          { expiresIn: '24h' }
        );
        
        return {
          user,
          token,
          errors: [],
          success: true
        };
        
      } catch (error) {
        return {
          user: null,
          token: null,
          errors: [
            {
              field: 'general',
              message: `Error during login: ${error.message}`
            }
          ],
          success: false
        };
      }
    }
  }
};
```

---

## 📡 **SUBSCRIPTIONS**

### **Configuración de Subscriptions**
```javascript
const { ApolloServer } = require('apollo-server-express');
const { PubSub } = require('graphql-subscriptions');
const { createServer } = require('http');
const { execute, subscribe } = require('graphql');
const { SubscriptionServer } = require('subscriptions-transport-ws');

const app = express();
const httpServer = createServer(app);

// PubSub para manejar eventos
const pubsub = new PubSub();

// Schema con subscriptions
const typeDefs = gql`
  type Subscription {
    postCreated: Post!
    postUpdated: Post!
    postDeleted: ID!
    commentAdded(postId: ID!): Comment!
    userStatusChanged: User!
  }
  
  # ... resto del schema
`;

// Resolvers de subscriptions
const subscriptionResolvers = {
  Subscription: {
    postCreated: {
      subscribe: () => pubsub.asyncIterator(['POST_CREATED'])
    },
    
    postUpdated: {
      subscribe: () => pubsub.asyncIterator(['POST_UPDATED'])
    },
    
    postDeleted: {
      subscribe: () => pubsub.asyncIterator(['POST_DELETED'])
    },
    
    commentAdded: {
      subscribe: (_, { postId }) => 
        pubsub.asyncIterator(`COMMENT_ADDED_${postId}`)
    },
    
    userStatusChanged: {
      subscribe: () => pubsub.asyncIterator(['USER_STATUS_CHANGED'])
    }
  }
};

// Servidor Apollo
const server = new ApolloServer({
  typeDefs,
  resolvers: { ...resolvers, ...subscriptionResolvers },
  context: ({ req }) => ({
    user: req.user,
    pubsub
  })
});

server.applyMiddleware({ app });

// Configurar WebSocket para subscriptions
const subscriptionServer = SubscriptionServer.create(
  {
    execute,
    subscribe,
    schema: server.schema
  },
  {
    server: httpServer,
    path: server.graphqlPath
  }
);

// Publicar eventos en mutations
const mutationsWithSubscriptions = {
  Mutation: {
    createPost: async (_, { input }, { user, pubsub }) => {
      // ... lógica de creación
      
      // Publicar evento
      pubsub.publish('POST_CREATED', { postCreated: post });
      
      return { post, success: true };
    },
    
    createComment: async (_, { input }, { user, pubsub }) => {
      // ... lógica de creación
      
      // Publicar evento específico del post
      pubsub.publish(`COMMENT_ADDED_${input.postId}`, {
        commentAdded: comment
      });
      
      return { comment, success: true };
    }
  }
};
```

---

## 🔒 **AUTENTICACIÓN Y AUTORIZACIÓN**

### **Middleware de Autenticación**
```javascript
const authMiddleware = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (token) {
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      const user = await User.findById(decoded.userId);
      req.user = user;
    }
    
    next();
  } catch (error) {
    req.user = null;
    next();
  }
};

// Aplicar middleware
app.use(authMiddleware);

// Context con usuario autenticado
const server = new ApolloServer({
  typeDefs,
  resolvers,
  context: ({ req }) => ({
    user: req.user,
    pubsub
  })
});
```

### **Directivas de Autorización**
```graphql
# Schema con directivas
directive @auth(requires: UserRole!) on FIELD_DEFINITION
directive @owner on FIELD_DEFINITION

type Mutation {
  updateUser(id: ID!, input: UpdateUserInput!): UpdateUserPayload! @auth(requires: USER)
  deleteUser(id: ID!): DeleteUserPayload! @auth(requires: ADMIN)
  createPost(input: CreatePostInput!): CreatePostPayload! @auth(requires: USER)
  updatePost(id: ID!, input: UpdatePostInput!): UpdatePostPayload! @owner
}

# Implementación de directivas
const schemaDirectives = {
  auth: class AuthDirective extends SchemaDirectiveVisitor {
    visitFieldDefinition(field) {
      const { resolve = defaultFieldResolver } = field;
      const { requires } = this.args;
      
      field.resolve = async function (...args) {
        const { user } = args[2];
        
        if (!user) {
          throw new Error('Authentication required');
        }
        
        if (requires && user.role !== requires) {
          throw new Error(`Role ${requires} required`);
        }
        
        return resolve.apply(this, args);
      };
    }
  },
  
  owner: class OwnerDirective extends SchemaDirectiveVisitor {
    visitFieldDefinition(field) {
      const { resolve = defaultFieldResolver } = field;
      
      field.resolve = async function (...args) {
        const { user } = args[2];
        const { id } = args[1];
        
        if (!user) {
          throw new Error('Authentication required');
        }
        
        // Verificar si el usuario es dueño del recurso
        const resource = await getResourceById(id);
        if (resource.authorId.toString() !== user._id.toString()) {
          throw new Error('Not authorized');
        }
        
        return resolve.apply(this, args);
      };
    }
  }
};
```

---

## 🧪 **TESTING DE GRAPHQL**

### **Testing con Apollo Server Testing**
```javascript
const { createTestClient } = require('apollo-server-testing');
const { gql } = require('apollo-server-express');

describe('GraphQL API', () => {
  let testClient;
  
  beforeAll(async () => {
    const server = new ApolloServer({
      typeDefs,
      resolvers,
      context: () => ({
        user: { _id: 'test-user-id', role: 'USER' }
      })
    });
    
    testClient = createTestClient(server);
  });
  
  describe('Queries', () => {
    it('should fetch users', async () => {
      const GET_USERS = gql`
        query GetUsers {
          users {
            edges {
              node {
                id
                username
                email
              }
            }
            totalCount
          }
        }
      `;
      
      const response = await testClient.query({ query: GET_USERS });
      
      expect(response.data.users).toBeDefined();
      expect(response.data.users.edges).toBeInstanceOf(Array);
      expect(response.data.users.totalCount).toBeGreaterThanOrEqual(0);
    });
    
    it('should fetch user by ID', async () => {
      const GET_USER = gql`
        query GetUser($id: ID!) {
          user(id: $id) {
            id
            username
            email
            fullName
          }
        }
      `;
      
      const response = await testClient.query({
        query: GET_USER,
        variables: { id: 'test-user-id' }
      });
      
      expect(response.data.user).toBeDefined();
      expect(response.data.user.id).toBe('test-user-id');
    });
  });
  
  describe('Mutations', () => {
    it('should create user', async () => {
      const CREATE_USER = gql`
        mutation CreateUser($input: CreateUserInput!) {
          createUser(input: $input) {
            user {
              id
              username
              email
            }
            success
            errors {
              field
              message
            }
          }
        }
      `;
      
      const response = await testClient.mutate({
        mutation: CREATE_USER,
        variables: {
          input: {
            username: 'testuser',
            email: 'test@example.com',
            password: 'password123'
          }
        }
      });
      
      expect(response.data.createUser.success).toBe(true);
      expect(response.data.createUser.user).toBeDefined();
      expect(response.data.createUser.errors).toHaveLength(0);
    });
  });
});
```

---

## 🎯 **EJERCICIOS PRÁCTICOS**

### **Ejercicio 1: Schema GraphQL**
Diseña un schema GraphQL completo para un sistema de blog con usuarios, posts, comentarios y categorías.

### **Ejercicio 2: Resolvers y Mutations**
Implementa resolvers para queries y mutations con validación y manejo de errores.

### **Ejercicio 3: Subscriptions**
Crea un sistema de subscriptions para notificaciones en tiempo real.

### **Ejercicio 4: Testing**
Escribe tests completos para tu API GraphQL.

---

## 📚 **RECURSOS ADICIONALES**

### **Documentación Oficial**
- [GraphQL Specification](https://spec.graphql.org/)
- [Apollo Server Documentation](https://www.apollographql.com/docs/apollo-server/)
- [GraphQL Tools](https://www.graphql-tools.com/)

### **Herramientas Recomendadas**
- [GraphQL Playground](https://github.com/prisma/graphql-playground) - IDE para GraphQL
- [Apollo Studio](https://studio.apollographql.com/) - Plataforma de desarrollo
- [GraphQL Code Generator](https://www.graphql-code-generator.com/) - Generación de código

### **Libros y Cursos**
- "GraphQL in Action" - Samer Buna
- "Learning GraphQL" - Eve Porcello
- GraphQL Course (Apollo)

---

*GraphQL representa el futuro de las APIs, ofreciendo flexibilidad, eficiencia y una experiencia de desarrollo superior. Dominar GraphQL te permitirá crear APIs más inteligentes y adaptables.* 