# Unit 4: Advanced Routing and Network Architecture

## Complete Study Notes

---

## Q1. Justify the need of Two-level internet architecture (CO1, L3, 5 marks)

### What is Two-Level Internet Architecture?

The two-level Internet architecture allows the Internet to be hierarchical, scalable, and efficient for global routing. It is a hierarchical structure that distinguishes between:
- **Network ID (Net-ID)** — identifies the network
- **Host ID** — identifies specific host within network

### Three Primary Needs

#### 1. Scalability — Massive Reduction in Router Table Size

| Problem | Solution |
|---------|----------|
| Cannot store entry for every host | Store entry for each **network** only |
| Billions of hosts | Millions of networks |
| Routers only need **Net-ID** entries | Dramatically smaller routing tables |

By grouping hosts into networks (identified by the Net-ID), a router only needs an entry for each network connected to the Internet, not for every host.

#### 2. Hierarchical Routing

**Two Routing Levels:**
- **Global Routing:** Uses Network-ID to route between networks
- **Local Routing:** Uses Host-ID to deliver within network

**Benefits:**
- Simplified routing decisions
- Faster packet processing
- Clear organizational boundaries
- Supports Global routing with Network-ID and Local routing with Host-ID

#### 3. Security and Privacy

It distinguishes between internal (datagrams of same organization) and external datagrams (datagrams between computers of different organizations).

**Distinguishes:**
- **Internal datagrams:** Between hosts in same organization (can remain private)
- **External datagrams:** Between different organizations (requires internet routing)

**Benefits:**
- Internal traffic stays private while communicating through external communication
- Better security policy enforcement
- Network isolation possible
- Provides security

**Conclusion:** Two-level architecture makes the Internet **scalable, efficient, and manageable** at global scale.

---

## Q2. What do you mean by NAT? Discuss the role of NAT Box (CO2, L2, 5 marks)

### What is NAT?

**Network Address Translation (NAT)** is a technology providing IP-level access between hosts at a site and the Internet **without requiring** each host to have a globally valid IP address.

**Requirements:**
- Single connection to global Internet
- At least **one globally valid IP address**

### Role of NAT Box

**NAT Box:** Computer with NAT software; all traffic passes through it.

**Key Functions:**

| Function | Outgoing Packets | Incoming Packets |
|----------|------------------|------------------|
| **Address Translation** | Replace source address (private) with global IP | Replace destination address (global) with correct private address |
| **Routing** | Forward to Internet | Forward to internal host |
| **Table Maintenance** | Track mappings | Look up destination |

**How It Appears:**
- **To internal hosts:** NAT box appears as **router** to global Internet
- **To external hosts:** All internal hosts appear to have **same global IP**

**Benefits:**
- Conserves IPv4 addresses
- Provides security (internal addresses hidden)
- Simplifies internal network management

---

## Q3. Assume Inside Local IP of three PCs PC1, PC2 and PC3 are 192.168.34.1, 192.168.34.2, 192.168.34.3, Inside Global IP is 12.1.1.1 and Outside Global IP is 13.1.1.1/24. i) Suggest the suitable NAT type to provide internet access to all PCs and justify your answer ii) If PC1 initiates connections to the web server, then write the IP address (both source and destination) of the outgoing and incoming packets before & after NAT process with port number. If PC2 and PC3 simultaneously request connection to the webserver with NAT table contents, demonstrate how these requests and replies will be handled by NAT Box (CO4, L4, 10 marks)

### i) Suitable NAT Type

**Answer:** **Port NAT (PAT/NAPT)** is the suitable method.

**Justification:**
- Only **one Inside Global IP** available (12.1.1.1)
- **Three PCs** need Internet access
- In the scenario only one inside global IP is specified but other NAT methods require more than one Inside global IP
- **PNAT uses port numbers** to distinguish connections from different internal hosts

### ii) NAT Process Demonstration

#### PC1 Connection to Web Server

**Outgoing Packet:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 192.168.34.1:1234 | 13.1.1.1:80 |
| **After NAT** | 12.1.1.1:50001 | 13.1.1.1:80 |

**Incoming Packet:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 13.1.1.1:80 | 12.1.1.1:50001 |
| **After NAT** | 13.1.1.1:80 | 192.168.34.1:1234 |

#### PC2 Connection

**Outgoing:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 192.168.34.2:1235 | 13.1.1.1:80 |
| **After NAT** | 12.1.1.1:50002 | 13.1.1.1:80 |

**Incoming:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 13.1.1.1:80 | 12.1.1.1:50002 |
| **After NAT** | 13.1.1.1:80 | 192.168.34.2:1235 |

#### PC3 Connection

**Outgoing:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 192.168.34.3:1236 | 13.1.1.1:80 |
| **After NAT** | 12.1.1.1:50003 | 13.1.1.1:80 |

**Incoming:**

| Stage | Source IP:Port | Destination IP:Port |
|-------|----------------|---------------------|
| **Before NAT** | 13.1.1.1:80 | 12.1.1.1:50003 |
| **After NAT** | 13.1.1.1:80 | 192.168.34.3:1236 |

### NAT Translation Table

| Inside Local | Inside Global | Outside Global |
|--------------|---------------|----------------|
| 192.168.34.1:1234 | 12.1.1.1:50001 | 13.1.1.1:80 |
| 192.168.34.2:1235 | 12.1.1.1:50002 | 13.1.1.1:80 |
| 192.168.34.3:1236 | 12.1.1.1:50003 | 13.1.1.1:80 |

**How NAT Box Handles Simultaneous Connections:**
- Uses **unique port numbers** (50001, 50002, 50003) to track each session
- Translates based on port mapping in table
- All external hosts see same IP (12.1.1.1) but different ports

---

