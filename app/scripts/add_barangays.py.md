### `main()`

Seeds the database with predefined barangay data.

**Steps performed:**

1. Opens a database session using `get_session()`.
2. Iterates over the predefined `barangays_data` list.
3. Creates a new `Barangay` object for each entry.
4. Adds the object to the session.
5. Commits the transaction to persist the data.
6. Prints confirmation (`"Barangays added!"`).

---

### `barangays_data`

A list of dictionaries containing sample barangay information.  
Each dictionary includes:

- `id`: Unique integer ID.
- `name`: Barangay name (string).
- `lat`: Latitude (float).
- `lon`: Longitude (float).

Example:

```python
{"id": 1, "name": "Barangay Dalahican, Lucena", "lat": 13.9317, "lon": 121.6233}
```

---

### **Usage in Project**

- Run this script manually to populate the `barangay` table with initial data.
- Helps bootstrap the system with real geographic data for testing or demo purposes.
- Not intended for production seeding without validation (may cause duplicate key errors if run multiple times).
