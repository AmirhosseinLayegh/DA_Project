# DA_Project
```python
for index,row in poverty_level.iterrows():
    tempcity = row["city"]
    citysplit = tempcity.split(" ")
    if(citysplit[-1] == "cdp" or citysplit[-1] == "city" or  citysplit[-1] == "town" or citysplit[-1]=="village"): 
        del citysplit[-1] 
    tempstring = " ".join(citysplit)
    poverty_level.set_value(index,"city",tempstring)

```
