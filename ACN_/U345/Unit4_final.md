# Unit 4: Advanced Routing, BGP, IPv6, and MPLS

---

## Routing Area Types

### Overview

In hierarchical routing architectures (especially OSPF), an Autonomous System is divided into multiple areas to improve scalability and reduce routing overhead. Each area type has specific characteristics and purposes.

---

### 1. Backbone Area (Area 0)

**Definition:** The central area to which all other areas must connect, directly or indirectly.

**Characteristics:**

| Feature         | Description                                   |
| --------------- | --------------------------------------------- |
| **Area ID**     | Always **Area 0** (0.0.0.0)                   |
| **Purpose**     | Central hub for inter-area routing            |
| **Requirement** | **All areas must connect to Area 0**          |
| **Function**    | Distributes routing information between areas |
| **Stability**   | Must always remain contiguous                 |

**Key Rule:** Non-backbone areas exchange routing information through the backbone area.

**Why It's Needed:**

- Prevents routing loops in multi-area networks
- Provides single point for inter-area route calculation
- Ensures predictable routing behavior

---

### 2. Standard Area

**Definition:** Regular OSPF area that can receive and store all types of routing updates.

**Routing Information Accepted:**

| LSA Type   | Description                       | Stored in Area?    |
| ---------- | --------------------------------- | ------------------ |
| **Type 1** | Router LSA (intra-area routes)    | ✓ Yes              |
| **Type 2** | Network LSA (DR information)      | ✓ Yes              |
| **Type 3** | Summary LSA (inter-area routes)   | ✓ Yes              |
| **Type 4** | ASBR Summary LSA                  | ✓ Yes              |
| **Type 5** | AS External LSA (external routes) | ✓ Yes              |
| **Type 7** | NSSA External LSA                 | ✗ No (unless NSSA) |

**Characteristics:**

| Feature          | Description                                 |
| ---------------- | ------------------------------------------- |
| **Flexibility**  | Full routing information available          |
| **Memory Usage** | Highest (stores all route types)            |
| **CPU Usage**    | Higher (processes all LSA types)            |
| **Typical Use**  | Core areas with sufficient resources        |
| **Route Types**  | Intra-area, inter-area, external all stored |

**Advantages:**

- Complete routing visibility
- Optimal path selection
- Full route diversity
- No additional configuration needed

**Disadvantages:**

- Larger LSDB (Link State Database)
- More memory required
- Higher CPU overhead
- More LSA flooding traffic

---

### 3. Stub Area

**Definition:** Special area that reduces routing table size and LSA flooding by blocking external route advertisements.

**What Gets Blocked:**

| Route Type                       | Allowed in Stub Area? | Alternative                   |
| -------------------------------- | --------------------- | ----------------------------- |
| **Type 5 LSA** (External Routes) | ✗ **Blocked**         | Default route injected by ABR |
| **Type 4 LSA** (ASBR info)       | ✗ **Blocked**         | Not needed without Type 5     |
| **Type 3 LSA** (Inter-area)      | ✓ **Allowed**         | Full inter-area routing       |
| **Type 1/2 LSA** (Intra-area)    | ✓ **Allowed**         | Full intra-area routing       |

**How It Works:**

1. **ABR blocks Type 5 LSAs** from entering stub area
2. **ABR injects default route** (0.0.0.0/0) into stub area
3. **Internal routers use default** for all external destinations
4. **Inter-area routes still propagated** normally

**Use Cases:**

**Best For:**

- Edge/branch office locations
- Areas with limited router resources
- Networks at network periphery
- When specific external routes not needed

**Not Suitable For:**

- Areas containing ASBRs (external route sources)
- Transit areas (passing traffic between other areas)
- Areas requiring specific external route visibility

**Advantages:**

| Benefit              | Description                 |
| -------------------- | --------------------------- |
| **Reduced LSDB**     | Smaller link-state database |
| **Lower Memory**     | Less RAM required           |
| **Faster SPF**       | Quicker convergence         |
| **Less CPU**         | Lower processing overhead   |
| **Reduced Flooding** | Fewer LSA updates           |

**Configuration Requirements:**

- **All routers** in stub area must be configured as stub
- Cannot contain ASBR (Autonomous System Boundary Router)
- Must have contiguous configuration (all or nothing)

**Types of Stub Areas:**

| Stub Type          | Type 3 LSA                 | Type 5 LSA             | Default Route   |
| ------------------ | -------------------------- | ---------------------- | --------------- |
| **Stub**           | ✓ Allowed                  | ✗ Blocked              | Injected by ABR |
| **Totally Stubby** | ✗ Blocked (except default) | ✗ Blocked              | Injected by ABR |
| **NSSA**           | ✓ Allowed                  | ✗ Blocked, Type 7 used | Optional        |
| **Totally NSSA**   | ✗ Blocked                  | ✗ Blocked, Type 7 used | Injected by ABR |

![Types of Area](attachments/Pasted%20image%2020260211222459.png)

