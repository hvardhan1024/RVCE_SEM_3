# Unit 5: Software-Defined Networking (SDN) and Advanced MPLS

---

## Introduction to Software-Defined Networking (SDN)

### Definition

**Software-Defined Networking (SDN):** Network architecture approach that enables networks to be **intelligently and centrally controlled**, or "programmed," using software applications.

**Core Concept:** **Separation** of the control plane from the data plane, allowing centralized network intelligence and programmability.

---

### Traditional Networking vs SDN

#### Traditional Networking Architecture

![Traditional Network Control](attachments/Pasted%20image%2020260211225659.png)

**Characteristics:**

| Aspect            | Traditional Approach                         |
| ----------------- | -------------------------------------------- |
| **Control Plane** | Distributed across all devices               |
| **Data Plane**    | Integrated with control plane in each device |
| **Configuration** | Manual, device-by-device (CLI)               |
| **Intelligence**  | Per-device routing decisions                 |
| **Scalability**   | Limited - complex as network grows           |
| **Flexibility**   | Low - vendor-specific, proprietary           |

**How It Works:**

1. Each router/switch runs routing protocols (OSPF, BGP, etc.)
2. Builds its own routing/forwarding table independently
3. Makes forwarding decisions locally
4. Administrator configures each device separately

**Problems:**

| Problem                | Impact                                               |
| ---------------------- | ---------------------------------------------------- |
| **Complex Management** | Configure hundreds/thousands of devices individually |
| **Slow Adaptation**    | Changes require manual reconfiguration               |
| **Vendor Lock-in**     | Proprietary CLIs and features                        |
| **Limited Visibility** | No centralized view of network state                 |
| **Policy Enforcement** | Difficult to apply consistent policies               |

---

#### SDN Architecture

![SDN Architecture](attachments/Pasted%20image%2020260211225741.png)

**Characteristics:**

| Aspect            | SDN Approach                                                |
| ----------------- | ----------------------------------------------------------- |
| **Control Plane** | **Centralized** in SDN Controller                           |
| **Data Plane**    | Simplified - just forwards based on controller instructions |
| **Configuration** | Programmatic via APIs (REST, NETCONF)                       |
| **Intelligence**  | Centralized network-wide view                               |
| **Scalability**   | High - controller manages all devices                       |
| **Flexibility**   | High - programmable, vendor-agnostic                        |

**How It Works:**

1. SDN Controller maintains network topology and state
2. Controller computes optimal paths centrally
3. Pushes forwarding rules to switches via southbound APIs
4. Switches simply forward packets based on rules
5. Applications interact with controller via northbound APIs

**Benefits:**

| Benefit                    | Description                                 |
| -------------------------- | ------------------------------------------- |
| **Centralized Management** | Single point of control for entire network  |
| **Programmability**        | Network behavior defined by software        |
| **Automation**             | Rapid provisioning, policy enforcement      |
| **Vendor Neutrality**      | Open standards (OpenFlow) reduce lock-in    |
| **Innovation**             | Easier to deploy new protocols and features |
| **Visibility**             | Complete real-time network view             |

---

## Control Plane vs Data Plane

### Overview

**All networking devices have two fundamental components:**

---

### 1. Control Plane

**Definition:** The "brain" of the network device - decides **how** packets should be handled.

**Responsibilities:**

| Function               | Description                | Examples                      |
| ---------------------- | -------------------------- | ----------------------------- |
| **Topology Discovery** | Learning network structure | LLDP, CDP                     |
| **Path Computation**   | Calculating best routes    | SPF, Dijkstra                 |
| **Protocol Operation** | Running routing protocols  | OSPF, BGP, EIGRP              |
| **Policy Application** | Implementing rules         | ACLs, QoS policies            |
| **Table Building**     | Creating forwarding tables | Routing table, FIB, MAC table |

**Key Point:** **Control plane builds the tables** that the data plane uses.

**In Traditional Router:**

- Runs on router CPU
- Processes routing protocol messages
- Builds routing table (RIB) and forwarding table (FIB)
- Consumes CPU cycles

**In SDN:**

- **Moved to centralized controller**
- Individual switches have minimal control plane
- Controller computes paths for entire network

---

### 2. Data Plane (Forwarding Plane)

**Definition:** The "muscle" of the network device - actually **forwards** packets based on control plane decisions.

**Responsibilities:**

| Function              | Description                            |
| --------------------- | -------------------------------------- |
| **Packet Reception**  | Receiving frames/packets               |
| **Table Lookup**      | Searching forwarding tables            |
| **Packet Forwarding** | Sending packets to correct output port |
| **Header Rewriting**  | Modifying MAC addresses, TTL           |
| **Encapsulation**     | Adding/removing headers                |

**Key Point:** **Data plane executes the instructions** from the control plane.

**In Traditional Router:**

- Uses FIB (Forwarding Information Base)
- Fast path switching (hardware-accelerated)
- Minimal CPU involvement

**In SDN:**

- **Flow tables** installed by controller
- Matches packet headers against rules
- Takes specified actions (forward, drop, modify)

---

### Control Plane vs Data Plane Comparison

![Software vs Hardware Forwarding](attachments/Pasted%20image%2020260211225942.png)

