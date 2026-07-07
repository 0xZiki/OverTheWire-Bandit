OverTheWire Bandit: Level 17 ➡️ Level 18
### 📝 Level Overview
- Objective: Compare two almost identical files (passwords.old and passwords.new) in the home directory, find the only line that has been changed, and extract it as the password for the next level.

- Current Access: bandit17

- Target Access: bandit18

- Key Lessons Learned: File Diffing (diff), Text Stream Manipulation, Linux Pipeline Optimization, Troubleshooting Output Ambiguity.
### 🔍 The Technical Challenge & Reconnaissance
In this level, we are faced with a data analysis problem rather than a network scan. We have two massive password lists containing corporate/system hashes from previous levels. The target credential is hidden inside passwords.new, and it is explicitly defined as the only modified line between the two files.
### Step 1: Initial Investigation
Checking the directory contents reveals two files:

```Bash
ls -l
```
Both files contain a large number of lines, meaning manual inspection is completely out of the question.
### Step 2: The Pipeline Approach (Initial Attempt)
A smart way to approach this using standard CLI text processing is combining cat, sort, and uniq with the unique flag (-u) to isolate unique lines across both files:

```Bash
cat passwords.old passwords.new | sort | uniq -u
```
* Result: This successfully filters out all duplicated lines. However, it presents an ambiguity problem: it returns two hashes instead of one (the old hash from the old file, and the new hash from the new file), leaving you to guess which one is the active password.
### Step 3: Precise Differentiation via Native Diffing
To eliminate guessing, we utilize Linux's native file-diffing utility which tracks structural changes and direction:
```Bash
diff passwords.old passwords.new
```
* Result: The terminal outputs the exact modification block with precise directional indicators:

- `<` points to the old file's value (the removed line).

- `>` points to the new file's value (the newly inserted line starting with 0QxXZjE...).

This gives us the exact password for Level 18 instantly.
## 🚧 Pitfalls, Traps, and Solutions (The Real Learning)
### ❌ Trap 1: The Ambiguity of `uniq -u` in Concatenated Streams
- The Mistake: Relying solely on `cat file1 file2 | uniq -u` to find modified configuration or credential lines.

- The Error/Limitation: While uniq `-u` is excellent for finding lines that appear exactly once, when a line is modified between two files, both the old version and the new version are technically "unique" in the combined stream. Therefore, the pipeline prints both, forcing the engineer to guess or cross-reference manually.

- The Lesson: When the objective is tracking changes or updates between two versions of a file (like configuration auditing or source code analysis), native diffing tools are structurally superior to raw text stream filtering.
### 🧠 Key Concepts Learned
1. Differential Data Analysis (`diff`)
We learned how to track system drift and configuration modifications using diff. Understanding how to read its output symbols (< for deletions/old state and > for additions/new state) is an essential skill for analyzing system updates, auditing modified configurations, or reviewing code patches in production environments.

2. Stream Pipelines vs. Context-Aware Tools
While combining multiple utilities via pipes (|) is one of Linux's greatest strengths, this level proved that a single context-aware tool (like diff) can sometimes be cleaner and more accurate than a long pipeline chain. Choosing the right tool for the right job prevents data noise and saves execution overhead.
