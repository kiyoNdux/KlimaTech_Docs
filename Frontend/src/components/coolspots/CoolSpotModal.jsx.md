## **Overview**

The **`CoolSpotModal` component** provides a **detailed view of a selected Cool Spot** in a modal window.  
It displays location details, a list of community reports, and allows users to submit new reports.

---

## **Dependencies**

- `React` → Base library for rendering the modal UI.

---

## **Code Breakdown**

### Conditional Rendering

```jsx
if (!spot) return null;
```

- Prevents rendering if no `spot` is selected.
- Ensures the modal only appears when valid data exists.


### Spot Details

```jsx
<h2>{spot.name}</h2>
<p>Type: {spot.type}</p>
<p>Barangay ID: {spot.barangay_id}</p>
<p>Latitude: {spot.lat}</p>
<p>Longitude: {spot.lon}</p>
```

- Displays **name, type, barangay ID, and coordinates** of the selected Cool Spot.


### Reports Section

```jsx
<h3>Reports:</h3>
<ul>
  {spot.reports.map((r, idx) => (
    <li key={idx}>
      {r.note} <br />
      <small>{r.date} {r.time}</small>
    </li>
  ))}
</ul>
```

- Loops through `spot.reports` to display community feedback.
- Each report shows:
    - **Note** (main text).
    - **Date & time** (as metadata).


### Report Submission Form

```jsx
<form onSubmit={onSubmitReport}>
  <input
    type="text"
    value={reportNote}
    onChange={e => setReportNote(e.target.value)}
    placeholder="Add a report..."
    required
    style={{ width: "80%" }}
  />
  <button type="submit" disabled={reportSubmitting}>
    {reportSubmitting ? "Submitting..." : "Add Report"}
  </button>
</form>
```

- **Text input** → Controlled by `reportNote` and `setReportNote`.
- **Submit button** → Calls `onSubmitReport` on form submission.
- Button label dynamically updates between `"Submitting..."` and `"Add Report"`.

### Close Button

```jsx
<button onClick={onClose}>Close</button>
```

- Closes the modal when clicked.
- Calls `onClose` callback provided by parent.

---

## **Props**

- `spot` _(object | null)_ → The currently selected Cool Spot.
    - Must include: `id`, `name`, `type`, `barangay_id`, `lat`, `lon`, `reports`.
- `reportNote` _(string)_ → Current text input for new report.
- `setReportNote` _(function)_ → Updates `reportNote` state.
- `reportSubmitting` _(boolean)_ → Indicates whether a new report is being submitted.
- `onSubmitReport` _(function)_ → Handles report form submission.
- `onClose` _(function)_ → Closes the modal.

---

## **Key Integrations**

- **CoolSpotMarker** → Triggers this modal when a user clicks **View Details**.
- **Parent State** → Manages `spot`, `reportNote`, and submission lifecycle.
- **Community Reports** → Allows continuous user feedback directly tied to a location.

---

## **Usage**

- Used when a user clicks **View Details** on a Cool Spot marker.
- Displays:
    - Full spot details (name, type, location).
    - A list of existing reports.
    - A form to add a new report.
- User can **close modal** or **submit feedback** directly.
