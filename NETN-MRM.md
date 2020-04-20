# NETN-MRM

Copyright (C) 2020 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md).

## Introduction

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for simulation. Depending on the purpose and requirements of the simulation, the models can have different levels of resolution and aggregation can be used to create representations of larger combined concepts. 

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module is a specification of how to perform aggregation and disaggregation of aggregated representation of entities, e.g. units, into other levels of aggregation or individual entities, e.g. platforms, in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module is used to specify how data is represented and exchanged in the federation. The NETN-MRM FOM module is available as an XML file for use in HLA based federations.


### Purpose

The purpose of NETN-MRM is to support federations where models are represented at multiple levels of resolution and where the level of resolution can change dynamically during a simulation.

For example:
* Disaggregation of a Battalion represented as an `NETN_Aggregate` object into Company level `NETN_Aggregate` objects.
* Disaggregation of a Company level unit, represented as an `NETN_Aggregate` object, to individual platforms e.g. `NETN_GroundVehicle` objects.
* Aggregation of platforms represented as, e.g. `NETN_GroundVehicle` objects, to an attribute of a unit e.g. a Platoon, represented as an `NETN_Aggregate` object.
* Dividing an individual equipment, e.g. UAV, from a Company unit to simulate some reconnaissanse operation in more detail.
* Merging of a Recce platoon, represented as an `NETN_Aggregate` object, on its return from a mission with its source Company unit. 

### Scope

NETN-MRM covers the following cases:

* Aggregation of simulated subunits and/or physical entities.
* Disaggregation of unit into subunits and/or physical entities.
* Division of simulated unit where resources are allocated between source unit and divided unit.
* Division of simulated unit where equipment resources are removed from source unit and instead represented as physical entities.
* Merging of previously divided entities with source unit.
* Activation or Inactivation of entities represented in the simulation.

### Dependencies

The NETN-MRM refers to simulated entities by UUID as defined in NETN-Aggregate and NETN-Physical. Implementation of Aggregation, Disaggregation and Divide also requires a knowledge of the structure and organization of units and allocation of equipment. Information contained in NETN-Aggregate, NETN-Physical, and indirectly in NETN-ORG, can be used to produce aggregation and diaggregation results. 

In the MRM patterns, the acquisition of modelling responsibilitiy of simulated entities may use NETN-TMR.

## Overview

NETN-MRM requires the use of NETN-Aggregate and NETN-Physical Object Classes for the aggregate representation units and physical entities in the simulation.

An unit can be represented in the simulation in various ways.

* Not explicitly registered in the federation as a simulated object but represented in scenario data such as NETN-ORG Unit or similar.

* Registered as a actively simulated `NETN_Aggregate` object. The state of the unit is explicitly simulated by federates in the distributed federated simulation.

* Registered as an inactive `NETN_Aggregate` object. The state of the unit is not explicitly simulated but derived from simulation of its subunits and/or pysical entities. 

* Registered as divided with a source `NETN_Aggregate` object where one or more of its parts are also simulated as `NETN_Aggregate` or NETN-Physical objects. The resources of the source unit are split between the simulated entities when divided.

Disaggregation of a unit always constitutes a full disaggregation of all subunits into active `NETN_Aggregate` objects. The aggregated unit itself can remain registered in the federation as an inactive `NETN_Aggregate` object and its state can be updated based on the simulation of its disaggregated parts.

Aggregation of a unit always constitutes a full aggregation of all subunits into an active `NETN_Aggregate` object. The disaggregated units can remain registered in the federation as an inactive `NETN_Aggregate` object and its state can be updated based on the simulation of its aggregated parent.

Division of a unit is a temporary allocation of specific resources from a source (orginal unit) represented as an active `NETN_Aggregate` object. The specified resources are contained and represented in the federation as an additional active `NETN_Aggregate` object or as individual physical entities representing specific equipment. 

Merging of a previously divided unit will incorporate `NETN_Aggregate` object(s) and physical entities back to its source `NETN_Aggregate` object.

MRM events can be requested and triggered using HLA interactions defined in the NETN-MRM FOM Module. All events are directed to a federate responsible for managing the MRM event. The result of a MRM event is reported to the requesting/triggering federate using a `Response` interaction.

<img src="./images/NETN-MRM Interaction Class Tree.png"/>

Figure: MRM interactions and events

The capability of a federate to support MRM events for a specific aggregate unit can be queried using the `QuerySupportedCapabilities` interaction. The response is provided using `CapabilitiesSupported` interaction and includes a list of names of available MRM events.

## MRM actions

All MRM actions use the same pattern of interaction. 
1. A triggering federate requests a specified federate to perform an action on a specified aggregate entity using subclasses of interaction `Request`
2. If possible, the action is performed.
3. The success of the requested action is reported using the interaction  `Response`

Before sending a more specific MRM request to a federate, the MRM capabilities supported by the federate may be queried. To query which MRM actions are supported, a `QueryCapabilitiesSupported` request interaction may be sent. All federates implementing NETN-MRM must implement support for `QueryCapabilitiesSupported` and provide a `CapabilitiesSupported` response interaction. If a federates is not responding to `QueryCapabilitiesSupported` it should be assumed not to support any MRM action. A federate not supporting a specific MRM action should never be requested such action.

<img src="./images/query.svg" width="100%"/>

