# FastAPI Basics and Core Concepts for a 2-Year Experienced Developer

## FastAPI Basics and Core Concepts
FastAPI is a modern, high-performance web framework for building APIs with Python. It is based on Pydantic and Starlette and is known for its speed and automatic data validation.

### Key Features:
- **Type Hints for Validation**: Uses Python type hints to validate request and response data.
- **Asynchronous Support**: Supports `async` and `await` for high-performance applications.
- **Auto-generated Documentation**: Provides Swagger UI (`/docs`) and ReDoc (`/redoc`).
- **Dependency Injection**: Makes managing dependencies easier.

### Example of a Simple FastAPI Application
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI!"}
```

---

## Request Body and Response Model in FastAPI
FastAPI allows handling request bodies using **Pydantic models** for data validation and serialization. When sending data to a FastAPI endpoint, you can use:

- **Request Body** (`Body`): To extract JSON data from the request.
- **Response Model**: To define the structure of the response.

### Example:
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    stock: bool = False

@app.post("/item")
def create_item(item: Item):
    return {"name": item.name, "price": item.price, "stock": item.stock}
```

---

## 422 Unprocessable Entity Error in FastAPI
### **What Does It Mean?**
A `422 Unprocessable Entity` error occurs when the request data **does not match** the expected model.

### **Example of an Issue**
#### **Code:**
```python
from fastapi import FastAPI, Body
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    stock: bool = False

@app.post("/item")
def create_item(
        item: Item,
        discount: float = Body(..., gt=0, lt=1)
):
    final_price = item.price * (1-discount)
    return {"name": item.name, "final_price": final_price}
```

#### **Incorrect Request Format**
```json
{
  "name": "Laptop",
  "price": 1000.0,
  "stock": true,
  "discount": 0.1
}
```
✅ Expected JSON structure:
```json
{
  "item": {
    "name": "Laptop",
    "price": 1000.0,
    "stock": true
  },
  "discount": 0.1
}
```

---

## Why Use `List` from `typing` Instead of `list`
FastAPI and Pydantic prefer `List[Type]` from `typing` for:

- **Type Validation**: Ensures that the list contains elements of a specific type.
- **Better Autocompletion**: Helps IDEs provide better hints.
- **Strict Data Validation**: Avoids runtime issues.

### Example:
```python
from typing import List
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float

class Order(BaseModel):
    items: List[Item]  # Enforces list of `Item` objects
```
✅ This ensures that `items` is **always a list of `Item` objects**, preventing unexpected data types.

---

## Handling Discount Errors (422 Error on Large Discounts)
### **Issue**: If `discount=70`, it exceeds the range `gt=0, lt=1`.

### **Solution 1: Allow Higher Discount Values**
```python
@app.post("/item")
def create_item(
    item: Item,
    discount: float = Body(..., gt=0, lt=100)  # ✅ Allow 0-99%
):
    final_price = item.price * (1 - (discount / 100))  # Convert to fraction
    return {"name": item.name, "final_price": final_price}
```

### **Solution 2: Custom Error Handling**
```python
from fastapi import HTTPException

@app.post("/item")
def create_item(
    item: Item,
    discount: float = Body(..., gt=0, lt=1)  # Keeps the original range
):
    if discount > 1:
        raise HTTPException(
            status_code=400, 
            detail="Discount should be between 0 and 1 (e.g., 0.7 for 70%)"
        )
    final_price = item.price * (1 - discount)
    return {"name": item.name, "final_price": final_price}
```

### **Solution 3: Auto-Convert Large Discounts**
```python
@app.post("/item")
def create_item(
    item: Item,
    discount: float = Body(..., gt=0)  # ✅ Remove `lt=1`
):
    if discount > 1:
        discount = discount / 100  # Convert 70 → 0.7
    final_price = item.price * (1 - discount)
    return {"name": item.name, "final_price": final_price}
```

---

## Final Takeaways
- Use `Body()` explicitly when combining **Pydantic models** and **separate body parameters**.
- Use `List[Type]` from `typing` for strict type validation in FastAPI.
- Handle `422` errors by defining **clear validation rules** and providing **meaningful error messages**.
- Convert `discount` values dynamically if users enter percentages instead of fractions.

🚀 **Following these practices ensures cleaner and more robust FastAPI applications!**
