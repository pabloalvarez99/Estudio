# 01. Proyecto: Sistema Bancario con Paradigmas de Programación

## 🎯 Descripción del Proyecto

Implementar un sistema bancario completo que demuestre el uso de diferentes paradigmas de programación: **POO**, **Programación Funcional**, **Patrones de Diseño** y **Principios SOLID**.

## 🏗️ Arquitectura del Sistema

### **Estructura de Clases**
```
SistemaBancario/
├── Core/
│   ├── Entidades (POO)
│   ├── Servicios (POO + Funcional)
│   └── Repositorios (Patrones)
├── Infraestructura/
│   ├── Base de Datos (Patrones)
│   └── Logging (Funcional)
└── API/
    └── Controladores (POO)
```

## 🔧 Implementación

### **1. Entidades del Dominio (POO)**

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass
from datetime import datetime
from typing import List, Dict, Optional
from decimal import Decimal
import uuid

@dataclass
class Cliente:
    id: str
    nombre: str
    email: str
    telefono: str
    fecha_registro: datetime
    activo: bool = True
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha_registro:
            self.fecha_registro = datetime.now()
    
    def obtener_info(self) -> str:
        return f"Cliente {self.nombre} ({self.email}) - {'Activo' if self.activo else 'Inactivo'}"

@dataclass
class Cuenta:
    id: str
    numero: str
    cliente_id: str
    tipo: str  # 'ahorro', 'corriente', 'inversion'
    saldo: Decimal
    fecha_apertura: datetime
    activa: bool = True
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha_apertura:
            self.fecha_apertura = datetime.now()
    
    def depositar(self, monto: Decimal) -> bool:
        if monto > 0 and self.activa:
            self.saldo += monto
            return True
        return False
    
    def retirar(self, monto: Decimal) -> bool:
        if 0 < monto <= self.saldo and self.activa:
            self.saldo -= monto
            return True
        return False
    
    def transferir(self, cuenta_destino: 'Cuenta', monto: Decimal) -> bool:
        if self.retirar(monto):
            if cuenta_destino.depositar(monto):
                return True
            else:
                # Rollback si falla el depósito
                self.depositar(monto)
        return False
    
    def obtener_info(self) -> str:
        return f"Cuenta {self.numero} - {self.tipo} - Saldo: ${self.saldo:.2f}"

@dataclass
class Transaccion:
    id: str
    cuenta_origen_id: str
    cuenta_destino_id: Optional[str]
    tipo: str  # 'deposito', 'retiro', 'transferencia'
    monto: Decimal
    fecha: datetime
    descripcion: str
    estado: str = 'completada'  # 'pendiente', 'completada', 'fallida'
    
    def __post_init__(self):
        if not self.id:
            self.id = str(uuid.uuid4())
        if not self.fecha:
            self.fecha = datetime.now()
    
    def obtener_info(self) -> str:
        if self.cuenta_destino_id:
            return f"{self.tipo.title()} ${self.monto:.2f} de {self.cuenta_origen_id} a {self.cuenta_destino_id}"
        else:
            return f"{self.tipo.title()} ${self.monto:.2f} en {self.cuenta_origen_id}"
```

### **2. Interfaces y Abstracciones (Principios SOLID)**

```python
from abc import ABC, abstractmethod
from typing import List, Optional, Protocol

class RepositorioCliente(ABC):
    @abstractmethod
    def guardar(self, cliente: Cliente) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_id(self, id: str) -> Optional[Cliente]:
        pass
    
    @abstractmethod
    def obtener_todos(self) -> List[Cliente]:
        pass
    
    @abstractmethod
    def actualizar(self, cliente: Cliente) -> bool:
        pass
    
    @abstractmethod
    def eliminar(self, id: str) -> bool:
        pass

