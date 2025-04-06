FastAPI fully supports **MySQL databases** using **SQLAlchemy ORM** for structured data handling. This guide covers **setting up MySQL with FastAPI**, performing **CRUD operations**, and implementing **best practices for performance optimization**.

---

## **1. Why Use MySQL with FastAPI?**

✅ **Structured Data** – Enforces schema with tables, relationships, and constraints.  
✅ **ORM Support** – SQLAlchemy provides an easy-to-use ORM for Python developers.  
✅ **Scalability** – MySQL supports sharding, replication, and clustering.  
✅ **High Performance** – Supports indexing, caching, and connection pooling.

---
## **2. Installing Dependencies**

Install the required packages:
```cmd
pip install fastapi[all] sqlalchemy pymysql alembic
```

- `sqlalchemy` → ORM for handling SQL operations.
- `pymysql` → MySQL driver for Python.
- `alembic` → For handling database migrations.

---

## **3. Setting Up MySQL Connection in FastAPI**

### **🔹 Step 1: Configure Database Connection**

Create a `database.py` file:

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "mysql+pymysql://username:password@localhost:3306/mydatabase"

engine = create_engine(DATABASE_URL, pool_size=10, max_overflow=20)  # Connection pooling

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

```

🔹 **Key Points:**

- `"mysql+pymysql://username:password@localhost:3306/mydatabase"` → Replace with your actual MySQL credentials.
- `create_engine()` → Establishes a connection with MySQL.
- `pool_size=10, max_overflow=20` → Optimizes database performance by **using connection pooling**.

---

### **🔹 Step 2: Define SQLAlchemy Models**

Create a `models.py` file:
```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship
from .database import Base

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    username = Column(String(100), unique=True, index=True)
    email = Column(String(150), unique=True, index=True)
    password = Column(String(255))

    tasks = relationship("Task", back_populates="owner")

class Task(Base):
    __tablename__ = "tasks"

    id = Column(Integer, primary_key=True, index=True)
    title = Column(String(200), index=True)
    description = Column(String(500))
    owner_id = Column(Integer, ForeignKey("users.id"))

    owner = relationship("User", back_populates="tasks")

```
🔹 **Key Features:**

- `String(100)`, `String(150)`, `String(255)` → Defines column sizes **optimized for MySQL storage**.
- `relationship("Task", back_populates="owner")` → Establishes a **one-to-many** relationship.

---

### **🔹 Step 3: Create Database Session Dependency**

Create a `dependencies.py` file:

```python
from sqlalchemy.orm import Session
from .database import SessionLocal
from fastapi import Depends

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

```
🔹 **Why use this?**

- Ensures **proper session management** (opens and closes connections automatically).
- Uses **dependency injection** (`Depends(get_db)`) in FastAPI routes.

---

### **🔹 Step 4: Create CRUD Operations**

Create a `crud.py` file:
```python
from sqlalchemy.orm import Session
from . import models, schemas

def create_user(db: Session, user: schemas.UserCreate):
    new_user = models.User(username=user.username, email=user.email, password=user.password)
    db.add(new_user)
    db.commit()
    db.refresh(new_user)
    return new_user

def get_users(db: Session, skip: int = 0, limit: int = 10):
    return db.query(models.User).offset(skip).limit(limit).all()

```

🔹 **Key Features:**

- `db.add(new_user)` → Adds a user to the database.
- `db.commit()` → Saves the changes.
- `db.refresh(new_user)` → Updates the model with the latest database values.
- `query(models.User).all()` → Retrieves all users from MySQL.

---

### **🔹 Step 5: Create API Routes**

Create a `main.py` file:
```python
from fastapi import FastAPI, Depends
from sqlalchemy.orm import Session
from . import models, schemas, crud
from .database import engine
from .dependencies import get_db

app = FastAPI()

models.Base.metadata.create_all(bind=engine)  # Auto-create tables

@app.post("/users/", response_model=schemas.User)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
    return crud.create_user(db, user)

@app.get("/users/", response_model=list[schemas.User])
def read_users(db: Session = Depends(get_db), skip: int = 0, limit: int = 10):
    return crud.get_users(db, skip=skip, limit=limit)

```

🔹 **Key Features:**

- `create_user()` → Creates a new user in MySQL.
- `read_users()` → Retrieves users with pagination support.
- `Depends(get_db)` → Injects the **database session dependency**.

---

## **4. Managing Database Migrations with Alembic**

If you modify your database schema (e.g., adding a new column), use **Alembic** for migrations.

### **🔹 Step 1: Initialize Alembic**
```python
alembic init alembic
```
This creates an `alembic` directory with configuration files.

### **🔹 Step 2: Configure Alembic for MySQL**

Edit `alembic/env.py` and update:
```python
from sqlalchemy import engine_from_config
from sqlalchemy import pool
from alembic import context
from myproject.database import Base, DATABASE_URL

config = context.config
config.set_main_option("sqlalchemy.url", DATABASE_URL)

target_metadata = Base.metadata

```

### **🔹 Step 3: Generate and Apply Migrations**

Generate migration:
```cmd
alembic revision --autogenerate -m "Initial migration"
```

apply the migration:
```cmd
alembic upgrade head
```

🔹 **Why use Alembic?**

- **Tracks schema changes** and applies them automatically.
- **Prevents data loss** by handling migrations safely.

---

## **5. Optimizing MySQL Performance in FastAPI**

✅ **Use Connection Pooling**

```cmd
engine = create_engine(DATABASE_URL, pool_size=10, max_overflow=20)
```

- `pool_size=10` → Allows up to **10 active connections**.
- `max_overflow=20` → Allows **20 additional temporary connections**.

✅ **Use Indexing for Faster Queries**
```cmd
Column(String, index=True)
```
- Speeds up search queries on frequently accessed columns.

✅ **Use Bulk Inserts for Efficiency**
```cmd
db.bulk_save_objects([user1, user2, user3])
db.commit()
```

- **Minimizes database round trips** and improves performance.

✅ **Use Asynchronous Database Access for High Load**  
Install `async MySQL` support:
```cmd
pip install aiomysql
```
Use `async SQLAlchemy` or `Tortoise ORM` for better performance.

✅ **Optimize Query Performance**  
Use `EXPLAIN` to analyze query execution:

```cmd
EXPLAIN SELECT * FROM users WHERE email='test@example.com';
```
- Helps identify slow queries and missing indexes.

---

## **6. Summary: Best Practices for MySQL in FastAPI**

✅ Use **SQLAlchemy ORM** for structured data handling.  
✅ Use **Alembic for database migrations**.  
✅ Use **Connection Pooling** to optimize performance.  
✅ Use **Indexes on frequently searched columns**.  
✅ Use **Bulk Inserts for inserting large data efficiently**.  
✅ Use **Async Database Access for high-traffic applications**.