

# ⚡ John the Ripper Lab: Offline Hash Cracking

An educational lab environment demonstrating offline password-cracking concepts, wordlist mutations, and cryptographic hash analysis using John the Ripper.

> **⚠️ Disclaimer:** This repository is strictly for educational and authorized testing purposes. Only run these exercises on your local machine, a dedicated virtual machine, a Docker container, or authorized lab environments (e.g., TryHackMe, Hack The Box). Never handle hashes or data from unauthorized sources.

---

## 🛠️ Environment Setup & Lab Execution

Follow these steps sequentially within your terminal environment to prepare the wordlists, generate a test hash, and perform the cracking exercises.

### Part 1: Dependency & Wordlist Preparation

#### 1. Update Packages & Install John the Ripper

Ensure your local package repository index is fresh and install John the Ripper.

```bash
sudo apt update && sudo apt install -y john

```

#### 2. Prepare the Wordlist Infrastructure

Extract the native Kali Linux password wordlist and generate a shortened 1,000-line subset to ensure rapid execution during this demonstration.

```bash
# Decompress rockyou wordlist if it hasn't been done already
[ -f /usr/share/wordlists/rockyou.txt.gz ] && sudo gunzip /usr/share/wordlists/rockyou.txt.gz

# Create a fast 1,000-item testing subset in your home directory
head -n 1000 /usr/share/wordlists/rockyou.txt > ~/rockyou-1000.txt

# Seed the valid password to guarantee an immediate match for the baseline test
echo "password123" >> ~/rockyou-1000.txt

```

---

### Part 2: Target Generation & Baseline Attack

#### 3. Generate the Target MD5 Hash

Create a standardized, raw MD5 hash of our target string (`password123`) and save it to a local text file.

```bash
echo -n "password123" | md5sum | awk '{print $1}' > ~/john-hashes.txt

```

To visually verify the signature format before processing, inspect the file contents:

```bash
cat ~/john-hashes.txt

```

#### 4. Execute Baseline Wordlist Attack

Instruct John the Ripper to test the entries inside your optimized wordlist directly against the target file.

```bash
john --format=raw-md5 --wordlist=~/rockyou-1000.txt ~/john-hashes.txt

```

#### 5. Review Recovered Credentials

Once processing completes, query John's internal storage database to display the plain-text results associated with the cracked hash.

```bash
john --show --format=raw-md5 ~/john-hashes.txt

```

---

### Part 3: Advanced Optimization (Rule Mutations)

#### 6. Execute Attack with Rule Modifications

In real-world scenarios, targets rarely use exact dictionary words. Use John the Ripper's internal rule engine to dynamically mutate your wordlist entries (e.g., modifying character casing, appending numbers, appending common special characters).

```bash
john --format=raw-md5 --wordlist=~/rockyou-1000.txt --rules ~/john-hashes.txt

```

| Command Flag | Operational Purpose |
| --- | --- |
| `--format=raw-md5` | Explicitly sets the hashing algorithm (avoids computational overhead from auto-detection). |
| `--wordlist=[path]` | Points the tool to the specific text file dictionary containing base password candidates. |
| `--rules` | Enables the mutation engine to alter base words on-the-fly based on common human patterns. |
| `--show` | Reads the session ledger to display successfully decrypted pairs without re-running the attack. |

---

Made with ❤️ by **Shafqat Ghafoor** — Happy Hacking!
