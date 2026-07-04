## 🧩 The Challenge

The password for the next level is stored in the file `data.txt` and is the only line of text that occurs only once.

---

## 🛠️ Solutions

### Method 1: The Standard Pipeline (Sorting and Filtering)

Since `data.txt` contains a massive number of repeated lines, we need to sort the file content first so that duplicate lines are grouped together. Once sorted, we can pass the output to the `uniq` command to filter out the noise:

```bash
sort data.txt | uniq -u
```
Command Breakdown:

sort data.txt : Arranges all lines alphabetically, placing identical lines right next to each other.

| (Pipe) : Forwards the sorted text stream into the next command as input.

-u (Unique Flag) : Directs uniq to only print lines that are completely unique (appear exactly once) and discard everything else.

⚠️ Critical Analysis: The Importance of the -u Flag
During testing, omitting the -u flag (sort data.txt | uniq) changes the behavior drastically:

Without -u: The uniq command acts as a traditional de-duplication tool. It condenses all repeated adjacent lines into a single instance, printing every distinct line found in the file. This leaves you with dozens of unique-looking lines, making it impossible to identify which line was originally the single password.

With -u: It switches uniq into an exclusion filter. It completely suppresses any line that had duplicates, leaving only the single, non-repeated string on the terminal screen.

🧠 Concepts Learned
Text Sorting (sort): Understanding that uniq relies strictly on adjacent line comparisons, making a prior sort mandatory for system-wide files.

Advanced Uniq Filtering (uniq -u): Isolating unique occurrences rather than just collapsing duplicate clusters.

Behavioral Flags: Recognizing how a single flag can pivot an administrative tool from a simple cleanup tool to a data filtering mechanism.
