***Thasneem Mohamed***

# Data Link Layer

The **Data Link Layer** is responsible for **node-to-node communication**, 
meaning it transfers data between **directly connected devices** (like computers, switches, or routers). 
* Instead of sending data across the whole internet, it only handles **one hop at a time**.
* The Higher layer also hides the complexity of the physical hardware, so upper layers don’t need to worry about whether data is sent via copper, fiber, or wireless.

The data link layer is divided into two parts: 
* The **Logical Link Control (LLC)** and
* The **Media Access Control (MAC)** sublayers.
* These two work together to prepare, send, and receive data between devices.

---

### Media Access Control (MAC)

The **MAC sublayer** directly interacts with the physical medium (cables or wireless). It is responsible for 
* **converting signals into bits when receiving**, and
* **bits into signals when transmitting**.

When data is received, the MAC converts electrical/light/radio signals into bits and organizes them into **frames**. When sending, it takes frames and converts them back into signals for transmission.


### Logical Link Control (LLC)

The **LLC sublayer** acts as a bridge between the **network layer and the MAC layer**. It receives data (usually IP packets), decides which network interface (like Wi-Fi or Ethernet) to use, and prepares the data for transmission.

It may also perform **multiplexing**, which allows multiple protocols to share the same network. Finally, it adds a **header** to create a frame and sends it to the MAC layer. When receiving data, this process is reversed.

---

## Frames and Their Purpose

A **frame** is the basic unit of data at the data link layer. It contains both the actual data and additional **metadata** like sender and receiver information.

Frames are important because each hop in a network needs **local addressing**. Once a frame reaches the next device, its header is removed, and a **new frame is created** for the next hop. This keeps communication efficient and scalable, no matter how large the network is.

---

## Ethernet (IEEE 802.3)

**Ethernet** is the most common data link protocol for **wired networks** (both copper and fiber). It defines the structure of frames used for communication.

An Ethernet frame includes fields like:

### **Preamble(8 Bytes)** → synchronizes communication 

   The first 8 bytes of an ethernet frame are the preamble, which is a series of alternating 1s and 0s used to synchrnoize clocks (by detecting the transmission rate)

   ​	10101010 10101010 10101010 10101010 10101010 10101010 10101010 **10101011**
   
   * First 7 bytes are alternating 1s and 0s - used to synchronize clocks (by detect transmission rate)
   
   * The last byte of the preamble called the start frame delimiter (or SFD) has it's last bit changed to a 1,  to signals the start of next section

### **MAC Address**

A **MAC address** is a **unique 48-bit identifier** assigned to every network device. It is used for communication within a local network.

It has two parts:

* **OUI (first 24 bits)** → identifies the manufacturer
* **Device-specific part (last 24 bits)**

Devices like routers or computers can have **multiple MAC addresses** if they have multiple interfaces (e.g., Wi-Fi and Ethernet).

### Special MAC Addresses

There are special types of MAC addresses used for communication:

* **Broadcast** → sends data to all devices (`FF:FF:FF:FF:FF:FF`)
* **Multicast** → sends data to a group of devices

Broadcast is commonly used for network discovery, while multicast is used for efficient group communication.

### VLAN (IEEE 802.1Q)

**VLANs (Virtual LANs)** allow multiple **logical networks** to exist on the same physical hardware. This improves **security and organization**, especially in large networks.

For example, different departments in a company can be separated into different VLANs even if they use the same switches. VLAN tagging ensures data only reaches the intended group. VLANs can also support **Quality of Service (QoS)** to prioritize important traffic.

### EtherType (2 Bytes)

Used to indicate which protocol is encapsulated in the payload of the frame

An EtherType value of:

* 0x0800 signals that the frame contains an IPv4 datagram.
* 0x0806 indicates an Address Resolution Protocol frame, which is a protocol used to bridge the Data link and network layer (discussed later), 
* 0x86DD indicates an IPv6 frame  

### Payload (46-1500 Bytes)

This is the content that is being delivered, typically an IP packet.

### Frame Check Sequence (FCS)

The **FCS** is used for **error detection**. It uses a method called **CRC32** to generate a checksum before transmission.

When data is received, the checksum is recalculated and compared. If it doesn’t match, the frame is **discarded**. This ensures data integrity, but it does not fix errors—it only detects them.

---

## Switches

A **network switch** is a data link layer device that **forwards frames intelligently**. Unlike hubs, it sends data **only to the intended device**, not to everyone.

Switches maintain a **MAC address table (FIB)** that maps devices to physical ports. When a frame arrives:

* The switch learns the **source MAC**
* It looks up the **destination MAC**
* It forwards the frame to the correct port

If the destination is unknown, the switch uses **flooding** to find it.

Example of a MAC Address table inside a switch 

```
#show mac address-table
          Mac Address Table
-------------------------------------------
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    00ld.70ab.5d60    DYNAMIC     Fa0/2
   1    00le.f724.al60    DYNAMIC     Fa0/3
   
Total Mac Addresses for this criterion: 2
```
---

## Wi-Fi (IEEE 802.11)

Wi-Fi is the data link protocol for **wireless communication**. It operates in **half duplex**, meaning devices take turns transmitting.

Because wireless communication can be unreliable, Wi-Fi includes extra mechanisms like:

* **Acknowledgments (ACKs)** to confirm delivery
* **Retransmission support** for lost data

   Each network has:

* **SSID** → network name
* **BSSID** → unique identifier (like MAC address)

### Wi-Fi Frames

802.11 or Wi-Fi operates in Half Duplex (Wifi messages are exchanged in Data + Ack pairs). Wi-Fi frames are more complex than Ethernet frames because of wireless challenges. They include:

* **Frame Control** → defines type (management, control, data)
* **Multiple MAC addresses** → sender, receiver, etc.
* **Payload** → data
* **FCS** → error detection

Wi-Fi also uses control signals like **RTS/CTS** to reduce collisions, especially in problems like the **hidden node problem**.

---



