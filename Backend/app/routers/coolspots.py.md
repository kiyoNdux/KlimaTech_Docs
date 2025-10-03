## **Overview**

This router manages all **CoolSpot-related API endpoints**, including:

- Retrieving all registered **CoolSpots**.
- Adding new **CoolSpots** to the database.
- Submitting **reports** tied to a specific CoolSpot.
- Fetching detailed information about a specific CoolSpot, including all associated reports.

It leverages the database models (`CoolSpot`, `Report`) and schemas (`CoolSpotRead`, `CoolSpotCreate`, `ReportRead`) for structured input/output validation.

---

## **Endpoints**

### **GET `/coolspots/all`**

**Handler:** `get_all_coolspots`

- Retrieves a list of all **CoolSpots** in the database.
- Returns structured data as a list of `CoolSpotRead` objects.

**Response Example:**

```json
[
  {
    "id": 1,
    "barangay_id": 2,
    "name": "Community Center",
    "type": "Indoor Facility",
    "lat": 14.5995,
    "lon": 120.9842
  }
]
```


### **POST `/coolspots/add`**

**Handler:** `create_coolspot`

- Adds a new **CoolSpot** record.
- Accepts data in the form of a `CoolSpotCreate` schema.
- Saves the record and returns the newly created `CoolSpotRead`.

**Request Example:**

```json
{
  "barangay_id": 3,
  "name": "Plaza Park",
  "type": "Park",
  "lat": 14.61,
  "lon": 120.98
}
```

**Response Example:**

```json
{
  "id": 2,
  "barangay_id": 3,
  "name": "Plaza Park",
  "type": "Park",
  "lat": 14.61,
  "lon": 120.98
}
```

---

### **POST `/coolspots/{coolspot_id}/report`**

**Handler:** `add_report`

- Submits a **Report** associated with a specific `coolspot_id`.
- Accepts a `ReportRead` object as input.
- Saves the report and returns a confirmation message with the new report ID.

**Request Example:**

```json
{
  "description": "Well-shaded and comfortable during afternoons",
  "reported_at": "2025-10-01T09:00:00"
}
```

**Response Example:**

```json
{
  "message": "Report added successfully",
  "report_id": 5
}
```

---

### **GET `/coolspots/{coolspot_id}`**

**Handler:** `get_coolspot`

- Retrieves a specific **CoolSpot** by its ID.
- If found, returns full details of the CoolSpot, including all associated reports.
- If not found, returns an error message.

**Response Example:**

```json
{
  "id": 1,
  "barangay_id": 2,
  "name": "Community Center",
  "type": "Indoor Facility",
  "lat": 14.5995,
  "lon": 120.9842,
  "reports": [
    {
      "id": 1,
      "coolspot_id": 1,
      "description": "Crowded in the afternoons",
      "reported_at": "2025-10-01T08:30:00"
    }
  ]
}
```

---

## **Key Integrations**

- **SQLModel ORM** → Manages persistence of `CoolSpot` and `Report` data.
- **Schemas (`CoolSpotRead`, `CoolSpotCreate`, `ReportRead`)** → Ensure consistent input/output validation.
- **Dependency Injection (`get_session`)** → Provides database session handling per request.
