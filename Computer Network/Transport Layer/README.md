# Transport Layer

## 1. Overview

- A transport-layer protocol provides for logical communication between application processes running on different hosts.
- Application processes use the logical communication provided by the transport layer to send messages to each other.
![This is an image](img/1.png)
- Transport-layer protocols are implemented in the end systems but not in network routers.
- On sending side:
  - Receives the application-layer messages from a sending application process.
  - Converts the messages into transport-layer packets (segments). If it is too large, the message is broken into small chunks and added a transport-layer header to each chunk and put into several segments.
  - Passes the segment to the network layer at the sending end system, where the segment is encapsulated within a network-layer packet (a datagram) and sent to the destination.
- On receiving side:
  - The network layer extracts the transport-layer segment from the datagram and passes the segment up to the transport layer.
  - The transport layer process the received segment, making the data in the segment available to the receiving application.

### 1.1. Relationship between transport and network layers

- Whereas a transport-layer protocol provides logical communication between *processes* running on different hosts, a network-layer protocol provides logical communication between *hosts*.
