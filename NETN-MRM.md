# NETN-MRM

Copyright (C) 2020 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md).

## Introduction

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for simulation. Depending on the purpose and requirements of the simulation, the models can have different levels of resolution and aggregation can be used to create representations of larger combined concepts. 

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module is a specification of how to perform aggregation and disaggregation of aggregated units into other aggregated subunits or entities, e.g. platforms, in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module is used to specify how data is represented and exchanged in the federation. The NETN-MRM FOM module is available as an XML file for use in HLA based federations.


### Purpose

The purpose of NETN-MRM is to support federations where models are represented at multiple levels of resolution and where the level of resolution can change dynamically during a simulation.

For example:
* Disaggregation of a Battalion represented as an Aggregate Entity into Company level Aggregate Entities
* Disaggregation of a Company to individual platforms such as vehicles and individual soldiers represented at an entity level
* Aggregation of platforms represented as individual entities to an attribute of an aggregate unit representing e.g. a Platoon.
* Divide UAV equipped vfrom a Company unit to simulate some reconnaissanse operation in more detail.
* Merge the Recce platoon on return from the mission with the Company unit. 

### Scope

NETN-MRM covers the following cases:

* Aggregation of subunits and/or physical entities
* Disaggregation of unit into subunits and/or physical entities
* Division of simulated unit into specific parts - resources divided and all entities simulated
* Merge of previously divided parts with simulated unit.
* Activate and Inactivate aggregate units' representation in the simulation

### Dependencies

NETN-MRM refers to units and entities by UUID as defined in NETN-ORG, NETN-Aggregate and NETN-Physical.

In the MRM patterns, the acquisition of Unit and Physical entities may use may use NETN-TMR for transferring modelling responsibility of attributes.

## Overview

NETN-MRM requires the use of NETN-Aggregate and NETN-Physical Object Classes for the repreaentation of aggregate units and physical entities in the simulation.

An aggregate unit can be represented in the simulation in various ways.

* Not explicitly registered in the federation as a simulated object but represented in scenario data such as NETN-ORG Unit or similar.

* Registered as a actively simulated aggregate unit. The state of the aggregate unit is explicitly simulated by federates in the distributed federated simulation.

* Registered as an inactive simulated Aggregate unit. The state of the aggregate unit is not explicitly simulated but derived from simulation of its subunits and/or pysical entities. 

* Registered as a divided aggregate unit where one or more of its parts are also simulated. Resources of the aggregated unit are split between the simulated entities when divided.

Deaggregation always constitutes a full deaggregation of all subunits and physical entities associated with an aggregate unit. The aggregated unit itself can remain registered in the federation as inactive and update state derived form the simulation of its deaggregated parts.

MRM events can be requested and triggered using HLA interactions defined in the NETN-MRM FOM Module. All events are directed to a federate responsible for managing the MRM event. The result of a MRM event is reported to the requesting/triggering federate using a `Response` interaction.

<img src="./images/NETN-MRM Interaction Class Tree.png" width="100%"/>

Figure: MRM interactions and events

The capability of a federate to support MRM events for a specific aggregate unit can be queried using the `QuerySupportedCapabilities` interaction. The response is provided using `CapabilitiesSupported` interaction and includes a list of names of available MRM events.


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

## Inactivate

## Response

