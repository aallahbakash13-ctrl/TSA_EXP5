# Ex.No: 05  IMPLEMENTATION OF TIME SERIES ANALYSIS AND DECOMPOSITION
### Date: 03-11-2026


### AIM:
To Illustrates how to perform time series analysis and decomposition on the monthly average temperature of a city/country and for airline passengers.
### DATASET:
world stock prices dataset
### SOFTWARE REQUIRED:
jupyter-notebook

### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the decomposition process for the required data.
4. Plot the data according to need, either seasonal_decomposition or trend plot.
5. Display the overall results.

### PROGRAM:
```
import pandas as pd
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# =========================
# Load dataset
# =========================
df = pd.read_csv('World-Stock-Prices-Dataset.csv')

# Clean column names
df.columns = df.columns.str.strip().str.lower()

# Filter for a specific stock (e.g., Amazon 'AMZN')
df = df[df['ticker'] == 'AMZN'].copy()

# Parse datetime and remove timezone if present
df['date'] = pd.to_datetime(df['date'], utc=True).dt.tz_convert(None)

# Remove duplicate dates to ensure a unique index
df = df.drop_duplicates(subset=['date'])

# Set date as index and sort
df = df.set_index('date').sort_index()

# Create a complete daily date range from start to end
full_range = pd.date_range(start=df.index.min(), end=df.index.max(), freq='D')

# Reindex and interpolate linearly in both directions to fill all missing days (weekends/holidays)
df = df[['close']].reindex(full_range)
df['close'] = df['close'].interpolate(method='linear', limit_direction='both')

# =========================
# 1. FIRST FIVE ROWS
# =========================
print("\nFIRST FIVE ROWS:\n")
print(df.head())

# =========================
# 2. PLOTTING THE DATA
# =========================
print("\nPLOTTING THE DATA:\n")

plt.figure(figsize=(8,4))
plt.plot(df.index, df['close'], color='blue')
plt.title('World Stock Close Price Over Time')
plt.xlabel('Date')
plt.ylabel('Close Price')
plt.grid(True)
plt.show()

# =========================
# 3. SEASONAL DECOMPOSITION
# =========================
decomposition = seasonal_decompose(df['close'], model='additive', period=7)

# =========================
# 3. SEASONAL PLOT
# =========================
print("\nSEASONAL PLOT REPRESENTATION:\n")

plt.figure(figsize=(8,4))
plt.plot(decomposition.seasonal.index, decomposition.seasonal, color='green')
plt.title('Seasonal Component (Close Price)')
plt.grid(True)
plt.show()

# =========================
# 4. TREND PLOT
# =========================
print("\nTREND PLOT REPRESENTATION:\n")

plt.figure(figsize=(8,4))
plt.plot(decomposition.trend.index, decomposition.trend, color='orange')
plt.title('Trend Component (Close Price)')
plt.grid(True)
plt.show()

# =========================
# 5. OVERALL REPRESENTATION
# =========================
print("\nOVERALL REPRESENTATION:\n")

decomposition.plot()
plt.show()
```

















### OUTPUT:
FIRST FIVE ROWS:

<img width="361" height="185" alt="image" src="https://github.com/user-attachments/assets/39a9e954-6400-4fc2-9795-aea33603bf90" />




PLOTTING THE DATA:

<img width="987" height="505" alt="image" src="https://github.com/user-attachments/assets/ad895df0-661f-435f-ac29-0c7722ddba4c" />



SEASONAL PLOT REPRESENTATION :

<img width="920" height="486" alt="image" src="https://github.com/user-attachments/assets/f13f990c-9db0-4749-b5ef-da1a45d787b7" />




TREND PLOT REPRESENTATION :

<img width="997" height="492" alt="image" src="https://github.com/user-attachments/assets/4b72735c-e93e-433b-aff2-b65c529b0f26" />


OVERAL REPRESENTATION:

<img width="912" height="602" alt="image" src="https://github.com/user-attachments/assets/0cf3bfd5-9a5b-4d24-83dc-4064c177fffe" />




### RESULT:
Thus we have created the python code for the time series analysis and decomposition.
