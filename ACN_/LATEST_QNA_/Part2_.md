### Q26: Consider a spectrum of bandwidth 33 MHz and assume every user requires 50 KHz BW for duplex channel. If the city is split into hexagonal cells with clusters find the numbers of users supported in the city for the following scenario. i) Whole city is assumed to be single cell ii) City is divided into 20 cells with 7 cells per cluster iii) City is divided into 30 cells with 5 cells per cluster iv) With reasons identify the advantage and disadvantages of increasing the number of cells in the city and increasing the number of cells per cluster (10 marks)

---

**Answer:**

#### Given Data

- Total Bandwidth: **33 MHz = 33,000 KHz**
- Channel Bandwidth: **50 KHz/duplex**

**Total Available Channels:**

```
Total Channels = 33,000 / 50 = 660 channels
```

#### i) Whole City as Single Cell

**Users Supported:** **660 users**

#### ii) 20 Cells with 7-Cell Cluster

**Calculation:**

```
Users = (Total BW / Channel BW / Cells per cluster) × No. of cells
Users = (33,000 / 50 / 7) × 20
Users = (660 / 7) × 20
Users = 94.3 × 20
Users ≈ 1,885 users
```

#### iii) 30 Cells with 5-Cell Cluster

**Calculation:**

```
Users = (33,000 / 50 / 5) × 30
Users = (660 / 5) × 30
Users = 132 × 30
Users = 3,960 users
```

#### iv) Analysis: Increasing Cells and Cluster Size

**Advantages of Increasing Number of Cells:**

- **Higher capacity:** More users can be served
- **Better frequency reusability**
- **Improved coverage**

**Disadvantages of Increasing Number of Cells:**

- **Co-channel interference:** Distance between clusters decreases
- **Adjacent channel interference:** More potential interference sources
- **Higher infrastructure cost**
- **Increased handoff frequency**

**Summary Table:**

| **Configuration** | **Users Supported** |
| ----------------- | ------------------- |
| Single Cell       | 660                 |
| 20 cells, K=7     | 1,885               |
| 30 cells, K=5     | 3,960               |

**Trade-off:** Smaller cluster size (K) increases capacity but requires careful interference management.

---

## Unit 3: Mobile IP

### Q27: Discuss the characteristics of a solution for mobile node to overcome the limitation of conventional IP for mobility (L2, 5 marks)

---

**Answer:**

#### Key Characteristics of Mobile IP Solution

| **Characteristic**                 | **Description**                                                                                                      |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Transparency**                   | Mobility is transparent to applications, transport layer protocols, and non-involved routers                         |
| **Scalability**                    | Solution scales to large internets; permits mobility across global Internet                                          |
| **Security**                       | Provides facilities to ensure all messages are authenticated                                                         |
| **Compatibility/Interoperability** | Mobile hosts using Mobile IP can interoperate with stationary hosts running conventional IPv4 and other mobile hosts |
| **Macro Mobility**                 | Focuses on long-duration moves (not micro-mobility within same area)                                                 |

**Benefits:**

- Seamless connectivity during movement
- No application changes required
- Works with existing Internet infrastructure
- Secure communication channels

---

### Q28: Discuss the role of the given mobile entities: i) Foreign Agent ii) Correspondent Node iii) Home Agent (CO1 L2, 5 marks)

---

**Answer:**

#### Mobile IP Entities

**i) Foreign Agent (FA)**

| **Aspect**   | **Details**                                                                            |
| ------------ | -------------------------------------------------------------------------------------- |
| **Location** | Router in foreign network                                                              |
| **Role**     | Assigns Care-of Address (CoA) to Mobile Node                                           |
| **Function** | Facilitates communication between Mobile Node and other nodes; acts as tunnel endpoint |

**ii) Correspondent Node (CN)**

| **Aspect**     | **Details**                                       |
| -------------- | ------------------------------------------------- |
| **Definition** | Device on Internet communicating with Mobile Node |
| **Type**       | Can be fixed or mobile node                       |
| **Example**    | Web server, email client, web browser             |

**iii) Home Agent (HA)**

| **Aspect**   | **Details**                                                                             |
| ------------ | --------------------------------------------------------------------------------------- |
| **Location** | Router in home network (domain of MN's home address)                                    |
| **Role**     | Binds MN's primary address with its CoA                                                 |
| **Function** | Forwards packets to appropriate network when MN is away using encapsulation (tunneling) |

---

### Q29: Assume a mobile node is configured with an IP 10.25.25.1 from the subnet 10.25.25.X and is currently associated with a router having the IP 10.25.20.7 in 10.25.20.X subnet. Identify the following: i) Home Agent IP ii) Mobile Node IP iii) Foreign Agent IP iv) Care of Address v) Co-located COA (CO3 L3, 5 marks)

---

**Answer:**

#### Mobile IP Configuration Analysis

| **Component**                 | **Value**     | **Notes**                                              |
| ----------------------------- | ------------- | ------------------------------------------------------ |
| **i) Home Agent IP**          | Not specified | Would be a router in 10.25.25.X subnet                 |
| **ii) Mobile Node IP**        | 10.25.25.1    | Home address (permanent)                               |
| **iii) Foreign Agent IP**     | 10.25.20.7    | Router in foreign network                              |
| **iv) Care of Address (CoA)** | 10.25.20.7    | Foreign Agent's address                                |
| **v) Co-located CoA**         | Not specified | Would be an IP from 10.25.20.X assigned directly to MN |

**Key Points:**

- Home Address: Permanent address from home subnet (10.25.25.1)
- CoA: Temporary address in foreign network (10.25.20.7)
- FA CoA used in this scenario (not co-located)

---

### Q30: A Correspondent Node with an IP 192.7.7.1 needs to communicate with the mobile node with IP 130.103.202.200 which is currently associated with a foreign agent having IP 130.111.111.111. A router with an IP 130.103.202.50 in the Home network of mobile node is acting as home agent. For this scenario demonstrate how the data is communicated between CN and MN (all 3 paths) (CO3 L3, 5 marks)

---

**Answer:**

#### Mobile IP Communication Paths

**Given:**