## Q4. Consider an organization is having a NAT box with multiple global IPs and the organization is required to provide internet access: i) For fixed number of PCs in private network, suggest the NAT type with advantages and disadvantages ii) For variable number of PCs in the private network suggest the NAT type with advantages and disadvantages (CO4, L4, 10 marks)

### i) Fixed Number of PCs — Static NAT

**Method:** **Static NAT** — One-to-one mapping between private and public IP addresses (permanently configured). A private host address will always be mapped to predetermined public IP.

**Advantages:**

| Benefit | Description |
|---------|-------------|
| **Predictable Mapping** | Each private host always mapped to same public IP |
| **Direct Access** | Hosts in the private network can be directly accessed from the Internet |
| **Simple Configuration** | Easy to set up and maintain |
| **No Port Management** | Full end-to-end connectivity |
| **One-to-One Mapping** | For fixed number of users in private network, provides internal to public IP mapping |

**Disadvantages:**

| Drawback | Description |
|----------|-------------|
| **Wastes Addresses** | Bandwidth wasted if mapped host not using Internet access |
| **No Scalability** | Cannot accommodate more hosts than public IPs available; if hosts in private network increase, this method cannot work |
| **Requires Multiple IPs** | Needs as many public IPs as internal hosts |

### ii) Variable Number of PCs — Dynamic NAT

**Method:** **Dynamic NAT** — Multiple private IP addresses are mapped to a **pool of public IP addresses** (assigned dynamically as needed).

**Advantages:**

| Benefit | Description |
|---------|-------------|
| **One-to-One Mapping** | Maintains clean IP relationships during active sessions; makes 1-to-1 mapping between private and public IP |
| **Efficient Bandwidth Use** | Public IPs assigned only when needed; returned to pool when idle; public to private mapping may vary based on available public IP address in NAT pool |
| **Better Utilization** | Handles varying numbers of simultaneous users; efficiently makes use of bandwidth |
| **Oversubscription** | Can support more internal hosts than available public IPs (not all active simultaneously) |

**Disadvantages:**

| Drawback | Description |
|----------|-------------|
| **Complex Management** | Requires NAT box to maintain dynamic mapping table |
| **Pool Exhaustion** | If all public IPs in use, new connections fail |
| **No Inbound Initiation** | External hosts cannot initiate connections (no fixed mapping) |

**Recommendation:** Use **PAT (Port NAT)** if public IP pool is limited — allows hundreds of internal hosts with single public IP.

---

## Q5. University is spread across huge campus, is required to divide into different areas for routing purpose. Suggest the routing areas that should be part of the University campus and justify your suggestions (CO4, L3, 5 marks)

University campus is considered as Autonomous system and if the departments are distant apart, then each department can be divided into different areas for internal routing purpose.

### Routing Areas Required:

**Backbone Area:**
- There should be one backbone area which connects all the stub and non-stub areas in the campus
- Acts as the central transit area

**Standard Area:**
- At least one standard (non-stub) area which connects the AS to the internet
- Required for external connectivity

**Stub Area:**
- Two or more stub areas in the campus for easy routing of packets within the college campus
- Simplifies internal routing

---

## Q6. University is spread across huge campus, is required to divide into different areas for routing purpose. Suggest the different types of routers and routing protocols required for proper operation of both internal network and internet (CO4, L3, 5 marks)

When the college/university is divided into different routing areas, the routers are required to interconnect all the areas within the AS.

### Types of Routers Required:

**Backbone Router:**
- This router is required to connect a Stub or Standard area to the Backbone area which is must

**Internal Router:**
- Has all of its interfaces in a single area
- Required to route the packets within the single area

**Area Border Router (ABR):**
- These routers are required to summarize the information about the area in which it lies and sends the information to some other area

**Autonomous System Border Router (ASBR):**
- If it is needed to connect an AS to another AS (internet), this router is must
- Connects to an area and also to an external AS

---

## Q7. Discuss how ICMP request and reply are handled in NAT box (CO4, L2, 5 marks)

Suppose an internal host uses ping to test reachability of a destination on the Internet. The host expects to receive an ICMP echo reply for each ICMP echo request message it sends.

When an ICMP message arrives from the Internet, NAT must first determine whether the message should be handled locally or sent to an internal host. Before forwarding to an internal host, NAT translates the ICMP message.

NAT device must permit ICMP Queries and their associated responses when the Query is initiated from a private host to external hosts. The NAT device transparently forwards ICMP Query packets and the responses to these Query packets. A NAPT device further translates the ICMP Query ID and the associated checksum in the ICMP header prior to forwarding.

---

## Q8. Scenario: Consider an organization is having a LAN with FOUR computers having IP 10.0.0.2, 10.0.0.3, 10.0.0.4 & 10.0.0.5 respectively and the LAN is connected to internet via NAT Box with one internal local IP 10.0.0.1 and ONE global public IP 203.10.1.1 which performs NAT Overloading (PNAT) (CO4, L4, 10 marks)

### i) If THREE PCs simultaneously initiate a HTTP session with the web server in the internet having IP 209.130.36.50. Demonstrate the steps involved in NAT process

**Network Configuration:**
- Private LAN IPs (Inside Local):
  - PC1 → 10.0.0.2
  - PC2 → 10.0.0.3
  - PC3 → 10.0.0.4
  - PC4 → 10.0.0.5
- NAT Box: Inside (Local) IP → 10.0.0.1
- Public (Global) IP → 203.10.1.1
- Web Server (Outside Global): 209.130.36.50
- NAT Type: NAT Overloading / PAT (Port Address Translation)
- Application: HTTP (Server Port = 80)

**Step 1: Packet Generation Inside LAN**

Each PC generates an HTTP request with:
- Source IP: Private IP of PC
- Source Port: Random ephemeral port
- Destination IP: 209.130.36.50
- Destination Port: 80 (HTTP)