| Aspect                     | Control Plane                              | Data Plane                      |
| -------------------------- | ------------------------------------------ | ------------------------------- |
| **Primary Function**       | **Decides** how to forward                 | **Executes** forwarding         |
| **Speed**                  | Slower (software, CPU)                     | Very fast (hardware, ASIC)      |
| **Location (Traditional)** | Router CPU, software                       | Line cards, ASICs               |
| **Location (SDN)**         | **Centralized controller**                 | Switch hardware                 |
| **Traffic Handled**        | Control messages (OSPF Hello, BGP updates) | **User data packets**           |
| **Example Operations**     | Routing protocol, topology learning        | Packet forwarding, switching    |
| **Performance Impact**     | Can be slow (complex computations)         | Wire-speed (optimized hardware) |
| **Table Management**       | **Builds** routing/flow tables             | **Uses** routing/flow tables    |

---

### Key Relationship

```
┌─────────────────┐
│  Control Plane  │ ──builds──→ ┌────────────────┐
│  (Intelligence) │             │ Forwarding     │
└─────────────────┘             │ Tables         │
                                └────────────────┘
                                        │
                                   uses by
                                        ↓
                                ┌─────────────────┐
                                │   Data Plane    │
                                │  (Forwarding)   │
                                └─────────────────┘
```

---

### SDN's Innovation: Separation

**Traditional:** Control plane and data plane **tightly coupled** in each device.

**SDN:** Control plane **physically separated** and centralized.

**Advantages of Separation:**

| Advantage                  | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| **Network-Wide View**      | Controller sees entire topology, makes better decisions |
| **Centralized Policy**     | Apply policies consistently across all devices          |
| **Simplified Devices**     | Switches become simple forwarding elements              |
| **Faster Innovation**      | Upgrade controller software, not every switch           |
| **Easier Troubleshooting** | Single point to monitor and debug                       |

---

## OpenFlow Protocol

### Definition

**OpenFlow:** The first and most widely adopted **southbound API protocol** in SDN, enabling communication between the SDN controller and network switches.

**Purpose:** Standardized protocol for controller to program flow tables in switches.

---

### OpenFlow Architecture

![OpenFlow Architecture](attachments/Pasted%20image%2020260211230034.png)

**Components:**

| Component               | Role                  | Function                                       |
| ----------------------- | --------------------- | ---------------------------------------------- |
| **OpenFlow Controller** | Control plane         | Centralized network brain, computes flows      |
| **OpenFlow Switch**     | Data plane            | Forwards packets based on flow tables          |
| **OpenFlow Protocol**   | Communication channel | Secure channel between controller and switches |
| **Flow Tables**         | Forwarding rules      | Store match-action rules in switches           |

---

### OpenFlow Communication

**Protocol Details:**

| Aspect        | Details                                       |
| ------------- | --------------------------------------------- |
| **Transport** | TCP (typically port 6653, old: 6633)          |
| **Security**  | TLS/SSL supported                             |
| **Direction** | Bidirectional (controller ↔ switch)           |
| **Messages**  | Controller-to-Switch, Asynchronous, Symmetric |

---

### OpenFlow Messages

#### 1. Controller-to-Switch Messages

**Purpose:** Controller instructs or queries switches.

| Message Type      | Purpose                         | Example Use                            |
| ----------------- | ------------------------------- | -------------------------------------- |
| **Features**      | Query switch capabilities       | What tables, ports, buffers available? |
| **Configuration** | Set switch parameters           | Configure miss handling                |
| **Modify-State**  | Add/delete/modify flow entries  | Install forwarding rules               |
| **Read-State**    | Query statistics                | Get port stats, flow counts            |
| **Packet-Out**    | Send packet from switch         | Inject ARP reply, LLDP                 |
| **Barrier**       | Ensure message processing order | Synchronization point                  |

---

#### 2. Asynchronous Messages

**Purpose:** Switches notify controller of events.

| Message Type     | Purpose                   | When Sent                               |
| ---------------- | ------------------------- | --------------------------------------- |
| **Packet-In**    | Send packet to controller | No matching flow entry (table miss)     |
| **Flow-Removed** | Notify flow deletion      | Flow timeout or explicit deletion       |
| **Port-Status**  | Report port state change  | Link up/down, port configuration change |
| **Error**        | Report error condition    | Malformed message, action failure       |

**Most Important:** **Packet-In** - This is how controller learns about new flows and makes decisions.

---

#### 3. Symmetric Messages

**Purpose:** Bidirectional communication for health checking.

| Message Type     | Purpose              | Direction           |
| ---------------- | -------------------- | ------------------- |
| **Hello**        | Establish connection | Controller ↔ Switch |
| **Echo**         | Verify liveness      | Controller ↔ Switch |
| **Experimenter** | Vendor extensions    | Either direction    |

---

### OpenFlow Flow Table

![OpenFlow Flow Table](attachments/Pasted%20image%2020260211230114.png)

**Structure:** Each flow entry consists of:

---

#### Flow Entry Components

