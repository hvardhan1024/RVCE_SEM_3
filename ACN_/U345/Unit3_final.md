# Unit 3: Mobile IP and Network Address Translation

---

## Mobile IP Introduction

### Understanding Mobility in Computing

**Mobility** refers to computing systems that allow devices to move from one location to another while maintaining network connectivity. This concept is fundamental to wireless technologies that enable movement across long distances at varying speeds.

**Key Challenge:** The primary issue with mobility is **IP addressing** - when a host moves from one network to another, traditional IP addressing schemes don't adapt automatically.

**Important Note:** In traditional networks, unplugging a desktop computer and connecting it to a different network requires complete IP reconfiguration. This manual process is impractical for mobile devices that frequently change networks.

---

## Limitations of Conventional IP for Mobility

![Limitation of Conventional IP](attachments/Pasted%20image%2020260211223455.png)

### The Core Problem

Traditional IP uses a **32-bit address** with two components:

| Component              | Purpose                                    | Problem with Mobility            |
| ---------------------- | ------------------------------------------ | -------------------------------- |
| **Network Identifier** | Identifies the subnet/network              | Remains tied to original network |
| **Host Identifier**    | Identifies specific device on that network | Doesn't update when device moves |

### Why Traditional IP Fails for Mobile Devices

**Scenario:** Consider a mobile node with IP address `10.6.6.1` attached to:

- **Subnet A:** `10.6.6.x` (Home Network)
- **Subnet B:** `10.6.15.x` (Foreign Network - where device moves)

**The Problem:**

1. Packets addressed to `10.6.6.1` are always routed to Subnet A
2. Even when the device moves to Subnet B, packets still go to Subnet A
3. The network portion (`10.6.6.x`) in the address directs routing decisions
4. Result: Packets never reach the mobile node at its new location

### Mobile IP Solution

**Mobile IP** is the IETF (Internet Engineering Task Force) solution that uses an **address redirection mechanism** to handle this mobility problem. It overcomes the limitations of the original IP addressing scheme by:

- Maintaining a permanent home address
- Using temporary care-of addresses when away from home
- Implementing tunneling to forward packets to current location

---

## Mobile IP Characteristics

### Essential Features

| Characteristic     | Description                                                                        | Benefit                                                                                       |
| ------------------ | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Transparency**   | Mobility is invisible to applications, transport protocols, and uninvolved routers | Seamless user experience - no difference between wireless and wired domains                   |
| **Scalability**    | Works across large internets including the global Internet                         | Supports millions of mobile users worldwide                                                   |
| **Security**       | Built-in authentication mechanisms                                                 | All messages can be verified and secured                                                      |
| **Compatibility**  | Mobile hosts work with stationary IPv4 hosts and other mobile hosts                | No special software needed on correspondent nodes                                             |
| **Macro Mobility** | Focuses on long-duration moves                                                     | Ideal for users who move and stay at new location for extended periods (e.g., business trips) |

**Use Case Example:** A user taking a laptop on a week-long business trip and connecting to hotel Wi-Fi - Mobile IP handles this scenario perfectly.

---

## Mobile IP Entities

### Core Components

#### 1. Mobile Node (MN)

**Definition:** The device requiring mobility support - can be a mobile terminal (phone, laptop) or mobile router.

**Characteristics:**

- Changes point of attachment between networks
- Assigned a **permanent IP address** (Home Address)
- Other hosts always send packets to this home address
- MN's location is transparent to correspondents

#### 2. Home Network

**Definition:** The network where the Mobile Node originally belongs based on its assigned IP address.

**Key Points:**

- The subnet to which MN's IP address naturally belongs
- Contains the Home Agent
- Where the MN is "normally" located

#### 3. Home Agent (HA)

**Definition:** Router in the home network responsible for the Mobile Node.

**Responsibilities:**

- Located on MN's home network
- Binds MN's home address with its Care-of Address (COA)
- Forwards packets to appropriate network when MN is away
- Uses **encapsulation** (tunneling) to redirect packets

#### 4. Foreign Network

**Definition:** The current network where MN is located when away from home.

**Characteristics:**

- Temporary network attachment point
- Provides connectivity through Foreign Agent
- Different subnet from home network

#### 5. Foreign Agent (FA)

**Definition:** Router in foreign network currently serving the Mobile Node.

**Functions:**

- Receives packets from Home Agent via tunnel
- Delivers packets to Mobile Node
- Provides Care-of Address for MN

#### 6. Correspondent Node (CN)

**Definition:** Any device on the internet communicating with the Mobile Node.

**Can be:**

- Fixed node (web server, database server)
- Another mobile node
- Examples: Web browser accessing services, email client

---

## Mobile IP Addressing

### Primary Address (Home Address)

| Aspect         | Details                                      |
| -------------- | -------------------------------------------- |
| **Nature**     | Permanent and fixed                          |
| **Usage**      | Used by applications and transport protocols |
| **Assignment** | Obtained from original home network          |
| **Stability**  | Never changes regardless of location         |

### Secondary Address (Care-of Address - COA)

| Aspect          | Details                                     |
| --------------- | ------------------------------------------- |
| **Nature**      | Temporary - changes with location           |
| **Validity**    | Only valid while visiting specific location |
| **Acquisition** | Obtained when moving to foreign network     |
| **Purpose**     | Sent to Home Agent for packet redirection   |

---

## Types of Care-of Addresses

### 1. Foreign Agent Care-of Address (FA-COA)

