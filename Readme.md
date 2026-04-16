# 📱 Mobile Data Web Scraping

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-WebScraping-green)
![Requests](https://img.shields.io/badge/Requests-HTTP-orange)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-yellow?logo=pandas)
![Scraper](https://img.shields.io/badge/Type-Web%20Scraper-purple)
![Dataset](https://img.shields.io/badge/Data-Mobile%20Network-lightgrey)
![Status](https://img.shields.io/badge/Status-Active-success)
![License](https://img.shields.io/badge/License-MIT-blue)

> A comprehensive comparison of three major Python web scraping frameworks — **Selenium**, **BeautifulSoup**, and **Scrapy** — applied to extract real mobile phone specifications and pricing data from [bgr.in](https://www.bgr.in/).

---

## 📌 Project Overview

This project demonstrates how to scrape structured mobile phone data (specs, price, availability) from a live website using three distinct scraping approaches. Each tool has its own notebook/script, scraped output, and behavioral characteristics that are compared in `explore.ipynb`.

**Data Source:** [BGR India – Mobile Phones](https://www.bgr.in/gadgets/mobile-phones/search/)

**Reference Article:** [Scrapy vs Selenium vs Beautiful Soup for Web Scraping](https://medium.com/analytics-vidhya/scrapy-vs-selenium-vs-beautiful-soup-for-web-scraping-24008b6c87b8)

---

## 📂 Project Structure

```
Mobile-Data-Web-Scraping/
│
├── selenium.ipynb              # Selenium scraping notebook
├── beautifulsoup.ipynb         # BeautifulSoup scraping notebook
├── explore.ipynb               # Data exploration & visualization
├── mobile_scrapy/              # Scrapy spider project folder
│   └── spiders/
│       └── amazon_scraper.py   # Scrapy spider
│
├── data/
│   ├── selenium_data.json      # 50 records scraped via Selenium
│   ├── beautifulsoup_data.json # 150 records scraped via BeautifulSoup
│   ├── scrapy_data.json        # 150 records scraped via Scrapy
│   └── scrapy_data.csv         # Same Scrapy data in CSV format
│
├── chromedriver.exe            # ChromeDriver binary for Selenium
├── scrap.png                   # Screenshot: listing page HTML structure
├── scrap2.png                  # Screenshot: specs page HTML structure
└── Readme.md                   # Project documentation
```

---

## 📊 Scraped Data Summary

| Tool           | Records Collected | Output Format       |
|----------------|:-----------------:|---------------------|
| Selenium       | 50                | JSON                |
| BeautifulSoup  | 150               | JSON                |
| Scrapy         | 150               | JSON + CSV          |

### Data Fields Captured

Each scraped record contains up to **29 fields**:

| Field                  | Example Value            |
|------------------------|--------------------------|
| `name`                 | Samsung Galaxy F22       |
| `Device Type`          | Smartphone               |
| `Sim Capability`       | Dual-SIM                 |
| `Processor`            | MediaTek Helio G80       |
| `Internal Memory`      | 4GB                      |
| `Internal Storage`     | 64GB                     |
| `Display Size`         | 6.40-inch                |
| `Display Technology`   | AMOLED                   |
| `Display Resolution`   | 720x1600                 |
| `Main Camera`          | Quad Camera              |
| `Main Camera resolution` | 48MP + 8MP + 2MP + 2MP |
| `Front Camera Resolution` | 13MP                 |
| `Battery Capacity`     | 6000mAh                  |
| `USB Type`             | USB Type-C               |
| `Fast Charging`        | Yes                      |
| `Wireless Charging`    | No                       |
| `Network Capability`   | 2G, 3G, 4G               |
| `Fingerprint Sensor`   | Side-Mounted             |
| `Face Unlock`          | Yes                      |
| `Software`             | Android 11               |
| `Availability`         | Available In India       |
| `price`                | ₹12,499                  |

---

## 🛠️ Tools & Dependencies

### Python Version
- Python 3.9.6

### Libraries

| Library        | Purpose                              | Install                         |
|----------------|--------------------------------------|---------------------------------|
| `selenium`     | Browser automation & dynamic scraping | `pip install selenium`         |
| `bs4`          | HTML parsing for static pages        | `pip install bs4`               |
| `requests`     | HTTP requests for BeautifulSoup      | `pip install requests`          |
| `scrapy`       | Full-featured crawling framework     | `pip install scrapy`            |
| `scrapyjs`     | JavaScript rendering with Scrapy     | `pip install scrapyjs`          |

### External Tool
- **ChromeDriver** — Required for Selenium. Must match your installed Chrome version.
  - Download: [chromedriver.chromium.org](https://chromedriver.chromium.org/)

---

## ⚙️ Installation

```bash
# Clone the repository
git clone https://github.com/theshashi001/Mobile-Data-Web-Scraping.git
cd Mobile-Data-Web-Scraping

# Install all dependencies
pip install selenium bs4 requests scrapy scrapyjs
```

---

## 🚀 Usage

### 1. Selenium
Open `selenium.ipynb` in Jupyter Notebook.

> **Important:** Update the `executable_path` for `chromedriver.exe` to match your local path before running.

```python
driver = webdriver.Chrome(executable_path='path/to/chromedriver.exe')
url = 'https://www.bgr.in/gadgets/mobile-phones/search/'
```

The scraper:
1. Navigates to the mobile listing page
2. Extracts product links from `<li class='mobile-listing'>` elements
3. Visits each product page and extracts specs from the spec table
4. Paginates through up to `no_of_pages` pages (configurable)
5. Saves results to `data/selenium_data.json`

---

### 2. BeautifulSoup
Open `beautifulsoup.ipynb` in Jupyter Notebook.

Uses `requests` to fetch page HTML and `BeautifulSoup` to parse it.
Results are saved to `data/beautifulsoup_data.json`.

---

### 3. Scrapy

```bash
cd mobile_scrapy
scrapy crawl amazon_scraper -o ../data/filename.csv
```

Scrapy handles request queuing, retries, and rate limiting automatically.
Outputs both `scrapy_data.json` and `scrapy_data.csv`.

---

## 🔍 How Selenium Works (Architecture)

The scraper follows a two-stage navigation pattern:

**Stage 1 — Listing Page**

```
bgr.in/gadgets/mobile-phones/search/
└── <ul>
    └── <li class="mobile-listing">
        └── <aside class="col-sm-12 col-lg-7 lstdesc">
            └── <h2>
                └── <a href="...">  ← Product URL extracted here
```

**Stage 2 — Product Specs Page**

```
bgr.in/gadgets/mobile-phones/[device-name]/
└── <aside class="mobile-details-r">
    └── <h1 itemprop="name">         ← Device name
    └── <span class="rsm">           ← Price
└── <section class="col-xs-12 blkSpecs">
    └── <ul>
        └── <li>
            ├── <span class="col-xs-12 col-sm-5 spec-lbl">  ← Spec key
            └── <span class="col-xs-12 col-sm-7 spec-val">  ← Spec value
```

---

## 🔬 Tool Comparison

| Feature                    | Selenium       | BeautifulSoup  | Scrapy         |
|----------------------------|:--------------:|:--------------:|:--------------:|
| Handles JavaScript         | ✅ Yes         | ❌ No          | ⚠️ With plugin |
| Speed                      | 🐢 Slow        | ⚡ Fast        | ⚡ Fast        |
| Built-in crawling          | ❌ Manual      | ❌ Manual      | ✅ Built-in    |
| Learning curve             | Medium         | Low            | Medium-High    |
| Browser required           | ✅ Chrome      | ❌ No          | ❌ No          |
| Records collected here     | 50             | 150            | 150            |
| Output formats             | JSON           | JSON           | JSON + CSV     |

---

## 📈 Data Exploration

`explore.ipynb` contains analysis and visualization of the scraped data including:
- Price distribution across brands
- Processor & RAM frequency charts
- Display size trends
- Battery capacity comparisons
- 5G vs 4G adoption breakdown

---

## ⚠️ Known Issues & Notes

- The Selenium notebook will throw a `WebDriverException` if `chromedriver.exe` is not in the system PATH or the path is incorrect. Update the path before running.
- The `selenium_data.json` contains 50 records (5 pages × ~10 phones per page) while BeautifulSoup and Scrapy collected 150 records each.
- Some fields like `IP Rating` and `SD Card Capacity` are only present in select entries depending on the phone model.
- Website structure may have changed since the data was collected — XPaths and CSS selectors may need updating.

---

## 📄 License

This project is for **educational purposes** only. Respect the terms of service of any website before scraping.

---

##  Acknowledgements

- Data sourced from [BGR India](https://www.bgr.in/)
- Comparison article: [Analytics Vidhya](https://medium.com/analytics-vidhya/scrapy-vs-selenium-vs-beautiful-soup-for-web-scraping-24008b6c87b8)
- GitHub Repository: [theshashi001/Mobile-Data-Web-Scraping](https://github.com/theshashi001/Mobile-Data-Web-Scraping)
