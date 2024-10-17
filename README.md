# ca-mfa-ucsb-tnc

A California material flow analysis (MFA), in collaboration with The Nature Conservancy

## Installation

```r
# Install necessary packages
```

### Rough draft, going to try to explain how all the pieces come together and then I will clean this up. 

### County level disposal data
The CalRecycle Disposal Reporting System (DRS) reports county level disposed waste from 1995-2019. From 2020 onward, CalRecycle deployed the Recycling and Disposal Reporting System (RDRS). 

##### DRS data source
The Multiyear Countywide Origin Summary data was used for this analysis. 
Description: "Waste origin means the jurisdiction where the waste was produced". 
Link: https://www2.calrecycle.ca.gov/LGCentral/DisposalReporting/Origin/CountywideSummary

##### RDRS data source
RDRS Report 1: Overall Jurisdiction Tons for Disposal and Disposal Related Uses was used.
Description: "Public Report 1 summarizes tons by quarter or for a report year summed for solid waste disposed and green material reused beneficially for alternative daily cover (ADC) from California jurisdictions. Solid waste may be either 1) disposed at in-California landfills, transformation facilities, and engineered municipal solid waste (EMSW) conversion facilities, or 2) exported out of California. Within this report, exported solid waste is included within the “landfill” category."
Link: https://www2.calrecycle.ca.gov/RecyclingDisposalReporting/Reports/OverallJurisdictionTonsForDisposal


#### Data processing
Run in order.

```r
source(RDRS County Waste 2020.rmd)
source(DRS County Waste 2005_2019.rmd)
```

###### RDRS County Waste 2020 
Processes Report 1 raw data, file path: ca-mfa-ucsb-tnc/Waste Management Data/raw data/CalRecycle/RDRS/report 1 data
Raw data consists of a separate .xlsx for each county. The script pulls each file in the folder and combines into a large dataframe which is then cleaned up. The final output is stored in 'processed data':'RDRS Report 1 Data Summarized.xlsx'

The RDRS data is categorized as "landfill", "transformation", "emsw", "green material adc".
EMSW stands for Engineered Municipal Solid Waste, which consists of energy recovery from the transformation process. Alternative Daily Cover (ADC) was discarded from this analysis.


##### DRS County Waste 2005_2019
Processes DRS Countywide Origin raw data, file path: ca-mfa-ucsb-tnc/Waste Management Data/raw data/CalRecycle/DRS/Countywide Origin Flow
Raw data consists of a separate .xlsx for each county. The script pulls each file in the folder and combines into a large dataframe which is then cleaned up.
The DRS data is categorized as "disposal", "export", "transformation", and "adc".

RDRS Report 1 Data Summarized.xlsx is brought in to add 2020 to the 2005-2019 dataframe. 
Since the RDRS "landfill" waste category includes exported waste (see description) it is not consistent with the DRS dataset. Therefore, the proportion of export/(export+disposal) from 2019 is used to separate exported waste from the RDRS 2020 "landfill" data. Also, RDRS "transformation" and "emsw" columns are added together to be consistent with DRS. The final output is stored in 'processed data':'RDS Managed Waste by County.xlsx'

### Statewide Disposal Facility-based Waste Characterization Studies
CalRecycle Waste Characterization Studies (WCS) report statewide disposed waste estimates by material type. The WCS from 2004, 2008, 2014, 2018m and 2021 are used in this analysis to extract the proportion of plastic waste from total disposed waste. 

##### Disposal Facility-based WCS data sources
2004: https://www2.calrecycle.ca.gov/WasteCharacterization/PubExtracts/34004005/Tables.pdf
2008: https://www2.calrecycle.ca.gov/WasteCharacterization/PubExtracts/2009023/Tables.pdf
2014: https://www2.calrecycle.ca.gov/WasteCharacterization/PubExtracts/2014/SigTableFig.pdf
2018: https://www2.calrecycle.ca.gov/Publications/Details/1666
2021: https://www2.calrecycle.ca.gov/Publications/Details/1738


#### Data processing

```r
source(WCS disposal-based resin conversion.rmd)
```

The WCS categorizes waste by material type, including several plastic material types. This script reads in the raw data, cleans it up, and then converts the plastic material types using the Milbrandt et al (2022) resin conversion equations. Finally, the data are interpolated to provide diposed resin tonnage for each year 2005 to 2020. The final output is stored in "processed data" :"Converted WCS Resin Data Interpolated 2005-2020.xlsx"


### Combine the RDS county level disposed waste with the statewide WCS





###### 2014 Generator-based WCS:
https://www2.calrecycle.ca.gov/Publications/Details/1543


