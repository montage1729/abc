
# Algorithmic Trading Assignment Report

## Subtask 3 of Assignment 1 - COP290

**Submitted by:**
- Soumyaprabha Dey (Entry Number: 2022CS11107)
- Ayush Gupta (Entry Number: 2022CS11114)

## Overview

In this assignment, we implemented various algorithmic trading strategies in C++ using historical stock price data. The primary goal was to generate daily cash flow and order statistics for different trading strategies. Each strategy was implemented following specific rules and constraints provided in the assignment document.

## Implementation Details

### Code Structure

The codebase is organized in a modular and clean manner, adhering to C++ best practices. Each trading strategy is implemented as a separate module, making it easy to understand and modify. The main components include:

- **Data Handling:** Reading stock price data from CSV files and organizing it for strategy execution.
- **Strategy Implementation:** Separate modules for each trading strategy, such as Basic, DMA, DMA++, MACD, RSI, ADX, Linear Regression, Pairs Trading, and Stop-Loss in Pairs Trading.
- **Result Output:** Writing daily cash flow, order statistics, and final PnL to CSV and text files.

### File Format Choice

CSV files were chosen to store daily cash flow and order statistics due to their simplicity and compatibility with various data analysis tools. The final PnL was stored in a text file for easy readability.

### Testing and Validation

Each strategy was thoroughly tested using different stocks and time periods. The results were compared against baseline results provided, ensuring the correctness of the implementation.

## Strategy Insights

### Basic Strategy

#### Insights and Intuitions for BASIC Strategy Implementation

#### Overview

The BASIC strategy is a fundamental algorithmic trading approach based on the observation of price trends over a specified period. The key actions involve buying if prices have been monotonically increasing for the last 'n' days and selling if prices have been monotonically decreasing. 

##### 1. Monotonic Trends

- **Buying Decision:** If the price has been consistently increasing for the past 'n' days, the strategy assumes a positive trend. The decision to buy reflects an expectation that the upward momentum will continue.

- **Selling Decision:** Conversely, if the price has been consistently decreasing for the past 'n' days, the strategy assumes a negative trend. The decision to sell is based on the anticipation of further price decreases.

##### 2. Constraints and Assumptions

- **Short-Selling Allowed:** The strategy accommodates short-selling, allowing for a short position in addition to long positions.

- **Position Limits:** The maximum and minimum positions are restricted to '+x' and '-x', respectively, ensuring controlled exposure to the market.

##### 3. Data Preparation

- **Past Data Inclusion:** As per the assumption, the strategy requires past 'n' days' data even for the start date. Python is used to generate the appropriate data file.

##### 4. Implementation Details

- **Price Trend Analysis:** The strategy involves analyzing the price trend over the specified period using increasing and decreasing counters.

- **Daily Cash Flow:** The algorithm maintains a record of daily cash flow, reflecting the impact of buying and selling decisions on available funds.

- **Order Statistics:** Order statistics are recorded only on days when a trade is made, providing insights into the timing and nature of transactions.

- **Position Squaring Off:** The strategy ensures positions are squared off at the closing price of the end date, adhering to the no-holding policy.

##### 5. Strategy Execution

- **Buy and Sell Conditions:** The strategy executes buy or sell actions based on the monotonic trend conditions. The number of shares bought or sold is limited by the specified position limits.

- **Cash Flow Calculation:** The cash flow is adjusted with each transaction, providing a running total of profits or losses.

- **End Date Squaring Off:** The strategy ensures positions are squared off at the closing price of the end date, finalizing the overall profit or loss.

##### 6. Outcome

- **File Output:** The implementation generates two CSV files - daily_cashflow.csv and order_statistics.csv, containing relevant information about the trading activities.

- **Final Profit/Loss:** The final profit or loss is recorded in the final_pnl.txt file, summarizing the overall performance of the BASIC strategy.

##### 7. Intuition

