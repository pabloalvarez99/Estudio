# ⚛️ Ejercicios React: Componentes, Estado y Hooks

## 🎯 **OBJETIVO**
Practicar y reforzar los conceptos de React aprendidos, implementando componentes funcionales, hooks, manejo de estado y patrones de React moderno.

---

## ⚛️ **EJERCICIO 1: SISTEMA DE BLOG CON REACT**

### **Descripción**
Crea un sistema de blog completo usando React con componentes funcionales, hooks personalizados y manejo de estado.

### **Requisitos**
- Componentes funcionales con hooks
- useState y useEffect para estado
- Componentes reutilizables
- Routing básico con React Router
- Hooks personalizados

### **Estructura del Proyecto**
```
blog-app/
├── public/
│   └── index.html
├── src/
│   ├── components/
│   │   ├── Header.jsx
│   │   ├── PostCard.jsx
│   │   ├── PostForm.jsx
│   │   ├── CommentList.jsx
│   │   └── Loading.jsx
│   ├── hooks/
│   │   ├── usePosts.js
│   │   ├── useComments.js
│   │   └── useLocalStorage.js
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── PostDetail.jsx
│   │   ├── CreatePost.jsx
│   │   └── EditPost.jsx
│   ├── utils/
│   │   └── api.js
│   ├── App.jsx
│   └── index.js
└── package.json
```

### **App.jsx**
```jsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header';
import Home from './pages/Home';
import PostDetail from './pages/PostDetail';
import CreatePost from './pages/CreatePost';
import EditPost from './pages/EditPost';
import './App.css';

function App() {
  return (
    <Router>
      <div className="App">
        <Header />
        <main className="main-content">
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/post/:id" element={<PostDetail />} />
            <Route path="/create" element={<CreatePost />} />
            <Route path="/edit/:id" element={<EditPost />} />
          </Routes>
        </main>
      </div>
    </Router>
  );
}

export default App;
```

### **usePosts.js (Hook Personalizado)**
```jsx
import { useState, useEffect, useCallback } from 'react';

export function usePosts() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Cargar posts
  const fetchPosts = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      
      // Simular API call
      const response = await fetch('/api/posts');
      if (!response.ok) throw new Error('Error al cargar posts');
      
      const data = await response.json();
      setPosts(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }, []);

  // Crear nuevo post
  const createPost = useCallback(async (postData) => {
    try {
      const response = await fetch('/api/posts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(postData)
      });

      if (!response.ok) throw new Error('Error al crear post');
      
      const newPost = await response.json();
      setPosts(prev => [newPost, ...prev]);
      return newPost;
    } catch (err) {
      throw err;
    }
  }, []);

  // Actualizar post
  const updatePost = useCallback(async (id, updates) => {
    try {
      const response = await fetch(`/api/posts/${id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(updates)
      });

      if (!response.ok) throw new Error('Error al actualizar post');
      
      const updatedPost = await response.json();
      setPosts(prev => prev.map(post => 
        post.id === id ? updatedPost : post
      ));
      return updatedPost;
    } catch (err) {
      throw err;
    }
  }, []);

  // Eliminar post
  const deletePost = useCallback(async (id) => {
    try {
      const response = await fetch(`/api/posts/${id}`, {
        method: 'DELETE'
      });

      if (!response.ok) throw new Error('Error al eliminar post');
      
      setPosts(prev => prev.filter(post => post.id !== id));
      return true;
    } catch (err) {
      throw err;
    }
  }, []);

  // Buscar posts
  const searchPosts = useCallback((query) => {
    if (!query.trim()) return posts;
    
    return posts.filter(post => 
      post.title.toLowerCase().includes(query.toLowerCase()) ||
      post.content.toLowerCase().includes(query.toLowerCase())
    );
  }, [posts]);

  // Obtener post por ID
  const getPostById = useCallback((id) => {
    return posts.find(post => post.id === parseInt(id));
  }, [posts]);

  useEffect(() => {
    fetchPosts();
  }, [fetchPosts]);

  return {
    posts,
    loading,
    error,
    createPost,
    updatePost,
    deletePost,
    searchPosts,
    getPostById,
    refetch: fetchPosts
  };
}
```

### **PostCard.jsx (Componente Reutilizable)**
```jsx
import React, { useState } from 'react';
import { Link } from 'react-router-dom';
import { formatDate } from '../utils/helpers';
import './PostCard.css';

