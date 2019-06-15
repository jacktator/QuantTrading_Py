# Stock Data

Data is King. Let's get some data.

## Modelling TradeDay

`DataClass` vs `NamedTuple`: A Battle of Performance in Python

[Source File](./models/README.md) 

## Single Stock Prices

Use Abupy to fetch Single Stock Prices as `DataFrame`. 

For example, we are fetching **AXP** (American Express).

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./historical/stock_price.ipynb) & [.csv file](./gen/usAXP_df.csv).

## Industry Stocks Prices

Use Abupy to fetch Single Stock Prices as `Panel`. 

For example, we are fetching the prices for all stocks belong to the **same industry as AXP** (American Express)

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./historical/industry_prices.ipynb) & [.csv file](./gen/fin_svs_p.csv).

## Data Analysis

Given historical prices, let's analyse it.

### Regression & Interpolation

[Source File](./analysis/regression.ipynb)

## Data Visualization

Given historical prices, let's plot it.

[TODO: Source File](./visualization/visualization.ipynb)