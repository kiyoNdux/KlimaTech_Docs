## **Overview**

This script is a **data population utility** designed to insert predefined barangay records into the database.  
It connects to the database using `get_session()`, clears existing records in the `barangay` table, and then inserts a set of predefined barangays.

This script is typically used for:

- Initial seeding of the database with test barangays.
- Resetting the barangay table to a clean state during development.

---

## **Dependencies**

- **`app.models.Barangay`** – SQLModel ORM model for the `barangay` table.
- **`app.db.get_session`** – database session generator function.
- **`sqlalchemy.text`** – used to execute raw SQL (`DELETE FROM barangay`).

---

## **Data: `barangays_data`**

A predefined list of dictionaries containing barangay records.  
Each record includes:

- `id` _(int)_ – Unique identifier for the barangay.
- `name` _(str)_ – Full name of the barangay.
- `lat` _(float)_ – Latitude coordinate.
- `lon` _(float)_ – Longitude coordinate.

Example:

```python
barangays_data = [
    {"id": 1, "name": "Barangay Ibabang Dupay, Lucena, Quezon", "lat": 13.9405, "lon": 121.617},
    {"id": 2, "name": "Bgry. Ikirin, Pagbilao, Quezon", "lat": 13.9968, "lon": 121.7309},
]
```

---

## **Functions**

### **`main()`**

The entry point of the script.  
Steps performed:

1. **Create session** – Retrieves a database session from `get_session()`.
2. **Clear old records** – Executes `DELETE FROM barangay` to remove all existing records.
3. **Insert barangays** – Iterates over `barangays_data` and creates `Barangay` objects.
4. **Commit changes** – Saves the new records into the database.
5. **Confirmation** – Prints `"Barangays added!"` after successful execution.

---

## **Execution**

The script can be run directly from the terminal:

```bash
python scripts/add_barangays.py
```

Expected output:

```
Barangays added!
```

---

## **Notes**

- Running this script will **delete all existing records** in the `barangay` table before inserting the predefined list.
- Extend `barangays_data` as needed to add more barangays.
- Designed for **development and testing**, not production seeding.
