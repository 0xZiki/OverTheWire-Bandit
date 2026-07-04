# Bandit Level 1 ➔ Level 2 Writeup

## Objective
Find the password hidden in a file named `-` located in the home directory.

## Solution
Since `-` is interpreted by Linux commands as standard input (stdin), we cannot open it using a normal `cat -` command. To force the command to treat it as a filename, we must specify its relative path:

```bash
cat ./- 
```
Concepts Learned
Dashed Filenames: How Linux commands handle special characters.

Relative Paths: Using ./ to reference the current directory.

