

# **EGAT Real-time Power Generation Scraper**

## **Overview**

This Python-based application retrieves live electricity generation data from the Electricity Generating Authority of Thailand (EGAT) via the URL [https://www.sothailand.com/sysgen/egat/](https://www.sothailand.com/sysgen/egat/). Utilizing Selenium, the script extracts real-time values directly from the browser’s developer console logs, where the site dynamically emits updated data. It is designed for continuous operation, capturing new entries at user-defined intervals.

---

## **Key Benefits**

* **Live Monitoring**
  Continuously collects real-time electricity generation data from EGAT, offering immediate insights into Thailand’s national grid performance.

* **Automated Data Collection**
  Automatically scrapes and stores data on a set schedule, reducing manual effort and ensuring a consistent historical log.

* **Forecasting Enablement**
  Supports predictive modeling for electricity demand and supply, helping optimize energy planning and distribution strategies.

* **Interactive Visualization**
  Displays real-time trends through an intuitive Streamlit dashboard, accessible to both technical and non-technical audiences.

* **Advanced Scraping Technique**
  Demonstrates a sophisticated scraping approach by capturing dynamic JavaScript-driven content from browser console logs using Selenium.

* **Time-Series Analysis**
  Creates a rich dataset suitable for trend analysis, anomaly detection, and operational planning.

---

## **Dataset Quality Checks**

| Criteria                            | Description                        | Status   |
| ----------------------------------- | ---------------------------------- | -------- |
| Minimum of 1,000 records            | Ensures adequate data volume       | ✅ Passed |
| Full 24-hour coverage               | Verifies complete daily data range | ✅ Passed |
| At least 90% data completeness      | Minimal missing or null values     | ✅ Passed |
| No columns with 'object' data types | Confirms correct data typing       | ✅ Passed |
| No duplicate records                | Guarantees record uniqueness       | ✅ Passed |

> **Dataset location**: `data/egat_realtime.parquet`

---

## **Project Structure**

```
DSI321_2025/
├── __pycache__/
│   └── egat_pipeline.cpython-312.pyc
├── data/
│   └── egat_realtime.parquet
├── test-scraping/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── run_scraper_and_save_to_lakefs.ipynb
├── UI/
│   └── streamlit_app.py
├── docker-compose.yml
├── egat_pipeline.py
├── quality_check.py
├── prefect.yaml
└── README.md
```

---

## **Technology Stack**

* **Web Scraping**: `Selenium`, `webdriver_manager`
* **Data Validation**: `Pydantic`
* **Data Storage**: `lakeFS`
* **Workflow Orchestration**: `Prefect`
* **Visualization**: `Streamlit`
* **CI/CD Pipeline**: `GitHub Actions`

---

## **System Requirements**

* Docker-supported system
* Internet access for EGAT data source
* Minimum 4 GB RAM
* Available port (e.g., `localhost:4200`) for Prefect UI

---

## **Setup Instructions**

1. **Create Virtual Environment**

   ```bash
   python -m venv .venv
   ```

2. **Install Dependencies**

   ```bash
   # Windows
   pip install -r test_scraping\requirements.txt

   # macOS/Linux
   pip install -r test_scraping/requirements.txt
   ```

3. **Activate Virtual Environment**

   ```bash
   # Windows
   .venv\Scripts\activate

   # macOS/Linux
   source .venv/bin/activate
   ```

---

## **Running Prefect**

1. **Fix PREFECT\_API\_URL (if error appears)**

   ```bash
   # Windows
   $env:PREFECT_API_URL = "http://127.0.0.1:4200/api"

   # macOS/Linux
   export PREFECT_API_URL="http://127.0.0.1:4200/api"
   ```

2. **Deploy Workflow**

   ```bash
   prefect deploy
   ```

3. **Start Worker**

   ```bash
   prefect worker start --pool 'default-agent-pool' --work-queue 'default'
   ```

---

## **Launch Dashboard**

Run the Streamlit UI:

```bash
streamlit run UI/streamlit_app.py
```


