# Unit 2: Bluetooth, ZigBee, and Wireless WANs/MANs

---

## BLUETOOTH

### Personal Area Network (PAN)

**What it is:**  
A network that connects personal devices (like phones, laptops, headphones) wirelessly within a small area.

**Why it is needed:**  
People need their devices to communicate with each other without wires and without needing Wi-Fi routers or internet infrastructure.

| Term | Meaning |
|------|---------|
| **PAN** | Personal Area Network – connects devices within ~10 meters |
| **Example** | Connecting your phone to wireless earbuds |

---

### Bluetooth Special Interest Group (SIG)

**What it is:**  
A group of companies that came together to create a standard for wireless personal device communication.

| Detail | Information |
|--------|-------------|
| **Formed** | May 1998 |
| **Founding Companies** | Ericsson, Intel, IBM, Nokia, Toshiba |
| **Goal** | Create a common standard so devices from different companies can work together |

---

### IEEE 802.15.1

**What it is:**  
An official IEEE standard based on Bluetooth technology.

| Standard | Purpose |
|----------|---------|
| **IEEE 802.15.1** | Official standard for Wireless Personal Area Networks (WPANs) |

---

### Bluetooth Definition

**What it is:**  
A wireless technology that lets devices exchange data over short distances using radio waves.

| Feature | Details |
|---------|---------|
| **Frequency Range** | 2.400 to 2.485 GHz (ISM band) |
| **Range** | Short distances (typically 10-100 meters) |
| **Wave Type** | UHF (Ultra High Frequency) radio waves |
| **Purpose** | Exchange data between fixed and mobile devices |

---

## BLUETOOTH Specifications

### Two Main Parts

| Part | Purpose |
|------|---------|
| **Core** | Provides basic data link and physical layer functions that all applications can use |
| **Profiles** | Defines how to use Bluetooth for specific applications (like headsets, file transfer, etc.) |

**Why divided this way:**  
- **Core** = Common foundation for all Bluetooth devices  
- **Profiles** = Ensures devices from different manufacturers work together for specific tasks

---

### Core Specification of Bluetooth 1.2

**Key Features:**

| Feature | What it Does |
|---------|--------------|
| **Adaptive Frequency-Hopping Spread Spectrum (AFH)** | Avoids radio interference by intelligently jumping between frequencies |
| **Higher Transmission Speeds** | Up to 721 kbit/s (faster data transfer) |
| **Extended Synchronous Connections (eSCO)** | Better audio quality for voice calls |
| **Flow Control and Retransmission Modes** | Ensures reliable data delivery through L2CAP protocol |

---

### Bluetooth Protocol Stack

**What it is:**  
A layered structure of protocols (rules) that handle different tasks for Bluetooth communication.

<img width="696" height="529" alt="image" src="https://github.com/user-attachments/assets/31eb513e-7eb4-4cae-8edb-5dda4d343adb" />

**What it does:**  
- Locates nearby devices  
- Connects to other devices  
- Exchanges data between devices

---

### Protocol Stack Logical Division

| Group | Contains | Purpose |
|-------|----------|---------|
| **Transport Protocol Group** | Radio layer, Baseband layer, Link manager layer, L2CAP, Host Controller Interface | Handles device location, connection setup, and link management |
| **Middleware Protocol Group** | RFCOMM, SDP, IrDA | Provides APIs and hides transport layer complexity from applications |
| **Application Group** | Modem dialer, Web browser, etc. | Applications that use Bluetooth links |

---

## Transport Protocol Group

### Purpose

**What it contains:**  
Protocols that help devices find each other and create/manage wireless connections.

---

### Radio (Physical) Layer

**What it does:**  
Handles the actual radio transmission – the hardware side of wireless communication.

| Aspect | Details |
|--------|---------|
| **Function** | Controls transceiver characteristics (frequency, interference, modulation) |
| **Frequency Band** | ISM (Industrial, Scientific, Medical) – 2.4 GHz |
| **Modulation** | GFSK (Gaussian Frequency Shift Keying) |
| **Speed** | • 64 Kbps for voice channels<br>• Up to 1 Mbps peak for data channels |
| **Link Range** | • Standard: 10 cm to 10 meters<br>• Extended: >100 meters (with higher transmit power) |

---