class RepositorioCuenta(ABC):
    @abstractmethod
    def guardar(self, cuenta: Cuenta) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_id(self, id: str) -> Optional[Cuenta]:
        pass
    
    @abstractmethod
    def obtener_por_cliente(self, cliente_id: str) -> List[Cuenta]:
        pass
    
    @abstractmethod
    def actualizar(self, cuenta: Cuenta) -> bool:
        pass

class RepositorioTransaccion(ABC):
    @abstractmethod
    def guardar(self, transaccion: Transaccion) -> bool:
        pass
    
    @abstractmethod
    def obtener_por_cuenta(self, cuenta_id: str) -> List[Transaccion]:
        pass
    
    @abstractmethod
    def obtener_por_fecha(self, fecha_inicio: datetime, fecha_fin: datetime) -> List[Transaccion]:
        pass

class ServicioNotificacion(ABC):
    @abstractmethod
    def enviar_notificacion(self, cliente_id: str, mensaje: str) -> bool:
        pass

class ServicioValidacion(ABC):
    @abstractmethod
    def validar_cliente(self, cliente: Cliente) -> bool:
        pass
    
    @abstractmethod
    def validar_transaccion(self, transaccion: Transaccion) -> bool:
        pass
```

### **3. Implementaciones de Repositorios (Patrones)**

```python
class RepositorioClienteMemoria(RepositorioCliente):
    def __init__(self):
        self.clientes: Dict[str, Cliente] = {}
    
    def guardar(self, cliente: Cliente) -> bool:
        try:
            self.clientes[cliente.id] = cliente
            return True
        except Exception:
            return False
    
    def obtener_por_id(self, id: str) -> Optional[Cliente]:
        return self.clientes.get(id)
    
    def obtener_todos(self) -> List[Cliente]:
        return list(self.clientes.values())
    
    def actualizar(self, cliente: Cliente) -> bool:
        if cliente.id in self.clientes:
            self.clientes[cliente.id] = cliente
            return True
        return False
    
    def eliminar(self, id: str) -> bool:
        if id in self.clientes:
            del self.clientes[id]
            return True
        return False

class RepositorioCuentaMemoria(RepositorioCuenta):
    def __init__(self):
        self.cuentas: Dict[str, Cuenta] = {}
    
    def guardar(self, cuenta: Cuenta) -> bool:
        try:
            self.cuentas[cuenta.id] = cuenta
            return True
        except Exception:
            return False
    
    def obtener_por_id(self, id: str) -> Optional[Cuenta]:
        return self.cuentas.get(id)
    
    def obtener_por_cliente(self, cliente_id: str) -> List[Cuenta]:
        return [cuenta for cuenta in self.cuentas.values() if cuenta.cliente_id == cliente_id]
    
    def actualizar(self, cuenta: Cuenta) -> bool:
        if cuenta.id in self.cuentas:
            self.cuentas[cuenta.id] = cuenta
            return True
        return False

class RepositorioTransaccionMemoria(RepositorioTransaccion):
    def __init__(self):
        self.transacciones: Dict[str, Transaccion] = {}
    
    def guardar(self, transaccion: Transaccion) -> bool:
        try:
            self.transacciones[transaccion.id] = transaccion
            return True
        except Exception:
            return False
    
    def obtener_por_cuenta(self, cuenta_id: str) -> List[Transaccion]:
        return [
            t for t in self.transacciones.values() 
            if t.cuenta_origen_id == cuenta_id or t.cuenta_destino_id == cuenta_id
        ]
    
    def obtener_por_fecha(self, fecha_inicio: datetime, fecha_fin: datetime) -> List[Transaccion]:
        return [
            t for t in self.transacciones.values() 
            if fecha_inicio <= t.fecha <= fecha_fin
        ]
```

### **4. Servicios de Negocio (POO + Funcional)**

```python
from typing import Callable, List
from functools import reduce
import logging