| Component                | Purpose                             | Example                                    |
| ------------------------ | ----------------------------------- | ------------------------------------------ |
| **Match Fields**         | Packet header fields to match       | IP src/dst, MAC src/dst, TCP ports, VLAN   |
| **Priority**             | Precedence (higher = checked first) | 100, 200, 32768                            |
| **Counters**             | Statistics                          | Packets matched, bytes transferred         |
| **Instructions/Actions** | What to do with matched packets     | Forward, drop, modify, send to controller  |
| **Timeouts**             | Idle timeout, hard timeout          | Remove entry after X seconds of inactivity |
| **Cookie**               | Opaque identifier                   | Controller uses for tracking               |

---

#### Match Fields (Examples)

| Layer       | Field                | Example Match                   |
| ----------- | -------------------- | ------------------------------- |
| **Layer 2** | Source MAC           | `00:11:22:33:44:55`             |
|             | Destination MAC      | `ff:ff:ff:ff:ff:ff` (broadcast) |
|             | VLAN ID              | `100`                           |
| **Layer 3** | Source IP            | `192.168.1.0/24`                |
|             | Destination IP       | `10.0.0.1`                      |
|             | IP Protocol          | `6` (TCP), `17` (UDP)           |
| **Layer 4** | TCP Source Port      | `80` (HTTP)                     |
|             | TCP Destination Port | `443` (HTTPS)                   |

**Wildcard Support:** Fields can be wildcarded (match any value) for broader rules.

---

#### Actions (Examples)

| Action           | Description                                 | Example                             |
| ---------------- | ------------------------------------------- | ----------------------------------- |
| **Output**       | Forward to port(s)                          | `output:3` (send to port 3)         |
| **Drop**         | Discard packet                              | (no output action)                  |
| **Modify Field** | Change header values                        | Set VLAN ID, rewrite IP, change MAC |
| **Push/Pop**     | Add/remove VLAN/MPLS tags                   | Push VLAN tag 100                   |
| **Set Queue**    | QoS queue assignment                        | High-priority queue                 |
| **Group**        | Forward to group (multipath, fast failover) | Group 5                             |
| **Controller**   | Send to controller                          | `output:CONTROLLER`                 |

---

### OpenFlow Packet Processing

**Flow:**

```
Packet arrives → Switch checks Flow Table(s)
                        │
                ┌───────┴────────┐
                │                │
          Match Found       No Match (Table Miss)
                │                │
         Execute Action    Send Packet-In to Controller
                │                │
      Forward/Drop/Modify    Controller decides action
                                 │
                          Installs flow entry
                                 │
                          Packet-Out (forward packet)
```

---

### Example OpenFlow Scenario

**Setup:**

- Host A (192.168.1.10) wants to reach Host B (192.168.1.20)
- Both connected to OpenFlow switch
- Controller manages switch

**Step-by-Step:**

| Step  | Entity     | Action                                                                                             |
| ----- | ---------- | -------------------------------------------------------------------------------------------------- |
| **1** | Host A     | Sends packet to Host B                                                                             |
| **2** | Switch     | Checks flow table - **no match**                                                                   |
| **3** | Switch     | Sends **Packet-In** to controller (includes packet headers)                                        |
| **4** | Controller | Analyzes packet, determines Host B on port 2                                                       |
| **5** | Controller | Sends **Flow-Mod** to install rule:<br/>`Match: dst IP = 192.168.1.20`<br/>`Action: output port 2` |
| **6** | Controller | Sends **Packet-Out** to forward original packet                                                    |
| **7** | Switch     | Installs flow entry, forwards packet                                                               |
| **8** | Future     | Subsequent A→B packets match rule, forwarded in hardware (fast path)                               |

**Result:** First packet goes to controller (slow), subsequent packets hardware-forwarded (fast).

---

## SDN Architecture Layers

![SDN Architecture Layers](attachments/Pasted%20image%2020260211230218.png)

### Three-Layer Model

---

### 1. Application Layer (Top)

**Purpose:** Network applications and services that define network behavior.

**Components:**

| Application Type           | Example                  | Function                          |
| -------------------------- | ------------------------ | --------------------------------- |
| **Traffic Engineering**    | MPLS TE, load balancing  | Optimize path selection           |
| **Security**               | Firewall, IDS/IPS        | Threat detection, blocking        |
| **Load Balancing**         | Server load balancer     | Distribute traffic across servers |
| **Network Virtualization** | Virtual networks (VxLAN) | Multi-tenant isolation            |
| **Monitoring**             | Network analytics        | Collect stats, visualize          |

**Interface:** **Northbound API** (REST, NETCONF, custom)

**Key Point:** Applications tell controller **what** they want; controller figures out **how**.

---

### 2. Control Layer (Middle)

**Purpose:** Centralized network intelligence and decision-making.

**Components:**

| Component                 | Function                              |
| ------------------------- | ------------------------------------- |
| **SDN Controller**        | Core network operating system         |
| **Topology Manager**      | Discovers and maintains network map   |
| **Path Computation**      | Calculates optimal routes             |
| **Policy Enforcement**    | Applies rules and constraints         |
| **Statistics Collection** | Gathers flow/port stats from switches |

**Popular Controllers:**

