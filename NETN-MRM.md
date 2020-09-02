# NETN-MRM

The NATO Education and Training Network (NETN) Multi-Resolution Modelling FOM Module.

Copyright (C) 2020 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md).

## Introduction

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for simulation. Depending on the purpose and requirements of the simulation, the models can have different levels of resolution and aggregation can be used to create representations of broader combined concepts. 

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module is a specification of how to perform aggregation and disaggregation of aggregated representation of entities, e.g. units, into other levels of aggregation or individual entities, e.g. platforms, in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module specifies how data is represented and exchanged in the federation. The NETN-MRM FOM module is available as an XML file for use in HLA based federations.


### Purpose

The purpose of NETN-MRM is to support federations with entities represented at multiple levels of resolution and where the level of resolution can change dynamically during a simulation. It supports patterns for aggregation and disaggregation of units and division and merging of unit resources. The module provides a common standard interface for the aggregated representation of equipment, personnel and supplies in a federated distributed simulation. The aggregated representation can be used to model the state of organisations, such as military units, without the need to represent each resource as individual simulated entities. The module extends the existing RPR-FOM v2.0 `AggregateEntity` object class with attributes to allow additional information to be associated with simulated aggregate entities.

For example:
* Disaggregation of a Battalion represented as a `NETN_Aggregate` object into Company level `NETN_Aggregate` objects.
* Disaggregation of a Company level unit, represented as a `NETN_Aggregate` object, to individual platforms, e.g. `NETN_GroundVehicle` objects.
* Aggregation of platforms represented as, e.g. `NETN_GroundVehicle` objects, to an attribute of a unit, e.g. a Platoon, represented as a `NETN_Aggregate` object.
* Dividing individual pieces of equipment, e.g. UAV, from a Company unit to simulate some reconnaissance operation in more detail.
* Merging of a Recce platoon, represented as a `NETN_Aggregate` object, on its return from a mission with its source Company unit. 

### Scope

NETN-MRM covers the following cases:

* Aggregate level modelling and simulation of units
* Aggregation of simulated subunits or physical entities.
* Disaggregation of a unit into subunits or physical entities.
* Division of simulated unit with re-allocation of resources between source unit and divided unit.
* Division of simulated unit where equipment resources are removed from source unit and instead represented as physical entities.
* Merging of previously divided entities with source unit.
* Activation or Inactivation of entities represented in the simulation.


### Dependencies

The NETN-MRM refers to simulated entities by UUID. Implementation of Aggregation, Disaggregation and Divide also requires a knowledge of the structure and organisation of units and allocation of equipment as defined in NETN-ORG.

In the MRM patterns, the acquisition of modelling responsibility of simulated entities may use NETN-TMR.

## Overview

NETN-MRM requires the use of NETN-Aggregate and NETN-Physical Object Classes for the aggregate representation units and physical entities in the simulation.

The model resolution of a unit can vary in a simulation.

* Not explicitly registered in the federation as a simulated object but represented in scenario data such as NETN-ORG `Unit` objects or similar.

* Registered as an actively simulated `NETN_Aggregate` object. The state of the unit is explicitly simulated by federates in the distributed federated simulation.

* Registered as an inactive `NETN_Aggregate` object. The simulation does not explicitly model the state. The simulation can derive the status from the simulation of its subunits and physical entities. 

* Registered as divided with a reduced source `NETN_Aggregate` object and subsets of the divided resources modelled as `NETN_Aggregate` or NETN-Physical objects. The resources of the source unit are split between the simulated entities when divided.

<img src="./images/NETN-Aggregate Object Class Tree.png" />

Figure: NETN_Aggregate object class as an extension of RPR-FOM


A `NETN_Aggregate` object is a representation of an organisational unit including all equipment, personnel and supplies allocated to subunits. It is possible to disaggregate a `NETN_Aggregate` object into subunits each represented as a `NETN_Aggregate` object. It is possible to divide an aggregate representation of a unit and represent divided parts as a `NETN_Aggregate` objects or physical entities.

