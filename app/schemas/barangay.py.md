## `BarangayBase`

```python
class BarangayBase(BaseModel):
    id: int
    name: str
    lat: float
    lon: float
```

### Description

Base schema representing the core information of a barangay.  
Used as the foundation for summary and detailed responses.

### Fields

- `id` _(int)_ – Unique identifier of the barangay
- `name` _(str)_ – Barangay name
- `lat` _(float)_ – Latitude
- `lon` _(float)_ – Longitude

---

## `BarangaySummary`

```python
class BarangaySummary(BarangayBase):
    heat_index: float
    risk_level: str
    updated_at: datetime
```

### Description

Extended schema for **summary views** of barangays, used in listing endpoints.  
Includes heat-related data alongside base attributes.

### Fields

- **Inherited from `BarangayBase`**
    - `id`, `name`, `lat`, `lon`
- **Additional**
    - `heat_index` _(float)_ – Calculated heat index value
    - `risk_level` _(str)_ – Heat risk category (e.g., _Low, Moderate, High_)
    - `updated_at` _(datetime)_ – Timestamp of last weather data update

---

## `BarangayDetail`

```python
class BarangayDetail(BarangayBase): 
    current: dict
    daily_briefing: dict
    forecast: Optional[list] = None
```

### Description

Detailed schema for **single barangay responses**, providing current weather, safety guidance, and (future) forecast data.

### Fields

- **Inherited from `BarangayBase`**
    - `id`, `name`, `lat`, `lon`
- **Additional**
    - `current` _(dict)_ – Current weather and heat risk data
        - Expected keys: `temperature`, `humidity`, `heat_index`, `risk_level`, `updated_at`
    - `daily_briefing` _(dict)_ – Health and safety recommendations
        - Example keys: `safe_hours`, `avoid_hours`, `advice`
    - `forecast` _(Optional[list])_ – Future weather/heat index forecasts (currently unused; defaults to `None`)
