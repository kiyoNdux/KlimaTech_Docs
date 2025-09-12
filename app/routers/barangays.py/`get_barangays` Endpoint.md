
```python
@router.get("/barangays", response_model=list[BarangaySummary])
async def get_barangays():
    ...
```

### Description

Retrieves a list of all barangays along with their **current weather conditions** and calculated **heat risk levels**.  
This endpoint fetches temperature and humidity data from the [Open-Meteo API](https://open-meteo.com/) for each barangay and computes the **heat index** and corresponding **risk level**.

---

### External API

- **URL:** `https://api.open-meteo.com/v1/forecast`
- **Parameters sent per barangay:**
    - `latitude` — geographic latitude of the barangay
    - `longitude` — geographic longitude of the barangay
    - `current` — list of weather variables to fetch (temperature and humidity)


---

### Process

1. Loops over the static `barangays_data` list.
2. For each barangay:
    - Sends a request to the **Open-Meteo API** to get current weather data.
    - Extracts:
        - `temperature_2m` (°C)
        - `relative_humidity_2m` (%)
    - Passes these values into `calculate_heat_index` to compute:
        - **Heat Index (HI)**
        - **Risk Level**
3. Appends a dictionary of results for each barangay, including:
    - `id`, `name`, `lat`, `lon`
    - `heat_index` (calculated value)
    - `risk_level` (categorical output from heat index)
    - `updated_at` (UTC timestamp)

---

### Response Model

- **Type:** `list[BarangaySummary]`
- **Fields returned per barangay:**
    - `id` _(int)_ – Unique identifier
    - `name` _(str)_ – Barangay name
    - `lat` _(float)_ – Latitude
    - `lon` _(float)_ – Longitude
    - `heat_index` _(float)_ – Calculated heat index
    - `risk_level` _(str)_ – Heat risk category (e.g., _Low, Moderate, High_)
    - `updated_at` _(datetime)_ – Timestamp of last update

---

### Example Response

```json
[
  {
    "id": 1,
    "name": "Barangay A",
    "lat": 14.5995,
    "lon": 120.9842,
    "heat_index": 35.7,
    "risk_level": "High",
    "updated_at": "2025-09-12T09:35:22Z"
  },
  {
    "id": 2,
    "name": "Barangay B",
    "lat": 10.3157,
    "lon": 123.8854,
    "heat_index": 30.1,
    "risk_level": "Moderate",
    "updated_at": "2025-09-12T09:35:22Z"
  }
]
```
