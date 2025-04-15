# Power_Grid_Simulation_System [ System Setup ]

This document shows the detailed steps to setup the power grid system in a subnet environment or in a local machine. This document include three main sections : 

- **Environment Configuration** : includes the libraries and tools installation, environment parameters configuration. 
- **Program Source File List** : Multiple lists of all the programs with a short introduction of the source code modules' function.
- **System Setup Example** : Use an example to show how to setup each components of the system in a network.

![](../img/logo_small.png)

```python
# Author:      Yuancheng Liu
# Created:     2025/03/11
# Version:     v_0.2.0
# DocNum:      Wiki_3_1
```

**Table of Contents**

[TOC]

------

### Environment Configuration

**Development/Execution Environment** 

- python 3.7.4+

**Additional Lib/Software Need** 

| Lib Module         | Version | Installation                 | Lib link                                 |
| ------------------ | ------- | ---------------------------- | ---------------------------------------- |
| **pyModbusTCP**    | 0.3.0   | `pip install pyModbusTCP`    | https://pypi.org/project/pyModbusTCP/    |
| **python_snap7**   | 1.3     | `pip install python-snap7`   | https://pypi.org/project/python-snap7/   |
| **wxPython**       | 4.1.0   | `pip install wxPython`       | https://pypi.org/project/wxPython/       |
| **python-weather** | 2.0.7   | `pip install python-weather` | https://pypi.org/project/python-weather/ |

If you install the wxPython on Windows-OS platform, please also install the Microsoft C++ Build Tools from this link: https://visualstudio.microsoft.com/visual-cpp-build-tools/

If you use Ubuntu system, to install snap 7 please follow below steps:

```bash
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:gijzelaar/snap7
$ sudo apt-get update
$ sudo apt-get install libsnap7-1 libsnap7-dev
$ pip install python-snap7==1.3
```

> Reference: https://github.com/LiuYuancheng/PLC_and_RTU_Simulator/issues/2#issuecomment-2244070501



------

### Program Source List

All the program source code are under the project `src` folder. Each module source code are under the related folder. The module double click execution program all under the src folder.

| Index | Program File                | Execution Env | Module Description                                           |
| ----- | --------------------------- | ------------- | ------------------------------------------------------------ |
| 1     | `requirements.txt`          | ANY           | Request python lib installation file.                        |
| 2     | `runHMI_PowerGrid_win.bat`  | WIN-OS        | SCADA HMI Program auto execution batch script.(double click) |
| 3     | `runLINK_PowerGrid_win.bat` | WIN-OS        | Power distribution program auto execution batch script.(double click) |
| 4     | `runPLC_PowerGrid_win.bat`  | WIN-OS        | PLC simulation program auto execution batch script.(double click) |
| 5     | `runPW_PowerGrid_win.bat`   | WIN-OS        | Physical World Emulator Program auto execution batch script.(double click) |
| 6     | `runRTU_PowerGrid_win.bat`  | WIN-OS        | RTU Simulation Program auto execution batch script.(double click) |
| 7     | `runWea_PowerGrid_win.bat`  | WIN-OS        | Weather State Fetcher Program auto execution batch script.(double click) |
|       |                             |               |                                                              |



#### Program Library Source

Directory : `src/lib` , [total 8 files]

| Index | Program File      | Execution Env | Module Description                                           |
| ----- | ----------------- | ------------- | ------------------------------------------------------------ |
| 1     | `ConfigLoader.py` | python 3      | Configuration files (xxxCfg.txt or xxxCfg.json) loading module. |
| 2     | `Log.py`          | python 3      | Program execution state real time (info, warning, error, exception) logger module. |
| 3     | `modbusTcpCom.py` | python 3      | Modbus-TCP data communication handling module.               |
| 4     | `plcSimulator.py` | python 3      | Virtual PLC simulator parent interface module.               |
| 5     | `rtuSimulator.py` | python 3      | Virtual RTU simulator parent interface module.               |
| 6     | `snap7.dll`       | WIN-OS        | S7comm Win-32 dll file.                                      |
| 7     | `snap7Comm.py`    | python 3      | Siemens S7Comm data communication handling module.           |
| 8     | `udpCom.py`       | python 3      | UDP data communication handling module.                      |
|       |                   |               |                                                              |



