## **Overview**

This file manages **database configuration and connections** for the project.  
It supports **two environments**:

- **Development (`dev`)** – Uses **SQLite** (local database).
- **Production (`prod`)** – Uses **PostgreSQL** (with credentials from environment variables).

It also provides utilities for:

- **Initializing the database schema** (`init_db`)
- **Providing a session generator** (`get_session`)

---

## **Dependencies**

- **`os`** – Access environment variables.
- **`sqlmodel`** – ORM and database engine handling (`SQLModel`, `create_engine`, `Session`).
- **`dotenv`** – Loads environment variables from `.env` file.

---

## **Environment Setup**

- Environment variable:
    - `ENV` – Determines environment mode (`dev` or `prod`). Defaults to `"dev"`.
- For **production (`prod`)**, the following variables must be set in `.env`:
    - `DB_USER`
    - `DB_PASSWORD`
    - `DB_HOST`
    - `DB_PORT`
    - `DB_NAME`

---

## **Configuration**

- **Development (`dev`)**
    - Database: SQLite
    - Path: `./heat_project.db`
    - Engine: `create_engine(DATABASE_URL, echo=True)`

- **Production (`prod`)**
    - Database: PostgreSQL
    - URL built dynamically from `.env` credentials
    - Example:

 ```
 postgresql://user:password@localhost:5432/mydb
```


---

## **Functions**

### **`init_db()`**

Initializes the database by creating tables defined in `SQLModel` models.

**Usage:**

```python
from db import init_db
init_db()
```


### **`get_session()`**

Generator function that yields a **database session**.  
Ensures proper session management using context handling.

**Usage (FastAPI dependency example):**

```python
from fastapi import Depends
from sqlmodel import Session
from db import get_session

@app.get("/items/")
def get_items(session: Session = Depends(get_session)):
    result = session.exec(select(Item)).all()
    return result
```


---

## **Notes**

- **`echo=True`** enables SQL query logging for debugging (turn off in production for performance).
- `get_session` is a generator designed to integrate smoothly with **FastAPI’s dependency injection** system.
- Switching between SQLite and PostgreSQL is controlled only by the `ENV` variable.