const PostCard = ({ post, onDelete, onEdit }) => {
  const [showActions, setShowActions] = useState(false);
  const [isDeleting, setIsDeleting] = useState(false);

  const handleDelete = async () => {
    if (window.confirm('¿Estás seguro de que quieres eliminar este post?')) {
      setIsDeleting(true);
      try {
        await onDelete(post.id);
      } catch (error) {
        console.error('Error al eliminar:', error);
      } finally {
        setIsDeleting(false);
      }
    }
  };

  const handleEdit = () => {
    onEdit(post);
  };

  return (
    <article className="post-card">
      <div className="post-header">
        <h3 className="post-title">
          <Link to={`/post/${post.id}`}>{post.title}</Link>
        </h3>
        
        <div className="post-meta">
          <span className="post-date">{formatDate(post.createdAt)}</span>
          <span className="post-author">por {post.author}</span>
        </div>
      </div>

      <div className="post-content">
        <p>{post.excerpt || post.content.substring(0, 150)}...</p>
      </div>

      <div className="post-footer">
        <div className="post-tags">
          {post.tags?.map(tag => (
            <span key={tag} className="tag">{tag}</span>
          ))}
        </div>

        <div className="post-actions">
          <button
            className="btn btn-secondary"
            onClick={() => setShowActions(!showActions)}
          >
            ⋯
          </button>
          
          {showActions && (
            <div className="actions-dropdown">
              <button 
                className="action-btn edit-btn"
                onClick={handleEdit}
              >
                ✏️ Editar
              </button>
              <button 
                className="action-btn delete-btn"
                onClick={handleDelete}
                disabled={isDeleting}
              >
                {isDeleting ? '🗑️ Eliminando...' : '🗑️ Eliminar'}
              </button>
            </div>
          )}
        </div>
      </div>

      {post.comments && (
        <div className="post-comments">
          <span className="comments-count">
            💬 {post.comments.length} comentarios
          </span>
        </div>
      )}
    </article>
  );
};

export default PostCard;
```

### **Home.jsx (Página Principal)**
```jsx
import React, { useState, useMemo } from 'react';
import { Link } from 'react-router-dom';
import PostCard from '../components/PostCard';
import Loading from '../components/Loading';
import { usePosts } from '../hooks/usePosts';
import './Home.css';

