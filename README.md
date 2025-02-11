# Transition(s) 2050 / ADEME
Implementation of French prospective scenarios from ADEME study "Transition(s) 2050" into ecoinvent database with premise


What does this repository do ?
-----------

This is a repository containing the implementation of prospective scenarios for France into ecoinvent. 
The prospective scenarios are provided in the "Transition(s) 2050" study by the French Agency for Ecological Transition - ADEME.   
The scope of ADEME prospective study is France, from now to 2050, and covers most of the economic sectors.
This repository does not cover the whole scope of ADEME study and creates market-specific activities in the LCA database ecoinvent for the following sectors :
* electricity
* hydrogen
* gas


ADEME prospective study 
------------------------

Prospective scenarios are extracted from the study : Transition(s) 2050, ADEME, 2021\
[`Website`](https://www.ademe.fr/les-futurs-en-transition/) \
[`Full Report`](https://librairie.ademe.fr/recherche-et-innovation/5072-prospective-transitions-2050-rapport.html) \
[`Data repository`](https://data-transitions2050.ademe.fr/)

ADEME provides 5 scenarios : 
* S1 : Frugal generation (Génération Frugale) > The transition is driven mostly by sobriety and constraint.
* S2 : Territorial cooperation (Coopération territoriale) > The society transformation is based on a shared governance and on territorialization strategies.
* S3 Renew : Green technologies based on renewables development (Technologies vertes) > The transition is based on innovation and development of low carbon technologies, especially renewable energies.  
* S3 Nuc : Green technologies based on nuclear development (Technologies vertes) > The transition is based on innovation and development of low carbon technologies, especially nuclear energy.  
* S4 : Repairing bet (Pari réparateur) > The transition relies highly on new technologies development, without any society lifestyle changes. 

Informations about scenarios : [`here in French`](https://www.ademe.fr/les-futurs-en-transition/les-scenarios/)

How is the repository organized ?
-----------

This repository is meant to be used with the open-source python library [`premise`](https://github.com/polca/premise), 
optionally in addition to a global IAM scenario, to provide refined projections at the country level.

The data relating to the annual production volumes of different energy carriers 
(e.g. electricity, hydrogen) for each scenario 
have been formatted and organised in a data package defined by the Frictionless standards 
(Walsh and Pollock, 2022). This data package is read and interpreted by `premise`. 
We therefore store a number of scenarios in a single data package.

This datapackage contains four files necessary to the scenarios implementation into the ecoinvent LCA database: 

* A datapackage.json file, which provides the metadata for the data package (e.g. authors, scenario descriptions, list and locations of resources, etc.). 
* A config.yaml file which provides the correspondence between the scenario variables and the LCA datasets in the ecoinvent DB, as well as the additional "LCA datasets" when they are not available in the ecoinvent DB. 
* A tabular data file containing the time series for each variable in the set of scenarios. 
* An optional Excel file containing the LCA inventories of the additional "LCA datasets" for any technology not initially present in the ecoinvent DB. 

Additionaly, a pdf document called "supplementary information" presents the methodological choices that where made to build this model.

How to use this notebook ?
------------------

To be completed. 

Explore the database
------------------

To be completed. 

The following market datasets are created: (search 'Tr2050')

* `market for electricity, high voltage, Tr2050` (FR)
* `market for electricity, medium voltage, Tr2050` (FR)
* `market for electricity, low voltage, Tr2050` (FR)



Ecoinvent database compatibility
--------------------------------
ecoinvent 3.9.1 cut-off

IAM scenario compatibility
---------------------------
The ADEME French scenarios can be connected to any Integrated Assessment Model (IAM) provided by premise
[`Link to explore IAM scenarios`](https://premisedash-6f5a0259c487.herokuapp.com/)  


Authors of this data package
----------------------------

* Joanna Schlesinger (joanna.schlesinger@minesparis.psl.eu)
* Romain Sacchi (romain.sacchi@psi.ch)
* Juliana Steinbach (juliana.steinbach@minesparis.psl.eu)
* Thomas Beaussier (thomas.beaussier@minesparis.psl.eu)
* Paula Perez-Lopez (paula.perez_lopez@minesparis.psl.eu)


Aknowledgements
----------------------------
We would like to thank ADEME experts for providing datasets and explanations to understand sceanrios and datasets. Special thanks to Jean-Michel Parrouffe.\
We also would like to thank Guillaume Batot from IFPEN for discussing the methodological choices made to build this model.


Funding
-------
This work is supported by the ADEME agency, in the context of
the [`HYSPI project`](https://www.psi.ch/en/ta/projects/hyspi). 




  

Fuels and gaz
------------------


Hydrogen
------------------
\
Few informations about scenarios
*****
* There is only one hydrogen scenario for S3 that is used both for S3 Renew and S3 Nuc.
* In S2 and S3, the hydrogen production in 2050 relies only on electrolysis whereas in S1 and S4, the hydrogen production in 2050 relies booth on electrolysis and steam methane reforming (of decarbonised gaz in S1 and of gas associated with Carbon Capture and Storage technology in S4).
* S3 is the only scenario that considers hydrogen imports (produced by electrolysis with renewable electricity).
* In S2, the hydrogen production by electrolysis is decentralised on few industrial sites without storage, whereas in S3, the production is more centralized, there are storage and grid facilities. In S1 and S4, as most of the production is from gas, there is no storage facilities.
* Note : in this model, the hydrogen storage and grid facilities are modelled in the same way in all scenarios. 


\
Input Data
*****
**Data Sources**
* Chapter 3 (production d'énergie) / section 5 of [`Full Report`](https://librairie.ademe.fr/recherche-et-innovation/5072-prospective-transitions-2050-rapport.html)
* Excel files hydrogene-g1 to g8 from [`Data repository`](https://data-transitions2050.ademe.fr/)
* Discussions with ADEME experts

**Remarks about input data**
* Hydrogen data is provided for 2019, 2030 and 2050. We assumed data of 2020 is similar as data for 2019. 
* All the quantities of hydrogen are given in TWh LHV (Low Heating Value)
* The hydrogen imported (scenario S3) is produced by electrolysis using renewable electricity (solar, wind) [Full report, p.529]. It is modeled as the hydrogen produced by electrolysis using French electricity mix S3. This modeling is more representative for S3 Renew than for S3 Nuc.
* The hydrogen produced from gas in scenario S4 is produced by SMR coupled with Carbon Capture and Storage technology [Full report, p.530] In other scenarios, the hydrogen produced from gas is produced by SMR without Carbon Capture and Storage.
* The efficiency considered for electrolysis changes over time (variable Efficiency|Hydrogen|Electrolyzer) is 0.65 in 2030 and 0.72 in 2050 [Source: ADEME experts]. An efficiency of 0.61 was chosen for 2019 (that corresponds to the reference inventory loaded). 

\
Hydrogen production inventories
******
The inventories datasets listed below are used to model the different ways of producing H2:
* For the direct production of hydrogen :

| Technologies in Tr2050        | LCI datasets used                                                       | Source | 
|-------------------------------|-------------------------------------------------------------------------| -------|
| Electrolysis                  | hydrogen production, gaseous, 30 bar, from PEM electrolysis, from grid electricity, domestic, FE2050 - FR | Dataset created for this datapackage, adapted from https://doi.org/10.1016/j.est.2021.102759 |
| Steam methane reforming       | hydrogen production, steam reforming of natural gas, 25 bar - FR | Dataset created for this datapackage, adapted from https://doi.org/10.1039/D0SE00222D |
| Steam methane reforming + Carbone Capture and Storage| hydrogen production, steam methane reforming of natural gas, with CCS (MDEA, 98% eff.), 25 bar - FR | Dataset created for this datapackage, adapted from https://doi.org/10.1039/D0SE00222D |

* For the production of hydrogen as a co-product, that is then consumed by this sector (fossil fuel refiner sector) :
| Technologies in Tr2050                        | LCI datasets used                                            | Source          | 
|-----------------------------------------------|--------------------------------------------------------------| ----------------|
| Hydrogen, co-product for fossil fuel refinery | hydrogen production, gaseous, petroleum refinery operation - FR| ecoinvent 3.9.1 |


For more inventories on Hydrogen : https://premise.readthedocs.io/en/latest/extract.html#hydrogen


\
**To do later**
* connect h2 production from gas to the new gaz market.

\
Markets created
*****
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

Note : in some scenarios, some markets are not created as the production for this market is equal to zero (eg, in S1 : `market for hydrogen, gaseous, for steel use, Tr2050` and `market for hydrogen, gaseous, for power to liquid use, Tr2050` are not created).

**Specifications for hydrogen markets**
* The market for transport use includes only direct use of hydrogen for transportation. 
* The market for power-to-liquid covers the hydrogen production that is then used to produce synthetic fuels / e-fuels. 
* The market for power-to-gaz covers the hydrogen production that is then used to produce methane by methanation process that is then injected in the gaz grid.
* The market for hydrogen is an assembly of all the markets modeled and thus covers all the modeled sector.
* The only sector for which co-production is considered is refinery of fossil fuel. It means that for other sectors (such as ammonia, of chemicals) the market modeled includes only the hydrogen that is produced in addition to co-production from this sector.
* The uses of hydrogen for each markets are explained in details in [Full Report, p. 520]. 

\
Market inventories
******
Each market inventory is composed of the following flows :
* One or several hydrogen production inventories. These inventories and their amount are specific for each market based on shares provided in Transition(s) 2050.
* 5 inventories modelling the transport (by pipeline) and the storage (geological). The electricity flow represents the electricity used for the process (compression) [!!! Ref to be added when it is published]. This activities are similar for each market and each scenarios.  
* A technosphere flow of itself and a hydrogen biosphere emission flow to model hydrogen losses among the distribution and storage value chain. This losses variable is static and considers 0.005% losses. 

[!!! To be changed if Romain does it in a different way for FE2050] These markets are not relinked to activities that consume hydrogen in France, as the created market did not exist in the original database. 





   











