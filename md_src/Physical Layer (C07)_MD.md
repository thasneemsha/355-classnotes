***Thasneem Mohamed***
# Physical Layer

The **physical layer** is responsible for the **bit-by-bit transmission of data** between devices. It focuses on the **hardware components**, transmission media, and the methods used to encode data into signals. At this layer, data is not yet organized into meaningful structures—it is simply a stream of bits being sent across a medium.

---

## Cables

Cables are typically **point-to-point connections**, meaning they directly connect two devices without additional hardware. In modern networking, there are two primary types of cables: 
* **twisted pair (copper cables)** and
*  **fiber optic cables**.
  
 Each type differs in how it transmits data and in its performance characteristics.


## Twisted Pair (Copper Cables)

Twisted pair cables use **electrical signals (voltage modulation)** to represent data. A bit value of `1` is usually indicated by the presence of an electrical charge, while `0` is represented by its absence, although more advanced encoding schemes may be used. 

A **Network Interface Card (NIC)** is responsible for converting digital data into electrical signals for transmission and converting received signals back into data.

These cables consist of **pairs of copper wires twisted together**. The twisting is important because electrical currents generate **magnetic fields**, which can interfere with nearby wires. This interference is known as **crosstalk**. By pairing wires with opposite polarities (positive and negative), their magnetic fields cancel each other out when placed close together. Twisting the wires ensures that they share the same average position, further reducing interference and improving signal quality.

Twisted pair cables are categorized (e.g., Cat-5, Cat-6) based on their **performance specifications**, such as speed and resistance to interference. Higher category cables typically have stricter standards, better shielding, and higher data transmission speeds.

## Fiber Optic Cables

Fiber optic cables transmit data using **light signals** instead of electricity. They are made of a **glass core** surrounded by **cladding**, which keeps the light contained within the core by reflecting it internally. This allows data to travel at extremely high speeds, close to the speed of light.

However, over long distances, signals can weaken due to **attenuation**, requiring the use of amplifiers. Fiber optics provide significantly higher bandwidth than copper cables and are widely used in the **internet backbone** and modern high-speed internet services.

---

# Channel Types

Communication channels define how data flows between devices. A **simplex channel** allows **one-way communication only**, such as in TV broadcasts or security cameras. In contrast, a **duplex channel** allows **two-way communication**.

Duplex communication can be further divided into:

* **Full duplex**, where both devices can transmit simultaneously
* **Half duplex**, where devices must take turns transmitting (e.g., walkie-talkies)

---

# Hubs 

A **hub**, also known as a repeater, is a basic networking device that allows multiple devices to connect. It works by receiving a signal and **broadcasting it to all connected devices**. This means every device receives every message, regardless of whether it is the intended recipient.

Hubs create a **collision domain**, where only one device can transmit at a time. If multiple devices send data simultaneously, their signals collide, resulting in corrupted data. Although cables may support full duplex communication, hubs cannot handle multiple incoming signals at once, leading to inefficiencies.


## Carrier Sense Multiple Access (CSMA)

To manage collisions, networks introduced **Carrier Sense Multiple Access (CSMA)**. In this method, a device checks if the channel is free before transmitting. However, early versions assumed all devices followed the protocol, which was not always true.

This led to improvements:

* **CSMA/CD (Collision Detection):** Devices stop transmitting immediately when a collision is detected
* **CSMA/CA (Collision Avoidance):** Devices wait for a **random time interval** before retrying, reducing repeated collisions and preventing livelock

---

# Wireless Communication

Wireless communication uses **radio frequency waves** to transmit data instead of physical cables. Common frequencies include **2.4 GHz and 5 GHz**. Wireless networks operate in **half duplex mode**, meaning devices take turns transmitting.

To manage multiple devices, wireless networks use control mechanisms and protocols like **CSMA/CA**, which help avoid collisions by coordinating access to the shared medium. Although newer technologies replace older hardware, many foundational protocols like CSMA continue to be adapted and reused.