class ServicioCliente:
    def __init__(self, repositorio: RepositorioCliente, validador: ServicioValidacion):
        self.repositorio = repositorio
        self.validador = validador
        self.logger = logging.getLogger(__name__)
    
    def crear_cliente(self, nombre: str, email: str, telefono: str) -> Optional[Cliente]:
        try:
            cliente = Cliente(
                id="",
                nombre=nombre,
                email=email,
                telefono=telefono,
                fecha_registro=datetime.now()
            )
            
            if self.validador.validar_cliente(cliente):
                if self.repositorio.guardar(cliente):
                    self.logger.info(f"Cliente creado: {cliente.id}")
                    return cliente
                else:
                    self.logger.error("Error al guardar cliente")
            else:
                self.logger.warning("Cliente no válido")
                
        except Exception as e:
            self.logger.error(f"Error al crear cliente: {e}")
        
        return None
    
    def obtener_cliente(self, id: str) -> Optional[Cliente]:
        return self.repositorio.obtener_por_id(id)
    
    def listar_clientes(self) -> List[Cliente]:
        return self.repositorio.obtener_todos()
    
    def actualizar_cliente(self, cliente: Cliente) -> bool:
        if self.validador.validar_cliente(cliente):
            return self.repositorio.actualizar(cliente)
        return False

class ServicioCuenta:
    def __init__(self, repositorio_cuenta: RepositorioCuenta, 
                 repositorio_cliente: RepositorioCliente):
        self.repositorio_cuenta = repositorio_cuenta
        self.repositorio_cliente = repositorio_cliente
        self.logger = logging.getLogger(__name__)
    
    def crear_cuenta(self, cliente_id: str, tipo: str, saldo_inicial: Decimal = Decimal('0')) -> Optional[Cuenta]:
        # Verificar que el cliente existe
        cliente = self.repositorio_cliente.obtener_por_id(cliente_id)
        if not cliente:
            self.logger.error(f"Cliente no encontrado: {cliente_id}")
            return None
        
        # Generar número de cuenta único
        numero_cuenta = self._generar_numero_cuenta()
        
        cuenta = Cuenta(
            id="",
            numero=numero_cuenta,
            cliente_id=cliente_id,
            tipo=tipo,
            saldo=saldo_inicial,
            fecha_apertura=datetime.now()
        )
        
        if self.repositorio_cuenta.guardar(cuenta):
            self.logger.info(f"Cuenta creada: {cuenta.numero}")
            return cuenta
        else:
            self.logger.error("Error al crear cuenta")
            return None
    
    def _generar_numero_cuenta(self) -> str:
        import random
        return f"CUENTA_{random.randint(100000, 999999)}"
    
    def obtener_cuenta(self, id: str) -> Optional[Cuenta]:
        return self.repositorio_cuenta.obtener_por_id(id)
    
    def obtener_cuentas_cliente(self, cliente_id: str) -> List[Cuenta]:
        return self.repositorio_cuenta.obtener_por_cliente(cliente_id)

