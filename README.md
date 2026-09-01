# OverTheWire Bandit 0–23

> 🏴 Beginner-friendly walkthrough of **OverTheWire Bandit Levels 0–23**
> Topics: Linux • Bash • SSH • Files • Permissions • Encoding • Compression • Networking • Cron • Shell Scripting

---

## 📖 About Bandit

**OverTheWire** provides security wargames for learning cybersecurity through practical challenges.

**Bandit** is designed for beginners and teaches the Linux/Unix basics needed for other security wargames.

Each level gives you a challenge. Solving it gives you the password for the **next level**.

> 🔑 **Important:** `Bandit X → X+1` means the password obtained while solving Level X is used to log in to **Bandit X+1**.

---

## ⚙️ Prerequisites

Recommended:

* 🐧 Linux / Kali Linux / WSL
* 💻 Terminal
* 🔐 Basic SSH knowledge
* 🌐 Internet connection
* 🧠 Basic file and directory concepts

---

## 🚀 Connection

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

### Meaning

| Part                          | Meaning                 |
| ----------------------------- | ----------------------- |
| `ssh`                         | Secure Shell client     |
| `bandit0`                     | Username                |
| `@`                           | Separates user and host |
| `bandit.labs.overthewire.org` | Server                  |
| `-p 2220`                     | SSH port                |

After every level, use its password to log into the next account.

---

# 🏴 Bandit Level 0 → Level 1

## 🎯 Aim

Learn how to connect to Bandit using SSH and read a file.

## 🧩 Problem

The password is stored in a file named `readme` in the home directory.

## 💻 Commands Used

### `ssh`

```bash
ssh [user]@[host] -p [port]
```

> Connects to a remote computer securely.

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

### `cat`

```bash
cat [file]
```

> Displays the contents of a file.

```bash
cat readme
```

### 🔁 Alternatives

```bash
less readme
```

> Opens the file using a pager.

```bash
head readme
```

> Displays the beginning of the file.

## 🔑 Password for Next Bandit

```text
6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
```

---

# 🏴 Bandit Level 1 → Level 2

## 🎯 Aim

Learn how to handle a filename beginning with `-`.

## 🧩 Problem

The password is stored in a file named exactly:

```text
-
```

A filename beginning with `-` can be interpreted as a command option.

## 💻 Commands Used

### `cat ./-`

```bash
cat ./-
```

> `./` clearly tells Linux that `-` is a filename in the current directory.

### 🔁 Alternative

```bash
cat -- -
```

> `--` tells the command that options have ended.

## 🔑 Password for Next Bandit

```text
PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
```

---

# 🏴 Bandit Level 2 → Level 3

## 🎯 Aim

Learn how to work with filenames containing spaces.

## 🧩 Problem

The password is stored in:

```text
--spaces in this filename--
```

## 💻 Commands Used

### Quoting

```bash
cat "filename with spaces"
```

> Quotes make Bash treat the complete text as one filename.

```bash
cat -- "--spaces in this filename--"
```

### 🔁 Alternative

Escape spaces:

```bash
cat -- --spaces\ in\ this\ filename--
```

