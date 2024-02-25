## Firstly, we load the data and show the first 5 row data ##



```python
import pandas as pd
df = pd.read_pickle(r"shared/Motor_Vehicle_Collisions_-_Crashes.pkl")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRASH DATE_CRASH TIME</th>
      <th>BOROUGH</th>
      <th>ZIP CODE</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>LOCATION</th>
      <th>ON STREET NAME</th>
      <th>CROSS STREET NAME</th>
      <th>OFF STREET NAME</th>
      <th>NUMBER OF PERSONS INJURED</th>
      <th>...</th>
      <th>CONTRIBUTING FACTOR VEHICLE 2</th>
      <th>CONTRIBUTING FACTOR VEHICLE 3</th>
      <th>CONTRIBUTING FACTOR VEHICLE 4</th>
      <th>CONTRIBUTING FACTOR VEHICLE 5</th>
      <th>COLLISION_ID</th>
      <th>VEHICLE TYPE CODE 1</th>
      <th>VEHICLE TYPE CODE 2</th>
      <th>VEHICLE TYPE CODE 3</th>
      <th>VEHICLE TYPE CODE 4</th>
      <th>VEHICLE TYPE CODE 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-09-11 02:39:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>WHITESTONE EXPRESSWAY</td>
      <td>20 AVENUE</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4455765</td>
      <td>Sedan</td>
      <td>Sedan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-03-26 11:45:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>QUEENSBORO BRIDGE UPPER</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4513547</td>
      <td>Sedan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-06-29 06:55:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>THROGS NECK BRIDGE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4541903</td>
      <td>Sedan</td>
      <td>Pick-up Truck</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-09-11 09:35:00</td>
      <td>BROOKLYN</td>
      <td>11208.0</td>
      <td>40.667202</td>
      <td>-73.866500</td>
      <td>(40.667202, -73.8665)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1211      LORING AVENUE</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4456314</td>
      <td>Sedan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-12-14 08:13:00</td>
      <td>BROOKLYN</td>
      <td>11233.0</td>
      <td>40.683304</td>
      <td>-73.917274</td>
      <td>(40.683304, -73.917274)</td>
      <td>SARATOGA AVENUE</td>
      <td>DECATUR STREET</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4486609</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



### 1. We discover is What's the number of crashes in the region since 2012? ###


```python
# Firstly, convert the data time to the datetime format
df['CRASH DATE_CRASH TIME'] = pd.to_datetime(df['CRASH DATE_CRASH TIME'])

# Filter the year since 2012 
df_since_2012 = df[df['CRASH DATE_CRASH TIME'].dt.year >= 2012]

# Based on the BOROUGH, to determine the number of accidents in different area
borough_crashes = df_since_2012['BOROUGH'].value_counts()

# Based on the data showed below, we can know the second highest borough
borough_crashes

```




    BOROUGH
    BROOKLYN         441026
    QUEENS           372457
    MANHATTAN        313266
    BRONX            205345
    STATEN ISLAND     58297
    Name: count, dtype: int64



### 2. Based on the previous question, we want to know which borough has the most crashes for every 100,000 people? ###


```python

# Population estimates for each borough
population_borough = {
    'BRONX': 1446788,
    'BROOKLYN': 2648452,
    'MANHATTAN': 1638281,
    'QUEENS': 2330295,
    'STATEN ISLAND': 487155
}

# Calculate the crash rate per 100,000 people for each borough
crush_borough = (borough_crashes / borough_crashes.index.map(population_borough)) * 100000

# show the name of borough with most crashes 
print(crush_borough.idxmax())
# show the most number of crash of borough
print(crush_borough.max())

```

    MANHATTAN
    19121.628096767283


### 3. What proportion of accidents are attributable to this cause? ###


```python
#3
# remove the missing value of data 
df_filtered = df[df['CONTRIBUTING FACTOR VEHICLE 1'] != 'Unspecified']

# find out which reason caused the most accident 
leading_cause = df_filtered['CONTRIBUTING FACTOR VEHICLE 1'].value_counts().idxmax()

# determine the number of accident in this reason 
leading_cause_count = df_filtered['CONTRIBUTING FACTOR VEHICLE 1'].value_counts().max()