const Home = () => {
  const { posts, loading, error, deletePost, updatePost } = usePosts();
  const [searchQuery, setSearchQuery] = useState('');
  const [sortBy, setSortBy] = useState('newest');
  const [filterBy, setFilterBy] = useState('all');

  // Filtrar y ordenar posts
  const filteredAndSortedPosts = useMemo(() => {
    let filtered = posts;

    // Aplicar filtros
    if (filterBy === 'published') {
      filtered = filtered.filter(post => post.status === 'published');
    } else if (filterBy === 'draft') {
      filtered = filtered.filter(post => post.status === 'draft');
    }

    // Aplicar búsqueda
    if (searchQuery) {
      filtered = filtered.filter(post =>
        post.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
        post.content.toLowerCase().includes(searchQuery.toLowerCase())
      );
    }

    // Aplicar ordenamiento
    const sorted = [...filtered].sort((a, b) => {
      switch (sortBy) {
        case 'newest':
          return new Date(b.createdAt) - new Date(a.createdAt);
        case 'oldest':
          return new Date(a.createdAt) - new Date(b.createdAt);
        case 'title':
          return a.title.localeCompare(b.title);
        case 'popular':
          return (b.likes || 0) - (a.likes || 0);
        default:
          return 0;
      }
    });

    return sorted;
  }, [posts, searchQuery, sortBy, filterBy]);

  // Estadísticas
  const stats = useMemo(() => {
    const total = posts.length;
    const published = posts.filter(p => p.status === 'published').length;
    const drafts = posts.filter(p => p.status === 'draft').length;
    const totalViews = posts.reduce((sum, p) => sum + (p.views || 0), 0);
    const totalLikes = posts.reduce((sum, p) => sum + (p.likes || 0), 0);

    return { total, published, drafts, totalViews, totalLikes };
  }, [posts]);

  if (loading) return <Loading />;
  if (error) return <div className="error">Error: {error}</div>;

  return (
    <div className="home-page">
      {/* Header con estadísticas */}
      <header className="page-header">
        <h1>Mi Blog Personal</h1>
        <div className="stats-grid">
          <div className="stat-card">
            <span className="stat-number">{stats.total}</span>
            <span className="stat-label">Posts</span>
          </div>
          <div className="stat-card">
            <span className="stat-number">{stats.published}</span>
            <span className="stat-label">Publicados</span>
          </div>
          <div className="stat-card">
            <span className="stat-number">{stats.totalViews}</span>
            <span className="stat-label">Vistas</span>
          </div>
          <div className="stat-card">
            <span className="stat-number">{stats.totalLikes}</span>
            <span className="stat-label">Likes</span>
          </div>
        </div>
      </header>

      {/* Controles de filtrado y búsqueda */}
      <div className="controls">
        <div className="search-controls">
          <input
            type="text"
            placeholder="Buscar posts..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="search-input"
          />
        </div>

        <div className="filter-controls">
          <select 
            value={filterBy} 
            onChange={(e) => setFilterBy(e.target.value)}
            className="filter-select"
          >
            <option value="all">Todos los posts</option>
            <option value="published">Solo publicados</option>
            <option value="draft">Solo borradores</option>
          </select>

          <select 
            value={sortBy} 
            onChange={(e) => setSortBy(e.target.value)}
            className="sort-select"
          >
            <option value="newest">Más recientes</option>
            <option value="oldest">Más antiguos</option>
            <option value="title">Por título</option>
            <option value="popular">Más populares</option>
          </select>
        </div>

        <Link to="/create" className="btn btn-primary">
          ✏️ Nuevo Post
        </Link>
      </div>

      {/* Lista de posts */}
      <section className="posts-section">
        {filteredAndSortedPosts.length === 0 ? (
          <div className="no-posts">
            <p>No se encontraron posts que coincidan con los criterios.</p>
            {searchQuery && (
              <button 
                onClick={() => setSearchQuery('')}
                className="btn btn-secondary"
              >
                Limpiar búsqueda
              </button>
            )}
          </div>
        ) : (
          <div className="posts-grid">
            {filteredAndSortedPosts.map(post => (
              <PostCard
                key={post.id}
                post={post}
                onDelete={deletePost}
                onEdit={(post) => {
                  // Navegar a la página de edición
                  window.location.href = `/edit/${post.id}`;
                }}
              />
            ))}
          </div>
        )}
      </section>

      {/* Paginación */}
      {filteredAndSortedPosts.length > 10 && (
        <div className="pagination">
          <button className="btn btn-secondary">← Anterior</button>
          <span className="page-info">Página 1 de 1</span>
          <button className="btn btn-secondary">Siguiente →</button>
        </div>
      )}
    </div>
  );
};

export default Home;
```

---

## ⚛️ **EJERCICIO 2: HOOKS PERSONALIZADOS AVANZADOS**

### **Descripción**
Crea hooks personalizados avanzados para funcionalidades comunes como formularios, API calls, y manejo de estado local.

### **Requisitos**
- Hooks personalizados reutilizables
- Manejo de estado complejo
- Integración con APIs
- Validación de formularios
- Cache y optimización

### **useForm.js (Hook para Formularios)**
```jsx
import { useState, useCallback, useEffect } from 'react';

