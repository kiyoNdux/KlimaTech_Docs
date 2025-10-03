## **Overview**

The **`CoolSpotMarker` component** represents an **individual Cool Spot on the Leaflet map**.  
It places a marker at the spot’s coordinates and displays a popup with details, reports, and an action button to view more information.

---

## **Dependencies**

- `react-leaflet.Marker` → Renders a marker on the map at given coordinates.
- `react-leaflet.Popup` → Displays an interactive popup when the marker is clicked.

---

## **Code Breakdown**

### Marker & Popup Rendering

```jsx
<Marker position={[spot.lat, spot.lon]}>
  <Popup>
    <strong>{spot.name}</strong>
    <br />
    Type: {spot.type}
    <br />
    {spot.reports && spot.reports.length > 0 && (
      <div>
        <hr />
        <strong>Reports:</strong>
        <ul>
          {spot.reports.map((r, idx) => (
            <li key={idx}>
              {r.note} <br />
              <small>{r.date} {r.time}</small>
            </li>
          ))}
        </ul>
      </div>
    )}
    <button onClick={() => onViewDetails(spot.id)}>
      View Details
    </button>
  </Popup>
</Marker>
```

- **Marker** → Plots the Cool Spot at `[spot.lat, spot.lon]`.
- **Popup** → Displays information when marker is clicked:
    - **Name** → Bold text (`spot.name`).
    - **Type** → Cool Spot category (e.g., shaded area, water source).
    - **Reports** (optional) → If `spot.reports` exists and has entries, lists community feedback with `note`, `date`, and `time`.
    - **Button** → “View Details” button calls `onViewDetails(spot.id)` to notify the parent component.

---

## **Props**

- `spot` _(object)_ → Data for the specific Cool Spot:
    - `id` → Unique identifier.
    - `name` → Display name.
    - `type` → Category of Cool Spot.
    - `lat` & `lon` → Coordinates for placement.
    - `reports` _(optional)_ → Array of community-submitted reports (`note`, `date`, `time`).
- `onViewDetails` _(function)_ → Callback triggered when user clicks **View Details**.

---

## **Key Integrations**

- **Leaflet Map** → Handles marker placement and popup interaction.
- **Parent Component** → Receives the `spot.id` via `onViewDetails` to load more details (e.g., open modal or sidebar).
- **Community Reports** → Displays user-submitted data to give context about the spot.

---

## **Usage**

- Used inside **HeatMap.jsx** to render all spots.
- Flow:
    1. User clicks a Cool Spot marker.
    2. Popup opens showing details.
    3. If reports exist, they’re listed.
    4. User can click **View Details** → triggers parent to show full information.
