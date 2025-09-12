
```python
@router.get("/{barangay_id}", response_model=BarangayDetail)
async def get_barangay(barangay_id: int):
    ...
```

### Description

Retrieves **detailed weather and heat risk information** for a single barangay, identified by its `barangay_id`.  
The endpoint queries the [Open-Meteo API](https://open-meteo.com/) to fetch the **current temperature and humidity**, calculates the **heat index** and **risk level**, and returns a structured response including safety guidance.

---

### Parameters

- `barangay_id` _(int)_ – The unique identifier of the barangay to fetch.

If no barangay is found with the given `barangay_id`, the endpoint returns:

```json
{ "error": "Barangay not found" }
```

---

### External API

- **URL:** `https://api.open-meteo.com/v1/forecast`
- **Parameters sent:**
    - `latitude` — barangay latitude
    - `longitude` — barangay longitude
    - `current` — `["temperature_2m", "relative_humidity_2m"]`


---

### Process

1. Search for the barangay in `barangays_data` using the given `barangay_id`.
2. If found:
    - Request current weather data from **Open-Meteo API**.
    - Extract:
        - `temperature_2m` (°C)
        - `relative_humidity_2m` (%)
    - Compute **heat index** and **risk level** using `calculate_heat_index`.
3. Construct and return a detailed response containing:
    - **Barangay details** (id, name, coordinates)
    - **Current weather data** (temperature, humidity, heat index, risk level, timestamp)
    - **Daily briefing** with safety tips (static placeholder for now)
    - **Forecast data** (currently an empty list, reserved for future implementation)


---

### Response Model

- **Type:** `BarangayDetail`
- **Fields returned:**
    - `id` _(int)_ – Unique identifier
    - `name` _(str)_ – Barangay name
    - `lat` _(float)_ – Latitude
    - `lon` _(float)_ – Longitude
    - `current` _(dict)_ – Current weather and risk info
        - `temperature` _(float)_ – Current temperature (°C)
        - `humidity` _(float)_ – Current relative humidity (%)
        - `heat_index` _(float)_ – Calculated heat index
        - `risk_level` _(str)_ – Risk category (_Low, Moderate, High, etc._)
        - `updated_at` _(datetime)_ – UTC timestamp
    - `daily_briefing` _(dict)_ – Safety advice (placeholder for now)
        - `safe_hours` _(str)_ – Suggested outdoor work hours
        - `avoid_hours` _(str)_ – Hours to avoid due to heat
        - `advice` _(str)_ – General health and safety advice
    - `forecast(optional)` _(list)_ – Placeholder for future forecast data

---

### Example Response

```json
{
  "id": 1,
  "name": "Barangay A",
  "lat": 14.5995,
  "lon": 120.9842,
  "current": {
    "temperature": 34.2,
    "humidity": 70,
    "heat_index": 41.0,
    "risk_level": "Very High",
    "updated_at": "2025-09-12T09:40:12Z"
  },
  "daily_briefing": {
    "safe_hours": "Before 10AM, After 4PM",
    "avoid_hours": "11AM–3PM",
    "advice": "Hydrate frequently and avoid prolonged outdoor work."
  },
  "forecast": []
}
```