**Definition:** The Foreign Agent's IP address used as COA.

**Characteristics:**

| Feature             | Description                                     |
| ------------------- | ----------------------------------------------- |
| **Ownership**       | FA's IP address                                 |
| **Tunnel Endpoint** | Foreign Agent receives and decapsulates packets |
| **Sharing**         | Multiple MNs can share same FA-COA              |
| **Discovery**       | MN discovers FA through agent advertisements    |
| **Delivery**        | FA forwards packets to MN on local network      |

**Process:**

1. MN discovers Foreign Agent identity
2. Contacts FA to obtain COA
3. FA provides its IP address as COA
4. Multiple mobile nodes can use same FA address

### 2. Co-located Care-of Address (Co-located COA)

**Definition:** Temporary IP address acquired by the Mobile Node itself.

**Characteristics:**

| Feature             | Description                                      |
| ------------------- | ------------------------------------------------ |
| **Ownership**       | Belongs to MN temporarily                        |
| **Tunnel Endpoint** | Mobile Node itself                               |
| **Address Type**    | Topologically correct for foreign network        |
| **Acquisition**     | Often obtained via DHCP                          |
| **Dual Addressing** | MN uses both home address and COA simultaneously |

**How It Works:**

- Applications use home address (upper layers)
- Lower layer software uses COA for packet receipt
- MN handles all forwarding and decapsulation

**Advantages:**

- Works with existing internet infrastructure
- No special Foreign Agent required
- Foreign routers treat MN as regular fixed node
- Address allocation uses standard mechanisms (DHCP)

**Disadvantages:**

- Requires extra software on mobile device
- MN must handle address management
- More complex configuration

---

## Mobile IP Entities - Working Overview

![Mobile IP Working](attachments/Pasted%20image%2020260211223636.png)

### Important Design Considerations

**Intended Use Case:**

- Mobile IP designed for **infrequent moves**
- Device remains at given location for **relatively long periods**
- Not optimized for constant movement

**Key Innovation:**

- Allows host to retain its home address
- Eliminates need for routers to learn host-specific routes
- Achieved by allowing computer to hold **two addresses simultaneously**

---

## Mobile IP Operation Overview

![Mobile IP Operation](attachments/Pasted%20image%2020260211223740.png)

### Step-by-Step Operation

#### Phase 1: Movement Detection and Registration

**When MN moves to foreign network:**

1. **Obtains Secondary Address (COA)**
   - Through Foreign Agent advertisement
   - Or via DHCP for co-located COA

2. **Sends COA to Home Agent**
   - Registration request message
   - Contains COA and security credentials

3. **Home Agent Setup**
   - Accepts and stores COA
   - Prepares to intercept packets

#### Phase 2: Communication from Correspondent Node

**Path 1: CN to HA (Original Route)**

| Step                | Details                                          |
| ------------------- | ------------------------------------------------ |
| **CN sends packet** | Destination: MN's home address (130.103.202.200) |
| **Packet routing**  | Standard IP routing to home network              |
| **HA receives**     | Home Agent (130.103.202.050) intercepts packet   |

**Path 2: HA to FA (Tunneling)**

| Step                | Details                                                |
| ------------------- | ------------------------------------------------------ |
| **Tunnel creation** | HA establishes tunnel to FA                            |
| **Encapsulation**   | Original packet wrapped in new IP packet               |
| **New headers**     | Source: HA address, Destination: COA (130.111.111.111) |
| **Transmission**    | Sent through internet to Foreign Agent                 |

**Path 3: FA to MN (Local Delivery)**

| Step               | Details                         |
| ------------------ | ------------------------------- |
| **FA receives**    | Packet arrives at Foreign Agent |
| **De-capsulation** | FA removes outer IP header      |
| **Local delivery** | Original packet delivered to MN |

#### Phase 3: Reply from Mobile Node

**Direct Path: MN to CN**

| Step                 | Details                                    |
| -------------------- | ------------------------------------------ |
| **MN creates reply** | Uses home address as source                |
| **Sends to FA**      | FA acts as default router                  |
| **Direct routing**   | **FA sends directly to CN** (no tunneling) |
| **Efficiency**       | Bypasses Home Agent completely             |

**Efficiency Note:** Return path is optimized - packets don't need to traverse home network.

---

## Mobile IP Support Services

### Four Essential Services

1. **Agent Discovery**
2. **Registration**
3. **Encapsulation**
4. **De-capsulation**

---

## Agent Discovery

### Purpose and Mechanism

**Goal:** Mobile Node must find Foreign Agents when moving to new networks.

**Technology Used:** **ICMP Router Discovery** mechanism (extended for Mobile IP)

### How It Works

#### Standard Router Behavior

| Message Type             | Purpose                      | Frequency            |
| ------------------------ | ---------------------------- | -------------------- |
| **Router Advertisement** | Routers announce presence    | Periodic broadcasts  |
| **Router Solicitation**  | Hosts request advertisements | On-demand from hosts |

#### Mobile IP Extension

**Mobility Agent Extension:** Additional information appended to router advertisements

**Detection Method:**

- MN compares IP header length with ICMP message length
- If IP datagram length > ICMP message length → Extension present
- Extension contains Mobile IP capabilities

---

## Mobility Agent Advertisement Extension (MAE)

![MAE Message Format](attachments/Pasted%20image%2020260211223842.png)

### Message Structure

