## **Overview**

The **`HeatMap` page** provides an **interactive map interface** for users to explore, add, and report **Cool Spots**.  
It integrates user geolocation, backend APIs, and Leaflet maps to support:

- **Viewing Cool Spots** from the backend.
- **Adding New Cool Spots** by selecting a type and clicking on the map.
- **Reporting Issues** for existing Cool Spots via a modal.
- **Showing User Location** with a custom marker.

---

## **Dependencies**

- **React & Hooks**
    - `useState`, `useEffect` → Local state management and side effects.
- **React Leaflet**
    - `MapContainer`, `TileLayer`, `Marker`, `Popup` → Map rendering and markers.
    - `useMapEvents` (via `AddSpotOnClick`) → Handles map click events.
- **Custom Components**
    - `CoolSpotMarker` → Renders markers for existing cool spots.
    - `CoolSpotModal` → Displays detailed info about a selected spot and allows reporting.
    - `AddSpotOnClick` → Handles new spot creation on map clicks.
- **Leaflet**
    - `L.Icon` → Defines a custom icon for the user’s current location marker.

---

## **Code Breakdown**

### **State Management**

```jsx
const [coolSpots, setCoolSpots] = useState([]);
const [addMode, setAddMode] = useState(false);
const [selectedSpot, setSelectedSpot] = useState(null);
const [showModal, setShowModal] = useState(false);
const [reportNote, setReportNote] = useState("");
const [reportSubmitting, setReportSubmitting] = useState(false);
const [newSpotType, setNewSpotType] = useState("Shaded Area");
const [userLocation, setUserLocation] = useState(null);
```

- `coolSpots` → Stores all cool spots fetched from backend.
- `addMode` → Enables map click mode to add a new spot.
- `selectedSpot` & `showModal` → Manage details modal state.
- `reportNote` & `reportSubmitting` → Manage user-submitted reports.
- `newSpotType` → Dropdown selection for spot type.
- `userLocation` → Stores geolocation coordinates of the user.


### **Data Fetching**

#### Fetch Cool Spots (on mount)

```jsx
useEffect(() => {
  fetch("/api/coolspots/all")
    .then(res => res.json())
    .then(data => setCoolSpots(data))
    .catch(err => console.error("Failed to fetch cool spots:", err));
}, []);
```

- Loads all cool spots from backend on component mount.

#### Fetch User Location (on mount)

```jsx
useEffect(() => {
  if ("geolocation" in navigator) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        setUserLocation({
          lat: position.coords.latitude,
          lon: position.coords.longitude
        });
      },
      (error) => console.warn("Geolocation error:", error)
    );
  }
}, []);
```

- Uses **browser Geolocation API** to set the user’s current location.


### **Handlers**

#### Add New Spot

```jsx
function handleAddSpot(newSpot) {
  fetch("/api/coolspots/add", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(newSpot)
  })
    .then(res => res.json())
    .then(spot => setCoolSpots(prev => [...prev, spot]))
    .catch(err => alert("Failed to add cool spot"));
  setAddMode(false);
}
```

- Sends `POST /api/coolspots/add` to create a new spot.
- Updates local state with the new spot.

#### View Spot Details

```jsx
function handleViewDetails(id) {
  fetch(`/api/coolspots/${id}`)
    .then(res => res.json())
    .then(data => {
      setSelectedSpot(data);
      setShowModal(true);
    })
    .catch(err => alert("Failed to fetch details"));
}
```

- Retrieves details from `GET /api/coolspots/:id`.
- Opens modal with full spot information.

#### Submit Report (inside modal)

```jsx
fetch(`/api/coolspots/${selectedSpot.id}/report`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ user_id: 0, note: reportNote })
})
```

- Posts report data for a selected spot.
- Refreshes spot details after submission.

---

## **Key Integrations**

- **Backend Endpoints**
    - `GET /api/coolspots/all` → Fetch all cool spots.
    - `POST /api/coolspots/add` → Create new cool spot.
    - `GET /api/coolspots/:id` → Get spot details.
    - `POST /api/coolspots/:id/report` → Submit a report.
- **Geolocation API** → Retrieves current user location.
- **Leaflet/React-Leaflet** → Provides mapping and visualization.
- **Custom Components** → Modular UI for markers, modal, and map interactions.

---

## **Usage**

- Accessed via the **Heat Map route** (e.g., `/heatmap`).
- End-users can:
    - View all known **Cool Spots**.
    - Add new ones (tagged with type like shaded area, open area, water source).
    - Report conditions or issues at specific locations.
    - See their **current location** for context.
