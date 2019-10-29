# NETN-MRM

Copyright (C) 2019 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md).

## Introduction

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for the purpose of a simulation. Depending on the purpose and requirements of the simulation, the models can have different levels of resolution and aggregation can be used to create representations of larger combined concepts. 

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module is a specification of how to perform negotiated and coordinated aggregation and disaggregation of models representing organizational units and individual entities, e.g. platforms, in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module is used to specify how data is represented and exchanged in the federation. The NETN-MRM FOM module is available as an XML file for use in HLA based federations.


### Purpose

The purpose of NETN-MRM is to support federations where models are represented at multiple levels of resolution and where the level of resolution can change dynamically during simulation.

The NETN-MRM FOM module provides a standard interface and protocol for conducting negotiated and coordinated aggregation and disaggregation of simulated units and entities.

For example:
* Disaggregation of a Batallion represented as an Aggregate Entity into Company level Aggregate Entities
* Disaggregation of a Company to individual platforms such as vechicles and individual soldiers represented at an entity level
* Aggregation of platforms represented as individual entities to an attribute of an aggregate unit representing e.g. a Platoon.
* Triggering of Aggregation by user command
* Triggering of Disaggregation based on geo-fencing

### Scope

NETN-MRM covers the following cases:

* Disaggregation of AggregateEntity into lower level AggregateEntities
* Disaggregation of AggregateEntity into Platforms
* Aggregation of AggregateEntities into higher level AggregateEntity
* Aggregation of Platforms into AggregateEntity
* Triggering of Disaggregation
* Triggering of Aggregation

### Dependencies

NETN-MRM is limited to units and entities referenced by UUID as defined in NETN-ORG, NETN-Aggregate and NETN-Physical. 

NETN-MRM may use NETN-TMR for transferring modelling responsibility of attributes as part of disaggregation and aggregation.

## Overview

An aggregate unit can be represented in the simulation at various state of aggregation.

* In a **Fully Disaggregate** state, the aggregate unit may exist in the federation with status set as `inactive` but no update of attributes are made. Instead, **all** subunits and/or entities are registered and updated in the federation.

* In a **Partially Disaggregate** state, the aggregate unit still updates all of its attributes but some entities and/or (sub)units are also represented and updated in the federation. The aggregate unit's `silent` attribute is used to reference these entitis.

*If Pseudo-Disaggregate, the aggregate unit sends updates and all Entities and/or (sub)units of the aggregate unit are updated but these “are not capable of full interaction with other entities” In a Disaggregate state, the aggregate unit sends updates and entities and/or (sub)units of the aggregate unit are updated*





The NETN-MRM concept of aggregation and disaggregation involves the following basic steps:

1. take the ownership of simulated entity attributes
2. publishing new entities (if not already exist) either on a more aggregate or disaggregate level and 
3. transfer modelling responsibility of these new entities to another federate.

This requires the use of a MRM Management federate meeting the following requirements and functions: 

* Responds to MRM Trigger interactions
* Implements NETN-TMR
* Have a representation of the Organization (ORBAT) with all units including UUIDs and holdings by subscription to NETN-ORG Units or by MSDL file.
* Publish and subscribe to the NETN-Aggregate object class
* Publish and subscribe to the NETN-Physical object class
* Register NETN-Aggregate and/or NETN-Physical when required during aggregation/disaggregation
* Update or transfer the modelling responsibility of updating aggegate unit attributes during the time the aggregate unit is in disaggregated state.
* Set the `Status` attribute of a fully disaggregated NETN-Aggregate entity to `inactive`.



### High Resolution Units
The High Resolution Units are the disaggregated elements of the original aggregate, and can themselves be smaller aggregates. A high resolution unit is an instance of the NETN Aggregate class or a NETN-Physical leaf class.

### Aggregate Dynamic Attribute Update Responsibility

When an aggregate unit is disaggregated, modelling responsibility for the aggregate unit's dynamic values shall be transferred to the MRM SP. Attributes with a static value shall not be transferred. The MRM SP is then responsible for updating the dynamic attributes. .

The federate responsible for the aggregate unit in Aggregate State gets information about the status of the subunits when the MRM SP updates the dynamic attributes of the aggregate unit according to the changes to the physical entities (the aggregate federate may not subscribe to physical object classes).


 
### High Resolution Dynamic Attribute Update Responsibility
The MRM SP registers the high resolution entities. Attributes with static values shall not be transferred and shall be initialised (and updated on a request) by the MRM SP with data from the ORBAT, e.g. UniqueID, Callsign, EntityType. Dynamic attributes shall be updated by a federate with the required capability.



### Aggregate Federate
The Aggregate Federate (Aggregate Sim) publishes and subscribes to the NETN-Aggregate class.

### Disaggregate Federate
The Higher Resolution Federate (HRF or Disaggregate Sim) shall publish and subscribe to the NETN-Physical leaf classes. It shall have the capability to manage physical entity attributes, such as movement and damage assessment. The Higher Resolution Federate can also publish and subscribe to the NETN-Aggregate class, e.g. when the result of a disaggregation of a battalion is units at platoon level.

### Trigger Federate
A federate that triggers a MRM event shall have the capability to recognize events that require a higher resolution than the Aggregate Federate can provide to the simulation. This capability can be included in any federate e.g. the MRM SP or an Aggregate Federate. The functionality that the Trigger Federate represents can be replaced if the MRM SP is manually operated and the Trigger Federate is omitted.


 
### MRM Interactions
 
