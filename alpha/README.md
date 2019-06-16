# Trade Strategies

To represent a strategy, an abstract [TradeStrategyBase](./strategies/strategy_base.ipynb) class is created. All strategies is an extension of **TradeStrategyBase**.    

## Strategy 1: Trend Following

When price rises above an absolute threshold `p_change_threshold` percent, default to 2%. 

Buy and hold for `hold_stock_threshold` days, default to 20.

[Source File](./strategies/strategy_1.ipynb) & [Demo Usage](./strategies/strategy_1_test.ipynb).

## Strategy 2: Mean Reversion
 
When price falls consecutively for two days, and cumulatively more than `p_change_threshold` percent in total, default to 10%.

Buy and hold for `hold_stock_threshold` days, default to 10.

[Source File](./strategies/strategy_2.ipynb) & [Demo Usage](./strategies/strategy_2_test.ipynb).

## Strategy 3: Moving Average + Standard Deviation
 
Use `_x_days` (Default 5) of data as moving average, when price rises or falls by Moving Average +- `_std_mutiplier`(Default 2.0) * Moving Standard Deviation.

Close when price moves back to moving average.

[Source File](./strategies/strategy_3.ipynb) & [Demo Usage](./strategies/strategy_3_test.ipynb).

## Strategy 4: Slope & Velocity
 
Use close price to draw a trend, use the derivative to find the `slope`. Use the derivative again to find the `velocity` for change.

- Long when price is rising (slope > _buy_slope_threshold > 0) & accelerating (velocity > _buy_velocity_threshold > 0) and,
    - Close when price starting to fall (slope < -_close_slope_threshold < 0 and velocity < -_close_velocity_threshold < 0)
- Short when price is falling (slope < _sell_slope_threshold < 0) & accelerating (velocity < _sell_velocity_threshold < 0) and,
    - Close when price starting to rise (slope > _close_slope_threshold > 0 and velocity > _close_velocity_threshold > 0)

[Source File](./strategies/strategy_4.ipynb) & [Demo Usage](./strategies/strategy_4_test.ipynb).

# Testing Strategies

This section covers the back testing and optimization of strategies.

## Back Testing

### Strategy 1

Use one set of 2 parameter, `p_change_threshold` (p1) & `hold_stock_threshold` (p2).

Trading AXP, when Price rises above **3**%, buy and hold for **5** days. 

Strategy Resulted **4.85**%.

[Source File](./strategies/strategy_1_test.ipynb) 

### Strategy 2

Use one set of 2 parameter, `p_change_threshold` (p1) & `hold_stock_threshold` (p2).

Trading AXP, when Price falls above **5**% in 2 consecutive days, buy and hold for **3** days. 

Strategy Resulted **-2.02**%.

[Source File](./strategies/strategy_2_test.ipynb)

### Strategy 3

Use one set of 2 parameter, `std_mutiplier` (p1) & `x_days` (p2).

Trading AXP, take `x_days`of rolling average and standard deviation, and trade when price deviates from moving average too much.  

Strategy Resulted **1.31**%.

[Source File](./strategies/strategy_3_test.ipynb)

### Strategy 4

Use one set of 6 threshold parameter.

Trading AXP, buy when slope and velocity is more than 0.1 and close when both is 0. 
Sell when slope and velocity is less then -0.1 and close when both is 0

Strategy Resulted **200.62**%.

[Source File](./strategies/strategy_4_test.ipynb)

## Strategy Optimization

Finding the Optimal Parameters for a given strategy. 

#### Multi-Processing Backtest

Using `itertools` and `ThreadPoolExecutor` to basktest with 1024 sets of parameters, and find out which set yields best results.

Best Result: **210.019729**%.

[Source File](strategies/optimization_1.ipynb).

Note, backtesting this way takes **VERY VERY long time**. For this case, 3:11 to complete 1024 tests. Hence the following faster method.

#### Numpy Backtet

Using `numpy` and `masks` to basktest with 1024 sets of parameters, and find out which set yields best results.

Best Result: **177.03**%. Finished in 24:48s.

[Source File](strategies/optimization_2.ipynb).

#### Namba JIT Backtet

Building upon Numpy Backtest, wrap it with Namba JIT to boost performance.

Best Result: **177.03**%. Finished in 15:34s.

[Source File](strategies/optimization_3.ipynb).

####  MultiProcessing + Numpy Backtet

Turbo charge Namba with MultiProcess.

Best Result: **177.03**%. Finished in 07:22s.

[Source File](strategies/optimization_4.ipynb).

## Strategy Analysis

### Linear Regression & Interpolation

Find a Linear Regression Model that describes the relationship between `date` and `price` using OLS. 

> price = k * price + b

The result is 

> price = 0.0574 * price + 85.6971

[Source File](../data/analysis/regression.ipynb)

### Using Monte Carlo

Utilizing Strategy4, Trading AXP, Use 10^6 sets of 6 parameters, back test all sets, and find the best 5 sets of parameters which yields the highest profit.

[TODO: Source File](./strategies/).

### Using Convex

[TODO: Source File](./analysis/convex.ipynb).