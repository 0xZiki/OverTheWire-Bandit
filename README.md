# 🏴‍☠️ OverTheWire Bandit - Linux CLI Journey

Welcome to my repository! This space is dedicated to documenting my journey in the **First Front** of my training plan, aimed at deeply understanding Linux system architecture, mastering the Command Line Interface (CLI), and networking, in preparation for advanced CTFs and System Hacking.

## 🎯 Objectives:
1. Solve and document **OverTheWire: Bandit** challenges step-by-step.
2. Understand the mechanics of commands and security concepts behind each level, not just getting the flag.
3. Build a solid foundation for handling Linux servers via CLI like a pro.

---

## 📑 Git & GitHub Quick Cheat Sheet
*My quick reference for daily workflow:*

### Daily Workflow:
```bash
git add .
git commit -m "Add Bandit Level X write-up"
git push
```
Quick Checks:
Check file status: git status

View commit history: git log --oneline

🚩 Write-ups & Levels:
* [Bandit Level 0 ➔ 1](OverTheWire-Bandit/Level_00_to_01.md) *(Concepts: SSH, Basic Navigation, cat)*
* [Bandit Level 1 ➔ 2](OverTheWire-Bandit/Level_01_to_02.md) *(Concepts: Dashed Filenames, Relative Paths)*
* [Bandit Level 2 ➔ 3](OverTheWire-Bandit/Level_02_to_03.md) *(Concepts: Handling Spaces, Escaping Characters, rev)*
* [Bandit Level 3 ➔ 4](OverTheWire-Bandit/Level_03_to_04.md) *(Concepts: Hidden Files, ls -al)*
* [Bandit Level 4 ➔ 5](OverTheWire-Bandit/Level_04_to_05.md) *(Concepts: File Utility, ASCII vs Binary Data, Terminal Reset)*
* [Bandit Level 5 → 6](OverTheWire-Bandit/Level_05_to_06.md) *(Concepts: The find Command, Exact File Sizing, Logical NOT)*
* [Bandit Level 6 → 7](OverTheWire-Bandit/Level_06_to_07.md) *(Concepts: System-Wide Searching, Ownership Filtering, I/O Redirection)*
* [Bandit Level 7 → 8](OverTheWire-Bandit/Level_07_to_08.md) *(Concepts: Text Filtering using grep, Linux Pipes)*
* [Bandit Level 8 → 9](OverTheWire-Bandit/Level_08_to_09.md) *(Concepts: Text Sorting, Advanced Uniq Filtering with -u)*
* [Bandit Level 9 → 10](OverTheWire-Bandit/Level_09_to_10.md) *(Concepts: Binary String Extraction using strings, Pattern Matching)*
* [Bandit Level 10 → 11](OverTheWire-Bandit/Level_10_to_11.md) *(Concepts: Data Encoding vs Encryption, Base64 Decoding)*
* [Bandit Level 11 → 12](OverTheWire-Bandit/Level_11_to_12.md) *(Concepts: Caesar Cipher, ROT13 Obfuscation, Text Translation using `tr`)*
* [Bandit Level 12 → 13](OverTheWire-Bandit/Level_12_to_13.md) *(Concepts: Reverse Engineering Hex Dumps (`xxd`), Multi-layered Decompression (`gzip`, `bzip2`, `tar`)*
* [Bandit Level 13 → 14](OverTheWire-Bandit/Level_13_to_14.md) *(Concepts: Key-Based SSH Authentication, File Permissions (`chmod 600`), Cryptographic Error Troubleshooting, Data Exfiltration )*
* [Bandit Level 14 → 15](OverTheWire-Bandit/Level_14_to_15.md) *(Concepts: Networking Basics, Localhost/Loopback (`127.0.0.1`), Network Ports, Raw Socket Data Transmission using Netcat (`nc`))*
* [Bandit Level 15 → 16](OverTheWire-Bandit/Level_15_to_16.md) *(Concepts: SSL/TLS Encryption, `openssl s_client` Connections)*
* [Bandit Level 16 → 17](OverTheWire-Bandit/Level_16_to_17.md) *(Concepts: Port Scanning with `nmap`, SSL Service Identification, Private Key Handling)*
* [Bandit Level 17 → 18](OverTheWire-Bandit/Level_17_to_18.md) *(Concepts: File Comparison with `diff`, Investigating Modified Files)*
* [Bandit Level 18 → 19](OverTheWire-Bandit/Level_18_to_19.md) *(Concepts: SSH Command Execution, Bypassing Modified/Restricted Shells)*
* [Bandit Level 19 → 20](OverTheWire-Bandit/Level_19_to_20.md) *(Concepts: SUID/SetUID Binaries, Executing Files as Another User)*
* [Bandit Level 20 → 21](OverTheWire-Bandit/Level_20_to_21.md) *(Concepts: Netcat Listening Mode, Inter-Process Communication, Backgrounding Jobs `&`)*
* [Bandit Level 21 → 22](OverTheWire-Bandit/Level_21_to_22.md) *(Concepts: Linux Cron Jobs, Scheduled Task Analysis, Inspecting `/etc/cron.d`)*
* [Bandit Level 22 → 23](OverTheWire-Bandit/Level_22_to_23.md) *(Concepts: Shell Script Reversing, MD5 Hashing, Dynamic/Predicted Paths)*
* [Bandit Level 23 → 24](OverTheWire-Bandit/Level_23_to_24.md) *(Concepts: Executing Arbitrary Shell Scripts via Insecure Spool Directory)*
* [Bandit Level 24 → 25](OverTheWire-Bandit/Level_24_to_25.md) *(Concepts: Network Bruteforce Scripting, Bash For-Loops, Multiple Port Iteration)*
* [Bandit Level 25 → 26](OverTheWire-Bandit/Level_25_to_26.md) *(Concepts: Non-Standard Shell Infiltration, Text Editors inside SSH, Overriding Default Logins)*
* [Bandit Level 26 → 27](OverTheWire-Bandit/Level_26_to_27.md) *(Concepts: Custom Interactive Shell Interception, Command Invocation Evasion)*
* [Bandit Level 27 → 28](OverTheWire-Bandit/Level_27_to_28.md) *(Concepts: Source Code Repository Auditing, Git over SSH, Secret Leakage)*
* [Bandit Level 28 → 29](OverTheWire-Bandit/Level_28_to_29.md) *(Concepts: Git Commit History Forensic Review, Code Diffing using `git show`)*
* [Bandit Level 29 → 30](OverTheWire-Bandit/Level_29_to_30.md) *(Concepts: Remote Branch Enumeration, Detached HEAD States, Multi-branch Security)*
* [Bandit Level 30 → 31](OverTheWire-Bandit/Level_30_to_31.md) *(Concepts: Git Tag Objects Analysis, Release-based Target Credential Extraction)*
* [Bandit Level 31 → 32](OverTheWire-Bandit/Level_31_to_32.md) *(Concepts: CI/CD Deployment Overrides, Bypassing `.gitignore` Restrictions via Force-Staging)*
* [Bandit Level 32 → 33](OverTheWire-Bandit/Level_32_to_33.md) *(Concepts: Restricted Shell Evasion, Positional Parameters Subversion using `$0`)*
* [Bandit Level 33 → 34](OverTheWire-Bandit/Level_33_to_34.md) *(Concepts: Mission Accomplished, Full-Stack Linux Security Framework Verification)*
