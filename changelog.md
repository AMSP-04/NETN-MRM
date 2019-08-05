## Changelog NETN-MRM

### Changes for v2.0

NETN-MRM FOM Module v2.0 was developed by MSG-163 and included in NETN-FOM v3.0.

### NETN-MRM#1 Rename FOM Module filename
* Filename changed to NETN-MRM.xml without version number.

### NETN-MRM#3 Update modelIdentification
* Change `modelIdentification` `securityClassification` from `unclassified` to `Not Classified`
* Change `modelIdentification` `other` to include license information
* Change `modelIdentification` `reference` to only refer to directly dependent FOM Modules
* Add `modelIdentification` `useLimitation` to reflect Scope of FOM Module
* Add `modelIdentification` `glyph` 
* Update `modelIdentification` `purpose` to reflect Purpose of FOM Module 
* Update `modelIdentification` `description` to reflect Introduction of FOM Module
* Update `modelIdentification` `name` to NATO Education and Training Network (NETN) Multi-Resolution Modelling (MRM) Module
* Update `modelIdentification` `poc` to include Release authority, Primary authors and Contributors
* Update `modelIdentification` `applicationDomain` to empty.



### Changes for v1.1.1 
NETN-MRM FOM Module v1.1.1 was developed by MSG-106 and released 2014-05-15.

* v1.1.0d1 - NETN2 First version
* v1.1.0d2 - Updated Identification, modified parameters at MRM_CancelRequest and MRM_ActionComplete
* v1.1.0d3 - Added CompletionResult parameter to MRM_ActionComplete, added semantics for parameter reason.
* v1.1.0d4 - Added NonComplianceReason and associated enumerated datatype to both MRM_DisaggregationResponse and MRM_AggregationResponse.
* v1.1.0d5 - Added UuidList to MRM_DisaggregationRequest. Changed MRM_DisaggregationRequest.Instance name to AggregateUuid.v1.1.0d6 - Add Aggregate to AggregateStateEnum, Changed semantics for AggregateFederate and HigherResolutionFederate in MRM_Object.
* v1.1.0d7 - Changed AggregateStateEnum to AggregateStateEnum32 and revised enumerated values.
* v1.1.0d8 - Added MRM_TriggerResponse interaction. Updated References tab.
* v1.1.0 - Removed draft number (d8) from name.
* v1.1.1 - Semantics update