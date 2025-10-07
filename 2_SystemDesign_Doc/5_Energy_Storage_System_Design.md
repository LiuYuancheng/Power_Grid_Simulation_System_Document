# Design of Energy Storage System In Power Grid OT Cyber Range

**Project Design Purpose** : This article introduces the design of a Battery Based Energy Storage System (BESS) within the OT Power Grid Simulation Cyber-Physical Environment that I developed. The goal is to use the software simulated realistic manageable BESS to demonstrate how large-scale energy storage supports the operation of a modern power grids.

![](5_Energy_Storage_System_Design_Img/logo_small.png)

In practice, BESS plays a vital role in balancing supply and demand by storing excess electricity from renewable energy sources such as solar and wind, and releasing it back to the grid when needed. Beyond energy balancing, BESS also provides essential services such as maintaining power supply continuity and supporting grid restart during or after outages. To make these functions tangible for cyber exercises and educational use, the project is structured into four main sections:

- **Physical BESS Simulation** – Implementation of three mid-size, 40 kWh battery energy storage stations integrated into the physical-world simulator of the power grid cyber range.
- **Electrical Devices and Energy Flow** – Modeling the electrical devices (such as battery pack, DC-DC-unit...), data collection from the battery stations, and representation of power flow control between BESS and the power grid.
- **OT Controller Design** – Using metering units and PLCs to mimic the controller layer for monitoring, battery control, and automatic management logic at each storage station.
- **Centralized Grid Control (HMI/SCADA)** – Designing a power grid operations center interface to connect to the SCADA network for monitoring and managing all energy storage stations.

This design framework highlights how battery-based storage can be realistically represented within a cyber range, making it possible to study and train on grid operations, control, and cybersecurity in a controlled simulation environment.

**Important** : The Battery energy storage system in cyber range  will distill (**NOT** 1:1 emulate) a few OT processes from the real world and use digital constructs to represent them for the cyber exercise and education usage. In the real world the Battery energy storage systems (BESS) is much more complex. 

```python
# Author:      Yuancheng Liu
# Created:     2025/09/23
# Version:     v_0.0.1
# Copyright:   Copyright (c) 2025 Liu Yuancheng
# License:     GNU General Public License V3
```

Table of Contents

[TOC]

------

### Introduction

This BESS is a new subsystem of the [Power Grid Simulation OT Cyber Range](https://www.linkedin.com/pulse/power-grid-ot-simulation-system-yuancheng-liu-dpplc) (`Version2.0`), it is embedded within the power generation stage of the grid for enhancing grid reliability, capacity and resilience through energy storage and delivery. We developed three mid-size 40 kWh BESS station in the system to linked with different power generation source or power collection node  (solar plant, oil plant and the substation) to support the reliability 185KWh(Max) power generation source.

As a simulation system, we only simulate part of the basic components and the related reading/information in the real BESS as shown below (Picture from [Volvo Penta main page](https://www.volvopenta.com/industrial/battery-energy-storage/?utm_source=google&utm_medium=cpc&utm_campaign=SI_search-BESS_phrase&utm_content=&gad_source=1&gad_campaignid=21901328062&gbraid=0AAAAAozWkbnoHn2eJuiAvLdXR6sbNBpPl&gclid=CjwKCAjwisnGBhAXEiwA0zEORzkOMW7mcfFOm7rkM_-W0W2QGQlL_xZUvZFt8RrahJO9xDbDRgVnWhoCnwwQAvD_BwE) ). 

















------

### Reference

- https://en.wikipedia.org/wiki/Grid_energy_storage
- https://www.gminsights.com/industry-analysis/energy-storage-systems-market?gad_source=1&gad_campaignid=21841937168&gbraid=0AAAAACuPGhW9A41yu679JCuOFyIOTxLRx&gclid=CjwKCAjwisnGBhAXEiwA0zEOR8s86vp5s4b601uN1anye_K9fQlpZlEQtxNF8SO68G8cOFxTQ5PdthoC8eQQAvD_BwE
- https://www.volvopenta.com/industrial/battery-energy-storage/?utm_source=google&utm_medium=cpc&utm_campaign=SI_search-BESS_phrase&utm_content=&gad_source=1&gad_campaignid=21901328062&gbraid=0AAAAAozWkbnoHn2eJuiAvLdXR6sbNBpPl&gclid=CjwKCAjwisnGBhAXEiwA0zEORzkOMW7mcfFOm7rkM_-W0W2QGQlL_xZUvZFt8RrahJO9xDbDRgVnWhoCnwwQAvD_BwE