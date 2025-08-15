# ⚡ Ejercicios Prácticos: Node.js Fundamentos

## 🎯 **OBJETIVO**
Aplicar los conceptos teóricos de Node.js mediante ejercicios prácticos que refuercen el aprendizaje y desarrollen habilidades de programación.

---

## 📋 **EJERCICIO 1: SISTEMA DE MÓDULOS**

### **Descripción**
Crear un sistema de módulos para una aplicación de gestión de biblioteca que incluya libros, usuarios y préstamos.

### **Requisitos**
- Usar CommonJS (`require`/`module.exports`)
- Crear módulos separados para cada entidad
- Implementar funciones de búsqueda y filtrado
- Manejar errores apropiadamente

### **Estructura de Archivos**
```
library-system/
├── models/
│   ├── Book.js
│   ├── User.js
│   └── Loan.js
├── services/
│   ├── BookService.js
│   ├── UserService.js
│   └── LoanService.js
├── utils/
│   ├── validators.js
│   └── helpers.js
├── index.js
└── package.json
```

### **Implementación**

#### **models/Book.js**
```javascript
class Book {
  constructor(id, title, author, isbn, genre, publishedYear) {
    this.id = id;
    this.title = title;
    this.author = author;
    this.isbn = isbn;
    this.genre = genre;
    this.publishedYear = publishedYear;
    this.isAvailable = true;
    this.createdAt = new Date();
  }

  toJSON() {
    return {
      id: this.id,
      title: this.title,
      author: this.author,
      isbn: this.isbn,
      genre: this.genre,
      publishedYear: this.publishedYear,
      isAvailable: this.isAvailable,
      createdAt: this.createdAt
    };
  }
}

module.exports = Book;
```

#### **services/BookService.js**
```javascript
const Book = require('../models/Book');

class BookService {
  constructor() {
    this.books = new Map();
    this.nextId = 1;
  }

  addBook(bookData) {
    try {
      const book = new Book(
        this.nextId++,
        bookData.title,
        bookData.author,
        bookData.isbn,
        bookData.genre,
        bookData.publishedYear
      );
      
      this.books.set(book.id, book);
      return book;
    } catch (error) {
      throw new Error(`Error adding book: ${error.message}`);
    }
  }

  getBookById(id) {
    const book = this.books.get(id);
    if (!book) {
      throw new Error('Book not found');
    }
    return book;
  }

  searchBooks(query) {
    const results = [];
    const searchTerm = query.toLowerCase();
    
    for (const book of this.books.values()) {
      if (book.title.toLowerCase().includes(searchTerm) ||
          book.author.toLowerCase().includes(searchTerm) ||
          book.genre.toLowerCase().includes(searchTerm)) {
        results.push(book);
      }
    }
    
    return results;
  }

  getBooksByGenre(genre) {
    return Array.from(this.books.values())
      .filter(book => book.genre.toLowerCase() === genre.toLowerCase());
  }

  updateBookAvailability(id, isAvailable) {
    const book = this.getBookById(id);
    book.isAvailable = isAvailable;
    return book;
  }

  getAllBooks() {
    return Array.from(this.books.values());
  }
}

module.exports = BookService;
```

### **Ejercicio para el Estudiante**
Implementa los módulos `User.js`, `UserService.js`, `Loan.js` y `LoanService.js` siguiendo el mismo patrón.

---

## 📋 **EJERCICIO 2: STREAMS Y BUFFERS**

### **Descripción**
Crear un sistema de procesamiento de archivos que use streams para leer, transformar y escribir datos de manera eficiente.

### **Requisitos**
- Implementar readable streams para lectura de archivos
- Crear transform streams para procesamiento de datos
- Usar writable streams para salida
- Manejar archivos grandes sin cargarlos completamente en memoria

### **Implementación**