class ServicioTransaccion:
    def __init__(self, repositorio_transaccion: RepositorioTransaccion,
                 repositorio_cuenta: RepositorioCuenta,
                 validador: ServicioValidacion,
                 notificador: ServicioNotificacion):
        self.repositorio_transaccion = repositorio_transaccion
        self.repositorio_cuenta = repositorio_cuenta
        self.validador = validador
        self.notificador = notificador
        self.logger = logging.getLogger(__name__)
    
    def realizar_deposito(self, cuenta_id: str, monto: Decimal, descripcion: str = "") -> bool:
        cuenta = self.repositorio_cuenta.obtener_por_id(cuenta_id)
        if not cuenta:
            self.logger.error(f"Cuenta no encontrada: {cuenta_id}")
            return False
        
        if cuenta.depositar(monto):
            transaccion = Transaccion(
                id="",
                cuenta_origen_id=cuenta_id,
                cuenta_destino_id=None,
                tipo="deposito",
                monto=monto,
                fecha=datetime.now(),
                descripcion=descripcion
            )
            
            if self.repositorio_transaccion.guardar(transaccion):
                self.repositorio_cuenta.actualizar(cuenta)
                self._notificar_transaccion(cuenta, transaccion)
                self.logger.info(f"Depósito realizado: {transaccion.id}")
                return True
        
        return False
    
    def realizar_retiro(self, cuenta_id: str, monto: Decimal, descripcion: str = "") -> bool:
        cuenta = self.repositorio_cuenta.obtener_por_id(cuenta_id)
        if not cuenta:
            self.logger.error(f"Cuenta no encontrada: {cuenta_id}")
            return False
        
        if cuenta.retirar(monto):
            transaccion = Transaccion(
                id="",
                cuenta_origen_id=cuenta_id,
                cuenta_destino_id=None,
                tipo="retiro",
                monto=monto,
                fecha=datetime.now(),
                descripcion=descripcion
            )
            
            if self.repositorio_transaccion.guardar(transaccion):
                self.repositorio_cuenta.actualizar(cuenta)
                self._notificar_transaccion(cuenta, transaccion)
                self.logger.info(f"Retiro realizado: {transaccion.id}")
                return True
        
        return False
    
    def realizar_transferencia(self, cuenta_origen_id: str, cuenta_destino_id: str, 
                             monto: Decimal, descripcion: str = "") -> bool:
        cuenta_origen = self.repositorio_cuenta.obtener_por_id(cuenta_origen_id)
        cuenta_destino = self.repositorio_cuenta.obtener_por_id(cuenta_destino_id)
        
        if not cuenta_origen or not cuenta_destino:
            self.logger.error("Una o ambas cuentas no encontradas")
            return False
        
        if cuenta_origen.transferir(cuenta_destino, monto):
            transaccion = Transaccion(
                id="",
                cuenta_origen_id=cuenta_origen_id,
                cuenta_destino_id=cuenta_destino_id,
                tipo="transferencia",
                monto=monto,
                fecha=datetime.now(),
                descripcion=descripcion
            )
            
            if self.repositorio_transaccion.guardar(transaccion):
                self.repositorio_cuenta.actualizar(cuenta_origen)
                self.repositorio_cuenta.actualizar(cuenta_destino)
                self._notificar_transaccion(cuenta_origen, transaccion)
                self._notificar_transaccion(cuenta_destino, transaccion)
                self.logger.info(f"Transferencia realizada: {transaccion.id}")
                return True
        
        return False
    
    def _notificar_transaccion(self, cuenta: Cuenta, transaccion: Transaccion):
        # Obtener cliente para notificación
        # En una implementación real, esto vendría de un servicio de cliente
        mensaje = f"Transacción {transaccion.tipo} de ${transaccion.monto:.2f} en cuenta {cuenta.numero}"
        self.notificador.enviar_notificacion(cuenta.cliente_id, mensaje)
    
    def obtener_historial_cuenta(self, cuenta_id: str) -> List[Transaccion]:
        return self.repositorio_transaccion.obtener_por_cuenta(cuenta_id)
    
    def obtener_transacciones_por_fecha(self, fecha_inicio: datetime, fecha_fin: datetime) -> List[Transaccion]:
        return self.repositorio_transaccion.obtener_por_fecha(fecha_inicio, fecha_fin)
```

### **5. Implementaciones de Servicios (Funcional)**

```python
class ValidadorCliente(ServicioValidacion):
    def validar_cliente(self, cliente: Cliente) -> bool:
        # Validaciones usando programación funcional
        validaciones = [
            lambda c: len(c.nombre.strip()) >= 2,
            lambda c: '@' in c.email and '.' in c.email,
            lambda c: len(c.telefono) >= 10,
            lambda c: c.fecha_registro <= datetime.now()
        ]
        
        return all(validacion(cliente) for validacion in validaciones)
    
    def validar_transaccion(self, transaccion: Transaccion) -> bool:
        validaciones = [
            lambda t: t.monto > 0,
            lambda t: t.fecha <= datetime.now(),
            lambda t: t.tipo in ['deposito', 'retiro', 'transferencia'],
            lambda t: t.estado in ['pendiente', 'completada', 'fallida']
        ]
        
        return all(validacion(transaccion) for validacion in validaciones)

