# 🎯 OverTheWire Bandit: Level 32 ➡️ Level 33

## 📌 Level Overview
* Objective: Break out of a highly restrictive command shell (Jailbreak/Shell Escape) that intercept inputs, and upgrade to an unrestricted POSIX compliance execution shell to read the next level password.
* Current Access: bandit32
* Target Access: bandit33
* Key Lessons Learned: Restricted Shell Evasion (rbash/sh escapes), Positional Parameters (`$0`), Shell Execution Contexts, Local Privilege Verification.

---

## 🕵️ The Technical Challenge & Reconnaissance

### Step 1: Analyzing the Prison (Restricted Shell)
Upon authenticating as `bandit32`, we were dropped into a specialized environment designed to block standard system commands or drop inputs entirely. The shell attempts to parse or neutralize binary calls to prevent traversal or process spawning.

---

## 🧠 Deep Linux Concepts Investigated

### 1. Restricted Shells (Jail Environments)
System administrators deploy restricted shells (like `rbash` or custom wrapper profiles) to confine users (like support staff or automated API accounts) to specific commands. These environments typically block:
* Executing commands containing forward slashes (`/`).
* Modifying environment variables like `PATH` or `SHELL`.
* Redirecting outputs via `>`, `>>`, or `<`.

### 2. The Power of `$0` (Positional Parameters)
In POSIX-compliant scripting frameworks, positional parameters hold transactional execution variables. While `$1`, `$2` represent arguments passed to a profile, **`$0`** represents the explicit name of the currently running shell execution binary itself.



If a restricted wrapper runs *on top* of or calls a base shell script, querying `$0` forces the environment to spawn a fresh sub-shell instance of the underlying system command processor, entirely bypassing the restrictive parsing layer applied to user input text.

---

## ⚔️ The Exploitation & Injection (Step-by-Step)

### Step 1: Triggering the Escape Vector
From the restrictive prompt interface, we injected the special variable parameter directly:
```bash
$0
```
Impact: The restricted configuration instantly collapsed, dropping the terminal back into an unmonitored standard POSIX tracking environment (`sh`).
### Step 2: Retrieving the Flag
With standard system commands unblocked, we safely read the target credential store:

```Bash
cat /etc/bandit_pass/bandit33
```
## 🏁 Extraction & Post-Exploitation
By invoking the execution engine pointer variable, we forced the operating system to spin up a raw shell instance outside the oversight of the jail interface profile.

Target Plaintext Password: `od93uuw9OmwscGs2vfZ78v7IdbQUY2or`