- **Trend Exploitation:** The strategy capitalizes on identified trends, aiming to benefit from the continuation of established price patterns.

- **Risk Management:** The position limits and squaring-off mechanism contribute to risk management, preventing excessive exposure to market fluctuations.

- **Simplicity:** The simplicity of the strategy makes it a good starting point, especially for those new to algorithmic trading.



In conclusion, the BASIC strategy, while simple, lays the foundation for more complex trading algorithms. It emphasizes trend analysis and controlled position management, providing valuable insights into the dynamics of algorithmic trading.
It emphasizes trend analysis and controlled position management, providing valuable insights into the dynamics of algorithmic trading.


### n-Day Moving Average (DMA)

#### Insights and Intuitions for DMA (n-Day Moving Average) Strategy Implementation

#### Overview

The DMA strategy extends the basic strategy by relaxing the monotonic assumption and introducing the concept of the n-day moving average (DMA). This strategy involves calculating the mean price of the past 'n' days and the standard deviation. Trading decisions are then made based on the current price's deviation from the DMA by a specified number of standard deviations ('p'). Below are insights and intuitions regarding the DMA strategy.

##### 1.Trend Identification

- **Buy Signal:** The strategy generates a buy signal when the current price exceeds the DMA by a significant margin, specifically by 'p' standard deviations. This is interpreted as a positive trend, with expectations of upward momentum.

- **Sell Signal:** Conversely, a sell signal is triggered when the current price falls below the DMA by 'p' standard deviations. This indicates a potential downward trend, prompting the strategy to sell.

##### 2. Inclusion of Standard Deviation

- **Confidence Level:** The introduction of standard deviation ('p') adds a confidence level to the trend identification process. A higher 'p' implies a greater required deviation from the mean for a buy/sell signal, making the strategy more conservative.

##### 3. Data Preparation

- **Moving Average Calculation:** The DMA and standard deviation are calculated over a rolling window of 'n' days. This ensures adaptability to changing market conditions.

##### 4. Implementation Details

- **Dynamic Mean and Standard Deviation:** The DMA and standard deviation are calculated dynamically as the window moves through the historical data. This provides a responsive measure of the prevailing market conditions.

- **Buy and Sell Execution:** The strategy executes buy or sell actions based on the calculated DMA, current price, and specified standard deviation threshold ('p'). Position limits are respected to control exposure.

- **Cash Flow Management:** Daily cash flow is updated with each transaction, reflecting the impact of buying and selling decisions on available funds.

#####   5. Squaring Off Positions

- **End Date Squaring Off:** Similar to the basic strategy, positions are squared off at the closing price of the end date, ensuring a clear assessment of the final profit or loss.

##### 6. Outcome

- **File Output:** The implementation generates two CSV files - daily_cashflow.csv and order_statistics.csv, providing insights into daily trading activities.

- **Final Profit/Loss:** The final profit or loss is recorded in the final_pnl.txt file, summarizing the overall performance of the DMA strategy.

##### 7. Salient Features in the Code

- **Dynamic Window Calculation:** The rolling window for DMA and standard deviation calculation ensures adaptability to changing market dynamics.

- **Confidence Level Adjustment:** The 'p' parameter allows traders to adjust the confidence level, influencing the sensitivity of the strategy to price deviations.

- **Position Limit Management:** The strategy adheres to position limits, preventing excessive exposure and contributing to risk management.

In conclusion, the DMA strategy introduces a more flexible approach to trend identification by incorporating moving averages and standard deviations. It aims to capture potential trends with added confidence, providing a nuanced strategy for algorithmic trading.

### Improved DMA (DMA++)

#### Insights and Intuitions for Improved DMA (DMA++) Strategy Implementation

#### Overview

The Improved DMA (DMA++) strategy enhances the basic DMA strategy by introducing two key features - stop-loss and a smoothing factor for adaptive Moving Average (AMA) calculation. These additions aim to refine the strategy's decision-making process and improve overall performance. Below are insights and intuitions regarding the DMA++ strategy.

