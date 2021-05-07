
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

