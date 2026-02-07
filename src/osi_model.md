## OSI model

The OSI (Open System Interconnection) model is a conceptual model which enables diverse systems to communicate using standard protocols.

```Rust
// L7: application
// L6: presentation
// L5: session
// L4: transport
// L3: network
// L2: data link
// L1: physical
```

**Host layers**: L7, L6, L5, L4. How the data is chopped up and reassembled for transport and how it is formatted to be understandable.

**Media layers**: L3, L2, L1. How data is moved between point A and point B.

### L1: Physical

- A physical medium can be copper (electrical), fibre (lights) or WIFI (radio frequency)
- L1 specifications define the transmission and reception of **raw bit streams** between a device and a shared physical medium. It defines things like voltage levels, timing, rates, distances, etc.
- This layer defines standards for transmitting onto the physical medium and standards for receiving
- Is primitive i.e. everything is broadcast and there is no inter-device communication or access control

### L2: Data link

- Requires a functional L1
- Devices at L2 have a unique hardware address (MAC)
- L2 provides frames which are a format for sending information over a L2 network
- The transmission and reception of frames is handled by L1
- A frame is a container of bits divided into parts eg. destination MAC address, source MAC address, payload, etc.
- L2 can interpret the parts of a frame it has received, L1 can't
- L2 facilitates data transfer between 2 devices on the same network (intra-network)

### L3: Network

- Used to facilitate data transfer between devices on different networks
- Adds IP for cross-network routing
- IP packets are moved from source to destination and encapsulated inside different frames along the way
- It is able to break down the segments that it receives from the transport layer into packets which are reassembled on the receiving device

It is useful to know at this point that a **subnet mask** allows a host to determine if an IP address; it needs to communicate with, is local or remote which influences if it needs to use a gateway or can communicate locally.

A subnet mask of 255.255.0.0 and /16 prefix mean the same thing.

**ARP** (Address Resolution Protocol) is used when you have a L3 packet and you want to encapsulate it inside a frame and then send that frame to a MAC address. The MAC address is not known and you need a protocol which can find the MAC address for a given IP address.

Routers are L3 devices i.e. they understand L1, L2 and L3.


### L4: Transport

- Protocols include TCP and UDP
- TCP is used for reliability, error correction and ordering of data. It is slower and creates a bidirectional channel of communication
- UDP is faster but less reliable
- TCP introduces segments which are encapsulated inside IP packets
- This layer relies on L3 for transport but introduces source and destination ports
- TCP establishes a reliable connection between 2 devices. A connection or a channel is a collection of segments.
- TCP uses a 3-way handshake with signals such as SYN, ACK and FIN