##### 1. Stop-Loss Implementation

- **Forceful Position Closure:** To prevent significant losses, the strategy implements a stop-loss mechanism. If positions cannot be closed within a specified time (max hold days), they are forcefully closed to limit potential losses.

- **Adaptation to Market Conditions:** The stop-loss feature adds adaptability to changing market conditions, preventing prolonged exposure to unfavorable trends.

##### 2. Smoothing Factor for Adaptive Moving Average (AMA)

- **Efficiency Ratio (ER):** The strategy calculates ER based on the change in price over the last 'n' days and the sum of absolute price changes over the same period. This ratio quantifies the effectiveness of price movements.

- **Smoothing Factor (SF):** SF is dynamically calculated, incorporating the calculated ER. It is designed to smooth out erratic price movements and provide a more adaptive AMA.

- **AMA Calculation:** The AMA is updated using the SF, adjusting the moving average based on recent price changes. This adaptive approach aims to capture trends more effectively.

##### 3. Trading Decisions

- **Buy Signal:** A buy signal is generated when the current price is greater than the AMA by a specified percentage ('p'). The buy decision is refined by considering the cumulative change in prices.

- **Sell Signal:** Conversely, a sell signal is triggered when the current price is less than the AMA by a specified percentage ('p'). This decision is further influenced by the cumulative change, introducing an additional layer of confirmation.

- **Queue Mechanism:** The strategy maintains a queue to manage forceful position closures, ensuring the earliest positions are closed first.

##### 4. Position Management

- **Earliest Position Closure:** When forced to close a position, the strategy prioritizes closing the one that was bought earliest. This ensures a fair and logical approach to position management.

##### 5. Parameters to the Strategy

- **x:** Maximum position allowed.
- **p:** Percentage change needed for buy/sell signals.
- **n:** Number of past days needed to calculate ER.
- **max hold days:** Maximum number of days a position can be held.
- **c1, c2:** Parameters for calculating SF.
  
##### 6. Salient Features in the Code

- **Dynamic Parameter Calculation:** ER, SF, and AMA are dynamically calculated based on recent price changes, adapting to evolving market conditions.

- **Forceful Position Closure:** The strategy uses a queue mechanism to force the closure of positions if not closed within the specified time frame.

- **Position Limit Management:** The strategy adheres to position limits, preventing excessive exposure and contributing to risk management.

- **Adaptive Trend Identification:** By incorporating ER and SF, the strategy aims to identify trends more adaptively and make informed buy/sell decisions.

In conclusion, the Improved DMA (DMA++) strategy adds sophistication to the basic DMA approach, providing enhanced risk management and adaptability to dynamic market conditions.


### MACD

#### Insights and Intuitions for MACD (Moving Average Convergence Divergence) Strategy Implementation

#### Overview

The MACD strategy is a trend-following algorithm that utilizes the concept of moving averages to generate buy and sell signals based on the convergence or divergence of short-term and long-term exponential weighted means (EWM). Below are insights and intuitions regarding the MACD strategy implementation.

##### 1. Exponential Weighted Means (EWM) Calculation

- **Short EWM (12 days):** Calculates the exponential weighted mean of prices over the past 12 days, representing the short-term trend.

- **Long EWM (26 days):** Calculates the exponential weighted mean of prices over the past 26 days, representing the long-term trend.

##### 2. MACD Calculation

- **MACD Formula:** MACD is computed as the difference between the Short EWM and Long EWM. It indicates the convergence or divergence of short and long-term trends.

##### 3. Signal Line Calculation

- **Signal Line:** Calculates the EWM of MACD over the past 9 days, providing a smoother line to identify potential buy/sell signals.

##### 4. Trading Decisions

- **Buy Signal:** Generated when MACD is greater than the Signal Line. A buy decision involves purchasing one share.

