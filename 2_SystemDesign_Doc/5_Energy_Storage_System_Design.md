# Design of Energy Storage System In Power Grid OT Cyber Range

**Project Design Purpose** : This Article will introduce the battery based energy storage system (BESS) we design in the OT Power Grid Simulation Cyber Physical environment I developed. This simulated grid large-scale energy storage systems help balance supply and demand by storing excess electricity from the renewables energy generators (solar and wind) releasing it when needed. They further provide essential grid services , such as helping to remain the power supply or restart the grid after or during  a power outage. We will introduce four main section of the system : 

1. Physical Battery based energy storage system simulation: we will introduce how we implement three 40KWh mid size battery based energy storage station integrated with other system in the physical world simulator of the power grid cyber range.
2. Electrical device connection and energy flow : We will show the electrical devices we simulated in the energy storage system, the battery/energy station data we collected and the power flow control between the BESS and the power grid. 
3. OT Controller design: Show how I use Metering unit and PLC to mimic the OT controller layer of energy storage station's battery control and monitoring system to simulate the individual energy storage station's auto management logic. 
4. Centralize control of the power grid HMI : Show how we design the the power grid operation center's human machine interface to link to the SCADA network to monitor and management all the energy storage stations

Important: The Battery energy storage system in cyber range  will distill (**NOT** 1:1 emulate) a few OT processes from the real world and use digital constructs to represent them for the cyber exercise and education usage. In the real world the Battery energy storage systems (BESS) is much more complex. 

![](5_Energy_Storage_System_Design_Img/logo_small.png)

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



















------

### Reference

- https://en.wikipedia.org/wiki/Grid_energy_storage
- https://www.gminsights.com/industry-analysis/energy-storage-systems-market?gad_source=1&gad_campaignid=21841937168&gbraid=0AAAAACuPGhW9A41yu679JCuOFyIOTxLRx&gclid=CjwKCAjwisnGBhAXEiwA0zEOR8s86vp5s4b601uN1anye_K9fQlpZlEQtxNF8SO68G8cOFxTQ5PdthoC8eQQAvD_BwE
- https://www.volvopenta.com/industrial/battery-energy-storage/?utm_source=google&utm_medium=cpc&utm_campaign=SI_search-BESS_phrase&utm_content=&gad_source=1&gad_campaignid=21901328062&gbraid=0AAAAAozWkbnoHn2eJuiAvLdXR6sbNBpPl&gclid=CjwKCAjwisnGBhAXEiwA0zEORzkOMW7mcfFOm7rkM_-W0W2QGQlL_xZUvZFt8RrahJO9xDbDRgVnWhoCnwwQAvD_BwE