---

## Types of Routers in OSPF

![Router Types in OSPF](attachments/Pasted%20image%2020260211222326.png)

### Overview

OSPF defines different router roles based on their position and function within the area hierarchy. Understanding these types is crucial for proper network design.

---

### 1. Internal Router

**Definition:** Router with **all interfaces** in the **same single area**.

**Characteristics:**

| Feature             | Description                         |
| ------------------- | ----------------------------------- |
| **Area Membership** | Belongs to exactly one area         |
| **LSDB**            | Maintains one link-state database   |
| **Adjacencies**     | Only with routers in same area      |
| **LSA Processing**  | Type 1 and Type 2 only for its area |

**Function:**

- Performs standard intra-area routing
- Runs SPF algorithm for single area
- Simplest OSPF router type

**Example:** Router in the middle of Area 1 with all connections to other Area 1 routers.

---

### 2. Backbone Router

**Definition:** Router with at least one interface in Area 0 (backbone area).

**Types of Backbone Routers:**

| Type              | Description                          |
| ----------------- | ------------------------------------ |
| **Pure Backbone** | All interfaces in Area 0             |
| **ABR**           | Interfaces in Area 0 and other areas |

**Responsibilities:**

| Task                   | Description                     |
| ---------------------- | ------------------------------- |
| **Inter-Area Routing** | Forwards traffic between areas  |
| **Backbone Stability** | Maintains backbone connectivity |
| **LSA Distribution**   | Propagates routing information  |

**Key Point:** Area 0 must remain **contiguous** (all connected). Virtual links can be used if physical contiguity is broken.

---

### 3. Area Border Router (ABR)

**Definition:** Router with interfaces in **multiple areas**, one of which **must be Area 0**.

**Critical Functions:**

| Function                 | Description                             |
| ------------------------ | --------------------------------------- |
| **Area Connection**      | Connects non-backbone areas to backbone |
| **LSA Translation**      | Converts Type 1/2 to Type 3             |
| **LSDB Maintenance**     | Maintains separate LSDB for each area   |
| **Route Summarization**  | Can summarize routes between areas      |
| **Stub Area Management** | Injects default routes into stub areas  |

**Multi-Database Operation:**

```
ABR maintains:
- One LSDB for Area 0
- One LSDB for each non-backbone area connected
- Separate SPF calculations for each area
```

**Example Scenario:**

| Interface | Area   | Role                     |
| --------- | ------ | ------------------------ |
| Gig0/0    | Area 0 | Backbone connection      |
| Gig0/1    | Area 1 | Standard area connection |
| Gig0/2    | Area 2 | Stub area connection     |

**Requirements:**

- At least one interface in Area 0
- Generates Type 3 Summary LSAs
- Higher memory and CPU requirements

---

### 4. Autonomous System Boundary Router (ASBR)

**Definition:** OSPF router that **redistributes external routes** into the OSPF domain from other routing protocols or sources.

**External Route Sources:**

| Source                      | Example                      |
| --------------------------- | ---------------------------- |
| **Other Routing Protocols** | RIP, EIGRP, BGP              |
| **Static Routes**           | Manually configured          |
| **Connected Networks**      | Direct interface connections |
| **Default Routes**          | Internet connectivity        |

**ASBR Functions:**

| Function            | Description                             |
| ------------------- | --------------------------------------- |
| **Route Injection** | Brings external routes into OSPF        |
| **LSA Generation**  | Creates Type 5 LSAs (AS-External)       |
| **Redistribution**  | Converts external routes to OSPF format |
| **Advertising**     | Type 4 LSAs advertise ASBR location     |

**LSA Types Used:**

| LSA Type   | Purpose           | Generated By                     |
| ---------- | ----------------- | -------------------------------- |
| **Type 5** | AS-External LSA   | ASBR itself                      |
| **Type 4** | ASBR Summary LSA  | ABR (to advertise ASBR location) |
| **Type 7** | NSSA External LSA | ASBR in NSSA                     |

**Example Scenario:**

```
ASBR Configuration:
- Gig0/0: OSPF Area 1
- Gig0/1: Connected to RIP network
- Function: Redistributes RIP routes into OSPF
```

**Key Points:**

- Can be located in **any area** (including Area 0)
- **Exception:** Cannot be in Stub Area (Type 5 LSAs blocked)
- NSSA allows ASBR with Type 7 LSAs (converted to Type 5 at ABR)

**ASBR Location Considerations:**

| Location          | Pros                 | Cons                                 |
| ----------------- | -------------------- | ------------------------------------ |
| **Area 0**        | Central distribution | Single point of failure              |
| **Standard Area** | Flexibility          | Requires ABR translation             |
| **NSSA**          | Edge redistribution  | Type 7 to Type 5 conversion overhead |

---

## Router Type Combinations

### Possible Combinations

A single router can have **multiple roles simultaneously**:

