# Advanced Computer Networks (ACN) – Exam Study Notes

### Q1: "ICMP is a error reporting protocol." Justify the statement with a proper example (CO1 L3, 5 marks)

---

**Answer:**

#### What is ICMP?

**Internet Control Message Protocol (ICMP)** is designed to report errors to the **source host** — it does **not transfer user data**.

#### Key Characteristics

- Used by network devices (routers and hosts) to communicate **control and error information**
- Helps diagnose network problems and ensure efficient packet routing
- Provides feedback on network conditions

#### Example: Destination Unreachable Message

**Scenario:**

1. **Host A** sends an IP packet to **Host B**
2. A router (**R1**) receives the packet but **cannot deliver it** (e.g., network down, no route available)
3. **R1 sends an ICMP error message** back to Host A indicating the failure

**Why this proves ICMP is an error reporting protocol:**

- The error message originates from an intermediate device (router)
- The source host receives diagnostic information
- No user data is transmitted — only control information

---

### Q2: Discuss the role of type field and code field of ICMP in error reporting (CO1 L2, 5 marks)

---

**Answer:**

| **Field** | **Size** | **Purpose**                                                    | **Role in Error Reporting**                                                                 |
| --------- | -------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Type**  | 8 bits   | Designates the general category/purpose of the ICMP message    | Categorizes message as **Error message** (e.g., Type 3) or **Query message** (e.g., Type 0) |
| **Code**  | 8 bits   | Provides additional detail about the message indicated by Type | Specifies the **exact reason** for failure; enables precision in error reporting            |

#### Example: Destination Unreachable

- **Type = 3**: Destination Unreachable (error category)
- **Code = 0**: Net Unreachable (specific reason)

**Benefits:**

- **Type** gives broad classification
- **Code** provides granular details for troubleshooting

---

### Q3: Show how ICMP Message supports ping command to diagnose the network connectivity (5 marks)

---

**Answer:**

#### How Ping Uses ICMP

**Ping Mechanism:**

1. **Sends** ICMP **Echo Request** messages (Type 8) to a target host
2. If the host is reachable, it **responds** with ICMP **Echo Reply** (Type 0)

#### Diagnostic Information Provided

| **Metric**                | **What it Measures**                                 |
| ------------------------- | ---------------------------------------------------- |
| **Round-Trip Time (RTT)** | Time for message to travel to destination and return |
| **Packet Loss**           | Number of requests without replies                   |
| **Connectivity**          | Whether destination is reachable                     |
| **Path Issues**           | Delays, routing problems, network congestion         |

**Use Cases:**

- Testing if a host is alive
- Measuring network latency
- Identifying connection problems

---

### Q4: Explain the different error messages that can be reported by ICMP protocol (5 marks)

---

**Answer:**

#### ICMP Error Message Types

| **Type**    | **Name**                    | **Purpose**                                                |
| ----------- | --------------------------- | ---------------------------------------------------------- |
| **Type 3**  | **Destination Unreachable** | Router/host cannot deliver the IP packet                   |
| **Type 11** | **Time Exceeded**           | Time limit expired (e.g., TTL reaches 0); packet discarded |
| **Type 12** | **Parameter Problem**       | Error/ambiguity in IP header; impossible to process        |
| **Type 5**  | **Redirect**                | Router informs host of a better route to destination       |
| **Type 4**  | **Source Quench**           | Flow control at network layer (deprecated)                 |

**Important Notes:**

- Each type addresses specific network failure scenarios
- Helps administrators pinpoint exact issues
- Essential for network troubleshooting and diagnostics

---

### Q5: Discuss the reasons for minimizing the use of network numbers (CO2 L2, 5 marks)

---

**Answer:**

#### Need for Minimizing Network Numbers

**Three Primary Reasons:**

1. **Administrative Overhead**
   - Managing large numbers of network addresses is complex
   - Requires significant resources for allocation and tracking

2. **Routing Table Explosion**
   - Routing tables in routers become extremely large
   - Increased computational load on routers
   - High network overhead when exchanging routing information

3. **Address Space Exhaustion**
   - Original address scheme cannot accommodate current Internet scale
   - IPv4 address space is limited

#### Consequences of Not Minimizing

- Heavy load on Internet infrastructure
- Significant computational effort in routers
- Inefficient use of address space

---

### Q6: In a network user gives the command ssh <ip>, assume the request datagram reaches the machine with specified ip, but user at source gets error report from ICMP. Predict the reasons for error and construct the corresponding ICMP packets with required message Type and Code (CO1 L4, 10 marks)

---

**Answer:**

#### Scenario Analysis

User attempts SSH connection (TCP port 22), datagram reaches destination, but **ICMP error** is received at source.

#### Predicted Reasons and ICMP Messages