### Baseband Layer

**What it does:**  
Controls how devices share the wireless channel and connect to each other.

| Function | Description |
|----------|-------------|
| **Frequency Hop Selection** | Chooses which frequency to use next (prevents interference) |
| **Connection Creation** | Establishes links between devices |
| **Medium Access Control** | Manages who can transmit when |

---

### Piconet

**What it is:**  
A small ad hoc network created when Bluetooth devices connect together.

| Component | Details |
|-----------|---------|
| **Definition** | Ad hoc network linking devices using Bluetooth |
| **Master Device** | 1 device controls the network |
| **Active Slaves** | Up to 7 devices actively communicating |
| **Parked Slaves** | Up to 255 additional devices on standby |
| **Channel** | All devices in a piconet use the same physical channel |


<img width="227" height="202" alt="image" src="https://github.com/user-attachments/assets/dd312874-b34a-4e4c-a236-22de2e24c6c8" />

**How it works:**
1. One device becomes the **master** (controller)
2. Up to 7 other devices become **active slaves**
3. More devices can be **parked** (inactive but ready)
4. Master can activate parked devices by parking an active one first

| Address Type | Details |
|--------------|---------|
| **AM_ADDR** | Active Member Address – unique within the piconet (for active slaves) |

---

### Bluetooth Device Address

**What it is:**  
Every Bluetooth device has a unique 48-bit address (like a MAC address).

| Part | Size | Purpose |
|------|------|---------|
| **48-bit Address** | Total address | Unique identifier for each Bluetooth device |
| **LAP** (Lower Address Part) | Lower bits | Used for piconet identification, error checking, security checks<br>Assigned internally by each organization |
| **Other Two Parts** | Upper bits | Proprietary addresses of manufacturing organizations |

---

### Native Clock

**What it is:**  
Every Bluetooth device has an internal clock.

| Feature | Details |
|---------|---------|
| **Clock Size** | 28-bit |
| **Tick Rate** | 3,200 times per second |
| **Purpose** | Used along with device address to form and synchronize piconets |

**Two Fundamental Elements for Piconet:**
1. Device Address (48-bit)
2. Native Clock (28-bit)


---

## Transport Protocol Group (Contd.)

### Bluetooth States

**What it is:**  
Different modes a Bluetooth device can be in during its operation.

---

### Standby Mode

| Mode | Description |
|------|-------------|
| **Standby** | Initial state – all devices start here |
| **Power** | Low power consumption |
| **Activity** | Device is not connected to any piconet |

---

### Inquiry State

**What it is:**  
The process of discovering other Bluetooth devices nearby.

**Why it is needed:**  
A device needs to find out what other Bluetooth devices are around before it can connect.

| Sub-state | What Happens |
|-----------|--------------|
| **Inquiry** | Master sends inquiry packets on different frequencies to find devices |
| **Inquiry Scan** | Slave listens periodically for inquiry packets (when it wants to be discovered) |
| **Inquiry Response** | Slave responds with FHS (Frequency Hopping Sequence) packet containing its address |

**How it works (Simple Steps):**
1. Device A (master) wants to find nearby devices
2. Device A enters **inquiry state** and broadcasts inquiry packets
3. Device B (slave) is in **inquiry scan state** listening for inquiries
4. Device B hears the inquiry and sends back an **inquiry response** (FHS packet with its address)
5. Device A now knows Device B exists and has its address

[[State diagram of Bluetooth communication]]

---

## Transport Protocol Group (Contd.)

### Bluetooth States (Continued)

---

### Link Manager Protocol (LMP)

**What it is:**  
A protocol responsible for managing power and security in Bluetooth connections.

| Responsibility | Purpose |
|----------------|---------|
| **Power Management** | Controls power-saving modes |
| **Security Management** | Handles authentication and encryption |

---

### Connection State

**What it is:**  
When devices are actively connected in a piconet.

**Contains 3 Sub-states (Power Management Modes):**