export function useForm(initialValues = {}, validationRules = {}) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [isValid, setIsValid] = useState(false);

  // Validar campo individual
  const validateField = useCallback((field, value) => {
    const rule = validationRules[field];
    if (!rule) return '';

    if (typeof rule === 'function') {
      return rule(value, values);
    }

    if (typeof rule === 'object') {
      const { required, minLength, maxLength, pattern, custom } = rule;
      
      if (required && !value) return 'Este campo es requerido';
      if (minLength && value.length < minLength) {
        return `Mínimo ${minLength} caracteres`;
      }
      if (maxLength && value.length > maxLength) {
        return `Máximo ${maxLength} caracteres`;
      }
      if (pattern && !pattern.test(value)) {
        return rule.message || 'Formato inválido';
      }
      if (custom) return custom(value, values);
    }

    return '';
  }, [validationRules, values]);

  // Validar todo el formulario
  const validateForm = useCallback(() => {
    const newErrors = {};
    let formIsValid = true;

    Object.keys(validationRules).forEach(field => {
      const error = validateField(field, values[field]);
      if (error) {
        newErrors[field] = error;
        formIsValid = false;
      }
    });

    setErrors(newErrors);
    setIsValid(formIsValid);
    return formIsValid;
  }, [validationRules, values, validateField]);

  // Actualizar valor de campo
  const setValue = useCallback((field, value) => {
    setValues(prev => ({ ...prev, [field]: value }));
    
    // Limpiar error del campo
    if (errors[field]) {
      setErrors(prev => ({ ...prev, [field]: '' }));
    }
  }, [errors]);

  // Actualizar múltiples campos
  const setValuesBatch = useCallback((updates) => {
    setValues(prev => ({ ...prev, ...updates }));
  }, []);

  // Marcar campo como tocado
  const setFieldTouched = useCallback((field) => {
    setTouched(prev => ({ ...prev, [field]: true }));
  }, []);

  // Manejar cambio de campo
  const handleChange = useCallback((field, value) => {
    setValue(field, value);
    setFieldTouched(field);
  }, [setValue, setFieldTouched]);

  // Manejar blur de campo
  const handleBlur = useCallback((field) => {
    setFieldTouched(field);
    const error = validateField(field, values[field]);
    setErrors(prev => ({ ...prev, [field]: error }));
  }, [setFieldTouched, validateField, values]);

  // Resetear formulario
  const resetForm = useCallback(() => {
    setValues(initialValues);
    setErrors({});
    setTouched({});
    setIsSubmitting(false);
    setIsValid(false);
  }, [initialValues]);

  // Manejar envío del formulario
  const handleSubmit = useCallback(async (onSubmit) => {
    if (!validateForm()) return false;

    setIsSubmitting(true);
    try {
      const result = await onSubmit(values);
      return result;
    } catch (error) {
      console.error('Error en submit:', error);
      return false;
    } finally {
      setIsSubmitting(false);
    }
  }, [validateForm, values]);

  // Validar formulario cuando cambian los valores
  useEffect(() => {
    validateForm();
  }, [validateForm]);

  return {
    values,
    errors,
    touched,
    isSubmitting,
    isValid,
    setValue,
    setValuesBatch,
    setFieldTouched,
    handleChange,
    handleBlur,
    handleSubmit,
    resetForm,
    validateField,
    validateForm
  };
}
```

### **useApi.js (Hook para API Calls)**
```jsx
import { useState, useEffect, useCallback, useRef } from 'react';