| **Reason**                                  | **ICMP Type** | **ICMP Code** | **Description**                                                      |
| ------------------------------------------- | ------------- | ------------- | -------------------------------------------------------------------- |
| **Port Unreachable** (Most Likely)          | 3             | 3             | No service listening on TCP port 22; local firewall blocked the port |
| **Communication Administratively Filtered** | 3             | 13            | Firewall/router blocks traffic to port 22 due to policy              |
| **TTL Exceeded**                            | 11            | 0             | Time-To-Live expired before reaching destination                     |

#### ICMP Packet Construction (Port Unreachable)

**ICMP Header:**

| **Field** | **Value (Decimal)** | **Value (Hex)** | **Description**         |
| --------- | ------------------- | --------------- | ----------------------- |
| Type      | 3                   | 0x03            | Destination Unreachable |
| Code      | 3                   | 0x03            | Port Unreachable        |
| Checksum  | (Calculated)        | -               | ICMP message checksum   |
| Unused    | 0                   | 0x00000000      | Four unused bytes       |

**ICMP Data Section:**

- Contains **IP Header** of original packet (source IP, destination IP, protocol)
- First **8 bytes** of original **TCP Header** (source port, destination port 22)
- Helps source identify which connection failed

**Administrative Filter (Alternative):**

- Type = 3, Code = 13
- Indicates policy-based rejection of SSH traffic

---

### Q7: Discuss the role of Transparent router in minimizing the number of IP address in network (5 marks)

---

**Answer:**

#### What is a Transparent Router?

A **Transparent Router** connects a **single host port** from a WAN to a LAN, appearing invisible to other hosts and routers on the WAN.

#### Key Characteristics

| **Feature**              | **Benefit**                                                        |
| ------------------------ | ------------------------------------------------------------------ |
| **Invisibility**         | Other WAN devices don't know it exists                             |
| **Address Conservation** | Local network doesn't need a separate IP prefix                    |
| **Demultiplexing**       | Routes WAN datagrams to appropriate local host using address table |
| **Multiplexing**         | Accepts datagrams from local hosts and forwards to WAN             |

#### How It Minimizes Addresses

- **No separate IP prefix** required for the local network
- Internal hosts appear to have **public WAN-routable addresses**
- Reduces the number of globally unique addresses needed
- Efficient for small networks with limited hosts

---

### Q8: A webserver in India transmits a TCP segment embedded in packet with packet size MTU=1500KB to a client in a London network which can handle packet with MTU=1000KB and not able to deliver the packet. Predict the reasons for error and construct the corresponding ICMP packets with required message Type and Code (10 marks)

---

**Answer:**

#### Problem Identification

**Reason:** **MTU (Maximum Transmission Unit) Mismatch**

| **Location**   | **MTU** | **Status**    |
| -------------- | ------- | ------------- |
| India Server   | 1500 KB | Packet sent   |
| London Network | 1000 KB | Cannot handle |

#### Root Cause Analysis

**MTU Mismatch Details:**

1. Packet size (1500 KB) **exceeds** London network's MTU (1000 KB)
2. Intermediate router encounters packet **larger than outgoing interface MTU**
3. If **"Don't Fragment" (DF) bit is set to 1** in IP header:
   - Router **cannot fragment** the packet
   - Packet is **dropped**
   - **ICMP error sent** to source

#### ICMP Packet Construction

**ICMP Error Message:**

| **Field** | **Value** | **Meaning**                           |
| --------- | --------- | ------------------------------------- |
| **Type**  | 3         | Destination Unreachable               |
| **Code**  | 4         | Fragmentation required but DF bit set |

**Message Flow:**

1. Router in London drops oversized packet
2. Sends ICMP Type 3, Code 4 to India server
3. Server receives notification about MTU issue

**Solution:** Server should implement **Path MTU Discovery** to determine optimal packet size

---

### Q9: Justify any 3 reasons for adopting Classless addressing (CO2 L4, 5 marks)

---

**Answer:**

#### Why Classless Addressing?

Classless addressing **overcomes limitations** of classful addressing through these key improvements:

#### 1. Conservation of IPv4 Address Space (VLSM)

**Problem with Classful:**

- Fixed network sizes (Class C: 254 hosts, Class B: 65,534 hosts)
- Organization needing 500 hosts forced to take Class B
- **Wastes 65,000+ addresses**

**Solution:**

- **Variable Length Subnet Masking (VLSM)** allows flexible allocation
- Precisely match address block to actual need

#### 2. Reduction in Routing Table Size (Route Aggregation)

**Problem with Classful:**

- Every network requires **separate routing table entry**
- Leads to **"routing table explosion"**
- Overwhelms router memory and slows processing

**Solution:**

- **Route aggregation/summarization** possible
- Multiple small prefixes combined into single larger block
- Example: Four /24 networks → One /22 prefix

#### 3. Efficient Bandwidth Utilization

**Benefits:**