| Mode | Power Level | Description |
|------|-------------|-------------|
| **Active Mode** | Normal | • Device actively participates in piconet<br>• Master polls slaves for transmissions<br>• Power optimization: slave can sleep if master tells when it will be addressed next |
| **Sniff Mode** | Low Power | • Slave reduces listening activity<br>• Master sends command with "sniff interval"<br>• Slave listens only at fixed intervals (not continuously) |
| **Hold Mode** | Very Low Power | • Slave temporarily stops participating in piconet<br>• Frees capacity for other functions (scanning, paging, inquiring)<br>• Can attend another piconet |
| **Park Mode** | Ultra Low Power | • Slave gives up its AM_ADDR (Active Member Address)<br>• Gets 8-bit Parked Member Address instead<br>• Stays synchronized but doesn't actively communicate<br>• Allows master to have more than 7 slaves total |

**Why different modes:**  
To save battery power when full communication isn't needed.

---

## Transport Protocol Group (Contd.)

### Link Manager Protocol

**What it is:**  
Protocol that resides in the data link layer (already covered above under LMP responsibilities).

---

### L2CAP (Logical Link Control and Adaptation Protocol)

**What it is:**  
A protocol that sits above the baseband layer and provides services to upper layer protocols.

**Why it is needed:**  
Different applications (voice calls, file transfer, web browsing) need different types of data services. L2CAP provides these services.

| Service Type | Description |
|--------------|-------------|
| **Connection-Oriented (SCO)** | Synchronous Connection-Oriented – for voice/audio |
| **Connectionless (ACL)** | Asynchronous Connectionless – for data transfer |

---

### L2CAP Functions

| Function | What it Does |
|----------|--------------|
| **Protocol Multiplexing** | Distinguishes between different upper layer protocols (SDP, RFCOMM, Telephony Control)<br>Gives each application its own channel |
| **Segmentation and Reassembly (SAR)** | • **Segmentation:** Breaks large L2CAP packets into smaller Baseband packets for transmission<br>• **Reassembly:** Combines received Baseband packets back into L2CAP packets<br>• Performs integrity check |

**How SAR works:**
1. Application sends large data packet to L2CAP
2. L2CAP breaks it into small pieces (segmentation)
3. Small pieces sent over Bluetooth radio
4. Receiving device's L2CAP collects pieces
5. Pieces combined back into original packet (reassembly)
6. Integrity check ensures nothing was corrupted

---

### Host Controller Interface (HCI)

**What it is:**  
A standardized interface between the Bluetooth hardware (controller) and the software (host stack).

**Why it is needed:**  
Different devices (PCs, phones, tablets) need a standard way to talk to Bluetooth chips.

| Component | Example |
|-----------|---------|
| **Host Stack** | PC operating system or mobile phone OS |
| **Controller** | Bluetooth IC (integrated circuit/chip) |

---

### HCI Transport Standards

| Transport | Used In |
|-----------|---------|
| **USB** | PCs and laptops |
| **UART** | Mobile phones and PDAs |

---

### HCI Packet Types

| Packet Type | Direction | Purpose |
|-------------|-----------|---------|
| **Command Packets** | Host → Controller | Host controls the Bluetooth device |
| **Event Packets** | Controller → Host | Device informs host of changes/events |
| **Data Packets** | Both directions | Actual data transfer |

---

## Middleware Protocol Group

### Purpose

**What it is:**  
A layer between transport protocols and applications that makes Bluetooth easier to use.

**Why it is needed:**  
Applications shouldn't have to deal with complex transport layer details. Middleware provides simple APIs (functions) for common tasks.

| Component | Purpose |
|-----------|---------|
| **RFCOMM** | Emulates serial port communication |
| **SDP** | Discovers what services other devices offer |
| **TCS** | Controls telephone calls |
| **IrDA** | Allows infrared apps to work on Bluetooth |

---

### RFCOMM (Radio Frequency Communication)

**What it is:**  
A protocol that makes Bluetooth act like a serial cable connection.

| Feature | Details |
|---------|---------|
| **Purpose** | Emulates RS232 serial ports over L2CAP |
| **Why Needed** | Many applications were designed for serial ports (COM ports)<br>RFCOMM lets them work over Bluetooth without code changes |
| **Simultaneous Connections** | Up to 60 connections between two Bluetooth devices |

**Example:**  
Old software that sends data through COM1 can now send it over Bluetooth using RFCOMM.

---

### SDP (Service Discovery Protocol)

**What it is:**  
A protocol that helps devices find out what services other Bluetooth devices offer.

