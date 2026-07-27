# How Hosts Communicate Across Networks

This module details how data frames and packets travel between hosts, covering same-network local switching, address resolution via ARP, and cross-network routing via Default Gateways.

---

## Scenario 1: Communication Within the Same Network

When **Host A** and **Host B** reside on the same IP subnet, communication happens directly via Layer 2 using MAC addresses.

### Process Breakdown

1. **Subnet Check:** Host A compares Host B’s IP address against its subnet mask and determines Host B is in the **same network**.
2. **Layer 3 Preparation:** Host A creates the Layer 3 header with Host B's IP as the destination.
3. **ARP Request (Broadcast):** Host A needs Host B’s MAC address. It broadcasts an ARP Request (`FF:FF:FF:FF:FF:FF`) containing:
   * **Source:** Host A's IP & MAC
   * **Target:** Host B's IP
4. **ARP Reply (Unicast):** Every device receives the broadcast, but only **Host B** responds directly to Host A with its MAC address.
5. **Cache Update:** 
   * Host A updates ARP Cache: `Host B IP → Host B MAC`
   * Host B updates ARP Cache: `Host A IP → Host A MAC`
6. **Frame Delivery:** Host A constructs the final Layer 2 frame (`Destination MAC = Host B MAC`) and sends the data directly.

---

## Scenario 2: Communication Across Different Networks

When **Host B** resides on a different subnet, Host A cannot deliver the frame directly. It must forward the data through its **Default Gateway** (the local router).

### Process Breakdown

1. **Subnet Check:** Host A determines Host B’s IP address belongs to a **different network**.
2. **Identify Next Hop:** Host A recognizes it must send the packet to its configured **Default Gateway (Router)**.
3. **Gateway ARP Request:** If the router’s MAC address is not cached, Host A sends an ARP Request for the **Router’s IP address**.
4. **Gateway ARP Reply:** The router responds with its local interface MAC address.
5. **Frame Construction:**
   * **Layer 3 (IP Header):** Destination IP = **Host B’s IP** *(Final Target)*
   * **Layer 2 (Ethernet Frame):** Destination MAC = **Router’s MAC** *(Next Hop)*
6. **Router Forwarding (Hop-by-Hop):**
   * The router receives the frame and strips the Layer 2 header.
   * It inspects the Layer 3 destination IP (Host B).
   * It consults its **Routing Table** to find the next path.
   * It re-encapsulates the IP packet into a new Layer 2 frame with updated source/destination MAC addresses and forwards it toward Host B's network.

---

## Key Takeaways

* **IP vs MAC Target:** When sending data outside the local subnet, the **Destination IP stays Host B**, but the **Destination MAC is the Router**.
* **ARP Cache:** ARP resolution only occurs when a mapping is missing from the local cache. Once learned, entries are reused until they expire.
