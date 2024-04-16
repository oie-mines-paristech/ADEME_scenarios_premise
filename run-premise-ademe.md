---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.0
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
ademe.resource_names
```

```python
S1_2050_SSP2base_db_name='ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S1'
S2_2050_SSP2base_db_name='ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S2'
S3Renew_2050_SSP2base_db_name='ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S3Renew'
S3Nuc_2050_SSP2base_db_name='ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S3Nuc'
S4_2050_SSP2base_db_name='ecoinvent_cutoff_3.9_image_SSP2-Base_2050_S4'
```

## for several scenarios

```python
scenarios = [
        {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S1 - Frugal generation", "data": ademe}]},
        {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S2 - Territorial cooperation", "data": ademe}]},
        {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S3 Renew - Green technologies renewables", "data": ademe}]},
        {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S3 Nuc - Green technologies nuclear", "data": ademe}]},
        {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S4 - Repairing bet", "data": ademe}]},
    ]
```

<!-- #region -->
scenarios = [
    {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S4 - Repairing bet", "data": ademe}]},
    ]



scenarios = [
    {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S3 Nuc - Green technologies nuclear", "data": ademe}]},
    ]

scenarios = [
    {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S3 Renew - Green technologies renewables", "data": ademe}]},
    ]



scenarios = [
    {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S2 - Territorial cooperation", "data": ademe}]},
]

scenarios = [
    {"model": "image", "pathway":"SSP2-Base", "year": 2050, "external scenarios": [{"scenario": "S1 - Frugal generation", "data": ademe}]},
    ]
<!-- #endregion -->

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
ndb.update()
```

```python
#it does not work with ndb.update("external") only
#ndb.update("external")
```

```python
ndb.write_db_to_brightway(
    name=[
S1_2050_SSP2base_db_name,
S2_2050_SSP2base_db_name,
S3Renew_2050_SSP2base_db_name,
S3Nuc_2050_SSP2base_db_name,
S4_2050_SSP2base_db_name,
        ]
    )
```

```python
bw2data.databases
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
