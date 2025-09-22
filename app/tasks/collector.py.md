## **Overview**

The `collector.py` script is responsible for **fetching real-time weather data** for all registered barangays from the [Open-Meteo API](https://open-meteo.com/) and storing the results in the database as `HeatLog` entries.

This serves as the **data ingestion pipeline** of the system, ensuring that heat index and risk levels are continuously updated for monitoring and analysis.

---

## **Key Features**

- Fetches **current weather data** (`temperature`, `humidity`, `wind speed`, `UV index`) for each barangay.
- Uses the **Open-Meteo API** as the external weather data source.
- Computes **Heat Index (HI)** and determines **Risk Level** using the utility function `calculate_heat_index`.
- Saves results into the database with a timestamp (`recorded_at`).
- Handles errors gracefully per barangay.

---

## **Dependencies**

- **httpx** → for making HTTP requests to the Open-Meteo API.
- **sqlmodel** → for database session management and ORM operations.
- **datetime** → to handle timestamps, with timezone adjustment for the Philippines (UTC+8).
- **app.models** (`Barangay`, `HeatLog`) → database models.
- **app.utils.heat_index** → custom module for heat index calculation logic.

---

## **Constants**

- `PH_TZ`: Sets timezone to **Asia/Manila (UTC+8)**.
- `OPEN_METEO_URL`: Base URL for Open-Meteo’s forecast API.

---

## **Function: `collect_heat_data()`**

### Purpose

Fetches the latest weather data for each barangay and logs it into the database with a computed heat index and risk level.

### Workflow

1. **Start Session**  
    Opens a database session with SQLModel.
2. **Query Barangays**  
    Retrieves all barangays stored in the database.
3. **For Each Barangay**
    - Build API request parameters (`latitude`, `longitude`, weather variables).
    - Send request to Open-Meteo API using `httpx`.
    - Parse response to extract:
        - `temperature_2m`
        - `relative_humidity_2m`
        - `wind_speed_10m`
        - `uv_index`
    - Pass temperature and humidity to `calculate_heat_index` to compute:
        - **Heat Index (HI)**
        - **Risk Level**
    - Create and save a new `HeatLog` entry with:
        - Barangay ID
        - Weather data
        - Computed HI and risk level
        - Current timestamp (localized to PH_TZ, but saved without timezone info)
4. **Commit & Print Log**  
    Save the new record and log a success message.
5. **Error Handling**  
    If fetching or saving fails, print an error message with the barangay name or ID.

---

## **Notes**

- The function can be scheduled (via **APScheduler**) to run periodically.
- Timestamps are localized to **Asia/Manila** but stored as naive `datetime` (without timezone info) for database consistency.
- Extendable: More weather metrics can be added if required.
