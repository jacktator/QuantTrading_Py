# Stock Data

Data is King. Let's get some data.

## Modelling TradeDay

`DataClass` vs `NamedTuple`: A Battle of Performance in Python

[Source File](./data/models/README.md) 

## Single Stock Prices

Use Abupy to fetch Single Stock Prices as `DataFrame`. 

For example, we are fetching **AXP** (American Express).

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./data/historical/stock_price.ipynb) & [.csv file](./data/gen/usAXP_df.csv).

## Industry Stocks Prices

Use Abupy to fetch Single Stock Prices as `Panel`. 

For example, we are fetching the prices for all stocks belong to the **same industry as AXP** (American Express)

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./data/historical/industry_prices.ipynb) & [.csv file](./data/gen/fin_svs_p.csv).

## Data Analysis

Given historical prices, let's analyse it.

[TODO: Source File](./data/analysis/analysis.ipynb)

## Data Visualization

Given historical prices, let's plot it.

[TODO: Source File](./data/visualization/visualization.ipynb)