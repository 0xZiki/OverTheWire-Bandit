# 🎯 OverTheWire Bandit: Level 30 ➡️ Level 31

## 📌 Level Overview
* Objective: Discover a hidden password tucked away inside a Git Tag object, avoiding the default branch and commit histories.
* Current Access: bandit30
* Target Access: bandit31
* Key Lessons Learned: Git Tag Enumeration (`git tag`), Object Inspection (`git show`), Release/Tag-based Security Auditing, Metadata Extraction.

---

## 🕵️ The Technical Challenge & Reconnaissance

### Step 1: Clone and Inspect
We established local tracking by cloning the `bandit30-git` instance via port `2220`:
```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
```
Checking the `master` branch and standard history (`git log`) yielded no plaintext secrets. Similarly, hunting active tracking branches (`git branch -a`) didn't return developmental anomalies.
### Step 2: Extracting Git Tags
To scan alternative release artifacts or milestones left by the deployment workflow, we queried the repository's tag metadata:

```Bash
git tag
```
Output returned a specific, highly suspicious tag identifier:

- secret
## 🧠 Deep Linux & SCM Concepts Investigated
### 1. Git Tags as Immutable Pointers
In Software Configuration Management (SCM), a Tag acts as a permanent pointer to a specific commit snapshot, often signifying releases (like `v1.0.0`). While a branch moves forward with every push, a tag remains stationary. Attackers audit tags because developers frequently tag a working state containing testing keys, certificates, or deployment passwords before cleaning up the main code line.
### 2. Tag Payload Examination
Using `git show <tag>` against a tag object forces Git to resolve the internal object hash—whether it points to a lightweight commit tracking element or an annotated tag object—and print its contents, author info, and code differential payload.
## ⚔️ The Exploitation & Injection (Step-by-Step)
### Step 1: Reading the Tag Payload
We targeted the identified secret tag reference using the dynamic inspection primitive:

```Bash
git show secret
```
### Step 2: Parsing the Flag
The metadata block revealed the cleartext string sequence tied explicitly to that structural reference.

```Plaintext
<PLACE_THE_PASSWORD_YOU_FOUND_HERE>
```
## 🏁 Extraction & Post-Exploitation
By leveraging tag infrastructure enumeration, we bypassed branch-level sanitization and pulled the credential marker.

- Target Plaintext Password: <PLACE_THE_PASSWORD_YOU_FOUND_HERE>