> `\` escapes the spaces.

## 🔑 Password for Next Bandit

```text
7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
```

---

# 🏴 Bandit Level 3 → Level 4

## 🎯 Aim

Learn how to find hidden files.

## 🧩 Problem

The password is stored in a hidden file inside `inhere`.

## 💻 Commands Used

### `cd`

```bash
cd [directory]
```

> Changes the current directory.

```bash
cd inhere
```

### `ls -la`

```bash
ls -la
```

* `-l` → detailed listing
* `-a` → include hidden files

Hidden Linux files usually start with `.`.

```bash
cat .hidden
```

### 🔁 Alternative

```bash
find . -maxdepth 1 -type f -name ".*"
```

> Finds hidden regular files in the current directory.

## 🔑 Password for Next Bandit

```text
xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
```

---

# 🏴 Bandit Level 4 → Level 5

## 🎯 Aim

Learn to identify file types.

## 🧩 Problem

Many files are inside `inhere`, but only one is human-readable.

## 💻 Commands Used

### `file`

```bash
file [file]
```

> Detects the type of a file.

```bash
cd inhere
file ./*
```

Find the file reported as human-readable text, then:

```bash
cat ./-file07
```

### 🔁 Alternative

```bash
find . -type f -exec file {} \;
```

> Runs `file` on every file found.

## 🔑 Password for Next Bandit

```text
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```

---

# 🏴 Bandit Level 5 → Level 6

## 🎯 Aim

Learn to search files using size and permission properties.

## 🧩 Problem

Find a file under `inhere` that is:

* Human-readable
* Exactly `1033` bytes
* Not executable

## 💻 Commands Used

### `find`

```bash
find [path] [conditions]
```

> Searches files and directories recursively.

Useful options:

```text
-type f       → regular file
-size 1033c   → exactly 1033 bytes
! -executable → not executable
```

```bash
find inhere -type f -size 1033c ! -executable
```

Then identify/read the matching file:

```bash
cat ./maybehere07/.file2
```

### 🔁 Alternative

```bash
du -ab inhere | grep 1033
```

> Searches displayed file sizes for `1033`.

## 🔑 Password for Next Bandit

```text
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
```

---

# 🏴 Bandit Level 6 → Level 7

## 🎯 Aim

Learn to search the entire filesystem using ownership and file size.

## 🧩 Problem

Find a file somewhere on the system that:

* Belongs to user `bandit7`
* Belongs to group `bandit6`
* Is exactly `33` bytes

## 💻 Commands Used

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Important options

| Part             | Meaning                     |
| ---------------- | --------------------------- |
| `/`              | Search from filesystem root |
| `-type f`        | Regular files               |
| `-user bandit7`  | Owned by `bandit7`          |
| `-group bandit6` | Group is `bandit6`          |
| `-size 33c`      | Exactly 33 bytes            |
| `2>/dev/null`    | Hide error messages         |

Read the matching file.

## 🔁 Alternative

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

> `-type f` is omitted, but the search is less precise.

## 🔑 Password for Next Bandit

```text
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```

---

# 🏴 Bandit Level 7 → Level 8

## 🎯 Aim

Learn basic text searching with `grep`.

## 🧩 Problem

The password is on the same line as the word `millionth` in `data.txt`.

## 💻 Commands Used

### `grep`

```bash
grep [pattern] [file]
```

> Searches a file for matching text.

```bash
grep millionth data.txt
```

### 🔁 Alternative

```bash
cat data.txt | grep millionth
```

> Works, but the direct `grep` command is simpler.

## 🔑 Password for Next Bandit

```text
VR1ljMayciFxbnUokuQmJFw6QC9VKtub
```

---

# 🏴 Bandit Level 8 → Level 9

## 🎯 Aim

Learn `sort`, `uniq`, and pipes.

## 🧩 Problem

The password is the only line that occurs exactly once in `data.txt`.

## 💻 Commands Used

### `sort`

```bash
sort [file]
```

> Sorts lines.

### `uniq -u`

```bash
uniq -u
```

> Displays only unique lines.

### Pipe `|`

```bash
command1 | command2
```

> Sends the output of the first command into the second command.

### Solution

```bash
sort data.txt | uniq -u
```

> `sort` is needed because `uniq` compares adjacent lines.

### 🔑 Password for Next Bandit

```text
EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
```

---

# 🏴 Bandit Level 9 → Level 10

## 🎯 Aim

Learn to extract readable text from binary data.

## 🧩 Problem

`data.txt` contains binary data. The password is in one of the readable strings preceded by several `=` characters.

## 💻 Commands Used

### `strings`

```bash
strings [file]
```

> Extracts human-readable text from binary files.

```bash
strings data.txt
```

### Combine with `grep`

```bash
strings data.txt | grep "="
```

> Shows strings containing `=`.

## 🔑 Password for Next Bandit

```text
B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
```

---

# 🏴 Bandit Level 10 → Level 11

## 🎯 Aim

Learn Base64 decoding.

## 🧩 Problem

`data.txt` contains Base64-encoded data.

## 💻 Commands Used

### `base64`

```bash
base64 [options] [file]
```

> Encodes or decodes Base64 data.

### `-d`

```bash
base64 -d [file]
```

> Decodes Base64.

### Solution

```bash
base64 -d data.txt
```

Alternative:

```bash
cat data.txt | base64 -d
```

## 🔑 Password for Next Bandit

```text
pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
```

---

# 🏴 Bandit Level 11 → Level 12

## 🎯 Aim

Learn the ROT13 substitution cipher.

## 🧩 Problem

Each alphabetic character has been rotated by 13 positions.

Example:

```text
A → N
B → O
N → A
```

## 💻 Commands Used

### `tr`

```bash
tr SET1 SET2
```

> Replaces characters from one set with characters from another set.

### Solution

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

This converts the ROT13 text back to normal text.

### 🔑 Password for Next Bandit

```text
GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```

---

# 🏴 Bandit Level 12 → Level 13

## 🎯 Aim

Learn hexadecimal dumps and repeated compression/decompression.

## 🧩 Problem

`data.txt` is a hexadecimal dump of a file that has been compressed multiple times.

## 💻 Commands Used

### `mktemp`

```bash
mktemp -d
```

> Creates a unique temporary directory.

### `cp`

```bash
cp [source] [destination]
```

> Copies files.

### `mv`

```bash
mv [source] [destination]
```

> Renames or moves files.

### `xxd -r`

```bash
xxd -r [file]
```

> Converts a hexadecimal dump back into binary data.

### Start

```bash
TMP=$(mktemp -d)
cd "$TMP"
cp ~/data.txt .
xxd -r data.txt > data
file data
```

Now repeatedly inspect the file:

```bash
file data
```

Then use the matching tool:

```bash
gunzip data
```

or:

```bash
bunzip2 data
```

or:

```bash
tar -xf data
```

Rename files when necessary:

```bash
mv data data.gz
gunzip data.gz
```

Continue:

```text
file → identify format → extract/decompress → file again
```

until you reach readable text.

### 🔁 Alternative

Use `file` after every step instead of trying to guess the next compression format.

> 🧠 **Important:** The filename does not tell you the real file type. `file` tells you what it actually is.

## 🔑 Password for Next Bandit

```text
qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```

---

# 🏴 Bandit Level 13 → Level 14

## 🎯 Aim

Learn SSH private-key authentication.

## 🧩 Problem

Instead of a password, you are given an SSH private key.

## 💻 Commands Used

### `ssh -i`

```bash
ssh -i [private_key] [user]@[host] -p [port]
```

> Uses a private key for SSH authentication.

### `chmod`

```bash
chmod [permissions] [file]
```

> Changes file permissions.

### Solution

The key is stored in:

```bash
cat sshkey.private
```

Save/copy it to a local file, then:

```bash
chmod 600 sshkey.private
```

Connect:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

Then:

```bash
cat /etc/bandit_pass/bandit14
```

### Why `chmod 600`?

```text
6 → read + write for owner
0 → no permissions for group
0 → no permissions for others
```

## 🔑 Password for Next Bandit

```text
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

---

# 🏴 Bandit Level 14 → Level 15

## 🎯 Aim

Learn basic TCP communication and ports.

## 🧩 Problem

Send the current password to port `30000` on `localhost`.

## 💻 Commands Used

### `nc`

```bash
nc [host] [port]
```

> Netcat creates network connections.

### Solution

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

Or:

```bash
echo "aaWecNkG4FhxJQxz07uiwzVP6bJiYS65" | nc localhost 30000
```

### `localhost`

> Means the same computer you are currently using.

### 🔑 Password for Next Bandit

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

---

# 🏴 Bandit Level 15 → Level 16

## 🎯 Aim

Learn SSL/TLS networking.

## 🧩 Problem

Send the current password to port `30001`, but this service requires SSL/TLS.

## 💻 Commands Used

### `openssl s_client`

```bash
openssl s_client -connect [host]:[port]
```

> Creates an SSL/TLS client connection.

### Solution

```bash
openssl s_client -connect localhost:30001
```

Then enter:

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

> Messages such as `DONE` or `RENEGOTIATING` can appear during the TLS session.

## 🔑 Password for Next Bandit

```text
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

---

# 🏴 Bandit Level 16 → Level 17

## 🎯 Aim

Learn port scanning, service discovery and SSH keys.

## 🧩 Problem

Find the SSL service listening on a port between `31000` and `32000`.

## 💻 Commands Used

### `nmap`

```bash
nmap [options] [target]
```

> Scans a host to discover open ports/services.

### `-p`

```bash
-p 31000-32000
```

> Scans only the specified port range.

### Solution

```bash
nmap -sV -p 31000-32000 localhost
```

Find the SSL-enabled service. In the current chain it is:

```text
31790
```

Connect:

```bash
openssl s_client -connect localhost:31790 -quiet
```

Enter:

```text
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

The service returns an SSH private key.

Save it, for example:

```bash
nano /tmp/bandit17.key
```

Then:

```bash
chmod 600 /tmp/bandit17.key
```

Connect:

```bash
ssh -i /tmp/bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

### 🔑 Password for Next Bandit

```text
OQxXZjELndr90zuhOTDYBEomI0SZITXI
```

---

# 🏴 Bandit Level 17 → Level 18

## 🎯 Aim

Learn how to compare files.

## 🧩 Problem

There are two files:

```text
passwords.old
passwords.new
```

Only one line was changed. The changed line in `passwords.new` is the next password.

## 💻 Commands Used

### `diff`

```bash
diff [file1] [file2]
```

> Shows differences between files.

### Solution

```bash
diff passwords.old passwords.new
```

Read the changed line from `passwords.new`.

### 🔁 Alternative

```bash
grep -Fxv -f passwords.old passwords.new
```

> Shows lines present in `passwords.new` but not in `passwords.old`.

## 🔑 Password for Next Bandit

```text
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
```

---

# 🏴 Bandit Level 18 → Level 19

## 🎯 Aim

Learn how to execute a remote SSH command without starting an interactive shell.

## 🧩 Problem

The `.bashrc` file logs you out immediately when you try to log in normally.

The password is stored in `readme`.

## 💻 Commands Used

### SSH remote command

```bash
ssh [user]@[host] -p [port] "[command]"
```

> Executes a command on the remote machine.

### Solution

From your local terminal:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

The command runs before the automatic logout.

### 🔑 Password for Next Bandit

```text
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```

---

# 🏴 Bandit Level 19 → Level 20

## 🎯 Aim

Learn about **SUID / setuid** programs.

## 🧩 Problem

A special program in the home directory can execute commands with another user's privileges.

## 💻 Commands Used

### Run a local program

```bash
./program
```

> `./` means "run the program from the current directory."

### Solution

First see how it works:

```bash
./bandit20-do
```

Then:

```bash
./bandit20-do whoami
```

It should run as `bandit20`.

Therefore:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

### 🧠 SUID

A SUID executable can run with the permissions of its owner.

> ⚠️ SUID programs can become security risks when they allow unintended privileged actions.

## 🔑 Password for Next Bandit

```text
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```

---

# 🏴 Bandit Level 20 → Level 21

## 🎯 Aim

Learn client/server communication using Netcat.

## 🧩 Problem

`./suconnect` connects to a port, reads the password for Bandit 20, and returns the password for Bandit 21 when the password is correct.

## 💻 Commands Used

### Terminal 1

Start a listener:

```bash
nc -l -p 4444
```

### Terminal 2

Run:

```bash
./suconnect 4444
```

Now return to Terminal 1 and send:

```text
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```

The program sends back the next password.

### 🔁 Alternative

On systems where supported:

```bash
echo "bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY" | nc -l 4444
```

Then in another terminal:

```bash
./suconnect 4444
```

> 🧠 The exact `nc -l` syntax can vary slightly between Netcat versions.

## 🔑 Password for Next Bandit

```text
RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz
```

---

# 🏴 Bandit Level 21 → Level 22

## 🎯 Aim

Learn how cron jobs automatically execute programs.

## 🧩 Problem

A cron job runs a script regularly. Find the script and see what it does.

## 💻 Commands Used

### `cat`

Inspect the cron configuration:

```bash
cat /etc/cron.d/cronjob_bandit22
```

Inspect the script:

```bash
cat /usr/bin/cronjob_bandit22.sh
```

The script creates a file in `/tmp`.

Read that file:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

### 🧠 Cron

**cron** is a Linux service that automatically runs commands at scheduled times.

`/etc/cron.d/` contains cron job configurations.

## 🔑 Password for Next Bandit

```text
gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw
```

---

# 🏴 Bandit Level 22 → Level 23

## 🎯 Aim

Learn to read and reproduce a shell script's logic.

## 🧩 Problem

The cron job calculates a filename using an MD5 hash, then writes the password into that file.

## 💻 Commands Used

Inspect the cron configuration:

```bash
cat /etc/cron.d/cronjob_bandit23
```

Inspect the script:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

The important input used by the script is:

```text
I am user bandit23
```

### `md5sum`

```bash
md5sum [input]
```

> Calculates an MD5 hash.

### `cut`

```bash
cut -d ' ' -f 1
```

* `-d ' '` → use a space as delimiter
* `-f 1` → select the first field

### Reproduce the filename

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Result:

```text
8ca319486bfbbc3663ea0fbe81326349
```

Read the generated file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

### 🧠 Key idea

Do not just run commands blindly. Read the script and reproduce its logic.

## 🔑 Password for Next Bandit

```text
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```

---

# 🏴 Bandit Level 23 → Level 24

## 🎯 Aim

Write your first shell script and understand how cron can execute it with another user's privileges.

## 🧩 Problem

A cron job executes scripts from a specific directory as `bandit24`.

Your job is to create a script that reads the Bandit 24 password and saves it somewhere you can read.

## 💻 Commands Used

### `mkdir`

```bash
mkdir [directory]
```

> Creates a directory.

Create a workspace:

```bash
mkdir /tmp/bandit23
cd /tmp/bandit23
```

### Create the script

```bash
nano exploit.sh
```

Put this inside:

```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/bandit24_pass
chmod 644 /tmp/bandit24_pass
```

### `chmod +x`

```bash
chmod +x exploit.sh
```

> Adds execute permission.

### Copy the script

```bash
cp exploit.sh /var/spool/bandit24/foo/
```

> The cron job executes scripts placed in this directory.

Wait for cron to execute the script, then:

```bash
cat /tmp/bandit24_pass
```

### Inspect the cron job

```bash
cat /etc/cron.d/cronjob_bandit24
```

Inspect its script:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

### 🧠 Important concepts

#### Shebang

```bash
#!/bin/bash
```

> Tells Linux to execute the script using Bash.

#### Output redirection

```bash
command > file
```

> Saves command output into a file.

#### `chmod 644`

```text
6 → owner: read + write
4 → group: read
4 → others: read
```

This allows the `bandit23` account to read the generated password file.

> ⚠️ The cron system removes submitted scripts after execution, so keep your original copy.

## 🔑 Password for Next Bandit

```text
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```