Assume the source port numbers:
- PC1 → Source Port 1025
- PC2 → Source Port 1026
- PC3 → Source Port 1027

**Step 2: NAT Translation (Inside → Outside)**

For each outgoing packet:
- Private source IP is replaced with 203.10.1.1
- Source port is changed to a unique public port
- Mapping is stored in the NAT translation table

Example:
- 10.0.0.2:1025 → 203.10.1.1:30001
- 10.0.0.3:1026 → 203.10.1.1:30002
- 10.0.0.4:1027 → 203.10.1.1:30003

**Step 3: Reply Handling**

When replies arrive from 209.130.36.50:
- NAT box checks the destination port
- Uses the NAT table to forward each packet to the correct private host
- Restores original private IP and port
- Each PC receives its respective HTTP response transparently

### ii) NAT Box Translation Table (Assumed Port Numbers)

| Inside Local IP | Inside Local Port | Inside Global IP | Inside Global Port | Outside Global IP | Outside Port |
|-----------------|-------------------|------------------|--------------------|-------------------|--------------|
| 10.0.0.2 | 1025 | 203.10.1.1 | 30001 | 209.130.36.50 | 80 |
| 10.0.0.3 | 1026 | 203.10.1.1 | 30002 | 209.130.36.50 | 80 |
| 10.0.0.4 | 1027 | 203.10.1.1 | 30003 | 209.130.36.50 | 80 |

---

## Q9. Discuss the role of tunneling, encryption and IP-in-IP in virtual private networks (VPN) (CO1, L2, 10 marks)

**Virtual Private Network (VPN)** is a secure, private communication channel over a public network such as the Internet.

### Role of Tunneling in VPNs

Tunneling is the core mechanism of a VPN. It allows private network traffic to pass securely across a public network by encapsulating one protocol inside another.

**How It Works:**
- The original data packets (private network packets) are encapsulated inside new packets
- These new packets are routed through the public Internet as normal IP traffic
- At the destination VPN endpoint, the packet is decapsulated and delivered to the private network

**Benefits:**
- Creates a virtual point-to-point link between two private networks or users
- Enables communication between geographically separated networks as if they were on the same LAN
- Hides internal IP addresses from the public network

### Role of Encryption in VPNs

Encryption ensures the confidentiality and integrity of data transmitted through the VPN tunnel.

**How It Works:**
- Data is converted into an unreadable format using cryptographic algorithms
- Only authorized VPN endpoints with the correct keys can decrypt the data

**Benefits:**
- Protects sensitive data from eavesdropping, interception, and tampering
- Ensures data confidentiality
- Ensures data integrity
- Supports authentication, verifying the identity of communicating parties

### Role of IP-in-IP Encapsulation in VPNs

IP-in-IP is a tunneling technique where one IP packet is encapsulated inside another IP packet.

**How It Works:**
- The original IP packet (with private source and destination IPs) becomes the payload
- A new IP header with public source and destination IPs is added
- The packet travels across the public Internet using the outer IP header
- The outer header is removed at the VPN endpoint

**Benefits:**
- Enables private IP traffic to traverse public IP networks
- Commonly used in site-to-site VPNs
- Forms the basis for tunneling mechanisms such as IPsec tunnel mode

---

## Q10. Differentiate between Software forwarding and hardware forwarding (CO2, L2, 5 marks)

### Software Forwarding

- CPU-driven forwarding of the modern dedicated network element (router/switch)
- Software techniques, such as special data structures and optimized kernels, provide the forwarding path
- Software switching occurs when traffic cannot be processed in hardware
- Software forwarding is significantly slower than hardware forwarding
- Packets forwarded by the CPU subsystem do not reduce hardware forwarding speed

### Hardware Forwarding

- Provides a high speed and low latency path
- Data plane implementation is in hardware using special ASICs (Application Specific IC)
- Lookups in hardware tables have proven to result in much higher packet forwarding performance
- Therefore have dominated network element designs, particularly for higher bandwidth network elements

---

## Q11. Discuss the working of Distributed control Plane in IP networks (CO2, L2, 5 marks)

### Distributed Control Plane

In a logically distributed architecture, the controllers are physically and logically distributed. Each controller makes a decision based on the global network view.

Individual elements or their proxies participate together to distribute reachability information in order to develop a localized view of a consistent, loop-free network.

**Examples:**
IP and MPLS forwarding are examples of a distributed control model.

---

## Q12. Give the comparison between Control plane and Data plane (CO2, L2, 5 marks)

| Aspect | Control Plane | Data Plane |
|--------|---------------|------------|
| **Function** | Intelligence of the network; determines how packets should be forwarded | Actual packet forwarding based on predefined rules |
| **Location** | Typically centralized in SDN controller | Implemented in switches and routers |
| **Operations** | Calculates paths, enforces policies, manages traffic flows | Forwards packets at high speed according to flow table entries |
| **Decision Making** | Makes routing decisions | Does not make decisions; only follows instructions |
| **Actions** | Maintains global view of network, installs/updates/removes flow rules | Performs simple actions: forward, drop, modify, encapsulate packets |

---

## Q13. Discuss the methods to reduce convergence time and increase the availability in SDN (CO1, L2, 5 marks)

The factor that affects convergence time is propagation delay of the update which is a function of distance.

### To Optimize Convergence Processing

Different vendors have:
- Message update packing
- Update prioritization
- Peer update grouping
- Other internal optimizations to reduce redundant update generation processing
- Increase the speed of convergence at the routing or control plane level

### To Optimize the Updates to the FIB

Different vendors have developed:
- Table organization strategies
- Event-driven reaction strategies for key components of the recursive nature of the FIB
- Minimize the number and type of changes to the FIB

---

