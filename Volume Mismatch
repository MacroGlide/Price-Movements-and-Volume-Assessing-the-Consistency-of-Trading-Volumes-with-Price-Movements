#Stage Ⅰ: Data Collection
import requests
import pandas as pd
import numpy as np

# Fetching data from API
urls = [
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey",
    "https://api.polygon.io/v2/aggs/ticker/Ticker/range/1/minute/StartDate/EndDate?adjusted=true&sort=asc&limit=50000&apiKey=YourAPIKey"
]

def fetch_data(urls):
    all_data = []
    for url in urls:
        try:
            response = requests.get(url)
            response.raise_for_status()  # Raise exception for bad status codes
            data = response.json()
            if 'results' in data:
                all_data.extend(data['results'])
            else:
                print(f"No data in API response for URL: {url}")
        except requests.exceptions.RequestException as e:
            print(f"Request error: {e}")
    return all_data

# Fetch all data
data = fetch_data(urls)

# Proceed if data is available
if data:
    # Create DataFrame
    df = pd.DataFrame(data)
    df = df[['t', 'o', 'c', 'v']]
    df.rename(columns={'t': 'Timestamp', 'o': 'Open', 'c': 'Close', 'v': 'Volume'}, inplace=True)
    df['Timestamp'] = pd.to_datetime(df['Timestamp'], unit='ms')
    df['Date'] = df['Timestamp'].dt.date
    df['Time'] = df['Timestamp'].dt.time
    start_time = pd.to_datetime('09:30:00').time()
    end_time = pd.to_datetime('16:00:00').time()
    df = df[(df['Time'] >= start_time) & (df['Time'] <= end_time)]
    df_1 = df[['Date', 'Time', 'Open', 'Volume']]

    with pd.ExcelWriter('SPY_trading_hours_data.xlsx', date_format='YYYY-MM-DD') as writer:
        df_1.to_excel(writer, index=False)
    print("Trading hours data saved to SPY_trading_hours_data.xlsx")
else:
    print("No data to process.")

#Stage Ⅱ: Calculation of Price Changes
# Calculate changes
df = df_1.copy()
df['Datetime'] = pd.to_datetime(df['Date'].astype(str) + ' ' + df['Time'].astype(str))
df = df.sort_values(by=['Date', 'Time'])
df['Minute Change'] = df['Open'].diff().shift(-1)
df['Minute Change %'] = (df['Minute Change'] / df['Open']) * 100

# Hourly changes
df['Hour'] = (df['Datetime'] + pd.Timedelta(minutes=30)).dt.floor('H')
hourly_df = df.groupby(['Date', 'Hour']).first().reset_index()
hourly_df['Hour Change'] = hourly_df['Open'].diff()
hourly_df['Hour Change %'] = (hourly_df['Hour Change'] / hourly_df['Open'].shift(1)) * 100
hourly_df['Hour Change'] = hourly_df['Hour Change'].shift(-1)
hourly_df['Hour Change %'] = hourly_df['Hour Change %'].shift(-1)
df = df.merge(hourly_df[['Date', 'Hour', 'Hour Change', 'Hour Change %']], on=['Date', 'Hour'], how='left')

# 4-hour changes
df['4Hour'] = (df['Datetime'] + pd.Timedelta(hours=2)).dt.floor('4H')
four_hourly_df = df.groupby(['Date', '4Hour']).first().reset_index()
four_hourly_df['4-Hour Change'] = four_hourly_df['Open'].diff()
four_hourly_df['4-Hour Change %'] = (four_hourly_df['4-Hour Change'] / four_hourly_df['Open'].shift(1)) * 100
four_hourly_df['4-Hour Change'] = four_hourly_df['4-Hour Change'].shift(-1)
four_hourly_df['4-Hour Change %'] = four_hourly_df['4-Hour Change %'].shift(-1)
df = df.merge(four_hourly_df[['Date', '4Hour', '4-Hour Change', '4-Hour Change %']], on=['Date', '4Hour'], how='left')

# Daily changes
first_minute_df = df.groupby('Date').first().reset_index()
first_minute_df['Daily Change'] = first_minute_df['Open'].diff().shift(-1)
first_minute_df['Daily Change %'] = (first_minute_df['Daily Change'] / first_minute_df['Open']) * 100
df_2 = pd.merge(df, first_minute_df[['Date', 'Daily Change', 'Daily Change %']], on='Date', how='left')
df_2.loc[df_2.duplicated(subset=['Date']), ['Daily Change', 'Daily Change %']] = np.nan

# Save to Excel
columns_to_save = ['Datetime', 'Date', 'Time', 'Open', 'Minute Change', 'Minute Change %', 'Hour Change', 'Hour Change %', '4-Hour Change', '4-Hour Change %', 'Daily Change', 'Daily Change %']
with pd.ExcelWriter('SPY_changes.xlsx', date_format='YYYY-MM-DD') as writer:
    df_2[columns_to_save].to_excel(writer, sheet_name='Changes', index=False)
