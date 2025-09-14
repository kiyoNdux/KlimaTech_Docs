## `Barangay`

```python
class Barangay(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    lat: float
    lon: float
    heat_logs: List["HeatLog"] = Relationship(back_populates="barangay")
```

### Description

Represents a **barangay** entity with geographical coordinates.  
Each barangay can have multiple associated `HeatLog` records.

### Fields

- `id` _(Optional[int])_ – Primary key, auto-incremented
- `name` _(str)_ – Name of the barangay
- `lat` _(float)_ – Latitude
- `lon` _(float)_ – Longitude
- `heat_logs` _(List[HeatLog])_ – Relationship to related `HeatLog` entries

---

## `HeatLog`

```python
class HeatLog(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    barangay_id: int = Field(foreign_key="barangay.id")
    temperature_c: float
    humidity: float
    heat_index_c: float
    risk_level: str
    recorded_at: datetime = Field(default_factory=datetime.utcnow)
    barangay: Optional["Barangay"] = Relationship(back_populates="heat_logs")
```

### Description

Stores a **historical record** of heat index calculations for a barangay, including weather conditions and associated risk level.

### Fields

- `id` _(Optional[int])_ – Primary key, auto-incremented
- `barangay_id` _(int)_ – Foreign key referencing `Barangay.id`
- `temperature_c` _(float)_ – Recorded air temperature (°C)
- `humidity` _(float)_ – Recorded relative humidity (%)
- `heat_index_c` _(float)_ – Calculated heat index (°C)
- `risk_level` _(str)_ – Risk category (e.g., _Safe, Caution, Danger_)
- `recorded_at` _(datetime)_ – UTC timestamp when record was created (default: `datetime.utcnow()`)
- `barangay` _(Optional[Barangay])_ – Relationship back to the parent barangay

---

## Relationships

- **One-to-Many:**
    - A `Barangay` → has many `HeatLog` entries
    - A `HeatLog` → belongs to a single `Barangay`

### ERD (Entity-Relationship Diagram)

```
+-------------+         +-----------------+
|  Barangay   |  1 ────>│    HeatLog      |
+-------------+         +-----------------+
| id (PK)     |         | id (PK)         |
| name        |         | barangay_id (FK)|
| lat         |         | temperature_c   |
| lon         |         | humidity        |
| heat_logs[] |         | heat_index_c    |
+-------------+         | risk_level      |
                        | recorded_at     |
                        +-----------------+
```