**Why it is needed:**  
Bluetooth devices are mobile – they come and go. A device needs to discover what services are available (printing, file transfer, audio, etc.) from nearby devices.

| Feature | Purpose |
|---------|---------|
| **Discovery** | Finds available services on other devices |
| **Dynamic** | Works with devices that move around |

**How it works:**
1. Your phone wants to print
2. Uses SDP to ask nearby devices "Who can print?"
3. Bluetooth printer responds "I can print!"
4. Connection established

---

## Middleware Protocol Group (Contd.)

### TCS (Telephony Control Protocol)

**What it is:**  
Protocol that controls phone calls over Bluetooth.

| Function | What it Does |
|----------|--------------|
| **Call Control** | Sets up calls for voice and data<br>Supports Point-to-Point (P-P) and Point-to-Multipoint (P-M) calls |
| **Group Management** | • Multiple telephone extensions<br>• Call forwarding<br>• Group calls (conference calls) |
| **Mobility Management** | Coordinates groups of TCS devices (multiple handsets) |

**Example:**  
Managing calls between a Bluetooth headset, phone, and cordless phone base station.

---

### IrDA Interoperability Protocol

**What it is:**  
Allows old infrared (IrDA) applications to work on Bluetooth devices.

**Why it is needed:**  
Many older devices used infrared for communication. This protocol lets those applications work on Bluetooth without rewriting them.

| Protocol | Purpose |
|----------|---------|
| **IrOBEX** | IrDA Object Exchange – for exchanging files/objects between devices |
| **IrMC** | Infrared Mobile Communications – for synchronization (contacts, calendar) |

---

## Bluetooth Profiles

### What Profiles Are

**What it is:**  
A profile is a specification that defines exactly how to use Bluetooth for a specific purpose.

**Why profiles are needed:**  
Two devices can only work together if they both support the same profile. Without profiles, a headset from Company A might not work with a phone from Company B.

**Example:**  
Both your phone and wireless headset must support the "Headset Profile" for them to work together.

---

### Four Groups of Bluetooth Profiles

---

### 1. Generic Profiles

| Profile | Purpose |
|---------|---------|
| **Service Discovery Profile** | Enables users to access SDP<br>Finds out which services a specific device supports |

---

### 2. Telephony Profiles

| Profile | Purpose |
|---------|---------|
| **Cordless Telephony Profile** | Designed for three-in-one phones (base station, cordless handset, headset) |
| **Headset Profile** | Provides wireless connection to headset (earphones/microphones)<br>Used with computers or mobile phones |

---

### 3. Networking Profiles

| Profile | Purpose |
|---------|---------|
| **General Networking** | Enables devices to connect to LAN through access points<br>OR form small wireless LAN among themselves |
| **Dial-up Networking Profile** | Provides dial-up internet connections via Bluetooth-enabled mobile phones |
| **FAX Profile** | Enables computers to send and receive faxes via Bluetooth-enabled mobile phone |

---

### 4. Serial and Object Exchange Profiles

| Profile | Purpose |
|---------|---------|
| **Serial Port Profile** | Emulates serial ports (RS232 & USB)<br>Used for file transfer, synchronization, and other serial line applications |

---

## ZigBee Specifications

### ZigBee Alliance

**What it is:**  
A group of companies that created the ZigBee standard.

**What ZigBee is:**  
The most popular wireless mesh networking standard for connecting sensors, instruments, and control systems.

---

### ZigBee Definition

**What it is:**  
A specification for communication in a Wireless Personal Area Network (WPAN), based on IEEE 802.15.4.

| Feature | Details |
|---------|---------|
| **Standard** | IEEE 802.15.4-based specification |
| **Power Consumption** | Very low |
| **Transmission Distance** | 10–100 meters line-of-sight (depends on power output and environment) |
| **Data Rate** | 250 kbit/s |
| **Mesh Networking** | Data can hop through intermediate devices to reach distant ones |

---

### ZigBee Frequency Bands

| Frequency Band | Region |
|----------------|--------|
| **2.4 to 2.4835 GHz** | Worldwide (unlicensed) |
| **902 to 928 MHz** | Americas and Australia |
| **868 to 868.6 MHz** | Europe |

*Note: Bands are country-specific*

---

### ZigBee Applications

