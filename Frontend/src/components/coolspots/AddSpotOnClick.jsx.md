## **Overview**

The **`AddSpotOnClick` component** enables users to **add new Cool Spots by clicking on the map**.  
It listens for map click events using Leaflet and, when in add mode, creates a new spot object and passes it back to the parent for handling.

---

## **Dependencies**

- `react-leaflet.useMapEvents` → Hook to register Leaflet map event listeners.

---

## **Code Breakdown**

### Map Event Handling

```jsx
useMapEvents({
  click(e) {
    if (addMode) {
      const newSpot = {
        barangay_id: 1,
        name: "Cool Spot",
        type: newSpotType,
        lat: e.latlng.lat,
        lon: e.latlng.lng
      };
      onAddSpot(newSpot);
    }
  }
});
```

- Registers a **click event listener** on the map.
- If `addMode` is `true`:
    - Creates a **new spot object** with:
        - `barangay_id` → Hardcoded to `1` (placeholder for testing).
        - `name` → Default label `"Cool Spot"`.
        - `type` → Uses the `newSpotType` prop.
        - `lat` & `lon` → Coordinates from the map click.
    - Calls `onAddSpot(newSpot)` → Sends the new spot data to the parent component for persistence.

---

### **Return Value**

```jsx
return null;
```

- The component **renders nothing** on screen.
- It exists solely to listen for map events and trigger logic.

---

## **Props**

- `addMode` _(boolean)_ → Toggles whether clicking the map should add a spot.
- `newSpotType` _(string)_ → Type of spot being added (e.g., shade, water source).
- `onAddSpot` _(function)_ → Callback to handle the newly created spot object.

---

## **Key Integrations**

- **Leaflet Map** → Provides the map and click coordinates.
- **Parent Component** → Receives the new spot data via `onAddSpot`.
- **Add Mode Toggle** → Ensures spots are only added intentionally.

---

## **Usage**

- Import into a **HeatMap.jsx**
- Used in combination with **Leaflet’s `<MapContainer>`**.
- Flow:
    1. User toggles **Add Mode ON**.
    2. Clicks on the map at the desired location.
    3. `AddSpotOnClick` creates the new spot and passes it up.
    4. Parent component saves/displays the spot.
