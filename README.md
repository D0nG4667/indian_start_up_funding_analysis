# Indian Start-up Funding Analysis

## Overview

This repository contains data analysis and insights into the funding landscape of Indian start-ups from 2018 to 2021. Leveraging detailed datasets for each year, including start-up profiles, funding amounts, and investor information, this project aims to uncover trends, patterns, and actionable insights within the Indian start-up ecosystem.

## Key Objectives

- Analyze funding trends and dynamics within the Indian start-up ecosystem.
- Examine sector-wise distribution of funding and geographical trends.
- Identify prominent investors and their impact on start-up funding.
- Provide valuable insights to inform strategic decision-making for investors, entrepreneurs, and stakeholders.

## Framework

The CRoss Industry Standard Process for Data Mining (CRISP-DM).

## Features

- Jupyter Notebook containing data analysis, visualizations, and interpretation.
- Detailed documentation outlining methodology, data sources, and analysis results.
- Interactive visualizations in Power BI showcasing funding trends and key insights.

### PowerBI Dashboard

![Dashboard](/screenshots/dashboard.png)

## Technologies Used

- Anaconda
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- Jupyter Notebooks
- Git
- Scipy
- Geopandas
- Geoplot
- Folium
- Geopy
- Nltk

## Installation

### Quick install

```bash
conda install jupyter numpy pandas pyodbc python-dotenv matplotlib seaborn plotly scipy geopandas geoplot folium geopy nltk
```

or

```bash
pip install --quiet jupyter numpy pandas pyodbc python-dotenv matplotlib seaborn plotly scipy geopandas geoplot folium geopy nltk
```

### Recommended install

```bash
conda env create -f isfa_environment.yml
```

## Sample Code- used to clean the amount column

```python
def floater(string):
    """
    Converts a string to a float.

    Parameters:
    string (str): The string to be converted.

    Returns:
    string_float: The converted float value, or NaN if conversion fails.
    """
    try:
        string_float = float(string)
    except ValueError:
        string_float = np.nan
    
    return string_float
    
def clean_amount(row):
    """
    Cleans the amount values.

    Parameters:
    row (pandas.Series): Treats index 0 as amount to be cleaned and index 1 as 'year'.

    Returns:
    amount: The cleaned amount value as a float, or NaN if amount is empty or cannot be cleaned.
    """ 
    amount = row[0]  # Expects index 0 to be amount or treats index 0 as amount and also in cases where investor or stage values may contain amounts  
    
    year   = row[1]  # year = row['year'], row.index=['amount', 'year] 
    
    # Source: https://www.ofx.com/en-au/forex-news/historical-exchange-rates/yearly-average-rates/
    exchange_rates = {
        2018: 0.014649,
        2019: 0.014209,
        2020: 0.013501,
        2021: 0.013527
    }
    
    exchange_rate = exchange_rates[year]   
    
    # Convert to string
    amount = str(amount)   
    
    if isinstance(amount, str):        
        # Set of elements to replace
        to_replace = {' ', ','}

        # Replace each element in the set with an empty string
        for r in to_replace:
            amount = amount.replace(r, '')        
                        
        if amount == '' or amount == '—': 
            amount = np.nan
        # If the amount is in INR (Indian Rupees), convert it to USD using the conversion rate of the year
        elif '₹' in amount:
            amount = amount.replace('₹', '')
            amount = floater(amount) * exchange_rate
        
        # If the amount is in USD, remove the '$' symbol and convert it to a float
        elif '$' in amount:
            amount = amount.replace('$', '')
            amount = floater(amount)
        else:
            amount = floater(amount)

    
    return amount

```

## Contributions

### How to Contribute

1. Fork the repository and clone it to your local machine.
2. Explore the Jupyter Notebooks and documentation.
3. Implement enhancements, fix bugs, or propose new features.
4. Submit a pull request with your changes, ensuring clear descriptions and documentation.
5. Participate in discussions, provide feedback, and collaborate with the community.

## Feedback and Support

Feedback, suggestions, and contributions are welcome! Feel free to open an issue for bug reports, feature requests, or general inquiries. For additional support or questions, you can connect with me on [LinkedIn](https://www.linkedin.com/in/dr-gabriel-okundaye).

Link to article on Medium: [From Rogue Data to Discovery: Exploring India's Startup Ecosystem - A data-driven approach](https://medium.com/@gabriel007okuns/from-rogue-data-to-discovery-exploring-indias-startup-ecosystem-a-data-driven-approach-93f3425e5ef2)

## Author

[Gabriel Okundaye](https://www.linkedin.com/in/dr-gabriel-okundaye).

## License

[MIT](/LICENSE)