- Smaller routing tables = **less data exchanged** between routers
- Reduced routing protocol overhead
- Faster convergence
- More scalable Internet infrastructure

---

### Q10: A class C address of 192.168.10.0/24 has been allocated. Perth, Sydney, and Singapore have a WAN connection to Kuala Lumpur. • Perth requires 60 hosts. • Kuala Lumpur requires 28 hosts. • Sydney and Singapore each require 12 hosts. Calculate VLSM subnets and the respective hosts IP and Broadcast IP for all the FOUR cities (CO3 L3, 10 marks)

---

**Answer:**

#### VLSM Subnet Calculations

**Step 1: Perth (60 hosts required)**

| **Parameter** | **Value**                     | **Calculation**                |
| ------------- | ----------------------------- | ------------------------------ |
| Hosts needed  | 60                            | Use 6 bits: 2⁶ - 2 = 62 usable |
| Subnet mask   | /26                           | 255.255.255.192                |
| Network ID    | 192.168.10.0                  | -                              |
| Host IP range | 192.168.10.1 to 192.168.10.62 | -                              |
| Broadcast IP  | 192.168.10.63                 | -                              |

**Step 2: Kuala Lumpur (28 hosts required)**

| **Parameter** | **Value**                      | **Calculation**                |
| ------------- | ------------------------------ | ------------------------------ |
| Hosts needed  | 28                             | Use 5 bits: 2⁵ - 2 = 30 usable |
| Subnet mask   | /27                            | 255.255.255.224                |
| Network ID    | 192.168.10.64                  | -                              |
| Host IP range | 192.168.10.65 to 192.168.10.94 | -                              |
| Broadcast IP  | 192.168.10.95                  | -                              |

**Step 3: Sydney (12 hosts required)**

| **Parameter** | **Value**                       | **Calculation**                |
| ------------- | ------------------------------- | ------------------------------ |
| Hosts needed  | 12                              | Use 4 bits: 2⁴ - 2 = 14 usable |
| Subnet mask   | /28                             | 255.255.255.240                |
| Network ID    | 192.168.10.96                   | -                              |
| Host IP range | 192.168.10.97 to 192.168.10.110 | -                              |
| Broadcast IP  | 192.168.10.111                  | -                              |

**Step 4: Singapore (12 hosts required)**

| **Parameter** | **Value**                        | **Calculation**                |
| ------------- | -------------------------------- | ------------------------------ |
| Hosts needed  | 12                               | Use 4 bits: 2⁴ - 2 = 14 usable |
| Subnet mask   | /28                              | 255.255.255.240                |
| Network ID    | 192.168.10.112                   | -                              |
| Host IP range | 192.168.10.113 to 192.168.10.126 | -                              |
| Broadcast IP  | 192.168.10.127                   | -                              |

---

### Q11: Assume a class C network address 192.168.1.0 having subnet mask of 255.255.255.240. Find i) Number of sub-networks ii) Number of hosts per sub-net iii) Network ID for first three sub-nets iv) Usable IP addresses for first three sub-networks v) Broadcast address for first three networks (10 marks)

---

**Answer:**

#### Subnet Analysis for 192.168.1.0/28

**Given:**

- Network: 192.168.1.0
- Subnet mask: 255.255.255.240 (/28)
- Host bits: 4

#### Solutions

**i) Number of Sub-networks:**

- 2⁴ = **16 subnets**

**ii) Number of Hosts per Subnet:**

- 2⁴ = 16 total addresses
- 16 - 2 = **14 usable hosts**

**iii) Network IDs for First Three Subnets:**

1. 192.168.1.0
2. 192.168.1.16
3. 192.168.1.32

**iv) Usable IP Addresses:**

| **Subnet** | **Usable Host Range**        |
| ---------- | ---------------------------- |
| Subnet 1   | 192.168.1.1 to 192.168.1.14  |
| Subnet 2   | 192.168.1.17 to 192.168.1.30 |
| Subnet 3   | 192.168.1.33 to 192.168.1.46 |

**v) Broadcast Addresses:**

1. 192.168.1.15
2. 192.168.1.31
3. 192.168.1.47

---

### Q12: Assume an IP is represented as a.b.c.d/27 with CIDR representation then give the dotted decimal and binary representation of subnet mask and number host that can be in the network (CO2 L3, 5 marks)

---

**Answer:**

#### CIDR /27 Subnet Analysis

**Subnet Mask Representations:**

| **Format**         | **Value**                           |
| ------------------ | ----------------------------------- |
| **CIDR Notation**  | /27                                 |
| **Dotted Decimal** | 255.255.255.224                     |
| **Binary**         | 11111111.11111111.11111111.11100000 |

**Host Calculation:**

- **Host bits:** 32 - 27 = 5 bits
- **Total addresses:** 2⁵ = 32
- **Usable hosts:** 32 - 2 = **30 hosts**