| Combination         | Example Scenario                                 |
| ------------------- | ------------------------------------------------ |
| **Internal Router** | Only in Area 1                                   |
| **Backbone Router** | Only in Area 0                                   |
| **ABR**             | Connects Area 0 and Area 1                       |
| **ASBR**            | Redistributes BGP in Area 2                      |
| **ABR + ASBR**      | Connects areas AND redistributes external routes |
| **Backbone + ASBR** | In Area 0 AND redistributes routes               |

**Most Complex:** Router that is ABR + ASBR + Backbone Router

- Interfaces in Area 0 and other areas (ABR)
- Redistributes external routes (ASBR)
- Part of backbone infrastructure (Backbone Router)

---

## Interior Gateway Protocols (IGP)

### Definition

**IGP (Interior Gateway Protocol):** Routing protocol designed to operate **within** a single Autonomous System.

**Purpose:** Handle routing **internal** to an organization's network.

---

### IGP Classification

#### 1. Distance Vector Protocols

**Principle:** Routers share routing tables with neighbors, advertising distance (metric) to destinations.

**Examples:**

| Protocol  | Metric                | Max Hop Count         | Convergence     |
| --------- | --------------------- | --------------------- | --------------- |
| **RIP**   | Hop count             | 15 (16 = unreachable) | Slow            |
| **RIPv2** | Hop count             | 15                    | Slow (improved) |
| **IGRP**  | Composite (BW, delay) | 255                   | Moderate        |

**Characteristics:**

- Simple to configure
- Lower CPU/memory requirements
- Slower convergence
- Routing loops possible
- Limited scalability

**How It Works:**

1. Router sends entire routing table to neighbors
2. Neighbors update their tables based on received information
3. Process repeats periodically
4. Routing information "ripples" through network

---

#### 2. Link-State Protocols

**Principle:** Each router builds complete network topology map, then calculates best paths using **SPF (Shortest Path First)** algorithm.

**Examples:**

| Protocol  | Standard        | Area Support       | Metric                    |
| --------- | --------------- | ------------------ | ------------------------- |
| **OSPF**  | Open (RFC 2328) | Yes (hierarchical) | Cost (based on bandwidth) |
| **IS-IS** | ISO standard    | Yes                | Cost (configurable)       |

**Characteristics:**

- Faster convergence
- Loop-free routing
- Higher memory/CPU requirements
- Better scalability
- More complex configuration

**How It Works:**

1. Router discovers neighbors (Hello protocol)
2. Builds Link-State Database (LSDB) through LSA flooding
3. Runs Dijkstra's SPF algorithm
4. Calculates shortest path tree
5. Installs best routes in routing table

**Comparison Table:**

| Feature                 | Distance Vector      | Link-State              |
| ----------------------- | -------------------- | ----------------------- |
| **Routing Info Shared** | Distance + Direction | Complete topology       |
| **Algorithm**           | Bellman-Ford         | Dijkstra SPF            |
| **Updates**             | Periodic full table  | Event-driven LSAs       |
| **Convergence**         | Slow                 | Fast                    |
| **Routing Loops**       | Possible             | Prevented               |
| **Scalability**         | Limited              | Excellent               |
| **Memory Usage**        | Low                  | High (full topology)    |
| **CPU Usage**           | Low                  | High (SPF calculations) |
| **Configuration**       | Simple               | Complex                 |

---

### RIP (Routing Information Protocol)

**Version History:**

| Version   | Features                                    |
| --------- | ------------------------------------------- |
| **RIPv1** | Classful, broadcast, no authentication      |
| **RIPv2** | Classless (VLSM), multicast, authentication |
| **RIPng** | IPv6 support                                |

**Key Specifications:**

| Parameter                   | Value                          |
| --------------------------- | ------------------------------ |
| **Metric**                  | Hop count only                 |
| **Maximum Hops**            | 15 (16 = infinite/unreachable) |
| **Update Timer**            | Every 30 seconds               |
| **Administrative Distance** | 120                            |
| **Protocol**                | UDP, Port 520                  |

**Limitations:**

- **No bandwidth consideration** (all links equal if same hop count)
- Small network maximum (15 hops)
- Slow convergence (can take minutes)
- High bandwidth usage (periodic full updates)

---

### OSPF (Open Shortest Path First)

**Version History:**

| Version    | Purpose         |
| ---------- | --------------- |
| **OSPFv2** | IPv4 (RFC 2328) |
| **OSPFv3** | IPv6 (RFC 5340) |

**Key Features:**

| Feature                 | Description                             |
| ----------------------- | --------------------------------------- |
| **Algorithm**           | Dijkstra SPF                            |
| **Metric**              | Cost = 100,000,000 / bandwidth (bps)    |
| **Areas**               | Hierarchical design (Area 0 = backbone) |
| **Updates**             | Event-triggered LSAs                    |
| **Convergence**         | Fast (seconds)                          |
| **Authentication**      | MD5, plain text                         |
| **VLSM Support**        | Yes                                     |
| **Route Summarization** | Manual at ABR/ASBR                      |

