# Network Devices & Infrastructure

This module covers the core components, devices, and foundational concepts that enable data transmission across local and wide area networks.

---

## Key Concepts

* **Hosts:** Any device that sends or receives network traffic.
  * Examples: Clients (workstations, mobile devices) and Servers.
* **IP Address:** The unique logical identity assigned to each host on a network.
* **Network:** The underlying infrastructure that transports traffic between hosts.
  * A logical grouping of hosts that share similar connectivity requirements (e.g., Subnetworks / Subnets).

---

## Network Hardware & Devices

| Device | Layer / Type | Key Function |
| :--- | :--- | :--- |
| **Repeater** | Physical Layer | Regenerates signals to extend transmission distance and prevent degradation. |
| **Hub** | Physical Layer | A multi-port repeater; broadcasts incoming traffic to all connected ports. |
| **Bridge** | Data Link Layer | Connects and filters traffic between Hub-connected segments/hosts. |
| **Switch** | Data Link Layer | Facilitates communication **within** a local network via switching (using MAC addresses). |
| **Router** | Network Layer | Facilitates communication **between** different networks (using IP routing, gateways, and routing tables). |

---

## Summary Breakdown

* **Intra-network Communication (Same Network):** Managed by **Switches**.
* **Inter-network Communication (Different Networks):** Managed by **Routers**.
* **Gateways & Routing:** Routers use **Routing Tables** to determine the best path for forwarding packets to external destination networks.