Disaggregation of a unit always constitutes a full disaggregation of all subunits into active `NETN_Aggregate` objects. The aggregated unit itself can remain registered in the federation as an inactive `NETN_Aggregate` object, and its state can be updated based on the simulation of its disaggregated subunits.

Aggregation of a unit always constitutes a full aggregation of all subunits into an active `NETN_Aggregate` object. The disaggregated units can remain registered in the federation as an inactive `NETN_Aggregate` object, and its state can be updated based on the simulation of its aggregated parent.

Division of a unit is a temporary allocation of specific resources from a source (original unit) represented as an active `NETN_Aggregate` object. It reallocates modelling responsibility of specific resources from the source to an additional active `NETN_Aggregate` object or individual physical entities representing specific equipment. 

Merging of a previously divided unit incorporate `NETN_Aggregate` object(s) and physical entities back to its source `NETN_Aggregate` object.

MRM events can be requested and triggered using HLA interactions defined in the NETN-MRM FOM Module. The result of an MRM event is reported to the requesting/triggering federate using a `Response` interaction.

<img src="./images/NETN-MRM Interaction Class Tree.png"/>

Figure: MRM interactions and events

The capability of a federate to support MRM events for a specific aggregate unit can be queried using the `QuerySupportedCapabilities` interaction. The response is provided using `CapabilitiesSupported` interaction and includes a list of names of available MRM events.

## NETN_Aggregate

The `NETN_Aggregate` object class is a specialization/subclass of the RPR-FOM object class `BaseEntity.AggregateEntity` and defines additional attributes.


<img src="./images/NETN_Aggregate.png"/>

Figure: The NETN_Aggregate object class

|Attribute|Description|
|---|---|
|UniqueId|**Required.** A unique identifier for the object. The Universally Unique Identifier (UUID) is either generated or defined as part of scenario initialisation, e.g. using NETN-ORG MSDL data. The unique identifier can serve dual purposes. It is a unique identification of the NETN_Aggregate object instance but can also be a reference to a NETN-ORG unit element with the same unique identifier.|
|Status|**Required.** Indicate if this aggregate unit currently is being simulated or not. E.g. units mounted or embarked on transports can be set to inactive. During an inactive state, the attribute values may not reflect an accurate, current value. Therefore, any subscribing federate can ignore inactive units. All attributes must be updated to represent the current status of the instance before setting the status to active.|
|SubunitList|**Optional.** Reference to disaggregated representations of subsets of the aggregate unit when registered in the federation. Each element should refer to an existing NETN_Aggregate object in the federation. If not published, disaggregation is not supported.|
|ParentUnit|**Optional.** Reference to parent aggregate entity. If not published, aggregation is not supported. The default value is 0000000000000000 (no parent unit).|
|DividedUnitList|**Optional.** Reference to other aggregate or physical entities divided from the aggregate unit to represent specific subsets of holdings. If not published, a division is not supported.|
|SourceUnit|**Optional.** Reference to another active NETN_Aggregate instance from which this aggregate was divided. If not published, merging is not supported.|
|EmbeddedUnitList|**Optional.** Reference to platforms or lifeforms embarked on this platform. If not published, transport of embedded units not supported.|
|HigherHeadquarters|**Optional.** A reference to an entity representing the aggregate unit's superior or headquarters from which orders are given and to which reports are sent. The highest level unit or headquarters will publish 0000000000000000 as its HigherHeadquarters value. The referenced entity may or may not be registered in the federation as a NETN_Aggregate and/or NETN-ORG unit. If not published, the aggregate does not have a superior unit or headquarter. The default value is 0000000000000000 (no higher headquarters).|
|Mounted|**Optional.** The percentage of aggregate personnel travelling on or in their organic transport. Default 100% - all personnel mounted.|
|SymbolId|**Optional.** A symbol identifier represented as a string. |
|Callsign|**Required.**  A callsign used to address the unit. Callsigns should be unique in the context in which they are used but not required to be globally unique.|
|Echelon|**Optional.** The size of the unit (level of command).|
|EntityList|**Optional.** This attribute provides data on all entities comprising the aggregate. Entities include equipment, e.g. platforms, weapons, sensors and lifeforms such as personnel. Each entity contains key status attributes and subunit allocation information. If not provided the status and allocation of entities is not modelled on an entity level.|
|SuppliesStatus|**Optional.** The type and quantities of supplies available (on hand) to the unit. If not provided, the amount of available supplies is undefined.|
|EquipmentStatus|**Optional.** This summarises the health status of the equipment comprising the aggregate. If not provided, the status of equipment is undefined.|
|PersonnelStatus|**Optional.** This summarises the health status of personnel comprising the aggregate. If not provided, the status of personnel is undefined.|
|VisualSignature|**Optional.** Describes the unit's susceptibility to electro-optical detection.|
|HUMINTSignature|**Optional.** Describes the unit's susceptibility to human intelligence (HUMINT), i.e. information collected and provided by human sources.|
|ElectronicSignature|**Optional.** Describes the aggregate's susceptibility to electronic detection both as a summary value and by identifying aggregate sensors together with their operational status.|
|CombatValue|**Optional.** A summary value (in per cent) of unit effectiveness based on the level of training, leadership, morale, personnel and equipment operational status, etc. The default value is 100%.|
|CoverStatus|**Optional.** Describes the unit's protection from the effects of weapons fire. Default is 0% - Fully affected by weapon fire.|
|CaptureStatus|**Optional.** The status of a person or unit with respect to their control or influence over their own activities. Default: 1 - Not Captured.|
|Mission|**Optional.** The operational task the aggregate has been ordered to perform.|
|Activity|**Optional.** The current activity of the aggregate. This may differ from the mission due to casualties, readiness, etc.|
|Route|**Optional.** The current path of movement. |
|Destination|**Optional.** The current destination of movement. |
|WeaponsControlOrder|**Optional.** Describes current Weapon Control Order Free, Tight, or Hold. Default is 0 - Other.|