**Cost Calculation Examples:**

| Interface Type       | Bandwidth  | Cost |
| -------------------- | ---------- | ---- |
| **Serial**           | 64 Kbps    | 1562 |
| **T1**               | 1.544 Mbps | 64   |
| **Ethernet**         | 10 Mbps    | 10   |
| **Fast Ethernet**    | 100 Mbps   | 1    |
| **Gigabit Ethernet** | 1 Gbps     | 1    |

**Note:** Gigabit and faster interfaces all default to cost 1 unless manually adjusted.

---

### IS-IS (Intermediate System to Intermediate System)

**Origin:** ISO/OSI networking standard adapted for IP.

**Key Characteristics:**

| Feature            | Description                         |
| ------------------ | ----------------------------------- |
| **Protocol Layer** | Runs directly over Layer 2 (not IP) |
| **Areas**          | Two-level hierarchy (L1, L2)        |
| **Metric**         | Flexible (default = 10 per link)    |
| **VLSM Support**   | Yes                                 |
| **Scalability**    | Excellent (ISP-grade)               |

**Level Types:**

| Level            | Description           | Equivalent         |
| ---------------- | --------------------- | ------------------ |
| **Level 1 (L1)** | Intra-area routing    | OSPF standard area |
| **Level 2 (L2)** | Inter-area routing    | OSPF Area 0        |
| **L1/L2**        | Router in both levels | OSPF ABR           |

**IS-IS vs OSPF:**

| Aspect                | IS-IS                    | OSPF                    |
| --------------------- | ------------------------ | ----------------------- |
| **Adoption**          | Large ISPs               | Enterprises + some ISPs |
| **Protocol Layer**    | Layer 2 (CLNP)           | Layer 3 (IP)            |
| **Complexity**        | More complex             | Simpler                 |
| **Flexibility**       | High                     | Moderate                |
| **Update Efficiency** | Better (partial updates) | Good                    |

---

## BGP (Border Gateway Protocol)

### Definition

**BGP (Border Gateway Protocol):** Path vector protocol used for routing **between** Autonomous Systems on the Internet.

**Purpose:** Handle inter-domain routing across different administrative boundaries.

---

### BGP Characteristics

| Feature             | Description                                              |
| ------------------- | -------------------------------------------------------- |
| **Protocol Type**   | Path Vector (hybrid of distance vector and policy-based) |
| **Current Version** | BGP-4 (RFC 4271)                                         |
| **Transport**       | TCP Port 179                                             |
| **Metric**          | Path attributes (policy-based, not just shortest)        |
| **Scalability**     | Handles entire Internet routing table (900,000+ routes)  |
| **Convergence**     | Slow (minutes to hours for global Internet)              |
| **Policy Control**  | Extensive (route filtering, manipulation)                |

---

### Path Vector Routing

**Concept:** BGP advertises **complete AS path** for each destination network.

**Example Route Advertisement:**

```
Destination: 192.168.10.0/24
AS Path: AS100 → AS200 → AS300 → AS400
```

**How It Prevents Loops:**

- Router receives advertisement with AS path
- Checks if **its own AS number** appears in path
- If yes → **Reject route** (loop detected)
- If no → Accept and potentially use route

**Path Attributes Include:**

| Attribute      | Type                     | Description                       |
| -------------- | ------------------------ | --------------------------------- |
| **AS_PATH**    | Well-known mandatory     | Sequence of ASes                  |
| **NEXT_HOP**   | Well-known mandatory     | Next router IP                    |
| **ORIGIN**     | Well-known mandatory     | Route origin (IGP/EGP/incomplete) |
| **LOCAL_PREF** | Well-known discretionary | Local preference value            |
| **MED**        | Optional non-transitive  | Multi-Exit Discriminator          |
| **COMMUNITY**  | Optional transitive      | Route tagging                     |

---

### BGP Peering

![BGP Transit and Peering](attachments/Pasted%20image%2020260211222536.png)

#### Types of BGP Sessions

#### 1. eBGP (External BGP)

**Definition:** BGP session between routers in **different** Autonomous Systems.

**Characteristics:**

| Feature                     | Details                               |
| --------------------------- | ------------------------------------- |
| **Hop Count**               | Default TTL = 1 (direct connection)   |
| **AS Path Modification**    | Adds own AS to path                   |
| **Administrative Distance** | 20                                    |
| **Next-Hop**                | Usually changed to advertising router |
| **Use Case**                | Internet peering, ISP connections     |

**Example:**

```
AS 100 (Company A) ←→ AS 200 (Company B)
Router R1 (AS 100) peers with Router R2 (AS 200)
```

#### 2. iBGP (Internal BGP)

**Definition:** BGP session between routers in the **same** Autonomous System.

**Characteristics:**

| Feature                     | Details                                   |
| --------------------------- | ----------------------------------------- |
| **Hop Count**               | No TTL restriction (can be multiple hops) |
| **AS Path**                 | Not modified                              |
| **Administrative Distance** | 200                                       |
| **Next-Hop**                | Usually NOT changed                       |
| **Use Case**                | Distributing external routes within AS    |

