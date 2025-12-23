# Unit 3: Mobile IP - Mobility, Routing and Addressing

---

## 1. Introduction to Mobility

### 1.1 What is Mobility?

**Mobile Computing**: System that allows computers to move from one location to another while maintaining network connectivity

**Key Characteristics:**
- Movement across long distances at required speeds
- Uses wireless technologies
- Maintains continuous connectivity during movement

### 1.2 Addressing Challenge in Mobility

**Core Problem**: When a mobile device moves from one network to another, its IP address-based location changes, but packets still need to reach it

**Traditional Scenario:**
- Desktop computer unplugged and moved to different network → requires IP reconfiguration
- This manual process is impractical for mobile devices that move frequently

---

## 2. Limitations of Conventional IP for Mobility

### 2.1 IP Address Structure

[[IP address format diagram showing 32-bit address divided into Network ID and Host ID]]

**IPv4 Address Components:**
- **32-bit address** divided into two parts:
  - **Network Identifier (Prefix)**: Identifies the network/subnet
  - **Host Identifier (Suffix)**: Identifies specific host within network

### 2.2 Why Conventional IP Fails for Mobile Hosts

**Problem Scenario:**
1. Mobile node has IP address 10.6.6.1 (belongs to subnet 10.6.6.x - Network A)
2. Mobile node moves to Network B (subnet 10.6.15.x)
3. Packets addressed to 10.6.6.1 are still routed to Network A
4. Mobile node cannot receive packets at new location

[[Diagram showing mobile node moving from subnet A (10.6.6.x) to subnet B (10.6.15.x) with packets still arriving at subnet A]]

**Why This Happens:**
- Routing is based on network prefix (10.6.6.x in this case)
- Routers deliver packets to subnet A based on address structure
- No mechanism in traditional IP to redirect packets to new location

**Consequences:**
- Mobile node becomes unreachable when it moves
- Cannot maintain ongoing connections
- Applications break when network changes

### 2.3 Solution: Mobile IP

**What it is**: IETF (Internet Engineering Task Force) standard that solves mobility problem through address redirection mechanism

**Core Concept**: Allows mobile host to use two IP addresses simultaneously
- **Home Address**: Permanent, remains constant
- **Care-of Address**: Temporary, changes with location

---

## 3. Mobile IP Characteristics

Mobile IP is designed with the following essential characteristics:

### 3.1 Transparency

**Definition**: Mobility is invisible to upper layers and uninvolved network components

**What it means:**
- Applications continue working without modification
- Transport layer (TCP/UDP) protocols unaffected
- User experiences no difference between wired and wireless connectivity
- Routers not involved in mobility don't need special configuration

**Benefit**: Existing applications work seamlessly with mobile devices

### 3.2 Scalability

**Definition**: Solution works across networks of any size

**What it means:**
- Supports mobility across global Internet
- Can handle large number of mobile users simultaneously
- No architectural limitations on network size
- Performance doesn't degrade significantly with more users

### 3.3 Security

**Definition**: Built-in authentication and security mechanisms

**Features:**
- All Mobile IP messages can be authenticated
- Prevents unauthorized access and hijacking
- Protects against malicious redirection of traffic
- Ensures only legitimate agents and nodes participate

### 3.4 Compatibility/Interoperability

**Definition**: Works with existing infrastructure without requiring universal upgrades

**What it means:**
- Mobile nodes can communicate with stationary hosts running standard IPv4
- Works with conventional routers and networks
- Mobile nodes can communicate with other mobile nodes
- No special software needed on correspondent nodes

### 3.5 Macro Mobility

**Definition**: Optimized for infrequent, long-duration moves

**Use Cases:**
- User takes laptop on business trip (stays at location for days/weeks)
- Device moves between branch offices
- Device relocates between cities/countries

**Note**: Not optimized for rapid handoffs (like moving car) - that's handled by link-layer protocols

| Characteristic | Description | Benefit |
|----------------|-------------|---------|
| **Transparency** | Invisible to applications and transport layer | No application changes needed |
| **Scalability** | Works across global Internet | Supports unlimited network size |
| **Security** | Authentication of all messages | Prevents unauthorized access |
| **Compatibility** | Works with standard IPv4 | No universal infrastructure upgrade |
| **Macro Mobility** | Long-duration moves | Efficient for typical business use |

---

## 4. Mobile IP Entities

### 4.1 Core Components

#### 1. Mobile Node (MN)

**What it is**: The mobile device that moves between networks

**Characteristics:**
- Can be mobile terminal (smartphone, laptop) or mobile router
- Assigned permanent IP address (home address)
- Receives packets via home address regardless of location
- Contains Mobile IP software for registration and communication

**Example**: Your smartphone, laptop, tablet

#### 2. Home Network

**What it is**: Original network to which mobile node belongs

**Characteristics:**
- Network whose prefix matches mobile node's home address
- MN's "permanent residence" in IP terms
- Contains home agent

**Example**: If MN's IP is 130.103.202.200, home network is 130.103.202.x

#### 3. Home Agent (HA)

**What it is**: Router on home network that tracks mobile node's location

**Responsibilities:**
- Maintains binding between MN's home address and current Care-of Address (COA)
- Intercepts packets destined for MN when it's away
- Tunnels intercepted packets to MN's current location
- Performs proxy ARP for mobile nodes away from home

**Location**: Always on the home network

**Example**: Router with IP 130.103.202.50 on home network 130.103.202.x

#### 4. Foreign Network

**What it is**: Network that mobile node is currently visiting (not its home network)

**Characteristics:**
- Temporary network attachment point
- May have foreign agent to assist mobile node
- Different network prefix than home network

