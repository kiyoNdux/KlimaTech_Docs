## **Overview**

This is the **entry point** of the FastAPI application.  
It:

- Initializes the **FastAPI app instance**
- Includes the **Barangays router** for handling API endpoints
- Runs **database initialization** on application startup
- Provides a **root endpoint** (`/`) for health check

---

## **Dependencies**

- **`fastapi.FastAPI`** – Core FastAPI application framework.
- **`app.routers.barangays`** – Contains all routes related to Barangays.
- **`app.db.init_db`** – Initializes database tables at startup.

---

## **Application Setup**

### **App Initialization**

```python
app = FastAPI()
```

Creates the FastAPI application instance.

### **Router Registration**

```python
app.include_router(barangays.router)
```

Registers the **Barangays API routes** under the app.  
All routes with prefix `/barangays` become accessible through this router.

---

## **Event Handlers**

### **`on_startup()`**

```python
@app.on_event("startup")
def on_startup():
    init_db()
```

- Runs **once when the application starts**.
- Calls `init_db()` to ensure database tables are created before serving requests.

---

## **Endpoints**

### **Root Endpoint**

```python
@app.get("/")
def root():
    return {"message": "Heat Risk API is running"}
```

- **Path**: `/`
- **Method**: `GET`
- **Response**: JSON message confirming the API is running.
- **Purpose**: Health-check / sanity check endpoint.

---

## **Example Usage**

### **Run the App**

```bash
uvicorn main:app --reload
```

### **Test the Root Endpoint**

```bash
curl http://127.0.0.1:8000/
```

**Response:**

```json
{
  "message": "Heat Risk API is running"
}
```

### **Access Barangays API**

All barangay-related endpoints are available under:

```
http://127.0.0.1:8000/barangays
```

---

## **Notes**

- `init_db()` ensures database models are ready without requiring manual setup.
- Routers make the code modular and maintainable (e.g., you can add more routers later for `users`, `auth`, etc.).
- The root endpoint doubles as a **status check** to confirm if the service is online.
