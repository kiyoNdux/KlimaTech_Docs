## `calculate_heat_index` Utility Function

```python
def calculate_heat_index(temp_c: float, humidity: float) -> tuple[float, str]:
    ...
```

### Description

Calculates the **heat index** (apparent temperature) based on air temperature and relative humidity, using the **NOAA Heat Index formula**.  
Returns the heat index in Celsius along with a categorical **risk level**.

---

### Parameters

- `temp_c` _(float)_ – Air temperature in degrees Celsius
- `humidity` _(float)_ – Relative humidity as a percentage (0–100)


### Process

1. Converts input temperature from Celsius to Fahrenheit.
2. Applies the **NOAA regression formula** to compute the heat index in Fahrenheit.
3. Converts the result back to Celsius.
4. Categorizes the result into a **risk level** based on thresholds:
    - `< 27°C` → _Safe_
    - `27–32°C` → _Caution_
    - `32–41°C` → _Extreme Caution_
    - `41–54°C` → _Danger_
    - `> 54°C` → _Extreme Danger_


### Returns

- `hi_c` _(float)_ – Heat index in Celsius (rounded to one decimal place)
- `risk` _(str)_ – Risk category (e.g., _Safe, Caution, Danger_)

---

### Example Usage

```python
>>> calculate_heat_index(30, 70)
(36.7, "Extreme Caution")

>>> calculate_heat_index(25, 50)
(25.7, "Safe")
```
