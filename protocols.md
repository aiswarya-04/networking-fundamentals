# Network Protocols, Transport Semantics & Critical Ports

This module covers the core fundamentals of network protocols, essential host configurations, common misconceptions regarding Layer 4 transport protocols (TCP vs. UDP), the TCP three-way handshake, basic network attack vectors, and essential well-known service ports.

---

# Section 1: Introduction to Network Protocols

A **protocol** is a standardized set of rules and formats that devices follow to communicate across a network.

For example, when using the **Address Resolution Protocol (ARP)** to resolve IP addresses to physical MAC addresses, protocols specify:

- **Message Structure:** What fields and data must be included in the frame.
- **Formatting Rules:** How data is ordered, encoded, and segmented.
- **Addressing Standards:** What destination addresses (e.g., Unicast vs. Broadcast) should be targeted.
- **Interaction Flow:** How devices should handle and respond to specific requests.

Because protocols enforce global standards, systems running completely different operating systems and hardware (e.g., an iOS smartphone and a Linux server) can seamlessly communicate.

---

# Section 2: Domain Name System (DNS) & Host Essentials

## Domain Name System (DNS)

Human users rely on memorable domain names like `google.com`, but network devices route traffic using numerical IP addresses.

```text
google.com
      │
      ▼
 DNS Resolution
      │
      ▼
142.250.190.46
```

DNS functions as the directory service of the Internet, resolving human-readable names into machine-routable IP addresses behind the scenes.

---

## The 4 Requirements for Network Connectivity

For any host to successfully connect to a network and communicate with external networks, it requires four core network parameters.

| Parameter | Function |
|-----------|----------|
| **IP Address** | The unique logical identity of the device on the network. |
| **Subnet Mask** | Identifies the boundary and size of the local IP network. |
| **Default Gateway** | The IP address of the local router used to forward traffic outside the local network. |
| **DNS Server IP** | The server queried to resolve domain names into IP addresses. |

> **Dynamic Host Configuration Protocol (DHCP):** Instead of manually configuring every device, DHCP automatically assigns all four network parameters when a client joins the network.

---

# Section 3: TCP vs. UDP — Clarifying Common Myths

Transport Layer (Layer 4) protocols handle communication between end hosts. Many common statements about TCP and UDP are oversimplified.

---

## Myth 1: "UDP is faster"

### Reality
UDP does **not** transmit data across the network faster than TCP.

### Better Statement
> **UDP introduces less protocol overhead and lower latency.**

### Explanation

UDP avoids:

- Connection establishment
- Handshakes
- Acknowledgements
- Packet ordering
- Flow control

This reduces processing time and latency.

---

## Myth 2: "TCP is more secure"

### Reality

TCP provides reliability—not security.

### Better Statement

> **TCP provides reliable delivery. Security is added by protocols like TLS.**

### Examples

- `HTTP` → TCP → Plaintext
- `HTTPS` → TCP + TLS → Encrypted

---

## Myth 3: "UDP is unreliable"

### Reality

UDP does not provide reliability **at the transport layer**, but applications can implement their own reliability mechanisms.

### Better Statement

> **UDP lacks built-in transport-layer reliability.**

### Examples

- **QUIC (HTTP/3)** runs over UDP while implementing packet recovery and congestion control.
- Voice calls, video streaming, and online games often prefer UDP because low latency is more important than retransmitting every lost packet.

---

## Myth 4: "TCP guarantees delivery"

### Reality

TCP repeatedly attempts retransmission but cannot overcome complete network or hardware failures.

### Better Statement

> **TCP provides reliable transport semantics—not guaranteed delivery.**

If the destination crashes or a cable is disconnected, TCP eventually times out and terminates the connection.

---

# Section 4: TCP Three-Way Handshake

Before TCP transfers application data, it establishes a reliable full-duplex connection.

```text
 Client                              Server

   |                                   |
   |----------- SYN ------------------>|
   |                                   |
   |<-------- SYN-ACK -----------------|
   |                                   |
   |----------- ACK ------------------>|
   |                                   |

      ===== Data Transfer Begins =====
```

## Simple Conversation

- **Client (SYN):** "Can we talk?"
- **Server (SYN-ACK):** "Yes, I hear you. Can you hear me?"
- **Client (ACK):** "Yes! Let's begin."

---

# Section 5: Network Security Concepts & Attacks

## IP Spoofing

IP Spoofing is the technique of forging the **source IP address** in an IP packet.

### Common Uses

1. Hiding the attacker's real identity.
2. Bypassing simple IP-based filters.
3. Redirecting responses toward another victim (reflection attacks).

---

## TCP SYN Flood Attack

A **SYN Flood** is a Denial-of-Service (DoS) attack that abuses the TCP three-way handshake.

```text
Attacker
    │
    ├── SYN (Spoofed IP)
    ▼
Target Server
    │
    ├── SYN-ACK
    ▼
Fake IP Address
    │
    └── No ACK Returned

Server keeps waiting...
Half-open connections accumulate.
Connection queue fills.
Legitimate users cannot connect.
```

### Attack Process

1. The attacker sends thousands of SYN packets using spoofed IP addresses.
2. The server replies with SYN-ACK packets.
3. No ACK is returned.
4. The server stores each half-open connection in memory.
5. Eventually, the backlog queue fills and legitimate users are denied service.

---

# Section 6: Well-Known Critical Ports

An **IP address** identifies the destination device.

A **port number** identifies the specific service running on that device.

| Port | Protocol | Service | Description | Encryption |
|------:|:--------:|---------|-------------|------------|
| **21** | TCP | FTP | File Transfer Protocol | Plaintext |
| **22** | TCP | SSH | Secure Shell | Encrypted |
| **23** | TCP | Telnet | Remote terminal access | Plaintext |
| **53** | UDP / TCP | DNS | Domain Name System | Plaintext |
| **80** | TCP | HTTP | Web traffic | Plaintext |
| **443** | TCP | HTTPS | Secure web traffic (TLS) | Encrypted |
| **3389** | TCP | RDP | Remote Desktop Protocol | Encrypted |

---

# Key Takeaways

- Protocols define the rules for communication.
- DNS converts domain names into IP addresses.
- DHCP automatically assigns network settings.
- TCP emphasizes reliability.
- UDP emphasizes low overhead and low latency.
- TCP establishes connections using the three-way handshake.
- IP spoofing hides the attacker's source address.
- SYN Flood attacks exhaust server connection resources.
- Port numbers identify services running on a host.