| Controller       | Language | Developer              | Features                              |
| ---------------- | -------- | ---------------------- | ------------------------------------- |
| **OpenDaylight** | Java     | Linux Foundation       | Modular, extensible, production-grade |
| **ONOS**         | Java     | ONF + Linux Foundation | Carrier-grade, high availability      |
| **Ryu**          | Python   | NTT                    | Lightweight, easy to extend           |
| **Floodlight**   | Java     | Big Switch Networks    | Simple, RESTful API                   |
| **POX**          | Python   | UC Berkeley            | Educational, prototyping              |

**Interfaces:**

- **Northbound API** (to applications)
- **Southbound API** (to infrastructure)

---

### 3. Infrastructure Layer (Bottom)

**Purpose:** Physical and virtual network devices that forward traffic.

**Components:**

| Device Type           | Role                                    | Examples                         |
| --------------------- | --------------------------------------- | -------------------------------- |
| **OpenFlow Switches** | Hardware switches with OpenFlow support | Open vSwitch, HP, Cisco          |
| **Virtual Switches**  | Software-based switches                 | Open vSwitch (OVS), Linux Bridge |
| **Legacy Devices**    | Traditional routers/switches            | Integrated via agents/proxies    |

**Interface:** **Southbound API** (OpenFlow, NETCONF, OVSDB, P4)

**Key Point:** Infrastructure layer just forwards packets; control plane is elsewhere.

---

### API Definitions

#### Northbound API

**Purpose:** Applications communicate with controller.

**Characteristics:**

| Aspect          | Details                                      |
| --------------- | -------------------------------------------- |
| **Direction**   | Application → Controller                     |
| **Protocols**   | REST (most common), NETCONF, custom          |
| **Data Format** | JSON, XML                                    |
| **Use Case**    | Request paths, install policies, query stats |

**Example REST Call:**

```
POST /controller/nb/v2/flow/add
{
  "match": {"nw_dst": "10.0.0.1"},
  "action": "output:2"
}
```

---

#### Southbound API

**Purpose:** Controller communicates with switches.

**Characteristics:**

| Aspect        | Details                                                  |
| ------------- | -------------------------------------------------------- |
| **Direction** | Controller ↔ Infrastructure                              |
| **Protocols** | OpenFlow (most common), NETCONF, OVSDB, P4               |
| **Use Case**  | Install flow rules, query port status, configure devices |

**Protocols:**

| Protocol     | Primary Use              | Adoption                       |
| ------------ | ------------------------ | ------------------------------ |
| **OpenFlow** | Flow table programming   | High (SDN standard)            |
| **NETCONF**  | Device configuration     | Growing (more flexible)        |
| **OVSDB**    | Open vSwitch management  | Common in virtual environments |
| **P4**       | Programmable data planes | Emerging (next-gen)            |

---

## Distributed vs Centralized Control Plane

### Overview

**Fundamental Design Choice:** Where should routing/forwarding intelligence reside?

---

### 1. Distributed Control Plane (Traditional)

![Distributed Control Plane](attachments/Pasted%20image%2020260211230258.png)

**Architecture:** Each network device has its own control plane.

#### Characteristics

| Aspect                    | Description                          |
| ------------------------- | ------------------------------------ |
| **Intelligence Location** | Every router/switch independently    |
| **Routing Protocols**     | OSPF, BGP, EIGRP run on each device  |
| **Table Building**        | Each device builds own routing table |
| **Communication**         | Peer-to-peer protocol messages       |
| **Decision Making**       | Local, per-device                    |

#### How It Works

**Example - OSPF:**

1. **Neighbor Discovery:** Each router discovers adjacent routers
2. **LSA Flooding:** Routers exchange link-state advertisements
3. **LSDB Construction:** Each router builds complete network topology
4. **SPF Calculation:** Each router independently runs Dijkstra
5. **FIB Installation:** Each router installs routes in forwarding table

**Key Point:** Every router does **same calculations independently**.

---

#### Advantages

| Advantage                      | Description                         |
| ------------------------------ | ----------------------------------- |
| **No Single Point of Failure** | Network survives controller failure |
| **Scalability**                | Distributed processing load         |
| **Fast Local Response**        | No controller communication needed  |
| **Mature Technology**          | Decades of proven operation         |
| **Vendor Support**             | Universal support                   |

---

#### Disadvantages

| Disadvantage                  | Description                        | Impact                              |
| ----------------------------- | ---------------------------------- | ----------------------------------- |
| **Complex Configuration**     | Each device configured separately  | High operational cost               |
| **Inconsistent Policies**     | Hard to enforce network-wide rules | Misconfigurations common            |
| **Slow Convergence**          | Distributed algorithms take time   | Service disruptions during failures |
| **Limited Visibility**        | No global network view             | Suboptimal routing decisions        |
| **Difficult Troubleshooting** | Must check each device             | Long MTTR (Mean Time To Repair)     |
| **Vendor Lock-In**            | Proprietary features/CLIs          | Limited flexibility                 |

---

### 2. Centralized Control Plane (SDN)

![Centralized Control Plane](attachments/Pasted%20image%2020260211230328.png)

**Architecture:** Single controller (or cluster) manages all devices.

#### Characteristics