| Application Area | Examples |
|------------------|----------|
| **Home and Office Automation** | Smart lights, thermostats, door locks |
| **Industrial Automation** | Factory sensors, equipment monitoring |
| **Medical Monitoring** | Patient vitals, health sensors |
| **Low-Power Sensors** | Temperature, motion, humidity sensors |

---

### ZigBee Target Domain

**What ZigBee is designed for:**

| Characteristic | Description |
|----------------|-------------|
| **Low Power** | Battery-powered devices that last years |
| **Low Duty Cycle** | Devices that transmit occasionally (not constantly) |
| **Low Data Rate** | Don't need to send large amounts of data quickly |

**Example:**  
A temperature sensor that wakes up once every 10 minutes, sends temperature reading, then goes back to sleep.

---

## WIRELESS WANS AND MANS

---

## THE CELLULAR CONCEPT

### What is the Cellular Concept?

**What it is:**  
A method for efficiently using limited radio spectrum by dividing a large area into small cells.

**Why it is needed:**  
Radio spectrum is limited. If one large tower serves an entire city, only a few users can be supported. By dividing the city into cells, the same frequencies can be reused many times.

| Feature | Description |
|---------|-------------|
| **Network Division** | Large area divided into hexagonal cells |
| **Cell Shape** | Hexagon (covers region without overlapping or gaps) |
| **Efficiency** | Allows frequency reuse across the network |

[[Diagram showing hexagonal cell layout]]

---

### Cellular Radio System Components

| Component | Description |
|-----------|-------------|
| **Base Station (BS)** | Located at center of each cell<br>Communicates with mobile terminals |
| **Uplink Channels** | Mobile Terminals → Base Station |
| **Downlink Channels** | Base Station → Mobile Terminals |
| **Mobile Terminals (MT)** | Phones, tablets, mobile devices |

---

### Frequency Reuse

**What it is:**  
Using the same frequency in different locations that are far enough apart that they don't interfere with each other.

**Why it is important:**  
This is the key to cellular efficiency – frequencies can be used multiple times across a city instead of just once.

---

### Cluster

**What it is:**  
A group of cells that together use the entire available radio spectrum.

| Term | Definition |
|------|------------|
| **Cluster** | Group of cells using entire spectrum |
| **Cluster Size (N)** | Number of cells in each cluster |
| **Frequency Reuse Rule** | No two cells within the same cluster use the same frequency |
| **Reuse Distance (D)** | Minimum distance between cells using same frequency |
| **Formula** | N = i² + j² + ij |

**How it works:**
1. Available spectrum is divided among N cells in a cluster
2. Each cell gets 1/N of the total spectrum
3. After one cluster, the pattern repeats
4. Same frequencies reused in next cluster (far enough away)

[[Diagram showing cluster pattern with frequency reuse]]

---

## THE CELLULAR CONCEPT (Contd.)

### Issues in Cellular Systems

---

### 1. Co-channel Interference

**What it is:**  
Interference from cells in different clusters using the same frequency.

| Aspect | Details |
|--------|---------|
| **Cause** | Using same frequencies in different clusters |
| **Solution** | Reuse distance D must be large enough to keep interference low |
| **Requirement** | Distance between cells with same frequency must not let interference affect signal strength |

---

### 2. Adjacent Channel Interference

**What it is:**  
Interference from nearby frequencies within the same cluster.

| Aspect | Details |
|--------|---------|
| **Cause** | Using adjacent frequencies (like 880 MHz and 881 MHz) in nearby cells |
| **Solution** | Channel assignment within cluster must minimize assigning neighboring frequencies to same cell |

---

### Advantage of Cellular Concept

**Why cellular is better than single large cell:**

**Example Calculation:**

| Scenario | Users Supported | Calculation |
|----------|----------------|-------------|
| **Single Large Cell** | 833 users | • Spectrum: 25 MHz = 25,000 KHz<br>• Per user: 30 KHz<br>• Users: 25,000 ÷ 30 = 833 |
| **Cellular System (7-cell clusters)** | 2,380 users | • Per cell spectrum: 25,000 ÷ 7 KHz<br>• Users per cell: (25,000 ÷ 7) ÷ 30 = 119<br>• Total cells: 20<br>• Total users: 119 × 20 = 2,380 |

**Result:** Cellular system supports **~3 times more users** than single cell!

