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
# Observing the structure of the 'kill' DataFrames
```python
kill.head(10)
```
# Observing the structure of the 'poverty_level' DataFrames
```python
poverty_level.head(10)
```
# Observing the structure of the 'median_house_income' DataFrames
```python
median_house_income.head(10)
```
# Observing the structure of the 'race_by_city' DataFrames
```python
race_by_city.head(10)
```
# Observing the structure of the 'percentage_complete_highschool' DataFrames
```python
percentage_complete_highschool.head(10)
```
# Removing Duplicates
```pytho
kill.drop_duplicates()
poverty_level.drop_duplicates()
median_house_income.drop_duplicates()
race_by_city.drop_duplicates()
percentage_complete_highschool.drop_duplicates()
```
