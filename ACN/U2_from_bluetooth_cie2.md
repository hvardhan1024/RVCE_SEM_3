# Unit 2: Bluetooth, ZigBee, and Wireless WANs/MANs

---

## 1. Bluetooth Technology

### 1.1 Overview
- **What it is**: Short-range wireless technology for connecting devices (typically 10m range)
- **Purpose**: Cable replacement, ad-hoc networking between personal devices
- **Operates in**: 2.4 GHz ISM band (unlicensed)
- **Standard**: IEEE 802.15.1

### 1.2 Bluetooth Specifications

| Parameter | Specification |
|-----------|---------------|
| Frequency Band | 2.4 GHz ISM (2.402 - 2.480 GHz) |
| Number of Channels | 79 channels (1 MHz each) |
| Modulation | GFSK (Gaussian Frequency Shift Keying) |
| Data Rate | 1 Mbps (Basic), up to 3 Mbps (EDR - Enhanced Data Rate) |
| Range | Class 1: 100m, Class 2: 10m, Class 3: 1m |
| Power Consumption | Very low (milliwatts) |
| Topology | Piconet and Scatternet |
| Maximum Devices | 8 active devices per piconet |

### 1.3 Bluetooth Network Topology

#### Piconet
- **What it is**: Basic Bluetooth network unit consisting of one master and up to 7 active slaves
- **Structure**: Master-slave architecture
- **Master**: Controls the piconet, provides clock synchronization
- **Slaves**: Synchronized to master's clock and frequency hopping pattern
- **Parked Devices**: Up to 255 devices can be in parked (inactive) state

**Piconet Formation Steps:**
1. **Inquiry Phase**: Master device searches for nearby Bluetooth devices
2. **Inquiry Scan**: Slave devices listen for inquiry messages
3. **Paging Phase**: Master pages specific devices to establish connection
4. **Connection Establishment**: Slaves synchronize with master's clock and hopping sequence
5. **Active Communication**: Data exchange begins with master controlling channel access

[[Piconet diagram showing 1 master (center) connected to 7 slaves in star topology]]

#### Scatternet
- **What it is**: Network formed by interconnecting multiple piconets
- **How**: A device can be a slave in one piconet and master in another, or slave in multiple piconets
- **Purpose**: Extends network coverage and capacity beyond single piconet limitations

[[Scatternet diagram showing multiple overlapping piconets with bridge devices]]

### 1.4 Bluetooth Protocol Stack

The Bluetooth protocol stack is divided into layers:

| Layer | Protocols/Components | Function |
|-------|---------------------|----------|
| **Application Layer** | User Applications | End-user services |
| **Middleware** | OBEX, WAP | Object exchange, wireless applications |
| **Upper Layers** | SDP, RFCOMM, TCS | Service discovery, serial port emulation, telephony |
| **Transport Protocol Group** | L2CAP | Logical Link Control and Adaptation Protocol |
| **Baseband & Link Manager** | LMP, Baseband | Link management, packet formatting, timing |
| **Physical Layer** | Radio | RF transmission at 2.4 GHz |

---

## 2. Transport Protocol Group

### 2.1 L2CAP (Logical Link Control and Adaptation Protocol)

**Purpose**: Provides connection-oriented and connectionless data services to upper layers

**Key Functions:**
- **Segmentation and Reassembly**: Breaks large packets into smaller baseband packets
- **Multiplexing**: Allows multiple protocols to share the same physical link
- **Quality of Service (QoS)**: Negotiates and manages connection parameters
- **Group Management**: Supports point-to-multipoint communication

| Feature | Description |
|---------|-------------|
| PDU Size | Up to 64 KB |
| Channel Types | Connection-oriented, Connectionless |
| Flow Control | Credit-based for connection-oriented channels |
| Error Control | Retransmission and error detection |

### 2.2 RFCOMM (Radio Frequency Communication)

**What it is**: Serial port emulation protocol

**Purpose**: Provides serial port interface over Bluetooth (replaces RS-232)

**Characteristics:**
- Emulates 9-pin RS-232 serial cable
- Supports up to 60 simultaneous connections
- Used by profiles requiring serial data transfer

### 2.3 SDP (Service Discovery Protocol)

**Purpose**: Allows devices to discover available services on other Bluetooth devices

