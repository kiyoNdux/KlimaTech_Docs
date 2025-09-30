## **Overview**

This file provides an API endpoint to retrieve **hourly heat risk forecasts** for a given barangay using the **Open-Meteo API**.

It fetches tomorrow’s forecasted weather data (temperature, humidity, wind speed, UV index) for the selected barangay, computes the **heat index** for each hour, and categorizes the associated **heat risk level**.

---

## **Dependencies**

- **`httpx`** – For making asynchronous HTTP requests to the Open-Meteo API.
- **`fastapi`** – API routing, dependency injection, and error handling.
- **`sqlmodel.Session`** – For database access to fetch barangay information.
- **`datetime`** – For timezone-aware time handling and filtering tomorrow’s forecast data.
- **`app.models.Barangay`** – Barangay model for retrieving barangay details from the database.
- **`app.db.get_session`** – Database session dependency.
- **`app.utils.heat_index.calculate_heat_index`** – Function to compute heat index and risk level.

---

## **Router Setup**

```python
router = APIRouter(prefix="/barangays", tags=["Barangays"])
```

- **Prefix**: `/barangays`
- **Tag**: `"Barangays"` – Groups endpoints related to barangay operations.

---

## **Constants**

- **`PH_TZ`**: Timezone set to **UTC+8** (Philippines).
- **`OPEN_METEO_URL`**: Base URL for the Open-Meteo forecast API.

---

## **Endpoint: Get Barangay Forecast**

### **Path**

```
GET /barangays/{barangay_id}/forecast
```

### **Description**

Retrieves the **hourly forecast for tomorrow** for the given barangay, including:

- Temperature (°C)
- Relative Humidity (%)
- Wind Speed (m/s or km/h depending on API)
- UV Index
- Computed Heat Index (°C)
- Heat Risk Level (Safe, Caution, Extreme Caution, Danger, Extreme Danger)

---

### **Parameters**

- **`barangay_id`** (_int_): ID of the barangay in the database.

### **Response**

```json
{
  "barangay_id": 1,
  "barangay": "Barangay Ibabang Dupay",
  "locality": "Lucena",
  "province": "Quezon",
  "lat": 13.9405,
  "lon": 121.617,
  "forecast": [
    {
      "time": "2025-09-29T08:00:00",
      "temperature": 30.5,
      "humidity": 70,
      "wind_speed": 3.2,
      "uv_index": 8,
      "heat_index": 35.1,
      "risk_level": "Extreme Caution"
    },
    ...
  ]
}
```

---

### **Error Handling**

- **404 Not Found** – If the `barangay_id` does not exist in the database:

```json
{
  "detail": "Barangay not found"
}
```

---

## **Internal Logic**

1. **Fetch barangay info** from the database using `Session.get()`.
2. **Request forecast data** from Open-Meteo API with hourly parameters (`temperature_2m`, `relative_humidity_2m`, `wind_speed_10m`, `uv_index`).
3. **Extract hourly data** arrays (`time`, `temperature`, `humidity`, `wind_speed`, `uv_index`).
4. **Filter only tomorrow’s forecast** using timezone-aware date handling.
5. For each forecasted hour:
    - Compute **heat index** using `calculate_heat_index`.
    - Categorize risk level.
    - Append structured forecast data to the response.
6. Return a JSON response containing barangay metadata and forecast details.

---

## **Notes**

- Timezone is explicitly set to **Asia/Manila** to ensure forecasts align with local time.
- The endpoint currently returns **only tomorrow’s forecast** (next calendar day).
- Extending support for multiple days or customizable ranges is possible by adjusting the date filtering logic.
- The barangay model must have `barangay`, `locality`, and `province` fields defined in `models.py`.