| Field                     | Size                    | Description                                                                                                                          |
| ------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Type**                  | 1 byte                  | Value: **16** (identifies MAE)                                                                                                       |
| **Length**                | 1 byte                  | Extension length in bytes (excludes Type and Length fields)<br/>Formula: 6 + (4 × number of COAs)                                    |
| **Sequence Number**       | 2 bytes                 | Counter starting at 0, incremented for each advertisement<br/>Helps detect lost/replayed messages                                    |
| **Registration Lifetime** | 2 bytes                 | Maximum registration duration (seconds) agent will accept<br/>**0xFFFF** = infinite lifetime<br/>**0** = not accepting registrations |
| **Flags**                 | 1 byte                  | Single-bit flags: R, B, H, F, M, G, r, T                                                                                             |
| **Reserved**              | 1 byte                  | Must be zero (future use)                                                                                                            |
| **Care-of Address(es)**   | Variable (4 bytes each) | Zero or more COA addresses<br/>FA must provide at least one<br/>HA omits this field                                                  |

### Flag Definitions

| Flag  | Name                  | Meaning                                    |
| ----- | --------------------- | ------------------------------------------ |
| **R** | Registration Required | MN must register even on home network      |
| **B** | Busy                  | Agent too busy to accept new registrations |
| **H** | Home Agent            | This router can serve as Home Agent        |
| **F** | Foreign Agent         | This router can serve as Foreign Agent     |
| **M** | Minimal Encapsulation | Supports minimal encapsulation             |
| **G** | GRE Encapsulation     | Supports GRE encapsulation                 |
| **r** | Reserved              | Reserved for future use                    |
| **T** | Reverse Tunneling     | Supports reverse tunneling                 |

---

## Agent Registration Process

### Overview

**Purpose:** Mobile Node registers its Care-of Address with Home Agent when away from home.

**Protocol:** Registration messages sent via **UDP** to well-known port **434**

### Registration Scenarios

#### Scenario 1: Foreign Agent COA

![Agent Registration](attachments/Pasted%20image%2020260211223958.png)

**Process Flow:**

| Step  | Sender | Receiver | Message                     | Action                             |
| ----- | ------ | -------- | --------------------------- | ---------------------------------- |
| **1** | MN     | FA       | Registration Request (RREQ) | MN discovers FA and sends request  |
| **2** | FA     | HA       | RREQ (relayed)              | FA forwards request with COA to HA |
| **3** | HA     | FA       | Registration Reply (RREP)   | HA confirms registration           |
| **4** | FA     | MN       | RREP (forwarded)            | Registration complete              |

#### Scenario 2: Co-located COA

**Process Flow:**

| Step  | Sender | Receiver | Message       | Action                            |
| ----- | ------ | -------- | ------------- | --------------------------------- |
| **1** | MN     | HA       | RREQ (direct) | MN performs registration directly |
| **2** | HA     | MN       | RREP (direct) | No Foreign Agent involved         |

**Note:** MN uses co-located address to communicate directly with Home Agent.

---

## Agent Registration Message Format

![Registration Message](attachments/Pasted%20image%2020260211224044.png)

### Registration Request Fields

| Field                       | Size     | Description                                                   |
| --------------------------- | -------- | ------------------------------------------------------------- |
| **Type**                    | 1 byte   | **1** = Registration Request<br/>**3** = Registration Reply   |
| **Flags (S,B,D,M,G,R,T,X)** | 1 byte   | Various request options                                       |
| **Lifetime**                | 2 bytes  | Requested registration duration (seconds)                     |
| **Home Address**            | 4 bytes  | MN's permanent IP address<br/>Uniquely identifies device      |
| **Home Agent**              | 4 bytes  | IP address of MN's Home Agent                                 |
| **Care-of Address**         | 4 bytes  | Current temporary IP address<br/>(FA's address or co-located) |
| **Identification**          | 8 bytes  | Timestamp/nonce for replay protection                         |
| **Extensions**              | Variable | **Authentication** (mandatory)<br/>Other optional extensions  |

### Key Flag Definitions

| Flag  | Meaning                                  |
| ----- | ---------------------------------------- |
| **S** | Simultaneous bindings allowed            |
| **B** | Broadcast datagrams to MN                |
| **D** | Decapsulation by MN (co-located COA)     |
| **M** | Minimal encapsulation requested          |
| **G** | GRE encapsulation requested              |
| **R** | Registration required (sent by HA to MN) |
| **T** | Reverse tunneling requested              |
| **X** | Reserved                                 |

---

## Communication Between MN and Foreign Agent

### The Challenge

**Problem:** When using FA-COA, the Mobile Node doesn't have a unique IP address on the foreign network. How can FA and MN communicate?

### The Solution

**Special Addressing Rules:**

| Direction   | Source Address    | Destination Address | ARP Used? |
| ----------- | ----------------- | ------------------- | --------- |
| **MN → FA** | MN's home address | FA's address        | No        |
| **FA → MN** | FA's address      | MN's home address   | No        |

**Important:** Foreign Agent **cannot use ARP** for MN's home address (it's not valid on local network).

### Address Binding Without ARP

**Mechanism:**

1. **During Registration:**
   - FA records MN information when registration request arrives
   - Stores: Home address, MAC address, other details
   - Maintains this binding during communication session

2. **For Packet Delivery:**
   - FA refers to stored information
   - Uses **hardware unicast** to MAC address
   - Bypasses ARP entirely