**Breakdown:**

- Network address: 1 (not usable for hosts)
- Broadcast address: 1 (not usable for hosts)
- Available for hosts: 30

---

### Q13: A class C address of 200.168.10.0/24 has been allocated. NET-1, NET-2, and NET-3 have a WAN connection to NET-4. NET-1 requires 60 hosts, NET-2 requires 28 hosts, NET-3 and NET-4 each require 15 hosts. Calculate VLSM subnets and the respective hosts IP and Broadcast IP for all the FOUR cities (CO3 L3, 10 marks)

---

**Answer:**

#### VLSM Design for Four Networks

**NET-1 (60 hosts required)**

| **Parameter**     | **Value**                    |
| ----------------- | ---------------------------- |
| Subnet            | 200.168.10.0/26              |
| Network ID        | 200.168.10.0                 |
| Subnet Mask       | 255.255.255.192              |
| Total addresses   | 64                           |
| Usable hosts      | 62                           |
| Usable Host Range | 200.168.10.1 – 200.168.10.62 |
| Broadcast Address | 200.168.10.63                |

**NET-2 (28 hosts required)**

| **Parameter**     | **Value**                     |
| ----------------- | ----------------------------- |
| Subnet            | 200.168.10.64/27              |
| Network ID        | 200.168.10.64                 |
| Subnet Mask       | 255.255.255.224               |
| Total addresses   | 32                            |
| Usable hosts      | 30                            |
| Usable Host Range | 200.168.10.65 – 200.168.10.94 |
| Broadcast Address | 200.168.10.95                 |

**NET-3 (15 hosts required)**

| **Parameter**     | **Value**                      |
| ----------------- | ------------------------------ |
| Subnet            | 200.168.10.96/27               |
| Network ID        | 200.168.10.96                  |
| Subnet Mask       | 255.255.255.224                |
| Total addresses   | 32                             |
| Usable hosts      | 30                             |
| Usable Host Range | 200.168.10.97 – 200.168.10.126 |
| Broadcast Address | 200.168.10.127                 |

**NET-4 (15 hosts required)**

| **Parameter**     | **Value**                       |
| ----------------- | ------------------------------- |
| Subnet            | 200.168.10.128/27               |
| Network ID        | 200.168.10.128                  |
| Subnet Mask       | 255.255.255.224                 |
| Total addresses   | 32                              |
| Usable hosts      | 30                              |
| Usable Host Range | 200.168.10.129 – 200.168.10.158 |
| Broadcast Address | 200.168.10.159                  |

---

### Q14: Discuss subnetting with its advantages and disadvantages. How VLSM overcomes the limitation of fixed subnetting? (CO2 L2, 10 marks)

---

**Answer:**

#### What is Subnetting?

**Subnetting** is the process of dividing a network into multiple smaller networks.

#### Advantages of Subnetting

| **Advantage**                 | **Benefit**                                                        |
| ----------------------------- | ------------------------------------------------------------------ |
| **Divides Broadcast Domains** | Traffic routed efficiently; improves speed and network performance |
| **Reduces Congestion**        | Decreases load on the network                                      |
| **Enhanced Security**         | Different subnets can have different security policies             |
| **Efficient IP Usage**        | Better utilization of address space                                |

#### Disadvantages of Subnetting

- **Increased Complexity:** Network becomes harder to manage
- **Special Routing Required:** Needs subnet-aware routing algorithms

#### Fixed Subnetting Limitation

**Problem:** In fixed-length subnetting, **IP addresses are wasted** because:

- Host IPs from one subnet **cannot be used** in another subnet
- All subnets have same size regardless of need

#### How VLSM Overcomes This

**Variable Length Subnet Masking (VLSM):**

- **Flexible subnet sizes** based on actual host requirements
- Different number of bits in network ID portion
- Single network divided efficiently according to needs
- **Eliminates address waste**

**Example:**

- Subnet A needs 100 hosts → Use /25
- Subnet B needs 10 hosts → Use /28
- No wasted addresses between subnets

---

### Q15: Consider a class C IP address 193.1.2.129 & 193.1.2.67 and sub-net mask 255.255.255.192 i) Identify are they belongs to same network ii) Identify the Network ID for both the address (CO3 L4, 5 marks)

---

**Answer:**

#### Analysis

**Given:**

- Subnet Mask: 255.255.255.192 → **/26**
- IPs per subnet: **64**

#### IP Address Analysis

**IP 1: 193.1.2.129**

- Falls in range: **128 – 191**
- **Network ID: 193.1.2.128**

**IP 2: 193.1.2.67**

- Falls in range: **64 – 127**
- **Network ID: 193.1.2.64**

#### Answers

**i) Do they belong to same network?**

- **NO** — They belong to **different networks**
- Different network IDs (193.1.2.128 vs 193.1.2.64)

**ii) Network IDs:**

