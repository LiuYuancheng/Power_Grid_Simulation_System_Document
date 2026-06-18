# Design of a Simulated 5G & Wi-Fi IIoT Thermal State Monitoring System for a Power Grid Cyber Twin

**Project Design Purpose** : This article introduces the design and implementation of a simulated Industrial Internet of Things (IIoT)-based thermal state monitoring system developed as part of the OT Power Grid Cyber Twin environment. The project recreates the core operational concepts of a wireless temperature monitoring solution, including Wi-Fi-IoT temperature sensors, a 5G IoT gateway, and a monitoring and control Human-Machine Interface (HMI).

The system is inspired by the functionality of the Schneider Electric ZBRTT1 wireless temperature monitoring solution and demonstrates how thermal condition monitoring can support power grid operations, equipment health assessment, abnormal condition detection, and fire risk alarm generation in modern electrical infrastructure. The project is organized into five major functional components:

- Real-Time Power Grid Thermal State Generation and Simulation
- Wi-Fi IIoT Power Grid Components Temperature Sensor 
- 5G Power Grid  Thermal IIoT Sensors Gateway Simulation
- System Components Communication and Data Transmission Security Function
- Thermal State Data Visualization and Abnormal Condition Detection



This article introduces the design of a simulated Industry IoT based thermal state monitoring system in the OT Power Grid Cyber Twin I developed. The goal is to use the software simulated the basic function of a Schneider ZBRTT1 wireless(wifi) temperature system, IoT gateway(5G) and the related monitor and control HMI to demonstrate how the thermal state monitoring system supports the operation, abnormal scenario detection and fire alarm raising of a modern power grids. 

The project is structured into four main sections :

- **Real Time Power Grid Thermal State simulation** : Generate different simulated operational temperature data of the component (transformer, high voltage, power transmission cable, connector and battery in BESS) for the physical world simulator of the power grid cyber twin. 
- **WIFI IoT Temperature Sensor Simulator** : Simulate the basic function and component temperature measurement progress of the Schneider ZBRTT1 wireless(wifi) IOT temperature sensor. 
- **5G IoT Gateway Simulator** : simulate the 5G IIoT gateway components which allocated near the IoT sensors (such as in one power station) to collect the data from IOT sensors via WIFI network then transfer the data to the power grid control center via 5G network (long range) 
- **IoT Communication and Data Encryption** : Simulate the IoT data publish and subscribe progress between each component via  Message Queuing Telemetry Transport protocol and the message encryption/decryption (used by ZBRTT1 ) for information security protection. 
- **Data Visualization and  Abnormal Detection**: Simulate the thermal state monitoring HMI (Local or Control Center) for data visualization and the data analysis with the abnormal or alarm detection logic.  

> **Important** : The simulated thermal state monitoring system in cyber twin will distill (**NOT** 1:1 emulate) a few OT processes from the real world and use digital constructs to represent them for the cyber exercise and education usage. The real world power grid's thermal monitor and control system will be much more complex than what I introduced in the article. 

```python
# Author:      Yuancheng Liu
# Created:     2026/06/18
# Version:     v_0.0.1
# Copyright:   Copyright (c) 2026 Liu Yuancheng
# License:     GNU General Public License V3
```

**Table of Contents**





















https://youtu.be/LKCDbCJlYPo?si=8S81zykMe3NsBzSZ

https://www.se.com/au/en/download/document/GDE58746/

