## **Overview**

This schema file (`schemas/coolspots.py`) defines the **Pydantic models** for handling **Cool Spot data** and their associated **user reports**.

It is used across the API to:

- Validate input when creating new cool spots.
- Shape the response data when retrieving cool spots and their associated reports.
- Ensure consistent structure for **Cool Spot** and **Report** objects in both request and response cycles.

---

## **Schemas**

### **`ReportRead`**

Represents a **report** submitted by a user about a **Cool Spot**.

**Fields:**

- `user_id: Optional[int]` → ID of the user who submitted the report (if available).
- `note: str` → The actual content of the report (user’s note).
- `date: Optional[str]` → Date of the report (string format, optional).
- `time: Optional[str]` → Time of the report (string format, optional).


### **`CoolSpotRead`**

Represents a **Cool Spot object** when reading data (e.g., via a GET request).

**Fields:**

- `id: int` → Unique identifier of the cool spot.
- `barangay_id: int` → The barangay the cool spot belongs to.
- `name: str` → Name of the cool spot (e.g., plaza, park, shaded area).
- `type: str` → Type/category of the cool spot (e.g., public park, indoor facility).
- `lat: float` → Latitude coordinate of the cool spot.
- `lon: float` → Longitude coordinate of the cool spot.
- `reports: List[ReportRead]` → List of **user-submitted reports** associated with this cool spot.


### **`CoolSpotCreate`**

Represents the **input schema** for creating a new Cool Spot.

**Fields:**

- `barangay_id: int` → The barangay the cool spot is being added to.
- `name: str` → Name of the cool spot.
- `type: str` → Type/category of the cool spot.
- `lat: float` → Latitude coordinate.
- `lon: float` → Longitude coordinate.

---

## **Usage Notes**

- Use **`CoolSpotCreate`** when creating a new Cool Spot via POST requests.
- Use **`CoolSpotRead`** when returning Cool Spot details along with user reports.
- The **`reports`** field in `CoolSpotRead` automatically nests `ReportRead` models, ensuring structured responses.
- Dates and times in reports are stored as optional strings and can be formatted before display if needed.