| **IP Address** | **Network ID** |
| -------------- | -------------- |
| 193.1.2.129    | 193.1.2.128    |
| 193.1.2.67     | 193.1.2.64     |

---

### Q16: Assume Node-A in WLAN is transmitting Packet-2 for the first time and Node-B is trying to transmit Packet-2 for the 2nd time. The packets from Node-A and Node-B collide with each other, find the contention window for both nodes and analyse which node gets the more chances for transmission by calculating the probability of collision and transmission for each node (CO3 L4, 10 marks)

---

**Answer:**

#### Contention Window Calculation

**Binary Exponential Backoff:**

| **Node**   | **Attempt** | **Contention Window (CW)** | **Possible K Values** |
| ---------- | ----------- | -------------------------- | --------------------- |
| **Node-A** | 1st attempt | (0, 1)                     | K ∈ {0, 1}            |
| **Node-B** | 2nd attempt | (0, 1, 2, 3)               | K ∈ {0, 1, 2, 3}      |

**Formula:** CW = 2^n - 1 (where n = attempt number)

#### Possible K Value Combinations

| **Node-A K** | **Node-B K** | **Outcome**   |
| ------------ | ------------ | ------------- |
| 0            | 0            | **Collision** |
| 0            | 1            | B waits       |
| 0            | 2            | B waits       |
| 0            | 3            | B waits       |
| 1            | 0            | A waits       |
| 1            | 1            | **Collision** |
| 1            | 2            | B waits       |
| 1            | 3            | B waits       |

**Total combinations:** 8 (2 × 4)

#### Probability Analysis

**Waiting Time:** WT = K × T_slot

**When K = 0:** Waiting time is 0 → immediate transmission

| **Metric**                      | **Node-A**  | **Node-B**  |
| ------------------------------- | ----------- | ----------- |
| **Transmission chances**        | 5 out of 8  | 1 out of 8  |
| **Probability of transmission** | 5/8 = 62.5% | 1/8 = 12.5% |
| **Collision probability**       | 2/8 = 25%   | 2/8 = 25%   |

**Conclusion:** **Node-A has significantly more chances** to transmit successfully because of its smaller contention window.

---

### Q17: With a neat diagram discuss the role of each protocol in Bluetooth protocol stack (CO1 L2, 10 marks)

---

**Answer:**

![Bluetooth Protocol Stack](attachments/Pasted%20image%2020260211203901.png)

**Refer to Diagram 1**

#### Bluetooth Protocol Stack Overview

The protocol stack **locates devices, connects them, and exchanges data**. It's divided into three logical groups:

#### 1. Transport Protocol Group

| **Layer**                       | **Role**                                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Radio Layer**                 | Physical transmission of radio signals                                                                  |
| **Baseband Layer**              | Connection establishment, addressing, packet format, timing, power control (equivalent to MAC sublayer) |
| **Link Manager Protocol (LMP)** | Power management, security management, link creation, monitoring, and termination                       |

#### 2. Middleware Protocol Group

**L2CAP (Logical Link Control and Adaptation Protocol):**

- Provides **connection-oriented (SCO)** and **connectionless (ACL)** data services
- Protocol multiplexing capability
- Segmentation and reassembly operations
- Distinguishes between upper layer protocols (SDP, RFCOMM, TCS)

**RFCOMM:**

- Serial port emulation
- Supports legacy applications

**SDP (Service Discovery Protocol):**

- Discovers available services

**TCS (Telephony Control Protocol):**

- Call control signaling for speech and data calls

#### 3. Host Controller Interface (HCI)

**Purpose:**

- Standardized communication between **host stack** (PC/mobile OS) and **controller** (Bluetooth IC)
- Access to hardware capabilities

**Common Interfaces:**

- **USB** (in PCs)
- **UART** (in mobile phones and PDAs)

#### 4. Application Group

Consists of applications using Bluetooth wireless links:

- Modem dialer
- Web-browsing client
- File transfer applications
- Audio streaming

---

### Q18: Discuss how Hidden terminal problem affect data transmission in WLAN with scenario. Demonstrate the steps to solve these problems (L3 CO3, 10 marks)

---

**Answer:**

![Hidden Terminal Problem](attachments/Pasted%20image%2020260211204112.png)

**Refer to Diagram 3**

#### What is Hidden Terminal Problem?

**Definition:** Occurs when a node (terminal) is within range of the Access Point (AP) or destination but **out of range** of another transmitting node communicating with the same AP/destination.

#### Scenario

**Setup:**

- **Station A** and **Station C** are transmitting stations
- **Station B** is the receiving station (e.g., AP)
- **A and C cannot hear each other** (hidden from each other)
- **Both are within range of B**

**Problem Sequence:**

1. **A transmits** data to B
2. **C cannot hear A's transmission** (A is hidden from C)
3. C senses medium as **idle**
4. **C starts its own transmission** to B
5. **Collision occurs at B** — data corrupted

