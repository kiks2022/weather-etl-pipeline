# 🌦️ Weather ETL Pipeline
**Week 7 Project — AnalystLab Africa**

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-150458?style=flat&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Charts-11557C?style=flat)
![SQLite](https://img.shields.io/badge/SQLite-Storage-003B57?style=flat&logo=sqlite&logoColor=white)
![OpenWeather](https://img.shields.io/badge/OpenWeather-API-EA6E00?style=flat&logo=openweathermap&logoColor=white)
![Status](https://img.shields.io/badge/status-complete-brightgreen?style=flat)
![License](https://img.shields.io/badge/license-MIT-blue?style=flat)

A small end-to-end ETL (Extract, Transform, Load) pipeline that pulls
live weather data from the OpenWeather API for three Nigerian cities,
cleans it with Pandas, stores it in three formats, and produces a
short analysis with charts.

---

## 📁 Repository Structure
```
weather-etl-pipeline/
├── Weather_ETL_Pipeline.ipynb   # the full pipeline, run in Google Colab
├── README.md
├── LICENSE
└── data/                        # everything the pipeline produces
    ├── weather_data.csv
    ├── weather_data.xlsx
    ├── weather.db
    ├── analysis_summary.txt
    ├── temperature_chart.png
    ├── humidity_chart.png
    ├── windspeed_chart.png
    └── weather_condition_chart.png
```

## 🌍 Data Source
- **API:** [OpenWeather Current Weather Data API](https://openweathermap.org/api)
- **Cities:** Lagos, Abuja, Port Harcourt

## 🛠️ Tools Used
| Tool | Purpose |
|---|---|
| 🐍 Python 3 (Google Colab) | Runtime |
| 📡 `requests` | API calls |
| 🐼 `pandas` | Cleaning and structuring data |
| 📊 `matplotlib` | Charts |
| 🗄️ `sqlite3` | Database storage |
| 🔐 `python-dotenv` | Keeping the API key out of source control |

## ⚙️ How the Pipeline Works
1. **🔽 Extract** — calls the OpenWeather API once per city and pulls
   raw fields: temperature, humidity, wind speed, weather condition,
   coordinates, and timestamp. A failed call for one city is logged
   and skipped rather than crashing the whole run.
2. **🔧 Transform** — renames raw API fields into clean, analysis-ready
   column names, converts numeric columns and timestamps to correct
   dtypes, drops incomplete rows, and sorts by city.
3. **💾 Load** — writes the cleaned dataset to `data/weather_data.csv`,
   `data/weather_data.xlsx`, and a `weather_data` table inside
   `data/weather.db`.
4. **📈 Analyze** — computes average temperature, humidity, and wind
   speed; identifies the hottest, most humid, and windiest city; and
   counts weather conditions. Saved to `data/analysis_summary.txt`.
5. **🎨 Visualize** — saves four bar charts to `data/`: temperature,
   humidity, wind speed, and weather condition frequency.

## 🚀 Running It Yourself
```bash
pip install pandas requests matplotlib openpyxl python-dotenv
```
The API key is never hardcoded. It's loaded from a local `.env` file
(`OPENWEATHER_API_KEY=your_key`), or — if none is found — the notebook
prompts for it securely at runtime via `getpass`. This kept the key
safe to run and share directly from Google Colab.

```bash
python weather_etl_pipeline.py
```
All outputs land in `data/`.

## 🔑 Key Findings
From a live run against Lagos, Abuja, and Port Harcourt:

| Metric | Value |
|---|---|
| 🌡️ Average temperature | 24.15 °C |
| 💧 Average humidity | 90.33 % |
| 🌬️ Average wind speed | 2.62 m/s |
| 🔥 Hottest city | Port Harcourt (24.46 °C) |
| 💦 Most humid city | Abuja (91 %) |
| 🌪️ Windiest city | Port Harcourt (3.26 m/s) |
| ☔ Weather condition | Rain (all 3 cities) |

All three cities were close in temperature and uniformly humid, which
tracks with a rainy-season snapshot across southern/central Nigeria.
Port Harcourt edged out the other two on both temperature and wind
speed, while Abuja had the highest humidity despite being further
inland.

## 💡 What I Learned About ETL Pipelines
Splitting Extract, Transform, and Load into separate functions made
the pipeline far easier to debug — when something failed, the error
pointed straight at which stage broke instead of a wall of mixed
logic. Explicit column renaming during the transform step mattered
more than expected: it documents exactly which raw API field became
which clean column, which matters once the API's naming and the
dataset's naming diverge. Wrapping each city's API call in its own
`try/except` meant one bad request (or a city typo) didn't take down
the whole run. And handling the API key via `.env`/`getpass` instead
of hardcoding it was a real lesson in keeping credentials out of
notebooks and version control — especially in Colab, where it's easy
to accidentally leave a key sitting in a saved cell.

---

## ✍️ Author
**Kikelomo Adekoya** — AnalystLab Africa, Week 7
