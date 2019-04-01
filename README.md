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

# Cleaning poverty level
```python
poverty_level.drop_duplicates() #removing the duplicates
poverty_level_miss_values = poverty_level.isnull().sum()#see the poverty_level DataFrame attributes that have Nan values
poverty_level = poverty_level [poverty_level.iloc[:,2] != '-']#removing rows that have '-' in poverty_rate
poverty_level.columns= ['state','city','poverty_rate']#renaming columns 
poverty_level.city = poverty_level.city.str.lower()
poverty_level.poverty_rate = pd.to_numeric(poverty_level.poverty_rate,downcast='float')#ensure that poverty rate column is of float
```
# poverty level after cleaning

# Cleaning median house income
```python
median_house_income.drop_duplicates() #removing duplicates
#see the Nan values of the median_house_income DataFrame
median_house_income_miss_values = median_house_income.isnull().sum()
#dropping the rows that have Nan values in median income
median_house_income.dropna(subset=['Median Income'],how='all', inplace = True)
#drop (X) and '-' values
median_house_income = median_house_income [median_house_income.iloc[:,2] != '(X)'] 
median_house_income = median_house_income [median_house_income.iloc[:,2] != '-']
#replacing median income which is -2500 with 2500
median_house_income.iloc[:,2] = median_house_income.iloc[:,2].replace('2,500-',2500) 
median_house_income.iloc[:,2] = median_house_income.iloc[:,2].replace('250,000+',250000)
#Rename columns
median_house_income.columns = ['state','city','median_income'] 
#make the city column to lower
median_house_income.city = median_house_income.city.str.lower()
median_house_income.median_income = pd.to_numeric(median_house_income.median_income,downcast='float')
```
# Cleaning race by city
```python
#removing duplicates
race_by_city.drop_duplicates()
#see the Nan values of the race_by_city DataFrame
race_by_city_miss_values = race_by_city.isnull().sum()
#replacing (X) values in race columns
race_by_city = race_by_city [race_by_city.iloc[:,2] != '(X)']
race_by_city = race_by_city [race_by_city.iloc[:,3] != '(X)']
race_by_city = race_by_city [race_by_city.iloc[:,4] != '(X)']
race_by_city = race_by_city [race_by_city.iloc[:,5] != '(X)']
race_by_city = race_by_city [race_by_city.iloc[:,6] != '(X)']
#replacing - values in race columns
race_by_city = race_by_city [race_by_city.iloc[:,2] != '-']
race_by_city = race_by_city [race_by_city.iloc[:,3] != '-']
race_by_city = race_by_city [race_by_city.iloc[:,4] != '-']
race_by_city = race_by_city [race_by_city.iloc[:,5] != '-']
race_by_city = race_by_city [race_by_city.iloc[:,6] != '-']
#renaming columns
race_by_city.columns= ['state','city','white','black','native_american','asian','hispanic']
race_by_city.city = race_by_city.city.str.lower()
#ensure that race percentage column is of float
race_by_city.white = pd.to_numeric(race_by_city.white, downcast='float')
race_by_city.black = pd.to_numeric(race_by_city.black, downcast='float')
race_by_city.native_american = pd.to_numeric(race_by_city.native_american, downcast='float')
race_by_city.asian = pd.to_numeric(race_by_city.asian, downcast='float')
race_by_city.hispanic = pd.to_numeric(race_by_city.hispanic, downcast='float')
```
# Cleaning complete highschool 
```python
#removing duplicates
percentage_complete_highschool.drop_duplicates()
#see the Nan values of the percentage_complete_highschool DataFrame
percentage_complete_highschool_miss_values = percentage_complete_highschool.isnull().sum()
#dropping the rows that have - in their rate
percentage_complete_highschool = percentage_complete_highschool [percentage_complete_highschool.iloc[:,2] != '-']
#renaming columns
percentage_complete_highschool.columns = ['state','city','completed_hs']
percentage_complete_highschool.city = percentage_complete_highschool.city.str.lower()
percentage_complete_highschool.completed_hs = pd.to_numeric(percentage_complete_highschool.completed_hs, downcast='float')
```
# People killed by race 
```python
#showing the total number of people killed by race
plt.figure(figsize=(15,5))
sns.countplot(data=kill, x='race')
plt.title('Total number of people killed, by race',fontsize=18)
```
# People killed as a proportion of their race
```python
# showing the number of people killed as a proportion of their respective race
race_list = ["A", "B", "H", "N", "O", "W"]
killed_perrace = []

for i in race_list:
    killings_c = kill.race.loc[(kill.race==i)].count()
    killed_perrace.append(killings_c)
    
print (killed_perrace) #showing killed people for each races
proportion_killed_perrace = []

for i in race_list:
    if i == "A":
        proportion_killings_c = killed_perrace[0]/14674252.0
        print (proportion_killings_c)
    elif i == "B":
        proportion_killings_c = killed_perrace[1]/38929319.0
        print (proportion_killings_c)
    elif i == "H":
        proportion_killings_c = killed_perrace[2]/50477594.0
        print (proportion_killings_c)
    elif i == "N":
        proportion_killings_c = killed_perrace[3]/2932248.0
        print (proportion_killings_c)
    elif i == "O":
        proportion_killings_c = killed_perrace[4]/22579629.0
        print (proportion_killings_c)
    else:
        proportion_killings_c = killed_perrace[5]/223553265.0
        print (proportion_killings_c)
        
    proportion_killed_perrace.append(proportion_killings_c)
plt.figure(figsize=(14,6))
sns.barplot(x=race_list, y=proportion_killed_perrace)        
```
# Number of People killed by state
```python
plt.figure(figsize=(20,10))
sns.countplot(data=kill, x=kill.state, order=kill['state'].value_counts().index)
plt.title("Number of police killings, by state", fontsize=27)
```

# Age distribution of people murdered by police
```python
# General age distribution
plt.figure(figsize=(15,5))
age_dist = sns.distplot(kill.age, bins= 40)
age_dist.set(xlabel='Age',ylabel='Count')
plt.title('Age distribution',fontsize=18)
```
# Age distribution of people murdered by race
```python
#age distribution by race
three_races = kill.loc[(kill["race"] == "B") | (kill["race"] == "W") | (kill["race"] == "H")]
g = sns.FacetGrid(data=three_races, hue="race", aspect=3, size=4)
g.map(sns.kdeplot, "age", shade=True)
g.add_legend(title="Race")
g.set_ylabels("Count")
plt.title("Age distribution, by race", fontsize=17)
```

# Age distribution of people murdered in the states with highest fatalities
```python
plt.figure(figsize=(20,10))
sns.boxplot(x='state', y='age',data=kill, order=['CA','TX','FL','AZ','OH','OK','NC','CO','GA','IL'])
```
# States with the highest poverty rate
```python
area_list = list(poverty_level['state'].unique())
poverty_ratio=[]
#calculate the eaverage rate for each city in terms of their cities
for i in area_list: 
    x=poverty_level[poverty_level['state']== i]
    area_poverty_rate= sum(x.poverty_rate)/len(x)
    poverty_ratio.append(area_poverty_rate)
data_poverty_level = pd.DataFrame({'area_list': area_list , 'poverty_rate':poverty_ratio})
poverty_level_index = (data_poverty_level['poverty_rate'].sort_values(ascending = False )).index.values
data_poverty_level = data_poverty_level.reindex(poverty_level_index)
data_poverty_level.head(10)
plt.figure(figsize=(15,10))
g=sns.barplot(x=area_list,y=data_poverty_level.poverty_rate,data=data_poverty_level)
g.set(xlim=(0, 19)) #to see the top20
```
