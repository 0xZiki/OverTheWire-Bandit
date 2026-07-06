# 🔍 OverTheWire Bandit: Level 16 ➡️ Level 17

## 📝 Level Overview
* **Objective:** Scan a specific port range (`31000` to `32000`) on `localhost` to find an SSL/TLS encrypted service, submit the current password, and retrieve an SSH Private Key for the next level.
* **Current Access:** `bandit16`
* **Target Access:** `bandit17` (via Key-based Authentication)
* **Key Lessons Learned:** Network Mapping (`nmap`), Encryption Tunnels (`ncat`), SSH Private Key Security, Linux File Permissions (`chmod`).

---

## 🔍 The Technical Challenge & Reconnaissance
This level is a massive jump in difficulty because it requires active reconnaissance. We are given a range of 1,000 potential ports. Some are closed, some are open in plaintext, and some are "Echo Servers" designed as traps to mimic a valid connection but only return what they receive. Only one port hosts the actual authentication daemon that provides the next level's credentials.

### Step 1: Initial Port Discovery
First, we scan the target range on `localhost` to filter out open ports from closed ones. Running a standard scan bypasses root privilege requirements:
```bash
nmap -p 31000-32000 localhost
```
Result: Discovered 5 open ports: 31046, 31518, 31691, 31790, and 31960.

### Step 2: Service & SSL/TLS Enumeration
To identify which of these open ports use SSL/TLS encryption, we run Nmap with the ssl-enum-ciphers script:

```Bash
nmap -p 31046,31518,31691,31790,31960 --script ssl-enum-ciphers localhost
```
Result: Confirmed which ports utilize valid SSL/TLS cipher suites (e.g., TLSv1.2/TLSv1.3).

### Step 3: Evading the Echo Traps & Extracting the Key
We use ncat with the --ssl flag to establish an encrypted tunnel.

Testing ports like 31046 revealed they were Echo Servers (they mirrored back our password).

Connecting to the correct daemon on port 31790:

```Bash
ncat --ssl localhost 31790
```
## 🚧 Upon entering the password (kS0Hf...), the server responded with Correct! and printed an OpenSSH Private Key.
Pitfalls, Traps, and Solutions (The Real Learning)
### ❌ Trap 1: The Localhost Block
The Mistake: Trying to connect to bandit17 using the private key from within the bandit16 session via localhost on port 2220 or port 22.

The Error: !!! Connecting from localhost is blocked to conserve resources. or !!! You are trying to log into this SSH server on port 22, which is not intended.

The Lesson: The server restricts internal jumping using SSH keys to enforce external client connections. The credential received is an Asymmetric Private Key, which means it must be extracted and used from an external machine (your personal client).
### ❌ Trap 2: Unprotected Private Key File (The SSH Guard)
The Mistake: After copying the key to the local machine, attempting to log in immediately or applying an execute flag (chmod +x bandit16_key).

The Error:
 ```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0711 for 'bandit16_key' are too open.
```
The Lesson: SSH has strict security policies. A Private Key file contains highly sensitive data and MUST NOT be accessible by anyone other than the owner. Adding an execute flag (+x) changes permissions to 0711 which breaks this security rule.
### 🔑 The Correct Path to Access Level 17
Copy the entire private key block from -----BEGIN OPENSSH PRIVATE KEY----- to -----END OPENSSH PRIVATE KEY-----.

Save it to a file named bandit16_key on your local personal machine.

Crucial Step: Restrict the file permissions so only your user can read/write it (Read = 4, Write = 2, Execute = 0 ➡️ 600):
```Bash
chmod 600 bandit16_key
```
Authenticate to the remote server using the private key over the external game port (2220):
```bash
ssh -i bandit16_key bandit17@bandit.labs.overthewire.org -p 2220
```
Success! The server accepts the secured key and logs directly into bandit17@bandit:~$ without asking for a password.
## 🧠 Key Concepts Learned

### 1. Active Network Reconnaissance & Filtering
In realistic environments, services are rarely explicitly pointed out. Using tools like `nmap` allows us to sweep ranges, identify live systems, and filter out noise (such as closed ports) from active entry points. Furthermore, we learned that open ports can be configured as **Echo Servers**—traps that reflect data back to confuse automated scripts, proving that a port being "open" doesn’t automatically mean it is the intended destination.

### 2. Automated SSL/TLS Tunneling (`ncat` vs `openssl`)
While `openssl s_client` is excellent for manually inspecting certificates and debugging handshakes, it outputs highly verbose cryptographic metadata. For seamless input/output data transmission, `ncat --ssl` acts as an automated utility that establishes a clean, raw cryptographic tunnel over TCP, hiding all plaintext traffic from external network sniffers.

### 3. Asymmetric Cryptography & Key-Based Authentication
Authentication isn't limited to static, human-readable passwords. Asymmetric cryptography utilizes a **Key Pair**: a Public Key residing on the server and a Private Key kept by the client. This challenge demonstrated how infrastructure access migrates away from passwords to cryptographic identity validation via an OpenSSH private key structure.

### 4. Linux File Security & The SSH Guard Mechanism
Linux file permissions are strictly enforced to protect cryptographic materials. Programs like SSH implement a fail-safe defensive check: if a sensitive Private Key file has loose permissions (e.g., `0711` or possessing an execution `+x` bit), the protocol assumes the client environment is locally compromised, entirely ignores the key, and drops back to standard password requests to mitigate risk. Restricting the file explicitly to `600` ensures absolute confidentiality.
