# OverTheWire Bandit: Level 15 ➡️ Level 16

## 📝 Level Overview
* **Objective:** Retrieve the next level's password by submitting the current level's password to port `30001` on `localhost` using SSL/TLS encryption.
* **Current Access:** `bandit15`
* **Target Access:** `bandit16`
* **Key Tools & Techniques:** SSL/TLS Encryption, Secure Sockets, OpenSSL (`openssl s_client`), Network Debugging.

---

## 🔍 The Technical Challenge
In the previous level, we transmitted data over a raw, unencrypted TCP socket. However, this challenge introduces **Transport Layer Security (SSL/TLS)**. 

Port `30001` is protected by an SSL/TLS daemon. If we attempt to use standard network utilities like `nc` (Netcat) without encryption flags, the server will immediately terminate the connection because it expects an encrypted handshake. To succeed, we must initiate a secure session that handles the cryptographic negotiation automatically.

---

## 🛠️ Step-by-Step Methodology

### Step 1: Secure the Authentication Token
First, read and copy the password for the current user (`bandit15`) to serve as our authentication payload.
```bash
cat /etc/bandit_pass/bandit15
```
### Step 2: Establish an Encrypted SSL/TLS Connection
Instead of standard Netcat, we use the openssl utility with the s_client plugin. This acts as a generic SSL/TLS client that connects to a remote (or local) host and establishes a secure tunnel.

```Bash
openssl s_client -connect localhost:30001
```
Note: The command connects to the loopback interface on port 30001, negotiates the cipher suites, validates the self-signed certificate, and keeps the raw connection open for transmission.
### Step 3: Data Transmission & Retrieval
Once the session handshake is complete and the terminal displays read R BLOCK (indicating it is waiting for user input), paste the copied password from Step 1 and hit Enter.

The daemon inside the encrypted tunnel will:

1. Receive the encrypted data packet.

2. Decrypt it and validate the bandit15 password.

3. Transmit back the plain-text password for Level 16 over the same secure channel.
### 🧠 Key Concepts Learned
1. SSL/TLS vs. Plaintext Traffic:

Standard tools like nc transmit data in plaintext, exposing it to sniffing attacks. SSL/TLS adds a layer of encryption over TCP, ensuring Confidentiality and Integrity during transit.

2. Cryptographic Handshakes:

Observed how the client and server exchange certificates, agree on cipher suites (e.g., TLS_AES_256_GCM_SHA384), and generate session keys before any actual application data is sent.

3. Session Interactivity:

Learned to interact with secure network services manually using terminal-based cryptographic tools.
