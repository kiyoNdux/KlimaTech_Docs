## **Overview**

These schemas are used in API endpoints to structure both **summary-level** and **detailed** responses for barangays.

---

## **Classes**

### **`BarangayBase`**

Base schema representing the common attributes of a barangay.

**Fields:**

- `id` _(int)_ → Unique identifier for the barangay.
- `name` _(str)_ → Name of the barangay.
- `lat` _(float)_ → Latitude coordinate.
- `lon` _(float)_ → Longitude coordinate.

**Usage:**

- Serves as the base class for other schemas (`BarangaySummary`, `BarangayDetail`).

---

### **`BarangaySummary`** _(extends `BarangayBase`)_

Schema for returning summarized information about a barangay, including its latest heat risk status.

**Additional Fields:**

- `heat_index` _(float)_ → Most recent computed heat index (in Celsius).
- `risk_level` _(str)_ → Heat risk category (e.g., _Safe, Caution, Danger_).
- `updated_at` _(datetime)_ → Timestamp when the data was last updated.

**Usage:**

- Returned by the `/barangays/all` endpoint to list multiple barangays with their heat risk summaries.

---

### **`BarangayDetail`** _(extends `BarangayBase`)_

Schema for returning detailed information about a specific barangay.

**Additional Fields:**

- `current` _(dict)_ → Current conditions (temperature, humidity, heat index, risk level, last update).
- `daily_briefing` _(dict)_ → Advice and guidelines for safe/unsafe hours.
- `forecast` _(Optional[list])_ → Placeholder for future forecast data (currently empty).

**Usage:**

- Returned by the `/barangays/{barangay_id}` endpoint to provide a full barangay report.

---

## **How it fits in the project**

- Used in `routers/barangays.py` to serialize and validate API responses.
- Ensures consistency between what the backend provides and what the client expects.
- Supports both lightweight summaries (for listings) and detailed views (for individual barangays).
