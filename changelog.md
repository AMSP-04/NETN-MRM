## Changelog NETN-MRM

### Changes for v2.0

NETN-MRM FOM Module v2.0 was developed by MSG-163 and included in NETN-FOM v3.0.

### NETN-MRM#1 Rename FOM Module filename
* Changed Filename to NETN-MRM.xml

### NETN-MRM#3 Update modelIdentification
* Changed `modelIdentification` `securityClassification` from `unclassified` to `Not Classified`
* Changed `modelIdentification` `other` to include license information
* Changed `modelIdentification` `reference` to only refer to directly dependent FOM Modules
* Added `modelIdentification` `useLimitation` to reflect Scope of FOM Module
* Added `modelIdentification` `glyph` 
* Updated `modelIdentification` `purpose` to reflect Purpose of FOM Module 
* Updated `modelIdentification` `description` to reflect Introduction of FOM Module
* Updated `modelIdentification` `name` to NATO Education and Training Network (NETN) Multi-Resolution Modelling (MRM) Module
* Updated `modelIdentification` `poc` to include Release authority, Primary authors and Contributors
* Updated `modelIdentification` `applicationDomain` to empty.

#### NETN-MRM#4 Make MRM not depend on TMR
* Removed NETN-MRM dependency on NETN-TMR.

#### NETN-MRM#5 Rename MRM_Object interaction class
* Removed InteractionClass `MRM_Object`
* Removed InteractionClass `MRM_TriggerResponse`
* Removed InteractionClass `MRM_Trigger`
* Renamed InteractionClass `MRM_DisaggregationRequest`
* Renamed InteractionClass `MRM_AggregationRequest`
* Removed InteractionClass `MRM_DisaggregationResponse` 
* Removed InteractionClass `MRM_AggregationResponse`
* Removed InteractionClass `MRM_CancelRequest`
* Removed InteractionClass `MRM_ActionComplete` 
* Added InteractionClass `MRM_Interaction`
* Added InteractionClass `Request`
* Added InteractionClass `Aggregate`
* Added InteractionClass `Disaggregate`
* Added InteractionClass `Divide`
* Added InteractionClass `Merge`
* Added InteractionClass `Inactivate`
* Added InteractionClass `Activate`
* Added InteractionClass `QuerySupportedCapabilities`
* Added InteractionClass `CapabilitiesSupported`
* Added InteractionClass `Response`
* Removed Datatype `AggregateStateEnum32`
* Renamed Datatype `NonComplianceReasonEnum` to `MRM_ResponseStatusEnumType`

#### NETN-MRM#6 Complete MRM Documentation
* Complete rewrite of the NETN-MRM Documentation. The pattern has been simplified and extended with additional capabilities for controlling the Aggregate Unit state during federation execution.

#### NETN-Aggregate#1 Update modelIdentification
* Changed `modelIdentification` `securityClassification` from `unclassified` to `Not Classified`
* Changed `modelIdentification` `other` to include license information
* Changed `modelIdentification` `reference` to only refer to directly dependent FOM Modules
* Added `modelIdentification` `useLimitation` to reflect Scope of FOM Module
* Added `modelIdentification` `glyph` 
* Updated `modelIdentification` `purpose` to reflect Purpose of FOM Module 
* Updated `modelIdentification` `description` to reflect Introduction of FOM Module

#### NETN-Aggregate#2 Harmonize data types
* Replaced datatype `PercentageUint32` with `PercentUnsignedInteger32`
* Removed datatype `PercentageUint32`
* Moved datatype `QuantityFloat64` to NETN-BASE
* Changed datatype of `NETN_Aggregate` object class attribute `Mounted` to `PercentFloat64`
* Replaced datatype `FacingDegreesFloat32` with `DirectionDegreesFloat32`
* Removed datatype `FacingDegreesFloat32`
* Replaced datatype `TimeSecInt64` with `EpochTimeSecInt64`
* Changed datatype of attribute `SourceUnit` from `HLAunicodeString` to `UuidArrayOfHLAbyte16`
* Changed datatype of attribute `Symbol` from `HLAunicodeString` to `SymbolIdentifierArray15`

#### NETN-Aggregate#3 Add Route and Destination Attributes
* Added attribute `Route` for  object class `NETN_Aggregate`
* Added attribute `Destination` for  object class `NETN_Aggregate`

#### NETN-Aggregate#4 Update Attribute Footprint datatype
* Changed datatype of `NETN_Aggregate` object class attribute `Footprint` from `ArrayOfWorldLocationStruct3` to `GeocentricPolygon `

#### NETN-Aggregate#5 Update and harmonize naming of Aggregate attributes to better match NETN-ORG
* Clarified semantics of attribute `UniqueID` of `NETN_Aggregate`
* Changed attribute `UniqueID` of `NETN_Aggregate` object class to `UniqueId`
* Removed attribute `Footprint`of `NETN_Aggregate` object class
* Removed attribute `SupportUnit` of `NETN_Aggregate` object class
* Changed datatype of attribute `Echelon` of `NETN_Aggregate` to `EchelonEnum32`
* Removed datatype `EchelonEnum8`
* Changed datatype of attribute `EntityList` of `NETN_Aggregate` to `ArrayOfEntityStruct`
* Removed datatype `EntityListVariableLengthStruct`
* Removed datatype `UpdateTypeEnum32`
* Removed datatype `SupportRelationshipStruct`
* Renamed attribute `UnitEquipment`  of `NETN_Aggregate` to `EquipmentStatus`
* Renamed attribute `UnitPersonnel`  of `NETN_Aggregate` to `PersonnelStatus`
* Renamed attribute `UnitSupplies`  of `NETN_Aggregate` to `Supplies`
* Removed datatype `SupportRelationshipEnum8`
* Added field `ResourceType` to datatype `ResourceStatusNumberStruct`
* Changed datatype of attribute `CoverStatus` of `NETN_Aggregate` to `PercentFloat64`
* Changed datatype of attribute `CombatValue` of `NETN_Aggregate` to `PercentFloat64`
* Removed datatype `CoverStatusStruct`
* Removed datatype `CoverEnum8`
* Removed datatype `CombatValueFloat64`
* Added field `UnitAllocation` to datatype `EntityStruct`




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