| Aspect                    | Description                            |
| ------------------------- | -------------------------------------- |
| **Intelligence Location** | Centralized SDN controller             |
| **Routing Protocols**     | Controller runs algorithms             |
| **Table Building**        | Controller computes, pushes to devices |
| **Communication**         | Controller ↔ Switch (OpenFlow)         |
| **Decision Making**       | Global, network-wide                   |

#### How It Works

**Example - SDN:**

1. **Topology Discovery:** Controller uses LLDP to map network
2. **Centralized Computation:** Controller runs path algorithms
3. **Flow Installation:** Controller pushes flow rules to switches
4. **Packet Forwarding:** Switches forward based on installed rules
5. **Statistics Collection:** Controller monitors performance

**Key Point:** Controller does **single calculation**, distributes results.

---

#### Advantages

| Advantage                  | Description                                 |
| -------------------------- | ------------------------------------------- |
| **Simplified Management**  | Configure/monitor from single point         |
| **Network-Wide View**      | Complete topology visibility                |
| **Consistent Policies**    | Enforced uniformly across all devices       |
| **Programmability**        | Software-defined behavior                   |
| **Rapid Innovation**       | Deploy new features via controller software |
| **Easier Troubleshooting** | Centralized logging/monitoring              |
| **Optimal Routing**        | Global optimization possible                |

---

#### Disadvantages

| Disadvantage                | Description                            | Impact                      |
| --------------------------- | -------------------------------------- | --------------------------- |
| **Single Point of Failure** | Controller down = network issues       | Must deploy HA (clustering) |
| **Scalability Concerns**    | Controller must handle all devices     | May hit processing limits   |
| **Latency**                 | First packet to controller (slow path) | Initial flow setup delay    |
| **Controller Overhead**     | Bottleneck for control traffic         | Network capacity impact     |
| **Maturity**                | Newer technology                       | Less deployment experience  |

---

### Comparison Table

| Aspect                     | Distributed (Traditional)       | Centralized (SDN)              |
| -------------------------- | ------------------------------- | ------------------------------ |
| **Control Plane Location** | Each device                     | Controller                     |
| **Configuration**          | Per-device (CLI)                | Centralized (API)              |
| **Network View**           | Local only                      | **Global**                     |
| **Convergence**            | Slower (distributed algorithms) | **Faster** (pre-computed)      |
| **Failure Impact**         | Local (per-device)              | **Potentially network-wide**   |
| **Policy Enforcement**     | Difficult (manual)              | **Easy** (programmatic)        |
| **Troubleshooting**        | Complex (many devices)          | **Simpler** (centralized logs) |
| **Innovation Speed**       | Slow (hardware/firmware)        | **Fast** (software)            |
| **Maturity**               | **High** (decades)              | Lower (years)                  |
| **Scalability**            | **High** (distributed)          | Depends on controller          |

---

### Hybrid Approaches

**Reality:** Most modern networks use **both**:

| Scenario              | Approach                  | Reason                                     |
| --------------------- | ------------------------- | ------------------------------------------ |
| **Data Center**       | Centralized (SDN)         | Rapid provisioning, automation             |
| **WAN Core**          | Distributed (traditional) | Maturity, reliability                      |
| **SD-WAN**            | Hybrid                    | Centralized policy, distributed data plane |
| **Enterprise Campus** | Transitioning to SDN      | Gradual migration                          |

---

## Convergence in Networking

### Definition

**Convergence:** The time it takes for all routers in a network to agree on the best paths after a topology change (link failure, new link, router failure).

**Goal:** **Minimize convergence time** to reduce service disruption.

---

### Convergence Process

**Trigger:** Network change event (link down, new router).

**Steps:**

| Phase               | Activity                       | Time Impact             |
| ------------------- | ------------------------------ | ----------------------- |
| **1. Detection**    | Detect topology change         | Seconds (hello timeout) |
| **2. Propagation**  | Flood LSAs/updates             | Milliseconds to seconds |
| **3. Calculation**  | Each router runs SPF/algorithm | Seconds                 |
| **4. Installation** | Update FIB, hardware tables    | Milliseconds to seconds |

**Total Convergence:** Sum of all phases.

---

### Convergence Comparison

| Protocol  | Typical Convergence | Factors                                   |
| --------- | ------------------- | ----------------------------------------- |
| **RIP**   | 30-180 seconds      | 30-second update timer, hop-count limits  |
| **OSPF**  | 1-10 seconds        | Fast hello detection, efficient flooding  |
| **EIGRP** | < 1 second          | Feasible successors (pre-computed backup) |
| **BGP**   | Minutes             | Large tables, path exploration            |
| **SDN**   | **Milliseconds**    | Pre-computed, instant flow push           |

---

### Why Convergence Matters

**Impact of Slow Convergence:**

| Issue                      | Result                                    |
| -------------------------- | ----------------------------------------- |
| **Packet Loss**            | Traffic dropped during convergence        |
| **Routing Loops**          | Temporary loops during update propagation |
| **Application Disruption** | TCP timeouts, session failures            |
| **User Experience**        | Noticeable service interruption           |

**Example:** 30-second RIP convergence means 30 seconds of **downtime** after link failure.

---

