# OverTheWire Bandit: Level 14 ➡ evel 15

## 📝 Level Overview
* **Objective:** Retrieve the next level's password by submitting the current level's password to a specific network port on the local machine.
* **Current Access:** `bandit14`
* **Target Access:** `bandit15`
* **Key Tools & Techniques:** Networking Basics, Ports, Localhost, Netcat (`nc`), Data Transmission over TCP.

---

## 🔍 The Technical Challenge
Instead of searching for a file hidden in the system, this level introduces **Network Communication**. The password for level 15 is guarded by a daemon (a background service) listening on port `30000` of the `localhost`. To pass the challenge, we must establish a TCP connection to this port and transmit the password of `bandit14` as authentication.

---

## 🛠️ Step-by-Step Methodology

### Step 1: Retrieve the Current Password
Before opening a connection, we need the authentication token (the current password) ready to be transmitted.
```bash
cat /etc/bandit_pass/bandit14
```
(Copy the output string to your clipboard).

### Step 2: Establish a Network Connection
Use Netcat (nc), often referred to as the "Swiss Army knife" of networking, to open a socket connection to the target port on the local machine.

```Bash
nc localhost 30000
```
### Step 3: Transmit Data & Receive the Flag
Once the connection is established (the terminal will wait for your input), paste the copied password from Step 1 and press Enter.

The service will validate the string.

Upon successful validation, the daemon responds by sending back the password for bandit15.

The connection will then automatically close.
### 🧠 Key Concepts Learned
1. Localhost & Loopback Interface:

localhost (IP: 127.0.0.1) is a loopback network interface. It allows a computer to communicate with itself over network protocols without the traffic ever leaving the machine.

2. Ports & Services:

A port is a logical endpoint for communication. While IP addresses identify machines, ports identify specific services or applications running on those machines (e.g., HTTP on 80, SSH on 22, or our custom service on 30000).

3. Netcat (nc) Mastery:

Introduced the ability to manually interact with network services, read/write data across network connections using TCP/UDP, and understand raw socket communication.