class NotificadorConsola(ServicioNotificacion):
    def enviar_notificacion(self, cliente_id: str, mensaje: str) -> bool:
        print(f"📧 Notificación para cliente {cliente_id}: {mensaje}")
        return True

class NotificadorEmail(ServicioNotificacion):
    def __init__(self, servidor_smtp: str):
        self.servidor_smtp = servidor_smtp
    
    def enviar_notificacion(self, cliente_id: str, mensaje: str) -> bool:
        # Simulación de envío de email
        print(f"📧 Email enviado a cliente {cliente_id} via {self.servidor_smtp}: {mensaje}")
        return True
```

### **6. Sistema Principal (Orquestador)**

```python
class SistemaBancario:
    def __init__(self):
        # Inicializar repositorios
        self.repositorio_cliente = RepositorioClienteMemoria()
        self.repositorio_cuenta = RepositorioCuentaMemoria()
        self.repositorio_transaccion = RepositorioTransaccionMemoria()
        
        # Inicializar servicios
        self.validador = ValidadorCliente()
        self.notificador = NotificadorConsola()
        
        self.servicio_cliente = ServicioCliente(
            self.repositorio_cliente, 
            self.validador
        )
        
        self.servicio_cuenta = ServicioCuenta(
            self.repositorio_cuenta,
            self.repositorio_cliente
        )
        
        self.servicio_transaccion = ServicioTransaccion(
            self.repositorio_transaccion,
            self.repositorio_cuenta,
            self.validador,
            self.notificador
        )
        
        self.logger = logging.getLogger(__name__)
    
    def crear_cliente(self, nombre: str, email: str, telefono: str) -> Optional[Cliente]:
        return self.servicio_cliente.crear_cliente(nombre, email, telefono)
    
    def crear_cuenta(self, cliente_id: str, tipo: str, saldo_inicial: Decimal = Decimal('0')) -> Optional[Cuenta]:
        return self.servicio_cuenta.crear_cuenta(cliente_id, tipo, saldo_inicial)
    
    def realizar_deposito(self, cuenta_id: str, monto: Decimal, descripcion: str = "") -> bool:
        return self.servicio_transaccion.realizar_deposito(cuenta_id, monto, descripcion)
    
    def realizar_retiro(self, cuenta_id: str, monto: Decimal, descripcion: str = "") -> bool:
        return self.servicio_transaccion.realizar_retiro(cuenta_id, monto, descripcion)
    
    def realizar_transferencia(self, cuenta_origen_id: str, cuenta_destino_id: str, 
                             monto: Decimal, descripcion: str = "") -> bool:
        return self.servicio_transaccion.realizar_transferencia(
            cuenta_origen_id, cuenta_destino_id, monto, descripcion
        )
    
    def obtener_estado_cuenta(self, cuenta_id: str) -> Optional[Cuenta]:
        return self.servicio_cuenta.obtener_cuenta(cuenta_id)
    
    def obtener_historial_cuenta(self, cuenta_id: str) -> List[Transaccion]:
        return self.servicio_transaccion.obtener_historial_cuenta(cuenta_id)
    
    def generar_reporte_clientes(self) -> str:
        clientes = self.servicio_cliente.listar_clientes()
        reporte = "=== REPORTE DE CLIENTES ===\n\n"
        
        for cliente in clientes:
            reporte += f"{cliente.obtener_info()}\n"
            cuentas = self.servicio_cuenta.obtener_cuentas_cliente(cliente.id)
            reporte += f"  Cuentas: {len(cuentas)}\n"
            for cuenta in cuentas:
                reporte += f"    - {cuenta.obtener_info()}\n"
            reporte += "\n"
        
        return reporte