## Q14. A network is comprised of network devices like routers/switches, hosts, business applications and network management tools. Incorporate these components into Software Defined Network architecture and demonstrate the working with neat diagram (CO2, L3, 10 marks)

![SDN Architecture Components](attachments/Pasted%20image%2020260211204305.png)

### SDN Architecture Components

The given components (routers/switches, hosts, business applications, and network management tools) are incorporated in a layered approach:

**SDN Infrastructure Layer:**
- **Networking devices:** Routers/switches and hosts
- **Role:** Control forwarding and data processing capabilities
- **Function:** Handle forwarding and processing of the data path
- Takes care of forwarding and processing of the data path

**SDN Control Layer:**
- **Network management tools:** Form the SDN controller part
- **Role:** Acts as the brain of the network
- **Functions:**
  - Receives instructions/requirements from application layer
  - Conveys them to networking components
  - Extracts information about network from hardware devices
  - Communicates back to applications

**Application Layer:**
- **Business applications:** Used to run large data centers
- Includes: Intrusion detection, firewall, load balancing

**Communication:** Layers communicate via northbound APIs (application ↔ control) and southbound APIs (control ↔ infrastructure).

---

## Q15. Assume a data plane is implemented in the lower box and has forwarding tables. These tables are populated and managed by the route processor's CPU/control plane program. With neat diagram demonstrate how data and control planes work for forwarding data (CO2, L3, 10 marks)

![Data and Control Plane Working](attachments/Pasted%20image%2020260211204439.png)

### Data and Control Plane Operation

**Scenario:** Packets are received by Switch A and need to be forwarded to Switch B

**Step 1: Packet Reception (Data Plane)**
- Packets are received on the input ports of the line card
- Data plane resides here

**Step 2: Unknown MAC Address Handling (Control Plane)**
- If packet from unknown MAC address is received
- Redirected to **control plane** of the device
- MAC address is **learned** here
- Packet forwarded onwards

**Step 3: RIB Processing (Control Plane)**
- Packet delivered to control plane
- Information contained therein is processed
- **Alters the RIB (Routing Information Base)**

**Step 4: FIB Update (Control and Data Plane)**
- When RIB becomes stable
- **FIB (Forwarding Information Base) updated** in:
  - Control plane
  - Data plane
- Subsequently, forwarding updated to reflect changes

**Result:** Efficient packet forwarding with separation of control logic from data forwarding.

---

## Q16. "Software Defined Network architecture optimizes and simplifies network operations", say True or False and justify your answer (CO4, L4, 5 marks)

**Answer:** True

### Justification

Software Defined Network (SDN) architecture separates the control plane from the data (forwarding) plane, allowing centralized and programmable network control.

This centralization simplifies network management by enabling administrators to:
- Configure, monitor, and optimize the entire network from a single controller
- Rather than configuring individual devices manually
- Support automation
- Enable dynamic traffic engineering
- Faster policy enforcement
- Reduced operational complexity compared to traditional, hardware-centric networks

SDN also allows configuration of the network through scripting, which optimizes network performance and simplifies day-to-day operations.

---

## Q17. Discuss the role of i) Data Plane ii) Control Plane (CO1, L2, 5 marks)

In Software Defined Networking (SDN), the network architecture is logically divided to improve flexibility, programmability, and manageability.

### i) Data Plane

The Data Plane (also called the forwarding plane) is responsible for actual packet forwarding based on predefined rules.

**Functions:**
- Forwards packets at high speed according to flow table entries
- Performs simple actions such as forward, drop, modify, or encapsulate packets
- Does not make routing decisions; only follows instructions from the control plane
- Implemented in SDN switches and routers

### ii) Control Plane

The Control Plane is the intelligence of the SDN architecture and is typically centralized in an SDN controller.

**Functions:**
- Determines how packets should be forwarded through the network
- Maintains a global view of the network topology and state
- Calculates paths, enforces policies, and manages traffic flows
- Installs, updates, and removes flow rules in data-plane devices

---

## Q18. Discuss how Distributed control plane is different from Centralized Control Plane (CO1, L2, 5 marks)

### Distributed Control Plane

In conventional networks, every switch has its own data plane and control plane. The control plane of various switches exchange topology information and constructs a routing table to decide where an incoming data packet has to be forwarded via the data plane. As the routing table is constructed individually, this is called Distributed Control Plane.

### Centralized Control Plane

One single control plane/network brain would push commands to each device, thus commanding it to manipulate its physical switching and routing hardware. In CCP, a network administrator can shape traffic via a centralized console without having to touch the individual switches. The forwarding activity is decided based on the entries in flow tables, which are pre-assigned by the controller.

---

## Q19. With neat diagram discuss the working of OpenFlow Protocol (CO1, L2, 5 marks)

![OpenFlow Protocol](attachments/Pasted%20image%2020260211204458.png)

### OpenFlow Protocol Overview

**Definition:**
- **Layer 2 communications protocol**
- Gives access to forwarding plane of network switch/router
- Enables communication between OpenFlow switches and controller

### Architecture

OpenFlow was architected for:
- Devices containing only **data planes**
- Respond to commands from **(logically) centralized controller**
- Controller houses **single control plane** for network

### Working Mechanism

**Protocol Specification:**
- Describes communication between:
  - OpenFlow switches (data plane devices)
  - OpenFlow controller (control plane)

**Key Concepts:**
- Centralized control plane sends commands
- Distributed data planes execute forwarding
- Flow-based forwarding using flow tables
- Controller installs/removes flow entries dynamically

---

## Q20. Define Convergence. Discuss the factors that affect the Convergence time in Distributed Control Plane (CO3, L2, 5 marks)

### Definition

**Convergence** is the time it takes from when a network element introduces a change in reachability of a destination due to a network event to when this change is seen and instantiated by all other relevant network elements.

