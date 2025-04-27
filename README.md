import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Download stock data
ticker = 'AAPL'
start_date = '2023-01-01'
end_date = '2024-01-01'
data = yf.download(ticker, start=start_date, end=end_date)

# Calculate daily returns
data['Daily Return %'] = data['Close'].pct_change() * 100
data = data.dropna()

# Line Chart – Price over time
plt.figure(figsize=(12, 6))
plt.plot(data.index, data['Close'], color='blue', label='Close Price')
plt.title(f'{ticker} Line Chart - Close Price')
plt.xlabel('Date')
plt.ylabel('Price ($)')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()

# Pie Chart – Gains vs. Losses
gain_days = (data['Daily Return %'] > 0).sum()
loss_days = (data['Daily Return %'] <= 0).sum()

plt.figure(figsize=(6, 6))
plt.pie([gain_days, loss_days],
        labels=['Gains', 'Losses'],
        autopct='%1.1f%%',
        colors=['green', 'red'],
        startangle=90,
        explode=(0.1, 0))
plt.title(f'{ticker} Pie Chart – Gains vs. Losses')
plt.tight_layout()
plt.show()

# Lollipop Chart – Daily % Change
plt.figure(figsize=(14, 6))
plt.stem(data.index, data['Daily Return %'], basefmt=" ", linefmt='grey', markerfmt='o')
plt.title(f'{ticker} Lollipop Chart – Daily % Change')
plt.xlabel('Date')
plt.ylabel('Daily Return (%)')
plt.grid(True)
plt.tight_layout()
plt.show()

# Wave Chart – Smoothed "wave-like" closing price using rolling average
data['Wave'] = data['Close'].rolling(window=10, center=True).mean()

plt.figure(figsize=(12, 6))
plt.plot(data.index, data['Wave'], color='teal', label='Wave Trend')
plt.fill_between(data.index, data['Wave'], color='teal', alpha=0.3)
plt.title(f'{ticker} Wave Chart – Smoothed Trend')
plt.xlabel('Date')
plt.ylabel('Price ($)')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
