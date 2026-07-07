🔍 OverTheWire Bandit: Level 21 ➡️ Level 22
### 📝 Level Overview
- Objective: Intercept and analyze a regularly executing system task (`cron job`) to find where restricted credentials are being temporarily cached, bypassing strict directory permissions.

- Current Access: `bandit21`

- Target Access: `bandit22`

- Key Lessons Learned: Linux Cron Jobs (`/etc/cron.d`), Bash Script Analysis, Insecure File Permissions, Information Disclosure Vulnerabilities.
## 🔍 The Technical Challenge & Reconnaissance
### Step 1: Investigating the Scheduled Jobs Context
The instructions state that a program runs automatically via `cron` (the Linux time-based job scheduler). To see what configurations are exposed to our current privilege level, we inspect the cron directory:
```bash
cd /etc/cron.d/
ls -l
```
We identify a configuration file named `cronjob_bandit22`
### Step 2: Reading the Cron Configuration
Reading the contents of `cronjob_bandit22`:

```Bash
cat cronjob_bandit22
```
Output Breakdown:

```Plaintext
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/nul
```
The five asterisks (`* * * * *`) indicate that this job executes every single minute. It is executed under the user context of `bandit22`, running a shell script located at `/usr/bin/cronjob_bandit22.sh`.
### Step 3: Deconstructing the Target Script
Since our active user cannot read `/etc/bandit_pass/bandit22` directly due to a `Permission Denied` restriction, we inspect the logic of the executing script to see how it handles the secret data:
```bash
cat /usr/bin/cronjob_bandit22.sh
```
Script Logic Analysis:

- `chmod 644 /tmp/t706lds9S0Rqqh9aMcz6ShpAoZKF7fgv`: This command changes the file permissions of a temporary file to be globally readable (`644`).
- `cat /etc/bandit_pass/bandit22 > /tmp/t706lds9S0Rqqh9aMcz6ShpAoZKF7fgv`: Running with elevated `bandit22` privileges, the script reads the password file and redirects (copies) its contents directly into that world-readable temporary file inside `/tmp`.
### 🚀 The Exploitation & Extraction
Because the script automatically leaves a readable copy of the password in a shared temporary directory every minute, we can act as an unauthorized auditor and read the leaked file directly:
```bash
cat /tmp/t706lds9S0Rqqh9aMcz6ShpAoZKF7fgv
```
- Result: The terminal prints the plaintext password for Level 22
### 🧠 Real-World Security Implications
1. Information Disclosure via Shared DirectoriesIn
In production environments (especially shared hosting or multi-tenant clouds), writing sensitive structural application data, credentials, or encryption keys into global scratchpad directories like `/tmp or /var/tmp` without explicitly restricting file masks (`umask`) introduces severe Information Disclosure vulnerabilities. Local attackers can monitor these directories to harvest active administrative credentials.
2. Privilege Escalation Vectors
If a system administrator misconfigures the permissions of the execution script itself (making it writeable by standard users), an attacker can inject malicious code into the cron file. When the high-privilege daemon re-runs the task on its automated schedule, it will execute the attacker's payload as the elevated user, completely compromising the operating system.

