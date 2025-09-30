## **Overview**

- **Framework**: FastAPI
- **Scheduler**: APScheduler (for periodic heat data collection)
- **Database**: Initialized at startup using `init_db()`
- **Routers**: Modular endpoints for `barangays`, `ml`, and `future_forecast`
- **Middleware**: Cross-Origin Resource Sharing (CORS) enabled

---

## **Dependencies**

- `fastapi.FastAPI` – Core FastAPI framework
- `fastapi.middleware.cors.CORSMiddleware` – Middleware to handle CORS
- `app.routers.barangays` – Barangay-related API routes
- `app.routers.ml` – Machine learning–related API routes
- `app.routers.future_forecast` – Forecast endpoints for heat risk
- `app.db.init_db` – Initializes the database tables
- `app.tasks.collector.collect_heat_data` – Background task to fetch and save heat logs
- `apscheduler.schedulers.background.BackgroundScheduler` – Schedules periodic tasks

---

## **Application Setup**

### FastAPI App Initialization

```python
app = FastAPI()
scheduler = BackgroundScheduler()
```

- Creates the main FastAPI instance `app`.
- Initializes the background scheduler for periodic jobs.

### Middleware

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"], 
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

- Enables **CORS** for all origins, methods, headers, and credentials.
- Ensures the API can be accessed from different frontends.


### Router Registration

```python
app.include_router(barangays.router)
app.include_router(ml.router)
app.include_router(future_forecast.router)
```

- Registers three modular routers:
    - `barangays.router`: Handles barangay-level heat data endpoints
    - `ml.router`: Handles machine learning or prediction endpoints
    - `future_forecast.router`: Provides future heat risk forecasts

---

## **Event Handlers**

### Startup Event

```python
@app.on_event("startup")
def on_startup():
    init_db()
    scheduler.add_job(collect_heat_data, "interval", minutes=60)
    scheduler.start()
    print("APScheduler started. Collecting heat data every hour.")
```

- Called **once when the application starts**.
- Initializes the database (`init_db()`).
- Schedules `collect_heat_data` to run **every 60 minutes**.
- Starts APScheduler to handle periodic tasks.

### Shutdown Event

```python
@app.on_event("shutdown")
def shutdown_event():
    scheduler.shutdown()
```

- Ensures that APScheduler is cleanly stopped when the app shuts down.

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
- **Response**: JSON confirming the API is running
- **Purpose**: Health-check / sanity-check endpoint

---

## **Note**

- `app/main.py` is the main entry point of the Heat Risk API.
- Combines **routers**, **middleware**, and **background tasks**.
- Provides a **root endpoint** for API health checking.
- Ensures database initialization and automated heat data collection via APScheduler.


---

## **Usage**

### Run the App

```bash
uvicorn app.main:app --reload
```

### Check Root Endpoint

```bash
curl http://127.0.0.1:8000/
```

**Response:**

```json
{
  "message": "Heat Risk API is running"
}
```