### SDN Advantage: Fast Convergence

**Why SDN is Faster:**

| Reason                       | Explanation                                             |
| ---------------------------- | ------------------------------------------------------- |
| **Pre-Computation**          | Controller calculates backup paths in advance           |
| **Instant Push**             | Flow rules installed immediately upon failure detection |
| **No Distributed Consensus** | No waiting for protocol messages to propagate           |
| **Global View**              | Controller sees entire topology instantly               |

**Result:** SDN can achieve **sub-second** or even **millisecond** convergence.

**Technique:** **Fast Reroute (FRR)** - backup paths pre-installed, switched immediately.

---

## High Availability and Load Balancing

### High Availability (HA)

**Definition:** Ensuring network services remain operational despite failures.

**Techniques:**

| Technique                 | Description                 | Convergence Time     |
| ------------------------- | --------------------------- | -------------------- |
| **Redundancy**            | Duplicate paths/devices     | Depends on detection |
| **Fast Reroute**          | Pre-computed backup paths   | < 50 milliseconds    |
| **Link Aggregation**      | Multiple links as one (LAG) | Instant (sub-second) |
| **VRRP/HSRP**             | Virtual router redundancy   | 1-3 seconds          |
| **Controller Clustering** | Multiple SDN controllers    | Seconds              |

---

### Load Balancing

**Definition:** Distributing traffic across multiple paths/servers to optimize resource utilization.

**Benefits:**

| Benefit                | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| **Increased Capacity** | Utilize multiple links simultaneously                  |
| **Better Performance** | Avoid single-link congestion                           |
| **Fault Tolerance**    | Continue operation if one path fails                   |
| **Cost Efficiency**    | Use cheaper links in parallel vs single expensive link |

---

#### Load Balancing Methods

##### 1. ECMP (Equal-Cost Multi-Path)

**Concept:** Distribute traffic across multiple paths with **same cost**.

**Traditional Routing:**

| Aspect           | Details                                    |
| ---------------- | ------------------------------------------ |
| **Trigger**      | Multiple routes with identical metric      |
| **Distribution** | Per-flow (based on hash of packet headers) |
| **Limitation**   | Only equal-cost paths                      |

**Example:**

- Two paths to destination, both 3 hops
- OSPF installs both in FIB
- Traffic load-balanced using 5-tuple hash

---

##### 2. SDN-Based Load Balancing

**Advantages over ECMP:**

| Feature                  | ECMP               | SDN                                  |
| ------------------------ | ------------------ | ------------------------------------ |
| **Path Cost**            | Must be equal      | Can be unequal                       |
| **Traffic Distribution** | Hash-based (fixed) | **Policy-based (flexible)**          |
| **Dynamic Adjustment**   | No                 | **Yes** (controller sees congestion) |
| **Application-Aware**    | No                 | **Yes** (controller sees app type)   |

**Example Use Case:**

- Video traffic: Send via high-bandwidth path
- Web traffic: Send via low-latency path
- Controller monitors link utilization, shifts traffic dynamically

---

#### SDN Load Balancing Example

**Scenario:** Two paths to destination:

- **Path A:** 10 Gbps, 5ms latency, 50% utilized
- **Path B:** 1 Gbps, 2ms latency, 20% utilized

**Traditional (ECMP):**

- Cannot use both (costs different)
- Uses only one path

**SDN:**

- Controller installs flows based on policy:
  - Bulk data → Path A (high bandwidth)
  - Interactive traffic → Path B (low latency)
- Monitors utilization, shifts traffic if Path A congested

---

## MPLS in Detail

### MPLS Review

**Purpose:** Label-based forwarding for performance and traffic engineering.

---

### MPLS Label Operations (Revisited)

![MPLS Operations](attachments/Pasted%20image%2020260211230418.png)

---

#### 1. PUSH (Label Imposition)

**Performed By:** Ingress LER (Label Edge Router)

**Process:**

| Step  | Action             | Details                              |
| ----- | ------------------ | ------------------------------------ |
| **1** | Receive IP packet  | Packet enters MPLS network           |
| **2** | Classify into FEC  | Based on destination, QoS, VPN       |
| **3** | Lookup label       | Consult LIB (Label Information Base) |
| **4** | Insert MPLS header | Add 32-bit label between L2 and L3   |
| **5** | Forward to LSR     | Send to next hop in LSP              |

**Example:**

```
Before: [Eth][IP: 10.1.1.1 → 10.2.2.2][Data]
After:  [Eth][Label: 100][IP: 10.1.1.1 → 10.2.2.2][Data]
```

**FEC Mapping:**

| Traffic Type   | FEC   | Label Assigned |
| -------------- | ----- | -------------- |
| To 10.2.2.0/24 | FEC-1 | 100            |
| To 10.3.3.0/24 | FEC-2 | 200            |
| VPN Customer A | FEC-3 | 300            |

---

#### 2. SWAP (Label Swapping)

**Performed By:** Core LSR (Label Switch Router)

**Process:**