#### Solution: RTS/CTS Handshake Mechanism

**Virtual Carrier Sense (IEEE 802.11):**

**Step 1: Request to Send (RTS)**

- Transmitting station (A) sends **RTS frame** to receiver (B)
- Includes **duration** of entire transmission (data + ACK)

**Step 2: Clear to Send (CTS)**

- Receiver (B) broadcasts **CTS frame**
- Includes remaining duration of transaction

**Step 3: NAV (Network Allocation Vector) Setting**

- Hidden node (C) hears the CTS from B
- CTS acts as **reservation signal**
- All nodes within B's range **refrain from transmitting** for specified duration
- C realizes transmission is ongoing and **holds off**

**Step 4: Data Transmission**

- A transmits data frame to B
- B responds with ACK
- **No collision** — problem solved

**Key Mechanism:** CTS broadcast ensures hidden nodes are aware of ongoing transmission.

---

### Q19: Demonstrate the steps involved in forming the piconet using Bluetooth technology (L3 CO2, 10 marks)

---

**Answer:**

#### What is a Piconet?

A **Piconet** is the fundamental network unit in Bluetooth — an **ad-hoc network** with:

- **One Master** device
- Up to **seven active Slave** devices

#### Formation Steps

**Step 1: Standby State**

- Initially, all devices are in **standby mode**

**Step 2: Device Discovery (Inquiry Procedure)**

**Purpose:** Master finds addresses and clock information of potential Slaves

| **Action**                | **Description**                                                           |
| ------------------------- | ------------------------------------------------------------------------- |
| **Master: INQUIRY**       | Transmits Inquiry Access Code (IAC) packets on 32 hop frequencies         |
| **Slave: INQUIRY SCAN**   | Potential Slaves listen for IAC packet                                    |
| **Slave: Response (FHS)** | Slave sends Frequency Hop Synchronization (FHS) packet                    |
| **FHS Contents**          | Contains Slave's BD_ADDR (Bluetooth Device Address) and Clock Information |
| **Master: Collection**    | Collects BD_ADDR and clock offset of all responding Slaves                |

**Step 3: Link Establishment (Page Procedure)**

**Purpose:** Master initiates direct connection to specific device

| **Action**            | **Description**                                                                        |
| --------------------- | -------------------------------------------------------------------------------------- |
| **Master: PAGE**      | Transmits packets addressed to target Slave's BD_ADDR                                  |
| **Slave: PAGE SCAN**  | Slave listens for messages specifically addressed to it                                |
| **Synchronization**   | Master and Slave exchange packets to synchronize clocks and frequency-hopping sequence |
| **Role Confirmation** | Device initiating Page becomes **Master**; responding device becomes **Slave**         |

**Step 4: Link Management (Connection State)**

**Result:**

- Devices are **logically connected**
- First **Master-Slave pair** formed
- Piconet is now established
- Additional slaves can be added (up to 7 active)

---

### Q20: Consider a spectrum of bandwidth 25 MHz, assume every user requires 30 KHz BW. If the city is split into hexagonal cells with 7 cells per cluster and there are 20 cells in the city, then find the numbers of users supported in the city (CO4 L3, 10 marks)

---

**Answer:**

#### Step-by-Step Calculation

**Step 1: Calculate Total Channels in the Spectrum**

**Given:**

- Total Bandwidth = 25 MHz = **25,000 KHz**
- Bandwidth per user = **30 KHz**

**Total channels:**

```
N_total = Total Bandwidth / BW per User
N_total = 25,000 KHz / 30 KHz
N_total ≈ 833.33
```

**Result:** **833 full channels** available in entire spectrum

**Step 2: Calculate Channels per Cell**

**Frequency Reuse Concept:**

- Cluster size (K) = **7 cells**
- Channels must be partitioned among 7 cells

**Channels per cell:**

```
N_cell = N_total / K
N_cell = 833 / 7
N_cell ≈ 119.05
```

**Result:** Each cell allocated **119 unique channels**

**Step 3: Calculate Total Users in City**

**Given:**

- Total cells in city = **20**
- Channels per cell = **119**

**Total users:**

```
N_city = N_cell × Total Cells
N_city = 119 × 20
N_city = 2,380
```

**Final Answer:** The city can support **2,380 users** simultaneously.

---

### Q21: Discuss how cellular networks supports the reusability of the same frequency by different users without interfering with each other. How clustering helps in avoiding co-channel interference (CO3 L2, 10 marks)

---

**Answer:**

#### Frequency Reuse in Cellular Networks

**Core Principle:** Limited radio frequency channels serve a large number of subscribers over a wide area through strategic reuse.

#### Cellular Structure Design

**1. Geographic Division:**

- Service area divided into **contiguous cells** (hexagons)
- Each cell has a **base station (tower)**

