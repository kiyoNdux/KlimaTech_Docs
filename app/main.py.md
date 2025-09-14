## `Code Overview`

```python
from fastapi import FastAPI
from app.routers import barangays
from app.db import init_db

app = FastAPI()

# Register routers
app.include_router(barangays.router)

# Database initialization on startup
@app.on_event("startup")
def on_startup():
    init_db()

# Root health-check endpoint
@app.get("/")
def root():
    return {"message": "Heat Risk API is running"}
```

---

### Components

#### `app = FastAPI()`

- Creates the main FastAPI application instance.
- Used to register routes, events, and middleware.


---
### Router Inclusion

```python
app.include_router(barangays.router)
```

- Imports and registers the **Barangay router**, making `/barangays` and related endpoints available.


---
### Startup Event

```python
@app.on_event("startup")
def on_startup():
    init_db()
```

- Runs when the FastAPI application starts.
- Calls `init_db()` from `db.py` to create tables in the database if they don’t exist.


---
### Root Endpoint

```python
@app.get("/")
def root():
    return {"message": "Heat Risk API is running"}
```

- Provides a simple JSON response confirming that the API is online.
- Useful as a **health-check** endpoint.