#### **FileProcessor.js**
```javascript
const fs = require('fs');
const { Transform } = require('stream');

class CSVToJSONTransform extends Transform {
  constructor(options = {}) {
    super(options);
    this.isFirstChunk = true;
    this.headers = [];
    this.buffer = '';
  }

  _transform(chunk, encoding, callback) {
    this.buffer += chunk.toString();
    let lines = this.buffer.split('\n');
    
    // Mantener la última línea incompleta en el buffer
    this.buffer = lines.pop();
    
    if (this.isFirstChunk && lines.length > 0) {
      this.headers = lines[0].split(',').map(h => h.trim());
      lines = lines.slice(1);
      this.isFirstChunk = false;
    }
    
    for (const line of lines) {
      if (line.trim()) {
        const values = line.split(',').map(v => v.trim());
        const obj = {};
        
        this.headers.forEach((header, index) => {
          obj[header] = values[index] || '';
        });
        
        this.push(JSON.stringify(obj) + '\n');
      }
    }
    
    callback();
  }

  _flush(callback) {
    if (this.buffer.trim()) {
      const values = this.buffer.split(',').map(v => v.trim());
      const obj = {};
      
      this.headers.forEach((header, index) => {
        obj[header] = values[index] || '';
      });
      
      this.push(JSON.stringify(obj) + '\n');
    }
    callback();
  }
}

class FileProcessor {
  static processCSVToJSON(inputFile, outputFile) {
    const readStream = fs.createReadStream(inputFile, { encoding: 'utf8' });
    const writeStream = fs.createWriteStream(outputFile);
    const transformStream = new CSVToJSONTransform();
    
    return new Promise((resolve, reject) => {
      readStream
        .pipe(transformStream)
        .pipe(writeStream)
        .on('finish', () => resolve('Processing completed'))
        .on('error', reject);
    });
  }

  static createLargeFile(filename, lines, lineGenerator) {
    const writeStream = fs.createWriteStream(filename);
    
    return new Promise((resolve, reject) => {
      let lineCount = 0;
      
      const writeLine = () => {
        let canContinue = true;
        
        while (canContinue && lineCount < lines) {
          const line = lineGenerator(lineCount);
          canContinue = writeStream.write(line + '\n');
          lineCount++;
        }
        
        if (lineCount < lines) {
          writeStream.once('drain', writeLine);
        } else {
          writeStream.end();
        }
      };
      
      writeStream.on('finish', resolve);
      writeStream.on('error', reject);
      
      writeLine();
    });
  }
}

module.exports = FileProcessor;
```

### **Ejercicio para el Estudiante**
1. Crea un archivo CSV de ejemplo con datos de usuarios
2. Implementa un transform stream que convierta nombres a mayúsculas
3. Añade validación de datos en el transform stream
4. Implementa un contador de líneas procesadas

---

## 📋 **EJERCICIO 3: EVENT EMITTER**

### **Descripción**
Implementar un sistema de notificaciones usando EventEmitter para diferentes tipos de eventos en una aplicación.

### **Requisitos**
- Crear un sistema de eventos personalizado
- Implementar diferentes tipos de notificaciones
- Manejar suscripciones y desuscripciones
- Implementar filtros de eventos

### **Implementación**

#### **NotificationSystem.js**
```javascript
const EventEmitter = require('events');

class NotificationSystem extends EventEmitter {
  constructor() {
    super();
    this.subscribers = new Map();
    this.eventHistory = [];
  }

  subscribe(userId, eventTypes, callback) {
    if (!this.subscribers.has(userId)) {
      this.subscribers.set(userId, new Set());
    }
    
    const subscription = { eventTypes, callback };
    this.subscribers.get(userId).add(subscription);
    
    // Emitir evento de suscripción
    this.emit('userSubscribed', { userId, eventTypes });
    
    return subscription;
  }

  unsubscribe(userId, subscription) {
    const userSubscriptions = this.subscribers.get(userId);
    if (userSubscriptions) {
      userSubscriptions.delete(subscription);
      
      if (userSubscriptions.size === 0) {
        this.subscribers.delete(userId);
      }
      
      this.emit('userUnsubscribed', { userId });
    }
  }

  notify(eventType, data) {
    const event = {
      type: eventType,
      data,
      timestamp: new Date(),
      id: this.generateEventId()
    };
    
    // Guardar en historial
    this.eventHistory.push(event);
    
    // Emitir evento global
    this.emit(eventType, event);
    
    // Notificar a suscriptores específicos
    this.notifySubscribers(event);
    
    return event;
  }

  notifySubscribers(event) {
    for (const [userId, subscriptions] of this.subscribers) {
      for (const subscription of subscriptions) {
        if (subscription.eventTypes.includes(event.type)) {
          try {
            subscription.callback(event, userId);
          } catch (error) {
            this.emit('notificationError', { error, userId, event });
          }
        }
      }
    }
  }

  getEventHistory(eventType = null, limit = 100) {
    let events = this.eventHistory;
    
    if (eventType) {
      events = events.filter(event => event.type === eventType);
    }
    
    return events.slice(-limit);
  }

  generateEventId() {
    return Date.now().toString(36) + Math.random().toString(36).substr(2);
  }

  getStats() {
    const stats = {
      totalSubscribers: this.subscribers.size,
      totalEvents: this.eventHistory.length,
      eventTypes: new Set(this.eventHistory.map(e => e.type)).size,
      activeSubscriptions: 0
    };
    
    for (const subscriptions of this.subscribers.values()) {
      stats.activeSubscriptions += subscriptions.size;
    }
    
    return stats;
  }
}

// Tipos de eventos predefinidos
const EventTypes = {
  USER_REGISTERED: 'user.registered',
  USER_LOGGED_IN: 'user.logged_in',
  POST_CREATED: 'post.created',
  POST_UPDATED: 'post.updated',
  COMMENT_ADDED: 'comment.added',
  SYSTEM_MAINTENANCE: 'system.maintenance'
};

module.exports = { NotificationSystem, EventTypes };
```

