# Transition(s) 2050 / ADEME
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
[`Website`](https://www.ademe.fr/les-futurs-en-transition/) \
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

| IAM scenario            | Tr2050  scenario                       |
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


What does this do?
------------------

![map electricity markets](assets/map.png)

This external scenario creates electricity and fuel markets for France listed below, according
to the projections from the ADEME's Transition(s) 2050 (yellow boundaries in map above).
Imports of electricity are provided by the regional IAM market for European electricity (blue boundaries in map above).

Electricity
***********

Fuels
*****


Hydrogen
********

**Input Data**

Sources
* Chapter 3 (production d'énergie) / section 5 of [`Full Report`](https://librairie.ademe.fr/recherche-et-innovation/5072-prospective-transitions-2050-rapport.html)
* Excel files hydrogene-g1 to g8 from [`Data repository`](https://data-transitions2050.ademe.fr/)
* Discussions with ADEME experts

Remarks about input data :
* All the quantities of hydrogen are given in TWh LHV (Low Heating Value)
* The hydrogen imported (scenario S3) is produced by electrolysis using renewable electricity (solar, wind) [Full report, p.529]. It is modeled as the hydrogen produced by electrolysis using French electricity mix S3. This modeling is more representative for S3 Renew than for S3 Nuc.
* The hydrogen produced from gas in scenario S4 is produced by SMR coupled with Carbon Capture and Storage technology [Full report, p.530] In other scenarios, the hydrogen produced from gas is produced by SMR without Carbon Capture and Storage.
* The efficiency considered for electrolysis changes over time (variable Efficiency|Hydrogen|Electrolyzer) is 0.65 in 2030 and 0.72 in 2050 [Source: ADEME experts]. An efficiency of 0.61 was chosen for 2019 [!!!!!!!!!!! to be discussed with Romain]

**Market created**

The following markets for hydrogen are created :
* `market for hydrogen, gaseous, for transport - direct use of H2, Tr2050` (FR)
* `market for hydrogen, gaseous, for fossil fuel refinery use, Tr2050` (FR)
* `market for hydrogen, gaseous, for biofuel refinery use, Tr2050` (FR)
* `market for hydrogen, gaseous, for power to liquid use, Tr2050` (FR)
* `market for hydrogen, gaseous, for ammonia use, Tr2050` (FR)
* `market for hydrogen, gaseous, for steel use, Tr2050` (FR)
* `market for hydrogen, gaseous, for chemicals use, Tr2050` (FR)
* `market for hydrogen, gaseous, for power to gaz, Tr2050` (FR)
* `market for hydrogen, gaseous, Tr2050` (FR)

Specifications
* The market for transport use includes only direct use of hydrogen for transportation. 
* The market for power-to-liquid covers the hydrogen production that is then used to produce synthetic fuels / e-fuels. 
* The market for power-to-gaz covers the hydrogen production that is then used to produce methane by methanation process that is then injected in the gaz grid.
* The market for hydrogen is an assembly of all the markets modeled and thus covers all the modeled sector.
* The only sector for which co-production is considered is refinery of fossil fuel. It means that for other sectors (such as ammonia, of chemicals) the market modeled includes only the hydrogen that is produced in addition to co-production from this sector.
* The uses of hydrogen for each markets are explained in details in [Full Report, p. 520]. 

**Hydrogen production inventories**
The datasets listed below are used to supply the above-listed markets:

| Technologies in Tr2050                        | LCI datasets used                                                       | 
|-----------------------------------------------|-------------------------------------------------------------------------|
| Hydrogen, from electrolysis                   | hydrogen production, gaseous, 30 bar, from PEM electrolysis, from grid electricity, domestic |
| Hydrogen, from SMR of NG                      | hydrogen production, steam methane reforming of natural gas, 25 bar     |
| Hydrogen, from SMR of NG + CCS                | hydrogen production, steam reforming  [!!! Jo to update]                |
| Hydrogen, co-product for fossil fuel refinery | hydrogen production, gaseous, petroleum refinery operation              |


**Market inventories** 
Each market inventory is composed of the following flows :
* One or several hydrogen production inventories. These inventories and their amount are specific for each market based on shares provided in Transition(s) 2050.
* 5 inventories modelling the transport (by pipeline) and the storage (geological). The electricity flow represents the electricity used to generate pressure in pipeline [!!! to be discussed with romain]. This activities are similar for each market.
* A technosphere flow of itself and a hydrogen biosphere emission flow to model hydrogen losses among the distribution and storage value chain. This losses variable is static and considers 0.005% losses. 

[!!!!!!!!!!! to be discussed with Romain] These markets are relinked to activities that consume hydrogen in France, according to their area of application. 