**2. Frequency Allocation:**

- Total spectrum (S) divided into **N channel groups**: F₁, F₂, …, F_N
- Each cell assigned **one channel group**
- **Adjacent cells always get different frequencies** (prevents immediate interference)

**3. Reuse Distance (D):**

- Same frequency group (F_i) **reused in another cell**
- Cells separated by **minimum reuse distance (D)**
- Distance ensures interference stays below acceptable threshold

#### Clustering and Co-Channel Interference

**What is a Cluster?**

- Group of **N adjacent cells**
- Each cell has **unique frequency group**
- Complete set of channels (F₁ to F_N) used **exactly once**
- Cluster replicated across service area

**Cluster Size Formula (Hexagonal Cells):**

```
N = I² + IJ + J²
```

Where I and J are integers

#### How Clustering Avoids Co-Channel Interference (CCI)

| **Parameter**           | **Effect on Interference**                    |
| ----------------------- | --------------------------------------------- |
| **Larger N**            | Greater distance (D) between co-channel cells |
| **Greater D**           | Lower interference levels                     |
| **Physical Separation** | Primary method to reduce CCI                  |

**Trade-off:**

- **Larger cluster size (N)** → Less interference BUT fewer channels per cell
- **Smaller cluster size (N)** → More channels per cell BUT more interference

---

### Q22: Discuss the methods to increase the capacity of the cellular network with their advantages and disadvantages (CO4 L2, 10 marks)

---

**Answer:**

#### Method 1: Cell Splitting (Cell Densification)

**Concept:** Subdivide congested large cells (macrocells) into smaller cells (microcells, picocells, femtocells), each with low-power base station.

**Advantages:**

| **Benefit**                       | **Details**                                                      |
| --------------------------------- | ---------------------------------------------------------------- |
| **Significant Capacity Increase** | Channels per area increases dramatically (4× when radius halved) |
| **Better S/I Ratio**              | Reduced power and radius = less co-channel interference          |
| **Improved Coverage**             | Shorter distance to BS = better signal strength                  |

**Disadvantages:**

| **Challenge**                | **Details**                                              |
| ---------------------------- | -------------------------------------------------------- |
| **High Infrastructure Cost** | Many new base stations, land, power, backhaul required   |
| **Increased Handoffs**       | Users cross boundaries more frequently = higher MSC load |
| **Power Control Complexity** | Mixed environments need careful management               |

#### Method 2: Cell Sectoring (Frequency Reuse Optimization)

**Concept:** Replace omnidirectional antenna with directional antennas (typically 3 sectors of 120° or 6 sectors of 60°).

**Advantages:**

| **Benefit**                         | **Details**                                                   |
| ----------------------------------- | ------------------------------------------------------------- |
| **Reduced Co-channel Interference** | Directional antennas focus transmission into smaller areas    |
| **Increased Capacity**              | Smaller reuse cluster size (N) possible = more frequent reuse |
| **Lower Cost than Splitting**       | No new base stations needed; only antennas and channel banks  |

**Disadvantages:**

| **Challenge**                     | **Details**                                        |
| --------------------------------- | -------------------------------------------------- |
| **Decreased Trunking Efficiency** | Smaller channel groups per sector = less efficient |
| **More Antennas**                 | More hardware at each site                         |
| **Increased Handoffs**            | Users moving between sectors                       |

#### Method 3: Microcell Zone Concept (Hybrid Approach)

**Concept:** Macrocell divided into small low-power "zones" all controlled by **single central base station**.

**Advantages:**

| **Benefit**              | **Details**                                          |
| ------------------------ | ---------------------------------------------------- |
| **Reduced Handoff Load** | Zone switch (not inter-cell handoff) = less MSC load |
| **Reduced Interference** | Radiation localized to active zone                   |
| **High Capacity**        | Retains benefits of densification                    |

**Disadvantages:**

| **Challenge**            | **Details**                                            |
| ------------------------ | ------------------------------------------------------ |
| **Complex Installation** | Multiple remote antennas + cabling/fiber to central BS |
| **High Initial Cost**    | Expensive cabling infrastructure in dense areas        |

---

### Q23: Assume the total spectrum allocated to cell phone service provider is 30 MHz and the total spectrum is divided into N channels with 30 KHz bandwidth. Find the number of users supported if the entire city is considered as one. If the city is split into hexagonal cells with 7 cells per cluster and there are 20 cells in the city, then find the numbers of users supported in the city (CO4 L3, 10 marks)

---

**Answer:**

#### Given Data

- Total Spectrum: **30 MHz = 30,000 KHz**
- Channel Bandwidth: **30 KHz**
- Cluster Size: **K = 7 cells**
- Total Cells in City: **20**

#### Part 1: Single Cell (Entire City)

**Calculate total channels:**

```
N = Total Spectrum / Channel Bandwidth
N = 30,000 KHz / 30 KHz
N = 1,000 channels
```