**Example**: If MN from 130.103.202.x visits network 130.111.111.x, the latter is foreign network

#### 5. Foreign Agent (FA)

**What it is**: Router on foreign network that assists mobile node

**Responsibilities:**
- Advertises its presence to mobile nodes
- Provides Care-of Address to visiting mobile nodes
- De-encapsulates tunneled packets from HA
- Delivers packets to mobile node on local network
- Forwards registration requests to HA

**Location**: On the foreign network

**Note**: Foreign agent is optional - mobile node can work without it using co-located COA

#### 6. Correspondent Node (CN)

**What it is**: Any host communicating with the mobile node

**Characteristics:**
- Can be stationary or mobile
- Uses MN's home address for communication
- Unaware of MN's actual location
- No special Mobile IP software required

**Examples**: Web server, email server, another mobile device, desktop computer

[[Mobile IP entities diagram showing MN, CN, HA, FA, home network, and foreign network with their interconnections]]

### 4.2 Mobile IP Addresses

#### Primary/Home Address

**What it is**: Permanent, fixed IP address assigned to mobile node

**Characteristics:**
- Never changes regardless of location
- Obtained from mobile node's home network
- Used by applications and transport protocols
- Part of home network's address space
- Visible to correspondent nodes

**Purpose**: Provides stable identifier for mobile node

**Example**: 130.103.202.200

#### Secondary/Care-of Address (COA)

**What it is**: Temporary IP address used while visiting foreign network

**Characteristics:**
- Changes each time mobile node moves to new network
- Valid only while at current foreign location
- Used for routing packets to current location
- Sent to home agent during registration
- Topologically correct address on foreign network

**Purpose**: Indicates mobile node's current location for packet delivery

**Types of COA**: Foreign Agent COA and Co-located COA

---

## 5. Types of Care-of Address (COA)

### 5.1 Foreign Agent COA

**What it is**: Care-of Address that belongs to the foreign agent

**How it works:**
- FA provides its own IP address as COA
- FA's IP address is the tunnel endpoint
- Multiple mobile nodes can share same FA COA
- FA responsible for receiving and forwarding packets

**Process:**
1. MN discovers foreign agent
2. FA advertises its IP address
3. MN uses FA's IP as its COA
4. MN registers this COA with HA
5. HA tunnels packets to FA
6. FA de-encapsulates and delivers to MN

**Characteristics:**
- MN doesn't need unique IP on foreign network
- MN uses home address as source in outgoing packets
- FA maintains table mapping home addresses to MAC addresses
- No ARP used between FA and MN

### 5.2 Co-located COA

**What it is**: Care-of Address temporarily acquired by mobile node itself

**How it works:**
- MN obtains additional IP address on foreign network
- This address acts as COA
- Address is topologically correct for foreign network
- Tunnel endpoint is at the mobile node itself
- MN handles all tunneling and de-encapsulation

**Acquisition Methods:**
- DHCP (Dynamic Host Configuration Protocol)
- Manual configuration
- Other address allocation mechanisms

**Characteristics:**
- MN has unique IP address on foreign network
- MN uses two addresses simultaneously:
  - Home address for applications
  - COA for receiving datagrams
- MN must handle IP-in-IP encapsulation
- Works without foreign agent

### 5.3 Comparison: Foreign Agent COA vs Co-located COA

| Feature | Foreign Agent COA | Co-located COA |
|---------|-------------------|----------------|
| **Address Owner** | Belongs to FA | Belongs to MN |
| **Uniqueness** | Shared among multiple MNs | Unique to each MN |
| **Tunnel Endpoint** | At Foreign Agent | At Mobile Node |
| **FA Required** | Yes | No |
| **MN Software Complexity** | Lower | Higher (must handle tunneling) |
| **Address Acquisition** | Automatic from FA | Through DHCP or similar |
| **Packet Delivery** | FA → MN | Direct to MN |
| **Infrastructure Dependency** | Requires FA support | Works with standard routers |
| **Scalability** | Limited by FA capacity | Better (distributed) |
| **Mobile IP Software on MN** | Minimal | Complete (with tunneling) |

### 5.4 Advantages and Disadvantages

**Foreign Agent COA:**

Advantages:
- Simpler MN software
- Works in environments where MN can't obtain IP address
- FA can provide additional services

Disadvantages:
- Requires FA infrastructure
- FA becomes bottleneck
- Single point of failure

**Co-located COA:**

Advantages:
- No FA dependency
- Works with existing Internet infrastructure
- Standard routers treat MN as regular host
- More scalable
- Standard address allocation (DHCP) works

Disadvantages:
- MN needs more complex software
- MN must handle tunneling operations
- Requires ability to obtain IP address on foreign network
- Higher processing overhead on MN

---

## 6. Overview of Mobile IP Operation

### 6.1 Complete Operation Flow

**Initial Setup (Mobile Node at Home):**
- MN operates normally on home network
- Packets delivered directly via routing table
- No special Mobile IP processing needed

**When Mobile Node Moves to Foreign Network:**

