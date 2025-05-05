---
title: Block + Process Diagram, and Message Structure
---

## Block Diagram

![Block Diagram](https://github.com/ASU-EGR314-2025-S-310/ASU-EGR314-2025-S-310.github.io/blob/main/assets/Team310BlockDiagram.png?raw=true)

For our block diagram, all systems are connected through the required 8-pin ribbon. All systems share power through pin 1, ground through pin 8, and UART through pin 2. The sensor and actuator subsystems share additional pins for direct IO communication, as UART is too slow to effectvely control the magnetic coils through the sensor's PCB. 

## Process Diagram

![Process Diagram](https://github.com/ASU-EGR314-2025-S-310/ASU-EGR314-2025-S-310.github.io/blob/main/assets/SequenceDiagram.png?raw=true)

The HMI receives user input through a button interface. This prompts the HMI to send a message to the actuator, which changes the coil strength to increase or decrease the speed of the ball. The actuator's timing of the coils is coordinated through IO messages sent from the sensor subsystem. The sensor subsystem also sends a calculated speed of the ball to the HMI system and MQTT for display. The MQTT is able to send a broadcast message to each subsystem to cause a soft reset. All subsystems send an error message to the MQTT for error display when a software error occurs. 

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
| 1 | Prefix (AZ)| Sensor ID (E)| HMI ID (H)|Sensor State (V) |   Sensor State (0-1) | Sensor Location (1-4) | Suffix (YB) |
| 2 | Prefix (AZ)| Sensor ID (E)| Broadcast ID (X)| Error (F) | Error type (0-5) | Byte 7-8 <br> Suffix (YB) |  |

### Actuator Messages

All are (uint8_t).

|  Message Type   | Byte 1-2 | Byte 3 | Byte 4 | Byte 5 | Byte 6 | Byte 7-8 | 
| ------- |----------|--------|-----------|---------| ------ | -----| 
| 3 | Prefix(AZ)  | Actuator ID (N) | Broadcast ID (X) | Data Type (F) | Error Type (0-5) | Suffix (YB) |

### HMI Messages

All are (uint8_t).

| Message Type | Message Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Receiver ID | Byte 5 <br> Data Type | Byte 6 <br> Data Value| Byte 7-8 |
|----------|---------------|--------|-----------|--------|--| --|
| 7 | Prefix (AZ)| HMI ID (H)| Actuator ID (N)| Speed (S) | Speed Setting (0-3) | Suffix (YB) |
| 8 | Prefix (AZ)| HMI ID (H)| Broadcast ID (X)| Error (F) | Error Message (0-5) | Suffix (YB) |

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
| 7 | -| R <br> (Change Actuator Speed via LED blinks)| -| S <br> (User Controlled Input) |
| 8 | - |-|R <br>(Address to MQTT Topic: EGR314/Team310/Error_Address <br> Error Code to MQTT Topic: EGR314/Team310/Error_Code)|S <br> (Error Code)|

|Item| Meaning|
|--|--|
|S | Sends the message|
|R|Receives the Message and does something|
| - | Does Nothing |


### Biggest Changes Design

Since Dominick was allowed to join our team near the end of the semester, most of our major changes were related to implementing his subsystem.

1. A new subsystem ID was added to the list of team addresses, designated "D"
2. Error handling was added to the MQTT system to incorporate the new subsystem
3. The HMI subsystem was required to add echoing and readdressing of incoming speed messages from the sensor subsystem.
4. The HMI subsystem incorporated the ability to display messages from the new subsystem to show functionality
5. The MQTT subsystem added functionality to handle a message from the new subsystem to control an LED. 