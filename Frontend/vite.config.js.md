## **Overview**

The **`vite.config.js`** file defines the **Vite build and development server configuration** for the React frontend.  
It sets up plugins, server options, and a proxy to connect the frontend with the FastAPI backend during development.

---

## **Dependencies**

- `vite.defineConfig` → Simplifies configuration with proper IntelliSense and type support.
- `@vitejs/plugin-react` → Adds React-specific optimizations (JSX, Fast Refresh).

---

## **Code Breakdown**

### Base Configuration

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  ...
})
```

- Imports Vite’s config helper and the React plugin.
- Registers **React plugin** for JSX support and fast hot reload.


### Dev Server Configuration

```js
server: {
  port: 3000,
  proxy: {
    '/api': {
      target: 'http://127.0.0.1:8000/',
      changeOrigin: true,
      rewrite: (path) => path.replace(/^\/api/, '')
    }
  }
}
```

- **`port: 3000`** → Runs the React dev server on port 3000.
- **`proxy`** → Redirects frontend API calls to the FastAPI backend during development.

#### Proxy Rules:

- Requests starting with `/api` are forwarded to:

 ```
    http://127.0.0.1:8000/
  ```

- `changeOrigin: true` → Ensures host headers are adjusted to match the backend.
- `rewrite` → Removes `/api` prefix before sending the request to the backend.

Example:

- Frontend request → `/api/barangays/1/forecast`
- Backend request → `http://127.0.0.1:8000/barangays/1/forecast`

---

## **Key Integrations**

- **Frontend (React)** runs on `http://localhost:3000`.
- **Backend (FastAPI)** runs on `http://127.0.0.1:8000`.
- The **proxy** ensures seamless communication without CORS issues in development.

---

## **Usage**

- Start frontend with:

```bash
 npm run dev   
```

- Open: `http://localhost:3000`
- API requests to `/api/...` will automatically route to the FastAPI backend.
