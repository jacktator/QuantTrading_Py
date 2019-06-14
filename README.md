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
    1. [Data Visualization](#data-visualization)
1. [Trade Strategies](#trade-strategies)
    1. [Trend Following](#strategy-1-trend-following)
    1. [Mean Reversion](#strategy-2-mean-reversion)
    1. [MA + Rolling STD](#strategy-3-moving-average--standard-deviation)
    1. [Slope & Velocity](#strategy-4-slope--velocity)
1. [Testing](#testing)
    1. [Back Testing](#back-testing)
    1. [Strategy Analysis](#strategy-analysis)
1. Risk Model (Planning)
1. Transaction Cost Model (Planning)

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

Given historical prices, let's analyse it.

[TODO: Source File](./data/analysis/analysis.ipynb)

### Data Visualization

Given historical prices, let's plot it.

[TODO: Source File](./data/visualization/visualization.ipynb)

## Trade Strategies

To represent a strategy, an abstract [TradeStrategyBase](./alpha/strategies/strategy_base.ipynb) class is created. All strategies is an extension of **TradeStrategyBase**.    

### Strategy 1: Trend Following

When price rises above an absolute threshold `p_change_threshold` percent, default to 2%. 

Buy and hold for `hold_stock_threshold` days, default to 20.

[Source File](./alpha/strategies/strategy_1.ipynb) & [Demo Usage](./alpha/strategies/strategy_1_usage.ipynb).

### Strategy 2: Mean Reversion
 
When price falls consecutively for two days, and cumulatively more than `p_change_threshold` percent in total, default to 10%.

Buy and hold for `hold_stock_threshold` days, default to 10.

[Source File](./alpha/strategies/strategy_2.ipynb) & [Demo Usage](./alpha/strategies/strategy_2_usage.ipynb).

### Strategy 3: Moving Average + Standard Deviation
 
Use `_x_days` (Default 5) of data as moving average, when price rises or falls by Moving Average +- `_std_mutiplier`(Default 2.0) * Moving Standard Deviation.

Close when price moves back to moving average.

[Source File](./alpha/strategies/strategy_3.ipynb) & [Demo Usage](./alpha/strategies/strategy_3_usage.ipynb).

### Strategy 4: Slope & Velocity
 
Use close price to draw a trend, use the derivative to find the `slope`. Use the derivative again to find the `velocity` for change.

- Long when price is rising (slope > _buy_slope_threshold > 0) & accelerating (velocity > _buy_velocity_threshold > 0) and,
    - Close when price starting to fall (slope < -_close_slope_threshold < 0 and velocity < -_close_velocity_threshold < 0)
- Short when price is falling (slope < _sell_slope_threshold < 0) & accelerating (velocity < _sell_velocity_threshold < 0) and,
    - Close when price starting to rise (slope > _close_slope_threshold > 0 and velocity > _close_velocity_threshold > 0)

[Source File](./alpha/strategies/strategy_4.ipynb) & [Demo Usage](./alpha/strategies/strategy_4_usage.ipynb).

## Testing

This section covers the back testing and optimization of strategies.

### Back Testing

#### Strategy 1

Use one set of 2 parameter, `p_change_threshold` (p1) & `hold_stock_threshold` (p2).

Trading AXP, when Price rises above **3**%, buy and hold for **5** days. 

Strategy Resulted **4.85**%.

[Source File](./alpha/strategies/strategy_1_usage.ipynb) 

#### Strategy 2

Use one set of 2 parameter, `p_change_threshold` (p1) & `hold_stock_threshold` (p2).

Trading AXP, when Price falls above **5**% in 2 consecutive days, buy and hold for **3** days. 

Strategy Resulted **-2.02**%.

[Source File](./alpha/strategies/strategy_2_usage.ipynb)

#### Strategy 3

Use one set of 2 parameter, `std_mutiplier` (p1) & `x_days` (p2).

Trading AXP, take `x_days`of rolling average and standard deviation, and trade when price deviates from moving average too much.  

Strategy Resulted **1.31**%.

[Source File](./alpha/strategies/strategy_3_usage.ipynb)

#### Strategy 4

Use one set of 6 threshold parameter.

Trading AXP, buy when slope and velocity is more than 0.1 and close when both is 0. 
Sell when slope and velocity is less then -0.1 and close when both is 0

Strategy Resulted **200.62**%.

[Source File](./alpha/strategies/strategy_4_usage.ipynb)

#### Multi-Processing Backtest

Using `itertools` and `ThreadPoolExecutor` to basktest with 1024 sets of parameters, and find out which set yields best results.

Best Result: **210.019729**%. 

[Source File](alpha/strategies/backtest_1.ipynb).

Note, backtesting this way takes **VERY VERY long time**. For this case, 3:11 to complete 1024 tests. Hence the following faster method.

#### Numpy Backtet

Using `numpy` and `masks` to basktest with 1024 sets of parameters, and find out which set yields best results.

Best Result: **204.35312**%. Finished in 24:34.

[Source File](alpha/strategies/backtest_2.ipynb).

#### Namba JIT Backtet

Building upon Numpy Backtest, wrap it with Namba JIT to boost performance.

Best Result: **204.35312**%. Finished in ??.

[Source File](alpha/strategies/backtest_3.ipynb).

### Strategy Analysis

#### Linear Regression & Interpolation

Find a Linear Regression Model that describes the relationship between `date` and `price` using OLS. 

> price = k * price + b

The result is 

> price = 0.0574 * price + 85.6971

[Source File](alpha/analysis/regression.ipynb)

#### Using Monte Carlo

Utilizing Strategy4, Trading AXP, Use 10^6 sets of 6 parameters, back test all sets, and find the best 5 sets of parameters which yields the highest profit.

[TODO: Source File](alpha/strategies/).

#### Using Convex

[TODO: Source File](alpha/analysis/convex.ipynb).
