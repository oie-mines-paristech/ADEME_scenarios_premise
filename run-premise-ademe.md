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
bw2data.projects.set_current("HySPI_premise_Tr2050_1")

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

# Generate a new version of ecoinvent according to scenarios with 2.1.0 dev0 version

```python
fp = r"datapackage.json"
ademe = Package(fp)
```

```python
#Datapackage relies on 3 resources files called
ademe.resource_names
```

```python
#Choose the world scenario
#world_scenario="SSP2-Base"
#world_scenario="SSP2-RCP26"
world_scenario="SSP2-RCP19"
```

```python
S1_2050_SSP2base_db_name='S1_2050_SSP2base'
S2_2050_SSP2base_db_name='S2_2050_SSP2base'
S3Renew_2050_SSP2base_db_name='S3Renew_2050_SSP2base'
S3Nuc_2050_SSP2base_db_name='S3Nuc_2050_SSP2base'
S4_2050_SSP2base_db_name='S4_2050_SSP2base'
```

```python
S1_2050_SSP2RCP26_db_name='S1_2050_SSP2RCP26'
S2_2050_SSP2RCP26_db_name='S2_2050_SSP2RCP26'
S3Renew_2050_SSP2RCP26_db_name='S3Renew_2050_SSP2RCP26'
S3Nuc_2050_SSP2RCP26_db_name='S3Nuc_2050_SSP2RCP26'
S4_2050_SSP2RCP26_db_name='S4_2050_SSP2RCP26'
```

```python
S1_2050_SSP2RCP19_db_name='S1_2050_SSP2RCP19'
S2_2050_SSP2RCP19_db_name='S2_2050_SSP2RCP19'
S3Renew_2050_SSP2RCP19_db_name='S3Renew_2050_SSP2RCP19'
S3Nuc_2050_SSP2RCP19_db_name='S3Nuc_2050_SSP2RCP19'
S4_2050_SSP2RCP19_db_name='S4_2050_SSP2RCP19'
```

```python
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

## for several scenarios

```python
scenarios = [
        {"model": "image", "pathway":world_scenario, "year": 2050, "external scenarios": [{"scenario": "S1 - Frugal generation", "data": ademe}]},
        {"model": "image", "pathway":world_scenario, "year": 2050, "external scenarios": [{"scenario": "S2 - Territorial cooperation", "data": ademe}]},
        {"model": "image", "pathway":world_scenario, "year": 2050, "external scenarios": [{"scenario": "S3 Renew - Green technologies renewables", "data": ademe}]},
        {"model": "image", "pathway":world_scenario, "year": 2050, "external scenarios": [{"scenario": "S3 Nuc - Green technologies nuclear", "data": ademe}]},
        {"model": "image", "pathway":world_scenario, "year": 2050, "external scenarios": [{"scenario": "S4 - Repairing bet", "data": ademe}]},
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
#ndb.update()
ndb.update(["electricity","external"])
```

```python
#it does not work with ndb.update("external") only because we need to update the European electricity market 
#which is used to model the FR electricity mix
```

```python
ndb.write_db_to_brightway(
    name=list_db_name
    )
```

```python
bw2data.databases
```

```python

```

## Explore nbd without wrint the database to brightway

```python
import numpy as np
```

```python
for ds in ndb.scenarios[0]["database"]:
    for e in ds["exchanges"]:
        if np.isnan(e["amount"]):
            print(e)
```

```python
for ds in ndb.scenarios[0]["database"]:
    if "Tr2050" in ds["name"]:    
        print(ds["name"])
```

```python
for ds in ndb.scenarios[0]["database"]:
    if "market for electricity, high voltage, Tr2050" in ds["name"]:    
        for e in ds["exchanges"]:
            print(e["name"], e["amount"])
```

```python
for ds in ndb.scenarios[4]["database"]:
    if "market for electricity, high voltage, Tr2050" in ds["name"]:    
        for e in ds["exchanges"]:
            print(e["name"], e["amount"])
```

```python
for ds in ndb.scenarios[0]["database"]:
    if "Tr2050" in ds["name"]:      
        for e in ds["exchanges"]:
            print(e["name"], e["amount"])
```

# Explore the new database

```python
S1_2050_SSP2base_db_name
S2_2050_SSP2base_db_name
S3Renew_2050_SSP2base_db_name
S3Nuc_2050_SSP2base_db_name
S4_2050_SSP2base_db_name
```

```python
acts=[act for act in bw2data.Database(S1_2050_SSP2base_db_name) if "Tr2050" in act["name"]]
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