**Why Needed?**

- OSPF/EIGRP cannot carry BGP attributes
- External routing policies must be preserved
- iBGP distributes externally-learned routes internally

**Critical Rule:** **Full mesh** or **route reflectors** required

- Every iBGP router must peer with every other iBGP router
- **OR** use route reflectors to reduce mesh complexity

---

### BGP Peering Relationships

#### 1. Transit (Provider-Customer)

**Definition:** One AS provides Internet access/transit service to another AS.

**Example:**

```
Customer AS 100 ←→ Provider AS 200 ←→ Internet
```

**Characteristics:**

| Aspect                | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| **Payment**           | Customer pays provider                                              |
| **Routes Advertised** | Provider: Full table or default route<br/>Customer: Own routes only |
| **Traffic Flow**      | Both inbound and outbound transit                                   |
| **Relationship**      | Hierarchical                                                        |

**Policy:** Provider advertises customer routes to entire Internet.

---

#### 2. Peering (Settlement-Free Interconnection)

**Definition:** Two ASes exchange routes for mutual benefit without payment.

**Example:**

```
ISP AS 100 ←→ ISP AS 200 (peers)
```

**Types:**

| Type                | Location                      | Example                            |
| ------------------- | ----------------------------- | ---------------------------------- |
| **Public Peering**  | Internet Exchange Point (IXP) | Multiple ISPs at same facility     |
| **Private Peering** | Direct connection             | Dedicated circuit between two ASes |

**Characteristics:**

| Aspect               | Description                       |
| -------------------- | --------------------------------- |
| **Payment**          | None (settlement-free)            |
| **Routes Exchanged** | Own customers + own networks only |
| **Traffic**          | Balanced (typically)              |
| **Relationship**     | Peer-to-peer                      |

**Policy Rules:**

- **DO** advertise: Your own routes, your customers' routes
- **DO NOT** advertise: Routes learned from other peers, routes from providers

**Why Peer?**

- Reduced costs (bypass upstream providers)
- Lower latency (direct connection)
- Increased redundancy
- Better control over traffic

---

### BGP Best Path Selection

**Challenge:** BGP may receive multiple paths to same destination.

**Selection Process (in order):**

| Step | Attribute              | Preference                |
| ---- | ---------------------- | ------------------------- |
| 1    | **Reachable Next Hop** | Must be reachable         |
| 2    | **Weight** (Cisco)     | Highest                   |
| 3    | **Local Preference**   | Highest                   |
| 4    | **Local Origin**       | Prefer locally originated |
| 5    | **AS Path Length**     | Shortest                  |
| 6    | **Origin Type**        | IGP > EGP > Incomplete    |
| 7    | **MED**                | Lowest                    |
| 8    | **eBGP over iBGP**     | Prefer eBGP               |
| 9    | **IGP Metric**         | Lowest cost to next hop   |
| 10   | **Oldest Route**       | Prefer oldest (stability) |
| 11   | **Router ID**          | Lowest                    |
| 12   | **Neighbor IP**        | Lowest                    |

---

## IPv6 (Internet Protocol version 6)

### Motivation for IPv6

**Primary Driver:** IPv4 address exhaustion

**IPv4 Problems:**

| Problem                 | Impact                                         |
| ----------------------- | ---------------------------------------------- |
| **Address Space**       | 32-bit = ~4.3 billion addresses (insufficient) |
| **NAT Complexity**      | End-to-end connectivity broken                 |
| **Header Inefficiency** | Complex header with optional fields            |
| **No Native Security**  | IPsec added later                              |
| **Limited QoS**         | Basic Type of Service field                    |

---

### IPv6 Solutions

| Feature                | Improvement                                  |
| ---------------------- | -------------------------------------------- |
| **Address Space**      | 128-bit = 340 undecillion addresses          |
| **Simplified Header**  | Fixed 40-byte header, no checksum            |
| **Auto-configuration** | Stateless Address Auto-Configuration (SLAAC) |
| **Built-in Security**  | IPsec mandatory                              |
| **Better QoS**         | Flow Label field for traffic classification  |
| **No NAT Required**    | Every device can have global address         |
| **Efficient Routing**  | Hierarchical addressing, route aggregation   |

---

### IPv6 Address Format

#### Structure

**Format:** 128 bits written as **8 groups of 4 hexadecimal digits**

**Example:**

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

![IPv6 Address Format](attachments/Pasted%20image%2020260211222611.png)

#### Compression Rules

**Rule 1: Leading Zero Suppression**

```
2001:0db8:0000:0042 → 2001:db8:0:42
```

**Rule 2: Double Colon (::) for Consecutive Zeros**

```
2001:0db8:0000:0000:0000:0000:0000:0001
→ 2001:db8::1
```

**Important:** Double colon (::) can only be used **once** in an address.

---

### IPv6 Address Types