- **Sell Signal:** Generated when MACD is less than the Signal Line. A sell decision involves selling one share.

##### 5. Position Management

- **Position Size (x):** Defines the maximum position allowed.

- **Cashflow Tracking:** Maintains a record of cashflow based on buy and sell decisions.

##### 6. Salient Features in the Code

- **Dynamic Parameter Calculation:** EWM, MACD, and Signal Line are dynamically calculated based on historical price data.

- **Buy/Sell Decision Logic:** The strategy's decisions are rooted in the comparison between MACD and the Signal Line, providing a straightforward approach to trend identification.

- **Output Generation:** Produces daily cashflow records and order statistics in CSV files, facilitating performance analysis.

- **Parameter Customization:** Configurable parameters include the maximum position allowed (x), start date, and end date.

In conclusion, the MACD strategy aims to capitalize on trend movements by identifying convergence and divergence in short and long-term moving averages. The strategy provides a systematic approach to decision-making, enhancing trend-following capabilities.

### Relative Strength Index (RSI)

#### Insights and Intuitions for RSI (Relative Strength Index) Strategy Implementation

#### Overview

The RSI strategy is a popular indicator-based algorithm that leverages the Relative Strength Index to identify potential trend reversal points. Here are insights and intuitions regarding the RSI strategy implementation.

##### 1. Relative Strength Index (RSI) Calculation

- **Average Gain/Loss:** Computes the average gain and loss over the last n days, considering the positive (gain) and negative (loss) changes in closing prices.

- **Relative Strength (RS):** Calculates RS as the ratio of average gain to average loss.

- **RSI Formula:** RSI is then calculated using the formula RSI = 100 - (100 / (1 + RS)). RSI values range from 0 to 100.

##### 2. Trading Decisions

- **Buy Signal:** Generated when RSI crosses below the oversold threshold. A buy signal indicates a potential reversal from a downtrend.

- **Sell Signal:** Generated when RSI crosses above the overbought threshold. A sell signal indicates a potential reversal from an uptrend.

##### 3. Position Management

- **Position Size (x):** Defines the maximum position allowed.

- **Cashflow Tracking:** Maintains a record of cashflow based on buy and sell decisions.

##### 4. Salient Features in the Code

- **Dynamic Parameter Calculation:** Calculates RSI, average gain, and average loss dynamically based on historical price data.

- **Buy/Sell Decision Logic:** The strategy's decisions are rooted in RSI values, providing a systematic approach to identify potential trend reversal points.

- **Output Generation:** Produces daily cashflow records and order statistics in CSV files, facilitating performance analysis.

- **Parameter Customization:** Configurable parameters include the maximum position allowed (x), the number of days for RSI calculation (n), oversold threshold, overbought threshold, start date, and end date.

##### 5. Visualization Tip

- **Graphical Analysis:** The documentation suggests visualizing stock prices and RSI graphs to gain a better understanding of the strategy's performance.

In conclusion, the RSI strategy aims to capture potential trend reversals by monitoring the relative strength of gains and losses. The strategy provides a systematic approach to decision-making, enhancing its utility for trend analysis.


### ADX (Average Directional Index)

#### Insights and Intuitions for ADX (Average Directional Index) Strategy Implementation

#### Overview

The ADX strategy is designed to identify potential trend strength based on the Average Directional Index. Here are insights and intuitions regarding the ADX strategy implementation.

##### 1. True Range (TR) Calculation

- **Definition:** TR is calculated as the maximum of High-Low, High-Previous Close, Low-Previous Close.

##### 2. Directional Movement Calculation

- **DM+ and DM- Calculation:** DM+ is the positive directional movement, and DM- is the negative directional movement. They are determined by comparing today's and yesterday's high and low prices.

##### 3. Average True Range (ATR) Calculation

