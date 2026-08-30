# 🚩 OverTheWire: Bandit Wargame Solutions

Documentation and command cheat-sheet for the OverTheWire Bandit wargame.

---

## 🛠️ Levels 0–10 Notes

* **Levels 0–3 (Navigation & Special Files):** Handled spaces and hyphens using `cat ./-file-` and `cat "file name"`. Located hidden dotfiles with `ls -la`.
* **Levels 4–6 (Searching & Permissions):** Filtered files by size, user, and non-executable properties while suppressing error noise using `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null` and extracted strings with `strings data.txt`.
* **Levels 7–10 (Text Processing & CLI Pipelines):** Extracted patterns using `grep "millionth" data.txt`, isolated non-repeated entries using `sort data.txt | uniq -u`, and decoded strings with `base64 -d data.txt`.

---

## 🛠️ Levels 11–20 Notes & Cheat-Sheet

* **Level 11 → 12 (ROT13 Cipher):**  
  Decoded ROT13 encoded text using character translation:  
  `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

* **Level 12 → 13 (Hexdump & Multi-Layer Decompression):**  
  Reverted hexdump back to binary with `xxd -r` and decompressed nested `gzip`, `bzip2`, and `tar` archives recursively using `file` to inspect archive types:  
  `xxd -r data.txt > data`  
  `file data`  
  `gzip -d file.gz` / `bzip2 -d file.bz2` / `tar -xf file.tar`

* **Level 13 → 14 (SSH Key Authentication):**  
  Authenticated to the next level host directly using an RSA private key:  
  `ssh -i sshkey.private bandit14@localhost -p 2220`

* **Level 14 → 15 (Netcat Port Transmission):**  
  Transmitted current level credentials to local port 30002 to retrieve the next password:  
  `cat /etc/bandit_pass/bandit14 | nc localhost 30002`

* **Level 15 → 16 (SSL/TLS Encrypted Connection):**  
  Established an encrypted SSL connection to port 30001 using OpenSSL:  
  `openssl s_client -quiet -connect localhost:30001`

* **Level 16 → 17 (Port Scanning & SSL Key Retrieval):**  
  Scanned open ports using Nmap and submitted credentials over SSL to retrieve the private SSH key.

* **Level 17 → 18 (File Comparison & Diffs):**  
  Compared two credential files to isolate changed entries:  
  `diff passwords.old passwords.new`

* **Level 18 → 19 (SSH Shell Bypass):**  
  Bypassed custom `.bashrc` session exits by passing immediate CLI commands via SSH:  
  `ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`

* **Level 19 → 20 (SUID Binary Privilege Execution):**  
  Executed a SUID binary to inspect elevated target files:  
  `./bandit20-do cat /etc/bandit_pass/bandit20`

* **Level 20 → 21 (Netcat Listener & SUID Handshake):**  
  Set up a local listening port with Netcat and triggered the SUID binary to complete the handshake and transmit credentials:  
  `nc -l -p 12345 < /etc/bandit_pass/bandit20`

---

📌 **Levels 21–34 Notes & Cheat-Sheet**

* **Level 21 → 22 (Cron Job Inspection):**
  Inspected `/etc/cron.d/cronjob_bandit22` to trace the scheduled shell script executing under `bandit22` context.
  `cat /etc/cron.d/cronjob_bandit22`

* **Level 22 → 23 (MD5 Hashed Cron Scripts):**
  Extracted user credentials by evaluating the shell script logic that mapped `$myname` via MD5 hashing.
  `echo I am bandit23 | md5sum | cut -d ' ' -f 1`

* **Level 23 → 24 (Cron Job Script Execution):**
  Created a writable temporary script inside `/var/spool/bandit24/foo` to dump the flag to a readable location.
  `echo '#!/bin/bash' > /tmp/bandit24/script.sh`
  `echo 'cat /etc/bandit_pass/bandit24 > /tmp/bandit24/password' >> /tmp/bandit24/script.sh`
  `chmod 777 /tmp/bandit24/script.sh`
  `chmod 777 /tmp/bandit24`
  `cp /tmp/bandit24/script.sh /var/spool/bandit24/foo/`

* **Level 24 → 25 (Pin Brute-Forcing over Port):**
  Brute-forced a 4-digit PIN combined with the current level password using a basic shell loop sent to Netcat port 30002.
  `for i in {0000..9999}; do echo "<password> $i"; done | nc localhost 30002`

* **Level 25 → 26 (More/Vi Shell Escape):**
  Resized the terminal window to force `more` pagination, then escaped into `vi` editor to launch a shell (`:e /bin/bash`).

* **Level 26 → 27 (Custom Shell Bypass):**
  Bypassed the bandit26 custom shell setup via `vi` commands to execute the setuid binary.

* **Level 27 → 28 (Git Clone):**
  Cloned the remote Git repository over SSH to retrieve the flag stored inside `README`.
  `git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo`

* **Level 28 → 29 (Git Commit History):**
  Inspected git commit logs to retrieve credentials removed in previous commits.
  `git log -p`

* **Level 29 → 30 (Git Branches):**
  Checked hidden branches using `git branch -a` and checked out dev branches to extract stored flags.
  `git checkout dev`

* **Level 30 → 31 (Git Tags):**
  Inspected Git tags using `git tag` and read tag data via `git show secret`.

* **Level 31 → 32 (Git Push & Pre-Receive Hook):**
  Pushed a tracked file to the master branch after resolving user configuration requirements.
  `echo "May I come in?" > key.txt`
  `git add -f key.txt`
  `git config user.email "bandit31@example.com"`
  `git config user.name "bandit31"`
  `git commit -m "pushing key"`
  `git push origin master`

* **Level 32 → 33 (Restricted Shell Escape):**
  Escaped the UPPERCASE SHELL by invoking the $0 parameter to launch the default shell binary.
  `>> $0`

* **Level 33 → 34 (Completion):**
  Read the final welcome note and verified game completion.

---

📌 **Status:** Levels 0–34 Fully Documented (Wargame Completed)
🔄 **Next Update:** TryHackMe & Network Security Labs