| Type               | Purpose                  | Example                  |
| ------------------ | ------------------------ | ------------------------ |
| **Unicast**        | One-to-one communication | 2001:db8::1              |
| **Global Unicast** | Internet-routable        | 2000::/3                 |
| **Link-Local**     | Single link only         | fe80::/10                |
| **Unique Local**   | Private (like RFC 1918)  | fc00::/7                 |
| **Multicast**      | One-to-many              | ff00::/8                 |
| **Anycast**        | Nearest of group         | (same format as unicast) |

**No Broadcast:** IPv6 uses multicast instead of broadcast.

---

### IPv6 Header Format

![IPv6 Header](attachments/Pasted%20image%2020260211222659.png)

#### Fixed Header: 40 Bytes

| Field                   | Size     | Description                                    |
| ----------------------- | -------- | ---------------------------------------------- |
| **Version**             | 4 bits   | Always 6 for IPv6                              |
| **Traffic Class**       | 8 bits   | Priority/QoS (like IPv4 ToS)                   |
| **Flow Label**          | 20 bits  | Identifies packet flow for QoS                 |
| **Payload Length**      | 16 bits  | Size of payload + extension headers            |
| **Next Header**         | 8 bits   | Type of next header (TCP/UDP/ICMPv6/extension) |
| **Hop Limit**           | 8 bits   | TTL equivalent (decremented by routers)        |
| **Source Address**      | 128 bits | Sender's IPv6 address                          |
| **Destination Address** | 128 bits | Receiver's IPv6 address                        |

---

#### Key Differences from IPv4

| Feature               | IPv4                   | IPv6                              |
| --------------------- | ---------------------- | --------------------------------- |
| **Header Size**       | 20-60 bytes (variable) | 40 bytes (fixed)                  |
| **Checksum**          | Included               | **Removed** (upper layers handle) |
| **Fragmentation**     | Router or source       | **Source only**                   |
| **Options**           | In main header         | **Extension headers**             |
| **Address Size**      | 32 bits                | 128 bits                          |
| **Header Processing** | Complex                | **Simplified**                    |

---

### IPv6 Extension Headers

**Purpose:** Optional headers between IPv6 header and payload for additional functionality.

**Chaining:** Multiple extension headers can be chained using **Next Header** field.

**Common Extension Headers:**

| Header Type             | Purpose                       | Use Case                             |
| ----------------------- | ----------------------------- | ------------------------------------ |
| **Hop-by-Hop Options**  | Processed by every router     | MLD (Multicast Listener Discovery)   |
| **Routing**             | Source routing                | Specify path through network         |
| **Fragment**            | Fragmentation information     | Large packets (source fragmentation) |
| **Destination Options** | Processed by destination only | Endpoint-specific options            |
| **Authentication (AH)** | Authentication and integrity  | IPsec                                |
| **ESP**                 | Encryption and authentication | IPsec                                |

**Order Matters:** Extension headers must appear in specific order (RFC 8200).

---

### IPv6 Address Compression Example

#### Original Address:

```
2001:0db8:0000:0000:0000:ff00:0042:8329
```

#### Step 1: Remove Leading Zeros

```
2001:db8:0:0:0:ff00:42:8329
```

#### Step 2: Apply :: (longest run of zeros)

```
2001:db8::ff00:42:8329
```

**Result:** Address reduced from 39 characters to 24 characters.

---

### IPv6 Subnetting

**Typical Allocation:**

| Entity                | Prefix Length | Addresses                |
| --------------------- | ------------- | ------------------------ |
| **Internet (Global)** | /3            | 2^125 addresses          |
| **ISP**               | /32           | 2^96 (billions of /64s)  |
| **Organization**      | /48           | 65,536 /64 subnets       |
| **Subnet**            | /64           | 18 quintillion addresses |
| **Host**              | /128          | Single address           |

**Standard:** Most networks use **/64** for subnets (allows SLAAC).

---

## MPLS (Multiprotocol Label Switching)

### Introduction

**Definition:** MPLS is a routing technique that uses short path **labels** instead of network addresses for forwarding decisions, improving speed and flexibility.

---

### Terminology

| Term                                   | Definition                                                                                               |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Label**                              | Short fixed-length identifier (20 bits) attached to packets                                              |
| **LSR (Label Switch Router)**          | Router supporting MPLS, performs label-based forwarding                                                  |
| **LER (Label Edge Router)**            | Entry/exit point of MPLS network<br/>• **Ingress LER:** Adds labels<br/>• **Egress LER:** Removes labels |
| **LSP (Label Switched Path)**          | Complete path through MPLS network defined by label sequence                                             |
| **FEC (Forwarding Equivalence Class)** | Group of packets receiving same forwarding treatment                                                     |

![Key Terminologies in MPLS](attachments/Pasted%20image%2020260211222731.png)

---

### MPLS Label Format

![MPLS Label Format](attachments/Pasted%20image%2020260211222810.png)

