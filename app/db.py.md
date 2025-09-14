## `Environment Variables`

Loaded via **`python-dotenv`** (`load_dotenv()`), the following variables must be defined in your `.env` file:

- `DB_USER` – Database username
- `DB_PASSWORD` – Database password
- `DB_HOST` – Database host (e.g., `localhost`)
- `DB_PORT` – Database port (e.g., `5432`)
- `DB_NAME` – Database name

---

### Database URL

```python
DATABASE_URL = f"postgresql://{DB_USER}:{DB_PASSWORD}@{DB_HOST}:{DB_PORT}/{DB_NAME}"
```

Constructs the PostgreSQL connection string dynamically from environment variables.


### Engine

```python
engine = create_engine(DATABASE_URL, echo=True)
```

- Creates the SQLModel engine for database interaction.
- `echo=True` enables SQL logging (prints all SQL statements to the console).

---

### Functions

#### `init_db()`

```python
def init_db():
    SQLModel.metadata.create_all(engine)
```

- Initializes the database schema by creating all tables defined in your SQLModel models.
- Should be run at application startup or migrations.

#### `get_session()`

```python
def get_session():
    with Session(engine) as session:
        yield session
```

- Provides a database session for use in dependency injection (e.g., in FastAPI routes).
- Uses `yield` to ensure the session is properly closed after use.
