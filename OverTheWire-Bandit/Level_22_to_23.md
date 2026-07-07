# 🔍 OverTheWire Bandit: Level 22 ➡️ Level 23

## 📝 Level Overview
* Objective: Reverse-engineer a dynamic cron job script that calculates storage paths using cryptographic hashing algorithms.
* Current Access: `bandit22`
* Target Access: `bandit23`
* Key Lessons Learned: Cryptographic Hashing (MD5), Information Hashing Logic, Command Redirection via Command Substitution.

---

## 🔍 The Technical Challenge & Reconnaissance

### Step 1: Locating the Dynamic Cron Script
We start by looking into the scheduled tasks inside `/etc/cron.d/`
```bash
cd /etc/cron.d/$ ls -l
```

We open the targeted cron configuration file `cat cronjob_bandit23` :

Output:
```bash
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```
This target script runs every minute under the administrative privileges of user "bandit23".

### Step 2: Analyzing the Hashing Script
We print the script to see how it hides the output directory `cat /usr/bin/cronjob_bandit23.sh`:
Output
```bash
#!/bin/bash
myname=$(whoami)
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
### 🧠 Core Logic Deconstruction:
1. The script identifies the automated context execution name using `whoami`. (When the system runs it, this equals bandit23).

2. It generates a predictable, deterministic unique signature via `md5sum`.

3. It takes the text string `I am user bandit23`, hashes it, and cuts away the trailing fields, assigning the resulting string to the variable `$mytarget`.

4. It reads the master password file and writes it to `/tmp/$mytarget`.
### The Exploitation & Extraction
Because cryptographic hashes are deterministic (the same input ALWAYS yields the same output), we don't need to break into the script. We simply recreate the calculation command manually on our active terminal session, replacing the automated context variable with the target name `bandit23`:
The input:
```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```
Output calculated hash target:
```bash
8ca319486bfbbc3663ea0fbe81326349
```
Now we know exactly where the background task drops the credentials. We extract the password directly from the dynamic filename:
```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```
- Result: Successfully retrieved plaintext credentials for Level 23!
## 🛡️ Deep Dive: What is Hashing & How Do Attacks Target It?
###  1️⃣ What is the Main Purpose of Hashing?
Unlike encryption, Hashing is a One-Way Function. You can easily turn data into a hash, but you can NEVER mathematically reverse a hash back to its original input string. Its major production uses include:
- Password Storage: Systems never save plaintext passwords. They save the hash. When you log in, the system hashes your input and compares it to the stored hash.
- Data Integrity Checks (Checksums): When downloading large files or operating system ISOs, developers provide an MD5/SHA256 checksum hash. If even a single bit of the file is modified by a corrupt download or malware injection, the computed hash completely changes (known as the Avalanche Effect).
##  2️⃣ Real-World Attacks Against Hashes
While you cannot "decrypt" a hash directly, attackers use clever strategies to break or bypass them during penetration testing:
### A. Dictionary Attacks & Rainbow Tables (Pre-computation)
If an attacker steals a database containing hashed passwords, they cannot reverse them. Instead, they take millions of common passwords (like `password123`), compute their hashes in advance, and store them in a massive lookup matrix called a Rainbow Table. They then compare the stolen hashes against this table to look for an instantaneous match.

- Defense: Modern secure applications inject random bits of data called a Salt to the plaintext password before hashing it (e.g., `password123 + random_salt`), rendering pre-computed Rainbow Tables completely useless.
### B. Hash Collision Attacks
A cryptographic collision occurs when two completely different, unique input strings pass through a hashing algorithm and accidentally produce the exact same final hash fingerprint.
- Algorithms like MD5 and SHA-1 are considered cryptographically broken today because researchers discovered mathematical patterns to intentionally generate collisions. Attackers can forge a malicious file that mimics the exact integrity hash signature of a trusted software file.
- Defense: Production systems must enforce modern collision-resistant algorithms like SHA-256, SHA-3, or key-stretching functions like bcrypt/Argon2 for password handling.
