# DataPreprocessing – Air Quality Dakar vs Douala

This project performs data preprocessing and exploratory analysis on **air quality measurements** for two African coastal cities:

- **Dakar, Senegal**
- **Douala, Cameroon**

The aim is to transform raw data from the OpenAQ platform into a clean, well‑structured format and then compare air quality indicators between the two locations over a common time window (23 September 2024 to 27 October 2024).

---

## Context

Air quality is a critical environmental and public health issue, especially in rapidly growing urban areas.  
This project focuses on Dakar and Douala, using data collected by **AirGradient** sensors and accessed via OpenAQ. The work was carried out at **AIMS‑Senegal** by Group 5 “Open Air Quality”, supervised by Dr. Rockefeller (Stellenbosch University).

---

## Data Sources

The notebook uses two CSV files (downloaded from OpenAQ):

- `data/air-qualdakarsept_oct.csv` – measurements for Dakar  
- `data/air_qualdoualasept_oct.csv` – measurements for Douala  

Each raw dataset contains, among others, the following columns:

- `location_id`, `location_name`
- `sensor_id` (in the original API responses)
- `parameter` (type of pollutant or meteorological variable)
- `value`, `unit`
- `datetimeUtc`, `datetimeLocal`, `timezone`
- `latitude`, `longitude`
- `owner_name`, `provider`
- Mobility/monitor flags

---

## Preprocessing Objectives

The preprocessing in the notebook is designed to:

1. **Transform raw, long-format measurements** into a tidy **wide-format** dataset where each parameter becomes a separate column.
2. **Align temporal information** (local datetime, date, and time) and select a common analysis window for both cities.
3. **Select relevant air quality and meteorological variables** for comparative analysis.
4. Prepare data structures that are convenient for plotting time series and computing summary statistics.

---

## Preprocessing Steps

### 1. Load Raw Data

For each city, the notebook:

- Reads the corresponding CSV into a `pandas` DataFrame.
- Inspects a random sample (`.sample(10)`) to understand the structure and content.

### 2. Pivot to Wide Format

The raw data is in long format (one row per measurement per parameter).  
The notebook pivots the data so that each parameter becomes its own column:

- Index used for pivoting:
  - `datetimeLocal`
  - `location_id`
  - `location_name`
  - `latitude`
  - `longitude`
- Parameters converted into columns (values in `value`):
  - `pm1`
  - `pm10`
  - `pm25`
  - `relativehumidity`
  - `temperature`
  - `um003` (0.3 µm particles)

The resulting DataFrames:

- `group_dakar` – preprocessed dataset for Dakar
- `group_douala` – preprocessed dataset for Douala

### 3. Datetime Handling

For each city:

- `datetimeLocal` is converted to a proper datetime type.
- Two additional columns are created:
  - `Date` – the calendar date
  - `Time` – the time of day

This facilitates grouping, resampling, and plotting time‑based trends (e.g. hourly or daily patterns).

### 4. Column Selection and Cleanup

For comparative analysis, the notebook keeps only the core variables:

- `pm1`, `pm10`, `pm25`
- `relativehumidity`
- `temperature`
- `um003`
- `datetimeLocal`, `Date`, `Time`

In `group_dakar`, location metadata columns (`location_id`, `location_name`, `latitude`, `longitude`) are dropped once they are no longer needed, since Dakar has a single station in this dataset.

---

## Exploratory Visualization

After preprocessing, the notebook performs preliminary visualizations, such as:

- **Hourly trends** for each pollutant and meteorological variable.
- Line plots overlaying Dakar and Douala (e.g. `pm1` time series) to compare their behavior over the same period.

These plots use `matplotlib` and are built directly from `group_dakar` and `group_douala`.

---

## Technologies Used

- **Python**
  - `pandas` – data manipulation and pivoting
  - `numpy` – numerical operations
  - `matplotlib` – plotting time series
- **Data Source**
  - OpenAQ / AirGradient CSV exports

---

## How to Run the Notebook

1. Place the notebook `air_quality_group5.ipynb` in the root of the project (or in a `notebooks/` folder).
2. Ensure the data files are available in the `data/` directory:
   - `air-qualdakarsept_oct.csv`
   - `air_qualdoualasept_oct.csv`
3. Install dependencies (e.g. in a virtual environment):

   ```bash
   pip install pandas numpy matplotlib
   ```

4. Launch Jupyter:

   ```bash
   jupyter notebook
   ```

5. Open `air_quality_group5.ipynb` and run the cells in order:
   - Data loading
   - Pivoting and datetime processing
   - Column selection
   - Visualizations

---

## Possible Extensions

- Add **resampling** (e.g. daily averages) and statistical summaries for each parameter and city.
- Compute and compare **AQI‑like indicators** or threshold exceedances.
- Integrate **additional stations or time periods** for broader spatial or temporal coverage.
- Export cleaned datasets (`group_dakar`, `group_douala`) to new CSV files for downstream modeling or dashboards.
