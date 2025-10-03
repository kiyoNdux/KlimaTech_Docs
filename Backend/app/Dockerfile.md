## **Overview**

This `Dockerfile` defines the containerized environment for running the **Heat Risk API** using **FastAPI** and **Uvicorn**.  
It creates a lightweight Python environment, installs dependencies, and launches the application in a production-ready configuration.

---

## **Dependencies**

- **Base Image**: [`python:3.12-slim`](https://hub.docker.com/_/python) (minimal Python image with version 3.12).
- **Requirements**: `requirements.txt` (lists all Python dependencies needed by the API).
- **FastAPI/Uvicorn**: Used to serve the Heat Risk API.

---

## **Steps in the Dockerfile**

1. **Base Image**

```dockerfile
  FROM python:3.12-slim
```

Uses a minimal Python 3.12 image to keep the container small and efficient.

2. **Set Working Directory**

```dockerfile
 WORKDIR /code
```

All subsequent commands will run inside `/code`.

3. **Copy Dependencies File**

```dockerfile
COPY ./requirements.txt /code/requirements.txt
  ```

Copies `requirements.txt` into the container.

4. **Install Dependencies**

```dockerfile
  RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
  ```

Installs required Python packages without caching to keep the image smaller.    

5. **Copy Application Code**

```dockerfile
 COPY ./app /code/app
  ```

Copies the project’s `app` folder into the container.

6. **Run Application with Uvicorn**

```dockerfile
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
  ```

Starts the FastAPI app with Uvicorn, binding to all interfaces (`0.0.0.0`) on port `80`.


---

## **Usage**

### Build the Image

```bash
docker build -t heat-risk-api .
```

### Run the Container

```bash
docker run -d -p 80:80 heat-risk-api
```

The API will now be accessible at:  
`http://localhost/`

---

## Notes

- Default port is **80** (we can change by editing the `CMD` or mapping a different host port).
- For local development with hot-reload, consider using `uvicorn` with `--reload` instead of this production config.
- Works seamlessly with **Docker Compose** when connecting to Postgres or Redis.
