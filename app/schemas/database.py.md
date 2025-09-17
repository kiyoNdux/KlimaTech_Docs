## **Overview**

These schemas are used in API request and response validation to ensure consistent data exchange between the backend and clients.

---

## **Classes**

### **`HeatLogCreate`**

Schema for creating a new `HeatLog` entry.

**Fields:**

- `barangay_id` _(int)_ → ID of the barangay this log belongs to.
- `temperature_c` _(float)_ → Temperature in Celsius.
- `humidity` _(float)_ → Relative humidity in percentage.
- `heat_index_c` _(float)_ → Computed heat index in Celsius.
- `risk_level` _(str)_ → Heat risk category (e.g., _Safe, Caution, Danger_).

**Usage:**

- Input schema for POST requests (`/barangays/{barangay_id}/heatlog`).

---

### **`HeatLogRead`**

Schema for reading `HeatLog` entries.

**Fields:**

- `id` _(int)_ → Unique identifier of the HeatLog record.
- `barangay_id` _(int)_ → ID of the barangay this log belongs to.
- `temperature_c` _(float)_ → Recorded temperature in Celsius.
- `humidity` _(float)_ → Recorded relative humidity in percentage.
- `heat_index_c` _(float)_ → Calculated heat index in Celsius.
- `risk_level` _(str)_ → Risk category based on heat index.
- `recorded_at` _(datetime)_ → Timestamp when the log was recorded.

**Usage:**

- Response schema for returning saved heat log records.
- Ensures clients receive full heat log details including timestamp.

---

## **How it fits in the project**

- These schemas work alongside the `HeatLog` SQLModel in `models.py`.
- `HeatLogCreate` is used for **data input** when creating logs.
- `HeatLogRead` is used for **data output** in API responses.
