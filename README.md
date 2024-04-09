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
the [`HYSPI project`](https://www.psi.ch/en/ta/projects/hyspi).

Ecoinvent database compatibility
--------------------------------

ecoinvent 3.9.1 cut-off


Prospective scenarios
---------------------------
ADEME provides 6 scenarios : 
* S1 : Frugal generation (Génération Frugale) > The transition is driven mostly by sobriety and constraint.
* S2 : Territorial cooperation (Coopération territoriale) > The society transformation is based on a shared governance and on territorialization strategies.
* S3 Renew : Green technologies based on renewables development (Technologies vertes) > The transition is based on innovation and development of low carbon technologies, especially renewable energies.  
* S3 Nuc : Green technologies based on nuclear development (Technologies vertes) > The transition is based on innovation and development of low carbon technologies, especially nuclear energy.  
* S4 : Repairing bet (Pari réparateur) > The transition relies highly on new technologies development, without any society lifestyle changes. 
* TEND : a trend-based scenario

Informations about scenarios : [`here in French`](https://www.ademe.fr/les-futurs-en-transition/les-scenarios/)

IAM scenario compatibility
---------------------------

[!!!!!!!! to be updated]
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

This external scenario creates electricity, hydrogen and fuel markets for France listed below, according
to the projections from the ADEME's Transition(s) 2050 (yellow boundaries in map above).


Electricity
------------------
\
Input Data
*****


**Sources**
* The data used are not the ones provided in the report but was directly provided by ADEME. This data was computed with an assumption of 39 GW of interconnexion (compared with 45 GW initially). The data was computed with this new assumption to fit better with RTE projections. 
* Discussions with ADEME experts

\
How the data provided by ADEME has been modified ?
*****

**Production data**

###### Hydro electricity production data
As the data provided by ADEME is not divided in reservoir hydro and run-of-river hydro, the actual percentage of repartition between both technologies as been applied to generate production data for both technologies. The percentage has been calculated based on market for electricity, high voltage (FR) taken from ecoinvent 3.10 : 84% for run-of-river and 16% for reservoir technologies.

###### 2020 production data
In the data file provided by ADEME, the 2020 data is not the same for Combined Cycle Gaz Technology (0.8 TWh/year to 2.2 TWh/year). The data has not been changed. 

###### Exports modelling : change of production volumes of "primary" electricity
As we aim to model the electricity mix consumed in France, the production data used has thus been resized by substracting the exports from the volumes of "primary" electricity produced. Modeling the exports this way involves that : 
* the exports are taken from the "primary" electricity production (ie the electricity that is not consumed then injected by flexibility technologies).
* the electricity consumed then injected by flexibility technologies is only dedicated to French electricity market.

The following market datasets are created:

* `market for electricity, high voltage, Tr2050` (FR)
* `market for electricity, medium voltage, Tr2050` (FR)
* `market for electricity, medium voltage, Tr2050` (FR)

These markets are relinked to activities that consume electricity in France.

\
Fuels
*****


Hydrogen
------------------
\
Input Data
*****
**Sources**
* Chapter 3 (production d'énergie) / section 5 of [`Full Report`](https://librairie.ademe.fr/recherche-et-innovation/5072-prospective-transitions-2050-rapport.html)
* Excel files hydrogene-g1 to g8 from [`Data repository`](https://data-transitions2050.ademe.fr/)
* Discussions with ADEME experts

**Remarks about input data**
* All the quantities of hydrogen are given in TWh LHV (Low Heating Value)
* The hydrogen imported (scenario S3) is produced by electrolysis using renewable electricity (solar, wind) [Full report, p.529]. It is modeled as the hydrogen produced by electrolysis using French electricity mix S3. This modeling is more representative for S3 Renew than for S3 Nuc.
* The hydrogen produced from gas in scenario S4 is produced by SMR coupled with Carbon Capture and Storage technology [Full report, p.530] In other scenarios, the hydrogen produced from gas is produced by SMR without Carbon Capture and Storage.
* The efficiency considered for electrolysis changes over time (variable Efficiency|Hydrogen|Electrolyzer) is 0.65 in 2030 and 0.72 in 2050 [Source: ADEME experts]. An efficiency of 0.61 was chosen for 2019 [!!!!!!!!!!! to be discussed]

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

**Specifications of hydrogen markets**
* The market for transport use includes only direct use of hydrogen for transportation. 
* The market for power-to-liquid covers the hydrogen production that is then used to produce synthetic fuels / e-fuels. 
* The market for power-to-gaz covers the hydrogen production that is then used to produce methane by methanation process that is then injected in the gaz grid.
* The market for hydrogen is an assembly of all the markets modeled and thus covers all the modeled sector.
* The only sector for which co-production is considered is refinery of fossil fuel. It means that for other sectors (such as ammonia, of chemicals) the market modeled includes only the hydrogen that is produced in addition to co-production from this sector.
* The uses of hydrogen for each markets are explained in details in [Full Report, p. 520]. 

\
Hydrogen production inventories
******
The datasets listed below are used to supply the above-listed markets:

| Technologies in Tr2050                        | LCI datasets used                                                       | 
|-----------------------------------------------|-------------------------------------------------------------------------|
| Hydrogen, from electrolysis                   | hydrogen production, gaseous, 30 bar, from PEM electrolysis, from grid electricity, domestic |
| Hydrogen, from SMR of NG                      | hydrogen production, steam methane reforming of natural gas, 25 bar     |
| Hydrogen, from SMR of NG + CCS                | hydrogen production, steam reforming  [!!! Jo to update]                |
| Hydrogen, co-product for fossil fuel refinery | hydrogen production, gaseous, petroleum refinery operation              |

\
Market inventories
******

Each market inventory is composed of the following flows :
* One or several hydrogen production inventories. These inventories and their amount are specific for each market based on shares provided in Transition(s) 2050.
* 5 inventories modelling the transport (by pipeline) and the storage (geological). The electricity flow represents the electricity used to generate pressure in pipeline [!!! to be discussed with romain]. This activities are similar for each market.
* A technosphere flow of itself and a hydrogen biosphere emission flow to model hydrogen losses among the distribution and storage value chain. This losses variable is static and considers 0.005% losses. 

[!!!!!!!!!!! to be discussed] These markets are relinked to activities that consume hydrogen in France, according to their area of application. 




\
To do Jo 
*****

* Elec: main difference with RTE : hydrogen not used to balance elec mix + voir notes réu ADEME. 

* how is the location of the reference ecoinvent flow chosen ? 

* new techno compared with FE2050 :
  * biogaz_chp : heat and power co-generation, biogas, gas engine
  * geothermal :electricity production, deep geothermal
  * gaz_chp : heat and power co-generation, natural gas, conventional power plant, 100MW electrical

* Nuclear = Pressurised water for now as we do not have disagregated data btw EPR and PWR (no SMR in the scenarios).
No SMR in all scenarios. No new nuclear (except Flamanville) in S1 S2 S3 Renew. S3Nuc and S4, new EPR nuclear.  

* gaz : Je pense que pour OCGT et CCGT je vais mettre un mix de gaz (biogaz + gaz nat + e-methane) et pour Autres thermiques, je vais mettre uniquement du gaz nat (sauf en 2020 avec un peu de fioul, je vais réfléchir comment désagréger la donnée) et pour CHP biogaz uniquement du biogaz. 

* how to make appear fioul (only in 2020 ?) Use data from RTE ? Use data from feuilleton mix elec p. 15 sur capa gaz
JM Parouffe : dans la catégorie «  CHP fioul, gaz », il n’y en a pas alimentées au fioul, sauf en 2020. 

* PV is not at low voltage
  
* Disagregated data not used yet 
  * Gaz OCGT CCGT but OCGT is really low
  * PV not disagregated for now. level 1 : roof / ground. Level 2 : roof large / roof small ; ground fixed / ground tracker
  * fixed vs floating wind = offshore total
  * Where does PV inventory come from ? 

* Hydro pumped storage : efficiency variation (how does efficency works ?)
why is Efficiency|Storage|Hydro|Pumped storage used in production pathways and Efficiency|Storage|Battery|Charge used in markets ? 

* medium_to_high et low_to_medium ??
* to do : connect Production|Electricity|Conventional|Gaz|Cycle Turbines total to the new gaz market.

* Aknoledgment : JM Parouffe