export function useApi(endpoint, options = {}) {
  const {
    method = 'GET',
    body = null,
    headers = {},
    immediate = true,
    cache = false,
    cacheTTL = 5 * 60 * 1000, // 5 minutos
    retries = 3,
    retryDelay = 1000,
    onSuccess = null,
    onError = null
  } = options;

  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [lastFetch, setLastFetch] = useState(null);
  
  const cacheRef = useRef(new Map());
  const abortControllerRef = useRef(null);

  // Función para ejecutar la llamada a la API
  const execute = useCallback(async (customBody = null, customHeaders = {}) => {
    // Cancelar request anterior si existe
    if (abortControllerRef.current) {
      abortControllerRef.current.abort();
    }

    // Crear nuevo abort controller
    abortControllerRef.current = new AbortController();

    setLoading(true);
    setError(null);

    try {
      const requestBody = customBody || body;
      const requestHeaders = {
        'Content-Type': 'application/json',
        ...headers,
        ...customHeaders
      };

      const config = {
        method,
        headers: requestHeaders,
        signal: abortControllerRef.current.signal
      };

      if (requestBody && method !== 'GET') {
        config.body = JSON.stringify(requestBody);
      }

      // Verificar cache para GET requests
      if (method === 'GET' && cache) {
        const cacheKey = `${endpoint}_${JSON.stringify(requestHeaders)}`;
        const cached = cacheRef.current.get(cacheKey);
        
        if (cached && Date.now() - cached.timestamp < cacheTTL) {
          setData(cached.data);
          setLoading(false);
          setLastFetch(new Date());
          return cached.data;
        }
      }

      // Intentar request con retry
      let lastError;
      for (let attempt = 1; attempt <= retries; attempt++) {
        try {
          const response = await fetch(endpoint, config);
          
          if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
          }

          const result = await response.json();
          
          // Cachear respuesta exitosa
          if (method === 'GET' && cache) {
            const cacheKey = `${endpoint}_${JSON.stringify(requestHeaders)}`;
            cacheRef.current.set(cacheKey, {
              data: result,
              timestamp: Date.now()
            });
          }

          setData(result);
          setLastFetch(new Date());
          
          if (onSuccess) {
            onSuccess(result);
          }

          return result;
        } catch (err) {
          lastError = err;
          
          if (err.name === 'AbortError') {
            throw new Error('Request cancelled');
          }
          
          if (attempt < retries) {
            await new Promise(resolve => setTimeout(resolve, retryDelay * attempt));
            continue;
          }
        }
      }

      throw lastError;
    } catch (err) {
      const errorMessage = err.message || 'Error en la llamada a la API';
      setError(errorMessage);
      
      if (onError) {
        onError(err);
      }
      
      throw err;
    } finally {
      setLoading(false);
    }
  }, [endpoint, method, body, headers, cache, cacheTTL, retries, retryDelay, onSuccess, onError]);

  // Función para cancelar request
  const cancel = useCallback(() => {
    if (abortControllerRef.current) {
      abortControllerRef.current.abort();
    }
  }, []);

  // Función para limpiar cache
  const clearCache = useCallback(() => {
    cacheRef.current.clear();
  }, []);

  // Función para invalidar cache específico
  const invalidateCache = useCallback((pattern) => {
    for (const [key] of cacheRef.current) {
      if (key.includes(pattern)) {
        cacheRef.current.delete(key);
      }
    }
  }, []);

  // Ejecutar inmediatamente si se especifica
  useEffect(() => {
    if (immediate && method === 'GET') {
      execute();
    }

    // Cleanup al desmontar
    return () => {
      if (abortControllerRef.current) {
        abortControllerRef.current.abort();
      }
    };
  }, [immediate, method, execute]);

  return {
    data,
    loading,
    error,
    lastFetch,
    execute,
    cancel,
    clearCache,
    invalidateCache
  };
}
```

### **useLocalStorage.js (Hook para LocalStorage)**
```jsx
import { useState, useEffect, useCallback } from 'react';

export function useLocalStorage(key, initialValue) {
  // Función para obtener valor del localStorage
  const getStoredValue = useCallback(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  }, [key, initialValue]);

  // Estado local
  const [storedValue, setStoredValue] = useState(getStoredValue);

  // Función para establecer valor
  const setValue = useCallback((value) => {
    try {
      // Permitir que value sea una función para tener la misma API que useState
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      
      // Guardar en estado local
      setStoredValue(valueToStore);
      
      // Guardar en localStorage
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
      
      // Disparar evento personalizado para sincronización entre tabs
      window.dispatchEvent(new StorageEvent('local-storage', { key, newValue: valueToStore }));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  }, [key, storedValue]);

  // Función para remover valor
  const removeValue = useCallback(() => {
    try {
      setStoredValue(initialValue);
      window.localStorage.removeItem(key);
      window.dispatchEvent(new StorageEvent('local-storage', { key, newValue: null }));
    } catch (error) {
      console.error(`Error removing localStorage key "${key}":`, error);
    }
  }, [key, initialValue]);

  // Función para limpiar todo el localStorage
  const clearAll = useCallback(() => {
    try {
      window.localStorage.clear();
      setStoredValue(initialValue);
      window.dispatchEvent(new StorageEvent('local-storage', { key: null, newValue: null }));
    } catch (error) {
      console.error('Error clearing localStorage:', error);
    }
  }, [initialValue]);

  // Escuchar cambios en localStorage de otras tabs
  useEffect(() => {
    const handleStorageChange = (event) => {
      if (event.key === key) {
        try {
          const newValue = event.newValue ? JSON.parse(event.newValue) : initialValue;
          setStoredValue(newValue);
        } catch (error) {
          console.error('Error parsing localStorage value:', error);
        }
      }
    };

    // Escuchar eventos de storage (cambios de otras tabs)
    window.addEventListener('storage', handleStorageChange);
    
    // Escuchar eventos personalizados (cambios de la misma tab)
    window.addEventListener('local-storage', handleStorageChange);

    return () => {
      window.removeEventListener('storage', handleStorageChange);
      window.removeEventListener('local-storage', handleStorageChange);
    };
  }, [key, initialValue]);

  // Función para obtener múltiples valores
  const getMultiple = useCallback((keys) => {
    const result = {};
    keys.forEach(k => {
      try {
        const item = window.localStorage.getItem(k);
        result[k] = item ? JSON.parse(item) : null;
      } catch (error) {
        console.error(`Error reading localStorage key "${k}":`, error);
        result[k] = null;
      }
    });
    return result;
  }, []);

  // Función para establecer múltiples valores
  const setMultiple = useCallback((values) => {
    Object.entries(values).forEach(([k, v]) => {
      try {
        window.localStorage.setItem(k, JSON.stringify(v));
      } catch (error) {
        console.error(`Error setting localStorage key "${k}":`, error);
      }
    });
    
    // Actualizar estado local solo para la key principal
    if (values[key] !== undefined) {
      setStoredValue(values[key]);
    }
  }, [key]);

  return {
    value: storedValue,
    setValue,
    removeValue,
    clearAll,
    getMultiple,
    setMultiple
  };
}
```

---

## ⚛️ **EJERCICIO 3: COMPONENTE DE TABLA AVANZADO**

### **Descripción**
Crea un componente de tabla reutilizable con funcionalidades avanzadas como ordenamiento, filtrado, paginación y selección múltiple.

### **Requisitos**
- Componente de tabla genérico
- Ordenamiento por columnas
- Filtrado avanzado
- Paginación
- Selección múltiple
- Columnas personalizables

### **DataTable.jsx**
```jsx
import React, { useState, useMemo, useCallback } from 'react';
import './DataTable.css';

