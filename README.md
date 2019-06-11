# QuantTrading_Py

A mini Quant Trading System with Alpha, Risks, Cost, Portfolio Construction Models.

![](./assets/quant-models.jpg)

Credit: Inside the Black Box : Summary

## Planning

This mini QuantTrading project will have the following features:

1. [Stock Data](#stock-data)
    1. [Modelling TradeDay](#modelling-tradeday)
    1. [Single Stock Prices](#single-stock-prices)
    1. [Industry Stocks Prices](#industry-stocks-prices)
    1. [Data Analysis](#data-analysis)
        1. [Analysis](#analysis)
        1. [Visualization](#visualization)
5. [Trade Strategies](#trade-strategies)
    1. [Trend Following](#strategy-1-trend-following)
    1. [Mean Reversion](#strategy-2-mean-reversion)
6. Back Testing
7. Risk Model (Planning)
8. Transaction Cost Model (Planning)

## Stock Data

Data is King. Let's get some data.

### Modelling TradeDay

`DataClass` vs `NamedTuple`: A Battle of Performance in Python

[Source File](./data/models/README.md) 

### Single Stock Prices

Use Abupy to fetch Single Stock Prices as `DataFrame`. 

For example, we are fetching **AXP** (American Express).

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./data/historical/stock_price.ipynb) & [.csv file](./data/gen/usAXP_df.csv).

### Industry Stocks Prices

Use Abupy to fetch Single Stock Prices as `Panel`. 

For example, we are fetching the prices for all stocks belong to the **same industry as AXP** (American Express)

- Fetch Data
- Save & Read to `.csv` file
- Info & Describe

[Source File](./data/historical/industry_prices.ipynb) & [.csv file](./data/gen/fin_svs_p.csv).

### Data Analysis

#### Analysis

Given historical prices, let's analyse it.

[TODO: Source File](./data/historical/analysis.ipynb)

#### Visualization

Given historical prices, let's plot it.

[TODO: Source File](./data/historical/visualization.ipynb)

### Trade Strategies

To represent a strategy, an abstract [TradeStrategyBase](./alpha/strategies/strategy_base.ipynb) class is created. All strategies is an extension of **TradeStrategyBase**.    

#### Strategy 1: Trend Following

When price rises above an absolute threshold `p_change_threshold` percent, default to 2%. 

Buy and hold for `hold_stock_threshold` days, default to 20.

[Source File](./alpha/strategies/strategy_1.ipynb) & [Demo Usage](./alpha/strategies/strategy_1_usage.ipynb).

#### Strategy 2: Mean Reversion
 
When price falls consecutively for two days, and cumulatively more than `p_change_threshold` percent in total, default to 10%.

Buy and hold for `hold_stock_threshold` days, default to 10.

[Source File](./alpha/strategies/strategy_2.ipynb) & [Demo Usage](./alpha/strategies/strategy_2_usage.ipynb).

## Structure:

assets
data

## TOC:

- Alpha Models
- Risk Models
- Cost Models
- Portfolio Construction Models
