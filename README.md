# Amazon Web Scraper Project

### Overview

This web scraper, built using Python, Beautiful Soup, and Requests, is designed to extract information from Amazon product pages. The goal is to retrieve product details such as title, price, ratings, and date, storing the data in a CSV file for further analysis or use.

### Skill Demonstrated
- Web Scraping Proficiency
- Data Processing and Cleaning
- Automation and Scripting
- CSV Data Storage

### Tools

- Python [Download here](https://www.python.org/downloads/)
- Jupyter Notebook (Beautiful soup, pandas, requests)

### Features

- **Product Information Extraction**: Extracts essential product details, including title, price, ratings, and date.
- **CSV Data Storage**: Saves the extracted information in a CSV file (`AmazonWebScraperDataset.csv`) for convenient data manipulation.
- **Customizable**: Easily adaptable for different Amazon product pages or additional information extraction.

### Usage

1. Installed Jupyter

```bash
pip install jupyter
```

2. Launched Jupyter Notebook

```bash
jupyter notebook
```
3. Created a new notebook with python in the Jupyter Notebook interface.
4. Imported the necessary libraries
   ```python
   from bs4 import BeautifulSoup
   import requests
   import time
   import datetime
   import smtplib
    ```
5. Connected to the website to pull the data
   ```python
      URL = 'https://www.amazon.com/Funny-Data-Systems-Business-Analyst/dp/B07FNW9FGJ/ref=sr_1_5?crid=2B4LQHJDAJHLR&dib=eyJ2IjoiMSJ9.WiKhGOLdBAacALLGC9ayNOuSFgH2acw5wZ3-xstLg4_swdoRKRSjtvVVD-eNmgait23JGAUqu0oK-D8jDjw0oaPjZ3j0poQyDL2ZxeamSjs0xqmLeBl6pagYqF4RZGE7sGRH2FOV-St2pHZjjKVX_8Rnx4RWyzGhZk-xwOva4C6alCFwePf4O0l7aJ-HhLgxSQZiLtenf5ghDJZZ-i7ZFvyPsP0__0KA0B4qKsmVAdAgMv-07nnPCiwUPa1ghSFCCy2mBxCWGCK2PrfPRYEIX1QXzfyGy41-LImlMCNDZ8E.962m5aTRMj4rk4pjp5JZ6P9Y5RYt0594NNnjEwvPpMg&dib_tag=se&keywords=data+analyst+shirt&qid=1708148038&sprefix=data+analyst+shirt%2Caps%2C729&sr=8-5'
      headers = {"User-Agent": "# YOUR USER AGENT GOES HERE", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}
      
      page = requests.get(URL, headers=headers)
      
      soup1 = BeautifulSoup(page.content, "html.parser")
   ```
6. Organize the data scraped from the website
   ```python
      soup2 = BeautifulSoup(soup1.prettify(), "html.parser")
   ```
7. Queried and cleaned "required" data from the web scrape
   ```python
      product = soup2.find(id='productTitle').get_text(strip=True)

      price_span = soup2.find('span', class_='a-price aok-align-center reinventPricePriceToPayMargin priceToPay')
      if price_span:
          whole_part = price_span.find('span', class_='a-price-whole').get_text(strip=True)
          fraction_part = price_span.find('span', class_='a-price-fraction').get_text(strip=True)
   
       price = f"{whole_part}{fraction_part}"
    
    ratings_span = soup2.find('span', class_='a-size-base a-color-base')
    ratings = ratings_span.get_text(strip=True)

   print(product)
   print(price)
   print(ratings)
   ```
8. Created a Timestamp to track when data was collected
```python
    today = datetime.date.today()
    print(today)
```
9. Created a csv file and populated it with the data we pulled and cleaned
     ```python
       import csv 
  
       header = ['Product', 'Price', 'Rating', 'Date']
       data = [product, price, ratings, today]
       
       with open('AmazonWebScraperDataset.csv', 'w', newline='',encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(header)
        writer.writerow(data)
  
       import pandas as pd
  
       df = pd.read_csv(r'C:\Users\user\AmazonWebScraperDataset.csv')
       print(df)
    ```
    ```python
        with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='UTF8') as f:
            writer = csv.writer(f)
            writer.writerow(data)
        import pandas as pd
    
        df = pd.read_csv(r'C:\Users\user\AmazonWebScraperDataset.csv')
        print(df)
    ```
10. Automate the process to pull data on a specific time frame, clean it, and add it to the csv file
    ```python
        def check_price():
    URL = 'https://www.amazon.com/Funny-Data-Systems-Business-Analyst/dp/B07FNW9FGJ/ref=sr_1_5?crid=2B4LQHJDAJHLR&dib=eyJ2IjoiMSJ9.WiKhGOLdBAacALLGC9ayNOuSFgH2acw5wZ3-xstLg4_swdoRKRSjtvVVD-eNmgait23JGAUqu0oK-D8jDjw0oaPjZ3j0poQyDL2ZxeamSjs0xqmLeBl6pagYqF4RZGE7sGRH2FOV-St2pHZjjKVX_8Rnx4RWyzGhZk-xwOva4C6alCFwePf4O0l7aJ-HhLgxSQZiLtenf5ghDJZZ-i7ZFvyPsP0__0KA0B4qKsmVAdAgMv-07nnPCiwUPa1ghSFCCy2mBxCWGCK2PrfPRYEIX1QXzfyGy41-LImlMCNDZ8E.962m5aTRMj4rk4pjp5JZ6P9Y5RYt0594NNnjEwvPpMg&dib_tag=se&keywords=data+analyst+shirt&qid=1708148038&sprefix=data+analyst+shirt%2Caps%2C729&sr=8-5'
    headers = {"User-Agent": "# YOUR USEER AGENT GOES HERE", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

    page = requests.get(URL, headers=headers)
    soup1 = BeautifulSoup(page.content, "html.parser")
    soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

    product = soup2.find(id='productTitle').get_text(strip=True)

    price_span = soup2.find('span', class_='a-price aok-align-center reinventPricePriceToPayMargin priceToPay')
    if price_span:
        whole_part = price_span.find('span', class_='a-price-whole').get_text(strip=True)
        fraction_part = price_span.find('span', class_='a-price-fraction').get_text(strip=True)
    
    price = f"{whole_part}{fraction_part}"
    
    ratings_span = soup2.find('span', class_='a-size-base a-color-base')
    ratings = ratings_span.get_text(strip=True)

    import datetime

    today = datetime.date.today()
    
    import csv 

    header = ['Product', 'Price', 'Rating', 'Date']
    data = [product, price, ratings, today]

    with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)
    ```
    ```python
        while(True):
        check_price()
        time.sleep(86400) #checks price after every 24hours and inputs data into your CSV
    ```
    ```python
        import pandas as pd

        df = pd.read_csv(r'C:\Users\user\AmazonWebScraperDataset.csv')
        print(df)
    ```
### Customization
Feel free to customize the notebook to meet your specific scraping needs. Modify headers, adapt to different Amazon product pages, or add more features as required.
  ```python
  # Example customization:
  # Add more fields to the CSV header
  header = ['Product', 'Price', 'Ratings', 'Date', 'AdditionalField']
  # Append additional data to the data list
  data = [product, price, ratings, today, additional_data]
  ```
### Contributing
Contributions are welcome! If you have ideas for improvements, bug fixes, or additional features, open an issue or submit a pull request.

.<br>
.<br>
.<br>
.<br>
<p align="center">
  <strong>#Pikkachoo 😫😁🦾</strong>
</p>
