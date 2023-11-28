# Web Scraping and ETL Project: World's Largest Banks

This repository contains a Python script for extracting, transforming, and loading (ETL) data related to the world's largest banks. The script utilizes web scraping techniques to gather information from a Wikipedia page, performs necessary data transformations, and stores the results in both a CSV file and a SQLite database.

## Overview

The project is divided into the following components:

1. **Extract (`extract` function):** The `extract` function uses BeautifulSoup to scrape data from a specified Wikipedia page and creates a DataFrame with relevant information such as bank names and market capitalization in USD billions.

   ```python
   def extract(url, table_attribs):
    page = requests.get(url).text
    data = BeautifulSoup(page,'html.parser')
    df = pd.DataFrame(columns=table_attribs)
    tables = data.find_all('tbody')
    rows = tables[0].find_all('tr')

    for row in rows:
        if row.find('td') is not None:
            col = row.find_all('td')
            bank_name = col[1].find_all('a')[1]['title']
            market_cap = col[2].contents[0][:-1]
            data_dict = {"Name":bank_name,
                         "MC_USD_Billion": float(market_cap)}
            df1 = pd.DataFrame(data_dict, index=[0])
            df = pd.concat([df,df1], ignore_index=True)
    return df

   ```

3. **Transform (`transform` function):** The `transform` function accesses a CSV file containing exchange rate information and adds three columns to the DataFrame. These columns represent the transformed market capitalization values in GBP, EUR, and INR (scaled by the corresponding exchange rate factors).

4. **Load to CSV (`load_to_csv` function):** The `load_to_csv` function saves the final DataFrame as a CSV file in the specified output path.

5. **Load to Database (`load_to_db` function):** The `load_to_db` function saves the final DataFrame to a SQLite database table with the provided name.

6. **Run Query (`run_query` function):** The `run_query` function executes SQL queries on the database table and prints the results to the terminal.

7. **Logging (`log_progress` function):** The `log_progress` function logs messages at different stages of the ETL process to a log file (`code_log.txt`).

## Usage

1. Install the required libraries:

    ```bash
    python3.11 -m pip install pandas
    python3.11 -m pip install numpy
    python3.11 -m pip install bs4
    ```

2. Run the script:

    ```bash
    python3.11 etl_script.py
    ```

## Additional Information

- The `url` variable in the script contains the link to the Wikipedia page from which data is being scraped. Make sure to update it if needed.

- The SQLite database is created with the name `Banks.db`, and the table within it is named `Largest_banks`.

- The CSV file is saved in the current working directory with the name `Largest_banks_data.csv`.

- The script logs progress messages to `code_log.txt`. 
