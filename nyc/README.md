
# Introduction 

This document will provide tidbits of information and explain any code that is uploaded to NYC. 

# Counties 

The FIPS Code for New York state is 36.

Name | FIPS County Code  | Borough Num | ZTRAX County |
---- | ---------------- | ----------- | ------------
Bronx | 5 | 2 | BRONX
Kings/Brooklyn | 47 | 3 | KINGS
New York | 61 | 1 | NEW YORK
Queens | 81 | 4 | QUEENS
Richmond/Staten Island | 85 | 5 | RICHMOND

# Census Geographic Identifiers 

- A summary of this [Census documentation](https://www.census.gov/programs-surveys/geography/guidance/geo-identifiers.html). 
- Block group code is the first digit of Census block code. 
- Block codes may contain a one character suffix.
- Helpful Stata code: 
    - Convert number to string with leading zeros, 5 characters total: `gen x = string(x_num, "%05.0f")`
    - Convert number to string with lagging zeros, 5 characters total: `gen x = x_num + substr(str(x_num), 1, 5 - length(x_num))`

    
Area Type | Number of Digits | Example | Example GEOID 
--------- | ---------------- | ------- | -------------
State | 2 | Texas | 48 
County | 3 | Harris County, TX | 201
Tract | 6 | 2231 in Harris County, TX | 223100
Block Group | 1 | Block Group 1 in Tract 2231 in Harris County, TX | 1 
Block | 4 | Block 1050 in Tract 2231 in Harris County, TX | 1050 

# ZTRAX data 

Information from [ZTRAX FAQs](https://www.zillow.com/research/ztrax/ztrax-faqs/).

Dataset | ZTrans | ZAsmt 
------- | ------ | -----
Unique IDs | TransId | RowID (parcel), BuildingOrImprovementNumber (building in parcel)
Data source | From legal proceedings | From county assessor

In ZTrans data, to get arms-length transactions which best represents market price, filter through: 
- DataClassStndCode ('H' deed with concurrent mortgage)
- DocumentTypeStndCode ('INTR' intra-family)
- IntraFamilyTransferFlag
- LoanTypeStndCode ('RE' refinance)
- PropertyUseStndCode

For dates, use DocumentDate (o/w SignatureDate, RecordingDate)