| Step  | Action                 | Details                                  |
| ----- | ---------------------- | ---------------------------------------- |
| **1** | Receive labeled packet | Packet arrives with Label X              |
| **2** | Lookup in LFIB         | Search Label Forwarding Information Base |
| **3** | Determine output       | Find outgoing label and next hop         |
| **4** | Replace label          | Swap Label X → Label Y                   |
| **5** | Decrement TTL          | Prevent loops                            |
| **6** | Forward packet         | Send to next LSR                         |

**LFIB Example:**

| Incoming Label | Incoming Interface | Outgoing Label | Outgoing Interface | Next Hop    |
| -------------- | ------------------ | -------------- | ------------------ | ----------- |
| 100            | Gig0/1             | 205            | Gig0/2             | 192.168.1.2 |
| 200            | Gig0/1             | 210            | Gig0/3             | 192.168.1.3 |
| 300            | Gig0/2             | 305            | Gig0/2             | 192.168.1.2 |

**Operation:**

- Packet arrives with Label 100 on Gig0/1
- LFIB lookup: 100 → 205
- Replace label: 100 becomes 205
- Forward on Gig0/2 to 192.168.1.2

**Key Point:** **No IP lookup required** - only label table lookup (faster).

---

#### 3. POP (Label Disposition)

**Performed By:** Egress LER (or penultimate LSR with PHP)

**Process:**

| Step  | Action                  | Details                       |
| ----- | ----------------------- | ----------------------------- |
| **1** | Receive labeled packet  | Last MPLS hop                 |
| **2** | Recognize egress status | Destination network reached   |
| **3** | Remove MPLS header      | Strip 32-bit label            |
| **4** | Expose IP header        | Return to native IP packet    |
| **5** | Perform IP routing      | Standard routing table lookup |
| **6** | Deliver to destination  | Forward to end host           |

**Example:**

```
Before: [Eth][Label: 300][IP: 10.1.1.1 → 10.2.2.2][Data]
After:  [Eth][IP: 10.1.1.1 → 10.2.2.2][Data]
```

---

#### Penultimate Hop Popping (PHP)

**Concept:** Remove label at **second-to-last** router, not egress LER.

**Why?**

| Reason                 | Benefit                                         |
| ---------------------- | ----------------------------------------------- |
| **Reduce Egress Load** | Egress LER doesn't do POP + IP lookup           |
| **Faster Processing**  | One less operation at busy egress point         |
| **Standard Behavior**  | Enabled by default in most MPLS implementations |

**How It Works:**

**Without PHP:**

```
R1 (PUSH) → R2 (SWAP) → R3 (SWAP) → R4 (POP + IP route)
```

**With PHP:**

```
R1 (PUSH) → R2 (SWAP) → R3 (POP) → R4 (IP route only)
```

**R3 (Penultimate Hop):**

- Receives labeled packet
- Recognizes next hop is egress
- Pops label before forwarding
- R4 receives native IP packet

**Result:** R4 (egress) only does IP routing, not label processing.

---

### MPLS Label Distribution

**Goal:** Routers must agree on label mappings to establish LSPs.

---

#### LDP (Label Distribution Protocol)

**Process:**

**1. Neighbor Discovery:**

```
Router A: "Hello, I'm LDP-capable" (UDP multicast 224.0.0.2)
Router B: "Hello, I'm LDP-capable"
```

**2. Session Establishment:**

```
Router A → Router B: TCP connection (port 646)
Negotiate: LDP version, label space, timers
```

**3. Label Advertisement:**

**Router B (downstream):**

- Has route to 10.0.0.0/8 via next hop
- Allocates label 300 for this FEC
- Advertises: "For FEC 10.0.0.0/8, use Label 300"

**Router A (upstream):**

- Receives advertisement
- Installs in LFIB:
  - Incoming traffic to 10.0.0.0/8 → PUSH label 300 → Forward to Router B

**4. Label Mapping:**

| Router | For FEC 10.0.0.0/8 | Incoming Label | Outgoing Label  | Next Hop    |
| ------ | ------------------ | -------------- | --------------- | ----------- |
| **R1** | (Ingress)          | None (IP)      | 100             | R2          |
| **R2** | (Core)             | 100            | 200             | R3          |
| **R3** | (Core)             | 200            | 300             | R4          |
| **R4** | (Egress)           | 300            | None (IP route) | Destination |

---

### MPLS Label Stack

**Feature:** MPLS supports **multiple labels** (stacked).

**Format:**

```
┌────────────────┐
│ Label 1 (Top)  │ ← Processed first (outer label)
├────────────────┤
│ Label 2        │
├────────────────┤
│ Label 3 (Bottom)│ ← S bit = 1 (last label)
├────────────────┤
│   IP Header    │
└────────────────┘
```

---

#### Use Cases

##### 1. MPLS VPNs

**Scenario:** Service provider offering Layer 3 VPNs to multiple customers.

**Label Stack:**

| Label                 | Purpose                                   |
| --------------------- | ----------------------------------------- |
| **Outer (Transport)** | Route through provider network (PE to PE) |
| **Inner (VPN)**       | Identify customer VPN                     |

**Example:**

```
Customer A Site 1 → Customer A Site 2
[Eth][Transport Label: 100][VPN Label: 500][IP][Data]
```

**Process:**

