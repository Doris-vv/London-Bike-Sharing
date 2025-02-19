# London Bike Sharing - Python & Tableau
The raw data for this project is only one file. This is the historical data for bike sharing in London 'Powered by TfL Open Data'. The data is manipulate in Python and presented in Tableau. This project involves the use of pandas

## Data source
You can download the data from https://www.kaggle.com/datasets/hmavrodiev/london-bike-sharing-dataset

## Objective
The govenment wants to know the count of London bike share in different time and condition to know when they should put in more bikes.

## Tools
|Tool       |purpose                                                |
|-----------|-------------------------------------------------------|
|Python   |   cleaninging the data|
|Mokkup AI|   designing the wireframe/mockup of the dashboard|
|Tableau |   visualizing the data via interactive dashboards|
|GitHub|      hosting the project documentation and version control|

## Data cleaning
### Transform the Data
```
# import the pandas library
import pandas as pd

# import zipfile library (we will use this to extract the file downloaded from Kaggle)
import zipfile

# import kaggle library (we will use this to download the dataset programatically from Kaggle)
import kaggle

# download dataset from kaggle using the Kaggle API
!kaggle datasets download -d hmavrodiev/london-bike-sharing-dataset

# extract the file from the downloaded zip file
zipfile_name = 'london-bike-sharing-dataset.zip'
with zipfile.ZipFile(zipfile_name, 'r') as file:
    file.extractall()

# read in the csv file as a pandas dataframe
bikes = pd.read_csv("london_merged.csv")

# specifying the column names that I want to use
new_cols_dict ={
    'timestamp':'time',
    'cnt':'count', 
    't1':'temp_real_C',
    't2':'temp_feels_like_C',
    'hum':'humidity_percent',
    'wind_speed':'wind_speed_kph',
    'weather_code':'weather',
    'is_holiday':'is_holiday',
    'is_weekend':'is_weekend',
    'season':'season'
}

# Renaming the columns to the specified column names
bikes.rename(new_cols_dict, axis=1, inplace=True)

# changing the humidity values to percentage (i.e. a value between 0 and 1)
bikes['humidity_percent'] = bikes['humidity_percent'] / 100

# creating a season dictionary so that we can map the integers 0-3 to the actual written values
season_dict = {
    '0.0':'spring',
    '1.0':'summer',
    '2.0':'autumn',
    '3.0':'winter'
}

# creating a weather dictionary so that we can map the integers to the actual written values
weather_dict = {
    '1.0':'Clear',
    '2.0':'Scattered clouds',
    '3.0':'Broken clouds',
    '4.0':'Cloudy',
    '7.0':'Rain',
    '10.0':'Rain with thunderstorm',
    '26.0':'Snowfall'
}

# changing the seasons column data type to string
bikes['season'] = bikes['season'].astype('str')
# mapping the values 0-3 to the actual written seasons
bikes['season'] = bikes['season'].map(season_dict)

# changing the weather column data type to string
bikes['weather'] = bikes['weather'].astype('str')
# mapping the values to the actual written weathers
bikes['weather'] = bikes['weather'].map(weather_dict)

# writing the final dataframe to an excel file that we will use in our Tableau visualisations. The file will be the 'london_bikes_final.xlsx' file and the sheet name is 'Data'
bikes.to_excel('london_bikes_final.xlsx', sheet_name='Data')
```

## Visualization
![Image](https://github.com/user-attachments/assets/f50bdfbe-f9be-4fad-9499-cc3771664113)

## Action Plan
1.In summer and autumn, the govenment should put more bikes in the market.

2.When the weather is scattered clouds and clear, more people will ride bikes.