const DataTable = ({
  data = [],
  columns = [],
  pageSize = 10,
  sortable = true,
  filterable = true,
  selectable = true,
  onRowSelect = null,
  onRowClick = null,
  className = ''
}) => {
  // Estado local
  const [currentPage, setCurrentPage] = useState(1);
  const [sortConfig, setSortConfig] = useState({ key: null, direction: 'asc' });
  const [filters, setFilters] = useState({});
  const [selectedRows, setSelectedRows] = useState(new Set());
  const [searchQuery, setSearchQuery] = useState('');

  // Calcular datos procesados
  const processedData = useMemo(() => {
    let result = [...data];

    // Aplicar búsqueda global
    if (searchQuery) {
      result = result.filter(row =>
        Object.values(row).some(value =>
          String(value).toLowerCase().includes(searchQuery.toLowerCase())
        )
      );
    }

    // Aplicar filtros de columna
    Object.entries(filters).forEach(([key, value]) => {
      if (value) {
        result = result.filter(row => {
          const cellValue = row[key];
          if (typeof cellValue === 'string') {
            return cellValue.toLowerCase().includes(value.toLowerCase());
          }
          if (typeof cellValue === 'number') {
            return cellValue === parseFloat(value) || cellValue.toString().includes(value);
          }
          if (typeof cellValue === 'boolean') {
            return cellValue.toString() === value;
          }
          return true;
        });
      }
    });

    // Aplicar ordenamiento
    if (sortConfig.key) {
      result.sort((a, b) => {
        const aValue = a[sortConfig.key];
        const bValue = b[sortConfig.key];

        if (aValue === null || aValue === undefined) return 1;
        if (bValue === null || bValue === undefined) return -1;

        if (typeof aValue === 'string') {
          return sortConfig.direction === 'asc' 
            ? aValue.localeCompare(bValue)
            : bValue.localeCompare(aValue);
        }

        if (typeof aValue === 'number') {
          return sortConfig.direction === 'asc' ? aValue - bValue : bValue - aValue;
        }

        if (typeof aValue === 'boolean') {
          return sortConfig.direction === 'asc' 
            ? (aValue === bValue ? 0 : aValue ? -1 : 1)
            : (aValue === bValue ? 0 : aValue ? 1 : -1);
        }

        return 0;
      });
    }

    return result;
  }, [data, searchQuery, filters, sortConfig]);

  // Calcular paginación
  const paginationData = useMemo(() => {
    const totalPages = Math.ceil(processedData.length / pageSize);
    const startIndex = (currentPage - 1) * pageSize;
    const endIndex = startIndex + pageSize;
    const currentData = processedData.slice(startIndex, endIndex);

    return {
      totalPages,
      startIndex,
      endIndex,
      currentData,
      hasNextPage: currentPage < totalPages,
      hasPrevPage: currentPage > 1
    };
  }, [processedData, currentPage, pageSize]);

  // Handlers
  const handleSort = useCallback((key) => {
    setSortConfig(prev => ({
      key,
      direction: prev.key === key && prev.direction === 'asc' ? 'desc' : 'asc'
    }));
    setCurrentPage(1);
  }, []);

  const handleFilter = useCallback((key, value) => {
    setFilters(prev => ({
      ...prev,
      [key]: value
    }));
    setCurrentPage(1);
  }, []);

  const handleSearch = useCallback((query) => {
    setSearchQuery(query);
    setCurrentPage(1);
  }, []);

  const handlePageChange = useCallback((page) => {
    setCurrentPage(page);
  }, []);

  const handleRowSelect = useCallback((rowId, checked) => {
    const newSelected = new Set(selectedRows);
    if (checked) {
      newSelected.add(rowId);
    } else {
      newSelected.delete(rowId);
    }
    setSelectedRows(newSelected);
    
    if (onRowSelect) {
      onRowSelect(Array.from(newSelected));
    }
  }, [selectedRows, onRowSelect]);

  const handleSelectAll = useCallback((checked) => {
    if (checked) {
      const allIds = paginationData.currentData.map(row => row.id);
      setSelectedRows(new Set(allIds));
      if (onRowSelect) {
        onRowSelect(allIds);
      }
    } else {
      setSelectedRows(new Set());
      if (onRowSelect) {
        onRowSelect([]);
      }
    }
  }, [paginationData.currentData, onRowSelect]);

  const handleRowClick = useCallback((row) => {
    if (onRowClick) {
      onRowClick(row);
    }
  }, [onRowClick]);

  // Renderizar header de tabla
  const renderTableHeader = () => (
    <thead>
      <tr>
        {selectable && (
          <th className="select-all-header">
            <input
              type="checkbox"
              checked={selectedRows.size === paginationData.currentData.length && paginationData.currentData.length > 0}
              onChange={(e) => handleSelectAll(e.target.checked)}
            />
          </th>
        )}
        
        {columns.map(column => (
          <th 
            key={column.key}
            className={`column-header ${sortable ? 'sortable' : ''}`}
            onClick={() => sortable && handleSort(column.key)}
          >
            <div className="header-content">
              <span>{column.title}</span>
              {sortable && sortConfig.key === column.key && (
                <span className="sort-indicator">
                  {sortConfig.direction === 'asc' ? '↑' : '↓'}
                </span>
              )}
            </div>
            
            {filterable && column.filterable !== false && (
              <div className="filter-control">
                <input
                  type="text"
                  placeholder={`Filtrar ${column.title}...`}
                  value={filters[column.key] || ''}
                  onChange={(e) => handleFilter(column.key, e.target.value)}
                  className="filter-input"
                />
              </div>
            )}
          </th>
        ))}
      </tr>
    </thead>
  );

  // Renderizar filas de tabla
  const renderTableRows = () => (
    <tbody>
      {paginationData.currentData.map((row, index) => (
        <tr 
          key={row.id || index}
          className={`table-row ${selectedRows.has(row.id) ? 'selected' : ''}`}
          onClick={() => handleRowClick(row)}
        >
          {selectable && (
            <td className="select-cell">
              <input
                type="checkbox"
                checked={selectedRows.has(row.id)}
                onChange={(e) => handleRowSelect(row.id, e.target.checked)}
                onClick={(e) => e.stopPropagation()}
              />
            </td>
          )}
          
          {columns.map(column => (
            <td key={column.key} className="table-cell">
              {column.render ? column.render(row[column.key], row) : row[column.key]}
            </td>
          ))}
        </tr>
      ))}
    </tbody>
  );

  // Renderizar paginación
  const renderPagination = () => {
    if (paginationData.totalPages <= 1) return null;

    const pageNumbers = [];
    const maxVisiblePages = 5;
    
    let startPage = Math.max(1, currentPage - Math.floor(maxVisiblePages / 2));
    let endPage = Math.min(paginationData.totalPages, startPage + maxVisiblePages - 1);
    
    if (endPage - startPage + 1 < maxVisiblePages) {
      startPage = Math.max(1, endPage - maxVisiblePages + 1);
    }

    for (let i = startPage; i <= endPage; i++) {
      pageNumbers.push(i);
    }

    return (
      <div className="pagination">
        <button
          className="pagination-btn"
          disabled={!paginationData.hasPrevPage}
          onClick={() => handlePageChange(currentPage - 1)}
        >
          ← Anterior
        </button>

        {startPage > 1 && (
          <>
            <button
              className="pagination-btn"
              onClick={() => handlePageChange(1)}
            >
              1
            </button>
            {startPage > 2 && <span className="pagination-ellipsis">...</span>}
          </>
        )}

        {pageNumbers.map(page => (
          <button
            key={page}
            className={`pagination-btn ${page === currentPage ? 'active' : ''}`}
            onClick={() => handlePageChange(page)}
          >
            {page}
          </button>
        ))}

        {endPage < paginationData.totalPages && (
          <>
            {endPage < paginationData.totalPages - 1 && (
              <span className="pagination-ellipsis">...</span>
            )}
            <button
              className="pagination-btn"
              onClick={() => handlePageChange(paginationData.totalPages)}
            >
              {paginationData.totalPages}
            </button>
          </>
        )}

        <button
          className="pagination-btn"
          disabled={!paginationData.hasNextPage}
          onClick={() => handlePageChange(currentPage + 1)}
        >
          Siguiente →
        </button>
      </div>
    );
  };

  return (
    <div className={`data-table-container ${className}`}>
      {/* Controles de tabla */}
      <div className="table-controls">
        <div className="search-control">
          <input
            type="text"
            placeholder="Buscar en toda la tabla..."
            value={searchQuery}
            onChange={(e) => handleSearch(e.target.value)}
            className="search-input"
          />
        </div>

        <div className="table-info">
          Mostrando {paginationData.startIndex + 1}-{Math.min(paginationData.endIndex, processedData.length)} de {processedData.length} registros
        </div>
      </div>

      {/* Tabla */}
      <div className="table-wrapper">
        <table className="data-table">
          {renderTableHeader()}
          {renderTableRows()}
        </table>
      </div>

      {/* Paginación */}
      {renderPagination()}

      {/* Información de selección */}
      {selectable && selectedRows.size > 0 && (
        <div className="selection-info">
          {selectedRows.size} fila(s) seleccionada(s)
          <button
            className="clear-selection-btn"
            onClick={() => setSelectedRows(new Set())}
          >
            Limpiar selección
          </button>
        </div>
      )}
    </div>
  );
};