**How it works:**
1. Client sends service search request
2. Server responds with list of matching services
3. Client can request detailed service attributes
4. Connection established to desired service

---

## 3. ZigBee Technology

### 3.1 Overview
- **What it is**: Low-power, low-data-rate wireless standard for IoT and sensor networks
- **Standard**: IEEE 802.15.4
- **Purpose**: Home automation, industrial control, wireless sensor networks
- **Key Feature**: Very low power consumption (battery life: months to years)

### 3.2 ZigBee Specifications

| Parameter | Specification |
|-----------|---------------|
| Frequency Bands | 2.4 GHz (global), 915 MHz (Americas), 868 MHz (Europe) |
| Data Rate | 250 kbps (2.4 GHz), 40 kbps (915 MHz), 20 kbps (868 MHz) |
| Range | 10-100 meters |
| Network Size | Up to 65,000 nodes |
| Topology | Star, Tree, Mesh |
| Power Consumption | Very low (microamps in sleep mode) |
| Modulation | DSSS (Direct Sequence Spread Spectrum) |

### 3.3 ZigBee Device Types

| Device Type | Role | Characteristics |
|-------------|------|-----------------|
| **ZigBee Coordinator (ZC)** | Network initiator and manager | One per network, always powered, stores network info |
| **ZigBee Router (ZR)** | Route discovery and message forwarding | Always powered, extends network range |
| **ZigBee End Device (ZED)** | Sensor/actuator node | Can sleep, battery-powered, lowest power consumption |

### 3.4 ZigBee Network Topologies

| Topology | Description | Use Case |
|----------|-------------|----------|
| **Star** | All devices connect to central coordinator | Simple home automation |
| **Tree** | Hierarchical structure with routers extending network | Building automation |
| **Mesh** | Devices interconnected with multiple paths | Industrial, large-scale deployments |

[[ZigBee network topologies diagram showing star, tree, and mesh configurations]]

### 3.5 ZigBee vs Bluetooth Comparison

| Feature | ZigBee | Bluetooth |
|---------|--------|-----------|
| Data Rate | 20-250 kbps | 1-3 Mbps |
| Range | 10-100m | 1-100m |
| Power Consumption | Very Low | Low to Medium |
| Network Size | 65,000 nodes | 8 active devices per piconet |
| Topology | Star, Tree, Mesh | Piconet, Scatternet |
| Application | IoT, Sensors, Automation | Audio, File transfer, Peripherals |
| Complexity | Simple | More complex |
| Cost | Lower | Higher |

---

## 4. Wireless WANs and MANs

### 4.1 The Cellular Concept

**What it is**: Method of dividing a geographic area into smaller regions called cells, each served by a base station

**Why it's needed:**
- Limited radio spectrum available
- Need to support large number of users
- Maximize spectrum utilization through frequency reuse

**Key Principle**: Same frequencies can be reused in non-adjacent cells (frequency reuse)

#### Cellular Concept Benefits

| Benefit | Explanation |
|---------|-------------|
| **Increased Capacity** | Frequency reuse allows serving more users with same spectrum |
| **Limited Transmit Power** | Smaller cells require less power, reduces interference |
| **Scalability** | Network can grow by adding more cells |
| **Better Coverage** | Smaller cells provide more uniform coverage |

[[Cellular network diagram showing hexagonal cell layout with frequency reuse pattern]]

### 4.2 Cellular Architecture

#### Components

**1. Mobile Station (MS)**
- User device (phone, modem)
- Contains subscriber identity (SIM card)
- Communicates with base station

**2. Base Station Subsystem (BSS)**
- **Base Transceiver Station (BTS)**: Radio equipment, antennas
- **Base Station Controller (BSC)**: Controls multiple BTSs, manages handoffs

**3. Network Switching Subsystem (NSS)**
- **Mobile Switching Center (MSC)**: Switching and routing calls
- **Home Location Register (HLR)**: Database of subscriber information
- **Visitor Location Register (VLR)**: Temporary database for roaming users
- **Authentication Center (AUC)**: Security and authentication

**4. Operation and Support Subsystem (OSS)**
- Network management and monitoring

[[Cellular architecture diagram showing MS, BTS, BSC, MSC, HLR, VLR components and their connections]]

### 4.3 Cell Shapes and Cluster Concept

#### Hexagonal Cell Layout
- **Why hexagonal?** Provides better coverage with no gaps or overlaps compared to circles or squares
- **Cell radius**: Distance from base station to cell edge

