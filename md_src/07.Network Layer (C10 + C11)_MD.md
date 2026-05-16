> ***Thasneem Mohamed***
## **Network Layer**

#### **Overview of Network Layer**

The **Network Layer** is responsible for communication between different networks. It allows devices on separate networks to send and receive data by forwarding packets from one router to another. This process is called **routing** and ensures that data travels from the source to the destination across multiple networks.

In this lecture, we assume that routers already know the best path to forward packets (like using an “oracle”). The actual algorithms that determine these paths (like Dijkstra or Bellman-Ford) are studied later. The key idea here is that data moves **hop by hop** through routers until it reaches its destination.

---

#### **Local Area Network (LAN)**

A **LAN (Local Area Network)** is a group of devices connected within a single physical network, such as a home or office network. Devices in a LAN can communicate easily with each other. However, communicating outside the LAN requires routers.

Routers connect multiple networks and act as gateways. If all devices and paths are known beforehand, communication can happen using only the data link layer. But on the Internet, we don’t control all networks, so the network layer is needed to route data across unknown paths.

---

#### **Internet Protocol (IPv4)**

The **Internet Protocol (IP)** is the main protocol used at the network layer. The most common version is **IPv4**. It is responsible for delivering packets between networks.

Before sending data, a device adds an **IP header** to the message. This header stays mostly unchanged across the journey, except for fields like TTL. Unlike data link headers, IP headers are not removed at every hop—they remain until the packet reaches its final destination.

---

#### **IP Addresses**

Every device on the Internet has a unique **IP address**, which identifies it. These addresses are usually assigned by Internet Service Providers (ISPs) and are often temporary unless you pay for a static IP.

IPv4 addresses are **32-bit numbers**, written in dotted decimal format like:

* 192.168.1.1
* 255.255.255.255

There are about **4.3 billion possible IPv4 addresses**, but due to the growth of the Internet, these have been exhausted (IPv4 exhaustion problem).

---

#### **Network vs Host Portion**

An IP address has two parts:

* **Network portion** → identifies the network
* **Host portion** → identifies a device within that network

If more bits are used for the host portion, more devices can exist in one network. But this reduces the number of total networks available.

---

#### **Classful Addressing (Old System)**

Originally, IP addresses were divided into fixed classes:

* **Class A** → Large networks (millions of devices)
* **Class B** → Medium networks
* **Class C** → Small networks (up to 254 devices)

This system was inefficient because organizations often got too many or too few addresses. It led to wasted IP space and scalability problems.

---

#### **CIDR (Modern Addressing)**

**CIDR (Classless Inter-Domain Routing)** replaced classful addressing. Instead of fixed divisions, CIDR allows flexible allocation of IP addresses using a prefix length.

Example:

* 64.64.0.0/20 → first 20 bits = network, rest = host

This allows networks to grow or shrink as needed, making IP allocation more efficient. CIDR helps reduce waste and supports better routing.

---

#### **Subnet Masks**

A **subnet mask** is another way to represent the network portion of an IP address.

Example:

* IP: 64.64.0.0
* Mask: 255.255.240.0

The mask helps extract the network portion using binary operations. However, CIDR notation is generally easier to understand and more commonly used today.

---

#### **IP Packet Structure**

An **IP packet (datagram)** contains:

* Header (control information)
* Payload (actual data)

Important fields include:

* **Source & Destination IP** → identifies sender and receiver
* **TTL (Time to Live)** → limits number of hops (prevents infinite loops)
* **Protocol** → identifies transport layer protocol (TCP/UDP)
* **Total Length** → size of packet

---

#### **Fragmentation**

If a packet is too large for a network’s **MTU (Maximum Transmission Unit)**, it is split into smaller fragments.

Each fragment contains:

* Same **Identification number**
* **Fragment offset** (position in original packet)
* **More Fragments flag**

Fragments are reassembled at the destination.

---

#### **How Routers Work**

Routers forward packets based on the destination IP address.

Steps:

1. Check if destination is in local network
2. If not, look up routing table
3. Forward packet to next hop
4. Decrease TTL
5. Fragment if necessary

Routers also use a **default gateway** when no specific route is found. At each hop, the data link header changes, but the IP header remains.

---

#### **Reserved IP Addresses**

Some IP ranges are reserved for special purposes:

* **127.0.0.0/8** → loopback (self communication)
* **192.168.x.x** → private networks
* **224.0.0.0/4** → multicast

These addresses are not used publicly on the Internet.

---

#### **Multicast**

**Multicast** allows one device to send data to multiple devices at once.

Uses include:

* Video streaming
* Video conferencing
* Network updates

Instead of sending multiple copies, one transmission is shared among many receivers, making it efficient.

---

#### **DHCP (Dynamic Host Configuration Protocol)**

DHCP automatically assigns IP addresses to devices on a network.

Steps:

1. **Discover** → device broadcasts request
2. **Offer** → server offers IP
3. **Request** → device accepts offer
4. **Acknowledge** → server confirms

This process allows devices to join networks easily without manual setup.

---

#### **Dynamic vs Static IP**

* **Dynamic IP** → temporary, assigned by DHCP, changes over time
* **Static IP** → manually assigned, does not change

Dynamic IPs are common for personal devices, while static IPs are used for servers or printers.

---

#### **ARP (Address Resolution Protocol)**

ARP maps an **IP address to a MAC address** within a local network.

If a device knows the IP but not the MAC:

1. It sends a broadcast ARP request
2. The correct device responds with its MAC address

Devices store this information in an **ARP table**, which updates over time.

---

**Exam question**

#### **Example 1 (/24 case)**

Are these in the same network?

* IP1: 192.168.1.45
* IP2: 192.168.1.200
* Subnet: /24


#### **Step 1: Convert to Binary**

```
192 = 11000000
168 = 10101000
1   = 00000001
```

Only first **24 bits matter**(so .45 dont needed), so we focus on first 3 octets:

IP1:

```
11000000.10101000.00000001
```

IP2:

```
11000000.10101000.00000001
```

#### **Step 2: Compare First 24 Bits** : 
They are **identical** Then they are frome the **Same network**

---

#### **Example 2 (Not Same Network)**

* IP1: 192.168.1.45
* IP2: 192.168.2.10
* Subnet: /24


**First 24 bits**: are not the same. Hence they are from different network

IP1:

```
11000000.10101000.00000001
```

IP2:

```
11000000.10101000.00000010
```
---
#### **Example 3**

Are these in same network?

* IP1: 172.16.5.1
* IP2: 172.31.10.2
* Subnet: /12

#### **Step 1: Convert First Two Octets**

```
172 = 10101100
16 = 00010000
```
```
172 = 10101100
31 = 00011111
```

#### **Step 2: COMPARE FIRST 12 BITS ONLY**

IP1:

```
10101100 0001
```

IP2:

```
10101100 0001
```

Since the First 12 bits match they are from **Same network**

---

>* Subnet (/N) = number of **network bits**
>* You ONLY compare those bits
>* Rest = host bits (ignore)

