# ["Computer Networking: A Top-Down Approach" by James F. Kurose & Keith W. Ross]
## (00/00/26 &ndash; 00/00/26)
## **Task:**
The task is to read the following chapters of [**"Computer Networking: A Top-Down Approach" by James F. Kurose & Keith W. Ross**](https://gaia.cs.umass.edu/kurose_ross/index.php):
- **Chapter 1: &ensp;Computer Networks and the Internet**
- **Chapter 2: &ensp;Appliccation Layer**
- **Chapter 3: &ensp;Transport Layer**
- **Chapter 4: &ensp;The Network Layer: Data Plane**
- **Chapter 8: &ensp;Security in Computer Networks**

The proof will be the per-chapter notes I write below.

## **Proof:**

### **The OSI Model:**
1. **Application Layer**
2. **Presentation Layer**
3. **Session Layer**
4. **Transport Layer**
5. **Network Layer**
6. **Data Link Layer**
7. **Physical Layer**

### **Ch. 1&emsp;COMPUTER NETWORKS AND THE INTERNET**<br>(00/00/26&ndash;00/00/26)

**Hosts/End Systems:** Any device that is connected to a computer network as a source or destination of data. (e.g., mobile computer, smartphone, router, server, cell phone tower)

End systems are connected together by a network of **communication links** and **packet switches**.
> <!-- --- -->
> **\*\*NOTE****<br>
> **Packets:** Small chunks of data that messages are broken into before being sent across a computer network.
> <!-- --- -->
**Packet Switch:** Takes a packet arriving on one of its incoming communication links and forwards that packet on one of its outgoing communication links.<br>
Two types of packet switches: **Routers** and **Link-layer Switches**.

**Protocols:** define the format and the order  of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

Two type of hosts: **Clients** and **Servers**.

**Client:** A device or program that request data or services from a server. *(e.g., desktops, laptops, smartphones)*
<br>
**Server:** A powerful system or program listens for client requests and provides requested resources. *(e.g., web servers, file servers, database servers)*

**Store-and-Forward Transmission:** The packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.

Sending one packet from source to destination over a path consisting of $N$ links *(thus, there are $N-1$ routers between source and destination)* each of rate $R$, end-to-end delay = $N\dfrac{L}{R}$.

In addition to store-and-forward delays, there's also **queuing delay** due to output buffers.
<br>
**Queuing delay:** The time a data packet spends in a router's buffer before it gets transmitted onto a link.

Buffer space is finite. If a packet arrivges when buffer is already full, there is **packet loss**.

In the Internet, every end systemn has an IP address.<br>
When a source end system wants to send a packet to a destination end system, the source includes the destination's IP address in the packet's header.

**Forwarding Table:** Maps destination addresses *(IP or MAC)* to specific outgoing ports. Stored in routers/switches.

**Packet Switching:** A networking method that splits data into packets, use a store-and-forward technique through nodes, and reassembles them at the destination.

**Circuit Switching:** A networking method that reserves a dedicated and fixed communication path *(a **circuit**)* between a sender and a receiver before any data moves.
<br>
When a network establishes a circuit, it also reserves a constant transmission rate in the network's links.
<br>
*(e.g., traditional telephone networks)*

> <!-- --- -->
> **\*\*NOTE****<br>
> **Multiplexing:** A technique that allows the simultaneous transmission of multiple signals through a single channel or link.
> <!-- --- -->

**Frequency-Division Multiplexing (FDM):** divides the frequency spectrum of a link into distinct, non-overlapping frequency bands, allowing multiple independent signals to share the same link simultaneously.

**Time-Division Multiplexing (TDM):** Divides time into fixed-duration frames, subdividing each frame into a fixed number of time slots, and assigns specific slots to individual connections, allowing multiple independent sources to share the same link sequentially.

Unlike FDM, in TDM, each connection has the entire width of the frequency band *(i.e., the **bandwidth**)* at its disposal.

Circuit Switching is considered wasteful because the dedicated circuits are idle during silent periods.

Packet Switching is arguably not suitable for real-time services *(e.g., telephone calls and video conference calls)*.

**Packet Switching Benefits:**
1. Better sharing of transmission capacity than circuit switching.
2. Simpler, more efficient, and less costly to implement than circuit switching.

In packet switching, link capacity is shared on a packet-by-packet basis and only among those users who have a packet to transmit over the link.
<br>
*(For example, hypothetically, if only one user is transmitting packets over the link and all other users are completely idle, no multiplexing is done at all)*

**Point of Presence:** A group of one or more routers (at the same location) in the provider's network where customer ISPs can connect into the provider ISP.
<br>
*(Customer ISP run cables from their Central Office to the PoP)*

**Multi-homing:** To connect to two or more provider ISPs at the same time.

**Peering:** A mutual, direct interconnection between two networks that are at the same level in the internet hierarchy. It allows all traffic exchanged between the two peers to travel over this direct connection instead of going through intermediaries (that charge transit fees).

**Internet Exchange Point (IXP):** A physical third-party location where multiple ISPs (and content-provider networks) connect for peering.

**Content-Provider Networks:** Private networks built and operated by large companies *(Google, Netflix, Amazon)* to host and distribute their own content directly to ISPs and end users.

**The Internet:**<br>

