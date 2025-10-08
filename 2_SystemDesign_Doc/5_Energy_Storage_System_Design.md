# Design of Energy Storage System In Power Grid OT Cyber Range

**Project Design Purpose** : This article introduces the design of a Battery Based Energy Storage System (BESS) within the OT Power Grid Simulation Cyber-Physical Environment that I developed. The goal is to use the software simulated realistic manageable BESS to demonstrate how large-scale energy storage supports the operation of a modern power grids.

![](5_Energy_Storage_System_Design_Img/logo_small.png)

In practice, BESS plays a vital role in balancing supply and demand by storing excess electricity from renewable energy sources such as solar and wind, and releasing it back to the grid when needed. Beyond energy balancing, BESS also provides essential services such as maintaining power supply continuity and supporting grid restart during or after outages. To make these functions tangible for cyber exercises and educational use, the project is structured into four main sections:

- **Physical BESS Simulation** – Implementation of three mid-size, 40 kWh battery energy storage stations integrated into the physical-world simulator of the power grid cyber range.
- **Electrical Devices and Energy Flow** – Modeling the electrical devices (such as battery pack, DC-DC-unit...), data collection from the battery stations, and representation of power flow control between BESS and the power grid.
- **OT Controller Design** – Using metering units and PLCs to mimic the controller layer for monitoring, battery control, and automatic management logic at each storage station.
- **Centralized Grid Control (HMI/SCADA)** – Designing a power grid operations center interface to connect to the SCADA network for monitoring and managing all energy storage stations.

This design framework highlights how battery-based storage can be realistically represented within a cyber range, making it possible to study and train on grid operations, control, and cybersecurity in a controlled simulation environment.

> **Important** : The Battery energy storage system in cyber range  will distill (**NOT** 1:1 emulate) a few OT processes from the real world and use digital constructs to represent them for the cyber exercise and education usage. In the real world the Battery energy storage systems (BESS) is much more complex. 

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

