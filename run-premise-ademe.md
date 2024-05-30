---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.2
  kernelspec:
    display_name: hyspi-premise-210dev0
    language: python
    name: hyspi-premise-210dev0
---

* Open a brightway project where ecoinvent 3.9.1 + biosphere is already loaded as databases of the project
*  If you have a problem concerning matching of biosphere flows, run 
`bw2io.create_default_biosphere3(overwrite=True)`
* Code run with premise 2.1.0.dev0 (pip install premise==2.1.0.dev0) [to be updated and included in the readme later]


# Initialisation

```python
from premise import *
import bw2data
import bw2io
from datapackage import Package
```

# Open brightway project

```python
bw2data.projects.set_current("HySPI_premise_Tr2050_2")

#bw2data.projects.current

#Print the databases that are in your project
bw2data.databases
```

```python
ecoinvent_3_9_db_name='ecoinvent-3.9.1-cutoff'
ecoinvent_3_9_db = bw2data.Database('ecoinvent-3.9.1-cutoff')
```

```python
#del bw2data.databases["ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S2"]
```

# Generate a new database according to scenarios with 2.1.0 dev0 version

```python
fp = r"datapackage.json"
ademe = Package(fp)
```

```python
#Datapackage relies on 3 resources files called
ademe.resource_names
```

## Exploration of the database without printing the database to brightway (to save time)


### Generate the database

```python
#Choose the scenario to generate
#IAM model
model="image"

#IAM scenario
#world_scenario="SSP2-Base"
#world_scenario="SSP2-RCP26"
world_scenario="SSP2-RCP19"

#Year
year=2050

#French scenario
fr_scenario="S1 - Frugal generation"
#fr_scenario="S2 - Territorial cooperation"
#fr_scenario="S3 Renew - Green technologies renewables"
#fr_scenario="S3 Nuc - Green technologies nuclear"
#fr_scenario="S4 - Repairing bet"

scenarios = [
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
        ]
```

```python
ndb = NewDatabase(
        scenarios = scenarios,        
        source_db=ecoinvent_3_9_db_name,
        source_version="3.9.1",
        key='tUePmX_S5B8ieZkkM7WUU2CnO8SmShwmAeWK9x2rTFo=',
        use_multiprocessing=True
)
```

```python
#This updates all the markets (included in premise) according to IAM scenarios chosen + the external French scenario
#ndb.update() 

#This updates only the external French scenario. It does not work as we need updated European market for electricity 
#to model the imports for French Tr20250 markets.
#ndb.update(["external"]) 

#This updates electricity markets according to IAM scenarios + the external French scenario
ndb.update(["electricity","external"])
```

## Explore nbd without printing the database to brightway

```python
import numpy as np
```

## Explore 

```python
#clear_cache() #if too slow
```

```python
# Choose the scenario number to be explored
n_scenario=1

list_act=ndb.scenarios[n_scenario-1]["database"]
```

```python
#Print all the activities that contain a keyword
keyword="Tr2050"

for act in list_act:
    if keyword in act["name"]:    
        print(act["name"])
```

```python
#Print all exchanges of a given activity
activity_name = "market for electricity, high voltage, Tr2050"

for act in list_act:
    if activity_name==act["name"]:    
        for e in act["exchanges"]:
            amount=e["amount"]
            print(f"{amount:.2f}","|", e["unit"],"|", e["name"] ) 
```

### Explanations (can be skipped)

```python
#ndb.scenarios is a list. Its length equals the number of scenarios.
type(ndb.scenarios), len(ndb.scenarios)
```

```python
#ndb.scenarios[0] refers to the first scenario extracted.
ndb.scenarios[0].keys()
```

```python
#ndb.scenarios[0]["database"] is the list of all the activities (dict) contained in the generated database
list_act=ndb.scenarios[0]["database"]
act_test=list_act[0]
type(list_act),len(list_act),act_test.keys()
```

```python
#List of the keys of an exchange
act_test["exchanges"][0].keys()
```

# Generatenew databases with 2.1.0 dev0 version

```python
#Choose the scenario to generate
#IAM model
model="image

#world scenario
#world_scenario="SSP2-Base"
#world_scenario="SSP2-RCP26"
world_scenario="SSP2-RCP19"

#Year
year=2050

#French scenario
fr_scenario="S1 - Frugal generation"
#fr_scenario="S2 - Territorial cooperation"
#fr_scenario="S3 Renew - Green technologies renewables"
#fr_scenario="S3 Nuc - Green technologies nuclear"
#fr_scenario="S4 - Repairing bet"
```

