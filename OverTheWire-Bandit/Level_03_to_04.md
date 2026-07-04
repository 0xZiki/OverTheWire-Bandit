# Bandit Level 3 ➔ Level 4 Writeup

## 🎯 Objective
Find the password hidden in a hidden file inside the `inhere` directory.

## 🧩 The Challenge
In Linux, files or directories that start with a dot `.` are hidden by default. Running a standard `ls` command inside the `inhere` folder returns nothing, making it look empty.

---

## 🛠️ Solution
To reveal hidden files, we need to pass the `-a` (all) flag to the `ls` command.

1. Navigate to the target directory:
   ```bash
   cd inhere
   ```
2. List all files, including hidden ones:

```Bash
ls -al
```
3. Read the revealed hidden file named ...Hiding-From-You:
```
Bash
cat ...Hiding-From-You
```
## 🧠 Concepts Learned
Hidden Files in Linux: Any file prefixing with . is hidden from standard directory listings.

ls -a Flag: Crucial command switch used to audit and view hidden configurations or hidden files.
