## **Overview**

This file is the **entry point** of the React frontend.  
It bootstraps the application by rendering the root component (`App`) into the DOM.

Key responsibilities:

- Initializes React in **strict mode** for better error checking.
- Attaches the React app to the root DOM node (`#root`).
- Imports global styles (`index.css`).

---

## **Dependencies**

- `react.StrictMode` – Wraps the app to highlight potential problems during development (does not affect production).
- `react-dom/client.createRoot` – Creates a root rendering target for React 18’s concurrent rendering.
- `./App.jsx` – The main application component, where routing and page layout are handled.
- `./index.css` – Global stylesheet applied to the entire app.

---

## **Code Breakdown**

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
```

- Imports necessary modules and the main app component.

```jsx
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

- `createRoot(document.getElementById('root'))`: Finds the DOM node with `id="root"` and mounts the React app there.
- `.render(<StrictMode>...</StrictMode>)`: Renders the app wrapped inside `StrictMode`.

---

## **Integration**

- `main.jsx` should remain **lightweight**: it should only handle rendering, while the app logic (routing, state management, API integration) lives in `App.jsx` and other components.
- Global providers (like React Router, Redux, or Context providers) are usually added here, wrapping `<App />`.

---

## **Usage**

The `main.jsx` file is automatically used by **Vite** (or similar bundlers) as the entry point.

### Start the frontend:

```bash
npm run dev
```

### Build for production:

```bash
npm run build
```
