# General Ledger Primer
The general ledger API is based on the AICPA Audit Standard with additional content added to support an API interface.

## Intent of the Standard

The intent of the standard is to facilitate the transfer of financial data between users across different systems. The primary purpose is to enable the extraction of detailed financial data for the performance of financial audits.  This is not the sole intent of the standard, and it the specification is written in a manner to support the general transfer of financial data between systems for any range of purposes.

## General Ledger API Rest Points

The General ledger supports the following API end points:

* Journal entries
* Trial balance
* Accounts
* XBRL Trial Balance

### Journal entries

The journal entries end point allows a user to request journal entries for a specific entity for a specific time range. If no request parameter is defined, then the system will attempt to return all available journal entries that the specific user making the request is permitted to see. 

To return data for a specific time range the user provides the start date and the end date.  This date refers to the effective date of the journal entry.

If the system contains journal entry data for multiple entities then the system will determine which entity information to return based on the user making the request.  This is system dependent.


### Trial Balance

The trial balance end point allows a user to request journal entries for a specific period end.  The end date must be provided.  The default trial balance will return values for the latest reporting year. If a start date is provided, then the period from the specified start date to the specified end date will be provided if available.  All available periods can be queried using the period end-point. (see base modules below).  The start date must use the start date provided form the period rest point.

Only one reporting period is returned at a time. 

The opening balances returned are the opening balances after opening journal entries have been applied.  To define a tradional year to year comparison of a trial balance to API calls would be made for the period ends requested. The opening balances are equal to the end of period balance + opening period journals.

Additional parameters can be provided to get budget information by passing a budget flag parameter with a value of budget.  If no value is provided the default value is the actual value.

If the system has information on multiple entities the entity parameter allows information for a specific entity to be requested using a system assigned entity identifier. If the entity identifier is not provided the default entity is returned. This is implementation specific.

### Accounts

The accounts entry point allows a user to request the chart of accounts for a given entity.  The user can pass the entity identifier as a parameter to retrieve the chart of accounts for that entity.  If no parameter is passed all the accounts of all entities available to the user are returned. Each account record includes the entity identifier.

The end points returns details of each account defined in the chart of the accounts for the entity.  This does not include balance or journal information. These details are available in the Trial Balance and Journal end points.

### Trial Balance XBRL

The trial balance in XBRL end point allows the user to retrieve an XBRL instance of the trail balance in an XBRL-JSON format. The API allows the user to pass the end date, the entity identifier and a specific G/L account number.

## Structure of Returned Data

The data returned is generally flat.  The structure of the returned data deliberately  avoids  nested structures. This is to allow maximum flexibility to support transfer from as many systems as possible.  This means some data is repeated in every set of data returned such as the entity name. The journal entry data is the one exception however where the lines to the journal entry are nested as a child of the journal entry. This was not flattened as the majority of systems will have a one to many relationship defined for a specific  journal and the line items of the journal.

Not all data has to be returned in the return fields as in many systems the data defined in the standard will not be available.  The API however does define for each end point some minimal data fields that do have to returned.  For example, a journal entry end point must return the following:

* Journal_ID
* Effective_Date
* GL_Account_Number
* Amount
* Amount_Currency
* Amount_Credit_Debit_Indicator


## Base Modules

The General Ledger module uses the base module to retrieve period and entity related information. The base module includes the following end points:
* Periods
* Entities

These end points provide meta data about the entities and periods available from the API.  These endpoints are separated out from the general ledger category as they would be used by other API modules.  The end points available in the base module are as follows:

### Periods

The periods end point allows the user to request the valid reporting periods of a given entity or all entities available to the requester.  No parameters are required for this end point.  However, the user can request reporting periods by start date, end date and entity.  The start data parameter returns all reporting periods with an end date equal or greater than the end date. The end data parameter returns all reporting periods with an end date equal or less than the end date.

### Entities

The entities end point allows the user to request all available entities available to the user on the system. The entities end point has one optional parameter for entity identifier. This parameter allows the user to get specific information about a specific entity. The end point returns the entity identifier the entity name and any subsidiary entities.


