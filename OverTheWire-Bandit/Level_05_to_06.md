## 🧩 The Challenge

The password for the next level is stored in a file somewhere under the `inhere` directory and has all of the following properties:
- Human-readable
- 1033 bytes in size
- Not executable

---

## 🛠️ Solutions

### Method 1: The Wildcard Approach (Visual Inspection)

Since the `inhere` directory contains multiple subdirectories (`maybehere00` to `maybehere19`), we can use a wildcard `*` to list the contents of all directories at once to manually inspect file sizes and permissions:

```bash
ls -al maybehere*
```
While this works for a small number of directories, it is not practical or scalable for real-world environments with thousands of files.

### Method 2: The Pro Way (Using the find command)
Instead of manually inspecting files, we can use the powerful find command to filter files based on the exact properties required by the challenge:

```Bash
find . -type f -size 1033c ! -executable
```
Command Breakdown:

`.` : Search starting from the current directory.

`-type f` : Look for regular files only (exclude directories).

`-size 1033c` : Look for files that are exactly 1033 bytes (c stands for bytes).

`! -executable` : Use the logical NOT operator (!) to ensure the file does not have execution permissions.

Output Analysis: The command will instantly reveal the exact path to the single file matching all criteria.

Knowing the path, we can print its content directly:

```Bash
cat <path_to_the_found_file>
```
## 🧠 Concepts Learned
The find Command: A highly versatile command-line utility used to search for files in a directory hierarchy based on specific criteria like type, size, and permissions.

Logical NOT (!): How to negate search conditions to exclude specific results (e.g., filtering out executable files).

Exact File Sizing (c flag): Understanding how to search for files by exact byte size.
