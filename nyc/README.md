
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
    - Convert number to string with lagging zeros, 5 characters total: `gen x = string(x_num) + substr("00000", 1, 5 - length(string(x_num)))`

    
Area Type | Number of Digits | Example | Example GEOID 
--------- | ---------------- | ------- | -------------
State | 2 | Texas | 48 
County | 3 | Harris County, TX | 201
Tract | 6 | 2231 in Harris County, TX | 223100
Block Group | 1 | Block Group 1 in Tract 2231 in Harris County, TX | 1 
Block | 4 | Block 1050 in Tract 2231 in Harris County, TX | 1050 

# Stata Packages 

[ols_spatial_HAC](http://www.fight-entropy.com/2010/06/standard-error-adjustment-ols-for.html) ([in R](http://www.trfetzer.com/using-r-to-estimate-spatial-hac-errors-per-conley/))

```
Syntax:

 ols_spatial_HAC Yvar Xvarlist, lat(latvar) lon(lonvar) Timevar(tvar) Panelvar(pvar) 
    [DISTcutoff(#) LAGcutoff(#) bartlett DISPlay star dropvar]

 
 Notes:
 
 A variable equal to 1 is required to estimate a constant term.
 
 Location arguments should specify lat-lon units in DEGREES, however
 distcutoff should be specified in KILOMETERS. 

 Distances are computed by approximating the planet's surface as a plane
 around each observation.  This allows for large changes in LAT to be
 present in the dataset (it corrects for changes in the length of
 LON-degrees associated with changes in LAT). However, it does not account
 for the local curvature of the surface around a point, so distances will
 be slightly lower than true geodesics. This should not be a concern so
 long as locCutoff is < O(~2000km), probably.

 ```
 
 [spatial_hac_iv](http://fmwww.bc.edu/repec/bocode/s/spatial_hac_iv.sthlp)
 
 ```
 spatial_hac_iv depvar indepvar (endog=inst) lon({it:varname}) lat({it:varname}) timevar({it:varname}) 
    panelvar({it:varname}) distcutoff({it:#}) lagcutoff({it:#}) [bartlett dropvar]
```

[reg2hdfespatial](http://www.trfetzer.com/conley-spatial-hac-errors-with-fixed-effects/)

# Segregation Statistics 

Sources from [here](https://www.census.gov/topics/housing/housing-patterns/guidance/appendix-b.html) and Rothwell, Massey 2009. 

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

