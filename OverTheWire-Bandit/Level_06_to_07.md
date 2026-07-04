## 🧩 The Challenge

The password for the next level is stored somewhere on the server and has all of the following properties:
- Owned by user `bandit7`
- Owned by group `bandit6`
- 33 bytes in size

---

## 🛠️ Solutions

### Method 1: System-Wide Search (Handling Permission Clutter)

Since the file is located "somewhere on the server", we must initiate the search from the root directory (`/`). A naive search command would look like this:

```bash
find / -user bandit7 -group bandit6 -size 33c
```
The Issue: Running this command as a regular user floods the terminal screen with hundreds of Permission denied errors from system directories, making it extremely difficult to spot the actual file path.
### Method 2: The Pro Way (Error Redirection to /dev/null)
To isolate the correct file path and suppress the error noise, we redirect the Standard Error (stderr represented by file descriptor 2) to the Linux "black hole" (/dev/null):

```Bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
Command Breakdown:

`/` : Starts the search from the absolute root of the system.

`-user bandit7` : Filters for files owned by the user bandit7.

`-group bandit6` : Filters for files belonging to the group bandit6.

`-size 33c` : Matches files that are exactly 33 bytes (c stands for bytes).

`2>/dev/null` : Redirects descriptor 2 (stderr) to /dev/null, completely silencing all permission errors.

Output Analysis: The command cleanly filters out the noise and instantly outputs the single, exact path to the target file.

Knowing the path, we can print its content directly:

```Bash
cat <path_to_the_found_file>
```
🧠 Concepts Learned
System-Wide Searching: Searching across the entire OS file system starting from the root directory (/).

Ownership Filtering: Utilizing -user and -group flags to narrow down files based on Linux permission owners.

I/O Redirection (2>/dev/null): Understanding Linux file descriptors (0: stdin, 1: stdout, 2: stderr) and how to discard unwanted error streams to keep the CLI clean.
