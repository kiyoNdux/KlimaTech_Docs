## **Overview**

This `requirements.txt` file defines all the Python dependencies needed to run the **Heat Risk API**.  
It includes core frameworks (FastAPI, SQLModel), database drivers (PostgreSQL/SQLite), HTTP clients, and supporting utilities.

---

## **Key Dependencies**

### Web Framework & Server

- **fastapi==0.116.1** → The main web framework for building the Heat Risk API.
- **starlette==0.47.3** → Provides ASGI tools, routing, and middleware (FastAPI is built on top of Starlette).
- **uvicorn==0.35.0** → ASGI server for running the FastAPI app.

### Database & ORM

- **sqlmodel==0.0.24** → ORM (built on top of SQLAlchemy + Pydantic) for models and schema validation.
- **SQLAlchemy==2.0.43** → Underlying database toolkit used by SQLModel.
- **psycopg2-binary==2.9.10** → PostgreSQL database driver.
- **greenlet==3.2.4** → Required by SQLAlchemy for efficient database operations.

### HTTP Requests

- **httpx==0.28.1** → Async HTTP client for fetching weather data from the Open-Meteo API.
- **httpcore==1.0.9** → Core networking layer for `httpx`.
- **h11==0.16.0** → HTTP/1.1 protocol implementation used by ASGI/HTTP clients.

### Data Validation & Types

- **pydantic==2.11.7** → Data validation and parsing library (used heavily in FastAPI & SQLModel).
- **pydantic_core==2.33.2** → Low-level validation engine powering Pydantic v2.
- **typing_extensions==4.15.0** → Backports of newer typing features for compatibility.
- **typing-inspection==0.4.1** → Type inspection utilities (used internally by Pydantic/FastAPI).
- **annotated-types==0.7.0** → Support for `typing.Annotated`, useful for FastAPI request validation.

### Utilities

- **python-dotenv==1.1.1** → Loads environment variables from `.env` files.
- **anyio==4.10.0** → Async concurrency library used by Starlette/FastAPI.
- **sniffio==1.3.1** → Detects which async library is running (required by `anyio`).
- **click==8.2.1** → Command-line interface building library (used by Uvicorn).
- **colorama==0.4.6** → Cross-platform colored terminal text (for CLI logging).
- **certifi==2025.8.3** → Provides up-to-date root SSL certificates.
- **idna==3.10** → Handles internationalized domain names in URLs.

---

## **Usage**

### Install dependencies

```bash
pip install -r requirements.txt
```

### Freeze updated dependencies

If new packages are installed during development:

```bash
pip freeze > requirements.txt
```

---

## **Notes**

- Versions are **pinned** for reproducibility, ensuring consistent builds across environments.
- For **local development**, we use `pip install -r requirements.txt` directly.
- In **production**, dependencies are installed automatically inside the Docker image (`Dockerfile`).
