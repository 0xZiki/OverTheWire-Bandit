## 🧩 The Challenge

The password for the next level is stored in the file `data.txt` next to the word `millionth`.

---

## 🛠️ Solutions

### Method 1: The Direct Search (Using `grep`)

Since `data.txt` is a massive text file containing thousands of lines, opening it completely with `cat` would overwhelm the terminal viewport. 

Instead, we can use the `grep` utility to filter the file content directly and search for the specific keyword provided by the challenge:

```bash
grep millionth data.txt
```
### Method 2: The Pipeline Approach (Using cat and grep)
Alternatively, we can read the file content and pipe the output as an input into grep using the Linux pipe operator (|):
```bash
cat data.txt | grep millionth
```
Output Analysis: Both methods cleanly bypass the massive text dump and instantly return the exact line containing the word millionth followed by the target password.

## 🧠 Concepts Learned
The file Command Utility: Using it beforehand to verify that data.txt is indeed a plain text file (Unicode text, UTF-8 text), which ruled out the need for decompression or decoding tools like gzip, tar, or base64.

The grep Tool: A powerful command-line utility used for searching plain-text data sets for lines that match a regular expression or a specific keyword.

Linux Pipes (|): A form of redirection that sends the output of one command (stdout) to another command as input (stdin), allowing for efficient command chaining.
