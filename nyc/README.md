
# Introduction 

This document will provide tidbits of information and explain any code that is uploaded to NYC. 

# Counties 

Name | FIPS State Code  | Borough Num 
---- | ---------------- | -----------
Bronx | 5 | 2
Kings/Brooklyn | 47 | 3
New York | 61 | 1
Queens | 81 | 4
Richmond/Staten Island | 85 | 5

# ZTRAX data 

Expanding upon [ZTRAX FAQs](https://www.zillow.com/research/ztrax/ztrax-faqs/).

## Unique Observations per parcel 

- RowID identifies assessor parcels
- Buildings/properties are uniquely identified by RowID _and_ BuildingOrImprovementNumber (different building on same assessor parcel) 
- Sometimes assessors will put down multiple parts of a building, so collapsing by RowID and BuildingOrImprovementNumber where BuildingAreaStndCode = “BAL”

## ZTrans vs. ZAsmt 

- Use the ZTrans data for sales prices b/c ZAsmt is not as thoroughly updated 
- To get arms-length transactions which best represents market price, filter through DataClassStndCode ('H' deed with concurrent mortgage), DocumentTypeStndCode ('INTR' intra-family), IntraFamilyTransferFlag, LoanTypeStndCode ('RE' refinance), and PropertyUseStndCode


## Dates 

- Use DocumentDate (o/w SignatureDate, RecordingDate)