- CN: 192.7.7.1
- MN (Home Address): 130.103.202.200
- HA: 130.103.202.50
- FA (CoA): 130.111.111.111

#### Path 1: CN to HA (Standard IP Routing)

| **Parameter**      | **Value**                                 |
| ------------------ | ----------------------------------------- |
| **Source IP**      | 192.7.7.1 (CN)                            |
| **Destination IP** | 130.103.202.200 (MN's home address)       |
| **Routing**        | Standard Internet routing to home network |
| **Received by**    | Home Agent (130.103.202.50)               |

#### Path 2: HA to FA (Tunneling)

| **Parameter**            | **Value**                          |
| ------------------------ | ---------------------------------- |
| **Outer Source IP**      | 130.103.202.50 (HA)                |
| **Outer Destination IP** | 130.111.111.111 (CoA/FA)           |
| **Inner Packet**         | Complete original packet (CN → MN) |
| **Mechanism**            | IP-in-IP tunneling/encapsulation   |

**Tunnel established between HA and FA**

#### Path 3: MN Reply (Direct Routing)

| **Parameter**      | **Value**                           |
| ------------------ | ----------------------------------- |
| **Source IP**      | 130.103.202.200 (MN)                |
| **Destination IP** | 192.7.7.1 (CN)                      |
| **Path**           | MN → FA → CN (direct route)         |
| **Advantage**      | No tunneling needed for return path |

**Note:** Return packets bypass HA — sent directly to CN (more efficient)

---

### Q31: "When a host from home network needs to communicate with mobile node in foreign network the Proxy ARP concept is used". Say True or False and justify your answer (CO4 L4, 5 marks)

---

**Answer:**

**TRUE**

#### Justification

**Scenario:**

- Local host on home network sends packet to Mobile Node
- Host performs **ARP request** for MN's hardware (MAC) address
- MN has **moved to foreign network** (not present on local link)

**Proxy ARP Solution:**

1. **Home Agent intercepts** ARP requests specifying MN as target
2. **HA responds with its own hardware address** (not MN's)
3. Local host receives HA's MAC address
4. Local host forwards datagram to HA
5. **HA tunnels packet** to MN's current location

**Key Points:**

- **Completely transparent** to local computers
- Local systems unaware MN has moved
- HA acts as proxy for MN on home network
- Standard IP forwarding used by local hosts

---

### Q32: With a neat diagram discuss Mobility Agent Advertisement Extension message format used in mobile IP supporting services (CO1 L2, 10 marks)

---

**Answer:**

#### Mobility Agent Advertisement Extension

**Purpose:** Used during **Agent Discovery phase** in Mobile IP (MIPv4).

#### Message Format

| **Field**                 | **Size** | **Description**                                                                         |
| ------------------------- | -------- | --------------------------------------------------------------------------------------- |
| **Type**                  | 1 byte   | Set to **16** — identifies Mobility Agent Advertisement Extension                       |
| **Length**                | 1 byte   | Length of extension (bytes), excluding Type and Length fields                           |
| **Sequence Number**       | 2 bytes  | Counter incremented with each advertisement; helps MN detect lost/replayed messages     |
| **Registration Lifetime** | 2 bytes  | Maximum lifetime (seconds) agent accepts in Registration Request; **0xFFFF** = infinite |
| **Flags**                 | 1 byte   | Single-bit flags defining supported services (R, B, H, F, M, G, r, T)                   |
| **Reserved**              | 1 byte   | Reserved for future use; must be zero                                                   |
| **Care-of Address(es)**   | Variable | Zero or more IP addresses FA offers as CoA; HA omits this field                         |

#### Flag Definitions

| **Flag** | **Meaning**                            |
| -------- | -------------------------------------- |
| **R**    | Registration required                  |
| **B**    | Busy — cannot accept new registrations |
| **H**    | Home Agent                             |
| **F**    | Foreign Agent                          |
| **M**    | Minimal encapsulation supported        |
| **G**    | GRE encapsulation supported            |
| **r**    | Reserved                               |
| **T**    | Reverse tunneling supported            |

**Usage:** MN listens for these advertisements to discover available agents and their capabilities.

---

### Q33: With neat diagram demonstrate the steps involved in the process of Agent registration in Mobile IP (CO1 L3, 10 marks)

---

**Answer:**

![Agent Registration in Mobile IP](attachments/Pasted%20image%2020260211203945.png)

**Refer to Diagram 2**

#### Agent Registration Process

**Step 1: Mobile Node to Foreign Agent (Registration Request)**

| **Action**    | **Details**                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| **Message**   | Registration Request (RREQ)                                                         |
| **Sender**    | Mobile Node (MN)                                                                    |
| **Recipient** | Foreign Agent (FA)                                                                  |
| **Contains**  | MN's Home Address (HoA), FA's CoA, HA's address, requested lifetime, authentication |

**Step 2: Foreign Agent to Home Agent (RREQ Relay)**

| **Action**    | **Details**                          |
| ------------- | ------------------------------------ |
| **FA Action** | Records MN's HoA in **Visitor List** |
| **Relay**     | Forwards RREQ to MN's Home Agent     |
| **Role**      | FA acts as relay for tunnel endpoint |

**Step 3: Home Agent to Foreign Agent (Registration Reply)**

| **Action**    | **Details**                                                       |
| ------------- | ----------------------------------------------------------------- |
| **HA Action** | Performs checks (authentication, validity)                        |
| **Binding**   | Creates/updates **Mobility Binding** (HoA ↔ CoA) in binding table |
| **Reply**     | Sends Registration Reply (RREP) to FA                             |
| **Status**    | Includes success/failure code and granted lifetime                |

**Step 4: Foreign Agent to Mobile Node (RREP Delivery)**

| **Action**    | **Details**                                               |
| ------------- | --------------------------------------------------------- |
| **FA Action** | Receives RREP, updates Visitor List with granted lifetime |
| **Relay**     | Forwards RREP to Mobile Node                              |
| **Result**    | MN receives confirmation of registration                  |

**Registration Complete:** MN can now send/receive data through FA using tunneling.

---

### Q34: Discuss the role of the given mobile entities: i) Mobile Node ii) Home Network iii) Secondary/Care Of Address (CO1 L2, 5 marks)

---

**Answer:**

#### Mobile IP Entities

**i) Mobile Node (MN)**

**Definition:** Mobile terminal system (end user) or mobile router for which mobility support is provided.

**Characteristics:**

- Changes point of attachment between networks
- Assigned **permanent IP (Home Address)**
- Other hosts send packets to Home Address regardless of MN's location
- Can be laptop, smartphone, mobile router

**ii) Home Network**