---

### General Formula

| Variable | Meaning |
|----------|---------|
| **S** | Total spectrum bandwidth |
| **W** | Bandwidth per user |
| **N** | Cluster size |
| **k** | Number of cells to cover area |
| **n** | Number of users supported |

**Formula:**  
n = (S ÷ N ÷ W) × k

---

## CELLULAR Capacity Enhancement

### Why Capacity Enhancement is Needed

**Reasons for reduced capacity:**
- Limited frequency reuse due to strict clustering
- Growing number of users
- Increased data usage

---

### Methods to Improve Capacity

---

### 1. Cell Splitting

**What it is:**  
Dividing a congested (overcrowded) cell into smaller cells.

| Aspect | Details |
|--------|---------|
| **Process** | Subdivide large cell into multiple smaller cells |
| **Each New Cell Has** | • Own base station<br>• Reduced antenna height<br>• Lower transmitter power |
| **Why it Works** | Increases capacity by increasing number of times channels are reused |

**How it works:**
1. Cell becomes overcrowded
2. Split into 4 smaller cells
3. Each small cell now reuses frequencies
4. Total capacity = 4× original

[[Diagram showing cell splitting process]]

---

### 2. Cell Layering (Micro-cells and Pico-cells)

**What it is:**  
Adding different sized cell layers on top of existing cells.

| Cell Type | Size | Purpose |
|-----------|------|---------|
| **Macro-cells** | Tens of kilometers | Original large cells for high-mobility users |
| **Micro-cells** | Less than 1 km radius | Smaller layer for low-mobility users |
| **Pico-cells** | Few meters radius | Very small cells for indoor areas |

**Why it works:**
- Fast-moving users (cars) use macro-cells
- Slow-moving users (walking) use micro-cells  
- Indoor users use pico-cells
- Each layer can reuse frequencies

---

## CELLULAR Capacity Enhancement (Contd.)

### 3. Sectorization

**What it is:**  
Dividing a cell into sectors (pie slices) using directional antennas instead of one omnidirectional antenna.

| Component | Traditional | With Sectorization |
|-----------|-------------|-------------------|
| **Antenna at BS** | 1 omni-directional (360°) | Multiple directional antennas |
| **Coverage** | Full circle | Divided into sectors |

---

### How Sectorization Works

**Concept:**  
Uses Space Division Multiple Access (SDMA) to reuse channels within shorter distances.

| Configuration | Description |
|---------------|-------------|
| **3-Sector** | Cell divided into three 120° sectors |
| **6-Sector** | Cell divided into six 60° sectors |

**Benefits:**
- Decreases co-channel interference
- Increases system performance
- Channels can be reused at shorter distances

**How it reduces interference:**
- Directional antennas only transmit/receive in their sector
- Signals from other directions are blocked
- Same frequency can be used closer together

[[Sectorization diagram showing 3-sector and 6-sector cells]]

---

## Channel Allocation Algorithms

### Purpose

**What it is:**  
Methods for assigning radio channels to cells to maximize the number of successful calls.

**Why it is important:**  
Efficient channel allocation greatly improves network throughput (number of calls supported).

---

### 1. Fixed Channel Allocation

**What it is:**  
Each cell is permanently assigned a fixed set of channels.

| Feature | Description |
|---------|-------------|
| **Channel Assignment** | Fixed set of channels per cell (determined in advance) |
| **Borrowing** | Cells may borrow channels from neighbors temporarily |
| **Constraint** | Borrowing must not violate minimum reuse distance |
| **Channel Locking** | Prevents identical frequencies from being too close |

---

### Disadvantage of Fixed Allocation

| Problem | Description |
|---------|-------------|
| **Unpredictable Demand** | Real-world call demand is very unpredictable |
| **Imbalance** | Some cells have too many channels (wasted)<br>Other cells have too few channels (calls blocked) |
| **Inefficiency** | Cannot adapt to changing traffic patterns |

---

### 2. Dynamic Channel Allocation

**What it is:**  
Channels are not pre-assigned to cells. Assignment happens on-demand when a call is requested.

| Feature | Description |
|---------|-------------|
| **No Local Channels** | Cells don't have permanently assigned channels |
| **Decision Making** | Channel allocation decided dynamically when needed |
| **Flexibility** | Extremely flexible, handles large demand variations |
| **Process** | Base stations check local interference and choose suitable channel |

