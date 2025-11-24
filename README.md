# Financial Market Analysis with a Finite State Machine (FSM)

## Back Testing
![Backtesting Annual Return for forex algo-trading](https://github.com/user-attachments/assets/31a3b66a-ce75-4d5f-9141-7468ae648104)

## Live Testing July - Sep 2025
![Livetesting Return for forex algo-trading](https://github.com/user-attachments/assets/2edc08a1-7c3b-428d-bdcb-ae3892a55948)

### Live Testing Cumulative PnL
![Live Testing Cumulative PnL](https://github.com/user-attachments/assets/fa6bdba0-4bdf-48fd-9daf-2b8c6ceaeaa7)


This repository hosts algorithmic trading strategies for forex and stock market, blending sophisticated technical analysis with robust statistical methods. The core of this project is a Finite State Machine (FSM) designed to identify high-probability trading setups based on market structure, trend dynamics, and Fibonacci levels.

For seamless integration, the repository includes code compatible with the Algogene trading platform. Additionally, a comprehensive Jupyter Notebook (`Visualisation_of_auto_technical_analysis_forex`) is provided for in-depth local backtesting, strategy validation, and detailed trade visualization.

---

## Algogene - Forex Strategy Overview

The FSM implements a classic market structure and Fibonacci-based trading strategy:

1.  **Identify Trend**: Wait for the market to establish a clear trend (e.g., EMA_30 > EMA_120 for a bullish trend).
2.  **Wait for Pullback**: Detect a counter-trend pullback.
3.  **Confirm Break of Structure (BOS)**: Wait for the price to break the previous market structure, confirming the trend's continuation. For a bearish trend, this would be a new Lower Low (LL).
4.  **Draw Fibonacci**: Once a new leg is formed (e.g., from a Lower High to a Lower Low), draw Fibonacci retracement levels.
5.  **Signal Entry**: Wait for the price to retrace to a key Fibonacci level (e.g., 0.618).

6.  **Confirm Entry**: Enter the trade upon the formation of a confirmation pattern, such as a bearish or bullish engulfing candle.
7.  **Z-Index Trade Filtering**: Use statistical metrics, e.g. z-index to filter out trades with a low probability of success, based on historical data.
8.  **Manage Trade**: Set a stop-loss based on ATR and a take-profit based on a fixed risk/reward ratio. The FSM monitors the trade until one of these levels is hit.

---

## About local jyputer notebook

- **Trend Analysis**: Automatically determines the market trend (bullish/bearish) using Exponential Moving Average (EMA) crossovers.
- **Stateful Trading Logic**: Implements a `TradingFSM` class that progresses through various states (e.g., `IDLE`, `PULLBACK_DETECTED`, `WAITING_ENTRY`) to systematically identify trading opportunities.
- **Market Structure Identification**: Automatically detects and labels key market structure points like Highs, Lows, Lower Highs (LH), and Higher Lows (HL).
- **Advanced Charting**: Generates detailed candlestick charts using `mplfinance` with multiple layers of information:
  - Fibonacci Retracement levels.
  - Short-term and long-term EMAs.
  - Entry signals and confirmation candles.
  - Custom labels for market structure points with variable heights for clarity.
  - Risk/Reward boxes visualizing the trade's potential.
- **Dynamic Risk Management**: Calculates stop-loss and take-profit levels based on the Average True Range (ATR) and a configurable risk-to-reward ratio.
- **Dual-Trend Capability**: The FSM is equipped to handle logic for both bearish and bullish market scenarios.

---

## Core Components in local jyputer notebook

### 1. Data Preparation

The script begins by loading historical financial data (e.g., from a `.csv` file). It is expected to have a `DatetimeIndex` and standard OHLC (Open, High, Low, Close) columns. The initial steps include:
- Filtering data for a specific date range.
- Resampling to a desired timeframe (e.g., 4-hour).
- Calculating EMAs (e.g., 30 and 120 periods) to establish a trend.

### 2. `TradingFSM` Class

This is the core engine of the project. It iterates through the price data candle-by-candle and transitions between states based on a predefined trading strategy. The key states include:

- **`IDLE`**: The initial state, waiting for a clear trend and a pullback pattern.

- **`PULLBACK_DETECTED`**: A valid pullback (e.g., 3-6 consecutive counter-trend candles) has been identified.
- **`BREAK_OF_STRUCTURE`**: The price has broken a key structural point, confirming a new market leg (e.g., a Lower Low in a downtrend).
- **`WAITING_ENTRY`**: A Fibonacci retracement is drawn on the new leg, and the FSM waits for the price to pull back to a key level (e.g., 0.618).

![Example of a Fibonacci retracement in a Bullish trend](https://github.com/user-attachments/assets/d052f537-082c-4e28-af64-3fbfa38c3dd2)

- **`ENTRY_SIGNAL`**: The price has reached the target Fibonacci level.

![The price has retract back to level 0.618 of the Fibonacci retracement](https://github.com/user-attachments/assets/926d7334-acb2-406e-a8da-433448ffba8d)

- **`ENTER_MARKET`**: A confirmation pattern (e.g., an engulfing candle) has appeared, and a trade is considered active. The FSM then monitors for a stop-loss or take-profit hit.

![A Bullish Engulfing Candle appears in a Bullish trend](https://github.com/user-attachments/assets/8db02568-9676-42fb-ab67-989c3355e008)

- **`EXIT_MARKET (not stated in the function)`**:The market hits a stop-loss or take-profit .

![Take-profit price is reached](https://github.com/user-attachments/assets/24157e95-faf7-477e-8432-4303a5a66031)

### 3. `plot_fibonacci_candlestick` Function

A powerful and highly customizable plotting function that visualizes the FSM's analysis. It is called at various stages to show:
- The initial Fibonacci setup.
- The entry signal.
- The final outcome of a trade, including the risk/reward box.

The function is designed to "zoom in" on the relevant area of the chart and uses dynamic label heights to keep the annotations clean and readable.

## How to Use

1.  **Prerequisites**: Ensure you have the required Python libraries installed.
    ```bash
    pip install pandas numpy mplfinance matplotlib
    ```

2.  **Run the Notebook**: Execute "Run All". The main execution loop at the end will instantiate the `TradingFSM` and run it over your dataset.

    ```python
    # Main execution block in the notebook
    fsm = TradingFSM(df_4h.copy())
    for idx in range(len(df_4h)):
        fsm.update(idx)
    ```

3.  **Analyze Output**: The FSM will print its state transitions to the console and generate plots for each completed trading setup, allowing for a visual back-test of the strategy.

---

## Future Improvements

- **Expand Financial Indicators**: Add volume-based tools like VWAP and OBV to confirm trends.
Use volatility metrics (e.g., Bollinger Bands) and momentum indicators (e.g., RSI, MACD) for better signal generation. Incorporate market sentiment from news and social media.
- **Contextual Market Analysis**: Integrate economic calendar data to avoid trading during high-impact events. Use correlation analysis between assets (e.g., currency pairs, commodities) to identify broader trends.
- **Machine Learning Enhancements**: Train models for pattern recognition (e.g., head-and-shoulders, flags). Assign probability scores to trade setups using historical success rates.
- **Cross-Market Application**: Extend FSM to other markets like equities, commodities, and cryptocurrencies, adapting to their unique characteristics.