**Convergence time** is a measure of how fast a group of routers reach the state of convergence.

### Factors Affecting Convergence Time

The factor that affects this convergence time is:
- **Propagation delay** of the update which is a function of distance
- Other delay components are updating of RIB and FIB

### Optimization Methods

To optimize convergence processing, different vendors have:
- Message update packing
- Update prioritization
- Peer update grouping
- Other internal optimizations to reduce redundant update generation processing
- Increase the speed of convergence at the routing or control plane level

---

## Q21. Define High Availability. Discuss how to provide High availability in IP networks (CO1, L2, 5 marks)

### Definition

**High Availability** is used to describe the period of time when a network service is available.

### Methods to Provide High Availability

**Redundancy at the Network Level:**
- Redundant routers/switches
- Redundant paths in the network design
- Allow for the failure of a link or element

**Redundancy at the Element Level:**
- Using redundant route processors/switch control modules
- The redundant processors can work in either a stateless active/standby mode

---

## Q22. Discuss the role of Routing Information Base (RIB) and Forwarding Information Base (FIB) in Software Defined Networks (CO1, L2, 5 marks)

### Routing Information Base (RIB)

The **RIB** is the data set used to store the network topology.

**Functions:**
- Often kept consistent (i.e., loop-free) through the exchange of information between other instances of control planes within the network
- Control plane establishes the local data set used to create the forwarding table entries
- These entries are in turn used by the data plane to forward traffic between ingress (accepts network traffic) and egress (forwards the network traffic) ports on a device

### Forwarding Information Base (FIB)

A **FIB**, also known as a forwarding table or MAC table, is most commonly used in network bridging, routing, and similar functions.

**Functions:**
- Finds the proper output network interface to which the input interface should forward a packet
- A dynamic table that maps MAC addresses to ports
- A memory construct used by Ethernet switch to map a station's MAC address to the switch port the station is connected to

---

## Q23. Discuss the advantages and disadvantages of Software Defined Network (CO1, L2, 5 marks)

### Advantages

1. **Programmability:** Network is programmable hence can easily be modified via the controller rather than individual switches

2. **Cost Reduction:** Switch hardware becomes cheaper since each switch only needs a data plane

3. **Hardware Abstraction:** Hardware is abstracted, hence applications can be written on top of controller independent of switch vendor

4. **Better Security:** Provides better security since the controller can monitor traffic and deploy security policies. For example, if the controller detects suspicious activity in network traffic, it can reroute or drop the packets

### Disadvantages

**Single Point of Failure:** The central dependency of the network means single point of failure, i.e., if the controller gets corrupted, the entire network will be affected.

---

## Q24. With a neat diagram explain the architecture of Software Defined Networks (CO1, L2, 10 marks)

![SDN Architecture Layers](attachments/Pasted%20image%2020260211204514.png)

### SDN Architecture - Three Layers

**Application Layer:**
- Contains typical network applications:
  - Intrusion detection
  - Firewall
  - Load balancing
- Business applications

**Control Layer:**
- **SDN Controller:** Acts as brain of network
- Maintains global view of network
- Functions:
  - Allows hardware abstraction to applications
  - Centralized network intelligence
  - Appears as single logical switch to applications

**Infrastructure Layer:**
- Physical switches forming **data plane**
- Carries out actual movement of data packets
- Forward packets based on flow table entries

### Communication Interfaces

- **Northbound APIs:** Between application and control layer
- **Southbound APIs:** Between control and infrastructure layer

### Key Characteristics

| Feature | Description |
|---------|-------------|
| **Directly Programmable** | Network control separated from forwarding functions |
| **Agile** | Dynamically adjust network-wide traffic flow to meet changing needs |
| **Centrally Managed** | Software-based SDN controller maintains global network view |
| **Programmatically Configured** | Configure, manage, secure, optimize via automated SDN programs which can be written by network managers themselves |

---

## Q25. Assume RV College of Engineering campus is having different departments & each dept is spread across and deployed with multiple routers. All the departments are interconnected to the routers in the administrative block. Considering the whole campus as one Autonomous System and required to connect to the internet, propose the different types of network areas and the routers needed to setup computer network across the campus (CO4, L4, 10 marks)

### Proposed Network Architecture

As entire RV College of Engineering campus is considered as a single Autonomous System (AS), an Interior Gateway Protocol (IGP) such as OSPF is the most suitable choice.

### Network Areas Required

**Backbone Area – Area 0:**
- Acts as the central transit area
- All other areas must connect to Area 0
- Ensures efficient inter-area communication
- Area 0 mandates that all inter-area traffic must pass through the backbone area
- Administrative Block is considered to be Area 0

**The backbone area may contain:**
- **Area Border Router (ABR):** An OSPF router that has one or more interfaces in the backbone area and one or more interfaces in a non-backbone area. These routers summarize the information about the area in which it lies and sends the information to some other area
- **Internal Router:** Has all of its interfaces in a single area. Floods the area with routing information
- **Autonomous System Border Router (ASBR):** Connects to an area and also to an external AS

### Departmental Areas – Regular OSPF Areas

Each department is assigned a separate OSPF area and can be named as:
- **Area 1** – Master of Computer Applications
- **Area 2** – Electronics Department
- **Area 3** – Computer Science Department

**Each area may be:**

**Stub Area** which may contain:
- Only the internal routers connecting only the interfaces in the same area

**Non-Stub Area** with:
- Internal routers connecting the routers within the area
- **Autonomous System Border Router (ASBR):** Connects to an area and also to an external AS
- **Area Border Router (ABR):** An OSPF router that has one or more interfaces in the backbone area and one or more interfaces in a non-backbone area

---

