# IP Addressing, Subnetting & NAT

This module introduces the fundamentals of subnetting, CIDR notation, Network Address Translation (NAT), and private IP addressing. Understanding these concepts is essential for networking, cybersecurity, and system administration.

---

# What is Subnetting?

**Subnetting** is the process of dividing a larger IP network into smaller, more manageable subnetworks (subnets).

### Why do we subnet?

- Improve network performance
- Reduce broadcast traffic
- Organize devices logically
- Increase security by isolating networks
- Use IP addresses more efficiently

---

# Seven Important Subnet Attributes

When subnetting a network, you'll commonly determine these seven values:

| Attribute | Description |
|-----------|-------------|
| **Network ID** | The first address in the subnet. Identifies the subnet itself. |
| **First Host IP** | The first usable IP address after the Network ID. |
| **Last Host IP** | The last usable IP address before the Broadcast Address. |
| **Broadcast Address** | The final IP address in the subnet. Used to communicate with all devices in that subnet. |
| **Next Network ID** | The first address of the next subnet. |
| **Number of IP Addresses** | Total number of IP addresses contained within the subnet. |
| **CIDR / Subnet Mask** | Defines the size of the subnet and how many bits belong to the network portion. |

---

# CIDR (Classless Inter-Domain Routing)

CIDR notation tells us how many bits belong to the **network portion** of an IP address.

Example:

```
192.168.1.0/24
```

- `192.168.1.0` → Network Address
- `/24` → First 24 bits are the network portion
- Remaining 8 bits are available for hosts

---

# CIDR Cheat Sheet

| CIDR | Usable Hosts | Typical Use |
|------|-------------:|-------------|
| `/32` | 1 | Single device |
| `/24` | 254 | Typical office or home subnet |
| `/16` | 65,534 | Large organization |
| `/8` | 16,777,214 | Very large network |

### Remember

- **Higher CIDR number → Smaller network**
- **Lower CIDR number → Larger network**

---

# Examples

```
10.0.0.5/32
```

One specific device.

---

```
10.0.0.0/24
```

One subnet containing **254 usable hosts**.

---

```
10.0.0.0/16
```

A much larger network containing thousands of devices and many subnets.

---

# Binary Place Values

Subnetting is based entirely on **binary mathematics**.

```
2⁷   2⁶   2⁵   2⁴   2³   2²   2¹   2⁰

128  64  32  16   8   4   2   1
```

These values represent the bits in a single octet.

Understanding these binary positions is essential when calculating subnet masks and determining network boundaries.

---

# Network Address Translation (NAT)

IPv4 provides approximately **4.3 billion unique addresses**, which is not enough for every Internet-connected device.

To solve this problem, networks use **private IP addresses** internally and **Network Address Translation (NAT)** to communicate with the Internet.

## How NAT Works

```
Private Devices
      │
      ▼
Router (NAT)
      │
      ▼
Public Internet
```

Multiple devices inside your home or office network share a **single public IP address**.

The router translates:

- Private IP → Public IP (outgoing traffic)
- Public IP → Private IP (incoming replies)

This allows many devices to access the Internet while using only one public IPv4 address.

---

# Private IP Address Ranges

These address ranges are reserved for private networks and are **not routable on the public Internet**.

| Private Range | Description |
|---------------|-------------|
| **10.0.0.0/8** | First octet must be **10** |
| **172.16.0.0/12** | Covers **172.16.x.x → 172.31.x.x** |
| **192.168.0.0/16** | First two octets must be **192.168** |

---

## Why Private IP Addresses?

Private IP addresses allow devices to communicate inside a local network without consuming public IPv4 addresses.

When Internet access is needed, **NAT translates the private address into a public address**.

---

# Understanding 172.16.0.0/12

This subnet often confuses beginners.

Remember:

```
/12 = 8 bits from the first octet
    + 4 bits from the second octet
```

That means the private range includes:

```
172.16.x.x
172.17.x.x
...
172.31.x.x
```

It does **not** stop at `172.16.x.x`.

---

# Key Takeaway

> **CIDR boundaries do not always align with octets.**

Subnetting works with **bits**, not whole octets.

For example:

- `/8` ends exactly after the first octet.
- `/16` ends exactly after the second octet.
- `/24` ends exactly after the third octet.
- `/12` ends halfway through the second octet.

This is why understanding binary is essential for subnetting.

---

# Summary

- Subnetting divides one network into multiple smaller networks.
- Every subnet has a Network ID, Broadcast Address, and usable host range.
- CIDR notation defines how many bits belong to the network.
- Higher CIDR numbers create smaller networks.
- NAT allows many private devices to share one public IP.
- Private IPv4 ranges are:
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- Subnetting is based on **binary**, not decimal or octets.