**Step 1: Agent Discovery**
- MN listens for agent advertisements
- Discovers foreign agent (or determines it's on foreign network)

**Step 2: Obtain Care-of Address**
- MN obtains COA (from FA or via DHCP for co-located)

**Step 3: Registration**
- MN registers COA with home agent (via FA if using FA-COA)
- HA acknowledges registration
- HA creates binding: Home Address ↔ COA

**Step 4: Data Communication (CN → MN)**

1. **CN sends packet to MN's home address**
   - Source: CN's IP
   - Destination: MN's home address
   - CN unaware of MN's movement

2. **Packet arrives at home network**
   - Routed based on home address prefix
   - Reaches home agent

3. **HA intercepts packet**
   - Checks binding table
   - Finds MN is away (has COA)

4. **HA creates tunnel (IP-in-IP Encapsulation)**
   - Original packet becomes payload
   - New IP header added:
     - Source: HA's IP
     - Destination: COA
   - Encapsulated packet sent through tunnel

5. **Tunnel endpoint receives packet**
   - If FA-COA: Foreign agent receives
   - If co-located COA: Mobile node receives

6. **De-encapsulation**
   - Outer IP header removed
   - Original packet extracted

7. **Delivery to MN**
   - If FA-COA: FA forwards to MN using MAC address
   - If co-located: MN processes directly

**Step 5: Data Communication (MN → CN)**

1. **MN sends packet directly to CN**
   - Source: MN's home address (for transparency)
   - Destination: CN's IP
   - Sent via foreign network's router

2. **Packet routed normally to CN**
   - No tunneling needed
   - Standard routing based on destination

3. **CN receives packet**
   - Sees source as MN's home address
   - Unaware MN is mobile or away from home

[[Mobile IP operation diagram showing CN, HA, FA, and MN with packet flow paths labeled Path I (CN to HA), Path II (HA to FA via tunnel), and Path III (MN to CN direct)]]

### 6.2 Detailed Example Scenario

**Given:**
- MN's home address: 130.103.202.200
- Home network: 130.103.202.x
- Home Agent: 130.103.202.50
- Foreign network: 130.111.111.x
- Foreign Agent (COA): 130.111.111.111
- Correspondent Node: 192.7.7.1

**Communication Flow:**

**Path I: CN → HA**
- CN sends to 130.103.202.200
- Packet routed to network 130.103.202.x
- Arrives at HA (130.103.202.50)

**Path II: HA → FA (Tunnel)**
- HA encapsulates packet
- Outer header: Source = 130.103.202.50, Dest = 130.111.111.111
- Inner packet: Source = 192.7.7.1, Dest = 130.103.202.200
- Tunneled to FA

**Path III: MN → CN (Direct)**
- MN sends to CN directly
- Source: 130.103.202.200, Dest: 192.7.7.1
- Routed through foreign network's router
- Reaches CN via standard routing

### 6.3 Key Points

**Triangular Routing:**
- Packets from CN go via HA (not optimal)
- Packets from MN go directly to CN
- Creates asymmetric routing paths

**Transparency:**
- CN always uses MN's home address
- CN doesn't know MN has moved
- No changes needed in CN's software

**Tunnel Purpose:**
- Ensures packets reach MN at new location
- Encapsulation preserves original packet
- HA and FA/MN are tunnel endpoints

---

## 7. Mobile IP Support Services

### 7.1 Agent Discovery

**Purpose**: Mobile node must discover available agents when it moves to new network

**Mechanism**: Extended ICMP Router Discovery Protocol

#### How Standard Router Discovery Works:
- Routers periodically send ICMP Router Advertisement messages
- Hosts can send ICMP Router Solicitation to request immediate advertisement
- Helps hosts discover available routers on network

#### Mobile IP Extensions:

**Agent Advertisement:**
- Agents (HA or FA) send ICMP Router Advertisement periodically
- Additional **Mobility Agent Advertisement Extension** appended
- Mobile node identifies extension by checking IP datagram length > ICMP message length

**Agent Solicitation:**
- Mobile node sends ICMP Router Solicitation when:
  - Just powered on
  - Moved to new network
  - Haven't heard advertisements for some time
- Prompts agents to send advertisement immediately

**What Information is Discovered:**
- Agent's IP address
- Whether agent is HA, FA, or both
- Available Care-of Addresses (if FA)
- Registration lifetime supported
- Agent capabilities and preferences

### 7.2 Mobility Agent Advertisement Extension (MAE)

**Purpose**: Conveys Mobile IP capabilities of agent to mobile nodes

**Why Needed:**
- Standard router advertisement doesn't include Mobile IP information
- MN needs to know agent's Mobile IP capabilities
- Indicates whether agent can serve as HA or FA
- Provides COA information

#### Message Format:

[[Mobility Agent Advertisement Extension message format diagram]]

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |    Length     |        Sequence Number        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|    Registration Lifetime      |R|B|H|F|M|G|r|T|   Reserved    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                   Zero or more Care-of Addresses              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Field Descriptions:

| Field | Size | Description |
|-------|------|-------------|
| **Type** | 1 byte | Extension type identifier = 16 for Mobile Agent Advertisement |
| **Length** | 1 byte | Extension length in bytes (excluding Type and Length fields)<br>= 6 + (4 × number of COAs) |
| **Sequence Number** | 2 bytes | Counter starting at 0, incremented for each advertisement<br>Helps MN detect missed advertisements |
| **Registration Lifetime** | 2 bytes | Maximum time (seconds) agent accepts for registration<br>MN must re-register before expiry |
| **Flags** | 1 byte | See flag details below |
| **Reserved** | 1 byte | Set to 0, ignored on reception |
| **Care-of Addresses** | 4 bytes each | Zero or more COA addresses<br>FA must provide at least one<br>HA typically provides none |

#### Flag Bits:

[[Flag bits diagram showing R, B, H, F, M, G, r, T positions]]

| Flag | Name | Meaning When Set (1) |
|------|------|---------------------|
| **R** | Registration Required | MN must register even when using co-located COA |
| **B** | Busy | Agent currently not accepting registrations (too busy) |
| **H** | Home Agent | Agent can act as Home Agent for mobile nodes |
| **F** | Foreign Agent | Agent can act as Foreign Agent for mobile nodes |
| **M** | Minimal Encapsulation | Agent supports minimal encapsulation (RFC 2004) |
| **G** | GRE Encapsulation | Agent supports GRE encapsulation (RFC 1701) |
| **r** | Reserved | Reserved for future use, must be 0 |
| **T** | Tunneling | Agent supports reverse tunneling |

#### Detection Process:

**Mobile Node Determines Network Status:**
1. Receives ICMP Router Advertisement
2. Checks if Mobility Agent Extension present (datagram length check)
3. If no extension → standard router, MN on network without Mobile IP support
4. If extension present → Mobile IP agent available

**Mobile Node Determines if Moved:**
- Compares network prefix of advertised addresses with home address
- If prefix matches → on home network
- If prefix differs → on foreign network

**Example Scenario:**
- Agent busy, willing to work as FA
- Flag bits set: R=0, B=1, H=0, F=1, M=0, G=0, r=0, T=0
- Binary: 01000100
- Meaning: FA available but temporarily not accepting registrations

---

## 8. Agent Registration

### 8.1 Purpose and Process

**Why Registration is Needed:**
- Informs HA of MN's current location (COA)
- Establishes binding between home address and COA
- Authenticates mobile node and agents
- Sets up tunneling parameters

**When Registration Occurs:**
- MN moves to foreign network
- Periodic re-registration before lifetime expires
- MN returns to home network (de-registration)
- MN changes COA on same foreign network

### 8.2 Registration with Foreign Agent COA

**Step-by-Step Process:**

1. **MN → FA: Registration Request (RREQ)**
   - MN creates registration request
   - Contains: Home address, HA address, COA, Lifetime
   - Sent to FA via UDP (well-known port 434)

2. **FA → HA: Forwarded Registration Request**
   - FA records MN's MAC address and request details
   - Forwards request to HA (destination: HA's IP)
   - May add its own authentication extension

3. **HA Processing**
   - Authenticates request
   - Checks if can provide service
   - Creates/updates binding table entry: Home Address ↔ COA
   - Determines acceptance/rejection

4. **HA → FA: Registration Reply (RREP)**
   - Contains: Accept/reject status, lifetime granted
   - Sent to FA (at COA)
   - Includes authentication

5. **FA → MN: Forwarded Registration Reply**
   - FA updates its visitor list if accepted
   - Forwards reply to MN (using stored MAC address)
   - MN now registered and ready for communication

[[Agent registration diagram showing message flow: MN ↔ FA ↔ HA with RREQ and RREP]]

### 8.3 Registration with Co-located COA

**Simplified Process:**
- No FA involved
- MN sends registration request directly to HA
- HA sends registration reply directly to MN (at COA)
- MN handles tunneling itself

### 8.4 Registration Tables

**Home Agent Binding Table:**

| Home Address | Care-of Address | Lifetime Remaining | Registration Time |
|--------------|-----------------|-------------------|------------------|
| 130.103.202.200 | 130.111.111.111 | 3600 sec | 12:00:00 |

**Foreign Agent Visitor List:**

| Home Address | Home Agent | MAC Address | Lifetime Remaining |
|--------------|------------|-------------|-------------------|
| 130.103.202.200 | 130.103.202.50 | 00:1A:2B:3C:4D:5E | 3600 sec |

### 8.5 Re-registration

**Why Needed:**
- Registration has limited lifetime
- MN must re-register before expiration
- Maintains active binding at HA

**Process:**
- Similar to initial registration
- Occurs before lifetime expires
- Can request different lifetime
- HA may grant different lifetime than requested

### 8.6 De-registration

**When Occurs:**
- MN returns to home network
- MN powers down
- MN wants to terminate mobility service

**How:**
- MN sends registration request with lifetime = 0
- HA removes binding entry
- Packets delivered directly on home network

---

## 9. Registration Message Format

### 9.1 Registration Request Message

[[Registration request message format diagram]]

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |S|B|D|M|G|r|T|x|        Lifetime               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          Home Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          Home Agent                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Care-of Address                         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                                 |
+                         Identification                          +
|                                                                 |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Extensions...                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Field Descriptions:

| Field | Size | Description |
|-------|------|-------------|
| **Type** | 1 byte | Message type = 1 for Registration Request |
| **Flags** | 1 byte | Control flags (see below) |
| **Lifetime** | 2 bytes | Requested registration lifetime in seconds<br>0 = de-registration |
| **Home Address** | 4 bytes | MN's permanent IP address<br>Uniquely identifies mobile node |
| **Home Agent** | 4 bytes | IP address of mobile node's home agent<br>Where to send registration |
| **Care-of Address** | 4 bytes | MN's current location address<br>Where to tunnel packets |
| **Identification** | 8 bytes | 64-bit timestamp or nonce<br>Prevents replay attacks |
| **Extensions** | Variable | Authentication and other extensions<br>Must include MN-HA authentication |

#### Registration Request Flags:

[[Registration request flags diagram]]

| Flag | Name | Meaning When Set (1) |
|------|------|---------------------|
| **S** | Simultaneous Bindings | HA should retain previous bindings (multiple COAs) |
| **B** | Broadcast Datagrams | HA should tunnel broadcast packets to MN |
| **D** | Decapsulation by MN | MN using co-located COA (MN does de-capsulation) |
| **M** | Minimal Encapsulation | Requests minimal encapsulation (RFC 2004) |
| **G** | GRE Encapsulation | Requests GRE encapsulation (RFC 1701) |
| **r** | Reserved | Reserved, must be 0 |
| **T** | Reverse Tunneling | Requests reverse tunnel for MN→CN traffic |
| **x** | Reserved | Reserved, must be 0 |

### 9.2 Registration Reply Message

[[Registration reply message format diagram]]

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     Type      |     Code      |           Lifetime            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          Home Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          Home Agent                            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                                 |
+                         Identification                          +
|                                                                 |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         Extensions...                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

#### Field Descriptions:

| Field | Size | Description |
|-------|------|-------------|
| **Type** | 1 byte | Message type = 3 for Registration Reply |
| **Code** | 1 byte | Result of registration attempt (see codes below) |
| **Lifetime** | 2 bytes | Granted registration lifetime in seconds<br>May differ from requested lifetime |
| **Home Address** | 4 bytes | Copied from request, identifies mobile node |
| **Home Agent** | 4 bytes | HA's IP address, copied from request |
| **Identification** | 8 bytes | Copied from request, for matching reply to request |
| **Extensions** | Variable | Authentication and other extensions |

#### Registration Reply Codes:

| Code | Meaning | Category |
|------|---------|----------|
| **0** | Registration accepted | Success |
| **1** | Registration accepted, but with simultaneous bindings | Success |
| **64** | Reason unspecified | Denial by FA |
| **65** | Administratively prohibited | Denial by FA |
| **66** | Insufficient resources | Denial by FA |
| **67** | MN failed authentication | Denial by FA |
| **68** | HA failed authentication | Denial by FA |
| **69** | Requested lifetime too long | Denial by FA |
| **128** | Reason unspecified | Denial by HA |
| **129** | Administratively prohibited | Denial by HA |
| **130** | Insufficient resources | Denial by HA |
| **131** | MN failed authentication | Denial by HA |
| **132** | FA failed authentication | Denial by HA |
| **133** | Registration identification mismatch | Denial by HA |
| **134** | Poorly formed request | Denial by HA |

### 9.3 Example Registration Scenario

**Scenario:**
- MN: 18.7.7.1 (home network: 18.7.7.x)
- Moves to: 18.7.20.x
- FA: 18.7.20.7
- HA: 18.7.7.20

**Step 1: MN creates Registration Request**
- Type: 1
- Flags: D=0 (using FA-COA), other flags as needed
- Lifetime: 3600 (1 hour requested)
- Home Address: 18.7.7.1
- Home Agent: 18.7.7.20
- COA: 18.7.20.7
- Identification: Current timestamp
- Extensions: MN-HA authentication

**Step 2: FA's Visitor List (after forwarding request)**

| Home Address | Home Agent | MAC Address | Pending |
|--------------|------------|-------------|---------|
| 18.7.7.1 | 18.7.7.20 | AA:BB:CC:DD:EE:FF | Yes |

**Step 3: HA's Binding Table (after accepting registration)**

| Home Address | Care-of Address | Lifetime | Source |
|--------------|-----------------|----------|--------|
| 18.7.7.1 | 18.7.20.7 | 3600 | FA |

**Step 4: Registration Reply**
- Type: 3
- Code: 0 (accepted)
- Lifetime: 3600 (granted)
- Home Address: 18.7.7.1
- Home Agent: 18.7.7.20
- Identification: Copied from request

**Step 5: FA's Updated Visitor List**

| Home Address | Home Agent | MAC Address | Lifetime Remaining |
|--------------|------------|-------------|--------------------|
| 18.7.7.1 | 18.7.7.20 | AA:BB:CC:DD:EE:FF | 3600 sec |

---

## 10. Communication with Foreign Agent

### 10.1 The Address Problem

**Issue**: When using Foreign Agent COA, multiple mobile nodes share the same IP address (FA's IP)

**Question**: How can FA and MN communicate if MN doesn't have unique IP address on foreign network?

### 10.2 Solution: Link-Layer Communication

**Special Rules for FA-MN Communication:**

**MN → FA:**
- MN allowed to use its **home address** as IP source address
- Even though home address is not valid on foreign network
- Violates normal IP addressing rules (special Mobile IP exception)

**FA → MN:**
- FA allowed to use MN's **home address** as IP destination address
- FA cannot use ARP for this address (would fail)
- Must use stored MAC address from registration

### 10.3 How FA Maintains Address Mapping

**During Registration:**
1. MN sends registration request to FA
2. Request contains link-layer (MAC) address of MN
3. FA records mapping in visitor list:
   - Home Address ↔ MAC Address
4. Mapping maintained throughout registration lifetime

**Visitor List Example:**

| Home Address | Home Agent | MAC Address | Lifetime |
|--------------|------------|-------------|----------|
| 130.103.202.200 | 130.103.202.50 | 00:1A:2B:3C:4D:5E | 3500 sec |
| 130.103.202.201 | 130.103.202.50 | 00:1A:2B:3C:4D:5F | 2800 sec |

### 10.4 Packet Delivery: FA → MN

**Process:**
1. FA receives tunneled packet from HA
2. FA de-encapsulates packet
3. FA extracts destination (MN's home address)
4. FA looks up home address in visitor list
5. FA retrieves corresponding MAC address
6. FA delivers using **hardware unicast** to MAC address
7. No ARP needed - direct MAC-level delivery

**Why This Works:**
- All necessary information stored during registration
- Link-layer delivery bypasses IP routing
- Efficient - no ARP overhead

### 10.5 Key Points

| Aspect | Detail |
|--------|--------|
| **MN's Address** | Uses home address even on foreign network |
| **Address Uniqueness** | Not unique at IP level, unique at MAC level |
| **ARP Usage** | Not used between FA and MN |
| **Binding Mechanism** | Registration stores MAC address |
| **Delivery Method** | Hardware unicast using MAC address |
| **Duration** | Mapping valid for registration lifetime |

---

## 11. Datagram Transmission and Reception

### 11.1 Receiving Datagrams (CN → MN)

**Complete Path:**

1. **CN sends packet**
   - Destination: MN's home address
   - CN unaware of MN's location
   - Standard IP packet

2. **Internet routing**
   - Routers forward based on home address prefix
   - Packet reaches MN's home network
   - Arrives at home agent

3. **HA intercepts**
   - HA maintains binding table
   - Finds MN is away (has COA entry)
   - Decides to tunnel packet

4. **HA encapsulates (IP-in-IP)**
   - Original packet becomes payload
   - New outer header:
     - Source: HA's address
     - Destination: COA
   - Creates tunnel to foreign location

5. **Tunneled packet travels to COA**
   - Routed through Internet to foreign network
   - Reaches tunnel endpoint (FA or MN)

6. **De-encapsulation**
   - If FA-COA: FA removes outer header
   - If co-located: MN removes outer header
   - Original packet recovered

7. **Final delivery to MN**
   - If FA-COA: FA delivers using MAC address
   - If co-located: MN processes directly
   - MN receives packet addressed to home address

**Encapsulation Example:**

Original Packet:
```
[IP Header: SRC=192.7.7.1, DST=130.103.202.200][Data]
```

After HA Encapsulation:
```
[Outer IP: SRC=130.103.202.50, DST=130.111.111.111]
  [Inner IP: SRC=192.7.7.1, DST=130.103.202.200][Data]
```

### 11.2 Sending Datagrams (MN → CN)

**Direct Routing (Standard Case):**

1. **MN creates packet**
   - Source: MN's home address (for transparency)
   - Destination: CN's address
   - Standard IP packet

2. **Packet sent to foreign network router**
   - MN uses foreign network's default router
   - No tunneling required
   - No involvement of HA or FA

3. **Internet routing**
   - Standard routing to CN
   - Routers forward based on CN's address

4. **Packet reaches CN**
   - CN receives packet from MN's home address
   - CN unaware MN is mobile
   - Reply goes to home address (cycle continues)

**Key Point**: MN → CN traffic is NOT tunneled in standard Mobile IP

### 11.3 Packet Flow Summary

| Direction | Path | Encapsulation | Efficiency |
|-----------|------|---------------|------------|
| **CN → MN** | CN → Internet → HA → Tunnel → FA/MN | Yes (IP-in-IP) | Indirect (suboptimal) |
| **MN → CN** | MN → Foreign Router → Internet → CN | No | Direct (optimal) |

### 11.4 Why Asymmetric Routing?

**Incoming (CN → MN):**
- Must go through HA (only HA knows MN's location)
- Tunneling required to reach correct network
- Longer path

**Outgoing (MN → CN):**
- CN's address is globally routable
- Standard routing works
- Shorter path

**Result**: Triangular routing pattern (see next section)

---

## 12. The Two-Crossing Problem (2X Problem)

### 12.1 Problem Description

**What it is**: Inefficient routing where packets cross the Internet twice when mobile node and correspondent are topologically close

**Also called**: 
- Double Crossing Problem
- Triangle Routing Problem

### 12.2 Problem Scenario

**Setup:**
- Mobile node on foreign network
- Correspondent node on same foreign network (or nearby)
- Home network is geographically distant

**What Happens:**

1. **MN → Destination (Efficient)**
   - MN sends to nearby destination host D
   - Packet travels through local router R4
   - Directly delivered to D
   - **This path is efficient ✓**

2. **Destination → MN (Inefficient)**
   - D sends to MN's home address
   - Packet travels across Internet to home network (First crossing)
   - Reaches home agent R1
   - HA tunnels packet back across Internet to foreign network (Second crossing)
   - Finally delivered to MN
   - **This path crosses Internet twice ✗**

[[Two-crossing problem diagram showing MN and D on same network, but D→MN packets go via distant HA]]

### 12.3 Detailed Example

**Scenario:**
- MN visiting foreign network: 192.168.50.x
- MN's home network: 10.20.30.x (distant location)
- Destination host D: 192.168.50.100 (same network as MN)
- Home Agent: 10.20.30.1

**Packet Flow D → MN:**
1. D → 192.168.50.1 (local router R4)
2. R4 → Internet → 10.20.30.0 network
3. 10.20.30.1 (HA) → Internet → 192.168.50.x
4. Foreign network → MN

**Total**: Two Internet crossings for hosts on same local network!

### 12.4 Why This Happens

**Root Cause**: MN uses its home address for all communication

**Consequences:**
- D doesn't know MN's current location
- D's packets routed based on home address
- Standard routing delivers to home network
- HA must redirect back to actual location

### 12.5 Impact

**Performance Issues:**
- Higher latency (long round-trip time)
- More bandwidth consumption
- Higher packet loss probability
- Increased cost (two Internet crossings more expensive)

**When Most Problematic:**
- MN and CN on same local network
- MN and CN in same geographic region
- Home network very distant from current location

### 12.6 Comparison: Local vs Mobile IP Routing

| Scenario | Path | Crossings | Efficiency |
|----------|------|-----------|------------|
| **Both hosts local** | MN → R → D | 0 | Optimal |
| **Mobile IP (MN→D)** | MN → R → D | 0 | Optimal |
| **Mobile IP (D→MN)** | D → R → Internet → HA → Internet → MN | 2 | Poor |

### 12.7 Solutions (Advanced Topic)

**Route Optimization (Not in standard Mobile IP):**
- CN learns MN's COA
- CN caches binding: Home Address → COA
- CN tunnels packets directly to COA
- Bypasses HA

**Trade-offs:**
- More efficient routing
- But: Increases complexity, reduces transparency
- Requires changes to correspondent nodes
- Security challenges

**Why Not Standard:**
- Violates transparency principle
- Requires CN to have Mobile IP awareness
- Not all hosts can be modified

### 12.8 Acceptance of 2X Problem

**Why Tolerated:**
- Maintains transparency (key Mobile IP goal)
- No changes needed to correspondent nodes
- Works with entire Internet
- Suitable for macro mobility (infrequent moves)

**Design Trade-off**: Simplicity and universality vs. routing efficiency

---

## 13. Communication with Computers on Home Network

### 13.1 The Local Delivery Problem

**Scenario**: Host on MN's home network wants to send packet to MN

**Expected Behavior**: 
- Both hosts on same network
- Should use direct delivery (no routing)
- Sender ARPs for destination's MAC address

**Problem When MN is Away:**
- Sender ARPs for MN's MAC address
- MN not present to answer ARP
- Packet cannot be delivered

### 13.2 Home Agent's Role

**HA as Router on Home Network:**
- Connects home network to rest of Internet
- Positioned to intercept all traffic for home network
- Maintains bindings for mobile nodes

**Challenge**: HA must intercept local traffic too, not just routed traffic

### 13.3 Proxy ARP Solution

**What is Proxy ARP**: Technique where HA answers ARP requests on behalf of absent mobile node

**How Proxy ARP Works:**

1. **Local host sends ARP request**
   - "Who has IP address 130.103.202.200?"
   - Broadcast on home network
   - Seeks MN's MAC address

2. **Home Agent listens for ARP**
   - Monitors all ARP requests
   - Checks each target IP against binding table

3. **HA identifies absent MN**
   - Finds MN in binding table
   - Sees MN is registered at foreign location
   - Determines interception needed

4. **HA sends ARP reply**
   - "130.103.202.200 is at [HA's MAC address]"
   - Provides its own MAC address
   - Pretends to be the mobile node

5. **Local host receives reply**
   - Caches: MN's IP → HA's MAC
   - Unaware it's communicating with proxy

6. **Local host sends packet**
   - Encapsulates for MN's IP
   - Uses HA's MAC address
   - Direct Ethernet delivery to HA

7. **HA receives packet**
   - Packet arrives at link layer
   - HA recognizes it's for absent MN
   - HA tunnels to MN's COA (like other packets)

[[Proxy ARP diagram showing local host, HA, and MN with ARP request/reply]]

### 13.4 Transparency of Proxy ARP

**From Local Host's Perspective:**
- Normal ARP request and reply
- Normal packet transmission
- No awareness of MN's mobility
- No special software needed

**From MN's Perspective:**
- Receives packets from home network nodes
- No difference from packets from Internet

**Complete Transparency:** Local hosts unaffected by MN's mobility

### 13.5 Proxy ARP Example

**Scenario:**
- Home network: 130.103.202.x
- MN: 130.103.202.200 (currently away)
- HA: 130.103.202.50 (MAC: AA:BB:CC:DD:EE:FF)
- Local host: 130.103.202.100
- MN's COA: 130.111.111.111

**Step-by-Step:**

1. Host 130.103.202.100 wants to send to 130.103.202.200
2. Host broadcasts ARP: "Who has 130.103.202.200?"
3. HA sees request, checks binding table
4. HA replies: "130.103.202.200 is at AA:BB:CC:DD:EE:FF"
5. Host sends Ethernet frame to AA:BB:CC:DD:EE:FF
6. HA receives frame, extracts IP packet
7. HA tunnels to 130.111.111.111 (MN's COA)

### 13.6 Important Characteristics

| Aspect | Detail |
|--------|--------|
| **Who Performs** | Home Agent only |
| **When Active** | Only when MN is away (has COA) |
| **Scope** | Home network local delivery only |
| **Transparency** | Complete - local hosts unaware |
| **ARP Cache** | Local hosts cache HA's MAC for MN's IP |
| **Direction** | Incoming to MN only (outgoing doesn't need this) |

### 13.7 Why Necessary

**Without Proxy ARP:**
- Local hosts cannot reach absent MN
- ARP fails (no response)
- Communication impossible within home network

**With Proxy ARP:**
- Seamless mobility
- Local communication works identically
- HA transparently handles redirection

### 13.8 Key Takeaway

Proxy ARP is essential for Mobile IP to intercept **all** datagrams destined for mobile node, including those from local hosts on home network who would normally use direct delivery rather than routing through HA.

---

## 14. Private and Hybrid Networks

### 14.1 Single-Level Internet Architecture Issues

**Traditional Internet Architecture:**
- All organizations directly connected to global Internet
- All traffic potentially visible to others
- No inherent privacy between organizations

**Privacy Concerns:**
- Internal organizational data exposed
- Sensitive communication passes through external networks
- Security vulnerabilities
- Compliance issues (data protection regulations)

**Scenario:**
- Multi-site organization (Branch A, Branch B, Headquarters)
- Sites need to communicate
- Don't want data traversing public Internet

### 14.2 Private Network Architecture

**What it is**: Organization builds completely separate TCP/IP internet, isolated from global Internet

**Components:**
- Private routers
- Leased dedicated circuits between sites
- Private IP addressing
- No external connectivity

**Characteristics:**

| Feature | Detail |
|---------|--------|
| **Isolation** | Complete separation from Internet |
| **Privacy** | All data remains within organization |
| **IP Addresses** | Can use any IP addresses (even non-routable) |
| **Access** | No external access possible |
| **Security** | Physical and logical separation |

**Advantages:**
- ✓ Maximum privacy and security
- ✓ Complete control over infrastructure
- ✓ Flexible IP addressing
- ✓ No external threats
- ✓ Predictable performance

**Disadvantages:**
- ✗ No Internet access for users
- ✗ High cost (dedicated circuits, private routers)
- ✗ Cannot communicate with external partners
- ✗ Cannot access web, email, cloud services
- ✗ Difficult to scale

**Use Cases:**
- Military networks
- High-security government facilities
- Financial trading systems
- Critical infrastructure (not needing external access)

### 14.3 Hybrid Network Architecture

**What it is**: Combines private networking benefits with global Internet connectivity

**Two-Level Architecture:**
1. **Internal Level**: Private communication within organization
2. **External Level**: Connectivity to global Internet

**How it Works:**
- Uses globally valid IP addresses (routable on Internet)
- Each site has Internet connection
- Traffic classified as internal or external
- Internal traffic stays private
- External traffic goes via Internet

[[Hybrid network diagram showing multiple sites with dual connections - internal private links and external Internet connections]]

**Implementation:**

**Site Configuration:**
- Two types of connections per site:
  - Private leased lines to other org sites
  - Internet connection via ISP
- Routers configured to distinguish traffic types

**Routing Strategy:**
- Internal destinations → Private links
- External destinations → Internet connection

**Example:**
- Branch A (Boston) to Branch B (Chicago): Private link
- Branch A (Boston) to customer website: Internet
- HQ (New York) to Branch offices: Private links

### 14.4 Hybrid Network Advantages

**Privacy:**
- Internal communication never exposed to public Internet
- Sensitive data protection
- Compliance with regulations

**Connectivity:**
- Users can access Internet resources
- Email, web browsing, cloud services available
- External partner communication possible

**Flexibility:**
- Best of both worlds
- Adaptable to different needs

**Security:**
- Internal traffic isolated
- External traffic can be secured (VPN, firewalls)

### 14.5 Hybrid Network Challenges

**Complexity:**
- More complex routing configuration
- Traffic classification required
- Managing dual infrastructure

**Cost:**
- Still requires private circuits
- Plus Internet connections
- Higher than Internet-only

**Address Management:**
- Must use globally routable addresses
- Cannot use private address spaces (10.x.x.x, 192.168.x.x)
- More expensive addressing

### 14.6 Comparison: Private vs Hybrid vs Public

| Feature | Private Network | Hybrid Network | Public (Internet-only) |
|---------|----------------|----------------|------------------------|
| **Privacy** | Maximum | High (internal traffic) | Low |
| **Internet Access** | None | Yes | Yes |
| **Cost** | Very High | High | Low |
| **Complexity** | Medium | High | Low |
| **Security** | Maximum | High | Depends on measures |
| **Scalability** | Difficult | Moderate | Easy |
| **IP Addressing** | Any | Globally valid | Globally valid |
| **Use Case** | High security, no external needs | Large organizations | General use |

### 14.7 Mobile IP in Private/Hybrid Networks

**Considerations:**

**Private Networks:**
- Mobile IP can work entirely within private network
- Home agents and foreign agents all private
- No Internet involvement

**Hybrid Networks:**
- Mobile node may roam between:
  - Internal private sites (use private links)
  - External locations (use Internet)
- HA can tunnel via private links or Internet based on COA
- More complex but flexible

**Configuration:**
- HA must have connectivity to both internal and external networks
- Tunneling path selection based on COA location

---

## 15. Summary and Key Concepts

### 15.1 Mobile IP Problem and Solution

**Problem**: Traditional IP addressing fails when hosts move between networks

**Solution**: Mobile IP allows host to maintain permanent address while moving

**Mechanism**: Two-address scheme (home address + care-of address)

### 15.2 Essential Components

| Component | Role |
|-----------|------|
| **Mobile Node** | Device that moves |
| **Home Agent** | Tracks MN, tunnels packets |
| **Foreign Agent** | Assists MN at foreign location (optional) |
| **Care-of Address** | Temporary location address |
| **Tunneling** | Delivers packets to current location |

### 15.3 Key Processes

1. **Agent Discovery**: MN finds FA/determines network
2. **Registration**: MN informs HA of COA
3. **Tunneling**: HA encapsulates packets to COA
4. **Delivery**: FA or MN receives and de-encapsulates

### 15.4 Important Features

**Transparency:**
- Applications unaware of mobility
- Correspondent nodes don't need modification
- Uses standard IPv4

**Trade-offs:**
- Simplicity vs efficiency (2X problem)
- Transparency vs optimized routing
- Universal compatibility vs performance

### 15.5 Critical Understanding Points

**Addressing:**
- Home address = permanent identity
- COA = temporary location
- Two addresses used simultaneously

**Routing:**
- CN → MN: indirect via HA (tunneled)
- MN → CN: direct (not tunneled)
- Asymmetric paths

**Two Types of COA:**
- FA-COA: Shared, FA delivers to MN
- Co-located: Unique to MN, MN handles tunneling

**Special Mechanisms:**
- Proxy ARP: For home network delivery
- No ARP between FA-MN: Uses stored MAC addresses
- Registration: Maintains bindings and mappings

### 15.6 Exam-Focused Concepts

**Address Identification:**
- Given IP and subnet, identify: HA, FA, COA, home address
- Home network = network matching home address prefix
- Foreign network = current network if different from home

**Packet Paths:**
- Path I: CN to HA (normal routing)
- Path II: HA to FA/MN (tunnel)
- Path III: MN to CN (direct)

**Message Formats:**
- Agent Advertisement Extension: Type, flags, COA
- Registration Request/Reply: Addresses, lifetime, codes

**Calculations:**
- Lifetime remaining
- Re-registration timing
- Message field values

**Problem Scenarios:**
- 2X problem: when and why
- Proxy ARP: when necessary
- FA-MN communication: how addressing works