#### Physical World Emulator Program

Directory : `src/PhysicalWorldEmu`, [total 11 files]

| Index | Program File                     | Execution Env | Module Description                                           |
| ----- | -------------------------------- | ------------- | ------------------------------------------------------------ |
| 1     | `img/*`                          |               | All the graphic user interface images, icon files used in the UI. |
| 2     | `itemStateCfg_template.json`     | JSON          | The UI components power state init configuration file template. |
| 3     | `powerGridAgent.py`              | python 3      | The physical power device components (such as generator) object agent module. |
| 4     | `powerGridPWConfig_template.txt` |               | Template of the main configuration file for the power grid physical world simulator. |
| 5     | `powerGridPWDataMgr.py`          | python 3      | Data management module for data communication with other lvl2 OT controllers. |
| 6     | `powerGridPWGlobal.py`           | python 3      | Global parameter init module.                                |
| 7     | `powerGridPWMapMgr.py`           | python 3      | UI Management module to control all the UI components' state. |
| 8     | `powerGridPWMap.py`              | python 3      | UI Main power grid physical world animation display panel.   |
| 9     | `powerGridPWPanel.py`            | python 3      | UI Function panel module for user to control the program.    |
| 10    | `PowerGridPWRun.py`              | python 3      | Main execution file of the physical world emulator.          |
| 11    | `runWinApp.bat`                  | WIN-OS        | Windows auto execution batch script.(double click)           |
|       |                                  |               |                                                              |



#### PLC Simulation Program

Directory : `src/plcCtrl`, [total 4 files]

| Index | Program File             | Execution Env | Module Description                                     |
| ----- | ------------------------ | ------------- | ------------------------------------------------------ |
| 1     | `plcConfig_template.txt` |               | Configuration file of PLC simulation program.          |
| 2     | `plcSimGlobalPwr.py`     | python3       | PLC global parameter init module.                      |
| 3     | `plcSimulatorPwr.py`     | python3       | Virtual PLC simulation program main execution file.    |
| 4     | `runWinApp.bat`          | WIN-OS        | PLC windows auto execution batch script.(double click) |
|       |                          |               |                                                        |



#### RTU Simulation Program

Directory : `src/rtuCtrl`, [total 5 files]

| Index | Program File             | Execution Env | Module Description                                     |
| ----- | ------------------------ | ------------- | ------------------------------------------------------ |
| 1     | `rtuConfig_template.txt` |               | Configuration file of RTU simulation program.          |
| 2     | `rtuSimGlobalPower.py`   | python3       | RTU global parameter init module.                      |
| 3     | `rtuSimulatorPower.py`   | python3       | Virtual RTU simulation program main execution file.    |
| 4     | `snap7.dll`              | WIN-OS        | Main S7Comm dll used by WIN-OS.                        |
| 5     | `runWinApp.bat`          | WIN-OS        | RTU windows auto execution batch script.(double click) |
|       |                          |               |                                                        |



#### Power Distribution Link Program 

Directory : `src/powerlink`, [total 5 files]

| Index | Program File                    | Execution Env | Module Description                                           |
| ----- | ------------------------------- | ------------- | ------------------------------------------------------------ |
| 1     | `powerLinkConfig_template.json` | JSON          | Configuration file of power distribution link.               |
| 2     | `ConfigLoader.py`               | python3       | Configuration files (xxxCfg.txt or xxxCfg.json) loading module. |
| 3     | `udpCom.py`                     | python 3      | UDP data communication handling module.                      |
| 4     | `PowerLinkRun.py`               | python 3      | Main power distribution link program.                        |
| 5     | `runWinApp.bat`                 | WIN-OS        | Power link windows auto execution batch script.(double click) |
|       |                                 |               |                                                              |



#### SCADA HMI Program

Directory : `src/ScadaHMI`, [total 10 files]

