## Changelog NETN-MRM

### Changes for v2.0

NETN-MRM FOM Module v2.0 was developed by MSG-163 and included in NETN-FOM v3.0.

* Changed Filename to NETN-MRM.xml
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

* Removed NETN-MRM dependency on NETN-TMR.
* Merged NETN-Aggregate to NETN-MRM.

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

* Removed datatype `AggregateStateEnum32`
* Removed datatype `EntityListVariableLengthStruct`
* Removed datatype `UpdateTypeEnum32`
* Removed datatype `SupportRelationshipStruct`
* Removed datatype `FacingDegreesFloat32`
* Removed datatype `CoverStatusStruct`
* Removed datatype `CoverEnum8`
* Removed datatype `CombatValueFloat64`
* Removed datatype `SupportRelationshipEnum8`
* Removed datatype `EchelonEnum8`
* Removed datatype `PercentageUint32`

* Renamed datatype `NonComplianceReasonEnum` to `MRM_ResponseStatusEnumType`

* Replaced datatype `PercentageUint32` with `PercentUnsignedInteger32`
* Replaced datatype `FacingDegreesFloat32` with `DirectionDegreesFloat32`
* Replaced datatype `TimeSecInt64` with `EpochTimeSecInt64`

* Moved datatype `QuantityFloat64` to NETN-BASE

* Changed datatype of `NETN_Aggregate.Mounted` to `PercentFloat64`
* Changed datatype of `NETN_Aggregate.SourceUnit` to `UuidArrayOfHLAbyte16`
* Changed datatype of `NETN_Aggregate.Symbol` to `SymbolIdentifierArray15`
* Changed datatype of `NETN_Aggregate.Footprint` to `GeocentricPolygon `

* Added attribute `Route` for  object class `NETN_Aggregate`
* Added attribute `Destination` for  object class `NETN_Aggregate`

* Updated semantics of attribute `NETN_Aggregate.UniqueId`

* Renamed attribute `NETN_Aggregate.UniqueID` to `NETN_Aggregate.UniqueId`

* Renamed attribute `UnitEquipment`  of `NETN_Aggregate` to `EquipmentStatus`
* Renamed attribute `UnitPersonnel`  of `NETN_Aggregate` to `PersonnelStatus`
* Renamed attribute `UnitSupplies`  of `NETN_Aggregate` to `Supplies`
* Removed attribute `Footprint`of `NETN_Aggregate` object class
* Removed attribute `SupportUnit` of `NETN_Aggregate` object class

* Added field `ResourceType` to datatype `ResourceStatusNumberStruct`

* Changed datatype of attribute `CoverStatus` of `NETN_Aggregate` to `PercentFloat64`
* Changed datatype of attribute `CombatValue` of `NETN_Aggregate` to `PercentFloat64`
* Changed datatype of attribute `EntityList` of `NETN_Aggregate` to `ArrayOfEntityStruct`
* Changed datatype of attribute `Echelon` of `NETN_Aggregate` to `EchelonEnum32`

* Added field `UnitAllocation` to datatype `EntityStruct`

### Changes for v1.1.1 
NETN-MRM FOM Module v1.1.1 was developed by MSG-106 and released 2014-05-15.