**Label Header:** 32 bits inserted between Layer 2 and Layer 3 headers

#### Field Breakdown

| Field                   | Size    | Description                                                |
| ----------------------- | ------- | ---------------------------------------------------------- |
| **Label Value**         | 20 bits | Actual label (2^20 = ~1 million possible values)           |
| **TC (Traffic Class)**  | 3 bits  | QoS priority, Experimental use                             |
| **S (Bottom of Stack)** | 1 bit   | **1** = last label in stack<br/>**0** = more labels follow |
| **TTL (Time to Live)**  | 8 bits  | Hop limit (prevents loops, like IP TTL)                    |

**Label Stack:** Multiple labels can be "stacked" for hierarchical routing/VPNs.

---

### MPLS Header Position

```
┌─────────────┬──────────────┬──────────────┬─────────┐
│ Layer 2 Hdr │  MPLS Label  │  IP Header   │ Payload │
│  (Ethernet) │   (32 bits)  │              │         │
└─────────────┴──────────────┴──────────────┴─────────┘
```

**"Shim Header":** MPLS label exists between Layer 2 and Layer 3, hence "2.5 layer protocol."

---

### MPLS Operations

![MPLS Operations](attachments/Pasted%20image%2020260211222906.png)

#### Core MPLS Operations

---

#### 1. PUSH (Label Imposition)

**Definition:** Adding an MPLS label to a packet.

**Performed By:** **Ingress LER** (Label Edge Router at entry)

**Process:**

| Step  | Action                                               |
| ----- | ---------------------------------------------------- |
| **1** | IP packet arrives at ingress LER                     |
| **2** | LER analyzes IP header (destination, QoS, etc.)      |
| **3** | Assigns packet to FEC (Forwarding Equivalence Class) |
| **4** | Looks up appropriate label for that FEC              |
| **5** | **Pushes label** onto packet (inserts MPLS header)   |
| **6** | Forwards labeled packet into MPLS network            |

**Example:**

```
Before: [Eth Header][IP Header][Data]
After:  [Eth Header][MPLS Label: 100][IP Header][Data]
```

**Purpose:** Converts IP packet to MPLS-forwarded packet.

---

#### 2. SWAP (Label Swapping)

**Definition:** Replacing incoming label with outgoing label.

**Performed By:** **Intermediate LSRs** (Label Switch Routers in core)

**Process:**

| Step  | Action                                                    |
| ----- | --------------------------------------------------------- |
| **1** | Labeled packet arrives with Label A                       |
| **2** | LSR looks up Label A in **Label Forwarding Table (LFIB)** |
| **3** | Table maps: Incoming Label A → Outgoing Label B, Next Hop |
| **4** | **Swaps** Label A with Label B                            |
| **5** | Forwards packet with Label B to next hop                  |
| **6** | Decrements TTL                                            |

**Example:**

```
Incoming: [Eth][Label: 100][IP][Data]
                  ↓
          Lookup in LFIB
                  ↓
Outgoing: [Eth][Label: 205][IP][Data]
```

**Key Feature:** **No IP lookup required** - only label table lookup (faster than routing table).

---

#### 3. POP (Label Disposition)

**Definition:** Removing MPLS label from packet.

**Performed By:** **Egress LER** (Label Edge Router at exit) or penultimate hop

**Process:**

| Step  | Action                                          |
| ----- | ----------------------------------------------- |
| **1** | Labeled packet arrives at egress LER            |
| **2** | LER recognizes itself as final MPLS destination |
| **3** | **Pops (removes)** MPLS label header            |
| **4** | Returns packet to standard IP format            |
| **5** | Performs normal IP routing to final destination |

**Example:**

```
Before: [Eth Header][MPLS Label: 300][IP Header][Data]
After:  [Eth Header][IP Header][Data]
```

**Optimization - Penultimate Hop Popping (PHP):**

- Label removed at **second-to-last router** instead of egress LER
- Egress LER receives native IP packet
- Reduces processing at egress
- Enabled by default in most implementations

---

### MPLS Forwarding Example

**Network Topology:**

```
PC A → R1 (Ingress LER) → R2 (LSR) → R3 (LSR) → R4 (Egress LER) → Server B
```

**Label Switched Path (LSP):** Labels assigned for path from A to B

| Router | Incoming Label     | Operation | Outgoing Label | Next Hop |
| ------ | ------------------ | --------- | -------------- | -------- |
| **R1** | (none - IP packet) | **PUSH**  | 100            | R2       |
| **R2** | 100                | **SWAP**  | 205            | R3       |
| **R3** | 205                | **SWAP**  | 300            | R4       |
| **R4** | 300                | **POP**   | (IP routing)   | Server B |

**Packet Transformation:**

```
A → R1: [IP: A→B][Data]
R1 → R2: [Label:100][IP: A→B][Data]     (PUSH)
R2 → R3: [Label:205][IP: A→B][Data]     (SWAP)
R3 → R4: [Label:300][IP: A→B][Data]     (SWAP)
R4 → B: [IP: A→B][Data]                 (POP)
```

