# Bandit Level 2 ➔ Level 3 Writeup

## 🎯 Objective
Find the password hidden in a file named `spaces in this filename` located in the home directory.

## 🧩 The Challenge
In Linux, spaces are used to separate commands and arguments. Running `cat spaces in this filename` makes the shell think you are trying to open four separate files.

---

## 🛠️ Solutions & Creative Approaches

### Method 1: Using Quotes (Standard Way)
Wrapping the filename in quotes forces the shell to treat it as a single string argument:
```bash
cat "spaces in this filename"
```
Method 2: Escape Characters (Auto-complete Way)
Using backslashes \ before each space escapes them, indicating they are part of the filename:

```Bash
cat spaces\ in\ this\ filename
```
Method 3: The Creative rev Trick (Our Custom Way)
Leveraging the rev trick learned from previous research combined with path expansion to read and reverse the output:

```Bash
rev ./--spaces\ in\ this\ filename-- | rev
```
🧠 Concepts Learned
Handling Spaces: Escaping spaces using backslashes \ or wrapping filenames in double quotes " ".

Command Adaptability: Reapplying alternative utilities like rev to bypass standard command restrictions.
