### **1. FastAPI Core Concepts Explained** 🚀

FastAPI is a high-performance web framework for building APIs with Python. It is designed to be **fast**, **efficient**, and **developer-friendly**. Here’s a deep dive into the **core concepts**, their purpose, use cases, and how to handle them properly.

---

## **✅ Path Operations & Routing**

### **What is it?**

Path operations define the **endpoints (routes)** in your FastAPI application. They determine how requests are handled when users access specific URLs.

### **Why use it?**

- Allows defining API endpoints (`GET`, `POST`, `PUT`, `DELETE`).
- Helps in organizing the application structure.
- Supports path and query parameters for dynamic behavior.

### **Where is it used?**

- Creating API endpoints for CRUD operations.
- Defining routes in RESTful applications.
- Handling dynamic URLs (e.g., fetching user profiles).

### **Example**
```python
from fastapi import FastAPI

app = FastAPI()

# Basic GET request
@app.get("/")
def home():
    return {"message": "Welcome to FastAPI"}

# Path parameter
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}

# Query parameter
@app.get("/search")
def search_items(q: str):
    return {"search_query": q}

```

### **Handling Issues**

- Use correct data types for parameters (`int`, `str`, etc.).
- Handle missing or optional parameters properly using default values (`q: str = None`).
- Use proper validation with Pydantic models for structured inputs.

---
## **✅ Request & Response Models (Pydantic Validation)**

### **What is it?**

FastAPI uses **Pydantic** models (`BaseModel`) to validate incoming requests and structure outgoing responses.

### **Why use it?**

- Ensures data integrity.
- Prevents incorrect or missing fields in API requests.
- Helps with automatic API documentation.

### **Where is it used?**

- Validating form data, JSON payloads, and request bodies.
- Structuring API responses.
- Defining schemas for user input (e.g., login, sign-up).

### **Example**
```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    email: str
    age: int

@app.post("/users/")
def create_user(user: User):
    return {"message": f"User {user.name} created!"}

```
### **Handling Issues**

- Use `Optional` fields when some values are not required.
- Implement custom validation for complex data.
- Catch validation errors using FastAPI’s exception handling.

----
## **✅ Query Parameters, Path Parameters, and Request Bodies**

### **What is it?**

FastAPI allows passing **parameters** in three different ways:

1. **Path Parameters** – Dynamic values in the URL.
2. **Query Parameters** – Key-value pairs in the URL after `?`.
3. **Request Body** – JSON or form data in `POST` requests.

### **Why use it?**

- Path parameters help in fetching resources (e.g., `/users/{id}`).
- Query parameters are great for filtering and searching (`/items?limit=10`).
- Request bodies are used to send structured data (`POST` or `PUT` requests).

### **Where is it used?**

- Path params → Fetching specific users, posts, or products.
- Query params → Searching, pagination, sorting.
- Request body → User registration, data submission.

### **Example**
```python
from fastapi import Query

@app.get("/products/")
def get_products(limit: int = 10, category: str = Query(default=None)):
    return {"limit": limit, "category": category}

@app.post("/orders/")
def create_order(order: dict):
    return {"status": "Order placed", "order_details": order}

```

### **Handling Issues**

- Use default values to avoid missing parameters (`limit: int = 10`).
- Validate query params (`Query(min_length=3)`).
- Handle missing body data with Pydantic models.
---
## **✅ Dependency Injection (`Depends()`)**

### **What is it?**

`Depends()` is used to inject reusable dependencies, such as **database connections, authentication checks, and services**, into FastAPI routes.

### **Why use it?**

- Reduces duplicate code by reusing functions.
- Makes code cleaner and modular.
- Useful for authentication, authorization, and database sessions.

### **Where is it used?**

- User authentication (`OAuth2PasswordBearer`).
- Database session management.
- Logging and request validation.

### **Example**
```python
from fastapi import Depends

def common_parameters(q: str = None):
    return {"q": q}

@app.get("/items/")
def read_items(commons: dict = Depends(common_parameters)):
    return commons

```
### **Handling Issues**

- Ensure dependencies are correctly typed (`Depends(SomeFunction)`).
- Handle authentication failures with proper exception handling.
- Keep dependencies modular and reusable.
[>>>For more about Dependecy Injection](DependecyInjection.md)

---
## **✅ Middleware**

### **What is it?**

Middleware functions **run before or after** every request in FastAPI.

### **Why use it?**

- Modify request or response globally.
- Implement CORS, authentication, or logging.
- Enhance security (e.g., rate limiting).

### **Where is it used?**

- Logging API requests.
- Adding security headers.
- Handling CORS for frontend integration.

### **Example**
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
    allow_headers=["*"],
)

```
### **Handling Issues**

- Ensure CORS is correctly configured for frontend-backend communication.
- Optimize middleware order to avoid unnecessary performance overhead.
[>>>For more about MiddleWare](MiddleWare.md)

---
## **✅ Background Tasks (`BackgroundTasks`)**

### **What is it?**

FastAPI allows running background tasks **without blocking the main request** using `BackgroundTasks`.

### **Why use it?**

- Avoids long response times.
- Improves API performance.
- Useful for sending emails, logging, and scheduled tasks.

### **Where is it used?**

- Sending confirmation emails after registration.
- Logging API requests asynchronously.
- Processing uploaded files.

### **Example**
```python
from fastapi import BackgroundTasks

def write_log(message: str):
    with open("log.txt", "a") as file:
        file.write(message + "\n")

@app.post("/notify/")
def send_notification(background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log, "User registered!")
    return {"message": "User notified!"}

```
### **Handling Issues**

- Ensure tasks are **non-blocking** (no DB operations in tasks).
- Use Celery for **heavy background tasks**.
- Monitor logs to debug failed background tasks.
---