![MPLS Working](attachments/Pasted%20image%2020260211222930.png)

---

### MPLS Label Distribution Protocols

**Question:** How do routers learn which labels to use?

**Answer:** Label Distribution Protocols establish LSPs by distributing label mappings.

#### Common Protocols

| Protocol                                                          | Type     | Use Case                                 |
| ----------------------------------------------------------------- | -------- | ---------------------------------------- |
| **LDP** (Label Distribution Protocol)                             | Standard | General MPLS label distribution          |
| **RSVP-TE** (Resource Reservation Protocol - Traffic Engineering) | Extended | MPLS Traffic Engineering, explicit paths |
| **BGP** (MP-BGP)                                                  | Extended | MPLS VPNs (VPNv4 routes + labels)        |

---

#### LDP (Label Distribution Protocol)

**Operation:**

1. **Neighbor Discovery:**
   - Routers send Hello messages (UDP port 646)
   - Discover other LDP-enabled routers

2. **Session Establishment:**
   - TCP session established (port 646)
   - Negotiate parameters

3. **Label Advertisement:**
   - Each router advertises labels for its FECs
   - Routers receive labels from downstream neighbors
   - Build LFIB tables

**Example:**

```
R2 advertises to R1: "For FEC 10.0.0.0/8, use Label 205"
R1 stores: Outgoing Label 205 → Next Hop R2 for 10.0.0.0/8
```

---

### MPLS Advantages

| Advantage                | Description                                  | Benefit                        |
| ------------------------ | -------------------------------------------- | ------------------------------ |
| **Fast Forwarding**      | Simple label lookup vs. longest-prefix match | Higher packet processing speed |
| **Traffic Engineering**  | Explicit path control through label stacking | Optimize network utilization   |
| **VPN Support**          | Label stacks enable L3 VPNs                  | Scalable private networks      |
| **QoS Integration**      | TC bits in label support QoS                 | Service differentiation        |
| **Protocol Independent** | Works with any Layer 3 protocol              | "Multiprotocol" in MPLS        |
| **Simplified Core**      | Core routers only do label lookups           | Reduced complexity             |
| **Scalability**          | Fewer routing table lookups in core          | Supports large networks        |

---

### MPLS Use Cases

#### 1. Traffic Engineering

**Purpose:** Control traffic paths beyond what standard routing protocols allow.

**How:**

- Define explicit paths using RSVP-TE
- Bypass congested links
- Load balancing across parallel paths

**Example:** Force specific customer traffic over high-bandwidth paths.

---

#### 2. Layer 3 VPNs

**Purpose:** Provide private IP connectivity across shared MPLS backbone.

**How:**

- Use **label stacks** (outer = transport, inner = VPN identifier)
- VPN routes isolated via VRFs (Virtual Routing and Forwarding)
- BGP distributes VPNv4 routes with labels

**Benefit:** Multiple customers share infrastructure securely.

---

#### 3. Fast Reroute (FRR)

**Purpose:** Sub-second failover when link/node fails.

**How:**

- Pre-compute backup LSPs
- Upon failure, instantly switch to backup path
- No routing protocol convergence wait

**Benefit:** High availability for critical traffic.

---

#### 4. Quality of Service (QoS)

**Purpose:** Prioritize specific traffic types (voice, video, data).

**How:**

- Use TC (Traffic Class) bits in label
- Different LSPs for different service classes
- Core routers honor TC bits for scheduling

**Benefit:** Consistent performance for latency-sensitive applications.

---

### MPLS vs Traditional IP Routing

| Aspect                  | Traditional IP Routing             | MPLS                           |
| ----------------------- | ---------------------------------- | ------------------------------ |
| **Forwarding Decision** | Longest prefix match (complex)     | Exact label match (simple)     |
| **Lookup Speed**        | Slower (routing table)             | Faster (label table)           |
| **Path Control**        | Limited (routing protocol decides) | Explicit (TE possible)         |
| **QoS Support**         | IP DSCP (limited)                  | TC bits + label-based (better) |
| **VPN Support**         | Complex (GRE, IPsec tunnels)       | Native (label stacks)          |
| **Scalability**         | Core routers need full routes      | Core routers only need labels  |
| **Protocol**            | Layer 3 only                       | Layer 2.5 (any L3 protocol)    |

---

### MPLS in Modern Networks

**Current Status:**

| Deployment            | Usage                                  |
| --------------------- | -------------------------------------- |
| **Service Providers** | Backbone infrastructure, VPN services  |
| **Enterprises**       | WAN connectivity via MPLS VPN services |
| **Data Centers**      | Less common (prefer VXLAN, EVPN)       |
| **5G Networks**       | Core transport (labeled routing)       |

**Future:**

- **Segment Routing (SR-MPLS):** Simplifies label distribution (no LDP/RSVP needed)
- **SRv6:** Segment Routing over IPv6 (labels encoded in IPv6 address)

---
