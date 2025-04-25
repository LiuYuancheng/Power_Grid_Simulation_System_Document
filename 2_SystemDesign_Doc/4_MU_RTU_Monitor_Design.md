# Power_Grid_Simulation_System [ MU-IED-RTU Monitor Flow Design ]

**Project Design Purpose** : This document provides a detailed overview of the monitoring subsystem inn OT power grid simulation environment, with a focus on the MU-IED-RTU Monitor Flow Design. It examines the functions and interactions of Metering Units (MU), Intelligent Electronic Devices (IED), and Remote Terminal Units (RTU) within the state monitoring architecture of a real-world power grid. The content is organized into three main sections:

- An introduction to the core functions of MU, IED, and RTU devices, and how they work together to enable real-time monitoring and control of the grid.
- A detailed explanation of how these components are digitally replicated within our power grid simulation system. 
- A technical breakdown of the software design, communication architecture, and monitoring workflow that underpin the operation of this digital twin environment.

![](../img/logo_small.png)

```python
# Author:      Yuancheng Liu
# Created:     2025/04/19
# Version:     v_0.2.0
# DocNum:      Wiki_2_2
```

**Table of Contents**

[TOC]



------

### Project Introduction 

In modern Operational Technology (OT)-based power grid monitoring systems, **Metering Units (MU)**, **Intelligent Electronic Devices (IED)**, and **Remote Terminal Units (RTU)** play critical roles in ensuring real-time state awareness and control. These devices are integral to digital substations that operate under the IEC 61850 standard, which defines the communication protocols and data models for substation automation.

The following diagram illustrates a simplified view of how MUs, IEDs, and RTUs interact under IEC 61850 communication architecture:

![](img/s_17.png)

At the field level, **Metering Units (MU)** act as data acquisition interfaces that convert analog signals (such as voltage and current from electrical sensors) into digital Sampled Values (SV). These SV messages are transmitted over the **IEC 61850 Process Bus** to the IEDs.

**Intelligent Electronic Devices (IEDs)** receive these digital measurements and equipment status signals from MUs and other sources. They analyze this data to make local control decisions—such as opening circuit breakers during faults or adjusting transformer tap changers to stabilize voltage levels. Communication between IEDs occurs over the **IEC 61850 Station Bus**, using high-speed protocols like GOOSE (Generic Object-Oriented Substation Event) and MMS (Manufacturing Message Specification).

At the system integration layer, the **RTU** serves as a gateway between the IED network and upper-level OT systems like SCADA, HMI, or PLCs. It aggregates data, supports complex control logic, and facilitates long-distance communication with control centers for remote monitoring and control.

This project aims to walk through the design and simulation of a digital twin system for this architecture, covering:

- **Physical device background knowledge**: Understanding the roles and functions of MU, IED, and RTU in real-world deployments.

- **RTU internal logic design**: Developing programmable logic (e.g., using virtual PLCs) to manage breaker operations and automation.

- **SCADA HMI integration**: Implementing SCADA-based visualization and control interfaces for grid state management.

- **Exception and alert handling**: Simulating fault scenarios and implementing recovery mechanisms like automated breaker resets.

- **Digital equivalent system simulation**: Building a fully functional simulation of the control sequence and data flow in a digital substation.

To support this implementation, two open-source subprojects are used:

- [**Python Virtual PLC & RTU Simulator**](https://github.com/LiuYuancheng/Power_Grid_Simulation_System): A tool for emulating PLC/RTU logic and behavior.
- [**Power Grid Simulation System**](https://github.com/LiuYuancheng/PLC_and_RTU_Simulator): A platform that simulates power grid devices and integrates SCADA HMI functionality for testing and visualization.

------

### Background Knowledge of Physical Devices

This section provides a foundational understanding of the core physical devices used in the power grid monitoring system: **Metering Units (MU)**, **Intelligent Electronic Devices (IED)**, and **Remote Terminal Units (RTU)**. It also highlights the differences between RTUs and Programmable Logic Controllers (PLC), which are often used together in OT systems.

![](img/s_19.png)

#### What is a Metering Unit (MU)?

A **power grid metering unit (MU)** is a critical device in electrical systems designed to measure, record, and communicate real-time electrical parameters such as voltage, current, power, frequency, and energy consumption across a power grid. Functioning as the "sensory layer" of the grid, MUs use high-precision sensors (e.g., current transformers, voltage transformers) to capture raw analog signals from transmission lines, substations, or distributed energy resources. These signals are then digitized and processed into standardized data formats (e.g., IEC 61850-9-2 Sampled Values) for compatibility with downstream systems.

#### What is an Intelligent Electronic Device (IED)?

An **Intelligent Electronic Device (IED)** is a microprocessor-based component in power grid systems that performs critical protection, control, and monitoring functions. IEDs receive real-time data from sensors, such as metering units (MUs), and process it to execute automated decisions—like tripping circuit breakers during faults, adjusting voltage levels, or isolating grid sections. They integrate communication protocols (e.g., IEC 61850) to exchange data with Remote Terminal Units (RTUs), SCADA systems, or other IEDs, enabling coordinated grid operations. Modern IEDs also support advanced analytics for fault detection, load management, and predictive maintenance. In power grid simulations, IED logic is digitally replicated to validate control designs, ensuring seamless interaction with PLC-based breaker systems and enhancing grid reliability in dynamic scenarios.

#### What is a Remote Terminal Unit (RTU)?

A **Remote Terminal Unit (RTU)** is a ruggedized hardware device deployed in power grid systems to collect, process, and transmit data from field equipment (e.g., circuit breakers, sensors, IED, metering units) to centralized control systems like SCADA. Acting as a communication gateway, RTUs digitize analog signals, monitor equipment status (e.g., breaker positions), and execute predefined control commands (e.g., remote switching). They use industrial protocols like IEC 60870-5-101/104 or DNP3 to ensure reliable data exchange with control centers, even in harsh environments. In modern grids, RTUs often integrate with **Intelligent Electronic Devices (IEDs)** and cloud platforms, enabling real-time visibility and automated responses for grid stability. In digital twin simulations, RTU behavior is emulated to validate network latency, protocol compliance, and failover mechanisms, ensuring seamless integration with PLC-based control systems and grid-wide automation workflows.

#### Difference between RTU and PLC

A **Remote Terminal Unit (RTU)** and a **Programmable Logic Controller (PLC)** are both critical to industrial automation but serve distinct roles. An RTU specializes in *remote monitoring and control* across geographically dispersed systems, such as power grids, by collecting data from field devices (e.g., sensors, breakers) and transmitting it to centralized SCADA systems using protocols like DNP3 or IEC 60870-5. RTUs prioritize ruggedness, long-range communication, and low power consumption for harsh or isolated environments. In contrast, a PLC focuses on *localized real-time control* of machinery or processes (e.g., assembly lines, breaker logic) using ladder logic programming. PLCs excel in high-speed, deterministic operations and interface directly with sensors/actuators via industrial protocols like Modbus or PROFINET. While modern systems blur these lines—with hybrid devices integrating RTU-PLC functionalities—the core distinction remains: RTUs bridge *data communication* between remote sites and control centers, whereas PLCs execute *automated control logic* at the edge. In power grid simulations, RTUs emulate wide-area data aggregation, while PLCs model localized breaker control algorithms, highlighting their complementary roles in grid automation.

> Reference: https://galooli.com/glossary/what-is-rtu/



------

### RTU Internal Logic Design

In our digital twin power grid simulation, the Remote Terminal Unit (RTU) plays a central role in data aggregation, validation, and anomaly detection. It integrates real-time input from Metering Units (MU), Intelligent Electronic Devices (IED), and Programmable Logic Controllers (PLC) to simulate the behavior and logic of a real-world substation RTU.

The RTU is programmed to monitor the electrical flow between power sources (e.g., **generators**, **transformers**) and power consumers (e.g., **breakers**, **next-level devices**) by verifying signal consistency and detecting operational faults. As illustrated in the system diagram shown below, each power source and load point is monitored by a corresponding **MU-IED pair**, and the RTU orchestrates data from multiple sources for high-level decision making.

![](img/s_20.png)

**RTU Workflow and Logic Implementation:**

- Metering Units (MU) collect voltage (V) and current (A) readings at both the output of power sources and the input of power loads.
- IEDs receive the MU data and compare it against preset safety thresholds. If voltage or current readings are outside safe ranges, IEDs raise local alerts.
- IEDs then calculate power flow values (e.g., apparent power S, real power P, energy imbalance) and send both raw sensor values and calculated data to the RTU via S7Comm protocol.
- The PLC monitors circuit breaker states (ON/OFF) and sends this digital signal to the RTU via Modbus-TCP.

**RTU Safety Logic Rules Example:**

The RTU internal logic will detect and evaluate the current system working state, below is an example rule. 

- If the breaker is ON:
  - If `V1 - V2` ≥ *voltage drop threshold* → Raise `Transmission Line Voltage Drop Alert`
  - If `A1 - A2` ≥ *current drop threshold* → Raise `Power Leakage Alert`
- If the breaker is OFF:
  - If `V2 > 0` or `A2 > 0` → Raise `Breaker Short-Circuit Alert`
  - If `A1 > 0` → Raise `Input Link Power Leakage Alert`

This safety logic is replicated across all MU-IED pairs (MU01–MU04), enabling end-to-end monitoring from generator output to next-level device input. If any error or abnormality occurs in the simulated grid, the system can immediately pinpoint the fault location based on the alerting MU and IED.

**Communication and Integration**

The RTU seamlessly integrates:

- Sensor and breaker data via Modbus-TCP from the PLC,
- Electrical measurements and calculations via S7Comm from IEDs,
- Radio communication channel to forward processed data to the SCADA-HMI system for remote supervision and visualization.



------

### Digital Equivalent System Simulation

In our power grid simulation system, we simulate the power grid data flow as shown below:

![](img/s_21.png)

The data will generated by the physical world simulation program's sensors, then send to the physical world simulators' MU and covert to digital value, then the value will send to IED program to do the 1st verification and calculation, then IED will send the data to RTU via S7Comm, the RTU will to further data process and send all the power grid data SCADA data receiver via Manufacturing Message Specification (MMS). After that the data will be visualized on the power grid Operation room's HMI. 

In the current version, A total of 20 Measurement Units (MUs), 8 IED and 2 RTU are integrated within the system, spanning the power generation, transmission, and distribution sections of the grid. Each unit is linked to specific components to provide comprehensive monitoring:

**Power Generation System:**

| Idx  | MU Set ID              | Sensor Num | Connected Components            | Metering Data                                    |
| ---- | ---------------------- | ---------- | ------------------------------- | ------------------------------------------------ |
| 1    | Solar Farm MU          | 3          | Solar Panel                     | Work State, Voltage, Current                     |
| 2    | Solar Storage MU       | 2          | Solar Power Storage Battery     | Battery Charge/Release, Battery Percentage       |
| 3    | Transformer-01-MU      | 3          | Solar Step-Up Transformer       | Work State, Voltage, Current                     |
| 4    | Wind Farm MU           | 4          | Wind Turbine                    | Work State, Turbine Blade RPM, Voltage, Current  |
| 5    | Transformer-02-MU      | 3          | Wind Step-Up Transformer        | Work State, Voltage, Current                     |
| 6    | Motor-01-MU            | 3          | Generator-01 Driven Motor       | Work State, Throttle Percentage, RPM             |
| 7    | Gen-01-MU              | 4          | Generator 01                    | Work State, RPM, Voltage, Current                |
| 8    | Gen-02-MU              | 4          | Generator 02                    | Work State, RPM, Voltage, Current                |
| 9    | Backup Gen-03-MU       | 4          | Backup Generator                | Work State, RPM, Voltage, Current                |
| 10   | Transformer-03-MU      | 3          | Power Plant Step-Up Transformer | Work State, Voltage, Current                     |
| 11   | Power Plant Storage MU | 2          | Power Plant Storage             | Storage Power Charge/Release, Storage Percentage |

**Power Transmission System:**

| Idx  | MU Set ID             | Sensor Num | Connected Components     | Metering Data                                                |
| ---- | --------------------- | ---------- | ------------------------ | ------------------------------------------------------------ |
| 12   | Substation MU         | 8          | Power Substation         | Work State, Input Bus Voltage/Current, Output Transmission Voltage/Current, Power Storage Voltage/Current |
| 13   | Substation Storage MU | 2          | Substation Power Storage | Storage Power Charge/Release, Storage Percentage             |
| 14   | Transmission MU       | 2          | Transmission Line        | High Voltage Transmission Voltage, Current                   |

**Power Distribution System:**

| Idx  | MU Set ID           | Sensor Num | Connected Components             | Metering Data                |
| ---- | ------------------- | ---------- | -------------------------------- | ---------------------------- |
| 15   | Lvl0-Transformer-MU | 3          | Level 0 Step-Down Transformer    | Work State, Voltage, Current |
| 16   | Station-Cus-MU      | 3          | Station Power Customer (Railway) | Work State, Voltage, Current |
| 17   | Lvl1-Transformer-MU | 3          | Level 1 Step-Down Transformer    | Work State, Voltage, Current |
| 18   | Primary-Cus-MU      | 3          | Primary Power Customer (Factory) | Work State, Voltage, Current |
| 19   | Lvl2-Transformer-MU | 3          | Level 2 Step-Down Transformer    | Work State, Voltage, Current |
| 20   | Secondary-Cus-MU    | 3          | Secondary Power Customer (Home)  | Work State, Voltage, Current |