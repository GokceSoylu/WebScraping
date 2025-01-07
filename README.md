# WebScraping

Web scraping methods

# Bundesliga Match Data Scraper

This README provides an overview of the Selenium-based Bundesliga match data scraper, including an explanation of the structure, design decisions, challenges faced, and their solutions.

## Overview
The script scrapes Bundesliga match data from the official Bundesliga website for seasons ranging from 2017 to 2025. The data includes:
- Season and matchday.
- Date of the match.
- Home and away teams.
- Match results.

The extracted data is saved to a CSV file for further analysis.

## Script Explanation

### Libraries Used
1. **Selenium**: To automate browser actions and scrape web pages.
2. **CSV**: To save the extracted data in a structured format.
3. **Time**: To introduce delays between requests to avoid being blocked.

### WebDriver Setup
The script uses Firefox with headless mode enabled to run the scraper without a GUI. The `webdriver.FirefoxOptions()` ensures a lightweight setup:
```python
options = webdriver.FirefoxOptions()
options.headless = True
driver = webdriver.Firefox(options=options)
```

### URL Construction
The scraper constructs URLs dynamically for each season and matchday:
```python
url = f'https://www.bundesliga.com/en/bundesliga/matchday/{year}-{next_year}/{week}'
```
This ensures scalability and flexibility for multiple seasons and matchdays.

### Data Extraction
The script uses Selenium’s CSS selectors and XPath to locate elements:
- **Match Rows**: Located using the `.matchRow` CSS selector.
- **Date Extraction**: Dates are dynamically located from preceding headers using XPath.
- **Team Names and Scores**: Retrieved using nested CSS selectors within each match row.

### Error Handling
Errors are logged using `try-except` blocks to handle issues such as missing elements or page load failures.
```python
try:
    date = get_date_for_match(row)
    home_team = row.find_element(...).text.strip()
except Exception as e:
    print(f"Failed to extract match: {e}")
```
This ensures the scraper continues execution even if some data is missing.

### Output
The collected data is written to a CSV file named `bundesliga_full_data.csv` with the following columns:
- Season
- Match Day
- Date
- Home Team
- Away Team
- Result

## Challenges and Solutions

### 1. Dynamic Loading of Content
- **Issue**: Match data is dynamically loaded by JavaScript, causing delays in rendering.
- **Solution**: Implemented `WebDriverWait` to ensure the content is fully loaded before scraping:
```python
WebDriverWait(driver, 15).until(
    EC.presence_of_element_located((By.CSS_SELECTOR, 'fixturescomponent'))
)
```

### 2. Inconsistent Date Headers
- **Issue**: Dates were not always attached directly to match rows.
- **Solution**: Used XPath to locate the closest preceding date header.

### 3. Blocking or Throttling
- **Issue**: Sending too many requests quickly could result in IP blocking.
- **Solution**: Introduced a `time.sleep(1)` delay between requests to mimic human browsing behavior.

### 4. Missing or Changed Selectors
- **Issue**: Changes in the website’s structure could cause the scraper to fail.
- **Solution**: Modularized element locators and implemented robust error handling to adapt to minor changes.

### 5. Headless Browser Challenges
- **Issue**: Some websites block headless browsers.
- **Solution**: Used a standard User-Agent string to mimic a real browser if necessary.

## Usage Instructions

### Prerequisites
- Python 3.x installed.
- GeckoDriver installed and added to the PATH.
- Selenium library installed:
  ```bash
  pip install selenium
  ```

### Running the Script
1. Save the script to a file, e.g., `bundesliga_scraper.py`.
2. Run the script:
   ```bash
   python bundesliga_scraper.py
   ```
3. The output file `bundesliga_full_data.csv` will be created in the same directory.

## Future Improvements
- Implementing retry logic for failed pages or matches.
- Adding multithreading to speed up scraping.
- Incorporating more advanced scraping libraries like BeautifulSoup for enhanced parsing capabilities.



---



## La Liga 
2000-2024 yılları arasındaki La Liga maç sonuçlarını https://www.skysports.com üzerinden kazımak için requests ve BeautifulSoup kütüphanelerini kullandım. Her sezon için dinamik URL'ler oluşturarak maç tarihlerini, takımları ve skor bilgilerini topladım. Veriler, CSV formatında kaydedildi.

Bazı durumlarda, HTML elementlerinin eksik olması nedeniyle AttributeError ile karşılaştım ve bu durumları atlamak için kodumu güncelledim. Ayrıca, verilerin doğru çekilmesi için sayfa yapısını dikkatlice analiz ettim. Bu süreç, veri kazıma becerilerimi geliştirmemi sağladı Gökçe Soylu 