- **ATR Formula:** ATR is the Exponential Weighted Mean (EWM) of TR over the past n days, providing a measure of price volatility.

##### 4. Directional Index Calculation

- **DI+ and DI- Calculation:** DI+ and DI- are calculated as the EWM of DM+ and DM- divided by ATR.

##### 5. DX and ADX Calculation

- **DX Formula:** DX is computed using the relative difference between DI+ and DI-, normalized to a percentage scale.

- **ADX Calculation:** ADX is the EWM of DX over the past n days, indicating the strength of the prevailing trend.

##### 6. Trading Decisions

- **Buy Signal:** Generated when ADX is greater than the specified adx_threshold, indicating a strong uptrend.

- **Sell Signal:** Generated when ADX is less than the specified adx_threshold, suggesting a weaker or sideways trend.

##### 7. Position Management

- **Position Size (x):** Defines the maximum position allowed.

- **Cashflow Tracking:** Maintains a record of cashflow based on buy and sell decisions.

##### 8. Salient Features in the Code

- **Dynamic Parameter Calculation:** Calculates TR, DM+, DM-, ATR, DI+, DI-, DX, and ADX dynamically based on historical price data.

- **Buy/Sell Decision Logic:** The strategy's decisions are rooted in the ADX values, providing a systematic approach to identify potential trend strength.

- **Output Generation:** Produces daily cashflow records and order statistics in CSV files, facilitating performance analysis.

- **Parameter Customization:** Configurable parameters include the maximum position allowed (x), the number of days for ATR and ADX calculation (n), ADX threshold, start date, and end date.

In conclusion, the ADX strategy aims to capture the strength of trends by considering directional movements, true range, and volatility. The strategy provides a systematic approach to decision-making, enhancing its utility for trend analysis.


### Linear Regression

#### Insights and Intuitions for Linear Regression Strategy Implementation

#### Overview

The Linear Regression strategy utilizes machine learning, specifically Linear Regression, to predict stock prices and make trading decisions based on the predicted values. Here are insights and intuitions regarding the Linear Regression strategy implementation.

##### 1. Linear Regression Equation

- **Equation:** The strategy uses a linear regression equation to predict the closing price at day t based on historical data. The equation involves coefficients (β0 to β7) corresponding to different features like previous close, open at t-1, VWAP at t-1, low at t-1, high at t-1, the number of trades at t-1, and open at t.

##### 2. Training the Model

- **Data Selection:** Historical data from `train_start_date` to `train_end_date` is used to train the linear regression model.

- **Normal Equations:** The implementation uses the normal form equations of linear regression for training, emphasizing consistency and ease of autograding.

##### 3. Prediction and Trading Decisions

- **Prediction:** The trained model is then used to predict the close price for the current day.

- **Buy/Sell Decision:** If the predicted price is greater than the actual price by `p%`, a buy signal is generated. Conversely, if the predicted price is less than the actual price by `p%`, a sell signal is generated.

##### 4. Position Management

- **Position Size (x):** Defines the maximum and minimum positions allowed. The position is constrained to stay within [-x, +x].

##### 5. Output Generation

- **CSV Files:** The strategy generates two CSV files - `daily_cashflow.csv` and `order_statistics.csv`.

- **Daily Cashflow:** Records daily cashflow, reflecting the impact of buy/sell decisions.

- **Order Statistics:** Keeps track of order direction, quantity, and price.

##### 6. Salient Features in the Code

- **Dynamic Feature Handling:** The code dynamically handles different features for both training and prediction.

- **Normalization of Dates:** Ensures consistency by normalizing date formats during data processing.

- **Output File Generation:** The implementation produces files facilitating easy analysis and verification of strategy performance.

##### Conclusion

The Linear Regression strategy leverages machine learning to make informed trading decisions based on predicted stock prices. By using historical data for training and normal equations for consistency, the strategy provides a systematic approach to capturing potential market trends.