| Index | Program File                  | Execution Env | Module Description                                           |
| ----- | ----------------------------- | ------------- | ------------------------------------------------------------ |
| 1     | `hmiMapMgr.py`                | python3       | SCADA UI display parameters value management module.         |
| 2     | `hmiPanel.py`                 | python3       | UI Function panel module for user to control the system.     |
| 3     | `hmiPanelMap.py`              | python3       | HMI circuit diagram display panel.                           |
| 4     | `scadaDataMgr.py`             | python3       | Data management module for data communication with other lvl2 OT controllers. |
| 5     | `scadaGlobal.py`              | python3       | SCADA HMI global parameter init module.                      |
| 6     | `img/*`                       |               | All the graphic user interface images, icon files used in the UI. |
| 7     | `scadaHMIConfig_template.txt` | python3       | Configuration file template of SCADA HMI program.            |
| 8     | `snap7.dll`                   | WIN-OS        | Main S7Comm dll used by WIN-OS.                              |
| 9     | `ScadaHMIRun.py`              | python3       | Main execution file of the SCADA HMI program.                |
| 10    | `runWinApp.bat`               | WIN-OS        | SCADA HMI Program windows auto execution batch script.(double click) |
|       |                               |               |                                                              |



#### Weather State Fetcher Program

Directory : `src/weatherFetcher`, [total 3 files]

| Index | Program File        | Execution Env | Module Description                                           |
| ----- | ------------------- | ------------- | ------------------------------------------------------------ |
| 1     | `weatherFetcher.py` | python3       | Main execution file of the weather state fetch program.      |
| 2     | `udpCom.py`         | python3       | UDP data communication handling module.                      |
| 3     | `runWinApp.bat`     | WIN-OS        | Weather state fetcher windows auto execution batch script.(double click) |
|       |                     |               |                                                              |



------

### System Deployment Example

This section will introduce 2 kinds of system setup steps:

- **All in One Deployment** : Setup the whole system in one local computer. 
- **Normal Deployment**: Setup the system in a subnetwork with one router, once switch and seven nodes(vm/server/computer).

#### All in One Deployment on Local Computer

For simple test and run on one local machine please copy the source code folder `src` to your machine and follow below steps (It will be good if you also follow the execution sequence):

##### Run the physical world simulator 

Rename the power state file `itemStateCfg_template.json` to `itemStateCfg.json` , then execute the program 

For Linux System run below cmds:

```bash
cd src/PhysicalWorldEmu
mv powerGridPWConfig_template.txt powerGridPWConfig.txt
sudo python3 PowerGridPWRun.py
```

For Windows OS system: Double click the `src/runPW_PowerGrid_win.bat`

##### Run the weather information fetcher

Set the city string in the config file.

For Linux System run below cmds:

```bash
cd src/weatherFetcher
mv weatherConfig_template.txt weatherConfig.txt
sudo python3 weatherFetcher.py
```

For Windows OS system: After change the config file name, double click the `src/runWea_PowerGrid_win.bat`

##### Run the power link program

For Linux System run below cmds:

```bash
cd src/powerlink
mv powerLinkConfig_template.json powerLinkConfig.json
python PowerLinkRun.py
```

For Windows OS system:  After change the config file name, double click the `src/runLINK_PowerGrid_win.bat`

##### Run the PLC simulator

For Linux System run below cmds:

```bash
cd src/plcCtrl
mv plcConfig_template.txt plcConfig.txt
sudo python3 plcSimulatorPwr.py
```

For Windows OS system: After change the config file name, double click the `src/runPLC_PowerGrid_win.bat`

##### Run the RTU simulator

For Linux System run below cmds:

```bash
cd src/rtuCtrl
mv rtuConfig_template.txt rtuConfig.txt
sudo python3 rtuSimulatorPower.py
```

For Windows OS system: After change the config file name, double click the `src/runRTU_PowerGrid_win.bat`

**Run the Scada HMI simulator**

For Linux System run below cmds:

```bash
cd src/ScadaHMI
mv scadaHMIConfig_template.txt scadaHMIConfig.txt
sudo python3 ScadaHMIRun.py
```

For Windows OS system: After change the config file name, double click the `src/runHMI_PowerGrid_win.bat`



#### Normal Deployment in Network

To deploy the system in a network under below network topology, please refer to the detailed deployment manual [2_Systen_Deployment_Manual.md](2_Systen_Deployment_Manual.md)

![](img/s_03.png)



------

> last edit by Liu Yuancheng (liu_yuan_cheng@hotmail.com) by 09/04/2025 if you have any question, please send me a message. 
