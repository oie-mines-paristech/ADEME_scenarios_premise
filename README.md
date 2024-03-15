# Futurs énergétiques 2050 (Energy future 2050) / RTE
Implementation of ADEME scenarios Transition(s) 2050 into ecoinvent database with premise


Description
-----------

This is a repository containing the implementation of the prospective scenarios provided by the 
French Agency for Ecological Transition - ADEME - into ecoinvent for the following sectors in France :

* electricity
* hydrogen
* other liquid and gaseous fuels

It is meant to be used in [`premise`](https://github.com/polca/premise), optionally in addition to a global IAM scenario, to provide 
refined projections at the country level.

This data package contains all the files necessary for `premise` to implement
this scenario and create market-specific composition for electricity (including imports from
neighboring countries), liquid and gaseous fuels (including hydrogen) in the LCA database ecoinvent.

The data relating to the annual production volumes of different energy carriers 
(e.g. electricity, hydrogen) for each Transtion(s) 2050 scenario (6 in number) 
have been formatted and organised in a data package defined by the Frictionless standards 
(Walsh and Pollock, 2022). This data package is read and interpreted by `premise`. 
We therefore store a number of scenarios in a single data package. 

This datapackage contains four files necessary to the scenarios implementation into the ecoinvent LCA database: 

* A datapackage.json file, which provides the metadata for the data package (e.g. authors, scenario descriptions, list and locations of resources, etc.). 
* A config.yaml file which provides the correspondence between the scenario variables and the LCA datasets in the ecoinvent DB, as well as the "LCA datasets to be created" as they are not available in the ecoinvent DB. 
* A tabular data file containing the time series for each variable in the set of scenarios. 
* An optional text file containing the LCA inventories of the "LCA datasets to be created" for any technology not initially present in the ecoinvent DB. 


Sourced from publication
------------------------

Projections are extracted from:

Transition(s) 2050\
ADEME\
[`Website`](https://www.rte-france.com/analyses-tendances-et-prospectives/bilan-previsionnel-2050-futurs-energetiques) \
[`Full Report`](https://librairie.ademe.fr/recherche-et-innovation/5072-prospective-transitions-2050-rapport.html) \
[`Data repository`](https://data-transitions2050.ademe.fr/)



Authors of this data package
----------------------------

* Joanna Schlesinger (joanna.schlesinger@minesparis.psl.eu)
* Romain Sacchi (romain.sacchi@psi.ch)
* Juliana Steinbach (juliana.steinbach@minesparis.psl.eu)
* Thomas Beaussier (thomas.beaussier@minesparis.psl.eu)
* Paula Perez-Lopez (paula.perez_lopez@minesparis.psl.eu)

Funding
-------

This work is supported by the ADEME agency, in the context of
the HYSPI project (https://www.psi.ch/en/ta/projects/hyspi).

Ecoinvent database compatibility
--------------------------------

ecoinvent 3.9.1 cut-off


Prospective scenarios
---------------------------
ADEME provides 6 scenarios : 
* S1 : Frugal generation (Génération Frugale)
* S2 : Territorial cooperation (Coopération territoriale)
* S3 Renew : Green technologies based on renewables development (Technologies vertes)
* S3 Nuc : Green technologies based on nuclear development (Technologies vertes)
* S4 : Repairing bet (Pari réparateur)
* TEND : a trend-based scenario


IAM scenario compatibility
---------------------------

The following coupling is done between IAM and FE2050+ scenarios:

| IAM scenario            | FE2050+ scenario                       |
|-------------------------|----------------------------------------|
| IMAGE SSP2-Base         | TEND                                   |
| IMAGE SSP2-Base         | S1                                     |
| IMAGE SSP2-Base         | S2                                     |
| IMAGE SSP2-Base         | S3 Renew                               |
| IMAGE SSP2-Base         | S3 Nuc                                 |
| IMAGE SSP2-Base         | S4                                     |


This pairing was done solely based on the author's opinions and does not reflect 
any official coupling between the two scenarios. This is to prevent coupling
between scenarios that are not compatible (e.g. coupling between a global scenario with
a high carbon price and a national scenario with a low carbon price).