**Result:** FA can send datagrams to MN by referencing stored MAC address mapping, even though standard ARP isn't used.

---

## The Two-Crossing Problem (2X Problem)

![Two Crossing Problem](attachments/Pasted%20image%2020260211224115.png)

### Problem Description

**Also Known As:** Double Crossing Problem, 2X Problem

**Major Disadvantage:** Inefficient routing when MN communicates with nodes on same network.

### The Scenario

**Setup:**

- Mobile Node (MN) on foreign network
- Destination Host (D) also on same foreign network
- Both connected to same local infrastructure

### Traffic Flow Analysis

#### Outgoing Traffic (MN → D): Efficient ✓

| Step          | Path        | Efficiency                |
| ------------- | ----------- | ------------------------- |
| MN sends to D | MN → R4 → D | **Direct local delivery** |
| Hops          | Minimal     | ✓ Optimal                 |

#### Incoming Traffic (D → MN): Inefficient ✗

| Step          | Path                                       | Problem                    |
| ------------- | ------------------------------------------ | -------------------------- |
| D sends to MN | D → R3 → Internet → Home Network (R1)      | Uses MN's home address     |
| HA intercepts | R1 (Home Agent) receives packet            | Far from actual location   |
| Tunneling     | R1 → Internet → Foreign Network → FA or MN | **Crosses internet twice** |

### Why This Happens

**Root Cause:** MN uses its **Home Address** for all communication, even when in Foreign Network.

**Result:**

1. Datagrams from D contain MN's home address as destination
2. Standard IP routing sends packets to home network
3. Home Agent must tunnel packets back across internet
4. Packet crosses internet infrastructure twice

### The Cost

**Impact:**

- **Crossing internet >> Local delivery** (much more expensive)
- Increased latency
- Wasted bandwidth
- Higher operational costs

### Solutions

**Partial Solution:** Route Optimization

- Correspondent nodes learn MN's COA
- Send packets directly to foreign network
- Requires host-specific routes

**Limitation:** Problem remains for destinations without the host-specific route.

---

## Communication With Computers on Home Network

### The Proxy ARP Solution

#### The Challenge

**Scenario:** A host on MN's home network wants to send datagram to MN.

**Normal Behavior:**

- IP specifies **direct delivery** over local network
- Sender will **not forward to router**
- Instead: Sender ARPs for MN's hardware address
- Encapsulates datagram and transmits on local network

**Problem:** If MN has moved, it's not on local network to respond to ARP!

#### Home Agent's Role

**Solution: Proxy ARP**

| Responsibility              | Action                                            |
| --------------------------- | ------------------------------------------------- |
| **Monitor ARP requests**    | HA listens for ARP requests targeting MN          |
| **Respond on behalf of MN** | HA answers with **its own** hardware address      |
| **Intercept datagrams**     | Local hosts send datagrams to HA thinking it's MN |
| **Forward to MN**           | HA tunnels packets to MN's current location       |

**Key Feature:** **Completely transparent** to local computers