print("Changes data saved to SPY_changes.xlsx")

#Stage Ⅲ: Classification and Aggregation of Volumes by Timeframes
# Volume grouping
df_changes = df[['Date', 'Time', 'Minute Change']]
df_volume = df_1[['Date', 'Time', 'Volume']]
merged_df = pd.merge(df_changes, df_volume, on=['Date', 'Time'])
merged_df['Group'] = merged_df['Minute Change'].apply(lambda x: 'Bullish' if x > 0 else ('Bearish' if x < 0 else np.nan))
merged_df['Datetime'] = pd.to_datetime(merged_df['Date'].astype(str) + ' ' + merged_df['Time'].astype(str))

hour_bins = [570, 630, 690, 750, 810, 870, 930, 960]
hour_labels = ['09:30', '10:30', '11:30', '12:30', '13:30', '14:30', '15:30']
merged_df['Hour'] = pd.cut(merged_df['Datetime'].dt.hour * 60 + merged_df['Datetime'].dt.minute, bins=hour_bins, labels=hour_labels, right=False)

four_hour_bins = [570, 810, 960]
four_hour_labels = ['09:30', '13:30']
merged_df['Four_Hour'] = pd.cut(merged_df['Datetime'].dt.hour * 60 + merged_df['Datetime'].dt.minute, bins=four_hour_bins, labels=four_hour_labels, right=False)

merged_df = merged_df.dropna(subset=['Hour', 'Four_Hour'])

grouped_daily_volume = merged_df.groupby(['Date', 'Group'])['Volume'].sum().unstack(fill_value=0)

# Update the Datetime column with the first actual Datetime in the interval
def get_first_datetime(df, interval):
    first_times = df.groupby(['Date', interval])['Datetime'].first().reset_index()
    return first_times

hourly_first_times = get_first_datetime(merged_df, 'Hour')
four_hour_first_times = get_first_datetime(merged_df, 'Four_Hour')

grouped_hourly_volume = merged_df.groupby(['Date', 'Hour', 'Group'])['Volume'].sum().unstack(fill_value=0).reset_index()
grouped_hourly_volume = grouped_hourly_volume.merge(hourly_first_times, on=['Date', 'Hour'])
grouped_hourly_volume = grouped_hourly_volume.rename(columns={'Datetime': 'First_Datetime'})
grouped_hourly_volume['Datetime'] = grouped_hourly_volume['First_Datetime']
grouped_hourly_volume = grouped_hourly_volume.drop(columns=['First_Datetime'])

grouped_four_hour_volume = merged_df.groupby(['Date', 'Four_Hour', 'Group'])['Volume'].sum().unstack(fill_value=0).reset_index()
grouped_four_hour_volume = grouped_four_hour_volume.merge(four_hour_first_times, on=['Date', 'Four_Hour'])
grouped_four_hour_volume = grouped_four_hour_volume.rename(columns={'Datetime': 'First_Datetime'})
grouped_four_hour_volume['Datetime'] = grouped_four_hour_volume['First_Datetime']
grouped_four_hour_volume = grouped_four_hour_volume.drop(columns=['First_Datetime'])

with pd.ExcelWriter('SPY_volume_by_group.xlsx', date_format='YYYY-MM-DD') as writer:
    grouped_daily_volume.to_excel(writer, sheet_name='Daily Volume by Group')
    grouped_hourly_volume.to_excel(writer, sheet_name='Hourly Volume by Group')
    grouped_four_hour_volume.to_excel(writer, sheet_name='Four Hour Volume by Group')
print("Volume data saved to SPY_volume_by_group.xlsx")

#Stage Ⅳ: Calculation of Confirmation and Non-Confirmation of Price Movements by Volumes
# Definition of Volume Trend
grouped_daily_comparison = grouped_daily_volume.copy()
grouped_daily_comparison['Volume Trend'] = grouped_daily_comparison.apply(
    lambda x: 'No Data' if x['Bullish'] == 0 and x['Bearish'] == 0 else ('Bullish' if x['Bullish'] > x['Bearish'] else 'Bearish'),
    axis=1
)

grouped_four_hour_comparison = grouped_four_hour_volume.copy()
grouped_four_hour_comparison['Volume Trend'] = grouped_four_hour_comparison.apply(
    lambda x: 'No Data' if x['Bullish'] == 0 and x['Bearish'] == 0 else ('Bullish' if x['Bullish'] > x['Bearish'] else 'Bearish'),
    axis=1
)

grouped_hourly_comparison = grouped_hourly_volume.copy()
grouped_hourly_comparison['Volume Trend'] = grouped_hourly_comparison.apply(
    lambda x: 'No Data' if x['Bullish'] == 0 and x['Bearish'] == 0 else ('Bullish' if x['Bullish'] > x['Bearish'] else 'Bearish'),
    axis=1
)

