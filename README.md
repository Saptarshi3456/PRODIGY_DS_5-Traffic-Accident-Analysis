# PRODIGY_DS_5-Traffic-Accident-Analysis

import pandas as pd

df = pd.read_csv('https://www.kaggle.com/code/harshalbhamare/us-accident-eda')

df.head()

df.isnull().sum()

df.dropna(subset=['Weather_Condition', 'Start_Time', 'End_Time', 'Road_Condition'], inplace=True)

df['Start_Time'] = pd.to_datetime(df['Start_Time'])
df['End_Time'] = pd.to_datetime(df['End_Time'])

df['Hour'] = df['Start_Time'].dt.hour

df.info()
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
plt.hist(df['Hour'], bins=24, edgecolor='black')
plt.title('Accident Frequency by Hour of Day and Severity')
plt.xlabel('Hour of Day')
plt.ylabel('Number of Accidents')
plt.xticks(range(0, 24))
plt.grid(True)
plt.show()
import folium
from folium.plugins import HeatMap

base_map = folium.Map(location=[37.0902, -95.7129], zoom_start=5)

locations = df[['Start_Lat', 'Start_Lng']].dropna()

HeatMap(data=locations, radius=10).add_to(base_map)

base_map.save('accident_hotspots.html')
base_map
df['Weather_Condition'] = df['Weather_Condition'].astype('category').cat.codes
df['Road_Condition'] = df['Road_Condition'].astype('category').cat.codes

correlation_matrix = df[['Hour', 'Weather_Condition', 'Road_Condition']].corr()

plt.figure(figsize=(8, 6))
plt.matshow(correlation_matrix, fignum=1, cmap='coolwarm')
plt.xticks(range(len(correlation_matrix.columns)), correlation_matrix.columns, rotation=45)
plt.yticks(range(len(correlation_matrix.columns)), correlation_matrix.columns)
plt.colorbar()
plt.title('Correlation Matrix', pad=20)
plt.show()