### Specific use of inherited attributes

| Attribute| Note|
|---|---|
|AggregateState|**Required.** Only the following values are used: `Aggregated`, `Disaggregated`, or `Fully Disaggregated`. When `Fully Disaggregated` the `Status` should be set to `Inactive`.|
|Dimensions|**Required.**|
|IsPartOf|**Optional.** Used when mounted or transported by other Aggregate entity|
|SubAggregatesIdentifiers|**Optional.** Reference to `NETN_Aggregate` object instances registered in the federation representing subunits.|


## MRM actions

All MRM actions use the same pattern of interaction. 
1. A triggering federate requests a federate to act on a specified aggregate entity using subclasses of the interaction `Request`.
2. The receiving federate performs the actions if possible.
3. The federate reports the success of the action using the interaction `Response`.

Before sending a more specific MRM request to a federate, the MRM capabilities supported by the federate can be queried. To query which MRM actions are supported, send a `QueryCapabilitiesSupported` request interaction. All federates implementing NETN-MRM must implement support for `QueryCapabilitiesSupported` and provide a `CapabilitiesSupported` response interaction. If a federates is not responding to `QueryCapabilitiesSupported` it should be assumed not to support any MRM action. A federate not supporting a specific MRM action should never be requested such action.

<img src="./images/query.svg"/>

