OverTheWire Bandit: Level 20 ➡️ Level 21
### 📝 Level Overview
- Objective: Pass the current level's password to a SetUID binary via a local network connection on a specified port to retrieve the next password.

- Current Access: `bandit20`

- Target Access: `bandit21`

- Key Lessons Learned: Network Sockets, Client-Server Communication, Port Listening, Concurrent SSH Sessions.
### 🔍 The Technical Challenge & Reconnaissance
We are given a SetUID binary named `suconnect` owned by `bandit21`. The binary requires a port number as an argument. Its internal logic is designed to:

1. Connect to `localhost` on the given port.

2. Read a line of text (expecting the `bandit20` password).

3. Compare the text. If it matches, it prints the password for `bandit21`.

This requires us to act as the "Server" (Listener) while the binary acts as the "Client".
### 🚀 The "Out of the Box" Solution (Concurrent Sessions)
Instead of dealing with complex background job control (`&`, `fg`, `/dev/tcp/`) or terminal multiplexers which can cause timing and EOF execution issues, the most robust and straightforward approach is to utilize Two Separate SSH Sessions.
### Step 1: Setting up the Listener (Terminal 1)
In the first SSH session, we open a listening port (e.g., 9999) in the foreground using Netcat:

```Bash
nc -lvnp 9999
```
The terminal will now hang and wait for an incoming connection.
### Step 2: Triggering the Binary (Terminal 2)
In a completely new terminal window, we establish a second SSH connection to `bandit20`. We then execute the binary and point it to the port we just opened:

```Bash
./suconnect 9999
```
Step 3: The Handshake & Payload Delivery (Terminal 1)
Immediately after executing the binary in Terminal 2, we switch back to Terminal 1. Netcat indicates a successful connection from localhost.
We paste the `bandit20` password into Terminal 1 and press `Enter`:
```Plaintext
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA```
- Result: The binary validates the string, and the password for bandit21 is printed directly to the Netcat session in Terminal 1, after which the connection cleanly terminates.
🧠 Key Concepts Learned & Real-World Applications
1. The Client-Server Model
This level perfectly simulates a basic client-server handshake. We manually acted as an authentication server using `nc` (Netcat), while the SetUID binary acted as a client validating its credentials against us.
2. Overcoming Terminal Constraints (Bypassing TTY limits)
In real-world penetration testing, exploits often fail due to unstable shells or restrictive TTY environments hindering background processes or payload delivery. Opening parallel authenticated sessions (when possible) to separate the "Listener" from the "Exploit execution" is a standard and highly reliable operational tactic.