## Q26. Discuss the different Interior Gateway Protocols used in Autonomous System (AS) to route the IP packets within the different areas of the same AS (CO2, L2, 10 marks)

### Overview

An **Interior Gateway Protocol (IGP)** is used within an Autonomous System (AS) to exchange routing information and route IP packets between routers that are under a single administrative control. IGPs ensure efficient, loop-free, and scalable routing inside the AS, often by dividing the network into areas for better management.

### Commonly Used IGPs

#### 1. Routing Information Protocol (RIP)

- Uses Distance Vector Routing (DVR) algorithm based on number of hops
- Works on the Network layer of the OSI model
- RIP uses port number 520
- The maximum hop count allowed between source and destination for RIP is 15
- A hop count of 16 is considered as network unreachable

#### 2. Open Shortest Path First (OSPF)

- Uses Link State Routing Algorithm
- Uses the Dijkstra's algorithm for calculating the shortest path
- Divides an autonomous system into areas
- Area is a collection of routers, hosts, networks all contained within an autonomous system
- Communication of hello packets are sent over each 10 sec
- When the reply is not received within 40 sec, it is considered timeout
- When there is a slight change in the router configuration (routers added/removed), the routing table is updated very fast
- Based on link state routing protocol

#### 3. Intermediate System to Intermediate System (IS-IS) Protocol

- Link-state IGP
- Uses flooding link state information throughout a network of routers
- IS-IS operates over Layer 2
- IS-IS doesn't require IP connectivity between the routers as updates are sent via CLNS (Connectionless Network Service) instead of IP
- CLNS uses network service access points (NSAPs) to address end systems
- IS-IS router can belong to only one area

---

## Q27. Discuss the role of Border Gateway Protocol (BGP) in routing the packets across different Autonomous Systems (CO2, L2, 5 marks)

### Overview

BGP is used to exchange routing information for the internet and is the protocol used between ISPs which are different AS.

### Key Characteristics

- BGP is EGP (Exterior Gateway Protocol) protocol
- Open standard protocol
- **Inter-Autonomous System Configuration:** The main role of BGP is to provide communication between two autonomous systems
- Coordination among ASBR (Autonomous System Border Routers), i.e., BGP speakers within the AS
- It is a Path-vector protocol
- BGP is application layer protocol
- Uses TCP for reliability
- TCP port 179

---

## Q28. Analyze the importance of BGP Peering and demonstrate the steps required for BGP peering (CO3, L3, 5 marks)

### Importance

**BGP peering** is the process where two routers (BGP speakers) establish a connection, typically over TCP, to exchange routing information (routes) between different Autonomous Systems (AS) on the internet, enabling them to direct traffic between networks efficiently.

### Peering Steps

**Step 1: Establish Connection**
- Routers configure each other as neighbors
- Forming a TCP session (port 179)

**Step 2: Exchange Messages**
- Once connected, they exchange BGP messages:
  - OPEN
  - KEEPALIVE
  - UPDATE
  - NOTIFICATION
- To confirm parameters and establish the session

**Step 3: Share Routes**
- Peers exchange Network Layer Reachability Information (NLRI) to announce reachable IP prefixes (routes)

---

## Q29. Discuss the Key features and components for IPv6 addressing (CO1, L2, 5 marks)

### Key Features/Advantages

**Large Address Space:**
- 128-bit addresses vastly increase available addresses

**Simplified Header Format:**
- Base header separated from optional fields
- Speeding up routing

**Enhanced Security:**
- Built-in support for IPsec (encryption/authentication)

**New Options & Extensibility:**
- Allows additional functionalities and future protocol extensions

### Components in IPv6 Address Format

- There are 8 groups of 4 hexadecimal digits (0-9, A-F)
- Each group represents 2 Bytes
- Each Hex-Digit is of 4 bits (1 nibble)
- Delimiter used: colon (:)

**Network Prefix (first 64 bits):**
- Used for routing
- Further divided into:
  - Global routing prefix (from an ISP)
  - Subnet ID (set by the network administrator)

**Interface Identifier (last 64 bits):**
- Uniquely identifies a specific device on a network

---

## Q30. State IPv6 compression rules and apply the stated rules and write following IPv6 address in compressed form (CO1, L3, 5 marks)
i) 2001:0db8:0001:0000:0000:0ab9:C0A8:0102
ii) fe80:0000:0000:0000:0202:b3ff:fe1e:8329
iii) 0000:0000:0000:0000:0000:0000:0000:0001

### IPv6 Compression Rules

**Leading Zero Omission:**
- Remove leading zeros from any group
- Examples:
  - 0db8 becomes db8
  - 0000 becomes 0
  - 0042 becomes 42
  - 0000 becomes 0
- Trailing zeros cannot be removed (e.g., F400 cannot become F4)

**Double Colon (::):**
- Replace one or more consecutive groups of all zeros with ::
- This can only be used once per address to avoid ambiguity

### Compressed Forms

i) **2001:0db8:0001:0000:0000:0ab9:C0A8:0102**
   - Compressed: **2001:db8:1::ab9:C0A8:102**

ii) **fe80:0000:0000:0000:0202:b3ff:fe1e:8329**
    - Compressed: **fe80::202:b3ff:fe1e:8329**

iii) **0000:0000:0000:0000:0000:0000:0000:0001**
     - Compressed: **::1**

---

## Q31. Discuss the different types of primary addressing methods supported by IPv6 (CO1, L2, 5 marks)

### Unicast

Identifies a single interface; packets are delivered to that specific device.

**Types:**
- **Global Unicast (2000::/3):** Publicly routable on the internet, similar to IPv4 public addresses
- **Link-Local (fe80::/10):** Automatically assigned to every interface; used for communication on the same local link and not routable
- **Unique Local (fc00::/7):** Used for private internal networks, similar to IPv4 private ranges