- Local systems receive ARP reply (from HA)
- Forward datagram as usual (to HA's MAC address)
- Don't know MN has moved
- No special configuration needed

---

## Private and Hybrid Networks

### The Privacy Challenge

**Problem with Single-Level Internet:** Lack of privacy

- Organizations with multiple sites
- Private data may pass through other organizations' networks
- Internet is public infrastructure
- Security concerns for sensitive data

### Two-Level Internet Architecture

**Purpose:** Distinguish between internal and external communication

| Traffic Type           | Description                                     | Privacy          |
| ---------------------- | ----------------------------------------------- | ---------------- |
| **Internal Datagrams** | Communication between same organization's sites | Private          |
| **External Datagrams** | Communication between different organizations   | Through internet |

**Goal:** Keep internal datagrams private while enabling external communication.

---

## Private Network

### Architecture

**Definition:** Organization's own TCP/IP internet completely separate from global Internet.

### Components

- **Routers:** Organization-owned and operated
- **Connections:** Leased digital circuits between sites
- **Addressing:** Can use arbitrary IP addresses (no conflict with internet)

### Advantages

| Benefit              | Description                               |
| -------------------- | ----------------------------------------- |
| **Complete Privacy** | All data remains within organization      |
| **Security**         | No outsider access to any part of network |
| **Address Freedom**  | Use any IP addressing scheme              |
| **Control**          | Full administrative control               |

### Disadvantages

| Challenge              | Impact                                 |
| ---------------------- | -------------------------------------- |
| **No Internet Access** | Cannot reach external resources        |
| **High Cost**          | Expensive leased lines between sites   |
| **Limited Reach**      | Only connects organization's locations |

---

## Hybrid Network

![Hybrid Network](attachments/Pasted%20image%2020260211224141.png)

### Architecture

**Definition:** Combines advantages of private networking with global Internet connectivity.

### Setup

| Component                  | Configuration                  |
| -------------------------- | ------------------------------ |
| **IP Addresses**           | Uses globally valid addresses  |
| **Site Connections**       | Each site connects to Internet |
| **Internal Communication** | Private, encrypted tunnels     |
| **External Access**        | Direct Internet connectivity   |

### Advantages

| Benefit             | Description                             |
| ------------------- | --------------------------------------- |
| **Internet Access** | All computers can reach global Internet |
| **Privacy**         | Internal communication remains secure   |
| **Flexibility**     | Best of both worlds                     |

### Disadvantages

| Challenge      | Impact                                          |
| -------------- | ----------------------------------------------- |
| **Cost**       | Still requires leased lines for private traffic |
| **Complexity** | More complex to configure and manage            |
| **Management** | Need to balance public and private routing      |

---

## Virtual Private Networks (VPN)

### Purpose

**Main Goal:** Use global Internet to connect organization's sites while keeping data private.

**Cost Advantage:** Eliminates expensive leased lines of private/hybrid networks.

### Core Technologies

#### 1. Tunneling

**Definition:** Encapsulation technique to create private path across public network.

**How It Works:**

- Define tunnel between routers at different sites
- Use **IP-in-IP encapsulation**
- Packets traverse global Internet in encapsulated form
- Destination router decapsulates and delivers

#### 2. Encryption

**Purpose:** Protect data confidentiality across public internet.

**Process:**

- Encrypt outgoing datagram before encapsulation
- Receiving router decrypts after receiving
- Outsiders cannot decode without encryption key

---

## VPN Encapsulation

![VPN IP-in-IP Encapsulation](attachments/Pasted%20image%2020260211224205.png)

### Structure

**Complete Encryption:** Entire inner datagram (including header) is encrypted before encapsulation.

| Layer                 | Component                  | Encrypted?                        |
| --------------------- | -------------------------- | --------------------------------- |
| **Outer IP Header**   | Public routing information | No (must be readable for routing) |
| **Encrypted Payload** | Inner IP header + data     | Yes (completely encrypted)        |

### Operation

**Sending (Encapsulation):**

1. Router receives internal datagram
2. Encrypts entire datagram (header + data)
3. Encapsulates in new outer IP datagram
4. Sends across internet through tunnel

**Receiving (Decapsulation):**

1. Router receives tunneled packet
2. Recognizes as VPN tunnel traffic
3. Decrypts data area
4. Reproduces original inner datagram
5. Forwards based on inner destination

**Security:** Even though outer datagram traverses public internet, content remains secure - outsiders lack encryption key.

---

## VPN Addressing and Routing

![VPN Routing](attachments/Pasted%20image%2020260211224342.png)

### Routing Table Structure

**VPN Tunnel as Circuit Replacement:**

- Each tunnel replaces what would be a leased circuit
- Routing table contains explicit routes for organizational destinations
- Tunnels used instead of physical leased lines

### Example Scenario

**Network Setup:**

- **Site 1:** Network 128.10.2.0
- **Site 2:** Network 128.210.0.0
- **Routers:** R1 (Site 1), R3 (Site 2)
- **Intermediate:** R2 (local router)

### Packet Flow

**Scenario:** Computer on `128.10.2.0` sends to computer on `128.210.0.0`

| Step   | Location        | Action                                                       |
| ------ | --------------- | ------------------------------------------------------------ |
| **1**  | Source          | Sends datagram to R2 (local router)                          |
| **2**  | R2              | Forwards to R1 (site border router)                          |
| **3**  | R1              | Consults routing table<br/>Destination requires tunnel to R3 |
| **4**  | R1 Encrypts     | Encrypts original datagram                                   |
| **5**  | R1 Encapsulates | Wraps in outer datagram (Destination: R3)                    |
| **6**  | Internet        | Outer datagram routed through ISP to R3                      |
| **7**  | R3 Receives     | Recognizes tunnel from R1                                    |
| **8**  | R3 Decrypts     | Decrypts to produce original datagram                        |
| **9**  | R3 Routes       | Looks up destination in routing table                        |
| **10** | R3 Forwards     | Sends to R4 for local delivery                               |

**Key Point:** Encryption/decryption and encapsulation/decapsulation are transparent to end hosts.

---

## VPN With Private Addresses

![VPN Private Addressing](attachments/Pasted%20image%2020260211224434.png)

### Concept

**Scenario:** Hosts don't need general Internet connectivity - only VPN communication.

**Solution:** Use private IP addresses internally, globally valid addresses only for tunneling.

### Addressing Scheme

| Network    | Internal Addresses    | External Address                |
| ---------- | --------------------- | ------------------------------- |
| **Site 1** | 10.1.0.0/16 (private) | One global IP for R1 connection |
| **Site 2** | 10.2.0.0/16 (private) | One global IP for R2 connection |

### Configuration

**Internal Routing:**

- Routing tables specify routes for private addresses
- Only VPN tunneling software uses global IP addresses

**External Access:**

- Only two globally valid addresses needed
- One for R1's internet connection
- One for R2's internet connection

### Advantages

| Benefit                  | Description                                 |
| ------------------------ | ------------------------------------------- |
| **Address Conservation** | Minimal use of global IP addresses          |
| **Privacy**              | Private addresses not routable on internet  |
| **Security**             | Additional layer of address obscurity       |
| **Simplicity**           | Internal addressing independent of internet |

### Limitation

**Cannot Provide General Internet Access:** For hosts to access internet services, must use hybrid architecture with valid IP addresses.

---

## Application Gateway (Application-Level Firewall)

### Purpose

**Problem Solved:** Allow multiple computers at organization to access Internet **without assigning globally valid IP addresses**.

**Key Difference:** Provides **service-level access**, not IP-level access.

### Architecture

#### Multi-Homed Host Configuration

| Component                | Connection                   | IP Address                |
| ------------------------ | ---------------------------- | ------------------------- |
| **External Interface**   | Connected to Internet        | Global IP                 |
| **Internal Interface**   | Connected to Intranet        | Private IP                |
| **Application Gateways** | Software on multi-homed host | Handles specific services |

### How It Works

**Process Flow:**

1. **Internal Host Request:**
   - Host sends request to appropriate gateway on multi-homed host
   - Uses private IP addressing

2. **Gateway Processing:**
   - Gateway receives request
   - Accesses service on Internet (using global IP)
   - Retrieves information

3. **Response Relay:**
   - Gateway sends response back
   - Delivered across internal network
   - Reaches original requesting host

### Service Handling

**One Gateway Per Service:**

| Service  | Gateway Software      | Port |
| -------- | --------------------- | ---- |
| **HTTP** | Web proxy gateway     | 80   |
| **SMTP** | Email gateway         | 25   |
| **FTP**  | File transfer gateway | 21   |
| **DNS**  | DNS proxy gateway     | 53   |

### Advantages

| Benefit                       | Description                             |
| ----------------------------- | --------------------------------------- |
| **No Infrastructure Changes** | Works with existing network             |
| **Address Conservation**      | No global IPs needed for internal hosts |
| **Centralized Control**       | All traffic through controlled gateway  |
| **Enhanced Security**         | Gateway can filter/monitor traffic      |

### Disadvantages

| Limitation                  | Impact                               |
| --------------------------- | ------------------------------------ |
| **Service-Specific**        | Each service needs separate gateway  |
| **Multiple Gateways**       | Complex management for many services |
| **Single Point of Failure** | Gateway down = service unavailable   |
| **Performance Bottleneck**  | All traffic through one point        |

---

## Network Address Translation (NAT)

### Overview

**Definition:** Technology providing IP-level access between hosts with private addresses and the Internet, without requiring globally valid IP addresses for each host.

**Requirements:**

- Single connection to global Internet
- At least **one globally valid IP address**
- NAT software/hardware

### NAT Box

**What It Is:** Computer or router with NAT software/hardware.

**Function:** All traffic (incoming/outgoing) passes through NAT box.

### Operations

![NAT Operation](attachments/Pasted%20image%2020260211224549.png)

| Direction    | Address Translation                                       |
| ------------ | --------------------------------------------------------- |
| **Outgoing** | Replace source address (private) → global IP              |
| **Incoming** | Replace destination (global IP) → correct private address |

**How Hosts See NAT:**

- **Internal Hosts:** NAT box appears as router to global Internet
- **External Hosts:** All internal hosts appear to share same global IP

---

## NAT Advantages

### Key Benefits

| Advantage             | Description                                         | Impact                    |
| --------------------- | --------------------------------------------------- | ------------------------- |
| **Generality**        | Any internal host can access any Internet service   | Complete flexibility      |
| **Transparency**      | Internal hosts use private (non-routable) addresses | No reconfiguration needed |
| **IPv4 Conservation** | Dramatically reduces need for public IPs            | Extends IPv4 lifespan     |
| **Security**          | Internal addresses hidden from Internet             | Additional security layer |

---

## NAT Translation Table

### The Challenge

**Question:** How does NAT know which internal host should receive incoming datagrams from Internet?

**Answer:** NAT maintains a **translation table** mapping external to internal addresses.

### Table Structure

| Inside Local IP                  | Inside Local Port   | Inside Global IP | Inside Global Port | Outside Global IP  | Outside Port |
| -------------------------------- | ------------------- | ---------------- | ------------------ | ------------------ | ------------ |
| Private address of internal host | Source port on host | Public IP of NAT | Translated port    | Internet server IP | Service port |

### Table Entry Components

**Each Entry Specifies:**

1. **IP address of host on Internet** (correspondent)
2. **Internal IP address of host at site** (private)
3. **Port mapping information** (for NAPT)

---

## NAT Table Creation

![NAT Table Creation](attachments/Pasted%20image%2020260211224640.png)

### How Translation Tables Are Built

#### Method 1: Manual Initialization

**Process:**

- Network manager configures table manually
- Before any communication occurs
- Static mappings

**Best For:**

- Servers that need consistent addressing
- Services accessed from Internet

#### Method 2: Outgoing Datagrams (Dynamic)

**Process:**

1. NAT receives datagram from internal host
2. Creates table entry automatically
3. Records:
   - Internal host address
   - Destination address
   - Port mapping (if NAPT)

**Trigger:** Side-effect of sending datagrams

**Best For:**

- Client applications
- Outgoing connections
- Most common scenario

#### Method 3: Incoming Name Lookups

**Process:**

1. External host performs DNS lookup for internal host
2. DNS software creates NAT table entry
3. Responds with global address (G)

**Result:**

- From outside, all hosts at site appear to map to address G
- Table ready when actual communication begins

**Best For:**

- Services with DNS names
- Predictive table population

---

## IP Masquerading

### Definition

**IP Masquerading:** Technique hiding entire IP address space (usually private) behind single IP address in another (usually public) address space.

### How It Works

**Process:**

1. Hidden addresses (private) changed to single public IP
2. Appears as source address of outgoing packets
3. Packets seem to originate from routing device, not hidden host

### Popularity

**Result of Technique's Popularity:**

- "NAT" has become virtually synonymous with "IP Masquerading"
- Most common NAT deployment type
- Critical for IPv4 address conservation

---

## Types of Addresses in Network

![Address Types in NAT](attachments/Pasted%20image%2020260211224716.png)

### NAT Terminology

| Address Type               | Definition                                                          | Example                        |
| -------------------------- | ------------------------------------------------------------------- | ------------------------------ |
| **Inside Local Address**   | IP assigned to host on inside (local) network                       | 10.0.0.5, 192.168.1.100        |
| **Inside Global Address**  | IP representing inside local addresses to outside world             | Public IP of NAT box           |
| **Outside Local Address**  | Actual IP of destination host in local network after translation    | May be same as outside global  |
| **Outside Global Address** | Outside host as seen from outside network<br/>IP before translation | 128.10.19.20 (Internet server) |

### Perspective Matters

**Inside Local:** What address **is** inside your network
**Inside Global:** What address **appears as** from outside
**Outside Local:** What outside address **appears as** inside
**Outside Global:** What address **is** on the outside

---

## Multi-Address NAT

### Concept

**Problem:** Single global IP allows only one internal host per external destination at a time.

**Solution:** Assign NAT box **N global IP addresses**.

### How It Works

**Process:**

| Connection Order | Internal Host | Destination | NAT Assigns | Table Entry |
| ---------------- | ------------- | ----------- | ----------- | ----------- |
| First            | Host A        | Server X    | G₁          | A ↔ G₁ ↔ X  |
| Second           | Host B        | Server X    | G₂          | B ↔ G₂ ↔ X  |
| Third            | Host C        | Server X    | G₃          | C ↔ G₃ ↔ X  |
| Nth              | Host N        | Server X    | Gₙ          | N ↔ Gₙ ↔ X  |

### Advantages

**Capacity:** Up to **N internal hosts** can access same destination **concurrently**

**Use Case:**

- Enterprise environments
- Multiple users accessing same popular services
- Improved performance over single address NAT

---

## Types of NAT

### 1. Static NAT (One-to-One NAT)

![Static NAT](attachments/Pasted%20image%2020260211224738.png)

**Definition:** Single private IP permanently mapped to single public IP.

#### Characteristics

| Feature                | Description                   |
| ---------------------- | ----------------------------- |
| **Mapping Type**       | Permanent one-to-one          |
| **Address Assignment** | Pre-configured, never changes |
| **Table Type**         | Static entries                |

#### When to Use

**Best For:**

- Web servers
- Mail servers
- FTP servers
- Any device requiring direct Internet access

#### Advantages

| Benefit            | Description                                          |
| ------------------ | ---------------------------------------------------- |
| **Predictable**    | Each private host always maps to same public IP      |
| **Direct Access**  | External hosts can directly access internal services |
| **Simple**         | Easy to configure and maintain                       |
| **No Port Issues** | Full end-to-end connectivity preserved               |

#### Disadvantages

| Limitation         | Impact                                   |
| ------------------ | ---------------------------------------- |
| **Address Waste**  | Bandwidth wasted if mapped host inactive |
| **No Scalability** | Cannot have more hosts than public IPs   |
| **Cost**           | Requires multiple public IP addresses    |
| **Inflexible**     | Cannot dynamically reassign addresses    |

---

### 2. Dynamic NAT

![Dynamic NAT](attachments/Pasted%20image%2020260211224808.png)

**Definition:** Multiple private IPs mapped to pool of public IPs, with mappings created dynamically as needed.

#### Characteristics

| Feature                | Description                       |
| ---------------------- | --------------------------------- |
| **Mapping Type**       | One-to-one, but dynamic selection |
| **Address Assignment** | From pool, assigned on-demand     |
| **Table Type**         | Dynamic entries with timeouts     |

#### How It Works

**Process:**

1. **Request Arrives:**
   - Internal host initiates connection
   - No translation exists yet

2. **Address Selection:**
   - NAT selects available public IP from pool
   - Creates mapping entry

3. **Active Phase:**
   - Translation active during communication
   - Entry maintained in table

4. **Timeout:**
   - After period of inactivity
   - Translation purged from table
   - Public IP returned to pool

#### Advantages

| Benefit                | Description                                                                |
| ---------------------- | -------------------------------------------------------------------------- |
| **Clean Mapping**      | One-to-one IP relationships maintained                                     |
| **Efficient Usage**    | Public IPs assigned only when needed                                       |
| **Better Utilization** | Handles varying numbers of users                                           |
| **Oversubscription**   | Can support more hosts than public IPs<br/>(not all active simultaneously) |

#### Disadvantages

| Limitation             | Impact                                     |
| ---------------------- | ------------------------------------------ |
| **Complex Management** | NAT must maintain dynamic table            |
| **Pool Exhaustion**    | New connections fail if pool depleted      |
| **No Inbound**         | External hosts cannot initiate connections |
| **Session Limits**     | Limited by pool size                       |

---

### 3. Port Address Translation (PAT) / NAT Overload

![PAT/NAPT Operation](attachments/Pasted%20image%2020260211224934.png)

**Definition:** Many local IPs mapped to **single public IP** using port numbers to distinguish connections.

**Also Known As:**

- NAT Overloaded
- NAPT (Network Address Port Translation)
- Dynamic NAPT

#### How It Works

**Key Innovation:** Use port numbers to multiplex multiple connections over single IP.

**Process:**

1. **Outgoing Request:**
   - Internal host sends packet (source IP + port)
   - NAT changes source IP to public IP
   - NAT changes source port to unique port number
   - Records mapping in table

2. **Return Traffic:**
   - NAT receives packet (destination = public IP + unique port)
   - Looks up port in table
   - Translates to original internal IP + port
   - Forwards to correct internal host

#### Example Scenario

**Setup:**

- 4 hosts on private network (10.0.0.x)
- 1 public IP (G)
- All accessing web server (128.10.19.20:80)

**Connection Details:**

| Internal Host | Internal Port | NAT Public IP | NAT Public Port | Internet Server | Server Port |
| ------------- | ------------- | ------------- | --------------- | --------------- | ----------- |
| 10.0.0.5      | 21023         | G             | 14003           | 128.10.19.20    | 80          |
| 10.0.0.1      | 386           | G             | 14010           | 128.10.19.20    | 80          |
| 10.0.0.2      | 1756          | G             | 14011           | 128.10.19.20    | 80          |
| 10.0.0.3      | 3492          | G             | 14012           | 128.10.19.20    | 80          |

**Key Point:** Even though both 10.0.0.5 and 10.0.0.1 use same destination and might have overlapping source ports, NAT assigns unique port numbers (14003, 14010) to distinguish them.

#### Advantages

| Benefit                 | Description                               |
| ----------------------- | ----------------------------------------- |
| **Most Cost-Effective** | Only one public IP needed                 |
| **High Capacity**       | Thousands of hosts behind single IP       |
| **Port Multiplexing**   | 65,535 possible ports per IP              |
| **Most Common**         | De facto standard for home/small business |

#### Disadvantages

| Limitation              | Impact                                     |
| ----------------------- | ------------------------------------------ |
| **Port Exhaustion**     | Limited to ~65,000 concurrent connections  |
| **Protocol Issues**     | Some protocols don't work well (FTP, PPTP) |
| **Logging Complex**     | Difficult to trace individual hosts        |
| **Peer-to-Peer Issues** | Complicates P2P applications               |

---

## Interaction Between NAT and ICMP

### The Challenge

**Problem:** Changes to IP addresses can cause unexpected effects on higher layer protocols.

**ICMP Specifics:** NAT must carefully handle ICMP messages.

### Ping Example

**Scenario:** Internal host uses `ping` to test Internet destination.

**Expected Behavior:**

- Host sends ICMP Echo Request
- Expects ICMP Echo Reply for each request

**NAT Requirement:** Must forward incoming echo replies to **correct internal host**.

### Message Handling

#### NAT Decision Process

**When ICMP arrives from Internet:**

| Message Category   | Action                   | Reason                       |
| ------------------ | ------------------------ | ---------------------------- |
| **Error Message**  | May process locally      | Could indicate NAT box issue |
| **Redirect**       | Process locally          | NAT routing may be incorrect |
| **Echo Reply**     | Forward to internal host | Response to internal ping    |
| **Query Response** | Forward to internal host | Response to internal query   |

**Critical:** NAT must determine whether message should be:

1. Handled locally (NAT box issue)
2. Forwarded to internal host (response to host's request)

### ICMP Translation

**Before forwarding to internal host:**

- NAT must **translate the ICMP message**
- Update addresses in ICMP payload
- Recalculate checksums

### ICMP Query Handling

**NAT Device Requirements:**

| Requirement                | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| **Permit Queries**         | Allow ICMP queries from private hosts to external hosts |
| **Transparent Forwarding** | Forward query packets and responses transparently       |
| **NAPT Translation**       | Translate ICMP Query ID in header                       |
| **Checksum Update**        | Recalculate ICMP checksum after translation             |

### Limitations

**General NAT Problem:**

- NAT **will not work** with applications that send IP addresses or protocol ports as **data** (in payload)
- Examples: FTP (in PORT command), H.323, SIP
- These protocols require special NAT traversal techniques or ALG (Application Layer Gateway)

---

## Autonomous System (AS)

### Definition

**Autonomous System:** Collection of networks and routers under one administrative authority.

### Characteristics

| Aspect                     | Description                                      |
| -------------------------- | ------------------------------------------------ |
| **Administrative Control** | Single organization manages all components       |
| **Internal Routing**       | Free to choose internal routing protocols        |
| **Connectivity**           | Connects to one or more other autonomous systems |
| **Policy Independence**    | Can implement own routing policies               |

### Purpose

**Why Use AS?**

- Simplifies Internet routing
- Allows independent network management
- Enables routing policy control
- Facilitates hierarchical internet structure

**Example AS Numbers:**

- AS7018 (AT&T)
- AS15169 (Google)
- AS32934 (Facebook)

**Reference:** [Understanding Autonomous Systems](https://www.youtube.com/watch?v=LPXi1j-FImo)

---

## Routing Area

### Concept

**Definition:** Single Autonomous System can be divided into smaller groups called **areas**.

### Purpose

**Benefits of Area Division:**

| Benefit                 | Description                                   |
| ----------------------- | --------------------------------------------- |
| **Reduced LSA Traffic** | Fewer link-state advertisements               |
| **Smaller Databases**   | Each area maintains smaller topology database |
| **Faster Convergence**  | Changes affect smaller domain                 |
| **Reduced Overhead**    | Less OSPF overhead traffic                    |
| **Scalability**         | Can grow network without overwhelming routers |

### Topology Size Reduction

**How It Helps:**

- Limits flooding of routing updates to area boundaries
- Reduces CPU processing on routers
- Decreases memory requirements
- Improves overall network stability

**Think of it as:** Breaking a large city into neighborhoods - you know your neighborhood well, but only need general directions to reach other neighborhoods.

---
