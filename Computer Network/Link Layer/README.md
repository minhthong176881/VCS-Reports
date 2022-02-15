# The Link Layer

## 1. Introduction

- Any device that run a link-layer protocol is called a **node**.
- Nodes include hosts, routers, switches, and WiFi access points.
- The communication channels that connect adjacent nodes along the communication path is called **links**.
- A transmitting node encapsulates the datagram in a **link-layer frame** and transmits the frame into the link.

### 1.1. Link Layer services

- Possible services that can be offered by a link-layer protocol include:
  - Framing: A frame consists of a data field, in which the network-layer datagram is inserted, and a number of header fields. The structure of the frame is specified by the link-layer protocol.
  - Link access: a medium access control (MAC) protocol specifies the rules by which a frame is transmitted onto the link.
    - The MAC protocol is simple, the sender can send a frame whenever the link is idle.
    - When multiple nodes share a single broadcast link, the MAC protocol serves to coordinate the frame transmissions of the many nodes.
  - Reliable delivery: guarantees to move each network-layer datagram across the link without error.
    - A link-layer reliable delivery service can be achieved with acknowledgements and retransmissions.
    - A link-layer reliable delivery service is often used for links that are prone to high error rates, such as a wireless link, with the goal of correcting an error locally (on the link where the error occurs) rather than forcing an retransmission.
    - Link-layer reliable delivery is unnecessary for low bit-error links (fiber, coax, twisted-pair copper links) -> many wired link-layer protocols do not provide a reliable delivery service.
  - Error detection and correction: many link-layer protocols provide a mechanism to detect bit errors: transmitting node include error-detection bits in the frame and the receiving node perform an error check.
    - Error detection in the link layer is implemented in hardware.
    - Error correction is similar to error detection, except that the receiver not only detects when bit errors occurred in the frame but also determines exactly where in the frame the errors have occured and then corrects these errors.

### 1.2. Link Layer Implementation

- For the most part, the link layer is implemented in a **network adapter**, sometime known as a **network interface card (NIC)**.

    ![1.png](img/1.png)

- At the heart of the network adapter is the link-layer controller, usually a single, special-purpose chip that implements many of the link-layer services.
- On the sending side, the controller takes a datagram that has been created and stored in host memory by the higher layers, encapsulates the datagram in a link-layer frame, and then transmits the frame into the communication link.
- On the receiving side, the controller receives the entire frame, and extracts the network-layer datagram. If the link layer performs error detection, the sending controller sets the error-detection bits in the frame header and the receiving controller performs error detection.
- A network adapter attched to a host's bus, where it looks much like any other I/O device to the other host component.
- Most of the link layer is implemented in hardware, part of the link layer is implemented in software that runs on the host's CPU.
  - The software components of the link layer implement higher-level link-layer functionality such as assembling link-layer addressing information and activating the controller hardware.
  - On the receiving side, link-layer software responds to controller interrupts, handling error conditions and passing datagram up to the network layer.

    -> The link layer is combination of hardware and software.

## 2. Error-Detection and -Correction techniques

### 2.1. Parity Checks

- The simplest form of error detection is the use of a single **parity bit**.
  
  ![2.png](img/2.png)

  - The sender includes one additional bit and chooses its value such that the total number of 1s in the d + 1 bits is even (or odd if in an odd parity scheme).
  - The receiver count the number of 1s in the received d + 1 bits. If an odd number of 1-valued bits are found with an even partity scheme, at least 1 bit error has occurred.

  -> Only work if the probability of multiple bit errors is extremely small.

- **Two-dimensional parity scheme**: d bits in D is divided into i rows and j columns. A parity value is computed for each row and for each column. The resulting i + j + 1 parity bits comprise the link-layer frames's error-detection bits.

    ![3.png](img/3.png)

  - The parity of both the column and the row contaning the filpped bit will be in error.
  - The receiver can not only detect the fact that a single bit error has occurred, but can use the column and row indices of the column and row with parity erros to actually identify the bit that was corrupted and correct that error.
- The ability o the receiver to both detect and correct errors is known as **forward error correction (FEC)**.

### 2.2. Checksum

- In checksumming techniques, the d bit of data are treated as a sequence of k-bit integers. One simple checksumming method is to simply sum these k-bit integers and use the resulting sum as the error-detecting bits.
- Because transport-layer error detection is implemented in software, it is important to have a simple and fast error-detection scheme such as checksumming.
- On the other hand, error detection at the link layer is implemented in dedicated hardware in adapters, which can rapidly perform the more complex cyclic redundancy check (CRC) operations.

### 2.3. Cyclic Redundancy Check (CRC)

- An error-detection technique used widely in today's computer networks is based on cyclick redundancy check (CRC) codes.
- CRC codes are also known as **polynomial codes**, since it is possible to view the bit string to be sent as a polynomial whose coefficients are the 0 and 1 values in the bit string, with operations on the bit string interpreted as polynomial arithmetic.
- CRC codes operate a follows:
  - Consider d-bit piece of data, D, that sending node wants to send to the receiving node.
  - The sender and receiver must first agree on an r + 1 bit pattern, known as a generator, denote as G.
  - Choose r additional bits, R, and append them to D such that the resulting d + r bit pattern is exactly divisible by G using modulo-2 arithmetic.
  - Error checking with CRCs: The receiver devides the d + r received bits by G. If the remainder is nonzero, the receiver knows that an error has occured; otherwise the data is accepted as being correct.

    ![4.png](img/4.png)

- <D, R> can be re-write in math formula D.2^r XOR R
- <D, R> divisible by G
  - D.2^r XOR R = n.G
  - D.2^r = n.G XOR R

    -> R is the remainder when D.2^r divided by G

- Example:

    ![5.png](img/5.png)

- The longer G, the more powerfull CRC.
- CRC is very effective and is used widely
  - Wifi, ATM, Ethernet...
  - Can detect all burst errors less than r + 1 bits.
  - Modulo 2 operator are implemented by hardware.

## 3. Multiple Access Links and Protocols

- There are 2 types of network links:
  - Point-to-point link: consists of a single sender ant one end of the link and the single receiver at the other end of the link.
  - Broadcast link: can have multiple sending and receiving nodes all connected to the same, single, shared broadcast channel.
- Multiple access protocols: nodes regulate their transmission into the shared broadcast channel.
- Multiple access protocols are needed in a wide variety of network settings, including both wired and wireless access networks, and satellite networks.

    ![6.png](img/6.PNG)

- All nodes are caple of transmitting frames, more than 2 nodes can transmit frames at the same time.

    -> All node receive multiple frames at the same time -> the transmitted frames collide at all of the receiver.

### 3.1. Channel Partitioning Protocols

- There are 2 techniques that can be used to partition a broadcast channel's bandwidth among all nodes sharing a channel:
  - Time-division multiplexing (TDM): divides time into **time frames** and further divides each time frame into N **time slots**.
    - Each time slot is then assigned to one of the N nodes.
    - Whenever a node has a packet to send, it transmits the packet's bits during its assigned time slot in the TDM frame.
    - TDM eliminates collisions and is perfectly fair: Each node get a dedicated transmission rate of R/N bps during each frame time.
    - Drawback:
      - A node is limited to an average rate of R/N bps even when it is the only node with packets to send/
      - A node must always wait for its turn in the transmission sequence.
  - Frequency-division multiplexing (FDM):  divides the R bps channel into different frequencies (each with a bandwidth of R/N) and assigns each frequency to one of the N nodes.
