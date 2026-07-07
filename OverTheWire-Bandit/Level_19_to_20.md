OverTheWire Bandit: Level 19 ➡️ Level 20
### 📝 Level Overview
- Objective: Retrieve the password for Level 20 from the restricted file /etc/bandit_pass/bandit20 by exploiting a SetUID binary located in the home directory.

- Current Access: `bandit19`

- Target Access: `bandit20`

- Key Lessons Learned: SetUID (Set User ID) Permissions, Privilege Escalation Foundations, Executing Commands as Another User, Linux File Ownership.
### 🔍 The Technical Challenge & Reconnaissance
In this level, the target password resides in its usual path: `/etc/bandit_pass/bandit20`. However, this file is strictly owned by `bandit20` and cannot be read by our current user `bandit19`, resulting in a `Permission Denied` error under normal circumstances.
### Step 1: Investigating the Home Directory
Listing the contents of the home directory reveals a special binary:

```Bash
ls -l
```
- Output Observation:
We see an executable named `bandit20-do`. Looking closely at its permission bits: `-rwsr-xr-x`. The presence of the `s` instead of an `x` in the owner's execution slot indicates that this is a SetUID binary, and its owner is `bandit20`.
### Step 2: Analyzing the Binary's Behavior
Following the level instructions, we execute the binary without any arguments to see its usage guidance:

```Bash
./bandit20-do
```
- Result: The binary outputs: `Run a command as another user`. along with an example: `./bandit20-do whoami`.
Testing the example:

```Bash
./bandit20-do whoami
```
- Result: The terminal outputs `bandit20`. This confirms that any command passed as an argument to this binary will inherit the execution privileges of the file owner (`bandit20`), not the active shell user (`bandit19`).
### Step 3: Exploiting SetUID for Data Extraction
Since the binary executes arbitrary commands with `bandit20` privileges, we can pass the standard `cat` command targeted at the restricted password file:
```Bash
./bandit20-do cat /etc/bandit_pass/bandit20
```
- Result: The binary executes `cat` under the context of `bandit20`, successfully bypasses the filesystem restrictions, and outputs the password for Level 20 directly to our terminal screen.
## 🚧 Pitfalls, Traps, and Solutions (The Real Learning)
### ❌ Trap 1: Confusing Process Contexts vs. User Shells
- The Concept: A common misconception is that running a SetUID binary changes your current interactive shell user permanently.

- The Reality: The privilege elevation is strictly temporary and encapsulated only within the lifecycle of that specific process. Once `./bandit20-do` finishes printing the data and exits, your execution context instantly drops back to the restricted `bandit19` permissions. This is why we must feed it the specific file-reading command as an argument directly rather than trying to spawn a normal shell inside it blindly.
### 🧠 Key Concepts Learned & Real-World Applications
1. Understanding SetUID (Set User ID) Mechanims
SetUID is a vital Linux permission feature used when standard users require temporary elevated privileges to perform core system tasks. A real-world example is the `passwd` binary (located at `/usr/bin/passwd`), which carries SetUID permissions owned by `root`. It allows standard users to temporarily write their new password hashes directly into the highly protected `/etc/shadow` file.
2. Linux Privilege Escalation (Penetration Testing)
During infrastructure security audits and penetration testing, finding custom or misconfigured SetUID binaries is one of the primary vectors for Local Privilege Escalation (LPE). If an administrator creates a wrapper script or binary with SetUID capabilities that allows arbitrary command execution (like a shell escape or unquoted parameters), an attacker can easily leverage it to execute commands as `root`, completely compromising the operating system.