<!--
participant Trigger
participant Federate
autonumber 
Trigger->Federate:QueryCapabilitiesSupported(Event#1, Federate)

Federate->Trigger:CapabilitiesSupported (Event#1, CapabilityNames)

space

Trigger->Federate:Request(Event#2, Federate, AggregateEntity)

activate Federate

box over Federate:Perform MRM Action
Trigger<-Federate:Response(Event#2, Status)


deactivate Federate

autonumber off

-->

Figure: Query and Request of MRM actions

### Request
|Attribute|Description|
|---|---|
|Federate|**Required:** Intended federate responsible for peforming the requested action. Sending federate should ensure that receiveing federate has capability to perform requested action. If not able to perform, a response interaction indicating failure should be returned. |
|AggregateEntity|**Required for all requests except QuerySupportedCapabilities:** Unique identifer for the AggregatedEntity for which this request is related to. |

### Response
|Attribute|Description|
|---|---|
|Status|**Required:** Specifies the result of the request action. 0 indicates success.|

### CapabilitiesSupported
|Attribute|Description|
|---|---|
|CapabilityNames|**Required:** An interaction sent in respons to a QuerySupportedCapabilities request. The respons include a list of names of the supported capabilities for the Aggregate entity specified in the query. The names are one or more of "Aggregate", "Disaggregate", "Divide", "Merge", "Activate" and "Inactivate".|



## Disaggregate

A `Disaggregation` request is directed to a specific `AggregateFederate` and in the request a reference to the `AggregateUnit` is also provided. 

Disaggregation can only be performed on aggregate units registered in the federation. Therefore,  a federate receiving this request should first attempt to ensure registration and ownership of the aggregate unit. If this is not possible a `Response` interaction should be sent with `Status` as `UnknownUnit`.


<img src="./images/disaggregate.svg" width="100%"/>



<!--
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Disaggregate(Event, Unit, Federate)

Federate->Federation:Update Attribute Value (Unit, Status = Inactive)


loop Activate Parts
alt Part not in Federation
Federate->Federation:Register Object Instance (Part)
Federation<-Federate:Update Attribute Values (Part, Attributes)
end
Federation<-Federate:Update Attribute Value (Part, Status = Active)
end
Trigger<-Federate:Response(Event, Status)
autonumber off



-->
Figure: Disggregation of a unit

1. A trigger federate initiates a `Disaggregate` request to a specified `AggregateFederate` and reference to the `AggregateUnit` to deaggregate.
2. The receiving federate performs actions to ensure that the referenced `AggregateUnit` is registered in the federation as an `NETN_Aggregate` object instance and that the federate has ownership of the instance attributes. If not successful a `Response` interaction indicating `Status` as `UnknownUnit` is sent. If successful the federate updates the `Status` attribute of the object instance to `Inactive`.
3. The federate will register each subunit and physical entity not already in the federation.
4. Initial attribute values will be updated for registered objects based on aggregate unit information and potentiallty complemented with data provided by `NETN-ORG` `Unit`.
5. The federate ensure ownership of the attributes of all subunits and physical entities and the `Status` for each is updated to `Active`.
6. On successful completion, a `Response` interaction is sent with `Status` set as `Success`. If the federate is unsucessful the `Status` is set to the corresponding reason for failure.

## Aggregate

<img src="./images/aggregation.svg" width="100%"/>

<!-- 
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Aggregate(Event, Unit, Federate, RemoveParts)

loop Inactivate Parts
alt RemoveParts = true
Federate->Federation:Delete Object Instance (Part)

else RemoveParts = false
Federation<-Federate:Update Attribute Value (Part, Status = Inactive)
end
end
alt Unit not Registered
Federate->Federation:Register Object Instance (Unit)
end
Federate->Federation:Update Attribute Value (Unit, Attributes, Status = Active)

Trigger<-Federate:Response(Event, Status)
autonumber off

-->

Figure: Aggregation of a unit

1. A trigger federate initiates an `Aggregate` request to a  federate including a reference to the unit to aggregate.
2. If indicated in the request, the federate deletes all subunits and/or physical entities registered in the federation.
3. The federates updates `Status` of all subunits and physical entities to `Inactive` if indicated in the request that the parts should remain in the federation.
4. The federate will ensure registion and ownership of attributes of the aggregate unit. 
5. The federate updates all status attributes of the unit to refelct the aggregate state and sets the `Status` of the unit to `Active`.
6. On successful completion, a `Response` interaction is sent with `Status` set as `Success`. If the federate is unsucessful the `Status` is set to the corresponding reason for failure.
 

## Divide

<img src="./images/divide.svg" width="100%"/>

Figure: Divide an aggregate unit
<!--
participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Divide(Event, Unit, Federate, Parts)



loop Parts
alt Part not in Federation
Federate->Federation:Register Object Instance (Part)
Federation<-Federate:Update Attribute Values (Part, Attributes)
end
Federation<-Federate:Update Attribute Value (Part, Status = Active)
end
Federation<-Federate:Update Attribute Values (Unit, Attributes)
Trigger<-Federate:Response(Event, Status)
autonumber off
-->
## Merge

<img src="./images/merge.svg" width="100%"/>

Figure: Merge divided parts with their aggregate unit

<!--

participant Trigger
participant Federate
participant Federation
autonumber 
Trigger->Federate:Merge(Event, Unit, Federate, Parts)



loop Parts
Federation<-Federate:Delete Object Instance (Part)


end
Federation<-Federate:Update Attribute Value (Unit, Status = Active)


Trigger<-Federate:Response(Event, Status)
autonumber off


-->



## Activate
An aggregate can not be activiated if any subparts is active.
An aggrgate can not be activated if a divided units exist.

## Inactivate
A divided unit can not be inactivated. Must first be merged.
A aggegate unit can not be inactivated if a divided unit exists.