**Definition:** Network to which Mobile Node originally belongs as per its assigned IP address.

**Characteristics:**

- Subnet to which MN's IP address belongs
- Contains Home Agent
- Where MN is "normally" located
- Provides permanent addressing

**iii) Secondary/Care of Address (CoA)**

**Definition:** Temporary address valid only while computer visits a given location.

**Characteristics:**

- **Changes as MN moves** between networks
- **Valid only in current foreign network**
- MN obtains CoA when moving to foreign network
- Sends CoA to Home Agent for registration

**Types of CoA:**

| **Type**              | **Description**                                                       |
| --------------------- | --------------------------------------------------------------------- |
| **Foreign Agent CoA** | FA's IP address used as CoA; FA is tunnel endpoint                    |
| **Co-located CoA**    | IP address assigned directly to MN's interface; MN is tunnel endpoint |

---

### Q35: A Node with IP 180.70.70.10 moves from its network to a foreign network with subnet 180.70.20.X. In foreign network router with IP 180.70.20.7 is acting as FA and a router with IP 180.70.70.20 is acting as HA in its home network. Demonstrate the steps that happen before data communication happens between the nodes (needs to specify the database contents at HA and FA) (CO1 L3, 10 marks)

---

**Answer:**

#### Mobile IP Setup Process

**Given:**

- MN Home Address: **180.70.70.10**
- Home Agent: **180.70.70.20**
- Foreign Agent: **180.70.20.7**
- CoA: **180.70.20.7** (FA CoA)

#### Steps Before Data Communication

**Step 1: Agent Discovery**

- MN receives Agent Advertisements
- Detects it's in foreign network (180.70.20.X)
- Identifies FA at 180.70.20.7

**Step 2: Registration Request (MN → FA)**

| **Field**       | **Value**                   |
| --------------- | --------------------------- |
| Home Address    | 180.70.70.10                |
| Home Agent      | 180.70.70.20                |
| Care-of Address | 180.70.20.7                 |
| Lifetime        | Requested registration time |

**Step 3: Registration Request (FA → HA)**

- FA relays RREQ to HA
- FA records MN in Visitor List

**Step 4: Registration Reply (HA → FA → MN)**

- HA creates mobility binding
- Sends RREP with success status
- FA updates Visitor List
- MN receives confirmation

#### Database Contents

**Home Agent Binding Table:**

| **Home Address** | **Care-of Address** | **Lifetime** | **Timestamp** |
| ---------------- | ------------------- | ------------ | ------------- |
| 180.70.70.10     | 180.70.20.7         | 3600 sec     | Current time  |

**Foreign Agent Visitor List:**

| **Home Address** | **Home Agent** | **MAC Address** | **Lifetime** |
| ---------------- | -------------- | --------------- | ------------ |
| 180.70.70.10     | 180.70.70.20   | MN's MAC        | 3600 sec     |

**Now ready for data communication via tunneling**

---

### Q36: Assume a mobile device M with home address 130.103.202.200 is attached to Home Agent R1 with IP 130.103.202.50 in the home network. Currently M is in Network having Foreign Agent with IP 130.111.111.111. If the device D with IP 130.111.111.105 transmits packets to mobile node M: i) Demonstrate with corresponding IP addresses, how the packet will be delivered from mobile node M to the destination device D ii) Identify the problem that rises in the above scenario (CO4 L4, 10 marks)

---

**Answer:**

#### i) Packet Delivery (M → D)

**Given:**

- Mobile Node (M): **130.103.202.200** (Home Address)
- Home Agent (R1): **130.103.202.50**
- Foreign Agent: **130.111.111.111**
- Device D: **130.111.111.105** (same network as FA)

**Step 1: M Creates Packet**

