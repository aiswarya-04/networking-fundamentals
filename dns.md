# Domain Name System (DNS)

The **Domain Name System (DNS)** is often called the **Internet's phonebook**.

Humans remember **domain names**, while computers communicate using **IP addresses**. DNS translates human-readable domain names into IP addresses so devices can locate one another on a network.

### Example

```text
www.google.com
        │
        ▼
142.xxx.xxx.xxx
```

---

# Why Do We Need DNS?

Imagine having to remember an IP address like:

```text
142.250.183.46
```

instead of:

```text
www.google.com
```

Remembering names is much easier than remembering numbers.

DNS makes the Internet human-friendly by allowing us to use domain names instead of IP addresses.

---

# Default DNS Port

| Port | Protocol | Purpose |
|------|----------|---------|
| **53** | **UDP** | Most DNS queries |
| **53** | **TCP** | Large DNS responses and Zone Transfers |

> **Note:** Most DNS lookups use **UDP** because it is lightweight and fast. **TCP** is used when responses are too large for UDP or when performing DNS Zone Transfers.

---

# DNS Hierarchy

DNS is **not a single server**. Instead, it is a hierarchy of servers that work together to resolve domain names.

```text
Client
   │
   ▼
Recursive Resolver
   │
   ▼
Root Name Server
   │
   ▼
TLD Name Server
   │
   ▼
Authoritative Name Server
   │
   ▼
IP Address Returned
```

---

# Step-by-Step DNS Resolution

Suppose you type:

```text
www.google.com
```

The following sequence occurs:

### 1. Your Computer

Your computer first checks its **local DNS cache**.

- If the IP address is already cached, it immediately returns the result.
- Otherwise, it queries a **Recursive Resolver**.

---

### 2. Recursive Resolver

The Recursive Resolver is usually provided by:

- Your ISP
- Google DNS (`8.8.8.8`)
- Cloudflare DNS (`1.1.1.1`)

Its job is to find the correct IP address on your behalf.

---

### 3. Root Name Server

The Root Server replies:

> "I don't know Google's IP address, but I know which **Top-Level Domain (TLD)** server manages `.com`."

---

### 4. TLD (Top-Level Domain) Name Server

The `.com` TLD server replies:

> "I don't know Google's IP address, but Google's **Authoritative Name Server** does."

---

### 5. Authoritative Name Server

The Authoritative Name Server returns the requested DNS record.

```text
www.google.com
        │
        ▼
142.xxx.xxx.xxx
```

---

### 6. Recursive Resolver

The Recursive Resolver:

- Stores the answer in its cache.
- Returns the IP address to your computer.

---

### 7. Your Computer

Your computer also caches the result for future requests.

---

### 8. Browser Connects

Now that the browser knows Google's IP address, it can establish a connection with the web server.

---

# Simplified DNS Flow

```text
You
 │
 ▼
Recursive Resolver
 │
 ▼
Root Server
 │
 ▼
.com TLD Server
 │
 ▼
Google's Authoritative Name Server
 │
 ▼
Recursive Resolver
 │
 ▼
Your Computer
 │
 ▼
Google Website
```

> **Important:** The **Recursive Resolver** performs the searching. The Root, TLD, and Authoritative servers simply tell it where to look next.

---

# DNS Servers and Their Responsibilities

## Recursive Resolver

Responsible for finding the answer for the client.

**Responsibilities**

- Finds the requested IP address
- Caches DNS responses
- Returns answers to the client

Common providers:

- ISP
- Google DNS
- Cloudflare DNS

---

## Root Name Server

The Root Server knows which **TLD Name Server** manages each top-level domain.

Examples:

- `.com`
- `.org`
- `.net`
- `.in`

> It **does not** know the actual IP address of websites.

---

## TLD (Top-Level Domain) Name Server

The TLD server knows which **Authoritative Name Server** is responsible for a domain.

Example:

```text
google.com
```

It replies with the location of Google's Authoritative Name Server.

> It **does not** know Google's IP address.

---

## Authoritative Name Server

The Authoritative Name Server is the **final source of truth** for a domain.

It stores:

- IPv4 addresses
- IPv6 addresses
- DNS Records
- Mail Server information
- Subdomains

Examples:

```text
google.com
        │
        ▼
Google's Authoritative Name Servers
```

```text
openai.com
        │
        ▼
OpenAI's Authoritative Name Servers
```

---

# Common DNS Record Types

DNS records are stored on the **Authoritative Name Server**.

They describe how a domain should behave.

| Record | Purpose | Example |
|---------|---------|---------|
| **A** | Maps a hostname to an IPv4 address | `google.com → 142.xxx.xxx.xxx` |
| **AAAA** | Maps a hostname to an IPv6 address | IPv6 Address |
| **CNAME** | Creates an alias for another hostname | `www.google.com → google.com` |
| **MX** | Specifies the mail server for a domain | `gmail.com → mail.gmail.com` |
| **TXT** | Stores text information | SPF, DKIM, DMARC, Domain Verification |

During DNS resolution:

```text
Authoritative Name Server
        │
Returns DNS Record
        │
        ▼
Recursive Resolver
        │
        ▼
Client
```

---

# DNS Cache

DNS responses are temporarily stored to improve performance.

Caching occurs in:

- Your Computer
- Recursive Resolver

### Benefits

- Faster browsing
- Reduced DNS traffic
- Lower latency
- Less load on DNS servers

---

# DNS Before Web Communication

DNS resolution always happens **before** an HTTP or HTTPS connection can begin.

```text
Client
   │
   ▼
DNS Resolution
   │
   ▼
IP Address Obtained
   │
   ▼
HTTP / HTTPS Connection
```

> If DNS resolution fails, the browser cannot determine the server's IP address, so the connection cannot be established.

---

# Key Takeaways

- DNS stands for **Domain Name System**.
- DNS translates **domain names into IP addresses**.
- DNS primarily uses **Port 53**.
- **UDP** is used for most DNS queries.
- **TCP** is used for large responses and Zone Transfers.
- DNS is a **hierarchical system**, not a single server.
- The **Recursive Resolver** searches for the answer on behalf of the client.
- The **Root Server** points to the correct TLD server.
- The **TLD Server** points to the Authoritative Name Server.
- The **Authoritative Name Server** provides the final DNS records.
- DNS responses are cached to improve performance.
- DNS resolution occurs **before** any HTTP or HTTPS communication.