---

### Advantage of Dynamic Allocation

| Benefit | Description |
|---------|-------------|
| **Adaptability** | Handles unpredictable, varying demand |
| **Efficiency** | Maximizes bandwidth utilization |

---

### Disadvantage of Dynamic Allocation

| Problem | Description |
|---------|-------------|
| **Computation** | Requires a lot of computation |
| **Complexity** | More complex to implement |

---

### 3. Hybrid Channel Allocation

**What it is:**  
A compromise between fixed and dynamic allocation.

| Feature | Description |
|---------|-------------|
| **Two Sets of Channels** | Each cell gets two types of channels |
| **Local Channels** | Fixed set assigned permanently (like fixed allocation) |
| **Borrowable Channels** | Pool of channels that can be borrowed (like dynamic allocation) |
| **Borrowing Rule** | Borrowing allowed only from the borrowable set |

---

### Comparison of Allocation Schemes

| Scheme | Complexity | Efficiency | Flexibility |
|--------|------------|------------|-------------|
| **Fixed** | Low | Low | Low |
| **Dynamic** | High | High | High |
| **Hybrid** | Medium | Medium | Medium |

---

## Handoffs

### What is a Handoff?

**What it is:**  
The process of transferring an ongoing call from one base station to another as the user moves.

**Why it is needed:**  
When you move from the coverage area of one base station to another, the call must be transferred without dropping.

---

### Two Phases of Handoff

| Phase | Description |
|-------|-------------|
| **1. Finding New Channel** | Locate a new uplink-downlink channel pair in the next cell |
| **2. Dropping Old Link** | Release the channel from the previous base station |

---

## Handoffs (Issues in Handoffs)

### 1. Optimal BS Selection

**Problem:**  
At cell boundaries, it's difficult to decide which base station should serve the mobile terminal.

**Why it's difficult:**
- Signal strength from multiple BSs may be similar
- Signals fluctuate
- Wrong choice can cause poor quality or dropped calls

---

### 2. The Ping-Pong Effect

**What it is:**  
Frequent back-and-forth handoffs between two base stations.

**Causes:**

| Cause | Description |
|-------|-------------|
| **Frequent Movement** | User moves back and forth across cell boundary |
| **Signal Fluctuation** | Signal strength fluctuates at boundary |

**Problem:**  
Wastes resources, degrades service quality

---

### 3. Detection of Handoff Requirement

**Three Methods:**

| Method | How It Works | Who Decides |
|--------|--------------|-------------|
| **Mobile-Initiated** | • MT monitors signal strength from BS<br>• Requests handoff when signal drops below threshold | Mobile Terminal |
| **Network-Initiated** | • BS monitors signals from MT<br>• BS checks with neighboring BSs about signal strength<br>• BS decides which cell should take the MT | Base Station |
| **Mobile-Assisted** | • MT monitors and reports signal strength<br>• BS makes final handoff decision | Combination |

---

## Handoffs (Issues in Handoffs) - Contd.

### 4. Handoff Delay

**What it is:**  
The time taken to transfer a call from current cell to new cell.

| Problem | Consequence |
|---------|-------------|
| **Too Large Delay** | Signal may fall below minimum C/I (Carrier to Interference ratio) |
| **Result** | Call gets dropped |
| **Goal** | Minimize handoff delay |

*C/I = Carrier to Interference ratio – measure of signal quality*

---

### 5. Duration of Interruption

**What it is:**  
The gap in communication when switching from old channel to new channel.

**Process:**
1. Old channel pair from current BS is canceled
2. Brief interruption
3. New channel pair from next BS is activated

| Impact | Description |
|--------|-------------|
| **Voice Calls** | Brief silence (noticeable but tolerable) |
| **Data Transmission** | Can cause packet loss, retransmission needed |
| **Goal** | Minimize interruption duration |

---

### 6. Handoff Success

**What it is:**  
The probability that a handoff completes successfully when a user crosses a cell boundary.

**Depends on:**

| Factor | Description |
|--------|-------------|
| **Available Channels** | Number of free channel pairs in adjacent cells |
| **Switching Capacity** | Ability to switch before signal falls below acceptable
