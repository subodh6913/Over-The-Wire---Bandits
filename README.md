# OverTheWire Bandit 0–23

> 🏴 **Beginner-friendly Linux & Cybersecurity Practice**

## 📖 About Bandit

**OverTheWire Bandit** is a beginner-friendly Linux security wargame.

Each level gives you a challenge.
The password you find is used to enter the **next level**.

---

## ⚙️ Prerequisites

* 🐧 Linux / Kali / WSL
* 💻 Terminal
* 🔐 Basic SSH
* 🌐 Internet

## 🚀 Connect

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

> `ssh` = remote login, `-p 2220` = SSH port.

---

# 🏴 Bandit 0 → 1

### 🎯 Aim

Learn SSH and reading files.

### 🧩 Problem

Password is inside `readme`.

### 💻 Commands

```bash
cat readme
```

> `cat` displays file contents.

### 🔑 Password

```text
6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
```

---

# 🏴 Bandit 1 → 2

### 🎯 Aim

Handle a filename beginning with `-`.

### 🧩 Problem

Password is in a file named `-`.

### 💻 Commands

```bash
cat ./-
```

> `./` tells Linux that `-` is a filename.

### 🔁 Alternative

```bash
cat -- -
```

### 🔑 Password

```text
PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
```

---

# 🏴 Bandit 2 → 3

### 🎯 Aim

Handle spaces in filenames.

### 🧩 Problem

Password is in a file containing spaces.

### 💻 Commands

```bash
cat -- "--spaces in this filename--"
```

> Quotes keep the filename together.

### 🔑 Password

```text
7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
```

---

# 🏴 Bandit 3 → 4

### 🎯 Aim

Find hidden files.

### 🧩 Problem

Password is in a hidden file inside `inhere`.

### 💻 Commands

```bash
ls -la inhere
cat inhere/.hidden
```

> `-a` shows hidden files. Linux hidden files usually start with `.`.

### 🔑 Password

```text
xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
```

---

# 🏴 Bandit 4 → 5

### 🎯 Aim

Identify file types.

### 🧩 Problem

One file in `inhere` contains readable text.

### 💻 Commands

```bash
file inhere/*
cat ./inhere/-file07
```

> `file` identifies the type of each file.

### 🔑 Password

```text
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```

---

# 🏴 Bandit 5 → 6

### 🎯 Aim

Search files using conditions.

### 🧩 Problem

Find a file that is readable, not executable, and exactly **1033 bytes**.

### 💻 Commands

```bash
find inhere -type f -size 1033c ! -executable -readable
cat inhere/maybehere07/.file2
```

> `find` searches files.
> `-type f` = file, `-size 1033c` = 1033 bytes.

### 🔑 Password

```text
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
```

---

# 🏴 Bandit 6 → 7

### 🎯 Aim

Search the whole filesystem.

### 🧩 Problem

Find a 33-byte file owned by `bandit7` and group `bandit6`.

### 💻 Commands

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

> `2>/dev/null` hides permission errors.

### 🔑 Password

```text
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```

---

# 🏴 Bandit 7 → 8

### 🎯 Aim

Search text with `grep`.

### 🧩 Problem

Password is next to `millionth` in `data.txt`.

### 💻 Commands

```bash
grep millionth data.txt
```

> `grep` searches for text.

### 🔑 Password

```text
VR1ljMayciFxbnUokuQmJFw6QC9VKtub
```

---

# 🏴 Bandit 8 → 9

### 🎯 Aim

Find a unique line.

### 🧩 Problem

Only one line appears once.

### 💻 Commands

```bash
sort data.txt | uniq -u
```

> `sort` groups identical lines.
> `uniq -u` shows lines occurring once.
> `|` sends output to the next command.

### 🔑 Password

```text
EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
```

---

# 🏴 Bandit 9 → 10

### 🎯 Aim

Extract readable text from binary data.

### 🧩 Problem

Password is hidden among readable strings.

### 💻 Commands

```bash
strings data.txt | grep "=="
```

> `strings` extracts readable text.

### 🔑 Password

```text
B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
```

---

# 🏴 Bandit 10 → 11

### 🎯 Aim

Decode Base64.

### 🧩 Problem

`data.txt` contains Base64-encoded text.

### 💻 Commands

```bash
base64 -d data.txt
```

> `-d` = decode.

### 🔑 Password

```text
pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
```

---

# 🏴 Bandit 11 → 12

### 🎯 Aim

Decode ROT13.

### 🧩 Problem

Letters were shifted by 13 positions.

### 💻 Commands

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

> `tr` replaces characters.

### 🔑 Password

```text
GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```

---

# 🏴 Bandit 12 → 13

### 🎯 Aim

Work with hexdumps and compressed files.

### 🧩 Problem

`data.txt` is a hex dump of repeatedly compressed data.