### Multicast (ff00::/8)

- Identifies a group of interfaces
- Packets are delivered to all members
- IPv6 does not have broadcast addresses; it uses multicast instead

### Anycast

- Assigned to multiple interfaces
- Packets are delivered to the "nearest" member as determined by routing metrics

---

## Q32. Differentiate between Link-Local address and Loopback address in IPv6 (CO1, L2, 5 marks)

| Feature | Link-Local Address | Loopback Address |
|---------|-------------------|------------------|
| **Purpose** | Communication between devices on the same local network segment (e.g., your computer and a local printer). Essential for Neighbor Discovery Protocol (NDP) and automatic address configuration | Testing and troubleshooting the device's own internal TCP/IP software stack |
| **IPv6 Range** | fe80::/10 (typically implemented as fe80::/64) | ::1/128 (full form is 0:0:0:0:0:0:0:1) |
| **Routability** | Not routable by routers. Packets with link-local source or destination addresses must not be forwarded beyond the local link | Not routable at all. Traffic never leaves the host device and bypasses all physical network interfaces |
| **Scope** | Communication is limited to the single physical network segment | Communication is strictly internal to the local machine |
| **Assignment** | Automatically configured on every IPv6-enabled physical interface (and loopback interfaces) | A single, predefined address that is part of the operating system's software, not tied to a physical interface |

---

## Q33. With a neat diagram discuss the role of each field of IPv6 packet format (CO1, L2, 10 marks)

![IPv6 Packet Format](attachments/Pasted%20image%2020260211204535.png)

### IPv6 Packet Header Fields

**Version (4 bits):**
- Indicates IP version
- Always **6** for IPv6 (bit sequence: 0110)

**Traffic Class (8 bits):**
- Indicates class/priority of IPv6 packet
- Similar to Service Field in IPv4
- Helps routers handle traffic based on priority
- Low-priority packets discarded during congestion

**Flow Label (20 bits):**
- Used by source to label packets belonging to same flow
- Requests special handling by intermediate routers
- Used for:
  - Voice/video calls
  - Real-time multimedia session
  - Interactive data stream
  - Any traffic needing special handling (QoS)
- All packets in a flow have same source, destination, and flow label

**Payload Length (16 bits):**
- 16-bit unsigned integer
- Total size of payload
- Includes extension headers (if any) and upper-layer packet

**Next Header (8 bits):**
- Indicates type of extension header following IPv6 header
- Or indicates upper-layer protocols (TCP, UDP)

**Hop Limit (8 bits):**
- Same as TTL in IPv4
- Maximum number of intermediate nodes packet can travel
- Decremented by 1 at each hop
- Packet discarded when reaches 0
- Prevents infinite loops

**Source Address (128 bits):**
- IPv6 address of original source

**Destination Address (128 bits):**
- IPv6 address of final destination
- Used by intermediate nodes to route packet correctly

**Extension Headers (Variable):**
- Rectifies limitations of IPv4 Option Field
- Important part of IPv6 architecture
- Next Header field points to first extension header
- Extension headers form chain

---

## Q34. "Is Multi-protocol Label Switching (MPLS) faster than Packet switching?" Justify your answer with technical reasons (CO4, L4, 5 marks)

**Answer:** Yes

### Justification

Multi-Protocol Label Switching (MPLS) is an advanced packet-forwarding technique used in modern networks. Instead of making routers look into complex Layer 3 routing tables for every IP packet, MPLS uses labels for forwarding decisions.

**How It Works:**
- These labels create pre-defined, efficient paths across the network
- Enhances speed, scalability and traffic management

**Technical Advantages:**
- MPLS is a mechanism that switches traffic based on labels instead of routing traffic
- MPLS can transport pretty much everything: IP, IPv6, Ethernet, frame-relay, PPP
- Faster forwarding decisions based on simple label lookup
- Reduced processing overhead compared to IP routing table lookups

---

## Q35. "MPLS operates at layer 2.5 in OSI layer architecture", say TRUE or FALSE and justify your answer (CO3, L4, 5 marks)

**Answer:** True

### Justification

At Layer 2, traditional forwarding is based on MAC addresses and at Layer 3, routing decisions are made using IP addresses.

**But MPLS introduces labels, which are inserted between the Layer 2 and Layer 3 headers.**

**Why Layer 2.5:**
- In MPLS, forwarding decisions are made using short labels, not IP addresses (unlike Layer 3)
- MPLS still relies on Layer 3 routing protocols (such as OSPF, IS-IS, or BGP) to establish label paths (unlike pure Layer 2 technologies)

Hence MPLS operates at layer 2.5 in OSI architecture.

---

## Q36. Discuss the different types of routers used in MPLS networks (CO1, L2, 5 marks)

### Router Types

**Provider Edge (PE) Router:**
- Router at the edge of the MPLS provider's network
- Adds/removes labels from packets

**Customer Edge (CE) Router:**
- Router at the customer's network edge that communicates with PE routers

**Label Switch Router (LSR):**
- Router inside the MPLS core that understands and processes labels

**Ingress LSR:**
- First router that receives the packet from CE
- Pushes (adds) the MPLS header

**Intermediate LSR:**
- Routers that swap labels as packets move across the MPLS path

**Egress LSR:**
- The last router in the MPLS domain
- Pops (removes) the MPLS header before sending the packet to the CE

---

## Q37. With a neat diagram demonstrate the role of different types of routers in MPLS networks (CO1, L3, 10 marks)

![MPLS Network Topology](attachments/Pasted%20image%2020260211204554.png)

### MPLS Router Types and Roles

**Customer Edge (CE) Router:**
- **Location:** Customer's network edge
- **Role:** Communicates with PE routers
- Sends/receives standard IP packets

