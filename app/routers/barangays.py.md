## **Overview**

This router handles all **Barangay-related API endpoints**, including:

- Recording new **HeatLog** entries from live weather data.
- Fetching summaries of all barangays with their latest heat index and risk levels.
- Fetching detailed information about a single barangay, including current conditions, risk status, and daily advice.

It integrates with the **Open-Meteo API** for real-time weather data and uses the local database (`Barangay`, `HeatLog`) for persistence and caching.

---

## **Helper Function/s**

### **`fetch_and_save_heatlog(barangay: Barangay, session: Session) -> HeatLog`**

Fetches the latest temperature and humidity for a given barangay, calculates the **heat index**, and stores the result as a `HeatLog` entry in the database.

**Steps performed:**

1. Sends a request to the Open-Meteo API with the barangay’s coordinates.
2. Extracts **temperature** and **humidity** values.
3. Calculates **heat index** and **risk level** using `calculate_heat_index`.
4. Saves a new `HeatLog` record in the database.
5. Returns the saved `HeatLog` object.

---

## **Endpoints**

### **POST `/barangays/{barangay_id}/heatlog`**

**Handler:** `save_heatlog_endpoint`

- Saves a new `HeatLog` entry for the specified barangay.
- If the barangay does not exist, raises `404 Not Found`.
- Returns the newly created `HeatLog` record.


### **GET `/barangays/all`**

**Handler:** `get_barangays`

- Retrieves a list of all barangays with their latest heat index and risk level.
- Uses the most recent `HeatLog` if it is less than **1 hour old**; otherwise, generates a new one from Open-Meteo.
- Returns a list of `BarangaySummary` objects.


### **GET `/barangays/{barangay_id}`**

**Handler:** `get_barangay`

- Retrieves detailed information about a specific barangay.
- Uses the most recent `HeatLog` if it is less than **1 hour old**; otherwise, generates a new one.
- Returns a `BarangayDetail` object, including:
    - Current weather (temperature, humidity, heat index, risk level).
    - A sample **daily briefing** with safe/avoid hours and advice.
    - Placeholder for **forecast data** (future implementation).

---

## **Key Integrations**

- **Open-Meteo API** → Provides real-time weather data.
- **SQLModel ORM** → Handles persistence of `Barangay` and `HeatLog` data.
- **Caching mechanism** → Prevents redundant API calls by reusing `HeatLog` data if it’s less than 1 hour old.