### Best of All Strategy
#### Insights and Intuitions for BEST_OF_ALL Strategy Implementation

#### Overview

The BEST_OF_ALL strategy combines multiple individual strategies by running them in parallel and selects the one with the highest overall Profit and Loss (PnL). This approach aims to capitalize on the strengths of different strategies and adapt to varying market conditions. Below are insights and intuitions regarding the BEST_OF_ALL strategy implementation.

##### 1. Parallel Execution

- **Parallelization:** The strategy employs OpenMP to run multiple strategies concurrently, improving efficiency and reducing execution time.

- **Strategy Independence:** Each strategy behaves independently, and their execution does not depend on the completion of others. This enables a faster overall computation.

##### 2. Strategy Combination

- **Included Strategies:** The BEST_OF_ALL strategy includes MACD, RSI, ADX, BASIC, DMA, DMA++, and LINEAR_REGRESSION. Each strategy runs with its predefined parameters.

- **Picking the Best:** After parallel execution, the strategy identifies the individual strategy with the highest overall PnL and selects it for detailed analysis and output generation.

##### 3. Parameter Consistency

- **Consistent Parameters:** To maintain consistency and ensure fair comparison, the strategy uses consistent parameters for each individual strategy. These include window sizes (n), maximum position size (x), thresholds, and other relevant parameters.

- **Common Training Period:** Linear Regression strategy uses a consistent one-year past data for training, guaranteeing that start date and end date lie in the same year.

##### 4. Output Generation

- **CSV Files:** Like other strategies, BEST_OF_ALL generates two CSV files - `daily_cashflow.csv` and `order_statistics.csv`. These files provide insights into the overall performance of the selected strategy.

##### 5. Dynamic Strategy Selection

- **Dynamic Strategy Invocation:** The selected strategy for detailed analysis is determined dynamically based on the one with the highest PnL. This allows for flexibility and adaptation to varying market conditions.

- **Fallback Execution:** If the selected strategy is one of the original individual strategies, it is rerun separately for more detailed analysis.

#### 6. Salient Features in the Code

- **Threaded Parallel Sections:** OpenMP is used with the `parallel sections` pragma, allowing multiple sections to run in parallel.

- **Efficient Strategy Selection:** The code efficiently identifies the strategy with the highest PnL without waiting for all strategies to complete their execution.

#### Conclusion

The BEST_OF_ALL strategy takes advantage of parallel computing to run multiple strategies concurrently, dynamically selects the best-performing strategy, and provides a comprehensive approach to trading by combining the strengths of different individual strategies.


### Pairs Trading

#### Insights and Intuitions for Pairs Trading Strategy Implementation

#### Overview

The Pairs Trading Strategy focuses on trading pairs of stocks by analyzing the spread between their prices. Here are key insights and intuitions regarding the implementation of this strategy:

##### 1. Conceptual Foundation

- **Spread Calculation:** The strategy defines the spread as the price difference between two stocks: Spread_t = Price_Stock1,t - Price_Stock2,t.

- **Z-Score Calculation:** Z-Score is a standardized measure calculated as (Spread - Rolling Mean) / Rolling Std Dev. It quantifies how far the current spread is from its historical mean in terms of standard deviations.

##### 2. Trading Signals

- **Sell Signal:** Generated when Z-Score > Threshold. Implies selling Stock1 and buying Stock2.

- **Buy Signal:** Generated when Z-Score < -Threshold. Implies buying Stock1 and selling Stock2.

##### 3. Position Management

- **Maximum Position Size:** The position "in the spread" is restricted to +x (long) and -x (short) for each stock. This constraint ensures risk management and position control.

##### 4. Parameter Configuration

- **Lookback Period (n):** Determines the number of past days considered for calculating the rolling mean and standard deviation.

- **Z-Score Threshold:** Defines the level at which trading signals are triggered. A higher threshold may lead to fewer but potentially stronger signals.

##### 5. Spread Dynamics

