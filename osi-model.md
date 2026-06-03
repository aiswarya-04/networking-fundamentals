# OSI Model (Open Systems Interconnection)

The OSI model is a conceptual model that helps us understand how data flows through a network.

It is not a strict rulebook. It simply divides networking into layers so that each layer has a specific responsibility. This reduces complexity because each layer can focus on its own job without worrying about the others.

---

## Layer Quick Reference Table

| Layer | Name | Data Unit (PDU) | Addressing | Device | Purpose |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **7** | Application | Data | — | Gateway / Firewall | User applications (HTTP, DNS, FTP, SMTP) |
| **6** | Presentation | Data | — | — | Data format and interpretation |
| **5** | Session | Data | — | — | Establishes and maintains sessions |
| **4** | Transport | **Segment** | Port Number | — | Service-to-service delivery |
| **3** | Network | **Packet** | IP Address | Router | End-to-end delivery |
| **2** | Data Link | **Frame** | MAC Address | Switch | Hop-to-hop delivery |
| **1** | Physical | **Bits** | — | Hub / Cable | Transmits bits |

---

## Layer Functions

### Layer 7 – Application Layer
This layer is where network applications operate. Protocols such as HTTP, FTP, SMTP, and DNS exist here. It defines what the application should do with the data.

### Layer 6 – Presentation Layer
This layer is responsible for how data is represented and interpreted. It ensures both sides understand the format of text, numbers, images, audio, and other data.

### Layer 5 – Session Layer
This layer establishes, maintains, and terminates communication sessions between applications. In modern networks, this functionality is often handled by applications themselves using mechanisms such as session cookies.

### Layer 4 – Transport Layer
This layer handles service-to-service delivery. It uses port numbers to make sure data reaches the correct application.

### Layer 3 – Network Layer
This layer handles end-to-end delivery between networks. It uses IP addresses. Routers operate at this layer.

### Layer 2 – Data Link Layer
This layer handles hop-to-hop delivery within the same network. It uses MAC addresses. Switches operate at this layer.

### Layer 1 – Physical Layer
This layer is responsible for transmitting raw bits over the medium. It deals with cables, electrical signals, WiFi signals, etc.

---

## Encapsulation

When data is sent, headers are added as it moves down the layers:

1. **Layer 4** → Port numbers are added → **Segment**
2. **Layer 3** → IP addresses are added → **Packet**
3. **Layer 2** → MAC addresses are added → **Frame**
4. **Layer 1** → Converted to bits and transmitted

**Data Flow Order:** Segment -> Packet -> Frame -> Bits

---

## Delivery Types & Addressing

* **Hop-to-hop delivery** → Uses MAC addresses (Layer 2)
* **End-to-end delivery** → Uses IP addresses (Layer 3)
* **Service-to-service delivery** → Uses Port numbers (Layer 4)

> **Important:** IP addresses usually stay the same from source to destination. MAC addresses change at every single hop because each router removes the old Layer 2 frame and creates a new one for the next network link.

---

## Modern Networking Note
Layers 5, 6, and 7 are often combined into a single **Application Layer** in the TCP/IP model because modern applications usually handle session management, data formatting, and application logic themselves.
