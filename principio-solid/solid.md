# 🏗️ Principios SOLID en Ingeniería de Software

> **Guía profesional para el diseño de software robusto, mantenible y escalable**

---

## 📌 ¿Qué son los principios SOLID?

Los principios **SOLID** son un conjunto de cinco reglas fundamentales en el diseño orientado a objetos, formuladas por **Robert C. Martin (Uncle Bob)** a principios de los 2000. Su objetivo es guiar a los desarrolladores hacia un código más limpio, flexible y fácil de mantener a lo largo del tiempo.

| Letra | Principio | Concepto clave |
|-------|-----------|----------------|
| **S** | Single Responsibility | Una sola responsabilidad por clase |
| **O** | Open/Closed | Abierto para extensión, cerrado para modificación |
| **L** | Liskov Substitution | Las subclases deben poder reemplazar a sus clases base |
| **I** | Interface Segregation | Interfaces específicas, no generales |
| **D** | Dependency Inversion | Depender de abstracciones, no de implementaciones concretas |

---

## 1️⃣ S — Single Responsibility Principle (SRP)

> *"Una clase debe tener una, y solo una, razón para cambiar."*

### Concepto
Cada clase, módulo o función debe encargarse de **una única responsabilidad** dentro del sistema. Si una clase hace demasiadas cosas, cualquier cambio en una de ellas puede afectar las demás.

### ❌ Mal diseño

```python
class UserManager:
    def create_user(self, data): ...
    def send_welcome_email(self, user): ...   # ← No es su responsabilidad
    def save_to_database(self, user): ...     # ← No es su responsabilidad
```

### ✅ Buen diseño

```python
class UserService:
    def create_user(self, data): ...

class EmailService:
    def send_welcome_email(self, user): ...

class UserRepository:
    def save(self, user): ...
```

### 💡 Beneficio
Facilita el mantenimiento, las pruebas unitarias y la reutilización del código.

---

## 2️⃣ O — Open/Closed Principle (OCP)

> *"Las entidades de software deben estar abiertas para su extensión, pero cerradas para su modificación."*

### Concepto
Puedes **agregar nuevo comportamiento** sin modificar el código ya existente. Esto se logra a través de la herencia, interfaces o composición.

### ❌ Mal diseño

```python
class DiscountCalculator:
    def calculate(self, user_type, price):
        if user_type == "premium":
            return price * 0.8
        elif user_type == "vip":
            return price * 0.7
        # Cada nuevo tipo obliga a modificar esta clase
```

### ✅ Buen diseño

```python
from abc import ABC, abstractmethod

class DiscountStrategy(ABC):
    @abstractmethod
    def apply(self, price: float) -> float: ...

class PremiumDiscount(DiscountStrategy):
    def apply(self, price): return price * 0.8

class VIPDiscount(DiscountStrategy):
    def apply(self, price): return price * 0.7

class DiscountCalculator:
    def __init__(self, strategy: DiscountStrategy):
        self.strategy = strategy

    def calculate(self, price):
        return self.strategy.apply(price)
```

### 💡 Beneficio
Permite escalar el sistema con nuevas funcionalidades sin riesgo de romper el código existente.

---

## 3️⃣ L — Liskov Substitution Principle (LSP)

> *"Si S es un subtipo de T, entonces los objetos de tipo T pueden reemplazarse por objetos de tipo S sin alterar el comportamiento correcto del programa."*

### Concepto
Las **subclases deben comportarse de manera coherente con su clase base**. Si una subclase cambia el comportamiento esperado, viola este principio.

### ❌ Mal diseño

```python
class Bird:
    def fly(self): ...

class Penguin(Bird):
    def fly(self):
        raise Exception("Los pingüinos no vuelan")  # ← Viola LSP
```

### ✅ Buen diseño

```python
class Bird:
    def eat(self): ...

class FlyingBird(Bird):
    def fly(self): ...

class Penguin(Bird):     # Solo hereda lo que puede hacer
    def swim(self): ...

class Eagle(FlyingBird): # Hereda la capacidad de volar
    pass
```

### 💡 Beneficio
Garantiza que el polimorfismo funcione de forma predecible y segura en todo el sistema.

---

## 4️⃣ I — Interface Segregation Principle (ISP)

> *"Los clientes no deben verse obligados a depender de interfaces que no utilizan."*

### Concepto
Es mejor tener **muchas interfaces pequeñas y específicas** que una sola interfaz grande y general. Así, cada clase solo implementa lo que realmente necesita.

### ❌ Mal diseño

```python
class Worker(ABC):
    @abstractmethod
    def work(self): ...
    @abstractmethod
    def eat(self): ...    # Un robot no come
    @abstractmethod
    def sleep(self): ...  # Un robot no duerme
```

### ✅ Buen diseño

```python
class Workable(ABC):
    @abstractmethod
    def work(self): ...

class Eatable(ABC):
    @abstractmethod
    def eat(self): ...

class HumanWorker(Workable, Eatable):
    def work(self): ...
    def eat(self): ...

class RobotWorker(Workable):   # Solo implementa lo necesario
    def work(self): ...
```

### 💡 Beneficio
Evita que las clases carguen con métodos innecesarios, reduciendo el acoplamiento y mejorando la cohesión.

---

## 5️⃣ D — Dependency Inversion Principle (DIP)

> *"Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones."*

### Concepto
Las clases de **alto nivel** (lógica de negocio) no deben depender directamente de las clases de **bajo nivel** (detalles de implementación como bases de datos, APIs, etc.). Ambos deben depender de **interfaces o abstracciones**.

### ❌ Mal diseño

```python
class MySQLDatabase:
    def save(self, data): ...

class OrderService:
    def __init__(self):
        self.db = MySQLDatabase()  # ← Acoplado a una implementación específica
```

### ✅ Buen diseño

```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def save(self, data): ...

class MySQLDatabase(Database):
    def save(self, data): ...

class MongoDatabase(Database):
    def save(self, data): ...

class OrderService:
    def __init__(self, db: Database):  # Depende de la abstracción
        self.db = db
```

### 💡 Beneficio
Facilita el cambio de tecnologías (de MySQL a MongoDB, por ejemplo) sin tocar la lógica de negocio. Además, mejora la testabilidad mediante mocks.

---

## 🔗 Relación entre los principios

```
SRP  →  Cada clase tiene un propósito claro
OCP  →  Se extiende sin romper lo existente
LSP  →  La herencia es confiable y coherente
ISP  →  Las interfaces son precisas y enfocadas
DIP  →  El sistema depende de contratos, no de detalles
```

Juntos, estos principios promueven una **arquitectura desacoplada, extensible y fácil de probar**.

---

## 📚 Recursos recomendados

- 📖 *Clean Code* — Robert C. Martin
- 📖 *Agile Software Development: Principles, Patterns, and Practices* — Robert C. Martin
- 🌐 [SOLID Principles — Refactoring Guru](https://refactoring.guru/es)
- 🌐 [SOLID en Wikipedia](https://es.wikipedia.org/wiki/SOLID)

---

## 👤 Autor

> Documento elaborado como parte de una investigación para portafolio académico en Ingeniería de Software.  
> Principios aplicables en lenguajes orientados a objetos: **Python, Java, C#, TypeScript**, entre otros.

---

*© Portafolio de Ingeniería de Software — GitHub*
