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
- The servies that a transport protocol can provide are often contrained by the service model of the underlying network-layer protocol.
  - If the network-layer protocol cannot provide delay or bandwidth guarantees for transport-layer segments sent between hosts, then the transport-layer protocol cannot provide delay or bandwidth guarantees for application messages sent between processes.
- However, certain services can be offered by a transport protocol even when the unerlying network protocol doesn't offer the corresponding service at the network-layer.
  - For exapmle, a transport protocol can offer reliable data transfer service to an application even when the underlaying network protocol is unreliable.

### 1.2. Overview of the Transport Layer in the Internet

- 2 distinct transport-layer protocols:
  - UDP (User Datagram Protocol): unreliable, connectionless service.
  - TCP (Transmission Control Protocol): reliable, connection-oriented service.
- Why there are 2 kind of services?
  - Various requirement about services from applications.
  - Applications that need 100% reliable data transfer, e.g. FPT, Mail... -> use TCP as transport services.
  - Application that need fast data transfer but can tolerate with packet lost, e.g. VoIP, Video Streaming... -> use UDP as transport services.
  
- Example:
  
  Application | Application layer protocol | Userlying transport protocol
  --- | --- | ---
  Email | SMTP | TCP
  Remote terminal access | Telnet | TCP
  Web | HTTP | TCP
  File transfer | FTP | TCP
  Streaming multimedia | Proprietary (e.g. RealNetworks) | UDP or TCP
  Internet telephony | Proprietary (e.g. Vonage, Diapad) | UDP

## 2. Multiplexing and Demultiplexing

- Transport-layer multiplexing and demultiplexing is extending the host-to-host delivery service provided by the network layer to a process-to-process delivery service for applications running on the hosts.
- Muliplexing: gathering data chunks at the source host from different sockets, encapsulating each data chink with header information to create segments, and passing the segments to the network layer.
- Demultiplexing: examines fields in header information of the segments to identify the receiving socket and then directs the segment to that socket.

  ![2.png](img/2.png)

- Transport-layer multiplexing requires that sockets have unique identifiers, and that each segment have special fields that indicate the socket to which the segment is to be delivered.
  
  ![3.png](img/3.png)

  - Port numbers:
    - 16-bit number, ranging from 0 - 65535.
    - The port numbers ranging from 0 - 1023 are called well-known port numbers and are restricted -> they are reserved for use by well-known application protocols such as HTTP (port 80), FTP (port 21)...
    - When develop a new application, a port number must be assigned to the application.
- Each socket in the host could be assigned a port number, and when a segment arrives at the host, the transport layer examines the destination port number in the segment and directs the segment to the corresponding socket.