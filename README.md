# NETN-MRM


|Version| Date| Dependencies|
|---|---|---|
|3.0|2023-04-09|NETN-ETR, NETN-BASE|

> [Full Documentation](NETN-MRM.md)

The purpose of NETN-MRM is to support federations using multiple levels of resolution and aggregation and where the level of resolution changes during a simulation.

Models of real-world objects, processes and phenomena are used to create a synthetic representation suitable for the simulation. Individual entities represent distinct objects in the scenario, while aggregated models represent a group of associated physical entities.

The NATO Education and Training Network Multi-Resolution Modelling (NETN-MRM) FOM Module specifies how to perform aggregation and disaggregation of aggregated entities, e.g. units, to other levels of aggregation or physical entities.


The MRM FOM module specifies interaction classes necessary to enable federation multi-resolution modelling. The specification is based on IEEE 1516 High Level Architecture (HLA) Object Model Template (OMT) and intends to support interoperability in a federated simulation (federation) based on HLA. An HLA-based Federation Object Model (FOM) is used to specify types of data and their encoding on the network. 

NETN-MRM covers the following cases: 
* Aggregation of entities representing subunits or physical entities
* Disaggregation of entities representing a unit into entities representing subunits or physical entities
* Division of simulated entities into parts - resources divided and all entities simulated
* Merge of previously divided entities.

## License

Copyright (C) 2020 NATO/OTAN. This work is licensed under a [Creative Commons Attribution-NoDerivatives 4.0 International License](LICENCE.md).

The work includes the NETN-MRM.xml FOM Module and documentation.

The licence gives you the right to use and redistribute the NETN FOM Module (XML file and Documentation) in its entirety without modification. You are also allowed to develop new FOM Modules (in separate XML files and separate documentation) that build on or extend the NETN module by referencing and including necessary scaffolding classes. You are NOT allowed to modify this FOM Module or its documentation without prior permission from the NATO Modelling and Simulation Group.

## Versions, updates and extensions

All updates and versioning of this work are coordinated by the NATO Modelling and Simulation Coordination Office (MSCO), managed by the NATO Modelling and Simulation Group (NMSG) and performed as NATO Science and Technology Organization (STO) technical activities in support of the NMSG Modelling and Simulation Standards Subgroup (MS3).

Feedback on the use of this work, suggestions for improvements and identified issues are welcome and can be provided using GitHub issue tracking. To engage in the development and update of this FOM Module please contact your national NMSG representative.

Version numbering of this FOM Module and associated documentation is based on the following principles:

* A new official version number is in effect only when a new release is made in the Master branch.
* An update of the major version number is made if the change constitutes a major restructuring, merging, addition or redefinition of semantics that breaks backward compatibility or covers entirely new concepts.
* An update of the minor version number is made if the change constitutes minor additions to existing concepts and editorial changes that do not break backward compatibility but may require updates of software to use new concepts.
* Patches are released to fix minor issues that do not break backward compatibility.

|Version|
|---|
|v1.1 - Initial version of NETN-MRM FOM Module released as part of NETN FOM v2.0.|
|v2.0 - Updated version by MSG-163 to be part of NETN FOM v3.0.|
|v3.0 - Updated version by MSG-191 to be part of NETN FOM v4.0|

> [Changelog](changelog.md)

