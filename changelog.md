## Changelog NATO Education and Training Network (NETN) Multi-Resolution Modelling (MRM) Module

### v1.1 - Initial version of NETN-MRM FOM Module released as part of NETN-FOM v2.0.

NETN-MRM FOM Module v1.1.1 was developed by MSG-106 and released 2014-05-15.


### v2.0 - Updated version by MSG-163 to be part of NETN-FOM v3.0.

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
* Changed datatype of `NETN_Aggregate.Symbol` to `SymbolIdentifier`
* Changed datatype of `NETN_Aggregate.Footprint` to `GeocentricPolygon `
* Added attribute `Route` for  object class `NETN_Aggregate`
* Added attribute `Destination` for  object class `NETN_Aggregate`
* Updated semantics of attribute `NETN_Aggregate.UniqueId`
* Renamed attribute `NETN_Aggregate.UniqueID` to `NETN_Aggregate.UniqueId`
* Renamed attribute `Symbol` of `NETN_Aggregate` to `SymbolId`
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


### v3.0 - Updated version by MSG-191 to be part of NATO-FOM v4.0

* Changed datatype `TransactionId` to `UUID`
* Replaced all use of Array datatype `NETN_ArrayOfSupplyStruct` with `SupplyStructArray`
* Replaced all use of `PercentUnsignedInteger32` with `PercentFloat32`
* Replaced all use of `PercentFloat64` with `PercentFloat32`
* Moved attribute `SourceUnit`from NETN-Physical Platform and Lifeform object classes to the same classes in NETN-MRM and renamed to `SourceAggregate`
* Changed datatype of `NETN_Aggregate` attribute `Route` to `ArrayOfWorldLocationStruct`
* Changed datatype of `NETN_Aggregate` attribute `Destination` to `WorldLocationStruct`
* Renamed `NETN_Aggregate` attribute `ParentUnit` to `ParentAggregate`
* Renamed `NETN_Aggregate` attribute `SourceUnit` to `SourceAggregate`
* Renamed `NETN_Aggregate` attribute `DividedUnitList` to `DividedEntities`
* Renamed `NETN_Aggregate` attribute `EmbeddedUnitList` to `MountedEntities`
* Renamed `NETN_Aggregate` attribute `SubunitList` to `DisaggregatedEntities`
* Changed datatype of `NETN_Aggregate` attribute `Mounted` to `MountStruct`
* Renamed `NETN_Aggregate` attribute `Mounted` to `MountedOn`
* Moved all attributes from `NETN_Aggregate` to extend RPR-Aggregate object class `AggregateEntity`
* Added attribute `Unit` to RPR-Physical `Aggregate` object class
* Added attribute `ParentAggregate` to RPR-Physical `Platform` object class
* Added attribute `EquipmentItem` to RPR-Physical `Platform` object class
* Renamed parameter `RemoveSubunits` of interaction class `Aggregate` to `RemoveDisaggregatedEntities`
* Renamed parameter `Subunits` of interaction class `Merge` to `DividedEntities`
* Renamed parameter `RemoveUnit` of interaction class `Deactivate` to `RemoveEntity`
* Renamed parameter `AggregateUnit` of interaction class `Request` to `AggregateEntity`
* Moved interaction class `QueryCapabilitySupported` from subclass of `Request` to subclass of `MRM_Interaction`
* Moved attribute `Federate` from interaction class `Request` to interaction class `QueryCapabilitySupported` and rename to `FederateApplication`
* Removed interaction class `Activate`
* Removed interaction class `Deactivate`