#### Cluster
- **What it is**: Group of cells using entire set of available frequencies
- **Cluster size (N)**: Number of cells in a cluster
- **Common values**: N = 3, 4, 7, 12, 21

**Frequency Reuse Distance (D)**: Minimum distance between centers of cells using same frequencies

**Relationship**: D = R√(3N)
- D = reuse distance
- R = cell radius
- N = cluster size

| Cluster Size (N) | Co-channel Interference | Capacity per Cluster |
|------------------|-------------------------|---------------------|
| 3 | Higher | Lower |
| 7 | Moderate | Moderate |
| 12 | Lower | Higher |

[[Hexagonal cell cluster diagram showing N=7 cluster pattern with frequency groups]]

### 4.4 Capacity Enhancement Techniques

#### 1. Cell Splitting
- **What**: Dividing large cells into smaller cells
- **How**: Replace single base station with multiple lower-power base stations
- **Result**: Increased capacity by allowing more frequency reuse
- **Consideration**: Smaller cells require more base stations (higher cost)

#### 2. Cell Sectoring
- **What**: Dividing a cell into sectors using directional antennas
- **Typical configuration**: 3 or 6 sectors per cell (120° or 60° antennas)
- **Result**: Reduces co-channel interference, increases capacity
- **Advantage**: Less expensive than cell splitting

#### 3. Microcells and Picocells
- **Microcells**: Very small cells for high-traffic areas (shopping malls, stadiums)
- **Picocells**: Indoor cells for buildings
- **Purpose**: Provide additional capacity in hotspots

[[Cell splitting diagram showing large cell divided into smaller cells]]

[[Cell sectoring diagram showing cell divided into 3 sectors with 120° directional antennas]]

### 4.5 Capacity Calculation

**Basic Formula:**
```
Total Users = (Total Bandwidth / Bandwidth per User) × Number of Cells
```

**For Cellular System:**
```
Users per Cell = Total Bandwidth / (Bandwidth per User × Cluster Size)
Total Users in City = Users per Cell × Total Number of Cells
```

**Example Calculation:**
- Bandwidth: 25 MHz = 25,000 kHz
- Per user: 30 kHz
- Single city-wide cell: 25,000 / 30 = 833 users

**With Cellular Concept:**
- Cluster size (N): 7
- Channels per cell: 833 / 7 ≈ 119 channels
- Total cells in city: 20
- Total users: 119 × 20 = 2,380 users

**Efficiency Improvement**: 2,380 / 833 = 2.86× more users supported

---

## 5. Channel Allocation Algorithms

### 5.1 Fixed Channel Allocation (FCA)

**What it is**: Each cell is permanently allocated a set of channels

**Characteristics:**
- Simple to implement
- No real-time channel management needed
- Channels cannot be borrowed by neighboring cells

**Advantages:**
- Simple, predictable
- No computational overhead
- No signaling overhead

**Disadvantages:**
- Inefficient spectrum utilization
- Cannot adapt to traffic variations
- Blocked calls even when channels free in other cells

### 5.2 Dynamic Channel Allocation (DCA)

**What it is**: Channels allocated to cells dynamically based on demand

**How it works:**
1. No fixed channel assignment to cells
2. When call arrives, system searches for available channel
3. Channel assigned temporarily for call duration
4. After call ends, channel returned to pool

**Advantages:**
- Better spectrum utilization
- Adapts to traffic patterns
- Reduced blocking probability
- Handles non-uniform traffic well

**Disadvantages:**
- Complex implementation
- Requires centralized or distributed control
- Higher computational overhead
- Increased signaling

### 5.3 Hybrid Channel Allocation

**What it is**: Combination of FCA and DCA

**How it works:**
- Some channels permanently assigned (FCA)
- Some channels kept in dynamic pool (DCA)
- Cells borrow from dynamic pool during high traffic

**Advantages:**
- Balances simplicity and efficiency
- Provides guaranteed capacity (fixed channels)
- Handles traffic peaks (dynamic channels)

| Allocation Type | Flexibility | Complexity | Efficiency | Best For |
|-----------------|-------------|------------|------------|----------|
| **FCA** | Low | Low | Low | Uniform traffic |
| **DCA** | High | High | High | Variable traffic |
| **Hybrid** | Medium | Medium | Medium | Mixed scenarios |

