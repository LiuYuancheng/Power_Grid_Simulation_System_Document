# Power_Grid_Simulation_System [ Network Packet Design ]

This document will introduce the OT network protocol communication packets structure and detail between each node (PCLs, RTU, and HMI ) in the Power Grid OT System Cyber Security Test Platform. All the packet example are captured by Wireshark. The normal Modbus-TCP handshake, S7Comm handshake, login/out or ACK messages are not included in this document. 

![](../img/logo_small.png)

```python
# Author:      Yuancheng Liu
# Created:     2025/02/15
# Version:     v_0.2.0
# DocNum:      Wiki_2_3
```

**Table of Contents**



------

### Power Grid Control PLC to HMI [Register Data]

- **Description**: PLC set 05  (PLC00, PLC01, PLC02) reply power grid 21 holding registers data (all the power grid circuit breaker's state sensor ) to the power grid control HMI.
- **Src-Node:** rail-ED-plc05-rail `10.107.[ team_nr ]08.10`
- **Dest-Node**:  rail-op-pg-rail `10.107.[ team_nr ]21.8` 
- **Protocol**: Modbus-TCP (port 502)

Protocol Packet Example:

```
Modbus
    .000 0011 = Function Code: Read Holding Registers (3)
    [Request Frame: 44]
    [Time from request: 0.000196000 seconds]
    Byte Count: 42
    Register 0 (UINT16): 1
        Register Number: 0
        Register Value (UINT16): 1
    Register 1 (UINT16): 1
        Register Number: 1
        Register Value (UINT16): 1
    Register 2 (UINT16): 1
        Register Number: 2
        Register Value (UINT16): 1
    Register 3 (UINT16): 1
        Register Number: 3
        Register Value (UINT16): 1
    Register 4 (UINT16): 0
        Register Number: 4
        Register Value (UINT16): 0
    Register 5 (UINT16): 1
        Register Number: 5
        Register Value (UINT16): 1
    Register 6 (UINT16): 1
        Register Number: 6
        Register Value (UINT16): 1
    Register 7 (UINT16): 1
        Register Number: 7
        Register Value (UINT16): 1
    Register 8 (UINT16): 1
        Register Number: 8
        Register Value (UINT16): 1
    Register 9 (UINT16): 1
        Register Number: 9
        Register Value (UINT16): 1
    Register 10 (UINT16): 1
        Register Number: 10
        Register Value (UINT16): 1
    Register 11 (UINT16): 1
        Register Number: 11
        Register Value (UINT16): 1
    Register 12 (UINT16): 1
        Register Number: 12
        Register Value (UINT16): 1
    Register 13 (UINT16): 1
        Register Number: 13
        Register Value (UINT16): 1
    Register 14 (UINT16): 1
        Register Number: 14
        Register Value (UINT16): 1
    Register 15 (UINT16): 1
        Register Number: 15
        Register Value (UINT16): 1
    Register 16 (UINT16): 1
        Register Number: 16
        Register Value (UINT16): 1
    Register 17 (UINT16): 1
        Register Number: 17
        Register Value (UINT16): 1
    Register 18 (UINT16): 1
        Register Number: 18
        Register Value (UINT16): 1
    Register 19 (UINT16): 0
        Register Number: 19
        Register Value (UINT16): 0
    Register 20 (UINT16): 1
        Register Number: 20
        Register Value (UINT16): 1
```

Table map packet information to the physical world and HMI simulation components:

| Physical World Item Sensor ID                   | PLC00               | PLC01                | PLC02                | HMI PLC tag | HMI Item ID |
| ----------------------------------------------- | ------------------- | -------------------- | -------------------- | ----------- | ----------- |
| Solar Panel Gen breaker sensor                  | Holding Registers 0 | —————                | —————                | gs00        | GenSW-4     |
| Wind Turbines Gen breaker sensor                | Holding Registers 1 | —————                | —————                | gs01        | GenSW-5     |
| Solar Step-up Transformer sensor                | Holding Registers 2 | —————                | —————                | gs02        | TransUp-2   |
| Wind Step-up Transformer sensor                 | Holding Registers 3 | —————                | —————                | gs03        | TransUp-3   |
| Power Plant Transformer sensor                  | Holding Registers 4 | —————                | —————                | gs04        | TransUp-1   |
| Motor 1 on/off sensor                           | Holding Registers 5 | —————                | —————                | gs05        | Motor-1     |
| Motor 2 on/off sensor                           | Holding Registers 6 | —————                | —————                | gs06        | Motor-2     |
| Motor 3 on/off sensor                           | Holding Registers 7 | —————                | —————                | gs07        | Motor-3     |
| Motor 1 to Gen1 switch sensor                   | —————               | Holding Registers 8  | —————                | gs08        | MotorSW-1   |
| Motor 2 to Gen2 switch sensor                   | —————               | Holding Registers 9  | —————                | gs09        | MotorSW-2   |
| Motor 3 to Gen3 switch sensor                   | —————               | Holding Registers 10 | —————                | gs10        | MotorSW-3   |
| Gen1 output switch sensor                       | —————               | Holding Registers 11 | —————                | gs11        | GenSW-1     |
| Gen2 output switch sensor                       | —————               | Holding Registers 12 | —————                | gs12        | GenSW-2     |
| Gen3 output switch sensor                       | —————               | Holding Registers 13 | —————                | gs13        | GenSW-3     |
| High Voltage  Transmission Input switch sensor  | —————               | Holding Registers 14 | —————                | ts14        | TranMSW-I   |
| High Voltage  Transmission Output switch sensor | —————               | Holding Registers 15 | —————                | ts15        | TranMSW-O   |
| Distribution Transformer switch 1 sensor        | —————               | —————                | Holding Registers 16 | ds16        | TransDSW-1  |
| Distribution Transformer switch 2 sensor        | —————               | —————                | Holding Registers 17 | ds17        | TransDSW-2  |
| Distribution Transformer switch 3 sensor        | —————               | —————                | Holding Registers 18 | ds18        | LoadSW-3    |
| Load railway input switch sensor                | —————               | —————                | Holding Registers 19 | ds19        | LoadSW-1    |
| Load factory input switch sensor                | —————               | —————                | Holding Registers 20 | ds20        | LoadSW-2    |



------

### Power Grid Control PLC to HMI [Coil Data]

- **Description**: PLC set 05  (PLC00, PLC01, PLC02) reply power grid 21 output coils data (all the power grid circuit breaker's control closer) to HMI
- **Src-Node:** rail-ED-plc05-rail `10.107.[ team_nr ]08.10`
- **Dest-Node**:  rail-op-pg-rail `10.107.[ team_nr ]21.8` 
- **Protocol**: Modbus-TCP (port 502)

Protocol Packet Example:

```
Modbus
    .000 0001 = Function Code: Read Coils (1)
    [Request Frame: 48]
    [Time from request: 0.000092000 seconds]
    Byte Count: 3
    Bit 0 : 1
        [Bit Number: 0]
        .... ...1 = Bit Value: True
    Bit 1 : 1
        [Bit Number: 1]
        .... ...1 = Bit Value: True
    Bit 2 : 1
        [Bit Number: 2]
        .... ...1 = Bit Value: True
    Bit 3 : 1
        [Bit Number: 3]
        .... ...1 = Bit Value: True
    Bit 4 : 0
        [Bit Number: 4]
        .... ...0 = Bit Value: False
    Bit 5 : 1
        [Bit Number: 5]
        .... ...1 = Bit Value: True
    Bit 6 : 1
        [Bit Number: 6]
        .... ...1 = Bit Value: True
    Bit 7 : 1
        [Bit Number: 7]
        .... ...1 = Bit Value: True
    Bit 8 : 1
        [Bit Number: 8]
        .... ...1 = Bit Value: True
    Bit 9 : 1
        [Bit Number: 9]
        .... ...1 = Bit Value: True
    Bit 10 : 1
        [Bit Number: 10]
        .... ...1 = Bit Value: True
    Bit 11 : 1
        [Bit Number: 11]
        .... ...1 = Bit Value: True
    Bit 12 : 1
        [Bit Number: 12]
        .... ...1 = Bit Value: True
    Bit 13 : 1
        [Bit Number: 13]
        .... ...1 = Bit Value: True
    Bit 14 : 1
        [Bit Number: 14]
        .... ...1 = Bit Value: True
    Bit 15 : 1
        [Bit Number: 15]
        .... ...1 = Bit Value: True
    Bit 16 : 1
        [Bit Number: 16]
        .... ...1 = Bit Value: True
    Bit 17 : 1
        [Bit Number: 17]
        .... ...1 = Bit Value: True
    Bit 18 : 1
        [Bit Number: 18]
        .... ...1 = Bit Value: True
    Bit 19 : 0
        [Bit Number: 19]
        .... ...0 = Bit Value: False
    Bit 20 : 1
        [Bit Number: 20]
        .... ...1 = Bit Value: True
```

Table map packet information to the physical world and HMI simulation components:

| Physical World Item motor ID                    | PLC00       | PLC01        | PLC02        | HMI PLC tag | HMI Item ID |
| ----------------------------------------------- | ----------- | ------------ | ------------ | ----------- | ----------- |
| Solar Panel Gen breaker closer                  | Coil Bits 0 | —————        | —————        | gS00        | GenSW-4     |
| Wind Turbines Gen breaker closer                | Coil Bits 1 | —————        | —————        | gS01        | GenSW-5     |
| Solar Step-up Transformer closer                | Coil Bits 2 | —————        | —————        | gS02        | TransUp-2   |
| Wind Step-up Transformer closer                 | Coil Bits 3 | —————        | —————        | gS03        | TransUp-3   |
| Power Plant Transformer closer                  | Coil Bits 4 | —————        | —————        | gS04        | TransUp-1   |
| Motor 1 on/off closer                           | Coil Bits 5 | —————        | —————        | gS05        | Motor-1     |
| Motor 2 on/off closer                           | Coil Bits 6 | —————        | —————        | gS06        | Motor-2     |
| Motor 3 on/off closer                           | Coil Bits 7 | —————        | —————        | gS07        | Motor-3     |
| Motor 1 to Gen1 switch closer                   | —————       | Coil Bits 8  | —————        | gS08        | MotorSW-1   |
| Motor 2 to Gen2 switch closer                   | —————       | Coil Bits 9  | —————        | gS09        | MotorSW-2   |
| Motor 3 to Gen3 switch closer                   | —————       | Coil Bits 10 | —————        | gS10        | MotorSW-3   |
| Gen1 output switch closer                       | —————       | Coil Bits 11 | —————        | gS11        | GenSW-1     |
| Gen2 output switch closer                       | —————       | Coil Bits 12 | —————        | gS12        | GenSW-2     |
| Gen3 output switch closer                       | —————       | Coil Bits 13 | —————        | gS13        | GenSW-3     |
| High Voltage  Transmission Input switch closer  | —————       | Coil Bits 14 | —————        | tS14        | TranMSW-I   |
| High Voltage  Transmission Output switch closer | —————       | Coil Bits 15 | —————        | tS15        | TranMSW-O   |
| Distribution Transformer switch 1 closer        | —————       | —————        | Coil Bits 16 | dS16        | TransDSW-1  |
| Distribution Transformer switch 2 closer        | —————       | —————        | Coil Bits 17 | dS17        | TransDSW-2  |
| Distribution Transformer switch 3 closer        | —————       | —————        | Coil Bits 18 | dS18        | LoadSW-3    |
| Load railway input switch closer                | —————       | —————        | Coil Bits 19 | dS19        | LoadSW-1    |
| Load factory input switch closer                | —————       | —————        | Coil Bits 20 | dS20        | LoadSW-2    |



------

### Power Grid Monitor RTU to HMI [Metering Unit Data]

- **Description**: RTU 02 reply power grid 32 metering unit's data to HMI.
- **Src-Node:** rail-ed-rtu02-rail `10.107.[ team_nr ]08.11`
- **Dest-Node**:  rail-op-pg-rail `10.107.[ team_nr ]21.8` 
- **Protocol**:  Siemens-s7comm (port 102)

Protocol Packet Example:

```
S7 Communication
    Header: (Job)
    Parameter: (Read Var)
        Function: Read Var (0x04)
        Item count: 1
        Item [1]: (DB 1.DBX 0.0 BYTE 8)
            Variable specification: 0x12
            Length of following address specification: 10
            Syntax Id: S7ANY (0x10)
            Transport size: BYTE (2)
            Length: 8
            DB number: 1
            Area: Data blocks (DB) (0x84)
            Address: 0x000000
                .... .000 0000 0000 0000 0... = Byte Address: 0
                .... .... .... .... .... .000 = Bit Address: 0
S7 Communication
    Header: (Ack_Data)
    Parameter: (Read Var)
        Function: Read Var (0x04)
        Item count: 1
    Data
        Item [1]: (Success)
            Return code: Success (0xff)
            Transport size: BYTE/WORD/DWORD (0x04)
            Length: 8
            Data: 0028005000210065
```

Table map RTU Data block[0x84] 's memory address and 'Byte Index with the value and unit:

| Index | Metering Unit On Physical World                     | Unit | Memory Idx | Byte index | Data type |
| ----- | --------------------------------------------------- | ---- | ---------- | ---------- | --------- |
| 1     | Solar panel generator output voltage                | V    | 1          | 0          | int       |
| 2     | Solar panel generator output current                | A    | 1          | 2          | int       |
| 3     | Solar step up transformer1 output voltage           | kV   | 1          | 4          | int       |
| 4     | Solar step up transformer1 output current           | A    | 1          | 6          | int       |
| 5     | Wind Turbine generator output voltage               | kV   | 2          | 0          | int       |
| 6     | Wind Turbine generator output current               | A    | 2          | 2          | int       |
| 7     | Wind step up transformer2 output voltage            | kV   | 2          | 4          | int       |
| 8     | Wind step up transformer2 output current            | A    | 2          | 6          | int       |
| 9     | Gen-Driven motor1-RPM                               | RPM  | 3          | 0          | int       |
| 10    | Generator1 output voltage                           | kV   | 3          | 2          | int       |
| 11    | Generator1 output current                           | A    | 3          | 4          | int       |
| 12    | Power storage 1 battery charging state              | -    | 3          | 6          | bool      |
| 13    | Gen-Driven motor2-RPM                               | RPM  | 4          | 0          | int       |
| 14    | Generator2 output voltage                           | kV   | 4          | 2          | int       |
| 15    | Generator2 output current                           | A    | 4          | 4          | int       |
| 16    | Power storage 2 battery charging state              | -    | 4          | 6          | bool      |
| 17    | Gen-Driven motor3-RPM                               | RPM  | 5          | 0          | int       |
| 18    | Generator 3 (backup) output voltage                 | kV   | 5          | 2          | int       |
| 19    | Generator 3 (backup) output current                 | A    | 5          | 4          | int       |
| 20    | Power storage 3 battery charging state              | -    | 5          | 6          | bool      |
| 21    | Substation to transmission tower output voltage     | kV   | 6          | 0          | int       |
| 22    | Substation to transmission tower output current     | A    | 6          | 2          | int       |
| 23    | Transmission tower to distribution input voltage    | kV   | 6          | 4          | int       |
| 24    | Transmission tower to distribution input current    | A    | 6          | 6          | int       |
| 25    | Step up transformer3 output voltage                 | kV   | 7          | 0          | int       |
| 26    | Step up transformer4 output voltage                 | A    | 7          | 2          | int       |
| 27    | Level 0 step down transformer to railway  voltage   | kV   | 7          | 4          | int       |
| 28    | Level 0 step down transformer to railway  current   | A    | 7          | 6          | int       |
| 29    | Level 1 step down transformer to factor voltage     | kV   | 8          | 0          | int       |
| 30    | Level 1 step down transformer to factor current     | A    | 8          | 2          | int       |
| 31    | Level 2 step down transformer to smart home voltage | V    | 8          | 4          | int       |
| 32    | Level 2 step down transformer to smart home current | A    | 8          | 6          | int       |



------

>last edit by LiuYuancheng (liu_yuan_cheng@hotmail.com) by 15/02/2025 if you have any problem, please send me a message. 