**Users Supported (Single Cell):**

- All channels available in one cell
- **Answer: 1,000 users**

#### Part 2: Cellular Network (7-cell cluster, 20 total cells)

**Calculate Channels per Cell:**

```
C_cell = N_total / Cluster Size (K)
C_cell = 1,000 / 7
C_cell ≈ 142 channels per cell
```

**Calculate Total Users in City:**

```
Total Users = C_cell × Total Cells (M)
Total Users = 142 × 20
Total Users = 2,840 users
```

#### Comparison

| **Configuration**                       | **Users Supported** |
| --------------------------------------- | ------------------- |
| **Single Cell**                         | 1,000               |
| **Cellular (7-cell cluster, 20 cells)** | 2,840               |

**Benefit of Cellular Design:** **2.84× increase** in capacity through frequency reuse!

---

### Q24: What do you mean by Inter-frame Spacing? Discuss the different types of IFS in 802.11 (CO1 L2, 10 marks)

---

**Answer:**

#### Inter-frame Spacing (IFS) Definition

**IFS** refers to **mandatory time intervals** that a wireless station must wait between transmission of consecutive frames in IEEE 802.11 (Wi-Fi).

**Purpose:**

- Controls channel access
- Provides priority mechanism
- Prevents collisions

#### Types of IFS in 802.11

**1. SIFS (Short Inter-frame Space)**

| **Characteristic** | **Details**                                            |
| ------------------ | ------------------------------------------------------ |
| **Duration**       | Shortest interval                                      |
| **Priority**       | Highest priority                                       |
| **Usage**          | High-priority transmissions (ACK, CTS, poll responses) |
| **Mechanism**      | Device responds immediately after receiving frame      |

**2. PIFS (PCF Inter-frame Space)**

| **Characteristic** | **Details**                                                 |
| ------------------ | ----------------------------------------------------------- |
| **Duration**       | Between SIFS and DIFS                                       |
| **Priority**       | Medium priority                                             |
| **Usage**          | Point Coordination Function (PCF) — contention-free periods |
| **Application**    | Real-time services, AP priority access                      |

**3. DIFS (DCF Inter-frame Space)**

| **Characteristic** | **Details**                                      |
| ------------------ | ------------------------------------------------ |
| **Duration**       | Longer than PIFS                                 |
| **Priority**       | Normal priority                                  |
| **Usage**          | Distributed Coordination Function (DCF) mode     |
| **Application**    | Standard frame transmission in contention period |

**4. EIFS (Extended Inter-frame Space)**

| **Characteristic** | **Details**                                            |
| ------------------ | ------------------------------------------------------ |
| **Duration**       | Longest of all IFS                                     |
| **Priority**       | Lowest                                                 |
| **Usage**          | Error recovery — when station receives corrupted frame |
| **Application**    | Resynchronization after incorrect MAC frame reception  |

**Priority Order:** SIFS < PIFS < DIFS < EIFS

---

### Q25: Assume two wireless devices A and C are 6 meters away from each other. Consider another wireless device B is in the middle of two devices A & C. A and C tries to transmit to device B simultaneously. In this scenario identify and represent (through diagram) the problem that arises. Propose a solution to the identified problem (10 marks)

---

**Answer:**

**Refer to Diagram 3**

#### Problem Identification

**Problem:** **Hidden Terminal Problem**

**Scenario:**

- Device A and Device C are **6 meters apart**
- Device B is in the **middle**
- **A and C cannot hear each other**
- Both try to transmit to B **simultaneously**
- **Collision occurs at B**

#### Solution: RTS-CTS Mechanism

**RTS (Request To Send):**

| **Component**   | **Details**                                                        |
| --------------- | ------------------------------------------------------------------ |
| **Sent by**     | Sender (A or C)                                                    |
| **Destination** | Intended receiver (B)                                              |
| **Contains**    | Receiver address, expected transmission duration                   |
| **Effect**      | All stations hearing RTS set their NAV (Network Allocation Vector) |

**CTS (Clear To Send):**

| **Component**   | **Details**                                                                      |
| --------------- | -------------------------------------------------------------------------------- |
| **Sent by**     | Receiver (B)                                                                     |
| **Destination** | Broadcast to all                                                                 |
| **Contains**    | Transmission duration                                                            |
| **Effect**      | All stations hearing CTS (including hidden nodes) set NAV and defer transmission |

**Step-by-Step Solution:**

1. **A sends RTS to B** (C may not hear)
2. **B sends CTS** (both A and C hear this)
3. **C receives CTS** and realizes B is busy
4. **C sets NAV** and waits
5. **A transmits data** to B without collision
6. **B sends ACK**
7. **NAV expires** — C can now attempt transmission

**Result:** Hidden terminal problem solved through virtual carrier sense mechanism.

---