**Provider Edge (PE) Router / Ingress LSR:**
- **Location:** Edge of MPLS provider's network
- **Role:** Adds/removes labels from packets
- **Ingress Function:** Receives packet from CE, **pushes (adds)** MPLS header

**Label Switch Router (LSR):**
- **Location:** Inside MPLS core
- **Role:** Understands and processes labels
- **Intermediate LSR Function:** **Swaps labels** as packets move across MPLS path
- Label lookup in LFIB (Label Forwarding Information Base)

**Egress LSR / PE Router:**
- **Location:** Exit point of MPLS domain
- **Role:** **Pops (removes)** MPLS header
- Forwards original IP packet to CE router

### MPLS Operations

| Operation | Description | Router Type |
|-----------|-------------|-------------|
| **Push** | Add MPLS label | Ingress PE |
| **Swap** | Replace existing label with new one | Intermediate LSR |
| **Pop** | Remove MPLS label | Egress PE |

---

## Q38. Discuss structure of MPLS header format with role of each field in the header (CO1, L2, 5 marks)

![MPLS Header Format](attachments/Pasted%20image%2020260211204608.png)

### MPLS Header Structure

**Label (20 bits):**
- Carries MPLS label value
- Used by LSRs to make forwarding decisions
- Router performs label lookup, then swaps/pushes/pops label

**EXP / Traffic Class (TC) (3 bits):**
- Originally called EXP (Experimental) field
- Now defined as Traffic Class field
- **Purpose:** Support Quality of Service (QoS) and Class of Service (CoS)
- Helps in:
  - Prioritizing traffic
  - Congestion control
  - Packet scheduling within MPLS network

**Bottom of Stack (BoS) (1 bit):**
- Indicates if current label is last in MPLS label stack
- **BoS = 1:** This is bottom (last) label
- **BoS = 0:** More labels exist below this label
- Supports label stacking for VPNs and traffic engineering

**Time To Live (TTL) (8 bits):**
- Prevents packets from looping indefinitely
- TTL value decremented by 1 at each MPLS hop
- Packet discarded when TTL reaches zero
- Similar to IP TTL field

---

## Q39. Discuss advantages and disadvantages of Multi-Protocol Label Switching networks (CO1, L2, 5 marks)

### Advantages of MPLS

- Faster packet forwarding (label-based)
- Supports multiple protocols (hence "multi-protocol")
- Enables Traffic Engineering (efficient use of resources)
- Facilitates Quality of Service (QoS)

### Disadvantages of MPLS

- Expensive to implement compared to IP routing
- Complexity in configuration and management
- Security is weaker than encrypted VPN solutions
- Less suitable for small-scale networks

---

## Q40. Identify the role of following routers in MPLS networks (CO2, L2, 5 marks)
i) Label Switch router
ii) Provider Edge router
iii) Customer Edge router

### Router Roles

**i) Label Switch Router (LSR):**
- Router inside the MPLS core that understands and processes labels

**ii) Provider Edge (PE) Router:**
- Router at the edge of the MPLS provider's network
- Adds/removes labels from packets

**iii) Customer Edge (CE) Router:**
- Router at the customer's network edge that communicates with PE routers

---

## Q41. Discuss the three operational stages of MPLS forwarding (CO1, L2, 5 marks)

### Ingress Stage (Push)

- CE sends an IP packet to the PE (Ingress LSR)
- Ingress PE assigns a label based on the destination
- Attaches an MPLS header

### Core Stage (Swap)

- Intermediate LSRs forward the packet based only on the label
- They swap the label with a new one as defined in their Label Forwarding Information Base (LFIB)

### Egress Stage (Pop)

- The Egress PE removes the MPLS header
- Forwards the original IP packet to the CE router

---

## Q42. "Proxy ARP concept helps, when a host from home network needs to communicate with mobile node in foreign network." Say True or False and justify your answer (CO4, L4, 5 marks)

**Answer:** True

### Justification

The sender will ARP for the mobile's hardware address, encapsulate the datagram, and transmit it.

If a mobile has moved to a foreign network, the home agent must intercept all datagrams, including those sent by local hosts.

**To do this:**
- A home agent must listen for ARP requests that specify the mobile as a target
- Must answer the requests by supplying its own hardware address

**Result:**
Proxy ARP is completely transparent to local computers - any local system that ARPs for a mobile's address will receive a reply, and will forward the datagram as usual.

---

## Additional Important Topics

### Control Plane vs Data Plane Comparison

| Aspect | Control Plane | Data Plane |
|--------|---------------|------------|
| **Function** | Network intelligence and decision-making | Actual packet forwarding |
| **Location** | Centralized (SDN) or distributed (traditional) | In switches/routers |
| **Speed** | Slower (complex processing) | Very fast (hardware-based) |
| **Tasks** | Routing table computation, policy enforcement | Packet lookup and forwarding |
| **Examples** | OSPF, BGP, SDN Controller | Flow tables, FIB lookups |

### Common Port Numbers

| Protocol | Port Number | Description |
|----------|-------------|-------------|
| HTTP | 80 | Web traffic |
| HTTPS | 443 | Secure web traffic |
| RIP | 520 | Routing Information Protocol |
| BGP | 179 | Border Gateway Protocol |

### Key Terms

- **AS (Autonomous System):** Collection of IP networks under single administrative control
- **IGP (Interior Gateway Protocol):** Routing protocol within an AS
- **EGP (Exterior Gateway Protocol):** Routing protocol between ASes
- **LSR (Label Switch Router):** MPLS core router
- **FIB (Forwarding Information Base):** Forwarding table
- **RIB (Routing Information Base):** Routing table
- **NAPT (Network Address Port Translation):** NAT with port mapping
- **QoS (Quality of Service):** Traffic prioritization mechanism

---

## End of Unit 4 Notes