- **Mean-Reverting Nature:** The strategy assumes mean-reversion in the spread. High Z-Score suggests an opportunity to profit from the expected return of the spread to its mean.

- **Pairs Dynamics:** The strategy leverages the relative performance of the two stocks rather than their absolute price movements.

##### 6. Output Generation

- **CSV Files:** The strategy generates three CSV files - `daily_cashflow.csv`, `order_statistics_1.csv` (for Stock1), and `order_statistics_2.csv` (for Stock2).

##### 7. Salient Features in the Code

- **Parallel Spread Calculation:** The spread between two stocks is calculated in parallel, improving efficiency.

- **Efficient Position Tracking:** The code tracks the position and generates orders based on Z-Score signals.

- **Dynamic Strategy Execution:** The strategy adheres to parameterized values such as position size, lookback period, and threshold.

#### Conclusion

The Pairs Trading Strategy offers a unique approach by focusing on the spread between two stocks, allowing traders to capitalize on mean-reverting dynamics. The implementation includes robust risk management and efficiently handles trading signals and position management.

### Stop-Loss in Pairs Trading

#### Insights and Intuitions for Stop-Loss in Pairs Trading Strategy

#### Overview

The Stop-Loss in Pairs Trading Strategy introduces a loss-based constraint to manage unwanted positions in pairs trading. Here are key insights and intuitions regarding the implementation of this stop-loss strategy:

##### 1. Rationale for Stop-Loss

- **Unwanted Positions:** In pairs trading, if the spread moves unexpectedly in the wrong direction, we may want to clear positions to make room for new opportunities and prevent further losses.

- **Stop-Loss Threshold:** The strategy employs a stop_loss_threshold to close positions when the z-score crosses this threshold in the opposite direction to the expected mean reversion.

##### 2. Stop-Loss Calculation

- **Same Mean and Std Dev:** The stop-loss threshold uses the same mean and standard deviation calculated at the time of opening the position. This ensures consistency in evaluating unexpected spread movements.

- **Closing Single Position:** The stop-loss triggers the closing of the single position when the z-score crosses the stop_loss_threshold.

##### 3. Constraints and Assumptions

- **Consistency:** The strategy maintains consistency with the assumptions and constraints of the Pairs Trading Strategy, including maximum position size and mean-reverting dynamics.

##### 4. Parameter Configuration

- **Additional Parameter:** The stop_loss_threshold is an additional parameter introduced to control when to close positions based on unexpected spread movements.

##### 5. Output Generation

- **CSV Files:** Similar to the Pairs Trading Strategy, the stop-loss strategy generates three CSV files - `daily_cashflow.csv`, `order_statistics_1.csv` (for Stock1), and `order_statistics_2.csv` (for Stock2).

##### 6. Salient Features in the Code

- **Dynamic Stop-Loss Execution:** The stop-loss strategy dynamically closes positions based on z-score movements, preventing prolonged exposure to unfavorable spread dynamics.

- **Integrated with Pairs Trading:** The stop-loss is seamlessly integrated into the Pairs Trading Strategy, enhancing risk management.

#### Conclusion

The Stop-Loss in Pairs Trading Strategy provides a mechanism to exit unwanted positions based on z-score movements, ensuring adaptive risk management. By introducing this stop-loss mechanism, the strategy enhances its robustness and flexibility in responding to unexpected market conditions.



## Graphs

We have drawn graphs of cashflow vs date for all the strategies that we have implemented 

## Conclusion

The implementation of various trading strategies provides a comprehensive understanding of algorithmic trading concepts. Each strategy has its strengths and weaknesses, and the choice of strategy depends on market conditions and risk tolerance. The use of machine learning (Linear Regression) and statistical indicators (RSI, ADX) adds sophistication to the trading approach. The Pairs Trading strategy, coupled with stop-loss mechanisms, demonstrates risk management practices in algorithmic trading.

