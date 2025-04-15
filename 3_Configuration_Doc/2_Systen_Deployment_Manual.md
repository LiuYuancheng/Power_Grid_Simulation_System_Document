# Power_Grid_Simulation_System [ System Deployment ]

This document guides green team engineers through deploying the simulation system's modules across various virtual machines (VMs) or physical devices. It also includes an example of integrating the power grid simulation with a power customer system, such as a land-based railway system. This document include two main sections : 

- **Network Environment Introduction** :  A short introduction about the network topology and the vm information (such as OS type, ip address configuration)
- **System Deployment Steps** : Detailed steps to configure the network routing rule and deploy the program on each VMs.

![](../img/logo_small.png)

```python
# Author:      Yuancheng Liu
# Created:     2025/04/14
# Version:     v_0.2.0
# DocNum:      Wiki_3_2
```

**Table of Contents**

[TOC]

------

### Network Environment Introduction

To deploy the Power_Grid_OT_Simulation_System system with one power customer, a minimum of seven virtual machines (VMs) or physical machine are required, along with two isolated networks connected through one network switches. 

- The **Power_Grid_Physical World Network** hosts all physical-world simulation programs, including the power grid physical world simulator, weather data fetcher, power link connector, and power customer modules. This network is exclusively accessible to the green and blue teams.
- The **Power_Grid_System SCADA Network** contains OT supervisory control and data acquisition components, such as one or multiple PLC simulators,  one or multiple RTU simulators, and a human-machine interface (HMI) program. This network can be accessed by the green, blue, and red teams.

The network configuration diagram is shown below:

![](img/s_03.png)

Each PLC and RTU VM is configured with dual network interfaces to enable communication across both networks.

The recommended operating system for the Power Customer VM, Power Grid VM, and SCADA-HMI VMs is **Windows 11**, while **Ubuntu 22.04** is recommended for all other VMs to ensure compatibility with the required modules and libraries.

Deployment should follow the sequence outlined in the table below:

| VM Name          | Deploy Seq | OS Type | Physical World IP | SCADA IP     | Program/Module Needed                |
| ---------------- | ---------- | ------- | ----------------- | ------------ | ------------------------------------ |
| **PW-Railway**   | 1          | Win11   | 10.107.109.7      | N.A          | `lib` , `<Railway>PhysicalWorldSimu` |
| **PW-PowerGrid** | 2          | Win11   | 10.107.109.8      | N.A          | `lib`, `<Powergrid>PhysicalWorldEmu` |
| **PW-PowerLink** | 3          | Ubuntu  | 10.107.109.5      | N.A          | `powerlink`                          |
| **PW-Weather**   | 4          | Ubuntu  | 10.107.109.6      | N.A          | `weatherFetcher`                     |
| **SCADA-PLC**    | 5          | Ubuntu  | 10.107.109.9      | 10.107.108.3 | `lib`, `plcCtrl`                     |
| **SCADA-RTU**    | 6          | Ubuntu  | 10.107.109.10     | 10.107.108.4 | `lib`, `rtuCtrl`                     |
| **SCADA-HMI**    | 7          | Win11   | N.A               | 10.107.108.5 | `lib`, `ScadaHMI`                    |

For details on the required modules, refer to the **Program File List** section in the system setup portion of the **README** file. Each VM must also have the necessary libraries installed according to the setup instructions provided.



------

### System Deployment Steps

This section will use the previous section as an example to deploy the program on each VM in the system.

#### Deploy Power Customer Railway System VM

**Step 1: Set Up the Power Customer Railway System Program Files**

- Follow the program setup instructions in railway system readme file https://github.com/LiuYuancheng/Railway_IT_OT_System_Cyber_Security_Platform/blob/main/README.md to install the required libraries.
- Create a program directory structure: `railway/src` in the machine or VM.
- Copy the `lib` and `RailwayPhysicalWorldSimu` modules into the `src` folder.

**Step 2: Update the Application Configuration File**

- Rename the `src/RailwayPhysicalWorldSimu/configFiles/metroConfig_template.txt` to `src/RailwayPhysicalWorldSimu/configFiles/metroConfig.txt` . 
- Since the railway system acts as the power customer in this deployment, update the configuration file to enable test mode by setting the flag to `True` (line 9). The modified section should look like this:

```
# This is the config file template for the railway system module <RailwayPWSimuRun.py>
# Setup the parameters with below format (every line follow <key>:<value> format, the
# key can not be changed):
#-----------------------------------------------------------------------------
# Config section 0: Basic general parameter config.
# Test mode flag:
# - True: use the physical word internal code to simulate the control logic. 
# - False: connect to PLC and let PLC(s) control all the signals and trains.
TEST_MD:True
...
```

**Step3: Run the power customer railway system program**

- Navigate(cd) to the `physicalWorldSimu` folder and run the railway physical world simulator program with command  `python3 RailwayPWSimuRun.py`.
- Once executed, the railway system UI will appear, as shown below:

![](img/s_04.png)



#### Deploy Power Grid Physical World Simulator VM

**Step1: Setup program files** 

- Follow the program setup instructions in readme file to install the required libraries.
- Create a program directory structure: `power/src` in the machine or VM.
- Copy the `lib` and `PhysicalWorldEmu` modules into the `src` folder.

**Step 2: Update the Application Configuration File**

- Rename the `src/PhysicalWorldEmu/powerGridPWConfig_template.txt` to `src/PhysicalWorldEmu/powerGridPWConfig.txt` 
- Open the configuration file and update it as shown below. Ensure the test mode flag (`TEST_MD`) is set to `False` to enable PLC-controlled signals.

```python
# This is the config file template for the module <PowerGridPWRun.py>
# Setup the parameters with below format (every line follow <key>:<val> format, the
# key can not be changed):
#-----------------------------------------------------------------------------
# Test mode:
# True: use the real word internal logic to simulator the control logic. 
# False: connect to PLC let plc control the signals 
TEST_MD:True
#-----------------------------------------------------------------------------
# Init the dataManager port for PLC to fetch and set data. 
UDP_PORT:3001
# Init Plc connection time out, if plc is not connect in num of seconds, the 
# system will treat it as offline
PLC_TIMEOUT:3
#-----------------------------------------------------------------------------
# define UI title name 
UI_TITLE:2D Power Grid Simulation System Physical World Simulator
# Init the UI update interval:
UI_INTERVAL:0.8
#-----------------------------------------------------------------------------
# Power grid item state config file name for the simulator
STATE_FILE:itemStateCfg.json
# flag to identify if the solar power will generate based on time
SOLAR_TIME_FLG:True
```

**Step3: Run the Power Grid Physical World Simulator**

- Navigate(cd) to the `physicalWorldSimu` folder and run the railway physical world simulator program with command  `python3 PowerGridPWRun.py`, 
- After running the program, the Power Grid system UI will appear, as shown below:

![](img/s_05.png)