export default DataTable;
```

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 4: Sistema de Notificaciones**
Crea un sistema de notificaciones con diferentes tipos y animaciones usando React.

### **Ejercicio 5: Modal y Overlay System**
Implementa un sistema de modales reutilizable con portales y gestión de estado.

### **Ejercicio 6: Infinite Scroll**
Crea un componente de lista con scroll infinito y virtualización.

### **Ejercicio 7: Drag and Drop**
Implementa funcionalidad de arrastrar y soltar para reordenar elementos.

### **Ejercicio 8: Formulario Dinámico**
Crea un sistema de formularios dinámicos con validación y campos condicionales.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Componentes y Hooks (30%)**
- Uso correcto de hooks
- Componentes reutilizables
- Separación de responsabilidades

### **Estado y Lógica (25%)**
- Manejo de estado apropiado
- Lógica de negocio clara
- Performance optimizada

### **Patrones de React (25%)**
- Patrones modernos de React
- Hooks personalizados
- Componentes funcionales

### **Calidad del Código (20%)**
- Legibilidad y mantenibilidad
- Manejo de errores
- Testing y documentación

---

## 💡 **CONSEJOS PARA LA PRÁCTICA**

1. **Usa hooks personalizados**: Para lógica reutilizable
2. **Optimiza re-renders**: Con useMemo y useCallback
3. **Maneja errores**: Implementa boundaries de error
4. **Testea componentes**: Escribe tests para funcionalidad crítica
5. **Documenta APIs**: Especialmente para componentes reutilizables

---

*Estos ejercicios te ayudarán a dominar React moderno, creando componentes robustos, hooks personalizados y aplicaciones escalables.* 