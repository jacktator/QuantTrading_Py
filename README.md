# QuantTrading_Py

A mini Quant Trading System with Alpha, Risks, Cost, Portfolio Construction Models.

![](./assets/quant-models.jpg)

Credit: Inside the Black Box : Summary

## Planning

This mini QuantTrading project will have the following features:

1. Fetch Historical Stock Price
2. Modeling Data
3. Data Analysis
4. Data Visualization
5. TradeStrategy x 2
6. Back Testing
7. Risk Model (Planning)
8. Transaction Cost Model (Planning)

### Fetch Historical Stock Price

### Modelling Data

### Data Analysis

### Data Visualization

### TradeStrategy x 2

#### Strategy 1

Trade Strategy 1: Trend Following.

When price rises above an absolute threshold `p_change_threshold` percent, default to 2%. 

Buy and hold for `hold_stock_threshold` days, default to 20.

[Source File](./alpha/strategies/strategy_1.ipynb) & [Demo Usage](./alpha/strategies/strategy_1_usage.ipynb).

#### Strategy 2

Trade Strategy 2: Reversion.
 
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
