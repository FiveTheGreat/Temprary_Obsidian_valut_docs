## **1. What is Middleware?**

Middleware is a function that runs **before** and **after** each request in FastAPI. It allows you to **modify requests and responses globally**.

Think of it as a **pipeline** where each request passes through a series of steps before reaching the actual API endpoint.

---

## **2. Why Use Middleware?**

✅ **Global Request and Response Handling** – Modify incoming requests or outgoing responses.  
✅ **Logging & Monitoring** – Track API requests, user activity, and response times.  
✅ **Security** – Implement authentication, request validation, and IP blocking.  
✅ **Performance Enhancements** – Enable caching, compression, or rate limiting.  
✅ **Cross-Origin Resource Sharing (CORS)** – Allow or restrict access from different domains.

---

## **3. Where is Middleware Used?**

- **Logging Requests & Responses** 📜
- **Handling Authentication & Authorization** 🔐
- **Adding Security Headers** 🛡️
- **Implementing Rate Limiting** ⏳
- **Modifying API Responses** 🔄
- **Caching Responses** ⚡
- **Handling CORS** 🌍

---

## **4. How to Use Middleware in FastAPI?**

### **🔹 Creating a Simple Middleware**
```python
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware

app = FastAPI()

class CustomMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        print("Before request")
        response = await call_next(request)  # Pass request to the next step
        print("After request")
        return response  # Send response back

app.add_middleware(CustomMiddleware)

@app.get("/")
def home():
    return {"message": "Hello, Middleware!"}

```

🔹 **How it works?**  
1️⃣ Middleware runs **before** the request reaches the endpoint (`"Before request"`).  
2️⃣ The request is processed by the FastAPI route.  
3️⃣ Middleware runs **after** the response is generated (`"After request"`).  
4️⃣ The response is returned to the client.

✅ **Useful for logging, debugging, and modifying requests/responses.**

---

## **5. Middleware for Logging Requests & Responses**

```python
import time
from fastapi import Request

class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        start_time = time.time()
        response = await call_next(request)
        process_time = time.time() - start_time
        print(f"Request: {request.method} {request.url} | Time: {process_time:.2f}s")
        return response

app.add_middleware(LoggingMiddleware)

```
✅ **Why use this?**

- Logs request method (`GET`, `POST`, etc.), URL, and execution time.
- Helps monitor API performance and identify slow endpoints.

---

## **6. Middleware for Authentication**
```python 
from fastapi import HTTPException

class AuthMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        token = request.headers.get("Authorization")
        if not token or token != "Bearer valid_token":
            raise HTTPException(status_code=401, detail="Unauthorized")
        return await call_next(request)

app.add_middleware(AuthMiddleware)

```
✅ **Why use this?**

- Ensures all API requests **must** have a valid authentication token.
- Centralized authentication logic instead of checking in every endpoint.

---

## **7. Middleware for Modifying Responses**
```python
class ResponseMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        response = await call_next(request)
        response.headers["X-Custom-Header"] = "Middleware Works!"
        return response

app.add_middleware(ResponseMiddleware)
```
✅ **Why use this?**

- Adds custom headers to all API responses.
- Useful for **security headers, API versioning, and tracking.**

---

## **8. Middleware for CORS (Cross-Origin Resource Sharing)**

If your frontend is on a different domain (`http://localhost:3000`) and the backend is on (`http://localhost:8000`), browsers block cross-origin requests by default.

### **🔹 Solution: Use CORS Middleware**
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # Allow all domains (*)
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["*"],  # Allow all headers
)
```

✅ **Why use this?**

- **Prevents CORS errors** in frontend-backend communication.
- Controls which origins, methods, and headers are allowed.

---

## **9. Middleware for Rate Limiting (Prevent API Abuse)**
```python
from starlette.middleware.base import BaseHTTPMiddleware
from fastapi.responses import JSONResponse

request_count = {}

class RateLimitMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        client_ip = request.client.host
        request_count[client_ip] = request_count.get(client_ip, 0) + 1

        if request_count[client_ip] > 5:  # Limit 5 requests per IP
            return JSONResponse(status_code=429, content={"message": "Rate limit exceeded"})

        return await call_next(request)

app.add_middleware(RateLimitMiddleware)

```
✅ **Why use this?**

- **Protects your API from spam and abuse.**
- Limits requests per IP address.

---

## **10. Best Practices for Using Middleware**

✅ **Use middleware for global concerns** → Logging, security, and performance.  
✅ **Avoid complex logic inside middleware** → Keep them lightweight.  
✅ **Use middleware caching wisely** → Prevent unnecessary computation.  
✅ **Stack multiple middleware** → Chain them for better modularity.  
✅ **Monitor middleware performance** → Optimize if it slows down the API.

---

## **11. When NOT to Use Middleware?**

❌ **For request-specific logic** → Use route dependencies instead.  
❌ **For database queries** → Middleware runs before routes, avoid DB calls here.  
❌ **For high-frequency operations** → Middleware affects every request, so keep it efficient.

---

## **12. Summary: Why Middleware is Powerful**

✅ **Global API Control** → Apply logic across all routes.  
✅ **Enhances Security** → Centralized authentication, request validation.  
✅ **Improves Performance** → Logging, monitoring, caching.  
✅ **Simplifies Development** → Reduces redundant logic in individual routes.