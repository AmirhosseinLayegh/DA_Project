# DA_Project
# Importing the libraries we need
```python
#importing the libraries that we use in our project
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib as mpl
import matplotlib.pyplot as plt
import plotly.plotly as py
from sklearn import linear_model, metrics

```
# Creating the DataFrames
```python
kill= pd.read_csv("F:\Les\Data analytic\Project\Data/PoliceKillingsUS.csv", encoding='latin-1')
poverty_level = pd.read_csv("F:\Les\Data analytic\Project\Data/PercentagePeopleBelowPovertyLevel.csv",encoding='latin-1')
median_house_income = pd.read_csv("F:\Les\Data analytic\Project\Data/MedianHouseholdIncome2015.csv",encoding='latin-1')
race_by_city = pd.read_csv("F:\Les\Data analytic\Project\Data/ShareRaceByCity.csv",encoding='latin-1')
percentage_complete_highschool = pd.read_csv("F:\Les\Data analytic\Project\Data/PercentOver25CompletedHighSchool.csv",encoding='latin-1')
```
#  The structure of the police killings DataFrames
```python
kill.head(10)
```
#  The structure of the 'poverty_level' DataFrame
```python
poverty_level.head(10)
```
#  The structure of the 'median_house_income' DataFrame
```python
median_house_income.head(10)
```
#  The structure of the 'race_by_city' DataFrame
```python
race_by_city.head(10)
```
# The structure of the 'percentage_complete_highschool' DataFrame
```python
percentage_complete_highschool.head(10)
```
# Cleaning US Police shooting (kill DataFrame)
```python
kill.drop_duplicates() #removing duplicates
kill_miss_values = kill.isnull().sum() #see the kill DataFrame attributes that have Nan values
kill.armed = kill.armed.fillna('unknown') #filling the armed Nan values by 'unknown'
kill['age'].fillna((kill['age'].mean()), inplace=True) #filling the age Nan values by the average 
kill.dropna(subset=['race'],how='all', inplace = True) #drop the rows that have Nan values in 'race' attribute
kill.drop('flee',axis=1,inplace=True) #droping the 'flee' attribute
kill['name'].replace(['TK TK'],'Unknown',inplace = True) #Replacing the rows that have 'TK TK' in 'name' attribute bt 'Unknown'
kill.armed = kill.armed.str.lower() #making the armed column to lower
kill.armed = kill.armed.mask(kill.armed == 'undetermined','unknown') #replacing variations with uniform values
kill.armed = kill.armed.mask(kill.armed == 'unknown weapon','unknown')
kill.armed= kill.armed.mask(kill.armed == 'guns and explosives','gun')
kill.armed= kill.armed.mask(kill.armed == 'gun and knife','gun') 
kill.armed= kill.armed.mask(kill.armed == 'hatchet and gun','gun')
kill.armed= kill.armed.mask(kill.armed == 'machete and gun','gun')
kill.armed = kill.armed.mask(kill.armed == 'sword','knife')
kill.armed = kill.armed.mask(kill.armed == 'machete','knife')
kill.armed = kill.armed.mask(kill.armed == 'box cutter','knife')
kill.armed = kill.armed.mask(kill.armed == 'bayonet','knife')
kill.armed = kill.armed.mask(kill.armed == 'meat cleaver','knife')
kill.armed = kill.armed.mask(kill.armed == 'pole and knife','knife')
kill.armed = kill.armed.mask(kill.armed == 'lawn mower blade','knife')
kill.armed = kill.armed.mask(kill.armed == 'straight edge razor','knife')
kill.armed = kill.armed.mask(kill.armed == 'motorcycle','vehicle')
kill.armed = kill.armed.mask (kill.armed == 'ax','hatchet')
kill.armed = kill.armed.mask(kill.armed == 'flagpole','metal object')
kill.armed = kill.armed.mask(kill.armed == 'metal pipe','metal object')
kill.armed = kill.armed.mask(kill.armed == 'metal hand tool','metal object')
kill.armed = kill.armed.mask(kill.armed == 'metal pole','metal object')
kill.armed = kill.armed.mask(kill.armed == 'metal stick','metal object')
kill.armed = kill.armed.mask(kill.armed == 'brick','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'baseball bat','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'pole','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'rock','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'piece of wood','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'baton','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'oar','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'baseball bat and fireplace poker','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'hammer','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'baseball bat and bottle','blunt object')
kill.armed = kill.armed.mask(kill.armed == 'pipe','blunt object')
kill.drop('id',axis=1,inplace=True) #dropping the id attribute
kill.columns =[ 'name','date','manner_of_death','armed','age','gender','race','city','state','mental_illness','threat',
               'body_camera'] #renaming the kill DataFrame column
kill.city = kill.city.str.lower() #making city to lowercase
kill.date = pd.to_datetime(kill.date,dayfirst=True) #ensure that date column is of date type
```
# Police killings after cleaning
