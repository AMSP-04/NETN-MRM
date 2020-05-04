# NETN-MRM
NATO Education and Training Network (NETN) Multi-Resolution Modelling (MRM) Module


## Introduction

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for simulation. Depending on the purpose and requirements of the simulation, the models can have different levels of resolution and aggregation can be used to create representations of larger combined concepts. 

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module is a specification of how to perform aggregation and disaggregation of aggregated representation of entities, e.g. units, into other levels of aggregation or individual entities, e.g. platforms, in a federated distributed simulation. 

The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and primarily intended to support interoperability in a federated simulation (federation) based on HLA. A Federation Object Model (FOM) Module specifies how data is represented and exchanged in the federation. The NETN-MRM FOM module is available as an XML file for use in HLA based federations.


### Purpose

The purpose of NETN-MRM is to support federations with entities represented at multiple levels of resolution and where the level of resolution can change dynamically during a simulation. It supports patterns for aggregation and disaggregation of units, and division and merging of unit resources.

For example:
* Disaggregation of a Battalion represented as a `NETN_Aggregate` object into Company level `NETN_Aggregate` objects.
* Disaggregation of a Company level unit, represented as a `NETN_Aggregate` object, to individual platforms, e.g. `NETN_GroundVehicle` objects.
* Aggregation of platforms represented as, e.g. `NETN_GroundVehicle` objects, to an attribute of a unit, e.g. a Platoon, represented as a `NETN_Aggregate` object.
* Dividing individual equipment, e.g. UAV, from a Company unit to simulate some reconnaissance operation in more detail.
* Merging of a Recce platoon, represented as a `NETN_Aggregate` object, on its return from a mission with its source Company unit. 

### Scope

NETN-MRM covers the following cases:

* Aggregation of simulated subunits or physical entities.
* Disaggregation of a unit into subunits or physical entities.
* Division of simulated unit with re-allocation of resources between source unit and divided unit.
* Division of simulated unit where equipment resources are removed from source unit and instead represented as physical entities.
* Merging of previously divided entities with source unit.
* Activation or Inactivation of entities represented in the simulation.

## Licence

Copyright (C) 2020 NATO/OTAN.
This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md). 

The work includes the [NETN-MRM.xml](NETN-MRM.xml) FOM Module and documentation NETN-MRM.md.

Above licence gives you the right to use and redistribute the NETN FOM Module (XML file and Documentation) in its entirety without modification. You are also allowed to develop new FOM Modules (in separate XML files and separate documentation) that build-on/extends the NETN module by reference and including necessary scaffolding classes. You are NOT allowed to modify this FOM Module or its documentation without prior permission by the NATO Modelling and Simulation Group. 

## Versions, updates and extensions

All updates and versioning of this work is coordinated by the NATO Modelling and Simulation Coordination Office (MSCO), managed by the NATO Modelling and Simulation Group (NMSG) and performed as NATO Science and Technology Organization (STO) technical activities in support of the NMSG Modelling and Simulation Standards Subgroup (MS3).

Feedback on the use of this work, suggestions for improvements and identified issues are welcome and can be provided using GitHub issue tracking. To engage in the development and update of this FOM Module please contact your national NMSG representative.

Version numbering of this FOM Module and associated documentation is based on the following principles:

* New official version number is assigned and in effect only when a new release is made in the Master branch.
* Updates in the Develop branch will not change the version number.
* In the FOM Module `useHistory` information includes only information on official releases.
* Update of the major version number is made if the change constitutes a major restructuring, merging, addition or redefinition of semantics that breaks backward compatibility or cover entirely new concepts.
* Update of the minor version number is made if the change constitutes minor additions to existing concepts and editorial changes that do not break backward compatibility but may require updates of software to use new concepts.
* Patches are released to fix minor issues that do not break backward compatibility.

|Version|Description|
|---|---|
|v1.1.1 |Initial version of NETN-MRM FOM Module released as part of NETN-FOM v2.0 in AMSP-04 Ed A. |
|v2.0.0 |Updated version of NETN-MRM FOM Module released as part of NETN-FOM v3.0 in AMSP-04 Ed B. |

[Changelog](changelog.md)

## Documentation

[Full Documentation](NETN-MRM.md)
