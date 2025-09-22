## **Overview**

This file defines the **database models** for the Heat Risk API using **SQLModel**.  
It contains two main entities:

- **`Barangay`** – Represents a geographical area (barangay) with coordinates.
- **`HeatLog`** – Represents recorded heat index data linked to a barangay.

The models use **relationships** for linking `Barangay` and its `HeatLogs`.

---

## **Dependencies**

- **`sqlmodel`**
    - `SQLModel` – Base class for defining models.
    - `Field` – Used to declare fields with properties (e.g., primary keys, defaults).
    - `Relationship` – Defines ORM relationships between models.
- **`typing`** – Provides type hints (`Optional`, `List`).
- **`datetime`** – Used to store timestamps for heat log records.

---

## **Models**

### **1. Barangay**

```python
class Barangay(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    name: str
    lat: float
    lon: float
    heat_logs: List["HeatLog"] = Relationship(back_populates="barangay")
```

**Fields:**

- `id`: Primary key (auto-incremented).
- `name`: Name of the barangay.
- `lat`: Latitude coordinate.
- `lon`: Longitude coordinate.
- `heat_logs`: One-to-many relationship with `HeatLog`.

**Purpose:**  

Represents a barangay (local administrative division), storing its location and related weather data logs.

---

### **2. HeatLog**

```python
class HeatLog(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    barangay_id: int = Field(foreign_key="barangay.id")
    temperature_c: float
    humidity: float
    heat_index_c: float
    uv_index: Optional[float] = None
    risk_level: str
    recorded_at: datetime = Field(default_factory=datetime.utcnow)
    barangay: Optional["Barangay"] = Relationship(back_populates="heat_logs")
```

**Fields:**

- `id`: Primary key (auto-incremented).
- `barangay_id`: Foreign key linking to a `Barangay`.
- `temperature_c`: Recorded temperature (°C).
- `humidity`: Recorded humidity (%).
- `heat_index_c`: Calculated heat index (°C).
- `uv_index`: Recorded UV Index.
- `risk_level`: Categorical risk level (e.g., _Safe_, _Caution_, _Danger_).
- `recorded_at`: Timestamp when the log was recorded (defaults to **current UTC**).
- `barangay`: Relationship back to the parent `Barangay`.

**Purpose:**  

Stores historical or real-time heat index data associated with a barangay.

---

## **Entity Relationship**

- **One Barangay → Many HeatLogs**
- Each `HeatLog` belongs to exactly **one** `Barangay`.
- Enables efficient queries such as:
    - Getting all logs for a specific barangay.
    - Fetching the most recent log to determine current heat risk.

---

## **Notes**

- Using `default_factory=datetime.utcnow` ensures logs always have a timestamp when inserted.
- The `Relationship` fields (`barangay` and `heat_logs`) allow **ORM-style navigation** between related records.
- This schema is **normalized**: logs are stored separately to avoid redundant data in the `Barangay` table.