### **Ejercicio para el Estudiante**
1. Implementa un sistema de prioridades para notificaciones
2. Añade persistencia de eventos en archivo
3. Implementa rate limiting para notificaciones
4. Crea un sistema de templates para mensajes

---

## 📋 **EJERCICIO 4: ASINCRONÍA Y PROMESAS**

### **Descripción**
Crear un sistema de procesamiento de tareas asíncronas que maneje múltiples operaciones concurrentes.

### **Requisitos**
- Implementar operaciones asíncronas con async/await
- Manejar múltiples promesas concurrentes
- Implementar retry logic para operaciones fallidas
- Crear un sistema de cola de tareas

### **Implementación**

#### **TaskProcessor.js**
```javascript
class TaskProcessor {
  constructor(maxConcurrent = 3) {
    this.maxConcurrent = maxConcurrent;
    this.queue = [];
    this.running = 0;
    this.completed = 0;
    this.failed = 0;
  }

  async addTask(task, priority = 0) {
    const taskWrapper = {
      id: this.generateTaskId(),
      task,
      priority,
      status: 'pending',
      createdAt: new Date(),
      attempts: 0,
      maxAttempts: 3
    };
    
    this.queue.push(taskWrapper);
    this.queue.sort((a, b) => b.priority - a.priority);
    
    this.processQueue();
    
    return taskWrapper.id;
  }

  async processQueue() {
    if (this.running >= this.maxConcurrent || this.queue.length === 0) {
      return;
    }
    
    const taskWrapper = this.queue.shift();
    this.running++;
    
    try {
      taskWrapper.status = 'running';
      taskWrapper.startedAt = new Date();
      
      const result = await this.executeTask(taskWrapper);
      
      taskWrapper.status = 'completed';
      taskWrapper.completedAt = new Date();
      taskWrapper.result = result;
      
      this.completed++;
      
    } catch (error) {
      taskWrapper.status = 'failed';
      taskWrapper.error = error.message;
      taskWrapper.attempts++;
      
      if (taskWrapper.attempts < taskWrapper.maxAttempts) {
        // Reintentar con delay exponencial
        const delay = Math.pow(2, taskWrapper.attempts) * 1000;
        setTimeout(() => {
          this.queue.unshift(taskWrapper);
          this.processQueue();
        }, delay);
      } else {
        this.failed++;
      }
    } finally {
      this.running--;
      this.processQueue(); // Procesar siguiente tarea
    }
  }

  async executeTask(taskWrapper) {
    // Simular trabajo asíncrono
    await new Promise(resolve => setTimeout(resolve, Math.random() * 2000));
    
    // Simular fallos aleatorios
    if (Math.random() < 0.2) {
      throw new Error('Random task failure');
    }
    
    return `Task ${taskWrapper.id} completed successfully`;
  }

  generateTaskId() {
    return Date.now().toString(36) + Math.random().toString(36).substr(2);
  }

  getStats() {
    return {
      queueLength: this.queue.length,
      running: this.running,
      completed: this.completed,
      failed: this.failed,
      total: this.completed + this.failed
    };
  }

  clearCompleted() {
    this.completed = 0;
    this.failed = 0;
  }
}

module.exports = TaskProcessor;
```

### **Ejercicio para el Estudiante**
1. Implementa un sistema de prioridades más sofisticado
2. Añade logging detallado de tareas
3. Implementa cancelación de tareas
4. Crea un sistema de métricas y monitoreo

---

## 📋 **EJERCICIO 5: TESTING Y DEBUGGING**

