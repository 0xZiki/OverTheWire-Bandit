# 🎯 OverTheWire Bandit: Level 31 ➡️ Level 32

## 📌 Level Overview
* Objective: Push a tracking configuration file (`key.txt`) containing a specific string validation back to the remote server's master branch to trigger an automated credential payout.
* Current Access: bandit31
* Target Access: bandit32
* Key Lessons Learned: Git Push Mechanics, Remote Branch Interactions, Working with `.gitignore` Overrides (`git add -f`), Automated Post-Receive Hooks.

---

## 🕵️ The Technical Challenge & Reconnaissance

### Step 1: Clone and Specification Auditing
We initialized by cloning the repository assigned to `bandit31-git` on the non-default port `2220`:
```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
```
Entering the repository folder and inspecting `README.md` revealed that instead of hunting for an existing secret, the challenge reversed the dynamic:

- Required Filename: `key.txt`

- Required Content: `May I come in?`

- Target Destination Branch: `master`
## 🧠 Deep Linux & SCM Concepts Investigated
### 1. Git Hooks & Automated Evaluation
Many CI/CD (Continuous Integration/Continuous Deployment) environments execute specialized scripts called Git Hooks (such as `post-receive` or `pre-receive`) on the remote server whenever code is pushed. In this security challenge, pushing the correctly structured file triggers a server-side hook that validates the submission and pipes back the next flag via standard output.

### 2. Bypassing `.gitignore` Restrictions
A common obstacle when pushing specific tracking artifacts is the presence of a `.gitignore` file, which blacklists explicit file extensions (like `.txt`) or directory trees from being tracked. Attempting a standard `git add` against a ignored file fails silently or throws an explicitly blocked warning. Attackers and developers bypass this constraint using the Force flag (`-f`).
## ⚔️ The Exploitation & Injection (Step-by-Step)
### Step 1: Crafting the Target Asset
We generated the required `key.txt` file ensuring the exact payload string mapping was populated without extraneous spacing issues:

```Bash
echo 'May I come in?' > key.txt
```
### Step 2: Force-Staging the Artifact
To override potential local system or remote architecture exclusions configured within the hidden `.gitignore` metadata files, we forcefully added the file to the active staging area:
```Bash
git add -f key.txt
```
### Step 3: Committing the Workspace State
We packaged the staging snapshot into a standard tracking block ready for transport:

```Bash
git commit -m "Submit validation tracking key"
```
### Step 4: Pushing Upstream to Trigger the Hook
We pushed the local tracking changes directly back onto the primary processing pipeline:

```Bash
git push origin master
```
When prompted for authorization, we provided the active credential for `bandit31`.

## 🏁 Extraction & Post-Exploitation
Upon a successful upload stream validation, the remote server's active deployment hook parsed the submission payload and printed the cleartext password tracking response into our interactive terminal stream.

Target Plaintext Password: `rmv118tHw3S7L374zS048b69z7r67SGO`