---

## 6. Handoff (Handover) Process

### 6.1 What is Handoff?

**Definition**: Process of transferring an ongoing call from one cell/channel to another as mobile moves

**Why needed:**
- Mobile moves between cells during call
- Signal strength decreases in current cell
- Maintain call continuity

### 6.2 Types of Handoff

#### Based on Initiation:

**1. Mobile-Initiated Handoff**

**Process:**
1. Mobile station continuously monitors signal strength from serving and neighboring base stations
2. Mobile measures received signal strength (RSS) from multiple base stations
3. When neighboring cell's signal becomes stronger than current cell (by threshold), mobile initiates handoff
4. Mobile reports measurements to network
5. Network evaluates and executes handoff if appropriate

**Characteristics:**
- Mobile has control over handoff decision
- Faster handoff initiation
- Mobile must have capability to measure multiple channels simultaneously
- Used in IS-95 (CDMA) systems

**Advantages:**
- Quick response to signal degradation
- Mobile has better knowledge of channel quality
- Reduced network overhead

**Disadvantages:**
- Requires sophisticated mobile hardware
- Mobile may not consider network load

**2. Network-Initiated Handoff**

**Process:**
1. Base stations continuously monitor signal strength from all mobiles in their cells
2. Base station measures uplink signal quality
3. When signal drops below threshold, base station reports to MSC
4. MSC requests measurements from neighboring base stations
5. MSC decides and executes handoff

**Characteristics:**
- Network has control over handoff decision
- Used in GSM systems
- Network can consider multiple factors (signal, interference, load)

**Advantages:**
- Network considers system-wide optimization
- Can balance load across cells
- Simpler mobile hardware

**Disadvantages:**
- Slower response time
- Higher signaling overhead
- Network may not detect degradation as quickly as mobile

#### Handoff Decision Parameters:

| Parameter | Mobile-Initiated | Network-Initiated |
|-----------|------------------|-------------------|
| **Control** | Mobile decides | Network decides |
| **Measurements** | Mobile measures | Base stations measure |
| **Response Time** | Faster | Slower |
| **Load Balancing** | Not considered | Can be optimized |
| **Mobile Complexity** | Higher | Lower |
| **Signaling** | Lower | Higher |
| **Used In** | CDMA (IS-95) | GSM, TDMA |

### 6.3 Handoff Types by Technology

**Hard Handoff (Break-before-make)**
- Connection to old cell broken before connection to new cell established
- Brief interruption in service
- Used in FDMA, TDMA systems

**Soft Handoff (Make-before-break)**
- Mobile connected to multiple base stations simultaneously
- No interruption in service
- Used in CDMA systems
- Improves reliability but uses more resources

[[Handoff process diagram showing mobile moving between cells with signal strength measurements]]

---

## 7. Key Performance Metrics

### 7.1 Capacity Metrics

| Metric | Formula | Meaning |
|--------|---------|---------|
| **Channels per Cell** | Total Channels / Cluster Size (N) | Frequency channels available per cell |
| **Users per Cell** | Total Bandwidth / (BW per User × N) | Maximum simultaneous users per cell |
| **System Capacity** | Users per Cell × Total Cells | Total users supported in network |

### 7.2 Coverage and Quality Metrics

| Metric | Description |
|--------|-------------|
| **Cell Radius (R)** | Distance from base station to cell edge |
| **Reuse Distance (D)** | D = R√(3N), minimum distance for frequency reuse |
| **Co-channel Reuse Ratio** | D/R = √(3N), larger ratio = less interference |
| **Signal-to-Interference Ratio (SIR)** | Measure of signal quality relative to interference |

---

## Summary: Efficiency of Cellular Concept

**Why Cellular Concept is Efficient:**

1. **Frequency Reuse**: Same frequencies used in non-adjacent cells multiplies capacity
2. **Scalability**: Easy to add cells to increase capacity
3. **Lower Power**: Smaller cells need less transmit power
4. **Flexible**: Can adapt to traffic patterns through cell splitting/sectoring
5. **Better Coverage**: Smaller cells provide more uniform signal strength

**Trade-off**: More infrastructure cost vs. much higher capacity and spectrum efficiency

**Quantitative Benefit**: With cluster size N=7 and proper planning, can support N times more users compared to single large cell covering same area.