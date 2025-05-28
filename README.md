
---

## **EGAT Real-time Power Generation Scraper**

This Python script is designed to extract real-time power generation data from the Electricity Generating Authority of Thailand (EGAT) website at [https://www.sothailand.com/sysgen/egat/](https://www.sothailand.com/sysgen/egat/). It utilizes **Selenium** to interact with the website and read data from the **Console Log**, which the website uses to dynamically update data. The extracted data is continuously saved into a **CSV file** at specified intervals.

---

### 📁 **Project Structure**

```
egat-scraper-project/
├── src/
│   ├── __init__.py
│   ├── scraper/
│   │   ├── __init__.py
│   │   └── egat_scraper.py        # Contains EGATRealTimeScraper class
│   ├── config.py                  # Configuration settings
│   ├── tasks.py                   # Prefect task definitions
│   └── flows.py                   # Prefect flow definitions
├── notebooks/
│   └── run_scraper_and_save_to_lakefs.ipynb  # Your original notebook for reference
├── run_scheduled_flow.py          # Script to run/deploy the Prefect flow
├── requirements.txt               # Project dependencies
├── docker-compose.yml             # Existing docker-compose
└── .env                           # For environment variables (you'll create this)
```

---

### 🌟 **Key Features**

* **Real-time scraping**: Records live power generation (MW) and temperature values.
* **Reads from Console Log**: Uses a specialized technique to extract messages logged by JavaScript in the browser console instead of parsing HTML.
* **Supports Headless Mode**: Runs Chrome in headless mode via Selenium for performance and background execution.
* **Automatic ChromeDriver management**: Uses `webdriver-manager` to avoid manual installation of the driver.
* **CSV storage**: Appends extracted data into a CSV file with a timestamp.
* **Supports continuous operation**: Can run in a loop at regular intervals.
* **Built-in logging**: Logs events for analysis or troubleshooting during execution.

---

### ⚙️ **How It Works**

The website [https://www.sothailand.com/sysgen/egat/](https://www.sothailand.com/sysgen/egat/) displays real-time power data. JavaScript on the page updates the power output (in MW) and temperature values and logs them in the Console. This script extracts the data by monitoring console messages using the following steps:

1. **Initialize WebDriver**: Launches Chrome in headless mode with logging enabled.
2. **Load the target website**.
3. **Wait for data to load**: Allows time for JavaScript to execute.
4. **Extract from Console**: Looks for messages that start with `updateMessageArea:` in the log.
5. **Parse and format data**: Uses regex to extract key fields (timestamp, MW, temp, etc.).
6. **Save to CSV**: Converts to a dictionary and appends to a CSV file.
7. **Repeat (if enabled)**: Runs continuously every *n* seconds if `scrape_continuously` is set.
8. **Properly shut down WebDriver**: Ensures Chrome closes cleanly after stopping.

---

### 🧰 **Prerequisites**

* Python 3.7+
* Google Chrome installed

---

### 🛠️ **Installation & Setup**

Clone the repository (if applicable) or download the script files:

```bash
# If it's a repository:
git clone <repository_url>
cd <repository_directory>
```

Create a virtual environment:

```bash
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
```

Install required packages:

```bash
pip install pandas selenium webdriver-manager
```

Or run:

```bash
pip install -r requirements.txt
```

---