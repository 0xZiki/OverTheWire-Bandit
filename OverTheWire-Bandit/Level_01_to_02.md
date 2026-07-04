# Bandit Level 1 ➔ Level 2 Writeup

## Objective
Find the password hidden in a file named `-` located in the home directory.

## Solution
Since `-` is interpreted by Linux commands as standard input (stdin), we cannot open it using a normal `cat -` command. To force the command to treat it as a filename, we must specify its relative path:

```bash
cat ./- 
```
Method 2: Input Redirection (Alternative)
We can use the < redirection operator to pass the file content directly into the cat command:

```Bash
cat < -
```
Method 3: Using the rev Command
The rev utility reverses the characters of lines. Interestingly, it treats - as a normal filename, allowing us to read it (and then we can reverse it back if needed, though for the password it prints it out):

```Bash
rev - | rev
```
🧠 Concepts Learned
Standard Input (stdin) Symbol: Understanding why - causes commands to wait for keyboard input.

Input Redirection (<): How to force commands to take a file as input.

Alternative CLI Utilities: Leveraging tools like rev or text editors to bypass naming constraints.
