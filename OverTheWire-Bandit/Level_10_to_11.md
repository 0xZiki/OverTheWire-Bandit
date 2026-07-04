## 🧩 The Challenge

The password for the next level is stored in the file `data.txt` and contains base64 encoded data.

---

## 🛠️ Solutions

### Method 1: Direct Decoding (Using `base64`)

Since the challenge explicitly states that the data is Base64 encoded, we can bypass raw reading and use the built-in `base64` utility with the decode flag to translate the encoded string back into plain human-readable text:

```bash
base64 -d data.txt
```
### Method 2: The Pipeline Decoding (Using cat & base64)
Alternatively, we can read the encoded text file using cat and pipe the stream directly into the base64 decoder:

```Bash
cat data.txt | base64 -d
```
Output Analysis: Both commands instantly process the encoded data and print out the decoded cleartext password on the terminal screen.

## 🧠 Concepts Learned
Data Encoding vs. Encryption: Understanding that Base64 is a binary-to-text encoding scheme used to safely transport data, not to encrypt or secure it.

Base64 Decoding (-d flag): Learning how to use command-line utilities to reverse encoding layouts and reveal the underlying original payload.

Stream Chaining: Reinforcing the usage of Linux pipes (|) to push raw inputs through decoding filters cleanly.
