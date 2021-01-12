# Data preparation

De student heeft data voorbereid  

We hebben een dataframe gemaakt voor consumptie om te gebruiken voor verschillende modellen.

```python
samplehours = 1

import pandas as pd
from sklearn.preprocessing import StandardScaler
import numpy as np

import sys
sys.path.insert(0,'/home/16095065/notebooks/zero/Imports:/')
import Load_data as ld #resample.mean()
a_dict = {'energyHeatpump': [2],
         'smartMeter': [6,7],
         'solar' : [2,3]}
```
Het inladen van data is een functie die Niels Schaiken heeft gemaakt. Dit was een ieder van ons project kon gebruiken om de data in te laden.


Het berekenen van consumptie is de volgende forumule:
- Consumptie = Energy.out + (Solar.in - Solar.out) - Energy.out

```python
#Calculating consumption
df = df.diff()
df['consumption'] = df.apply(lambda x: x['smartMeter_6'] + (x['solar_3']-x['solar_2'])-x['smartMeter_7'], axis=1)
df = df.dropna()
df = df.filter(['consumption'])
df.head(1)
```

Resamplen hebben we gedaan met de volgende code
```python
# Resampling to an hour
df = df['2019']
df = df.resample(str(samplehours)+'H').sum()
```

Vervolgens hebben consumption geshift
```python
def shifting(sf, shift:int):
    sf['cons_T-'+str(shift)] = sf['consumption'].shift(periods=shift, freq='H')
    return sf

temp_df = df.filter(items=['consumption'])
day_temp_df = df.filter(items=['consumption'])

shiftDagen = [24, 48, 72, 168]

#week
for i in range(24, 168+1):
    temp_df = shifting(temp_df, i)
temp_df = temp_df.drop(['consumption'], axis=1)

#day
for i in range(24, 48+1):
    day_temp_df = shifting(day_temp_df, i)
day_temp_df = day_temp_df.drop(['consumption'], axis=1)

#Shifted days
for i in shiftDagen:
    df = shifting(df, i)

#columns added
df['day_mean'] = day_temp_df.mean(axis=1, skipna=True)
df['week_mean'] = temp_df.mean(axis=1, skipna=True)
```
 
