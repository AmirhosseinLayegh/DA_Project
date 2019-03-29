# DA_Project
```python
for index,row in kill.iterrows():
    tempcity = row["city"]
    citysplit = tempcity.split(" ")
    if(citysplit[-1] == "cdp" or citysplit[-1] == "city" or  citysplit[-1] == "town" or citysplit[-1]=="village"): 
        del citysplit[-1] 
    tempstring = " ".join(citysplit)
    kill.set_value(index,"city",tempstring)

```