The **Battery Energy Storage System (BESS)** introduced in this work is a newly developed subsystem of the [**Power Grid Simulation OT Cyber Range**](https://www.linkedin.com/pulse/power-grid-ot-simulation-system-yuancheng-liu-dpplc) (**Version 2.0**). It is designed as part of the **power generation stage** of the simulated grid, enhancing overall reliability, capacity, and resilience through dynamic energy storage and delivery mechanisms. Within this system, three mid-size 40 kWh BESS stations have been developed and integrated with different power collection nodes — including the main out transformer of a solar power plant, an oil-based power plant, and a substation — collectively supporting up to 185 kWh (maximum) of simulated power generation capacity.

As a simulation platform, the goal is **not** to replicate a full-scale industrial BESS in all its complexity, but rather to model selected core components and basic operational behaviors that generate realistic data and interactions representative of a real system. The simulated subsystems are inspired by the concept and idea of industrial solutions from [**Volvo Penta**](https://www.volvopenta.com/industrial/battery-energy-storage/?utm_source=google&utm_medium=cpc&utm_campaign=SI_search-BESS_phrase&utm_content=&gad_source=1&gad_campaignid=21901328062&gbraid=0AAAAAozWkbnoHn2eJuiAvLdXR6sbNBpPl&gclid=CjwKCAjwisnGBhAXEiwA0zEORzkOMW7mcfFOm7rkM_-W0W2QGQlL_xZUvZFt8RrahJO9xDbDRgVnWhoCnwwQAvD_BwE), and focus on the essential part of structural and functional layers of a BESS (as illustrated below).

![](5_Energy_Storage_System_Design_Img/s_03.png)

The key feature simulated in the BESS simulation covers the components from Level 0 OT Env Layer (Physical Process Field I/O  Device) to the Level 2 OT Env Layer (Power Grid OCC SCADA-HMI). A simulated function mapping table is shown below:

| BESS Subsystem                | Simulated Function/data                                      | OT Env Level              |
| ----------------------------- | ------------------------------------------------------------ | ------------------------- |
| [1] Cube battery pack         | Batteries set pack capacity value [kWh, Percentage]          | level-0                   |
| [2] Battery management system | Simulate the auto battery charge/discharge control sequence with PLC | level-0, level-1          |
| [4] Monitoring System         | Simulate the MU-PLC data collection and the HMI data visualization | level-0, level-1, level-2 |
| [5] Electrical safety         | Simulate the charge/discharge, volage, current safety control in PLC level and the alarm visualization in HMI level | level-1, level-2          |
| [7] DC interface              | Battery link with the power collection node to provide 48V DC power transmission interface | level-0                   |
| [9] OEM Scope                 | Simulate the basic DC-AC converter to integrate in the power plants AC-AC step up transformer | level-0                   |

Use in cyber exercise

This BESS integration not only strengthens the realism of the Power Grid OT Cyber Range but also serves as an cyber exercise educational and research platform for exploring energy storage control, grid automation, and OT cybersecurity in hybrid energy systems. As the system is designed for cyber exercise, so the main usage for the simulated BESS is providing a target system or provide different attack vectors in the grid for the red team to attack and blue team to defense. It can also use for an important blue team exercise which is real time incidence response, when the attacker launches the attack to cause the power outage, the BESS system will support power to keep the system working for a time interval and the blue team need to find way to recover the system before the BESS's battery used up.  



------

### Background knowledge about Energy Storage System (ESS) in the Power Grid

Before we start introduce the technical detail of BESS, we will also show some  background knowledge of Energy Storage System n the Power Grid. The below image from wiki (https://en.wikipedia.org/wiki/Grid_energy_storage)  summrize clearly about the function and the energy flow of the ESS.

![](5_Energy_Storage_System_Design_Img/s_04.png)

An **Energy Storage System (ESS)** is a critical component in modern power grid systems, designed to store excess electrical energy and release it when needed. ESS plays a vital role in balancing supply and demand, improving grid stability, and enabling the integration of renewable energy sources such as solar and wind, which are inherently intermittent.

By storing surplus energy during periods of low demand or high renewable generation, and discharging it during peak demand or supply shortages, ESS enhances the **reliability, flexibility, and resilience** of the grid. Different technologies are used in ESS, including **batteries (lithium-ion, flow batteries, lead-acid)**, **mechanical storage (pumped hydro, flywheels, compressed air)**, and **thermal storage**.

Key functions of ESS in the power grid include:

- **Peak Shaving & Load Leveling** – Reducing stress on generation and transmission during high-demand periods.
- **Frequency & Voltage Regulation** – Providing fast-response services to maintain grid stability.
- **Renewable Integration** – Storing excess renewable energy for use during low-generation periods.
- **Backup Power & Resilience** – Ensuring reliable supply during outages or emergencies.

As the global energy landscape shifts toward **decarbonization and smart grids**, ESS has become an enabling technology for **microgrids, smart manufacturing, hybrid power systems, and future energy markets**.





------

### BESS Architecture Overview

The components of the BESS in the Power Grid Cyber Range's three levels of OT environment architecture is high lighted in the  below diagram :

![](5_Energy_Storage_System_Design_Img/s_05.png)

**Level 0 Physical Process Field I/O  Device Layer**

At the **physical-world equipment simulation level**, the system models several key BESS components, including:

- Cube battery packs
- Battery Management System (BMS)
- Monitoring and electrical safety systems
- OEM-level connection and control scope

In the simulated configuration, each BESS is positioned adjacent to the energy collection transformer of its associated power source. The BESS shares the same DC-AC/DC-DC step-up transformer as the generators. Functionally, the BESS acts as a direct power consumer when charging and as a power generator when discharging. The modeled DC interface between the battery system and the transformer (within the OEM scope) operates at 48 V DC.

**Level 1 OT System Controller LAN** 

At the OT controller level, the system employs a PLC simulation program that emulates an [**ABB AC500 PLC**](https://new.abb.com/plc/plc-technology/ac500-plc-technical-features/ac500-connectivity/iec-60870), communicating via the IEC 60870-5-104 protocol. This setup allows the simulated controller to function as a flexible Remote Terminal Unit (RTU) within different layers of the automation architecture. The PLC autonomously manages the BESS’s charge/discharge operations based on linked transformer output, collects relevant operational data, and transmits it back to the power grid control HMI through the simulated RTU communication channel.

**Level 2 Power Grid Control Center Processing LAN**

On the SCADA-HMI side, the system provides a comprehensive visualization and control interface to display the real-time BESS information, enables manual overload and control commands with deferent gauge, indicator control buttons. It also integrates automated safety and protection mechanisms to ensure realistic grid management behavior within the cyber range environment.



------















------













------

### Reference

- https://en.wikipedia.org/wiki/Grid_energy_storage
- https://new.abb.com/plc/plc-technology/ac500-plc-technical-features/ac500-connectivity/iec-60870
- https://www.gminsights.com/industry-analysis/energy-storage-systems-market?gad_source=1&gad_campaignid=21841937168&gbraid=0AAAAACuPGhW9A41yu679JCuOFyIOTxLRx&gclid=CjwKCAjwisnGBhAXEiwA0zEOR8s86vp5s4b601uN1anye_K9fQlpZlEQtxNF8SO68G8cOFxTQ5PdthoC8eQQAvD_BwE
- https://www.volvopenta.com/industrial/battery-energy-storage/?utm_source=google&utm_medium=cpc&utm_campaign=SI_search-BESS_phrase&utm_content=&gad_source=1&gad_campaignid=21901328062&gbraid=0AAAAAozWkbnoHn2eJuiAvLdXR6sbNBpPl&gclid=CjwKCAjwisnGBhAXEiwA0zEORzkOMW7mcfFOm7rkM_-W0W2QGQlL_xZUvZFt8RrahJO9xDbDRgVnWhoCnwwQAvD_BwE