# Web Fundamentals

This module covers the fundamentals of web communication, including HTTP, HTTP methods, HTTP status codes, HTTPS, encryption, and Public Key Infrastructure (PKI).

---

# Section 1: HTTP (Hypertext Transfer Protocol)

HTTP (Hypertext Transfer Protocol) is the application-layer protocol used for communication between a client (such as a web browser) and a web server.

It defines how requests and responses are formatted and exchanged over the web.

When you visit a website:

1. Your browser sends an **HTTP request** to the web server.
2. The server processes the request.
3. The server returns an **HTTP response**, which includes:
   - A status code (e.g., `200 OK`, `404 Not Found`)
   - Response headers
   - The requested content (such as HTML, CSS, JavaScript, images, or JSON data)

### HTTP Request–Response Model

```text
Client (Browser)
       │
       │ HTTP Request
       ▼
Web Server
       │
       │ HTTP Response
       ▼
Client (Browser)
```

### HTTP vs HTTPS

| HTTP | HTTPS |
|------|-------|
| Data is sent in plaintext | Data is encrypted using TLS |
| Default port: **80** | Default port: **443** |
| Less secure | Secure and encrypted |
| Vulnerable to interception | Protects confidentiality and integrity |

> **Note:** HTTPS is simply HTTP running over **TLS (Transport Layer Security)**, providing authentication, encryption, and data integrity.

---

# Section 2: HTTP Status Codes

HTTP Status Codes are **3-digit numbers** returned by a web server in response to an HTTP request.

They indicate whether the request was successful or if an error occurred.

---

## Status Code Categories

| Category | Meaning | Examples |
|----------|---------|----------|
| **1xx** | Informational | `100 Continue` |
| **2xx** | Success | `200 OK`, `201 Created` |
| **3xx** | Redirection | `301 Moved Permanently`, `302 Found` |
| **4xx** | Client Errors | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` |
| **5xx** | Server Errors | `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable` |

---

## Common HTTP Status Codes

| Status Code | Meaning |
|-------------|---------|
| **200 OK** | The request completed successfully. |
| **201 Created** | A new resource was created successfully. |
| **301 Moved Permanently** | The requested resource has permanently moved to another URL. |
| **302 Found** | The requested resource has temporarily moved. |
| **400 Bad Request** | The request is malformed or invalid. |
| **401 Unauthorized** | Authentication is required. |
| **403 Forbidden** | Authentication succeeded, but permission is denied. |
| **404 Not Found** | The requested resource does not exist. |
| **500 Internal Server Error** | A generic server-side error occurred. |
| **502 Bad Gateway** | A server received an invalid response from another server. |
| **503 Service Unavailable** | The server is temporarily unavailable. |
| **418 I'm a Teapot ☕** | A humorous status code introduced in RFC 2324. |

---

# Section 3: HTTP Methods

HTTP methods (also called **HTTP verbs**) specify the action a client wants the web server to perform.

---

## GET

**Purpose**

Retrieve (read) data from the server.

**Example**

```http
GET /index.html
```

---

## POST

**Purpose**

Send data to the server, usually to create a new resource.

**Common Uses**

- Logging in
- Creating an account
- Submitting a form

**Example**

```http
POST /login
```

---

## PUT

**Purpose**

Update or replace an existing resource.

**Example**

```http
PUT /profile
```

---

## DELETE

**Purpose**

Delete an existing resource.

**Example**

```http
DELETE /post/123
```

---

## HTTP Methods Summary

| Method | Purpose |
|---------|----------|
| **GET** | Retrieve (Read) data |
| **POST** | Create or Send data |
| **PUT** | Update or Replace data |
| **DELETE** | Delete data |

---

# Section 4: Basic Idea of Asymmetric Encryption

Imagine a **Client** and a **Server** that want to communicate securely.

## Communication Process

1. The Client sends a request to the Server.
2. The Server sends its **Public Key**.
3. The Client generates a random **Symmetric Session Key**.
4. The Client encrypts the session key using the Server's **Public Key**.
5. The encrypted session key is sent back to the Server.
6. The Server decrypts it using its **Private Key**.
7. Both devices now share the same symmetric session key.
8. All further communication uses **Symmetric Encryption**.

### Process Diagram

```text
Client                               Server
   │                                    │
   │────────── Request ────────────────>│
   │                                    │
   │<──────── Public Key ───────────────│
   │                                    │
Generate Session Key                    │
Encrypt with Public Key                 │
   │                                    │
   │──── Encrypted Session Key ────────>│
   │                                    │
                        Decrypt with Private Key
   │                                    │
   └──────── Secure Communication Begins ────────┘
