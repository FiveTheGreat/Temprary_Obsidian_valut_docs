## **1. What is Dependency Injection?**

**Dependency Injection (DI)** is a design pattern where dependencies (external services, configurations, or reusable functions) are **injected into a function or class instead of being created inside it**.

In FastAPI, DI is implemented using the `Depends()` function. It allows us to **reuse code**, **manage dependencies centrally**, and **write cleaner, more modular applications**.

---

## **2. Why Use Dependency Injection?**

✅ **Reusability** → No need to repeat code in multiple routes.  
✅ **Separation of Concerns** → Keeps route handlers clean and focused.  
✅ **Scalability** → Easy to extend and manage growing applications.  
✅ **Testability** → Makes unit testing easier by allowing mock dependencies.  
✅ **Performance** → FastAPI caches dependencies by default for efficiency.

---

## **3. Where is Dependency Injection Used?**

- **Database Connections** → Injecting a DB session.
- **Authentication & Authorization** → Verifying user roles and tokens.
- **Logging & Monitoring** → Injecting logging functions.
- **Configuration Management** → Centralized app settings.
- **Reusable Business Logic** → Code reuse across multiple endpoints.

---

## **4. How to Use Dependency Injection in FastAPI?**

### **🔹 Basic Example**
```python
from fastapi import FastAPI, Depends

app = FastAPI()

def get_message():
    return "Hello from dependency!"

@app.get("/")
def home(message: str = Depends(get_message)):
    return {"message": message}

```
🔹 **How it works?**

- `get_message()` is injected into the `home()` route using `Depends()`.
- FastAPI **calls `get_message()` automatically** and passes the return value (`"Hello from dependency!"`) to `message`.
- The API response will be:
```json
{"message": "Hello from dependency!"}
```

---
## **5. Advanced Dependency Injection Examples**

### **🔹 Injecting Database Connection (SQLAlchemy)**
```python
from fastapi import Depends
from sqlalchemy.orm import Session
from database import SessionLocal

def get_db():
    db = SessionLocal()
    try:
        yield db  # Provides DB session
    finally:
        db.close()  # Closes session after request

@app.get("/users/")
def get_users(db: Session = Depends(get_db)):
    return db.query(User).all()

```

✅ **Why use `yield`?**

- **Keeps DB connections clean** by automatically closing them after use.
- **Avoids connection leaks** in high-traffic applications.
---
### Injecting Authentication & Authorization
```python
from fastapi import HTTPException, Security
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def get_current_user(token: str = Security(oauth2_scheme)):
    if token != "valid_token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return {"username": "testuser"}

@app.get("/profile/")
def get_profile(user: dict = Depends(get_current_user)):
    return {"user": user}

```

✅ **Why use `Security()`?**

- `Security()` works similarly to `Depends()`, but is specifically designed for **authentication and authorization**.
- Used for injecting authentication mechanisms like JWT tokens, API keys, OAuth2, etc.

---

### **🔹 Using Class-Based Dependencies**
```python
class Config:
    def __init__(self):
        self.setting = "Production Mode"

def get_config():
    return Config()

@app.get("/config/")
def read_config(config: Config = Depends(get_config)):
    return {"setting": config.setting}

```
✅ **Why use class-based dependencies?**

- Useful for managing **configurations, business logic, and services** in large applications.

---

### **🔹 Using Dependencies in Middleware**
You can use DI inside **middleware** to inject dependencies globally.

```python
from fastapi import Request
from starlette.middleware.base import BaseHTTPMiddleware

class CustomMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        response = await call_next(request)
        response.headers["X-Injected"] = "Dependency Injection Works!"
        return response

app.add_middleware(CustomMiddleware)

```
✅ **Why use dependency injection in middleware?**

- To **modify requests and responses globally** (e.g., logging, security headers, rate limiting).

---

## **6. Dependency Injection with Parameters**

You can pass **parameters** to dependency functions.
```python
def common_parameters(q: str = None, limit: int = 10):
    return {"query": q, "limit": limit}

@app.get("/search/")
def search(params: dict = Depends(common_parameters)):
    return params

```

✅ **Why use parameters in dependencies?**

- It makes dependencies **more dynamic and configurable**.
- Useful for **pagination, filtering, and other request-based logic**.

---

## **7. Handling Dependency Injection Errors**

### **🔹 Example: Handling Authentication Errors**
```python
from fastapi import HTTPException

def get_current_user(token: str = Depends(oauth2_scheme)):
    if token != "valid_token":
        raise HTTPException(status_code=401, detail="Unauthorized")
    return {"user": "admin"}

```

✅ **Best Practices for Handling Errors in Dependencies:**

- Use `HTTPException` for clear API responses.
- Always **validate user input and authentication tokens** before processing requests.

---

## **8. Performance Optimization with Dependencies**

FastAPI **caches dependencies** by default (when they don’t use `yield`), meaning they **only run once per request**.

### **🔹 Example: Caching Expensive Computations**
```python
import time

def slow_dependency():
    time.sleep(2)  # Simulate expensive operation
    return "Cached Result"

@app.get("/expensive/")
def expensive_route(result: str = Depends(slow_dependency)):
    return {"result": result}

```

✅ **Why is this important?**

- Dependencies **run only once per request**, avoiding redundant computations.
- Improves **API response time** and **server performance**.

---

## **9. Overriding Dependencies (For Testing)**

In **unit tests**, you may need to override dependencies.
```python
from fastapi.testclient import TestClient

app.dependency_overrides[get_db] = lambda: None  # Override DB with a mock

client = TestClient(app)

def test_get_users():
    response = client.get("/users/")
    assert response.status_code == 200

```

✅ **Why override dependencies?**

- To **mock external services** (e.g., database, authentication) for testing.
- Helps **run tests in isolation** without modifying real data.

---

## **10. When NOT to Use Dependency Injection**

❌ **For very simple logic** (like adding two numbers).  
❌ **If it introduces unnecessary complexity** in small applications.  
❌ **If performance overhead is a concern** (though FastAPI’s DI is highly optimized).

---

## **Conclusion: Why Dependency Injection is Powerful**

✅ **Reduces Code Duplication** → Write once, reuse everywhere.  
✅ **Improves Maintainability** → Cleaner and more modular architecture.  
✅ **Enhances Testability** → Easily mock dependencies for unit tests.  
✅ **Boosts Performance** → FastAPI caches dependencies automatically.

---