### **Descripción**
Implementar tests unitarios y de integración para los módulos creados, incluyendo debugging y manejo de errores.

### **Requisitos**
- Usar Jest para testing
- Implementar mocks y stubs
- Crear tests de edge cases
- Implementar debugging efectivo

### **Implementación**

#### **tests/BookService.test.js**
```javascript
const BookService = require('../services/BookService');

describe('BookService', () => {
  let bookService;
  
  beforeEach(() => {
    bookService = new BookService();
  });
  
  describe('addBook', () => {
    it('should add a book successfully', () => {
      const bookData = {
        title: 'Test Book',
        author: 'Test Author',
        isbn: '1234567890',
        genre: 'Fiction',
        publishedYear: 2023
      };
      
      const book = bookService.addBook(bookData);
      
      expect(book).toBeDefined();
      expect(book.id).toBe(1);
      expect(book.title).toBe(bookData.title);
      expect(book.isAvailable).toBe(true);
    });
    
    it('should generate unique IDs for multiple books', () => {
      const book1 = bookService.addBook({ title: 'Book 1', author: 'Author 1' });
      const book2 = bookService.addBook({ title: 'Book 2', author: 'Author 2' });
      
      expect(book1.id).toBe(1);
      expect(book2.id).toBe(2);
    });
  });
  
  describe('getBookById', () => {
    it('should return book when found', () => {
      const book = bookService.addBook({ title: 'Test', author: 'Author' });
      const foundBook = bookService.getBookById(book.id);
      
      expect(foundBook).toEqual(book);
    });
    
    it('should throw error when book not found', () => {
      expect(() => {
        bookService.getBookById(999);
      }).toThrow('Book not found');
    });
  });
  
  describe('searchBooks', () => {
    beforeEach(() => {
      bookService.addBook({ title: 'JavaScript Guide', author: 'John Doe', genre: 'Programming' });
      bookService.addBook({ title: 'Python Basics', author: 'Jane Smith', genre: 'Programming' });
      bookService.addBook({ title: 'Fiction Novel', author: 'Author Name', genre: 'Fiction' });
    });
    
    it('should find books by title', () => {
      const results = bookService.searchBooks('JavaScript');
      expect(results).toHaveLength(1);
      expect(results[0].title).toBe('JavaScript Guide');
    });
    
    it('should find books by author', () => {
      const results = bookService.searchBooks('John');
      expect(results).toHaveLength(1);
      expect(results[0].author).toBe('John Doe');
    });
    
    it('should find books by genre', () => {
      const results = bookService.searchBooks('Programming');
      expect(results).toHaveLength(2);
    });
    
    it('should return empty array for no matches', () => {
      const results = bookService.searchBooks('Nonexistent');
      expect(results).toHaveLength(0);
    });
  });
});
```

### **Ejercicio para el Estudiante**
1. Implementa tests para UserService y LoanService
2. Añade tests de integración
3. Implementa tests de performance
4. Crea tests para edge cases y manejo de errores

---

## 🎯 **EJERCICIOS ADICIONALES**

### **Ejercicio 6: Sistema de Logging**
Implementa un sistema de logging que use streams para escribir logs en archivos y consola.

### **Ejercicio 7: Clustering**
Crea una aplicación que use el módulo cluster para aprovechar múltiples núcleos de CPU.

### **Ejercicio 8: Middleware Pattern**
Implementa un sistema de middleware similar a Express.js usando el patrón de chain of responsibility.

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Funcionalidad (40%)**
- ✅ Código ejecutable sin errores
- ✅ Implementación completa de requisitos
- ✅ Manejo correcto de casos edge

### **Calidad del Código (30%)**
- ✅ Estructura y organización
- ✅ Nombres descriptivos
- ✅ Comentarios apropiados

### **Testing (20%)**
- ✅ Cobertura de tests
- ✅ Tests significativos
- ✅ Mocks y stubs apropiados

### **Documentación (10%)**
- ✅ README claro
- ✅ Instrucciones de uso
- ✅ Ejemplos de implementación

---

## 🚀 **PRÓXIMOS PASOS**

Una vez completados estos ejercicios, estarás listo para:
1. **Conceptos Avanzados**: Profundizar en patrones de diseño
2. **Integración**: Conectar con bases de datos y APIs
3. **Performance**: Optimización y profiling
4. **Deployment**: Containerización y CI/CD

---

*Estos ejercicios te proporcionarán una base sólida en Node.js y te prepararán para los desafíos más avanzados del desarrollo backend.* 