| **Field**      | **Value**                 |
| -------------- | ------------------------- |
| Source IP      | 130.103.202.200 (M's HoA) |
| Destination IP | 130.111.111.105 (D)       |

**Step 2: M → FA**

- M forwards packet to FA (default router)

**Step 3: FA Routes Packet**

- FA checks destination: 130.111.111.105
- **D is on same network as FA** (130.111.111.X)
- FA uses **standard IP routing**

**Step 4: FA → D**

- Packet delivered **directly** to D
- **No tunneling through HA needed**

#### ii) Problem: Triangle Routing (Two-Crossing Problem)

**What is the Problem?**

| **Aspect**       | **Issue**                                 |
| ---------------- | ----------------------------------------- |
| **Forward Path** | CN/D → HA → FA → M (indirect, through HA) |
| **Return Path**  | M → FA → D (direct)                       |
| **Result**       | Non-optimal **triangular path**           |

**Consequences:**

1. **Suboptimal Route Length**
   - If D and M are physically close (same network!)
   - Forward packets take unnecessary detour through HA
   - Significantly **increased latency**

2. **Wasted Resources**
   - Extra bandwidth consumed
   - Processing load on HA
   - Inefficient use of network

**Solution:** Route Optimization extensions allow CN to learn MN's CoA and send directly, bypassing HA.

---

### Q37: A Correspondent Node CN (200.100.1.50) need to transmit packets to Mobile Node MN (130.103.202.200) which was initiated from the Home Agent 130.103.202.50 and currently in the Foreign Network with Foreign Agent 130.111.111.111. Demonstrate how packets will be delivered to MN using Tunneling concept. (Note: Required to specify SOURCE & DESTINATION IP address in every stage) (CO4 L4, 10 marks)

---

**Answer:**

#### Given Mobile IP Configuration

| **Entity**                      | **IP Address**  |
| ------------------------------- | --------------- |
| Correspondent Node (CN)         | 200.100.1.50    |
| Mobile Node (MN) — Home Address | 130.103.202.200 |
| Home Agent (HA)                 | 130.103.202.50  |
| Foreign Agent (FA) — CoA        | 130.111.111.111 |

#### Step 1: CN to HA (Standard IP Routing)

**IP Header:**

| **Field**          | **Value**                           |
| ------------------ | ----------------------------------- |
| **Source IP**      | 200.100.1.50 (CN)                   |
| **Destination IP** | 130.103.202.200 (MN's Home Address) |

**Process:**

- CN is unaware MN has moved
- Sends to MN's permanent Home Address
- Standard Internet routing to home network
- **Packet received by:** Home Agent (130.103.202.50)
- HA intercepts (via proxy ARP or routing table)

#### Step 2: HA to FA (Encapsulation and Tunneling)

**Outer IP Header:**

| **Field**          | **Value**                |
| ------------------ | ------------------------ |
| **Source IP**      | 130.103.202.50 (HA)      |
| **Destination IP** | 130.111.111.111 (FA/CoA) |

**Inner IP Header (Original Packet):**

| **Field**          | **Value**                  |
| ------------------ | -------------------------- |
| **Source IP**      | 200.100.1.50 (CN)          |
| **Destination IP** | 130.103.202.200 (MN's HoA) |

**Process:**

- HA performs **IP-in-IP encapsulation**
- Original packet becomes **payload** of new packet
- HA = **tunnel entry point**
- FA CoA = **tunnel endpoint**
- Intermediate routers see only outer header
- **Packet received by:** Foreign Agent (130.111.111.111)

#### Step 3: FA to MN (Decapsulation and Final Delivery)

**IP Header (After Decapsulation):**

| **Field**          | **Value**                  |
| ------------------ | -------------------------- |
| **Source IP**      | 200.100.1.50 (CN)          |
| **Destination IP** | 130.103.202.200 (MN's HoA) |

**Process:**

- FA acts as **tunnel exit point**
- **Strips outer IP header** (decapsulation)
- Examines original inner packet
- Looks up 130.103.202.200 in **Visitor List**
- Finds MN's link-layer (MAC) address
- Delivers **original packet locally** to MN
- **Packet received by:** Mobile Node (130.103.202.200)

**Key Concept:** Tunneling makes foreign network transparent; MN receives packet as if still on home network.

---

### Q38: Assume a mobile device M with home address 130.103.202.200 is attached to Home Agent with IP 130.103.202.50 in the home network. Currently M is in the Network having Foreign Agent with IP 130.111.111.111. Identify i) Who initiates the Registration request ii) The devices with their IP address and role in completing the registration process in Mobile IP (CO3 L3, 10 marks)

---

**Answer:**

#### i) Initiator of Registration Request

**Answer:** The **Mobile Node (M)** initiates the registration request.

**When?**

- When MN detects it has **moved from home to foreign network**
- Upon receiving **Agent Advertisement** from Foreign Agent
- Must inform Home Agent of new location

**Action:**

- MN generates and sends initial **Registration Request (RREQ)** message to FA

#### ii) Devices, IP Addresses, and Roles

**Device 1: Mobile Node (M)**

| **Aspect**       | **Details**                  |
| ---------------- | ---------------------------- |
| **Home Address** | 130.103.202.200              |
| **Role**         | **Initiator**                |
| **Action**       | Creates and sends RREQ to FA |

**Device 2: Foreign Agent (FA)**

| **Aspect**           | **Details**                                                                                                                             |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **IP Address (CoA)** | 130.111.111.111                                                                                                                         |
| **Role**             | **Relay & Tunnel Endpoint**                                                                                                             |
| **Actions**          | • Receives RREQ from MN<br>• Adds MN's HoA to **Visitor List**<br>• Relays RREQ to HA<br>• Acts as tunnel endpoint for incoming packets |

**Device 3: Home Agent (HA)**

| **Aspect**     | **Details**                                                                                                                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **IP Address** | 130.103.202.50                                                                                                                                                                                |
| **Role**       | **Binding Authority**                                                                                                                                                                         |
| **Actions**    | • Receives RREQ (relayed by FA)<br>• **Authenticates** the request<br>• Creates/updates **Mobility Binding** (HoA ↔ CoA) in binding table<br>• Sends **Registration Reply (RREP)** back to FA |

**Registration Flow:** MN → FA → HA → FA → MN

---

### Q39: With a neat diagram discuss Agent Registration Message Format used in mobile IP supporting services (CO1 L2, 10 marks)

---

**Answer:**

#### Registration Request Message Format

The Registration Request is the primary message used by MN to inform HA of its current location.

#### Message Structure

| **Field**           | **Size** | **Description**                                                  |
| ------------------- | -------- | ---------------------------------------------------------------- |
| **Type**            | 1 byte   | Identifies message type; **Type = 1** for Registration Request   |
| **Flags**           | 1 byte   | S, B, D, M, G, r, T, x (various options)                         |
| **Lifetime**        | 2 bytes  | Requested registration duration (seconds)                        |
| **Home Address**    | 4 bytes  | Permanent IP address of MN (home network)                        |
| **Home Agent**      | 4 bytes  | IP address of device acting as MN's Home Agent                   |
| **Care-of Address** | 4 bytes  | Current IP address MN is using (FA's address or co-located)      |
| **Identification**  | 8 bytes  | Timestamp/nonce for replay protection                            |
| **Extensions**      | Variable | Authentication extensions (mandatory); other optional extensions |

#### Key Flag Definitions

| **Flag** | **Meaning**                          |
| -------- | ------------------------------------ |
| **S**    | Simultaneous bindings                |
| **B**    | Broadcast datagrams                  |
| **D**    | Decapsulation by MN (co-located CoA) |
| **M**    | Minimal encapsulation                |
| **G**    | GRE encapsulation                    |
| **T**    | Reverse tunneling requested          |

#### Important Fields Explained

**Home Address:**

- Uniquely identifies MN regardless of how request is conveyed
- Permanent address from home network

**Care-of Address:**

- Temporary address in foreign network
- Either FA's address or co-located address on MN

**Extensions:**

- **Authentication mandatory** for security
- Prevents unauthorized registration

---

### Q40: Assume a mobile node is configured with an IP 15.7.7.1 from the subnet 15.7.7.X and is currently associated with a router having the IP 15.7.20.7 in 15.7.20.X subnet. Find i) Mobile Node IP ii) Home Agent IP iii) Co-located COA iv) Care of Address v) Foreign Agent IP (CO3 L3, 10 marks)

---

**Answer:**

#### Mobile IP Configuration Analysis

| **Component**           | **Value**     | **Explanation**                                         |
| ----------------------- | ------------- | ------------------------------------------------------- |
| **i) Mobile Node IP**   | 15.7.7.1      | Home Address (permanent address from home subnet)       |
| **ii) Home Agent IP**   | Not specified | Would be one of the routers in 15.7.7.X subnet          |
| **iii) Co-located CoA** | Not specified | Would be an IP from 15.7.20.X if MN uses co-located CoA |
| **iv) Care of Address** | 15.7.20.7     | Current temporary address (Foreign Agent's address)     |
| **v) Foreign Agent IP** | 15.7.20.7     | Router in foreign network (15.7.20.X)                   |

**Key Observations:**

- Home subnet: **15.7.7.X**
- Foreign subnet: **15.7.20.X**
- Using **FA CoA** (not co-located) since associated with router 15.7.20.7

---

### Q41: Differentiate between Foreign Agent COA and Co-located COA (CO1 L2, 5 marks)

---

**Answer:**

#### Comparison of CoA Types

| **Feature**             | **Foreign Agent CoA (FACoA)**                         | **Co-located CoA (CCoA)**                               |
| ----------------------- | ----------------------------------------------------- | ------------------------------------------------------- |
| **Address Owner**       | Foreign Agent (FA) router                             | Mobile Node (MN) itself                                 |
| **Tunnel Endpoint**     | Foreign Agent                                         | Mobile Node                                             |
| **Decapsulation**       | FA removes tunnel header                              | MN removes tunnel header                                |
| **Address Sharing**     | Multiple MNs can share same FACoA                     | One MN uses unique CCoA                                 |
| **Registration Path**   | MN → FA → HA                                          | MN → HA (direct)                                        |
| **Need for FA**         | Requires dedicated FA function on router              | No FA required; MN handles mobility independently       |
| **Address Acquisition** | Advertised by FA in Agent Advertisements              | Acquired by MN (DHCP, manual configuration)             |
| **Advantages**          | No IP configuration needed by MN; conserves addresses | No FA dependency; more flexible                         |
| **Disadvantages**       | Requires FA support in foreign network                | MN must acquire unique address; consumes more addresses |

**When to Use:**

- **FACoA:** Standard deployment with Mobile IP-aware routers
- **CCoA:** Foreign network lacks FA support; MN has IP configuration capability

---

### Q42: When does the TWO cross problem arise? Demonstrate the occurrence of this problem in a device with mobile IP with a scenario (CO4 L3, 10 marks)

---

**Answer:**

![Two Cross Problem in Mobile IP](attachments/Pasted%20image%2020260211204149.png)

**Refer to Diagram 4**

#### Definition

**Two Cross Problem:** When the same packet crosses the Internet **twice**, creating an inefficient triangular routing path.

#### When Does It Arise?

**Condition:** Mobile Node (MN) and Correspondent Host are **both in the foreign network**, but communication still goes through Home Agent (HA) in home network.

**Root Cause:** MN uses its **Home Address** for communication even when in foreign network.

#### Scenario

**Setup:**

- **Mobile Node (MN)** in foreign network with CoA
- **Host** also in same foreign network
- **Home Agent (HA)** in home network (geographically distant)

#### Problem Demonstration

**Outbound Path (MN → Host):**

1. MN → Router R4 (foreign network)
2. R4 → Host (delivered locally)
3. ✅ Efficient path

**Inbound Path (Host → MN):**

1. Host → Router R4 (foreign network)
2. R4 → **Internet** → Home Network
3. Packet reaches **Home Agent (R1)** ❌ (First crossing)
4. HA → **Internet** → Foreign Network ❌ (Second crossing)
5. Foreign Agent → MN

**Result:** Packet crosses Internet **twice** unnecessarily!

#### Why This Happens

- Host sends to MN's **Home Address** (130.103.202.200)
- Routing tables direct packet to **home network**
- HA intercepts and **tunnels back** to foreign network
- Even though Host and MN are on **same physical network**!

#### Impact

| **Issue**             | **Consequence**              |
| --------------------- | ---------------------------- |
| **Increased Latency** | Unnecessary round-trip delay |
| **Wasted Bandwidth**  | Double network traversal     |
| **HA Overhead**       | Unnecessary processing load  |

**Solution:** Route Optimization allows Host to learn MN's CoA and send directly.

---

### Q43: Discuss the purpose and method of Agent discovery process in mobile IP (CO1 L2, 5 marks)

---

**Answer:**

#### Purpose of Agent Discovery

**Primary Goals:**

| **Goal**                   | **Description**                                                   |
| -------------------------- | ----------------------------------------------------------------- |
| **Identify Network Type**  | Determine if current network is Home or Foreign                   |
| **Locate Mobility Agents** | Find addresses of available Home Agent (HA) or Foreign Agent (FA) |
| **Obtain CoA**             | Learn Foreign Agent Care-of Address if on foreign network         |

**Benefit:** Enables MN to detect movement and initiate registration process.

#### Method: Agent Advertisement

**Based on:** Extension of **ICMP Router Discovery** messages with special mobility extensions.

**Process:**

**1. Agent Broadcast:**

- Both HAs and FAs **periodically broadcast** Agent Advertisement messages
- Uses **extended ICMP Router Advertisement**
- Sent on all links they serve

**2. Mobility Extension Contents:**

| **Information** | **Purpose**                                                     |
| --------------- | --------------------------------------------------------------- |
| **Flags**       | Indicate agent capabilities (HA, FA, registration requirements) |
| **CoA**         | Available Care-of Addresses (if FA)                             |
| **Lifetime**    | How long registration is valid                                  |

**3. MN Listens:**

- MN continuously listens for advertisements
- Examines source IP and flags
- Determines if on Home Network (advertisement from HA) or Foreign Network (from FA)

**4. Movement Detection:**

- If MN receives advertisement from **new agent** → moved
- If **old agent stops advertising** → moved
- MN then initiates **Registration process**

**Alternative:** **Agent Solicitation** — MN can actively request advertisements instead of waiting.

---

### Q44: Discuss the need of Mobility Agent Extension message in Agent discovery process (CO1 L2, 5 marks)

---

**Answer:**

#### Why Mobility Agent Extension is Needed

The Mobility Agent Extension **transforms** standard ICMP Router Advertisement into **Mobile IP Agent Advertisement**.

#### Key Roles

**1. Declares Mobility Services**

**Flag Bits:**

- **H bit:** Set if router is Home Agent
- **F bit:** Set if router is Foreign Agent
- Allows MN to **instantly determine** router's role

**2. Provides Care-of Address**

When F bit is set (Foreign Agent):

- Extension **carries CoA** or multiple CoAs
- FA offers these addresses to visiting MNs
- Essential for MN to obtain temporary address

**3. Communicates Capabilities and Conditions**

| **Information**           | **Purpose**                                             |
| ------------------------- | ------------------------------------------------------- |
| **Registration Lifetime** | How long FA accepts registrations                       |
| **Encapsulation methods** | Supported tunneling techniques (IP-in-IP, GRE, minimal) |
| **Busy flag**             | Whether FA currently accepting new registrations        |

**4. Enables Location Discovery**

Without extension:

- Standard ICMP advertisement only indicates router presence
- **No Mobile IP-specific information**

With extension:

- MN knows **exactly what services available**
- Can make intelligent decisions about registration

**Conclusion:** Mobility Agent Extension is the **vehicle** that delivers all Mobile IP-specific information required for successful Agent Discovery and move detection.

---

### Q45: With a proper scenario discuss the limitation of conventional IP for Mobility (CO3 L4, 5 marks)

---

**Answer:**

#### Fundamental Limitation

**Core Problem:** Conventional IP **tightly binds** device's:

- **IP address** (identity) to
- **Geographical location** (network attachment point)

#### Scenario: Mobile VoIP Call

**Setup:**

- User **Ramesh** on VoIP call using laptop
- Walks between buildings on campus

**Initial State (MCA Building):**

- Connected to Wi-Fi in **Home Network**
- IP Address: **192.168.1.1**
- VoIP call established

**Movement (to CSE Department):**

- Connects to **different Wi-Fi network**
- New network: **10.0.2.0/24**

#### Problems with Conventional IP

**Problem 1: Break in Communication**

| **Issue**                      | **Details**                                     |
| ------------------------------ | ----------------------------------------------- |
| **IP must change**             | New network requires IP from 10.0.2.0/24 range  |
| **Existing connections break** | VoIP call established to 192.168.1.1            |
| **No automatic redirection**   | Packets still sent to old address (192.168.1.1) |
| **Result**                     | **Call drops** — complete loss of connectivity  |

**Problem 2: Change in Identity**

| **Issue**                  | **Details**                                             |
| -------------------------- | ------------------------------------------------------- |
| **IP = Identity**          | Higher-layer protocols use IP as identifier             |
| **New IP = New Identity**  | System thinks it's a different device                   |
| **Can't maintain session** | TCP connection bound to old IP                          |
| **Result**                 | Must **re-establish connection** — poor user experience |

**Why Conventional IP Can't Handle This:**

- Routing based on **network prefix** (location-dependent)
- No mechanism to **redirect** packets to new location
- Transport layer connections **tied to IP address**

**Mobile IP Solution:** Separates identity (Home Address) from location (CoA), enabling seamless mobility.

---

### Q46: Assume the mobile node is requesting for registration but the agent is busy to accept and the agent is willing to work as foreign agent. Configure this scenario in AEM message (CO3 L4, 5 marks)

---

**Answer:**

#### Agent Advertisement Extension Message Configuration

**Scenario Requirements:**

- Agent willing to work as **Foreign Agent**
- Currently **too busy** to accept new registrations

#### Message Configuration

**Field Values:**

| **Field**                 | **Value**           | **Explanation**                     |
| ------------------------- | ------------------- | ----------------------------------- |
| **Type**                  | 16                  | Identifies Mobility Agent Extension |
| **Code**                  | 0                   | Standard MAE                        |
| **Length**                | (appropriate value) | Length of extension                 |
| **Sequence Number**       | (incremental)       | Advertisement counter               |
| **Registration Lifetime** | **0**               | Not accepting registrations         |
| **Reserved**              | 0                   | Future use                          |
| **CoA**                   | (none)              | No CoA provided when busy           |

#### Flag Configuration

| **Flag**                      | **Value** | **Meaning**                          |
| ----------------------------- | --------- | ------------------------------------ |
| **H (Home Agent)**            | **0**     | Not acting as Home Agent             |
| **F (Foreign Agent)**         | **1**     | Willing to work as Foreign Agent     |
| **B (Busy)**                  | **1**     | Too busy to accept new registrations |
| **R (Registration Required)** | 0/1       | (as appropriate)                     |
| **Other flags**               | 0         | (as appropriate)                     |

#### Key Configuration Points

**Critical Combination:**

- **F = 1** (Foreign Agent capability exists)
- **B = 1** (Currently busy)
- **Registration Lifetime = 0** (Reinforces "busy" status)

#### MN Behavior Upon Receiving

Mobile Node will:

1. Recognize FA capability (F=1)
2. See B=1 and Lifetime=0
3. **Not attempt registration** with this FA
4. Either **wait** or **seek another FA**

---

## Unit 4: Advanced Routing and Network Architecture

### Q47: Justify the need of Two-level internet architecture (CO1 L3, 5 marks)

---

**Answer:**

#### What is Two-Level Internet Architecture?

Hierarchical structure distinguishing between:

- **Network ID** (Net-ID) — identifies the network
- **Host ID** — identifies specific host within network

#### Three Primary Needs

**1. Scalability — Massive Reduction in Router Table Size**

| **Problem**                          | **Solution**                          |
| ------------------------------------ | ------------------------------------- |
| Cannot store entry for every host    | Store entry for each **network** only |
| Billions of hosts                    | Millions of networks                  |
| Routers only need **Net-ID** entries | Dramatically smaller routing tables   |

**2. Hierarchical Routing**

**Two Routing Levels:**

- **Global Routing:** Uses Network-ID to route between networks
- **Local Routing:** Uses Host-ID to deliver within network

**Benefits:**

- Simplified routing decisions
- Faster packet processing
- Clear organizational boundaries

**3. Security and Privacy**

**Distinguishes:**

- **Internal datagrams:** Between hosts in same organization (can remain private)
- **External datagrams:** Between different organizations (requires internet routing)

**Benefits:**

- Internal traffic stays private
- Better security policy enforcement
- Network isolation possible

**Conclusion:** Two-level architecture makes the Internet **scalable, efficient, and manageable** at global scale.

---

### Q48: What do you mean by NAT? Discuss the role of NAT Box (CO2 L2, 5 marks)

---

**Answer:**

#### What is NAT?

**Network Address Translation (NAT)** is a technology providing IP-level access between hosts at a site and the Internet **without requiring** each host to have a globally valid IP address.

**Requirements:**

- Single connection to global Internet
- At least **one globally valid IP address**

#### Role of NAT Box

**NAT Box:** Computer with NAT software; all traffic passes through it.

**Key Functions:**

| **Function**            | **Outgoing Packets**                            | **Incoming Packets**                                              |
| ----------------------- | ----------------------------------------------- | ----------------------------------------------------------------- |
| **Address Translation** | Replace source address (private) with global IP | Replace destination address (global) with correct private address |
| **Routing**             | Forward to Internet                             | Forward to internal host                                          |
| **Table Maintenance**   | Track mappings                                  | Look up destination                                               |

**How It Appears:**

- **To internal hosts:** NAT box appears as **router** to global Internet
- **To external hosts:** All internal hosts appear to have **same global IP**

**Benefits:**

- Conserves IPv4 addresses
- Provides security (internal addresses hidden)
- Simplifies internal network management

---

### Q49: Assume Inside Local IP of three PCs PC1, PC2 and PC3 are 192.168.34.1, 192.168.34.2, 192.168.34.3, Inside Global IP is 12.1.1.1 and Outside Global IP is 13.1.1.1/24. i) Suggest the suitable NAT type to provide internet access to all PCs and justify your answer ii) If PC1 initiates connections to the web server, then write the IP address (both source and destination) of the outgoing and incoming packets before & after NAT process with port number. If PC2 and PC3 simultaneously request connection to the webserver with NAT table contents, demonstrate how these requests and replies will be handled by NAT Box (10 marks)

---

**Answer:**

#### i) Suitable NAT Type

**Answer:** **Port NAT (PAT/NAPT)** is the suitable method.

**Justification:**

- Only **one Inside Global IP** available (12.1.1.1)
- **Three PCs** need Internet access
- Other NAT methods require multiple global IPs
- **PNAT uses port numbers** to distinguish connections from different internal hosts

#### ii) NAT Process Demonstration

**PC1 Connection to Web Server:**

**Outgoing Packet:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 192.168.34.1:1234  | 13.1.1.1:80             |
| **After NAT**  | 12.1.1.1:50001     | 13.1.1.1:80             |

**Incoming Packet:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 13.1.1.1:80        | 12.1.1.1:50001          |
| **After NAT**  | 13.1.1.1:80        | 192.168.34.1:1234       |

**PC2 Connection:**

**Outgoing:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 192.168.34.2:1235  | 13.1.1.1:80             |
| **After NAT**  | 12.1.1.1:50002     | 13.1.1.1:80             |

**Incoming:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 13.1.1.1:80        | 12.1.1.1:50002          |
| **After NAT**  | 13.1.1.1:80        | 192.168.34.2:1235       |

**PC3 Connection:**

**Outgoing:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 192.168.34.3:1236  | 13.1.1.1:80             |
| **After NAT**  | 12.1.1.1:50003     | 13.1.1.1:80             |

**Incoming:**

| **Stage**      | **Source IP:Port** | **Destination IP:Port** |
| -------------- | ------------------ | ----------------------- |
| **Before NAT** | 13.1.1.1:80        | 12.1.1.1:50003          |
| **After NAT**  | 13.1.1.1:80        | 192.168.34.3:1236       |

#### NAT Translation Table

| **Inside Local**  | **Inside Global** | **Outside Global** |
| ----------------- | ----------------- | ------------------ |
| 192.168.34.1:1234 | 12.1.1.1:50001    | 13.1.1.1:80        |
| 192.168.34.2:1235 | 12.1.1.1:50002    | 13.1.1.1:80        |
| 192.168.34.3:1236 | 12.1.1.1:50003    | 13.1.1.1:80        |

**How NAT Box Handles Simultaneous Connections:**

- Uses **unique port numbers** (50001, 50002, 50003) to track each session
- Translates based on port mapping in table
- All external hosts see same IP (12.1.1.1) but different ports

---

### Q50: Consider an organization is having a NAT box with multiple global IPs and the organization is required to provide internet access: i) For fixed number of PCs in private network, suggest the NAT type with advantages and disadvantages ii) For variable number of PCs in the private network suggest the NAT type with advantages and disadvantages (CO4 L4, 10 marks)

---

**Answer:**

#### i) Fixed Number of PCs — Static NAT

**Method:** **Static NAT** — One-to-one mapping between private and public IP addresses (permanently configured).

**Advantages:**

| **Benefit**              | **Description**                                                 |
| ------------------------ | --------------------------------------------------------------- |
| **Predictable Mapping**  | Each private host always mapped to same public IP               |
| **Direct Access**        | Hosts in private network can be directly accessed from Internet |
| **Simple Configuration** | Easy to set up and maintain                                     |
| **No Port Management**   | Full end-to-end connectivity                                    |

**Disadvantages:**

| **Drawback**              | **Description**                                         |
| ------------------------- | ------------------------------------------------------- |
| **Wastes Addresses**      | Bandwidth wasted if mapped host not using Internet      |
| **No Scalability**        | Cannot accommodate more hosts than public IPs available |
| **Requires Multiple IPs** | Needs as many public IPs as internal hosts              |

#### ii) Variable Number of PCs — Dynamic NAT

**Method:** **Dynamic NAT** — Multiple private IPs mapped to **pool of public IPs** (assigned dynamically as needed).

**Advantages:**

| **Benefit**                 | **Description**                                                                           |
| --------------------------- | ----------------------------------------------------------------------------------------- |
| **One-to-One Mapping**      | Maintains clean IP relationships during active sessions                                   |
| **Efficient Bandwidth Use** | Public IPs assigned only when needed; returned to pool when idle                          |
| **Better Utilization**      | Handles varying numbers of simultaneous users                                             |
| **Oversubscription**        | Can support more internal hosts than available public IPs (not all active simultaneously) |

**Disadvantages:**

| **Drawback**              | **Description**                                               |
| ------------------------- | ------------------------------------------------------------- |
| **Complex Management**    | Requires NAT box to maintain dynamic mapping table            |
| **Pool Exhaustion**       | If all public IPs in use, new connections fail                |
| **No Inbound Initiation** | External hosts cannot initiate connections (no fixed mapping) |

**Recommendation:** Use **PAT (Port NAT)** if public IP pool is limited — allows hundreds of internal hosts with single public IP.

---

### Q51: A network is comprised of network devices like routers/switches, hosts, business applications and network management tools. Incorporate these components into Software Defined Network architecture and demonstrate the working with neat diagram (CO2 L3, 10 marks)

---

**Answer:**

![SDN Architecture Components](attachments/Pasted%20image%2020260211204305.png)

#### SDN Architecture Components

The given components (routers/switches, hosts, business applications, and network management tools) are incorporated in a layered approach:

**SDN Infrastructure Layer:**

- **Networking devices:** Routers/switches and hosts
- **Role:** Control forwarding and data processing capabilities
- **Function:** Handle forwarding and processing of the data path

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

### Q52: Assume a data plane is implemented in the lower box and has forwarding tables. These tables are populated and managed by the route processor's CPU/control plane program. With neat diagram demonstrate how data and control planes work for forwarding data (CO2 L3, 10 marks)

---

**Answer:**

![Data and Control Plane Working](attachments/Pasted%20image%2020260211204439.png)

#### Data and Control Plane Operation

**Scenario:** Packets received by Switch A need to be forwarded to Switch B

**Step 1: Packet Reception (Data Plane)**

- Packets received on input ports of line card
- Data plane resides here

**Step 2: Unknown MAC Address Handling (Control Plane)**

- If packet from unknown MAC address received
- Redirected to **control plane** of device
- MAC address **learned** here
- Packet forwarded onwards

**Step 3: RIB Processing (Control Plane)**

- Packet delivered to control plane
- Information contained therein processed
- **Alters the RIB (Routing Information Base)**

**Step 4: FIB Update (Control and Data Plane)**

- When RIB becomes stable
- **FIB (Forwarding Information Base) updated** in:
  - Control plane
  - Data plane
- Subsequently, forwarding updated to reflect changes

**Result:** Efficient packet forwarding with separation of control logic from data forwarding.

---

### Q53: With neat diagram discuss the working of OpenFlow Protocol (CO1 L2, 5 marks)

---

**Answer:**

![OpenFlow Protocol](attachments/Pasted%20image%2020260211204458.png)

#### OpenFlow Protocol Overview

**Definition:**

- **Layer 2 communications protocol**
- Gives access to forwarding plane of network switch/router
- Enables communication between OpenFlow switches and controller

#### Architecture

**OpenFlow was architected for:**

- Devices containing only **data planes**
- Respond to commands from **(logically) centralized controller**
- Controller houses **single control plane** for network

#### Working Mechanism

**Protocol Specification:**

- Describes communication between:
  - OpenFlow switches (data plane devices)
  - OpenFlow controller (control plane)

**Key Concept:**

- Centralized control plane sends commands
- Distributed data planes execute forwarding
- Flow-based forwarding using flow tables
- Controller installs/removes flow entries dynamically

---

### Q54: With a neat diagram explain the architecture of Software Defined Networks (CO1 L2, 10 marks)

---

**Answer:**

![SDN Architecture Layers](attachments/Pasted%20image%2020260211204514.png)

#### SDN Architecture - Three Layers

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

#### Communication Interfaces

- **Northbound APIs:** Between application and control layer
- **Southbound APIs:** Between control and infrastructure layer

#### Key Characteristics

| **Feature**                     | **Description**                                                |
| ------------------------------- | -------------------------------------------------------------- |
| **Directly Programmable**       | Network control separated from forwarding functions            |
| **Agile**                       | Dynamically adjust network-wide traffic flow                   |
| **Centrally Managed**           | Software-based SDN controller maintains global network view    |
| **Programmatically Configured** | Configure, manage, secure, optimize via automated SDN programs |

---

### Q55: With a neat diagram discuss the role of each field of IPv6 packet format (CO1 L2, 10 marks)

---

**Answer:**

![IPv6 Packet Format](attachments/Pasted%20image%2020260211204535.png)

#### IPv6 Packet Header Fields

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
- Used for: voice/video calls, real-time multimedia, QoS traffic
- All packets in flow have same source, destination, flow label

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

### Q56: With a neat diagram demonstrate the role of different types of routers in MPLS networks (CO1 L3, 10 marks)

---

**Answer:**

![MPLS Network Topology](attachments/Pasted%20image%2020260211204554.png)

#### MPLS Router Types and Roles

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

#### MPLS Operations

| **Operation** | **Description**                     | **Router Type**  |
| ------------- | ----------------------------------- | ---------------- |
| **Push**      | Add MPLS label                      | Ingress PE       |
| **Swap**      | Replace existing label with new one | Intermediate LSR |
| **Pop**       | Remove MPLS label                   | Egress PE        |

---

### Q57: Discuss structure of MPLS header format with role of each field in the header (CO1 L2, 5 marks)

---

**Answer:**

![MPLS Header Format](attachments/Pasted%20image%2020260211204608.png)

#### MPLS Header Structure

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
