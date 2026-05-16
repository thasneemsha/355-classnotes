> #### *Thasneem Mohamed*
## Transport Layer :

The transport layer is responsible for delivering data from one application process to another application process on different devices. While the network layer ensures that data reaches the correct device using IP addresses, the transport layer ensures that the data is delivered to the correct application using port numbers.

This layer provides process-to-process communication and manages multiple simultaneous connections on a single device. It also handles segmentation, flow control, and error control depending on the protocol being used.

Two primary protocols operate at this layer:

* Transmission Control Protocol (TCP)
* User Datagram Protocol (UDP)

The unit of data at this layer is called:

* Segment (TCP)
* Datagram (UDP)

---

## Ports and Process Communication

A port is a logical endpoint that identifies a specific application or service running on a device. It allows multiple applications to use the network simultaneously without interfering with each other.

Ports are 16-bit numbers, giving a total of 65,536 possible ports.

### Port Categories

| Range       | Name             | Description                                   |
| ----------- | ---------------- | --------------------------------------------- |
| 0–1023      | Well-known ports | Reserved for standard services like HTTP, FTP |
| 1024–49151  | Registered ports | Used by user applications                     |
| 49152–65535 | Ephemeral ports  | Temporary ports assigned by OS                |

Well-known ports are typically used by system-level services and often require administrative privileges to access. Registered ports are more flexible and can be used by applications, but developers should avoid conflicts with commonly used software.

Ephemeral ports are automatically assigned by the operating system for temporary communication. These are commonly used by client-side applications when initiating a connection.

---

## Use of Source and Destination Ports

When a client communicates with a server:

* Destination port identifies the service on the server
* Source port identifies the client application

For example, when accessing a web server:

* Destination port = 80 (HTTP)
* Source port = dynamically assigned ephemeral port

This design allows multiple connections to exist simultaneously, even to the same server, because each connection is uniquely identified by a combination of IP addresses and port numbers.

---

## User Datagram Protocol (UDP)

UDP is a connectionless protocol that provides minimal services for sending data. It does not establish a connection before sending data and does not guarantee delivery, order, or duplication protection.

UDP is designed for applications where speed is more important than reliability.

### UDP Header Structure

| Field            | Description                   |
| ---------------- | ----------------------------- |
| Source Port      | Port of sending application   |
| Destination Port | Port of receiving application |
| Length           | Length of UDP header and data |
| Checksum         | Error detection field         |

The UDP header is small (8 bytes), making it efficient for fast transmission.

---

### Characteristics of UDP

* No connection setup or teardown
* No acknowledgments
* No retransmission of lost packets
* No flow or congestion control

Because of these characteristics, UDP has low latency and minimal overhead.

---

### Use Cases of UDP

UDP is commonly used in applications where occasional data loss is acceptable:

* Video streaming (minor visual artifacts tolerated)
* Voice over IP (brief distortions acceptable)
* Online gaming (real-time responsiveness prioritized)

Applications using UDP must implement their own error handling if needed.

---

## Transmission Control Protocol (TCP)

TCP is a connection-oriented protocol that provides reliable and ordered delivery of data between applications. It ensures that all data is received correctly and in the correct order.

TCP includes mechanisms for error detection, retransmission, and flow control.

---

## TCP Header Structure

The TCP header is more complex and typically 20 bytes in size (without options).

| Field                 | Description                |
| --------------------- | -------------------------- |
| Source Port           | Sending application port   |
| Destination Port      | Receiving application port |
| Sequence Number       | Position of data in stream |
| Acknowledgment Number | Next expected byte         |
| Data Offset           | Start of data              |
| Flags                 | Control signals            |
| Window Size           | Flow control               |
| Checksum              | Error detection            |

---

## TCP Flags

TCP uses flags to control connection behavior.

| Flag | Meaning                   |
| ---- | ------------------------- |
| SYN  | Initiate connection       |
| ACK  | Acknowledge received data |
| FIN  | Terminate connection      |
| RST  | Reset connection          |
| PSH  | Push data immediately     |