1. **Ingress PE:** Pushes VPN label (500), then transport label (100)
2. **Core routers:** Swap transport label only (don't see VPN label)
3. **Egress PE:** Pops transport label, sees VPN label 500, routes to Customer A

**Benefit:** Core routers don't need VPN routing tables (scalability).

---

##### 2. MPLS Traffic Engineering

**Scenario:** Establish explicit paths through network.

**Label Stack:**

| Label     | Purpose                         |
| --------- | ------------------------------- |
| **Outer** | TE tunnel label (explicit path) |
| **Inner** | Standard forwarding label       |

**Example:** Force traffic via specific routers to avoid congestion.

---

##### 3. Hierarchical MPLS

**Scenario:** Large network with multiple levels of hierarchy.

**Example:**

- Metro network
- Regional network
- Core network

**Label Stack:** Each level adds/removes labels.

---

### MPLS Advantages Revisited

| Advantage               | Description                                 | Use Case                   |
| ----------------------- | ------------------------------------------- | -------------------------- |
| **Fast Forwarding**     | Simple label lookup vs longest prefix match | Core routers               |
| **Traffic Engineering** | Explicit paths via label stacking           | Optimize bandwidth usage   |
| **VPN Support**         | Label stacks isolate customer traffic       | Service provider L3 VPNs   |
| **QoS Integration**     | TC bits in label for priority               | Voice/video prioritization |
| **Protocol Agnostic**   | Works with any L3 protocol (IP, IPv6, IPX)  | Multi-protocol networks    |
| **Fast Reroute**        | Pre-computed backup LSPs                    | High availability          |

---

### MPLS vs SDN

**Comparison:**

| Aspect                  | MPLS                                | SDN                          |
| ----------------------- | ----------------------------------- | ---------------------------- |
| **Forwarding Basis**    | Labels                              | Flow tables (match-action)   |
| **Control Plane**       | Distributed (LDP, RSVP)             | **Centralized** (controller) |
| **Programmability**     | Limited (protocols define behavior) | **High** (software-defined)  |
| **Granularity**         | Per-FEC (aggregate)                 | **Per-flow** (fine-grained)  |
| **Maturity**            | **Very mature** (decades)           | Emerging (years)             |
| **Use Case**            | SP core, enterprise WAN             | **Data center**, campus      |
| **Traffic Engineering** | RSVP-TE (complex)                   | **Controller** (simpler)     |

**Future:** **Segment Routing (SR)** combines MPLS-like forwarding with SDN-like centralized control.

---

## SDN Benefits Summary

| Benefit                 | Description                      | Impact                   |
| ----------------------- | -------------------------------- | ------------------------ |
| **Centralized Control** | Single point of management       | Simplified operations    |
| **Programmability**     | Software-defined behavior        | Rapid feature deployment |
| **Automation**          | API-driven provisioning          | Reduced manual work      |
| **Agility**             | Fast service deployment          | Business flexibility     |
| **Cost Reduction**      | Commodity hardware, reduced opex | Lower TCO                |
| **Innovation**          | Easier to test new protocols     | Competitive advantage    |
| **Visibility**          | Complete network view            | Better troubleshooting   |
| **Consistency**         | Uniform policy enforcement       | Fewer errors             |

---

## SDN Challenges

| Challenge              | Description                    | Mitigation                               |
| ---------------------- | ------------------------------ | ---------------------------------------- |
| **Controller Failure** | Single point of failure        | Deploy controller clusters (HA)          |
| **Scalability**        | Controller capacity limits     | Hierarchical controllers, load balancing |
| **Security**           | Centralized target for attacks | Secure channels (TLS), access control    |
| **Maturity**           | Newer technology               | Careful planning, phased deployment      |
| **Interoperability**   | Multi-vendor compatibility     | Use standard protocols (OpenFlow)        |
| **Performance**        | First packet to controller     | Flow caching, proactive rules            |

---

## SDN Use Cases

### 1. Data Center Networking

**Benefits:**

- Automated VM migration (network follows compute)
- Microsegmentation (security)
- Dynamic load balancing

**Examples:** VMware NSX, Cisco ACI, Juniper Contrail

---

### 2. SD-WAN (Software-Defined WAN)

**Benefits:**

- Dynamic path selection (choose best link)
- Application-aware routing
- Simplified branch office connectivity

**Examples:** Cisco Viptela, VMware SD-WAN, Fortinet Secure SD-WAN

---

### 3. Network Function Virtualization (NFV)

**Benefits:**

- Virtual firewalls, load balancers, routers
- Rapid service deployment
- Reduced hardware costs

**SDN Role:** Controller orchestrates NFV infrastructure.

---

### 4. 5G Mobile Networks

**Benefits:**

- Network slicing (dedicated virtual networks per service)
- Low latency (edge computing)
- Dynamic resource allocation

**SDN Role:** Control plane for 5G core network.

---

## Conclusion

**SDN represents a paradigm shift:**

- **From:** Distributed, manual, rigid networks
- **To:** Centralized, automated, programmable networks

**Key Takeaway:** Separation of control and data planes enables unprecedented network flexibility and innovation.

**Future:** SDN principles expanding beyond data centers to WAN, IoT, 5G, and beyond.

---
