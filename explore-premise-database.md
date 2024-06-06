---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.2
  kernelspec:
    display_name: lca_alg
    language: python
    name: lca_alg
---

* Why impacts of background database are different if we change national scenario but no change in global scenario?
* premise_gwp ??? 

```python
#importation of usefull packages
from init import *
import bw2data
import bw2io
```

```python
#To do
PROJECT_NAME="HySPI_premise_Tr2050_3"
#PROJECT_NAME='HySPI_premise_FE2050_6'
```

```python
#Set in the right project and print the databases
bw2data.projects.set_current(PROJECT_NAME)
bw2data.databases
agb.list_databases() #equivalent lca_algebraic function
```

```python

```

# Manipulating databases

```python
ecoinvent_3_9_db= bw2data.Database('ecoinvent-3.9.1-cutoff')
ecoinvent_3_9_db_name='ecoinvent-3.9.1-cutoff'
```

```python
#generate a list of generated databases by premise
premise_db_name_list=list(bw2data.databases.keys())
for db_name in bw2data.databases.keys():
    if "biosphere" in db_name:
        premise_db_name_list.remove(db_name)
    elif "ecoinvent" in db_name:
        premise_db_name_list.remove(db_name)
premise_db_name_list
```

```python
def db_name_list(*args):
    """ Generate a list of database's name containing given keywords (one or several)"""
    list_db_name=[]
    for db_name in bw2data.databases.keys():
        a=0
        for arg in args:
            if arg in db_name:
                a=a+1
        if a==len(args):
            list_db_name.append(db_name)
    return list_db_name
```

```python
#Example 
db_name_list('Base','S1')
```

# Methods 

```python
EF = 'EF v3.0 no LT'
climate = (EF, 'climate change no LT', 'global warming potential (GWP100) no LT')
impacts=[climate]
```

# Impact of electricity


## Impact 1 kWh of electricity

```python
#To do : generate a list of databases to exploe
#list_db_name=db_name_list('S1')
list_db_name=premise_db_name_list
list_db_name
```

```python
df=pd.DataFrame([],columns=['scenario','act','impact','unit'])
list_act=[]

for dbtoexplore_name in list_db_name:
    act_elec_1kWh=agb.findActivity("market for electricity, high voltage, Tr2050", db_name=dbtoexplore_name)
    list_act.append(act_elec_1kWh)

for act in list_act:
   lca = act.lca(method=climate, amount=1)
   score = lca.score
   unit = bw2data.Method(climate).metadata["unit"]
   df.loc[len(df.index)] = [act["database"],act["name"],score,unit]
```

```python
def style_red(v, props=''):
    return props if type(v)==float and v > 0.100 else None
def style_orange(v, props=''):
    return props if type(v)==float and v < 0.100 and v > 0.03 else None
def style_green(v, props=''):
    return props if type(v)==float and v < 0.03 else None
```

```python
df1 = df.style.map(style_red, props='background-color:red;')\
             .map(style_orange, props='background-color:orange;')\
             .map(style_green, props='background-color:green;')
df1
```

## Imports contribution  

```python
list_db_name=premise_db_name_list
```

```python
df=pd.DataFrame([],columns=['scenario','% import','% impact of imports','impact 1 kWh elec','impact 1 kWh import','impact 1kWh without import','unit'])
list_act=[]

for dbtoexplore_name in list_db_name:
    act_elec_1kWh=agb.findActivity("market for electricity, high voltage, Tr2050", db_name=dbtoexplore_name)
    list_act.append(act_elec_1kWh)

for act_elec_1kWh in list_act:
    lca = act_elec_1kWh.lca(method=climate, amount=1)
    impact_elec_1kWh = lca.score

    import_kWh=[exc for exc in act_elec_1kWh.exchanges() if exc["name"]=='market group for electricity, high voltage'][0]
    act_import=import_kWh.input
    
    lca = act_import.lca(method=climate, amount=1)
    impact_import_1kWh = lca.score
    ratio_impact=impact_import_1kWh*import_kWh["amount"]/impact_elec_1kWh 
    act_elec_without_import=agb.findActivity("market for electricity without imports, high voltage, Tr2050",db_name=act_elec_1kWh["database"])
    #act_elec_without_import=agb.copyActivity(act_elec_1kWh["database"],act_elec_1kWh,"market for electricity without imports, high voltage, Tr2050")
    #act_elec_without_import.deleteExchanges('market group for electricity, high voltage')
    lca = act_elec_without_import.lca(method=climate, amount=1)
    impact_elec_without_import_1kWh = lca.score/(1-import_kWh["amount"])
    
    unit = bw2data.Method(climate).metadata["unit"]
    
    df.loc[len(df.index)] = [act_elec_1kWh["database"],import_kWh["amount"],ratio_impact,impact_elec_1kWh,impact_import_1kWh,impact_elec_without_import_1kWh,unit]
```

```python
def style_neg(v, props=''):
    return props if type(v)==float and v < 0 else None

df_import=df.style.map(style_neg, props='background-color:red;',subset='impact 1kWh without import')
df_import
```

## Impact of background activities used to model FR electricity market (depends on IAM model)

```python
#list_db_name=db_name_list('Base')
#list_db_name=db_name_list('RCP26')
list_db_name=db_name_list('RCP19')

list_db_name
```

```python
df=pd.DataFrame([],columns=['scenario','act','impact','unit'])
list_exc=[]
list_act=[]

for dbtoexplore_name in list_db_name:
    act_elec_1kWh=agb.findActivity("market for electricity, high voltage, Tr2050", db_name=dbtoexplore_name)
    list_act.append(act_elec_1kWh)

for act in list_act:
    excs=[exc.input for exc in act.exchanges() if exc["type"]=='technosphere' and exc["name"]!="market for electricity, high voltage, Tr2050"]
    for exc in excs:
#        if exc["name"] not in str(list_exc):
            list_exc.append(exc)

for exc in list_exc:
    lca = exc.lca(method=climate, amount=1)
    score = lca.score
    unit = bw2data.Method(climate).metadata["unit"]
    df.loc[len(df.index)] = [exc["database"],exc["name"],score,unit]
```

```python
def style_neg(v, props=''):
    return props if type(v)==float and v < 0 else None

```

```python
df_Base = df.style.map(style_neg, props='background-color:red;')
df_Base
```

```python
df_RCP26 = df.style.map(style_neg, props='background-color:red;')
df_RCP26
```

```python
df_RCP19_bis = df.style.map(style_neg, props='background-color:red;')
df_RCP19_bis
```

# Basic test


## Test ecoinvent

```python
elec_ecoinvent=agb.findActivity("market for electricity, high voltage", loc='FR', db_name=ecoinvent_3_9_db_name)
elec_ecoinvent
```

```python
agb.printAct(elec_ecoinvent)
```

```python
agb.compute_impacts(elec_ecoinvent,impacts)
```

## Test premise database

```python
dbtoexplore=db_name_list('S2','RCP19')[0]
dbtoexplore	
```

```python
elec_2050=agb.findActivity("market for electricity, high voltage, Tr2050", db_name=dbtoexplore)
```

```python
agb.printAct(elec_2050)
```

```python
agb.compute_impacts(elec_2050,impacts)
```

```python
pv_S2_RCP19=agb.findActivity("electricity production, photovoltaic", loc='FR', db_name=dbtoexplore)
```

```python
agb.compute_impacts(pv_S2_RCP19,impacts)
```

# Draft

```python

```
