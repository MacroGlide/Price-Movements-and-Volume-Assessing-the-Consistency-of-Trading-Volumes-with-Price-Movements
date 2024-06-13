## Overview

This project aims to analyze historical stock prices and volume data for SPY (SPDR S&P 500 ETF) to identify trends and mismatches between price movements and trading volumes.

## Prerequisites

- Python 3.7 or higher
- Required Python packages:
  - requests
  - pandas
  - numpy
  - openpyxl

You can install the required packages using the following command:
pip install requests pandas numpy openpyxl

## Usage

1. Data Collection:
   - Fetch historical stock price data using the provided API URLs.
   - Save the collected data to SPY_trading_hours_data.xlsx.

2. Change Calculation:
   - Calculate minute, hourly, 4-hourly, and daily changes in stock prices.
   - Save the changes data to SPY_changes.xlsx.

3. Volume by Group:
   - Group the trading volume data based on price changes (bullish or bearish).
   - Save the volume data to Volume_by_Group.xlsx.

4. Trend Comparison:
   - Compare price trends with volume trends to identify matches or mismatches.
   - Save the trend comparison data to trend_comparison.xlsx.

## Execution

Run the Python script to execute the entire process:
Volume_Mismatch.py

The script will output several Excel files:

- SPY_trading_hours_data.xlsx
- SPY_changes.xlsx
- Volume_by_Group.xlsx
- trend_comparison.xlsx

These files contain the collected data, calculated changes, grouped volume, and trend comparison results.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contact

For any inquiries or questions, please contact [macroglide@gmail.com](mailto:macroglide@gmail.com).

## Acknowledgements

- Thanks to Polygon.io for providing the stock market data API.
- Special thanks to everyone who contributed to the development of this project.