### 💻 Commands

```bash
mkdir /tmp/bandit12
cp data.txt /tmp/bandit12
cd /tmp/bandit12
xxd -r data.txt data
file data
```

Then repeatedly decompress according to `file`:

```bash
gunzip data
bunzip2 data
tar -xf data
```

> Use `file` after every step to know what to do next.

### 🔑 Password

```text
qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```

---

# 🏴 Bandit 13 → 14

### 🎯 Aim

Use an SSH private key.

### 🧩 Problem

A private key is provided instead of a password.

### 💻 Commands

```bash
chmod 600 sshkey.private
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
cat /etc/bandit_pass/bandit14
```

> `chmod 600` protects the private key.
> `-i` tells SSH which key to use.

### 🔑 Password

```text
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

---

# 🏴 Bandit 14 → 15

### 🎯 Aim

Learn basic networking.

### 🧩 Problem

Send the current password to port `30000`.

### 💻 Commands

```bash
echo "aaWecNkG4FhxJQxz07uiwzVP6bJiYS65" | nc localhost 30000
```

> `nc` = Netcat, used for network connections.

### 🔑 Password

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

---

# 🏴 Bandit 15 → 16

### 🎯 Aim

Learn SSL/TLS connections.

### 🧩 Problem

Send the password to port `30001` using SSL.

### 💻 Commands

```bash
openssl s_client -connect localhost:30001
```

Then enter:

```text
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

> `openssl s_client` creates an SSL/TLS connection.

### 🔑 Password

```text
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

---

# 🏴 Bandit 16 → 17

### 🎯 Aim

Learn port scanning and SSH keys.

### 🧩 Problem

Find the SSL service between ports `31000–32000`.

### 💻 Commands

```bash
nmap -sV -p 31000-32000 localhost
```

Find the SSL port, then:

```bash
openssl s_client -connect localhost:31790 -quiet
```

Enter:

```text
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

Save the returned private key:

```bash
chmod 600 key
ssh -i key bandit17@bandit.labs.overthewire.org -p 2220
```

### 🔑 Password

```text
OQxXZjELndr90zuhOTDYBEomI0SZITXI
```

---

# 🏴 Bandit 17 → 18

### 🎯 Aim

Compare files.

### 🧩 Problem

Only one line changed between two files.

### 💻 Commands

```bash
diff passwords.old passwords.new
```

> `diff` shows differences between files.

### 🔑 Password

```text
KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI
```

---

# 🏴 Bandit 18 → 19

### 🎯 Aim

Execute a remote command through SSH.

### 🧩 Problem

Normal login immediately logs you out.

### 💻 Commands

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

> The command runs remotely without opening a normal shell.

### 🔑 Password

```text
4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA
```

---

# 🏴 Bandit 19 → 20

### 🎯 Aim

Understand SUID programs.

### 🧩 Problem

A special program can execute commands as `bandit20`.

### 💻 Commands

```bash
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```

> `./` runs a program from the current directory.
> SUID allows the program to run with its owner's privileges.

### 🔑 Password

```text
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```

---

# 🏴 Bandit 20 → 21

### 🎯 Aim

Learn client/server communication.

### 🧩 Problem

`suconnect` connects to a port and checks the current password.

### 💻 Commands

**Terminal 1:**

```bash
nc -l 4444
```

**Terminal 2:**

```bash
./suconnect 4444
```

Enter in Terminal 1:

```text
bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY
```

### 🔑 Password

```text
RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz
```

---

# 🏴 Bandit 21 → 22

### 🎯 Aim

Understand cron jobs.

### 🧩 Problem

A cron job regularly runs a script that creates the password file.

### 💻 Commands

```bash
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

> `cron` runs commands automatically on a schedule.

### 🔑 Password

```text
gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw
```

---

# 🏴 Bandit 22 → 23

### 🎯 Aim

Understand scripts and MD5 hashing.

### 🧩 Problem

The cron script creates a filename using an MD5 hash.

### 💻 Commands

```bash
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
```

Reproduce the hash:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Then:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

> `md5sum` creates an MD5 hash.
> `cut` extracts the needed part.

### 🔑 Password

```text
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```

---

# 🏴 Bandit 23 → 24

### 🎯 Aim

Learn Bash scripting and cron-based privilege execution.

### 🧩 Problem

Cron runs files from `/var/spool/bandit24/foo/` as `bandit24`.

### 💻 Commands

Create a script:

```bash
nano solve.sh
```

Put:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/pass24
chmod 644 /tmp/pass24
```

Make it executable:

```bash
chmod +x solve.sh
```

Copy it:

```bash
cp solve.sh /var/spool/bandit24/foo/
```

After cron runs:

```bash
cat /tmp/pass24
```

> The script runs as `bandit24`, so it can read the Bandit 24 password.

### 🔑 Password

```text
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```