# calculate the number of Total accidents 
total_filtered_crashes = df_filtered.shape[0]

# find the probability of the reason in total accident 
proportion_of_leading_cause = leading_cause_count / total_filtered_crashes

print(leading_cause)
proportion_of_leading_cause
```

    Driver Inattention/Distraction





    0.3027229539746618



### 4. The top 5 causes of crashes (ignoring 'Unspecified') account for what proportion of total crashes? ###


```python
# Calculate the count of crashes for the top 5 causes, excluding 'Unspecified'
top_5_causes_counts = specified_factors_df['CONTRIBUTING FACTOR VEHICLE 1'].value_counts().head(5)

# Calculate the proportion of total crashes attributable to these top 5 causes
total_crashes = specified_factors_df['CONTRIBUTING FACTOR VEHICLE 1'].count()

#Calculate the total count of crashes for these top 5 causes and the proportion 
(top_5_causes_counts.sum()) / total_crashes


```




    0.5803878374209062



### 5. The total count of accidents that involved two or more fatalities? ###


```python

df['CRASH DATE_CRASH TIME'] = pd.to_datetime(df['CRASH DATE_CRASH TIME'])

# Filter the year since 2012
df_since_2012 = df[df['CRASH DATE_CRASH TIME'].dt.year >= 2012]

# let missing value of 'NUMBER OF PERSONS KILLED' be 0
df_since_2012['NUMBER OF PERSONS KILLED'] = df_since_2012['NUMBER OF PERSONS KILLED'].fillna(0)

# count the number of person killed above 2 
df_since_2012[df_since_2012['NUMBER OF PERSONS KILLED'] >= 2].shape[0]

```




    88



### 6. On average, for every 1000 accidents, how many have resulted in at least one person dead? ###


```python
# let the missing value of 'NUMBER OF PERSONS KILLED' be 0
df['NUMBER OF PERSONS KILLED'] = df['NUMBER OF PERSONS KILLED'].fillna(0)

# calculate the number of person killed greater than 1 
accidents_least_one_death = df[df['NUMBER OF PERSONS KILLED'] >= 1].shape[0]

# count the number of at least one person death in every 1000 accidents 
(accidents_least_one_death / (df.shape[0])) * 1000

```




    1.3893258747079764



### 7. The proportion of accidents in the data do not have a Cross Street Name ###


```python
# count the number of accident which not have a cross street name
accidents_without_cross_street = df[df['CROSS STREET NAME'].isna()].shape[0]

# count the proportion 
accidents_without_cross_street / (df.shape[0])


```




    0.37435098315615795



### 8. Which combination of vehicles have the most number of accidents in type 1 vehicle and type 2 vehicle ###


```python
#8
# create a new column and combine the 
# df['VEHICLE COMBINATION'] = df['VEHICLE TYPE CODE 1'] + ' & ' + df['VEHICLE TYPE CODE 2']
df['VEHICLE TYPE CODE 1'] = df['VEHICLE TYPE CODE 1'].astype(str)
df['VEHICLE TYPE CODE 2'] = df['VEHICLE TYPE CODE 2'].astype(str)
df['SORTED VEHICLE COMBINATION'] = df.apply(lambda x: ' & '.join(sorted([x['VEHICLE TYPE CODE 1'], x['VEHICLE TYPE CODE 2']])), axis=1)

# 步骤2: 计算每种组合的出现次数
combination_counts = df['SORTED VEHICLE COMBINATION'].value_counts()
combination_counts
```




    SORTED VEHICLE COMBINATION
    Sedan & Station Wagon/Sport Utility Vehicle                                  247727
    Sedan & Sedan                                                                197944
    PASSENGER VEHICLE & PASSENGER VEHICLE                                        193260
    Sedan & nan                                                                  138151
    Station Wagon/Sport Utility Vehicle & Station Wagon/Sport Utility Vehicle    133780
                                                                                  ...  
    Box Truck & POST                                                                  1
    Station Wagon/Sport Utility Vehicle & wagon                                       1
    Van ( & nan                                                                       1
    Van & trail                                                                       1
    PEDICAB & nan                                                                     1
    Name: count, Length: 5950, dtype: int64



### 9. Among crashes where the contributing factor was alcohol involvement, what proportion resulted in a fatality? ###


```python
# filter the accident which caused by Alcohol Involvement in CONTRIBUTING FACTOR VEHICLE 1
alcohol_related_accidents = df[df['CONTRIBUTING FACTOR VEHICLE 1'] == 'Alcohol Involvement']

