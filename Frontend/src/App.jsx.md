## **Overview**

`App.jsx` is the **root component** of the frontend application.  
It defines the main **layout structure** (Header → Main → Footer) and wraps the application inside a **React Router** to enable client-side routing.  

Key responsibilities:  
- Provides a **global layout** for all pages.  
- Initializes **React Router** for navigation.  
- Imports global and library styles (App and Leaflet).  

---

## **Dependencies**

- `./App.css` – Local stylesheet for global app-level styles.  
- `leaflet/dist/leaflet.css` – Styles for **Leaflet**, a mapping library (ensures maps render correctly).  
- `./components/Header` – Top navigation/header component.  
- `./components/Main` – Main content area (likely handles routing to different pages).  
- `./components/Footer` – Footer component for global links or branding.  
- `react-router-dom.BrowserRouter` – Enables declarative routing in the React app.  

---

## **Code Breakdown**

```jsx
import './App.css'
import 'leaflet/dist/leaflet.css'
import { Header } from './components/Header'
import { Main } from './components/Main'
import { Footer } from './components/Footer'
import { BrowserRouter as Router } from "react-router-dom";
```

- Loads styles and imports layout components.  
- Pulls in React Router (`BrowserRouter`) to handle navigation.  

```jsx
function App() {
  return (
    <Router>
      <Header />
      <Main />
      <Footer />
    </Router>
  );
}
```

- Wraps the entire app with `<Router>` → enables route-based navigation in the `Main` component.  
- Defines the global structure:  
  - **Header** → Always visible at the top.  
  - **Main** → Central container for routed pages.  
  - **Footer** → Always visible at the bottom.  

```jsx
export default App
```

- Exports `App` so it can be rendered in `main.jsx`.  

---

## **Integration**

- This file is the **root layout** and is rendered in `main.jsx`.  
- Any **route definitions** (e.g., `/`, `/about`, `/map`) are expected to live inside `Main.jsx`.  
- Global providers (e.g., Redux, Context, or Theme providers) can also be added here above `<Router>`.  

---

## **Usage**

When the app runs, the structure looks like:  

```
[Header]
   ↓
[Main Content → Route-based pages]
   ↓
[Footer]
```

For example:  
- Visiting `/` → Home page inside `Main`.  
- Visiting `/map` → Map view inside `Main`.  
