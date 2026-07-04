# Bandit Level 11 → Level 12 Base Documentation

## 📝 Level Description
The password for the next level is stored in a file named `data.txt`. Inside this file, all lowercase (a-z) and uppercase (A-Z) letters have been rotated/shifted by 13 positions using a classic substitution cipher known as **ROT13**.

---

## 🔍 Concepts Learned

### 1. Caesar Cipher & ROT13
The **Caesar Cipher** is one of the earliest and simplest forms of encryption. It is a substitution cipher where each letter in the plaintext is shifted a certain number of places down the alphabet.



**ROT13** ("Rotate by 13 places") is a specific variant of the Caesar Cipher. Since the English alphabet has 26 letters, shifting by 13 means that applying ROT13 twice restores the original text ($13 + 13 = 26$). This makes it a symmetric encryption algorithm where the same function is used for both encryption and decryption.

* **Example Structure:**
  * `A` $\rightarrow$ `N`
  * `B` $\rightarrow$ `O`
  * `a` $\rightarrow$ `n`

### 2. The `tr` (Translate) Command
The `tr` command in Linux is a powerful utility used for translating, squeezing, or deleting characters from the standard input (`stdin`) and writing to the standard output (`stdout`). 
- It maps a set of source characters to a set of destination characters position-by-position.

---

## 🛠️ Methodology & Command Breakdown

To decipher the content of `data.txt`, we need to pipe its content into the `tr` command and reverse the 13-character shift for both uppercase and lowercase sets.

### Command Executed:
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
Breakdown of the Pipeline:
`cat data.txt`: Reads the encrypted file and sends the obfuscated text into the pipeline.

`|` (Pipe): Takes the output of the cat command and feeds it as input directly into the tr command.

`tr 'A-Za-z' 'N-ZA-Mn-za-m'` :

`'A-Za-z'`: Specifies the input character range (all uppercase followed by all lowercase letters).

`'N-ZA-Mn-za-m'`: Specifies the output mapping range. It starts shifting uppercase from N to Z, then wraps around from A to M. It does the exact same wrap-around for lowercase (n-z then a-m).

By executing this command, the standard output immediately revealed the plaintext password required to access Bandit Level 12.
## 🧠 Concepts Learned Summary
* **Symmetric Obfuscation (ROT13):** Learned how data can be easily hidden using basic mathematical shifts ($+13$), and why it offers zero cryptographic security nowadays but is great for understanding basic ciphers.
* **Character Mapping (`tr`):** Mastered translating character sets in real-time via the pipeline, expanding control over text manipulation in the Linux CLI.
* **Stream Piping (`|`):** Re-enforced the power of piping encrypted streams directly into translation utilities for instant decryption.