Figure 6-1: The Interaction Classes in the MRM FOM Module.
For a description of the MRM interactions and the interaction parameters see the MRM FOM module.




## Disaggregation

1.	The MRM SP sends an MRM_DisaggregationRequest to both the Aggregate Sim and the Disaggregate Sim (Higher Resolution Federate).
2.	The MRM SP waits for the MRM_DisaggregationResponse from the Aggregate Sim. When the MRM SP receives the response it makes a TMR request (acquire) for instance attributes at the aggregate object instance.
3.	The HLA Ownership callback attributeOwnershipNotification to the MRM SP indicates that the TMR action between the MRM SP and the Aggregate Sim is complete.
4.	The MRM SP registers entities if they do not already exist, and updates static and dynamic attributes of the entities.
5.	When the Disaggregate Sim has discovered all entities specified in the UuidList of the associated MRM_DisaggregationRequest, and is ready to manage the entities, it sends the MRM_DisaggregationResponse.
6.	The MRM SP waits for the MRM_DisaggregationResponse from the Disaggregate Sim. When the MRM SP receives the response, it starts the transfer of modelling responsibility. The MRM SP uses the TMR pattern to divest the ownership of instance attributes with dynamic attribute values to the Disaggregate Sim.
7.	The MRM SP will need to divest ownership of attributes of multiple object instances during an MRM disaggregation process. For each (entity) object instance: The HLA Ownership service attributeOwnershipDivestureIfWanted is a synchronized service and returns a set of instance attributes that have been divested. This is an indication for the MRM SP that the TMR action for an object instance (entity) with the Disaggregate Sim is complete.
8.	When the TMR-actions are completed for all entities, the MRM SP sends the interaction MRM_ActionComplete to inform the Aggregate Sim and the Disaggregate Sim that the disaggregation is completed successfully (or failed).
9.	The Disaggregate Sim updates the entity attribute values according to the events in the federation execution.
10.	The MRM SP listens to the entity updates and then updates the corresponding attributes of the aggregate unit.
11.	The Aggregate Sim listens to the updated attributes of the aggregate instance.
 
Figure 6-2: MRM Disaggregate. MRM Disaggregation Sequence Diagram.




## Aggregation

1.	The MRM SP sends an MRM_AggregationRequest to both the Aggregate Sim and the Disaggregate Sim (Higher Resolution Federate).
2.	The MRM SP waits for the MRM_AggregationResponse from the Aggregate Sim and the Disaggregate Sim. If both federates are able to comply with the aggregation request, the MRM SP starts the transfer of modelling responsibility (TMR) pattern to acquire the ownership of instance attributes with dynamic attribute values from the Disaggregate Sim.
3.	The MRM SP will need to acquire ownership of attributes of multiple object instances during a MRM aggregation process. For each (entity) object instance: The HLA Ownership callback attributeOwnershipNotification indicates that the TMR action between the MRM SP and the Disaggregate Sim has completed for the entity.
4.	The entities are either deleted or deactivated by the MRM SP.
5.	The MRM SP makes a TMR request (divest) to the Aggregate Sim for instance attributes of the aggregate object instance.
6.	The HLA Ownership service attributeOwnershipDivestureIfWanted is a synchronized service and returns a set of instance attributes that have been divested. This is an indication for the MRM SP that the TMR action for the aggregate object instance with the Aggregate Sim is complete.
7.	The MRM SP sends MRM_ActionComplete to inform the Aggregate Sim, and Disaggregate Sim that the aggregation is complete or cancelled.
 
Figure 6-3: MRM Aggregate. MRM Aggregation Sequence Diagram.




## MRM TRIGGER
1.	The MRM_Trigger interaction is sent from a federate that in some sense can detect the need for a disaggregation. It is not mandated that the MRM_Trigger interaction must be sent to the MRM Service Provider (MRM SP). The disaggregation process can start without a trigger interaction, e.g. a manually operation.
2.	The MRM SP sends an MRM_TriggerResponse to the Trigger Federate, which contains the TransactionId which is used in subsequent interactions related to this MRM_Trigger.
3.	The Disaggregation or the Aggregation sequence is executed depending on the AggregateState in the MRM_Trigger interaction.
4.	The MRM SP sends MRM_ActionComplete to inform the Trigger Federate that the aggregation is complete.
 
Figure 6-4: A Trigger Activates a Disaggregation or an Aggregation Event.

## UNABLE TO COMPLY
If the service for some reason is not doable by the MRM SP it shall first respond with an MRM_TriggerResponse and then send an MRM_ActionComplete with the parameter CompletionResult (Boolean) indicating that the requested action in the MRM_Trigger will not be executed. One reason for a negative response can for example be that an aggregate unit already has the requested aggregate state or a TMR action was not performed.
 
Figure 6-5: The MRM Request from the Trigger is Not Performed.
 

<!--
participant Aggregate

participant MRM Manager

participant Disaggregate


autonumber 

MRM Manager ->> Aggregate:DisaggregateRequest(Aggregate, LevelOfDisaggregation)
Aggregate->>MRM Manager:Offer(OK)

aboxright over Aggregate, MRM Manager:TMR Acquisition(Aggregate.Attributes)


Disaggregate <- MRM Manager:Register Entities(DisaggregateEntities)


aboxright over Disaggregate, MRM Manager:TMR Divestiture(DisaggregateEntities.Attributes)

Disaggregate -> MRM Manager:Update Attributes(DisaggregateEntities.Attributes)

autonumber off
-->