```

### **7. Ejemplo de Uso**

```python
def main():
    # Configurar logging
    logging.basicConfig(level=logging.INFO)
    
    # Crear sistema bancario
    banco = SistemaBancario()
    
    print("🏦 SISTEMA BANCARIO INICIADO\n")
    
    # Crear clientes
    cliente1 = banco.crear_cliente("Ana García", "ana@email.com", "1234567890")
    cliente2 = banco.crear_cliente("Bob Smith", "bob@email.com", "0987654321")
    
    if cliente1 and cliente2:
        print(f"✅ Cliente creado: {cliente1.nombre}")
        print(f"✅ Cliente creado: {cliente2.nombre}\n")
        
        # Crear cuentas
        cuenta1 = banco.crear_cuenta(cliente1.id, "ahorro", Decimal('1000'))
        cuenta2 = banco.crear_cuenta(cliente2.id, "corriente", Decimal('500'))
        
        if cuenta1 and cuenta2:
            print(f"✅ Cuenta creada: {cuenta1.numero}")
            print(f"✅ Cuenta creada: {cuenta2.numero}\n")
            
            # Realizar operaciones
            print("💰 REALIZANDO OPERACIONES:\n")
            
            # Depósito
            if banco.realizar_deposito(cuenta1.id, Decimal('200'), "Depósito inicial"):
                print("✅ Depósito realizado")
            
            # Retiro
            if banco.realizar_retiro(cuenta1.id, Decimal('100'), "Retiro para gastos"):
                print("✅ Retiro realizado")
            
            # Transferencia
            if banco.realizar_transferencia(cuenta1.id, cuenta2.id, Decimal('150'), "Transferencia"):
                print("✅ Transferencia realizada")
            
            print("\n" + "="*50)
            
            # Mostrar estados
            print("\n📊 ESTADO DE CUENTAS:")
            estado1 = banco.obtener_estado_cuenta(cuenta1.id)
            estado2 = banco.obtener_estado_cuenta(cuenta2.id)
            
            if estado1:
                print(f"  {estado1.obtener_info()}")
            if estado2:
                print(f"  {estado2.obtener_info()}")
            
            print("\n" + "="*50)
            
            # Historial de transacciones
            print("\n📋 HISTORIAL DE TRANSACCIONES:")
            historial1 = banco.obtener_historial_cuenta(cuenta1.id)
            for trans in historial1:
                print(f"  {trans.obtener_info()}")
            
            print("\n" + "="*50)
            
            # Reporte completo
            print("\n📈 REPORTE COMPLETO:")
            print(banco.generar_reporte_clientes())

if __name__ == "__main__":
    main()
```

## 🎯 Características del Proyecto

### **Paradigmas Implementados**
- **POO**: Clases, herencia, encapsulamiento, polimorfismo
- **Funcional**: Funciones puras, map/filter/reduce, composición
- **Patrones**: Repository, Strategy, Observer, Factory
- **SOLID**: Separación de responsabilidades, inversión de dependencias

### **Funcionalidades**
- ✅ Gestión de clientes
- ✅ Gestión de cuentas
- ✅ Transacciones (depósitos, retiros, transferencias)
- ✅ Validaciones
- ✅ Notificaciones
- ✅ Logging
- ✅ Reportes

### **Extensibilidad**
- Fácil agregar nuevos tipos de cuentas
- Nuevos métodos de notificación
- Diferentes implementaciones de repositorios
- Nuevas validaciones

## 🚀 Mejoras Futuras

1. **Persistencia Real**: Base de datos SQL/NoSQL
2. **API REST**: Endpoints HTTP
3. **Autenticación**: JWT, OAuth
4. **Transacciones Atómicas**: Rollback automático
5. **Auditoría**: Logging detallado de cambios
6. **Testing**: Unit tests, integration tests
7. **Documentación**: API docs, Swagger

---

**Siguiente**: [02. Proyecto Gestor de Archivos](./02-proyecto-gestor-archivos.md) 