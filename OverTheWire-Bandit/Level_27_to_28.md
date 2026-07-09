# 🎯 OverTheWire Bandit: Level 27 ➡️ Level 28

## 📌 Level Overview
* Objective: Clone a remote Git repository hosted on the OverTheWire server to extract hidden password credentials from the repository files.
* Current Access: bandit27
* Target Access: bandit28
* Key Lessons Learned: Version Control Enumeration, Git over SSH, Source Code Review, Secret Leakage Auditing.

---

## 🕵️ The Technical Challenge & Reconnaissance

### Step 1: Identifying the Vector
The level details provide a specific connection string for a Git repository managed under the `bandit27-git` user profile:
`ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo`

### Step 2: Authentication Mapping
The challenge dictates that the password for the Git account matches the system password discovered for `bandit27` in the prior challenge:
`YnQpBuifNMas1hcUFk70Zmqfc1030MOU`

---

## 🧠 Deep Linux Concepts Investigated

### 1. Secret Leakage in Source Code Management (SCM)
In production security environments, developers frequently hardcode sensitive information (API tokens, database credentials, private keys) directly into software repositories for rapid testing, inadvertently pushing them upstream. Auditing exposed Git repositories is a critical assessment vector during external penetration testing.

### 2. Custom Port Redirection in Git SSH Strings
Standard Git SSH URLs default to port 22. Because OverTheWire isolates its infrastructure on port `2220`, the connection string must be appended with the correct port notation (`:2220`) immediately following the host domain to prevent connection timeouts.

---

## ⚔️ The Exploitation & Injection (Step-by-Step)

### Step 1: Cloning the Repository Locally
From a local terminal equipped with `git`, execute the cloning function targeting the explicit port sequence:
```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```
- When prompted for password verification, inject the `bandit27` flag credential.
### Step 2: Project Traversal & File Review
Once the directory structure is pulled locally, enter the cloned repository directory:

```Bash
cd repo
```
Inspect the structure to locate project documentation or active text descriptors:
```Bash
ls -la
```
Output reveals a `README` configuration file inside the root repository.
### Step 3: Extracting the Flag
Read the cleartext documentation contents:

```Bash
cat README
```
## 🏁 Extraction & Post-Exploitation
Reviewing the `README` output isolates the configuration documentation alongside the hardcoded target credential.
Output:

```Plaintext
The password for bandit28 is <INSERT_BANDIT28_PASSWORD_HERE>
```
- Result: The plaintext flag for Level 28 is successfully retrieved.

- Note: This password changes from one user instance to another.