<!--
participant Trigger
participant Federate
autonumber 
Trigger->Federate:QueryCapabilitiesSupported(Event#1, Federate)

Federate->Trigger:CapabilitiesSupported (Event#1, CapabilityNames)

space

Trigger->Federate:Request(Event#2, Federate, AggregateUnit)

activate Federate

box left of Federate:Perform MRM Action
Trigger<-Federate:Response(Event#2, Status)


deactivate Federate

autonumber off
-->

Figure: Query and Request of MRM actions

**Request Interaction Class**
|Parameter|Description|
|---|---|
|Federate|**Required.** Intended federate responsible for performing the requested action. Sending federate should ensure that receiving federate can perform requested action. If not able to perform, a response interaction indicating failure should be returned. |
|AggregateUnit|**Required for all requests except QuerySupportedCapabilities.** Unique identifier for the AggregateUnit for which this request is related to. |

**Response Interaction Class**
|Parameter|Description|
|---|---|
|Status|**Required.** Specifies the result of the request action. TRUE indicates success.|

**CapabilitiesSupported Interaction Class**
|Parameter|Description|
|---|---|
|CapabilityNames|**Required.** A list of names of the supported capabilities for the Aggregate entity specified in the query. The names are one or more of "Aggregate", "Disaggregate", "Divide", "Merge", "Activate" and "Deactivate".|



### Disaggregate

*Conditions*

* Disaggregation is only allowed for an aggregate unit registered in the federation as a non-divided and active `NETN_Aggregate` object. 
* The federate performing disaggregation must be able to acquire modelling responsibility of the `Status` attribute of the `NETN_Aggregate` object representing the aggregated unit. 
* The federate performing disaggregation must be able to acquire modelling responsibility of the `Status` attribute of `NETN_Aggregate` objects representing the aggregated unit's subunits.  

The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false.

<img src="./images/disaggregate.svg"/>



<!--
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Disaggregate(Event, Federate, \nAggregateUnit)

Federate->Federation:Update Attribute Value (AggregateUnit, Status = Inactive)


loop Activate Subunits

alt Create new NETN_Aggregate
Federate->Federation:Register Object Instance (NETN_Aggregate)
else Reuse existing NETN_Aggregate
aboxleft over Federate, Federation:Acquire Modelling Responsibility NETN_Aggregate
end

Federation<-Federate:Update Attribute Values (NETN_Aggregate, Attributes)
Federation<-Federate:Update Attribute Values (NETN_Aggregate, ParentUnit=AggregateUnit)

Federation<-Federate:Update Attribute Value (NETN_Aggregate, Status = Active)
end
Federation<-Federate:Update Attribute Values (AggregateUnit, SubunitList)
Trigger<-Federate:Response(Event, Status)
autonumber off

-->
Figure: Disaggregation of a unit

1. A trigger federate sends a `Disaggregate` request to a specified `Federate` with reference to the `AggregateUnit` object representing the unit to disaggregate.
2. The receiving federate performs actions to acquire modelling responsibility of the `Status` attribute for the `NETN_Aggregate` object referenced by the `AggregateUnit` parameter. If successful, the federate updates the `Status` attribute of the object instance to `Inactive`.
3. The federate registers subunits as `NETN_Aggregate` objects if not already registered in the federation. If registered, the modelling responsibility of the `Status` attribute is acquired. 
4. The federate update initial attribute values for the new `NETN_Aggregate` objects.
5. The federate updates the `ParentUnit` attribute of new `NETN_Aggregate` objects to refer to the `AggregateUnit`.
6. The federate update the `Status` attribute of the `NETN_Aggregate` objects to `Active`.
7. The federate update the `SubunitList` attribute of the `AggregateUnit` objects which include a reference to all aggregate entities representing the subunits.
8. On completion, the federate sends a `Response` interaction with the `Status` parameter set to indicate successful completion of the request.

### Aggregate

*Conditions*

* Aggregation is only allowed for an aggregate unit registered in the federation as an inactive `NETN_Aggregate` object. 
* The federate performing aggregation must be able to acquire modelling responsibility of the `Status` attribute of the `NETN_Aggregate` object representing the aggregated unit. 
* The federate performing aggregation must be able to either remove `NETN_Aggregate` objects representing subunits from the federation or acquire modelling responsibility of the `Status` attribute of these objects. 

The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false.

<img src="./images/aggregation.svg"/>

<!-- 
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Aggregate(Event, Federate, \nAggregateUnit, RemoveSubunits)

loop for all Subunits as NETN_Aggregate
alt RemoveSubunits = true
Federate->Federation:Delete Object Instance (NETN_Aggregate)

else RemoveSubunits = false
Federation<-Federate:Update Attribute Value (NETN_Aggregate, Status = Inactive)
Federation<-Federate:Update Attribute Value (NETN_Aggregate, ParentUnit = 0)
end
end

Federate->Federation:Update Attribute Value (AggregateUnit, attributes)

Federate->Federation:Update Attribute Value (AggregateUnit, Status = Active)

Trigger<-Federate:Response(Event, Status)
autonumber off
-->

Figure: Aggregation of a unit

1. A trigger federate sends an `Aggregate` request to a specified `Federate` with reference to the `AggregateUnit` object representing the unit to aggregate.
2. If indicated in the request, the federate deletes all subunits and physical entities registered in the federation.
3. The federates updates `Status` of all subunits, represented as `NETN_Aggregate` objects in the federation, to `Inactive` if indicated in the request that they should remain in the federation.
4. The federate updates all status attributes of the unit to reflect the aggregate state.
5. The federate updates the `Status` of the unit to `Active`.
6. On completion, the federate sends a `Response` interaction with the `Status` parameter set to indicate successful completion of the request.
 

### Divide
*Conditions*
* The federate performing division must be able to acquire modelling responsibility of the attributes of the `NETN_Aggregate` object representing the aggregated unit. 
* Division is only allowed for non-divided aggregate units registered in the federation as active `NETN_Aggregate` objects. 


The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false.

<img src="./images/divide.svg"/>

Figure: Divide an aggregate unit

<!--
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Divide(Event, Federate, \nSourceAggregateUnit, \nHoldings, \nRepresentPhysicalAsObject)


alt RepresentPhysicalAsObject
abox over Federate, Federation:Divide and represent as NETN-Physical.*

else Represent as a single Aggregate

abox over Federate, Federation:Divide and represent as  NETN_Aggregate
end

Trigger<-Federate:Response(Event, Status)
autonumber off
-->

The division of aggregate unit:

1. A federate sends a `Divide` request to a specified `Federate` indicating the `SourceAggregateUnit` object to divide and a list of the divided holdings. In the request, a flag indicates if each physical entities in the holdings should be registered as individual objects or not.

Divide by either acquiring or registering an additional `NETN_Aggregate` or instances of `NETN-Physical` entities for platforms and lifeforms. 

2. On completion, the federate sends a `Response` interaction with the `Status` parameter set to indicate successful completion of the request.

#### Dividing to Physical Entities
If indicated, the federate registers specified platforms and lifeforms entities as individual objects in the federation. These objects are instances of `NETN-Physical` leaf object classes and can either be already existing or created based on the divided holdings information. 

*Conditions*

* Reuse of existing instances for representing divided holdings is only allowed if not already in use, i.e. the `SourceUnit` attribute is not published or set to all zeros.
* Reuse of existing instances for representing divided holdings requires the Federate performing division must be able to acquire modelling responsibilities for attributes `SourceUnit` and `Status` of these instances.

The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false.

<img src="./images/dividephysical.svg"/>

Figure: Divide an aggregate unit into physical entities.

<!--
participant Federate
participant Federation
autonumber 

loop For Each Platform and Lifeform in Holdings
alt Create new entity
Federate->Federation:Register Object Instance (NETN-Physical.*)
else Reuse entity
aboxleft over Federate, Federation:Acquire Modelling Responsibility NETN-Physical.*
end

Federation<-Federate:Update Attribute Values (NETN-Physical.*, Attributes)
Federation<-Federate:Update Attribute Values (NETN-Physical.*, SourceUnit=SourceAggregateUnit)
Federation<-Federate:Update Attribute Values (SourceAggregateUnit, DividedUnitList)
Federation<-Federate:Update Attribute Values (SourceAggregateUnit, Attributes)
Federation<-Federate:Update Attribute Values (NETN-Physical.*, Status=Active)
end

autonumber off

-->
The division into physical entity objects:
1. If not reusing an existing object in the federation, the federate registers a physical entity in the federation as a corresponding `NETN-Physical` leaf class. Otherwise, acquire modelling responsibilities of the object to be reused as a representation for the physical entity.
2. The federate updates the physical entity object with appropriate initial attribute values.
3. The `SourceUnit` attribute of the registered physical entity is updated to reference the `SourceAggregateUnit`.
4. The attribute `DividedUnitList` of the `SourceAggregateUnit` is updated to include a reference to the physical entity object.
5. The `SourceAggregateUnit` attributes are updated to reflect the reduction of holdings.
6. The federate update the `Status` attribute of the physical entity object to `Active`.

#### Dividing to Aggregate Entity
If indicated, the federate registers each specified resource as holdings of a new Aggregate entity object in the federation.

<img src="./images/dividesub.svg"/>

Figure: Subdivide an aggregate unit.

<!--
participant Federate
participant Federation
autonumber 

alt Create new entity
Federate->Federation:Register Object Instance (NETN_Aggregate)
else Reuse entity
aboxleft over Federate, Federation:Acquire Modelling Responsibility NETN_Aggregate
end

Federation<-Federate:Update Attribute Values (NETN_Aggregate, Attributes)

Federation<-Federate:Update Attribute Values (NETN_Aggregate, SourceUnit=SourceAggregateUnit)
Federation<-Federate:Update Attribute Values (SourceAggregateUnit, DividedUnitList)
Federation<-Federate:Update Attribute Values (SourceAggregateUnit, Attributes)
Federation<-Federate:Update Attribute Values (NETN_Aggregate, Status=Active)

-->
Division of aggregate entities:

1. If not reusing an existing object in the federation, then a single `NETN_Aggregate` object is registered in the federation. Otherwise, acquire modelling responsibilities of the `NETN_Aggregate` object to be reused as a representation for the divided holdings.
2. The federate updates the `NETN_Aggregate` object with appropriate initial attribute values.
3. The `SourceUnit` attribute of the `NETN_Aggregate` object is updated to reference the `SourceAggregateUnit`.
4. The attribute `DividedUnitList` of the `SourceAggregateUnit` is updated to include a reference to the `NETN_Aggregate` object.
5. The `SourceAggregateUnit` attributes are updated to reflect the reduction of holdings.
6. The federate update the`Status` attribute of the `NETN_Aggregate` object to `Active`.

 
### Merge
*Conditions*
* The federate performing merge must be able to acquire modelling responsibility of the attributes of the `NETN_Aggregate` object representing the source aggregated unit. 
* The federate performing merge must be able to acquire modelling responsibility of the attributes of the divided object instances.
* Merge is only allowed for divided aggregate units registered in the federation.
* Merge is only allowed for an active `NETN_Aggregate` objects. 


The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false.

<img src="./images/merge.svg"/>

Figure: Merge divided parts with their aggregate unit

<!--
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Merge(Event, Federate, \nSourceAggregateUnit, \nDividedUnits, RemoveDivided)
loop for all DividedUnits
alt RemoveDivided = true
Federate->Federation:Delete Object Instance (DividedUnit)
else RemoveDivided = false
Federation<-Federate:Update Attribute Value (DividedUnit, Status = Inactive)
Federation<-Federate:Update Attribute Value (DividedUnit, SourceUnit = 0)
end
Federate->Federation: Update Attribute Values(SourceAggregateUnit, DividedUnitList)
Federate->Federation: Update Attribute Values(SourceAggregateUnit)
end
Trigger<-Federate:Response(Event, Status)
autonumber off
-->

Trigger the merge:

1. A trigger federate sends a `Merge` request to a specified `Federate` with reference to the `SourceAggregateUnit` object and a list of the divided units to merge. In the request, a flag indicates if the divided units should remain in the federation as inactive entities or if they should be removed. 

For each divided unit:

2. Delete from the federation if indicated.
3. If the divided unit should remain, update the `Status` attribute to `Inactive`. 
4. If the divided unit should remain, update the `SourceUnit` attribute to 0 (no source unit).
5. Update the `SourceAggregateUnit` attribute `DividedUnitList` to exclude the divided units now merged. 
6. Update the `SourceAggregateUnit` attributes to reflect the merged status.

On completion:

7.  The federate sends a `Response` interaction with the `Status` parameter set to indicate successful completion of the request.


### Activate
*Conditions*
* An `AggregateUnit` can not be activated if any subunit is active.

The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if any of these conditions are false. Otherwise, the `AggregateUnit` attribute is updated to `Active`.

### Deactivate
*Conditions*

* An `AggregateUnit` can not be deactivated if a divided unit exists.

The federate send a `Response` interaction with the `Status` parameter set to `FALSE` if this condition is false.