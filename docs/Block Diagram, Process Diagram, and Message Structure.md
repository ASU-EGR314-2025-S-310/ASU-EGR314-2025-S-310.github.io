---
title: Block Diagram, Process Diagram, and Message Structure
---

## Block Diagram

![Block Diagram](https://github.com/ASU-EGR314-2025-S-310/ASU-EGR314-2025-S-310.github.io/blob/main/assets/Team310BlockDiagram.png?raw=true)

## Process Diagram

![Process Diagram](https://github.com/ASU-EGR314-2025-S-310/ASU-EGR314-2025-S-310.github.io/blob/main/assets/SequenceDiagram.png?raw=true)

## Message Types

| Message Type (Byte 1-2) | System Send From & Message Description |
|-------------------------|--------------------------------------|
| 1  | Sensor: Sensor State |
| 2  | Sensor: Sensor Error |
| 3 | Actuator: Error |
| 4 | MQTT: Wifi Connection |
| 5  | MQTT: Master Reset |
| 6  | MQTT: Error|
| 7  | HMI: Send Data |
| 8 | HMI: Error |


### Sensor Messages

All are (uint8_t).

| Message Type | Message Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Receiver ID | Byte 5 <br> Data Type | Byte 6 <br> Data Value| Byte 7 | Byte 8-9 |
|----------|---------------|--------|-----------|--------|--| --| - |
| 1 | Prefix (AZ)| Sensor ID (E)| HMI ID (H)|Sensor State (S) |   Sensor State (0-1) | Sensor Location (1-4) | Suffix (YB) |
| 2 | Prefix (AZ)| Sensor ID (E)| Broadcast ID (X)| Error (F) | Error type (0-5) | Byte 7-8 <br> Suffix (YB) |  |

### Actuator Messages

All are (uint8_t).

|  Message Type   | Byte 1-2 | Byte 3 | Byte 4 | Byte 5 | Byte 6-7 | 
| ------- |----------|--------|-----------|---------| -----| 
| 3 | Prefix(AZ)  | Actuator ID (N) | Broadcast ID (X) | Error Type (0-6) | Suffix (YB) |

### HMI Messages

All are (uint8_t).

| Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Message | Byte 5-57 <br> Message 2  | Byte 58 |
|----------|---------------|--------|-----------|--------|
| 17 | HMI ID | Receiver ID | Send Data <br> -Speed Setting <br> - MQTT Data | Stop| 
| 18 | HMI ID | Receiver ID | Start Communication | Stop |
| 19 | HMI ID | Receiver ID | End Communication | Stop | 
| 20 | HMI ID| Receiver ID  | Error Message | Stop |

### MQTT Board Messages

All are (uint8_t).

| Message Type | Message Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Receiver ID | Byte 5 <br> Data Type | Byte 6 <br> Data Value| Byte 7-8 |
|----------|---------------|--------|-----------|--------|--| --|
| 4 | Prefix (AZ)| MQTT ID (K)| HMI ID (H)| Wifi (W)| Disconnected - 0 <br> Connected - 1 | Suffix (YB) |
| 5 | Prefix (AZ)| MQTT ID (K)| Broadcast ID (X)| Reset (R)| Reset - 1 | Suffix (YB) |
| 6 | Prefix (AZ)| MQTT ID (K)| Broadcast ID (X)| Error (F)| Error Message: 0 - 9 | Suffix (YB) |

## System Message Handling
| Message Type |  Evan <br> Role: Sensor <br> ID: E| Noah<br> Role: Actuator <br> ID: N | Kirk <br> Role: MQTT <br> ID: K | Hunter <br> Role: HMI <br> ID: H|
|----------|---------------|--------|-----------|--------|
| 1 | S <br> (Sensor  State) | - | R <br>( MQTT Topic: EGR314/Team310/Speed)| R <br> (Displays Speed)|
| 2 | S <br> (Error Code) | - | R <br>(Address to MQTT Topic: EGR314/Team310/Error_Address <br> Error Code to MQTT Topic: EGR314/Team310/Error_Code| -||
| 3 | - | S <br> (Error Code)| R <br>(Address to MQTT Topic: EGR314/Team310/Error_Address <br> Error Code to MQTT Topic: EGR314/Team310/Error_Code| -|
| 4 | -| -| S <br> (Wifi Connection State)| R <br> (Displays Wifi Connection)|
| 5 | R <br> (System Resets)| R <br> (System Resets)| S <br> (Master Reset Trigger)| R <br> (System Resets)|
| 6 | -| -| S <br> (MQTT Error Code)<br> R <br>(Address to MQTT Topic: EGR314/Team310/Error_Address <br> Error Code to MQTT Topic: EGR314/Team310/Error_Code | -|
| 7 | -| R <br> (Change Actuator Speed)| -| S <br> (User Controlled Input) |
| 8 | - |-|R <br>(Address to MQTT Topic: EGR314/Team310/Error_Address <br> Error Code to MQTT Topic: EGR314/Team310/Error_Code)|S <br> (Error Code)|

|Item| Meaning|
|--|--|
|S | Sends the message|
|R|Receives the Message and does something|
| - | Does Nothing |
