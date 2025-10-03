## **Overview**

This file provides a utility function to calculate the **heat index** (or "apparent temperature") based on air temperature and relative humidity.

The **heat index** represents how hot it feels to humans by combining temperature and humidity effects. The calculation is based on the **NOAA Heat Index formula**.


---

## **Functions**

### **`calculate_heat_index(temp_c, humidity)`**

Computes the heat index in Celsius and assigns a corresponding risk category.

#### **Parameters**

- `temp_c` _(float)_ – Air temperature in degrees Celsius.
- `humidity` _(float)_ – Relative humidity (percentage, e.g., `65` for 65%).

#### **Process**

1. **Convert temperature** from Celsius to Fahrenheit.
2. **Apply NOAA formula** for heat index in Fahrenheit.
3. **Convert result** back to Celsius.
4. **Classify risk level** based on heat index thresholds.

#### **Returns**

A tuple:

- `hi_c` _(float)_ – Heat index in Celsius (rounded to 1 decimal place).
- `risk` _(str)_ – Risk level classification:
    - `"Safe"`: < 27°C
    - `"Caution"`: 27–31.9°C
    - `"Extreme Caution"`: 32–40.9°C
    - `"Danger"`: 41–53.9°C
    - `"Extreme Danger"`: ≥ 54°C

#### **Example**

```python
from utils.heat_index import calculate_heat_index

hi, risk = calculate_heat_index(30, 70)
print(hi, risk)
```

**Output:**

```
36.8 Extreme Caution
```

---

## **Notes**

- The formula is designed for **temperatures ≥ 27°C (80°F)** and **humidity ≥ 40%**.  
    Values outside this range may give less accurate results.
- Useful for **weather analysis, risk alerts, or health advisories**.
