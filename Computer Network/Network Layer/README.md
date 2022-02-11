# Network Layer

## 1. Introduction

![1.png](img/1.png)

### 1.1. Forwarding and Routing

- The role of the network layer is to move packets from a sending host to a receiving host. To do so, two important network-layer functions can be defined:
  - Forwarding: when a packet arrives at router's input link, the router must move the packet to the appropriate output link.
  - Routing: the network layer must determine the route or path taken by packets as they flow from a sender to a receiver. The algorithms that calculate these paths are referred to as **routing algorithms**.

- Every router has an forwarding table. A router forwards a packet by examining the value of a field in the arriving packet's header, and then using this header value to index into the router's forward table.

  ![2.png](img/2.png)

  - The value stored in the fowarding table entry for that header indicates the router's outgoing link interface to which that packet is to be forwarded.
  - Depending on the network-layer protocol, the header value could be the destinatio address of the packet or an indication of the connection to which the packet belongs.
- The routing algorithm may be centralized (with an algorithm executing on a central site that downloading routing information to each of the routers) or decentralized (with a piece of the distributed routing algorithm running in each router).
- Packet switch: general packet-switching device that transfers a packet from input link interface to output link interface, according to the value in a field in the hader of the packet.
- Packet switches:
  - Link-layer switches: base their forwarding decision on values in the fields of the link-layer frame -> swithes are referred to as link-layer devices.
  - Routers: base their forwarding decision on the value in the network-layer field -> routers are network-layer devices.