```python
scenarios = [
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
        {"model": model, "pathway":world_scenario, "year": year, "external scenarios": [{"scenario": fr_scenario, "data": ademe}]},
    ]
```

```python
ndb = NewDatabase(
        scenarios = scenarios,        
        source_db=ecoinvent_3_9_db_name,
        source_version="3.9.1",
        key='tUePmX_S5B8ieZkkM7WUU2CnO8SmShwmAeWK9x2rTFo=',
        use_multiprocessing=True
)
```

```python
#This updates all the markets (included in premise) according to IAM scenarios chosen + the external French scenario
#ndb.update() 

#This updates only the external French scenario. It does not work as we need updated European market for electricity 
#to model the imports for French Tr20250 markets.
#ndb.update(["external"]) 

#This updates electricity markets according to IAM scenarios + the external French scenario
ndb.update(["electricity","external"])
```

```python
#Choose the name of the databases that are exported to brightway > list_db_name

S1_2050_SSP2base_db_name='S1_2050_SSP2base'
S2_2050_SSP2base_db_name='S2_2050_SSP2base'
S3Renew_2050_SSP2base_db_name='S3Renew_2050_SSP2base'
S3Nuc_2050_SSP2base_db_name='S3Nuc_2050_SSP2base'
S4_2050_SSP2base_db_name='S4_2050_SSP2base'

S1_2050_SSP2RCP26_db_name='S1_2050_SSP2RCP26'
S2_2050_SSP2RCP26_db_name='S2_2050_SSP2RCP26'
S3Renew_2050_SSP2RCP26_db_name='S3Renew_2050_SSP2RCP26'
S3Nuc_2050_SSP2RCP26_db_name='S3Nuc_2050_SSP2RCP26'
S4_2050_SSP2RCP26_db_name='S4_2050_SSP2RCP26'

S1_2050_SSP2RCP19_db_name='S1_2050_SSP2RCP19'
S2_2050_SSP2RCP19_db_name='S2_2050_SSP2RCP19'
S3Renew_2050_SSP2RCP19_db_name='S3Renew_2050_SSP2RCP19'
S3Nuc_2050_SSP2RCP19_db_name='S3Nuc_2050_SSP2RCP19'
S4_2050_SSP2RCP19_db_name='S4_2050_SSP2RCP19'

if world_scenario=="SSP2-Base":
    list_db_name=[
         S1_2050_SSP2base_db_name,
         S2_2050_SSP2base_db_name,
         S3Renew_2050_SSP2base_db_name,
         S3Nuc_2050_SSP2base_db_name,
         S4_2050_SSP2base_db_name
        ]
    
if world_scenario=="SSP2-RCP26":
    list_db_name=[
        S1_2050_SSP2RCP26_db_name,
        S2_2050_SSP2RCP26_db_name,
        S3Renew_2050_SSP2RCP26_db_name,
        S3Nuc_2050_SSP2RCP26_db_name,
        S4_2050_SSP2RCP26_db_name]

if world_scenario=="SSP2-RCP19":
    list_db_name=[
        S1_2050_SSP2RCP19_db_name,
        S2_2050_SSP2RCP19_db_name,
        S3Renew_2050_SSP2RCP19_db_name,
        S3Nuc_2050_SSP2RCP19_db_name,
        S4_2050_SSP2RCP19_db_name]
    
list_db_name
```

```python
ndb.write_db_to_brightway(
    name=list_db_name
    )
```

```python
bw2data.databases
```

### Explore the new databases

```python
db_to_explore_name=S1_2050_SSP2base_db_name
#S2_2050_SSP2base_db_name
#S3Renew_2050_SSP2base_db_name
#S3Nuc_2050_SSP2base_db_name
#S4_2050_SSP2base_db_name
```

```python
acts=[act for act in bw2data.Database(db_to_explore_name) if "Tr2050" in act["name"]]
```

```python
acts
```

```python
act=[act for act in bw2data.Database(S1_2050_SSP2base_db_name) if "market for electricity, low voltage, Tr2050" in act["name"] and act["location"]=="FR"][0]
act
```

```python
exc = [exc for exc in act.exchanges()]
exc
#exc = [exc for exc in act.exchanges() if "wind" in e.input["name"]][0]  # ¡¡¡Nota: e.input et torna l'activitat!!!!
```