```

---

## The Problem

How does the Client know that the Public Key actually belongs to the Server?

An attacker could intercept the connection and replace the Server's public key with their own.

The attacker could then:

- Decrypt the session key.
- Read the communication.
- Re-encrypt it using the Server's real public key.
- Forward the traffic to the Server.

Neither side would realize an attacker is sitting between them.

This is known as a **Man-in-the-Middle (MITM) Attack**.

---

# Why Does HTTPS Use Both Asymmetric and Symmetric Encryption?

HTTPS combines the strengths of both encryption methods.

## Asymmetric Encryption

- Solves the trust problem.
- Securely exchanges a shared secret.
- Much slower.

## Symmetric Encryption

- Encrypts all application data.
- Much faster.
- More efficient.

---

## Comparison

| Asymmetric Encryption | Symmetric Encryption |
|------------------------|----------------------|
| Proves identity | Encrypts data |
| Used during the handshake | Used after the handshake |
| Slower | Faster |

---

## Why Not Use Only Asymmetric Encryption?

Asymmetric encryption requires much more computation.

If every webpage, image, video, and file used asymmetric encryption:

- Connections would be much slower.
- CPUs would perform significantly more calculations.
- Servers would support fewer users.

Instead:

- **Asymmetric Encryption** establishes the secure connection.
- **Symmetric Encryption** protects the rest of the communication.

This provides:

- Strong Security
- High Performance

---

# Section 5: Public Key Infrastructure (PKI)

Without certificates, anyone could pretend to be a legitimate server.

PKI solves this trust problem.

---

## The Problem with Public Keys Alone

Without certificates:

1. Client requests a secure connection.
2. Server sends its Public Key.
3. Client encrypts a Session Key.
4. Server decrypts it.
5. Communication begins.

Although encrypted, there is no guarantee that the Public Key actually belongs to the intended server.

---

## Man-in-the-Middle (MITM) Attack

```text
Client
   │
   │
Attacker
   │
   │
Server
```

The attacker intercepts the connection and replaces the Server's Public Key with their own.

The attacker can:

- Read the traffic.
- Modify the traffic.
- Forward the traffic.

Both the Client and Server believe they are communicating directly.

---

# Certificate Authorities (CA)

A **Certificate Authority (CA)** is a trusted third party that verifies the identity of websites.

Its job is to answer one question:

> **"Does this public key really belong to this website?"**

---

# How Certificates Work

Before a website goes online:

1. The Server generates:
   - Public Key
   - Private Key
2. The Server proves its identity to a Certificate Authority.
3. The CA creates a certificate containing:
   - Server's Public Key
   - Domain Name
   - Owner Information
   - Issuer
   - Expiration Date
4. The CA digitally signs the certificate.
5. The Server stores the certificate.

---

# During a Secure Connection

The process is:

1. Client requests a secure connection.
2. Server sends its certificate.
3. Client verifies the certificate.

The Client checks:

- Is the certificate expired?
- Does the domain name match?
- Is the Certificate Authority trusted?
- Is the digital signature valid?

If every check succeeds, the Client trusts the Server's Public Key.

---

# Digital Signatures

The Certificate Authority signs the certificate using its **Private Key**.

The browser verifies the signature using the CA's **Public Key**.

Successful verification confirms:

- The certificate has not been modified.
- It was issued by a trusted CA.
- The Server's Public Key is authentic.

---

# Trusted Root Certificate Authorities

Browsers and operating systems already include trusted Root CAs.

Examples include:

- DigiCert
- GlobalSign
- Let's Encrypt
- Sectigo

The browser normally **does not contact the CA** during every HTTPS connection.

---

# Root CA vs Intermediate CA

| Root CA | Intermediate CA |
|----------|-----------------|
| Highest trusted authority | Receives trust from the Root CA |
| Private key is heavily protected | Signs website certificates |
| Rarely signs certificates directly | Used for day-to-day certificate issuance |

Using Intermediate CAs helps protect the Root CA's private key.

---

## Trust Chain

```text
Root CA
   │
   │ signs
   ▼
Intermediate CA
   │
   │ signs
   ▼
Server Certificate
   │
   ▼
Browser verifies the chain
```

---

# Key Takeaways

- HTTP uses status codes to indicate request results.
- HTTP methods specify the action requested by the client.
- HTTPS combines asymmetric and symmetric encryption.
- Asymmetric encryption establishes trust and exchanges a session key.
- Symmetric encryption protects ongoing communication.
- PKI verifies server identity using digital certificates.
- Certificate Authorities (CAs) issue and sign trusted certificates.
- Browsers trust certificates through a chain beginning with Root CAs.
