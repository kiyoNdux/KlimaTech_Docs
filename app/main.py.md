## **Overview**

This is the **entry point** of the FastAPI application.  
It:

- Initializes the **FastAPI app instance**
- Includes the **Barangays router** for handling API endpoints
- Runs **database initialization** on application startup
- Integrates a **background scheduler (APScheduler)** to periodically collect heat data
- Provides a **root endpoint** (`/`) for health check

---

## **Dependencies**

- **`fastapi.FastAPI`** – Core FastAPI application framework.
- **`app.routers.barangays`** – Contains all routes related to Barangays.
- **`app.db.init_db`** – Initializes database tables at startup.
- **`app.tasks.collector.collect_heat_data`** – Task function that fetches and saves heat data from Open-Meteo.
- **`apscheduler.schedulers.background.BackgroundScheduler`** – Scheduler that runs jobs in the background at fixed intervals.

---

## **Application Setup**

### App Initialization

```python
app = FastAPI()
scheduler = BackgroundScheduler()
```

- Creates the FastAPI application instance.
- Initializes a background scheduler to manage periodic tasks.

### Router Registration

```python
app.include_router(barangays.router)
```

Registers the **Barangays API routes** under the app.  
All routes with prefix `/barangays` become accessible through this router.

---

## **Event Handlers**

### `on_startup()`

```python
@app.on_event("startup")
def on_startup():
    init_db()
    scheduler.add_job(collect_heat_data, "interval", minutes=60)
    scheduler.start()
    print("APScheduler started. Collecting heat data every hour.")
```

- Runs **once when the application starts**.
- Calls `init_db()` to ensure database tables are created.
- Registers a background job that calls `collect_heat_data()` every **60 minutes**.
- Starts the APScheduler.

### `shutdown_event()`

```python
@app.on_event("shutdown")
def shutdown_event():
    scheduler.shutdown()
```

- Runs when the application shuts down.
- Gracefully stops the scheduler to avoid dangling background threads.

---

## **Endpoints**

### Root Endpoint

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

## **Background Job: Heat Data Collection**

- **Job Function**: `collect_heat_data`
- **Interval**: Every **60 minutes**
- **Purpose**:
    - Fetches weather data for all barangays
    - Computes heat index & risk level
    - Logs results into the database

---

## **Example Usage**

### Run the App

```bash
uvicorn app.main:app --reload
```

### Test the Root Endpoint

```bash
curl http://127.0.0.1:8000/
```

**Response:**

```json
{
  "message": "Heat Risk API is running"
}
```

### Check Scheduler Logs

When the app starts:

```
APScheduler started. Collecting heat data every hour.
```

Every hour, the scheduler will log new heat data entries for each barangay.

---

## **Notes**

- `APScheduler` runs jobs **in the same process** as FastAPI.
- `init_db()` ensures database tables exist before jobs or API requests access them.
- The background job (`collect_heat_data`) makes the service **self-updating**, without needing manual triggers.
- More jobs can be scheduled by adding them to the `scheduler` in `on_startup()`.
