# Heat Risk API – Documentation

## **Overview**

This repository serves as the **documentation hub** for the **Heat Risk API backend system**.  
It explains the purpose, design, and functionality of each file in the backend, along with supporting materials such as database design diagrams and environment setup references.

This is **not the production repository** — no running server or live API is included here.  
Instead, it provides structured documentation for developers, collaborators, and future maintainers.

---

## **Repository Structure**

```bash
backend/
│── app/
│   │── routers/
│   │   │── barangays.py        # API routes for barangays
│   │── schemas/
│   │   │── barangay.py         # Pydantic schemas (Barangay)
│   │   │── database.py         # Pydantic schemas (HeatLog)
│   │── scripts/
│   │   │── add_barangays.py    # Script to seed barangay data
│   │── tasks/
│   │   │── collector.py        # Weather data collection task
│   │── utils/
│   │   │── heat_index.py       # Heat index calculation
│   │── __init__.py
│   │── db.py                   # Database configuration (SQLite/Postgres)
│   │── main.py                 # Application entrypoint
│   │── model.py                # SQLModel ORM models
│   │── heat_project.db         # SQLite database (dev example)
│── .env                        # Example environment variables
│── .gitignore
│── DB_design.svg               # Database schema design (diagram)
│── requirements.txt            # Python dependencies
frontend/                       # (placeholder for frontend documentation)
venv/                           # Virtual environment
.gitignore
instructions.txt                # Setup or reference notes
README.md                       # This documentation overview
```

---

## **Documentation Coverage**

Each major file/module has its own detailed documentation, stored directly in this repository:

- **`main.py`** → API entrypoint, event handlers, router inclusion, APScheduler jobs
- **`db.py`** → Database setup (SQLite for dev, PostgreSQL for prod)
- **`models.py`** → SQLModel ORM definitions (Barangay, HeatLog)
- **`schemas/`** → Pydantic models for request/response validation
- **`routers/barangays.py`** → Endpoints for barangay-level data access
- **`scripts/add_barangays.py`** → Database seeding script
- **`tasks/collector.py`** → Hourly background job to fetch Open-Meteo data
- **`utils/heat_index.py`** → Heat index computation with NOAA risk levels
- **`requirements.txt`** → Dependency documentation
- **`Dockerfile`** → Deployment containerization

---

## **Database Design**

The **ERD diagram** for this project is available in:

```
DB_design.svg
```

It visually explains how **Barangays** and **HeatLogs** are related.


---

## **Notes**

- This repo **does not contain the live backend**. It only documents its structure and behavior.
- For implementation and deployment, refer to the **actual backend repository** → [here](https://github.com/neophiles/KlimaTech.git).
- The documentation assumes familiarity with **FastAPI**, **SQLModel**, and **Python 3.12**.
