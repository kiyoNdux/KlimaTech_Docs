## **Overview**

This router provides **hourly heat risk forecasts** for a specific barangay using the **Open-Meteo API**.

- Retrieves hourly **temperature, humidity, wind speed, and UV index**.
- Computes the **heat index** and **risk level** for each hour.
- Restricts forecasts to the **current day (Asia/Manila timezone)**.
- Returns structured forecast data along with barangay details.

---

## **Dependencies**

- `httpx.AsyncClient` → Makes async API requests to Open-Meteo.
- `sqlmodel.Session` → Retrieves barangay data from the database.
- `app.utils.heat_index.calculate_heat_index` → Calculates heat index and risk levels.
- `datetime`, `timezone` → Handles timezone-aware filtering (Philippines time).

---

## **Constants**

- `PH_TZ` → Defines the Philippines timezone (`UTC+8`).
- `OPEN_METEO_URL` → Base URL for the Open-Meteo forecast API.

---

## **Endpoints**

### **GET `/barangays/{barangay_id}/forecast`**

**Handler:** `get_barangay_forecast`

- Retrieves the **hourly forecast** for a specific barangay.
- If the barangay does not exist, returns **404 Not Found**.

**Request Parameters**

- `barangay_id` (int) → ID of the barangay.

**Steps performed:**

1. Look up the barangay in the database.
2. Call the Open-Meteo API with the barangay’s latitude and longitude.
3. Extract hourly data for:
    - Temperature (`temperature_2m`)
    - Relative humidity (`relative_humidity_2m`)
    - Wind speed (`wind_speed_10m`)
    - UV index (`uv_index`)
4. Filter results for **today only** (Asia/Manila timezone).
5. Compute **heat index** and **risk level** for each hour.
6. Return structured forecast data.

**Response Example:**

```json
{
  "barangay_id": 1,
  "barangay": "Barangay 123",
  "locality": "Quezon City",
  "province": "Metro Manila",
  "lat": 14.657,
  "lon": 121.028,
  "forecast": [
    {
      "time": "2025-10-03T08:00:00+08:00",
      "temperature": 32.1,
      "humidity": 70,
      "wind_speed": 2.4,
      "uv_index": 5.2,
      "heat_index": 38.5,
      "risk_level": "Caution"
    },
    ...
  ]
}
```

---

## **Key Integrations**

- **Open-Meteo API** → Supplies real-time hourly weather data.
- **Barangay model (SQLModel)** → Provides location coordinates and barangay info.
- **Heat Index Utility** → Calculates heat index and maps it to a risk category.
