# NETN-MRM

Copyright (C) 2019 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENSE.md).

## Introduction

## Purpose

The purpose of MRM is to enable interoperability sufficient for the intended purpose of the federation among HLA federates employing different levels of resolution.

Resolution is defined as "The degree of detail used to represent aspects of the real world or a specified standard or referent by a model or simulation" [1] as it supports development of guidance and/or standards representing different "dimensions" of resolution [2]. Davis and Bigelow decompose an object's resolution into process, spatial, and temporal dimensions with the object-related dimension further decomposed into three components: entity level, attributes, and logical dependencies among attributes.

## Scope

## MRM PATTERN CONCEPT

### Modelling Responsibility
The MRM Pattern uses the Transfer of Modelling Responsibility Pattern to transfer ownership of instance attributes during the disaggregation and aggregation events. This is only sparsely described in this chapter; for detailed information about the TMR Pattern; see the Transfer of Modelling Responsibility (TMR) chapter.

### High Resolution Units
The High Resolution Units are the disaggregated elements of the original aggregate, and can themselves be smaller aggregates. A high resolution unit is an instance of the NETN Aggregate class or a NETN-Physical leaf class.

### MRM Service Provider

The MRM pattern describes a functionality implemented in a federate, either a federate whose main purpose is to act as a MRM Service Provider (MRM SP) or it may be implemented in a federate with another main purpose, e.g. a CGF federate. 

The MRM SP requirements are:
a.	The MRM SP shall have knowledge about the hierarchy in the ORBAT, which can be found in the MSDL file that defines the scenario units.
b.	The MRM SP shall publish and subscribe to the NETN-Aggregate class.
c.	The MRM SP is responsible for actions associated with the multi-resolution modelling. For example, the MRM SP is responsible for registering the entity instances (NETN-Physical leaf classes) according to the hierarchy defined in the ORBAT.
d.	The MRM SP may be able to manage the high resolution units, for example, moving the units and assigning damage to them. The attributes associated with these capabilities shall be published and subscribed to.
e.	If the MRM SP does not have the capability to manage the high resolution units and is not responsible for modelling these, it shall have knowledge of which federates are capable, and transfer the modelling responsibility (TMR) appropriately.
f.	Only one MRM SP shall respond to a Trigger request. If more than one MRM SPs exist in a federation execution then the responsibility for aggregate units shall be divided between the MRM SPs.

### Aggregate Dynamic Attribute Update Responsibility

When an aggregate unit is disaggregated, modelling responsibility for the aggregate unit's dynamic values shall be transferred to the MRM SP. Attributes with a static value shall not be transferred. The MRM SP is then responsible for updating the dynamic attributes. According to the DIS standard a fully disaggregated unit shall not be updated. The MRM pattern modifies this, the MRM SP shall update the dynamic attributes and the Status attribute (NETN) shall be set to Inactive.

The federate responsible for the aggregate unit in Aggregate State gets information about the status of the subunits when the MRM SP updates the dynamic attributes of the aggregate unit according to the changes to the physical entities (the aggregate federate may not subscribe to physical object classes).
Table 6-1: Responsibility for Updating the Dynamic Attributes 
at NETN Aggregate Instances in Different Aggregate States.
 
### High Resolution Dynamic Attribute Update Responsibility
The MRM SP registers the high resolution entities. Attributes with static values shall not be transferred and shall be initialised (and updated on a request) by the MRM SP with data from the ORBAT, e.g. UniqueID, Callsign, EntityType. Dynamic attributes shall be updated by a federate with the required capability.

### Aggregate Federate
The Aggregate Federate (Aggregate Sim) publishes and subscribes to the NETN-Aggregate class.

### Higher Resolution Federate
The Higher Resolution Federate (HRF or Disaggregate Sim) shall publish and subscribe to the NETN-Physical leaf classes. It shall have the capability to manage physical entity attributes, such as movement and damage assessment. The Higher Resolution Federate can also publish and subscribe to the NETN-Aggregate class, e.g. when the result of a disaggregation of a battalion is units at platoon level.

### Trigger Federate
A federate that triggers a MRM event shall have the capability to recognize events that require a higher resolution than the Aggregate Federate can provide to the simulation. This capability can be included in any federate e.g. the MRM SP or an Aggregate Federate. The functionality that the Trigger Federate represents can be replaced if the MRM SP is manually operated and the Trigger Federate is omitted.

### Identification
To identify units (aggregates and physical units) each unit shall have a specified unique id represented in an attribute. This attribute is defined in the NETN FOM modules and the value is specified in the MSDL specification of the ORBAT (or equivalent).
 
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
 
