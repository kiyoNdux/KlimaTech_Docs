## **Overview**

The **`Dashboard` page** serves as the **main user interface for heat risk monitoring**.  
It fetches weather and heat index data from the backend and displays it using multiple interactive widgets:

- **HeatGauge** → Visual gauge for heat index.
- **LocationWidget** → Displays barangay, locality, and province.
- **AdvisoryWidget** → Shows current risk level and health advice.
- **BriefingsWidget** → Summarizes key weather metrics (temperature, humidity, wind speed, UV index).
- **HeatClockWidget** → Displays heat-related time-based information (safe/avoid hours).

---

## **Dependencies**

- `react.useState` & `react.useEffect` → Manages local state and data fetching lifecycle.
- `../components/HeatGauge` → Renders gauge for heat index.
- `../components/LocationWidget` → Displays barangay and location info.
- `../components/AdvisoryWidget` → Provides contextual advice based on heat risk.
- `../components/BriefingsWidget` → Displays weather details.
- `../components/HeatClockWidget` → Shows safe/avoid hours in a clock-like format.
- `../scripts/api.fetchWeatherData` → Async API call to fetch weather/heat data from the backend.

---

## **Code Breakdown**

### State Management

```jsx
const [weatherData, setWeatherData] = useState(null);
const [error, setError] = useState(null);
```

- `weatherData` → Holds fetched weather/heat data.
- `error` → Tracks API fetch errors.

### Data Fetching

```jsx
useEffect(() => {
    async function getData() {
        try {
            const data = await fetchWeatherData(1); // barangayId = 1 for testing
            setWeatherData(data);
        } catch (err) {
            console.error(err);
            setError(err);
        }
    }

    getData();
}, []);
```

- Runs once on component mount (`[]`).
- Fetches weather/heat data for `barangayId = 1` (currently hardcoded for testing).
- Sets `weatherData` on success or `error` on failure.

---

### **Loading & Error States**

```jsx
if (error) return <div>Error loading data</div>;
if (!weatherData) return <div>Loading...</div>;
```

- Displays **fallback UI** while loading or if an error occurs.

---

### **Data Destructuring**

```jsx
const {
    id,
    barangay,
    locality,
    province,
    lat,
    lon,
    current: { temperature, humidity, wind_speed, uv_index, heat_index, risk_level, updated_at },
    daily_briefing: { safe_hours, avoid_hours, advice },
    forecast
} = weatherData;
```

- Unpacks the JSON response from the backend into local variables.
- `current` → Real-time weather data.
- `daily_briefing` → Safe/avoid hours and health advice.
- `forecast` → Hourly/daily forecast data (currently unused in this component).

---

### **UI Composition**

- **HeatGauge** → Displays heat index as a visual gauge.
- **LocationWidget** → Shows geographic context (barangay/locality/province).
- **AdvisoryWidget** → Provides health/safety guidance.
- **BriefingsWidget** → Shows detailed metrics.
- **HeatClockWidget** → Highlights time-based safety advice.

---

## **Key Integrations**

- **Backend API** → Provides weather + heat index data via `fetchWeatherData`.
- **Custom Widgets** → Modular components for visualization (`HeatGauge`, `HeatClockWidget`, etc.).
- **React Hooks** → Used for lifecycle (`useEffect`) and state (`useState`).

---

## **Usage**

- This page is displayed when navigating to the **Dashboard route** (e.g., `/dashboard`).
- Acts as the **central hub** for end-users to:
    - See current heat risk.
    - Get location-based context.
    - Read advice on safe/avoid hours.
    - Access weather condition summaries.
