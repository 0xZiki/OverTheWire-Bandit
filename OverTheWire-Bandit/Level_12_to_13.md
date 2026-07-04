"""# Bandit Level 12 → Level 13 Base Documentation

## 📝 Level Description
The password for the next level is stored in the file `data.txt`, which is a hex dump of a file that has been repeatedly compressed using multiple formats (`gzip`, `bzip2`, and `tar`). To solve this level, we must establish a volatile sandbox directory, reconstruct the binary file from its hexadecimal representation, and systematically decompress every layer by analyzing structural file headers.

---

## 🔍 Commands & Parameter Breakdown

To master this level, understanding the exact flags and switches used with each command is crucial. Below is a detailed technical breakdown of the parameters employed during the extraction process:

### 1. `xxd` (Hex Dump Utility)
* **Command Used:** `xxd -r data.txt > datafile`
* **Flags Explained:**
  * **`-r` (Reverse):** By default, `xxd` takes a binary file and outputs a hex dump. The `-r` switch tells the utility to do the exact opposite: read the plain text hex dump from `data.txt` and reconstruct it back into its original binary format.

### 2. `tar` (Tape Archive Utility)
* **Command Used:** `tar -xf datafile.tar`
* **Flags Explained:**
  * **`-x` (Extract):** Instructs `tar` to extract the contents out of an archive file.
  * **`-f` (File):** Specifies that the very next argument is the name of the archive file to operate on. Without `-f`, `tar` expects input from standard devices or magnetic tapes, which would cause an error in a standard CLI environment.

### 3. `mv` (Move / Rename Utility)
* **Command Used:** `mv datafile datafile.gz`
* **Context:** Linux compression tools like `gunzip` and `bunzip2` are strict regarding file extensions. The `mv` command is used here purely to append the required extension (`.gz` or `.bz2`) to force the decompression utilities to accept the file.

---

## 🛠️ Multi-Layer Methodology (Step-by-Step)

### Step 1: Creating the Sandbox
We isolate our workspace within the volatile `/tmp` directory to avoid access violations:
```bash
mkdir /tmp/ziki_challenge12
cd /tmp/ziki_challenge12
cp ~/data.txt .
```
### Step 2: Reversing the Hex Dump
Reconstruct the raw compressed payload using the reverse hex flag:

```Bash
xxd -r data.txt > datafile
```
### Step 3: Peeling the Decompression Layers
Every extraction layer is audited using the file command to ensure the correct extraction tool and flags are chosen:
Layer #,Current File,Detected Type (via file),Command Executed,Parameters Used,Resulting File
Layer 1,datafile,gzip compressed data,mv datafile datafile.gz && gunzip datafile.gz,gunzip (No flags required once .gz is appended),datafile
Layer 2,datafile,POSIX tar archive,mv datafile datafile.tar && tar -xf datafile.tar,"-x (Extract), -f (File target)",data5.bin
Layer 3,data5.bin,POSIX tar archive (GNU),tar -xf data5.bin,"-x (Extract), -f (File target)",data6.bin
Layer 4,data6.bin,bzip2 compressed data,mv data6.bin data6.bz2 && bunzip2 data6.bz2,bunzip2 (No flags required once .bz2 is appended),data6
Layer 5,data6,POSIX tar archive,tar -xf data6,"-x (Extract), -f (File target)",data8.bin
Layer 6,data8.bin,gzip compressed data,mv data8.bin data8.gz && gunzip data8.gz,gunzip (No flags required once .gz is appended),data8

###Step 4: Final Flag Extraction
After stripping the final gzip wrapper, auditing the text stream reveals:

```Bash
file data8
# Output: data8: ASCII text
cat data8
```
### 🧠 Concepts Learned Summary
Flag Mechanics: Mastered the structural utility of the -xf parameters in tar archiving and the -r reconstruction capability of xxd.

File Integrity vs Extensions: Reinforced the reality that file signatures (headers) dictate true identity, whereas file extensions in Linux are merely administrative or required by specific tool syntaxes.

Encapsulation Architecture: Understood how data payloads can be nested through successive algorithm transformations, providing foundational knowledge for binary analysis.
