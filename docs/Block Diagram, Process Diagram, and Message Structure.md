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
| 1  | Sensor: Start Communication |
| 2  | Sensor: Sensor State + Sensor Location |
| 3  | Sensor: Sensor Error |
| 4  | Sensor: Reset |
| 5  | Sensor: End Processes |
| 6  | Sensor: End Com |
| 7  | Sensor:  |
| 8  | Sensor:  |
| 9  | Actuator: Magnetic Switching State |
| 10 | Actuator: Error - Incomplete / Bad Message Received |
| 11 | Actuator: Reset |
| 12 | Actuator: Magnet Activation Time |
| 13 | Actuator:  |
| 14 | Actuator:  |
| 15 | MQTT: Start Communication |
| 16 | MQTT: Send Data |
| 17 | MQTT: Transmission Error |
| 18 | MQTT: End Communication |

### HMI Messages

All are (uint8_t).

| Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Message | Byte 5-57 <br> Message 2  | Byte 58 |
|----------|--------|-----------|---------|
| 16 | HMI ID | Receiver ID | Send Data <br> -Speed Setting <br> - MQTT Data |
| 17 | HMI ID | Receiver ID | Start Communication |
| 18 | HMI ID | Receiver ID | End Communication |
| 22 | HMI ID| Receiver ID  | Error Message |


### Actuator Messages

All are (uint8_t).

| Byte 1-2 | Byte 3 | Byte 4-57 | Byte 58 |
|----------|--------|-----------|---------|
| 9  | System Sent To | On/Off | Stop |
| 10 | System Sent To | Message Type | Stop |
| 11 | System Sent To | Stop Processes | Stop |
| 12 | System Sent To | Magnet Time Activated | Stop |
| 13 | System Sent To |  | Stop |
| 14 | System Sent To |  | Stop |

### Sensor Messages

All are (uint8_t).

| Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Message | Byte 5-57 <br> Message 2  | Byte 58 |
|----------|---------------|--------|-----------|--------|
| 1  | Sender ID | Start Communication | Start Communication | Stop |
| 2  | Sender ID | Sensor Triggered | Sensor Location | Stop |
| 3  | Sender ID | Sensor Error | Sensor Location | Stop |
| 4  | Sender ID | Reset | Reset | Stop |
| 5  | Sender ID | End Processes | End Processes | Stop |
| 6  | Sender ID | End Com | End Com | Stop |

### MQTT Board Messages

All are (uint8_t).

| Byte 1-2 <br> Message Prefix | Byte 3 <br> Sender ID | Byte 4 <br> Receiver ID | Byte 5-57 <br> Message | Byte 58 |
|----------|---------------|--------|-----------|--------|
| 15 | MQTT Board ID | Receiver ID | Establish Communication | Stop |
| 16 | MQTT Board ID | Receiver ID | Data | Stop |
| 17 | MQTT Board ID | Receiver ID | Error Message | Stop |
| 18 | MQTT Board ID | Receiver ID | End Communication | Stop |

### MQTT Client/Broker Messages

| Message Type | Byte 1 <br> Control/Flags | Byte 2 <br> Remaining Length Flag (<127 bytes) <br> *3rd byte used if message > 127 bytes | Bytes 3-4 <br> Variable Header | Bytes 5-58 <br> Payload |
|-------------|--------|--------------|--------|-------------------|
| Connect | (Client > Broker) <br> Hex: 10 | Variable, represents number of bytes remaining in packet | Length, Protocol name, level, connect flags | Client ID, Username, Password |
| Connect Acknowledge  | (Broker > Client) <br> Hex: 20 | Variable, represents number of bytes remaining in packet | Length, Acknowledge flags, connect reason codes, properties | none |
| Publish  | (Broker <> Client) <br> Hex: 3x <br> Bit 3: DUP <br> Bit 2+1: QoS <br> Bit 0: RETAIN|  Variable, represents number of bytes left in packet | Topic name, packet identifier, properties | Control data (velocity, position, acceleration) |
| Subscribe  | (Client > Broker) <br> Hex: 82 | Variable, represents number of bytes left in packet | Packet identifier, properties | Topic filter, subscription options |
| Subscribe Acknowledge  | (Broker > Client) <br> Hex: 90 | Variable, represents number of bytes left in packet | Packet identifier that is being acknowledged, properties | Reason code corresponding to topic filter |
| Disconnect  | (Client > Broker) <br> Hex: E0 | Variable, represents number of bytes left in packet | Disconnect reason code, properties | none |