# let missing value of 'NUMBER OF PERSONS KILLED' is 0
alcohol_related_accidents['NUMBER OF PERSONS KILLED'] = alcohol_related_accidents['NUMBER OF PERSONS KILLED'].fillna(0)

# filter the data which number of person killed is greater than 1 and calculate the number of accident 
alcohol_accidents = alcohol_related_accidents[alcohol_related_accidents['NUMBER OF PERSONS KILLED'] >= 1].shape[0]

# calculate the total number of accident 
total_alcohol = alcohol_related_accidents.shape[0]

# get proportion 
alcohol_accidents / total_alcohol


```

    /tmp/ipykernel_133/2240504011.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      alcohol_related_accidents['NUMBER OF PERSONS KILLED'] = alcohol_related_accidents['NUMBER OF PERSONS KILLED'].fillna(0)





    0.0046638345031400075



### 10. What proportion of crashes occur during the evening rush hour, defined as starting at 4 PM, and before 7 PM? ###


```python
# filter the time between 4 pm and 7 pm 
evening_rush_hour_crashes = df[(df['CRASH DATE_CRASH TIME'].dt.hour >= 16) & (df['CRASH DATE_CRASH TIME'].dt.hour < 19)]

# calculate the number of accident in the range and the total number of accident to get the proportion
(evening_rush_hour_crashes.shape[0]) / (df.shape[0])
```




    0.20514010935243243



### 11. Among crashes involving motorcycles, what proportion resulted in injuries but no fatalities? ###


```python
# From VEHICLE TYPE CODE 1 and VEHICLE TYPE CODE 2, filter the MOTORCYCLE and combine to motorcycle
motorcycle = df[(df['VEHICLE TYPE CODE 1'].str.contains('MOTORCYCLE', na=False)) | 
                        (df['VEHICLE TYPE CODE 2'].str.contains('MOTORCYCLE', na=False))]

# let the missing value of 'NUMBER OF PERSONS INJURED' and 'NUMBER OF PERSONS KILLED' be 0 
motorcycle['NUMBER OF PERSONS INJURED'] = motorcycle['NUMBER OF PERSONS INJURED'].fillna(0)
motorcycle['NUMBER OF PERSONS KILLED'] = motorcycle['NUMBER OF PERSONS KILLED'].fillna(0)

# filter the number of person injured greater than 0 and let the number of killed equal to 0 
injuries_without_fatalities = motorcycle[(motorcycle['NUMBER OF PERSONS INJURED'] > 0) & 
                                                  (motorcycle['NUMBER OF PERSONS KILLED'] == 0)]

# count the accident of injured without fatalities
injuries_without_fatalities_count = injuries_without_fatalities.shape[0]

# count the total number of motorcycle accident 
total_motorcycle = motorcycle.shape[0]

# calculate the proportions 
injuries_without_fatalities_count / total_motorcycle

```

    /tmp/ipykernel_133/1467522488.py:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      motorcycle['NUMBER OF PERSONS INJURED'] = motorcycle['NUMBER OF PERSONS INJURED'].fillna(0)
    /tmp/ipykernel_133/1467522488.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      motorcycle['NUMBER OF PERSONS KILLED'] = motorcycle['NUMBER OF PERSONS KILLED'].fillna(0)





    0.5004565018912221



### 12. Determine the number of crashes involved bicycles as one of the vehicles ###


```python
# combine the VEHICLE TYPE CODE 1 and VEHICLE TYPE CODE 2 data with the bicycle 
bicycle_crashes = df[(df['VEHICLE TYPE CODE 1'].str.contains('BICYCLE', na=False)) | 
                     (df['VEHICLE TYPE CODE 2'].str.contains('BICYCLE', na=False))]

# count the total number 
total_bicycle_crashes = bicycle_crashes.shape[0]
total_bicycle_crashes

```




    19108


