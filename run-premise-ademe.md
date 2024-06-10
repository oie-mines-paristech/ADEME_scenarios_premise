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
* avec from premise_gwp import add_premise_gwp, add_premise_gwp()
* Je ne me souviens plus si j'ai encore overwrite la biosphere dans ce doc ou seulement après l export d ecoinvent

# Initialisation

```python
from premise import *
import bw2data
import bw2io
from datapackage import Package
```

# Open brightway project

```python
bw2data.projects.set_current("HySPI_premise_Tr2050_3")

#bw2data.projects.current

#Print the databases that are in your project
bw2data.databases
```

```python
ecoinvent_3_9_db_name='ecoinvent-3.9.1-cutoff'
ecoinvent_3_9_db = bw2data.Database('ecoinvent-3.9.1-cutoff')
```

```python
#del bw2data.databases["S3Nuc_2050_SSP2Base"]
```

```python
#from premise_gwp import add_premise_gwp
#add_premise_gwp()
```

# Load input data

```python
fp = r"datapackage.json"
ademe = Package(fp)
```

```python
#Datapackage relies on 3 resources files called
ademe.resource_names
```

```python
#Choose the scenario to generate
#IAM model
model="image"

#world scenario
world_scenario_1="SSP2-Base"
world_scenario_2="SSP2-RCP26"
world_scenario_3="SSP2-RCP19"

#Year
year=2050

#French scenario
fr_scenario_1="S1 - Frugal generation"
fr_scenario_2="S2 - Territorial cooperation"
fr_scenario_3="S3 Renew - Green technologies renewables"
fr_scenario_3bis="S3 Nuc - Green technologies nuclear"
fr_scenario_4="S4 - Repairing bet"
```

## For tests : exploration of one database 


### Generate the database

```python
scenarios = [
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_1, "data": ademe}]},
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
#Not mandatory for tests
ndb.write_db_to_brightway(name=['test_tobedeleted_3'])
```

## Explore nbd without printing the database to brightway (to save time) 

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
activity_name = "market for hydrogen, gaseous, for transport - direct use of H2, Tr2050"

for act in list_act:
    if activity_name==act["name"]:    
        print(act["location"])
```

```python
#Print all exchanges of a given activity
activity_name = "market for hydrogen, gaseous, for transport - direct use of H2, Tr2050"

for act in list_act:
    if activity_name==act["name"]:    
        for e in act["exchanges"]:
            amount=e["amount"]
            print(f"{amount:.2f}","|", e["unit"],"|", e["name"] ) 
```

```python
#Print all exchanges of a given activity
activity_name = "market for hydrogen, gaseous, for transport - direct use of H2, Tr2050"
#activity_name = "hydrogen production, steam methane reforming of natural gas, with CCS (MDEA, 98% eff.), 25 bar"
#activity_name = "hydrogen production, gaseous, 30 bar, from PEM electrolysis, from grid electricity"

for act in list_act:
    if activity_name==act["name"] and act["location"]=='FR':    
        for e in act["exchanges"]:
            amount=e["amount"]
            if e['type']=='technosphere':
                print(f"{amount:.2f}","|", e["unit"],"|", e["name"],"|", e["location"] ) 
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

# Generate databases for several scenarios 

```python
#Choose the scenarios to be modeled
scenarios = [
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_1, "data": ademe}]},
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_2, "data": ademe}]},
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_3, "data": ademe}]},
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_3bis, "data": ademe}]},
        {"model": model, "pathway":world_scenario_1, "year": year, "external scenarios": [{"scenario": fr_scenario_4, "data": ademe}]},
        {"model": model, "pathway":world_scenario_2, "year": year, "external scenarios": [{"scenario": fr_scenario_1, "data": ademe}]},
        {"model": model, "pathway":world_scenario_2, "year": year, "external scenarios": [{"scenario": fr_scenario_2, "data": ademe}]},
        {"model": model, "pathway":world_scenario_2, "year": year, "external scenarios": [{"scenario": fr_scenario_3, "data": ademe}]},
        {"model": model, "pathway":world_scenario_2, "year": year, "external scenarios": [{"scenario": fr_scenario_3bis, "data": ademe}]},
        {"model": model, "pathway":world_scenario_2, "year": year, "external scenarios": [{"scenario": fr_scenario_4, "data": ademe}]},
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_1, "data": ademe}]},
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_2, "data": ademe}]},
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_3, "data": ademe}]},
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_3bis, "data": ademe}]},
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_4, "data": ademe}]},
    ]
```

```python
scenarios = [
        {"model": model, "pathway":world_scenario_3, "year": year, "external scenarios": [{"scenario": fr_scenario_3bis, "data": ademe}]},
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
ndb.update() 

#This updates only the external French scenario. It does not work as we need updated European market for electricity 
#to model the imports for French Tr20250 markets.
#ndb.update(["external"]) 

#This updates electricity markets according to IAM scenarios + the external French scenario
#ndb.update(["electricity","external"])
```

```python
#fait une fois la première fois que j'ai fait un extract. La première fois ça a planté, j'ai relancé
#bw2io.create_default_biosphere3(overwrite=True)
```

```python
#Choose the name of the databases that are exported to brightway > list_db_name
#if world_scenario_1=="SSP2-Base":
list_db_name_1=[
         'S1_2050_SSP2Base',
         'S2_2050_SSP2Base',
         'S3Renew_2050_SSP2Base',
         'S3Nuc_2050_SSP2Base',
         'S4_2050_SSP2Base'
        ]

list_db_name_2=[
        'S1_2050_SSP2RCP26',
        'S2_2050_SSP2RCP26',
        'S3Renew_2050_SSP2RCP26',
        'S3Nuc_2050_SSP2RCP26',
        'S4_2050_SSP2RCP26']

list_db_name_3=[
        'S1_2050_SSP2RCP19',
        'S2_2050_SSP2RCP19',
        'S3Renew_2050_SSP2RCP19',
        'S3Nuc_2050_SSP2RCP19',
        'S4_2050_SSP2RCP19']
    

list_db_name_4=[
         'S1_2050_SSP2Base',
         'S2_2050_SSP2Base',
         'S3Renew_2050_SSP2Base',
         'S3Nuc_2050_SSP2Base',
         'S4_2050_SSP2Base',
        'S1_2050_SSP2RCP26',
        'S2_2050_SSP2RCP26',
        'S3Renew_2050_SSP2RCP26',
        'S3Nuc_2050_SSP2RCP26',
        'S4_2050_SSP2RCP26',        
        'S1_2050_SSP2RCP19',
        'S2_2050_SSP2RCP19',
        'S3Renew_2050_SSP2RCP19',
        'S3Nuc_2050_SSP2RCP19',
        'S4_2050_SSP2RCP19'
        ]
```

```python
list_db_name=list_db_name_4
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
db_to_explore_name='S1_2050_SSP2Base'
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
act=[act for act in bw2data.Database(db_to_explore_name) if "market for electricity, high voltage, Tr2050" in act["name"] and act["location"]=="FR"][0]
act
```

```python
exc = [exc for exc in act.exchanges()]
exc
#exc = [exc for exc in act.exchanges() if "wind" in e.input["name"]][0]  # ¡¡¡Nota: e.input et torna l'activitat!!!!
```

```python

```