These flags are essential for establishing, maintaining, and closing connections.

---

## TCP Connection Establishment

TCP uses a three-step process known as the three-way handshake to establish a connection.

### Three-Way Handshake

1. Client sends SYN with initial sequence number
2. Server responds with SYN + ACK
3. Client responds with ACK

This process ensures both sides are ready to communicate and have synchronized sequence numbers.

---

## Data Transmission in TCP

After the connection is established, data is transmitted in segments.

Each segment contains:

* Sequence number indicating the position of data
* Payload data

The receiver acknowledges received data using acknowledgment numbers.

Acknowledgment number = last received byte + 1

---

## Error Control and Retransmission

TCP ensures reliability through acknowledgments and retransmission.

* If acknowledgment is not received within a timeout period, data is retransmitted
* Duplicate acknowledgments indicate possible packet loss
* Sender retransmits missing segments based on acknowledgment numbers

---

## Flow Control

Flow control ensures that the sender does not overwhelm the receiver.

This is achieved using the window size field:

* Specifies how much data the receiver can handle at once
* Sender limits transmission based on this value

---

## Congestion Control

Congestion control prevents network overload.

TCP dynamically adjusts transmission rate based on network conditions. If packet loss or delays are detected, the sender reduces the rate of transmission.

---

## TCP Connection Termination

TCP uses a four-step process to close a connection.

### Four-Way Handshake

1. One side sends FIN
2. Other side sends ACK
3. Other side sends FIN
4. First side sends ACK

This ensures both sides have completed data transmission before closing.

---

## Full Duplex Communication

TCP supports full duplex communication, meaning both sides can send and receive data simultaneously.

Each direction maintains its own:

* Sequence numbers
* Acknowledgment numbers

---

## Network Address Translation (NAT)

NAT allows multiple devices on a private network to share a single public IP address.

Private IP addresses are typically in ranges such as:

* 192.168.x.x

The router modifies outgoing packets by replacing:

* Source IP with public IP
* Source port with a unique port

---

## NAT Table

The router maintains a NAT table to keep track of connections.

| Private IP | Private Port | Public IP | Public Port |
| ---------- | ------------ | --------- | ----------- |

When a response is received, the router uses this table to forward the data to the correct internal device.

---

## NAT Operation Example

1. Device A sends request to external server
2. Router replaces private IP with public IP
3. Router assigns a unique port
4. Mapping stored in NAT table
5. Response arrives at router
6. Router looks up mapping and forwards to correct device

This allows multiple devices to share one IP address without conflict.

---

## Port Forwarding

Port forwarding allows external devices to initiate connections to a device inside a private network.

The router is configured to forward incoming traffic on a specific port to a designated internal IP and port.

Example:

* External port 443 mapped to internal IP 192.168.0.5 port 443

This is required for hosting services such as web servers or game servers.

---

## Socket

A socket is an endpoint for communication that consists of:

* IP address
* Port number

A socket is created when an application requests the operating system to open a port for communication.

Sockets enable sending and receiving data over a network.

---

## TCP vs UDP Comparison

| Feature     | TCP                       | UDP               |
| ----------- | ------------------------- | ----------------- |
| Connection  | Connection-oriented       | Connectionless    |
| Reliability | Reliable                  | Unreliable        |
| Ordering    | Guaranteed                | Not guaranteed    |
| Speed       | Slower                    | Faster            |
| Overhead    | High                      | Low               |
| Use cases   | Web, email, file transfer | Streaming, gaming |

---

## Limitations of UDP

UDP does not provide:

* Guaranteed delivery
* Packet ordering
* Retransmission
* Congestion control

Applications must implement these features if required.

---

## QUIC Protocol

QUIC is a modern transport protocol built on top of UDP. It provides reliability, congestion control, and faster connection setup compared to TCP.

It is commonly used in HTTP/3 and modern web applications.

QUIC allows developers to combine the speed of UDP with reliability features similar to TCP, reducing latency and improving performance.

---

