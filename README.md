# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 17-05-2026

## AIM:
To perform regular differncing,seasonal adjustment and log transformatio on climate change data
## ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
## PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv('/content/drive/MyDrive/Time_Series/DailyDelhiClimateTest.csv')
data['date'] = pd.to_datetime(data['date'])
data.set_index('date', inplace=True)
data = data.sort_index()
data.index.freq = 'D' # Set frequency to daily

data['meantemp_diff'] = data['meantemp'].diff()
data['meantemp_log'] = np.log(data['meantemp'])
data['meantemp_log_diff'] = data['meantemp_log'].diff()

# For daily data, a period of 7 is appropriate for weekly seasonality
data['meantemp_sea_diff'] = seasonal_decompose(
    data['meantemp'], model='additive', period=7 # Changed period to 7 for weekly seasonality in daily data
).resid

data['meantemp_log_seasonal_diff'] = seasonal_decompose(
    data['meantemp_log_diff'].dropna(), model='additive', period=7 # Changed period to 7 for weekly seasonality
).resid


plt.figure(figsize=(12, 10))

plt.subplot(4, 1, 1)
plt.plot(data.index, data['meantemp'], label='Original Data')
plt.title('Original Mean Temperature Data (Delhi)')
plt.xlabel('Date')
plt.ylabel('Mean Temperature (°C)')
plt.legend()

plt.subplot(4, 1, 2)
plt.plot(data.index, data['meantemp_diff'], label='Regular Difference') 
plt.title('Regular Differencing')
plt.xlabel('Date')
plt.ylabel('Differenced Mean Temperature (°C)') 
plt.legend()

plt.subplot(4, 1, 3)
plt.plot(data.index, data['meantemp_sea_diff'], label='Seasonal Adjustment (Period=7)') 
plt.title('Seasonal Adjustment (Daily Mean Temperature)')
plt.xlabel('Date')
plt.ylabel('Seasonally Adjusted Mean Temperature (°C)') 
plt.legend()

plt.subplot(4, 1, 4)
plt.plot(data.index, data['meantemp_log'], label='Log Transformation') 
plt.title('Log Transformation')
plt.xlabel('Date')
plt.ylabel('Log(Mean Temperature)') 
plt.legend()

plt.tight_layout()
plt.show()
```

## OUTPUT:
### Original Data
<img width="1215" height="244" alt="image" src="https://github.com/user-attachments/assets/4db36bac-5ce7-4730-b3bf-7ad5d57ecfc5" />

### REGULAR DIFFERENCING:
<img width="1235" height="246" alt="image" src="https://github.com/user-attachments/assets/d990a5d0-8ecc-4bbe-9cf2-ab050538d2e8" />

### SEASONAL ADJUSTMENT:
<img width="1204" height="250" alt="image" src="https://github.com/user-attachments/assets/58fd4ed1-37e2-4a9d-9723-bcdd0ce382e5" />

### LOG TRANSFORMATION:
<img width="1262" height="248" alt="image" src="https://github.com/user-attachments/assets/9b079bf4-b779-48d9-b952-740fca007fba" />

## RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on climate change data and executed successfully
