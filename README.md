# Weather ETL Pipeline — Week 7 Project (AnalystLab Africa)

## Project Overview
A simple ETL (Extract, Transform, Load) pipeline that pulls live weather
data for three Nigerian cities from the OpenWeather API, cleans and
structures it with Pandas, stores it in three formats (CSV, Excel,
SQLite), and produces a short analysis with charts.

## Data Source
- **API:** [OpenWeather Current Weather Data API](https://openweathermap.org/api)
- **Cities:** Lagos, Abuja, Port Harcourt

## Tools Used
- Python 3 (run in Google Colab)
- `requests` — API calls
- `pandas` — data cleaning and structuring
- `matplotlib` — charts
- `sqlite3` — database storage
- `python-dotenv` — keeping the API key out of source control

## ETL Process
1. **Extract** (`extract_weather`, `extract_all`): calls the OpenWeather
   API once per city and pulls the raw JSON fields (temperature,
   humidity, wind speed, weather condition, coordinates, etc). Failed
   calls for one city are logged and skipped rather than crashing the
   whole run.
2. **Transform** (`transform_data`): renames raw API fields to clean,
   analysis-ready column names (see `COLUMN_RENAME_MAP`), converts
   numeric columns and timestamps to correct dtypes, drops incomplete
   rows, and sorts by city.
3. **Load** (`save_csv`, `save_excel`, `save_database`): writes the
   cleaned dataset to `data/weather_data.csv`, `data/weather_data.xlsx`,
   and a `weather_data` table inside `data/weather.db`.
4. **Analyze** (`analyze`): computes average temperature/humidity/wind
   speed, identifies the hottest/most humid/windiest city, and counts
   weather conditions. Saved to `data/analysis_summary.txt`.
5. **Visualize** (`create_charts`): saves four bar charts to `data/` —
   temperature, humidity, wind speed, and weather condition frequency.

## Steps Taken to Run This Project
```bash
pip install pandas requests matplotlib openpyxl python-dotenv
```
The API key is never hardcoded in the script. It's loaded either from
a local `.env` file (`OPENWEATHER_API_KEY=your_key`) or, if none is
found, the script prompts for it securely at runtime via `getpass` —
this made it safe to run and share from Google Colab.

```bash
python weather_etl_pipeline.py
```
All outputs are written to the `data/` folder.

## Key Findings
Pulled from a live run against Lagos, Abuja, and Port Harcourt:

- **Average temperature:** 24.15 °C
- **Average humidity:** 90.33 %
- **Average wind speed:** 2.62 m/s
- **Hottest city:** Port Harcourt (24.46 °C)
- **Most humid city:** Abuja (91 %)
- **Windiest city:** Port Harcourt (3.26 m/s)
- **Weather conditions observed:** Rain (all 3 cities reported rain at
  the time of extraction)

All three cities were close in temperature and uniformly humid, which
tracks with a rainy-season snapshot across southern/central Nigeria.
Port Harcourt edged out the other two on both temperature and wind
speed, while Abuja had the highest humidity despite being further
inland.

## What I Learned About ETL Pipelines
Separating Extract, Transform, and Load into distinct functions made
the pipeline much easier to debug — when something failed, the error
pointed straight at which stage broke instead of a wall of mixed logic.
Explicit column renaming during the transform step also mattered more
than expected: it documents exactly which raw API field became which
clean column, which matters when the API's naming and the dataset's
naming don't match. Wrapping each city's API call in its own
try/except meant one bad request (or a city name typo) didn't take
down the whole run. Finally, handling the API key via `.env`/`getpass`
instead of hardcoding it was a real lesson in keeping credentials out
of notebooks and version control — especially once running in Colab,
where it's easy to accidentally leave a key in a saved cell.
