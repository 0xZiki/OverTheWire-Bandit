## 🧩 The Challenge

The password for the next level is stored in the file `data.txt` in one of the few human-readable strings, preceded by several `=` characters.

---

## 🛠️ Solutions

### Method 1: Isolating Human-Readable Text (Using `strings` & `grep`)

Since running the `file data.txt` command reveals that the file type is `data` (a non-text binary file), opening it directly with `cat` would corrupt and clutter the terminal screen with unprintable characters. 

To solve this, we can use the `strings` command to extract only the human-readable text sequences from the binary file, and then pipe the output into `grep` to filter for the `=` pattern:

```bash
strings data.txt | grep "=="
```
Command Breakdown:

strings data.txt : Scans the binary file and extracts embedded ASCII strings that are human-readable.

| (Pipe) : Forwards those extracted text lines to the next command.

grep "==" : Searches through the strings and isolates the exact line containing the multiple = prefix.

Output Analysis: The terminal cleanly bypasses the binary garbage and returns only the line containing the target password right next to the ========== characters.
🧠 Concepts Learned
Binary vs Text Files: Utilizing the file command to recognize that non-text files (data) require specialized tools instead of raw standard outputs like cat.

The strings Utility: A foundational tool in binary analysis and reverse engineering used to find printable strings in non-text files.

Pattern Matching on Binaries: Composing pipelines that combine extraction (strings) with advanced filtering (grep) to locate hardcoded assets or credentials.