# Getting rid of empty datetimes in subsets
daily_df = df_2[['Date', 'Daily Change']].dropna()
daily_df = daily_df.reset_index(drop=True)

four_hour_df = df_2[['Datetime', '4-Hour Change']].dropna()
four_hour_df = four_hour_df.reset_index(drop=True)

hourly_df = df_2[['Datetime', 'Hour Change']].dropna()
hourly_df = hourly_df.reset_index(drop=True)

# Definition of Price Trend
daily_df['Price Trend'] = daily_df['Daily Change'].apply(lambda x: 'Bullish' if x > 0 else 'Bearish')
four_hour_df['Price Trend'] = four_hour_df['4-Hour Change'].apply(lambda x: 'Bullish' if x > 0 else 'Bearish')
hourly_df['Price Trend'] = hourly_df['Hour Change'].apply(lambda x: 'Bullish' if x > 0 else 'Bearish')

#Comparison of Price Trend and Volume Trend
grouped_daily_comparison = grouped_daily_comparison.merge(daily_df[['Date', 'Price Trend']], on='Date', how='left')
grouped_four_hour_comparison = grouped_four_hour_comparison.merge(four_hour_df[['Datetime', 'Price Trend']], on='Datetime', how='left')
grouped_hourly_comparison = grouped_hourly_comparison.merge(hourly_df[['Datetime', 'Price Trend']], on='Datetime', how='left')

grouped_daily_comparison['Mismatch'] = grouped_daily_comparison.apply(
    lambda x: 0 if pd.isna(x['Price Trend']) or x['Volume Trend'] == 'No Data' else (0 if x['Volume Trend'] == x['Price Trend'] else 1),
    axis=1
)
grouped_four_hour_comparison['Mismatch'] = grouped_four_hour_comparison.apply(
    lambda x: 0 if pd.isna(x['Price Trend']) or x['Volume Trend'] == 'No Data' else (0 if x['Volume Trend'] == x['Price Trend'] else 1),
    axis=1
)
grouped_hourly_comparison['Mismatch'] = grouped_hourly_comparison.apply(
    lambda x: 0 if pd.isna(x['Price Trend']) or x['Volume Trend'] == 'No Data' else (0 if x['Volume Trend'] == x['Price Trend'] else 1),
    axis=1
)

grouped_daily_comparison['Mismatch Sum'] = grouped_daily_comparison['Mismatch'].sum()

grouped_four_hour_comparison['Mismatch Sum'] = grouped_four_hour_comparison['Mismatch'].sum()

grouped_hourly_comparison['Mismatch Sum'] = grouped_hourly_comparison['Mismatch'].sum()

# Filter out rows where Price Trend is empty or Volume Trend is "No Data"
valid_daily_comparison = grouped_daily_comparison.dropna(subset=['Price Trend'])
valid_daily_comparison = valid_daily_comparison[valid_daily_comparison['Volume Trend'] != 'No Data']

valid_four_hour_comparison = grouped_four_hour_comparison.dropna(subset=['Price Trend'])
valid_four_hour_comparison = valid_four_hour_comparison[valid_four_hour_comparison['Volume Trend'] != 'No Data']

valid_hourly_comparison = grouped_hourly_comparison.dropna(subset=['Price Trend'])
valid_hourly_comparison = valid_hourly_comparison[valid_hourly_comparison['Volume Trend'] != 'No Data']

# Calculate mismatch percent based on valid rows
grouped_daily_comparison['Mismatch Percent'] = (valid_daily_comparison['Mismatch'].sum() / len(valid_daily_comparison)) * 100
grouped_four_hour_comparison['Mismatch Percent'] = (valid_four_hour_comparison['Mismatch'].sum() / len(valid_four_hour_comparison)) * 100
grouped_hourly_comparison['Mismatch Percent'] = (valid_hourly_comparison['Mismatch'].sum() / len(valid_hourly_comparison)) * 100

with pd.ExcelWriter('SPY_volume_price_comparison.xlsx', date_format='YYYY-MM-DD') as writer:
    grouped_daily_comparison.to_excel(writer, sheet_name='Daily', index=False)
    grouped_four_hour_comparison.to_excel(writer, sheet_name='4-Hourly', index=False)
    grouped_hourly_comparison.to_excel(writer, sheet_name='Hourly', index=False)
print("Comparison data saved to SPY_volume_price_comparison.xlsx")

#PRINT NEEDED DATA
print(f"Daily Mismatch Percent: {grouped_daily_comparison['Mismatch Percent'].iloc[0]}%")
print(f"4-Hourly Mismatch Percent: {grouped_four_hour_comparison['Mismatch Percent'].iloc[0]}%")
print(f"Hourly Mismatch Percent: {grouped_hourly_comparison['Mismatch Percent'].iloc[0]}%")
