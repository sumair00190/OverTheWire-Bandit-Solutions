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

📌 **Status:** Levels 0–20 Documented  
🔄 **Next Update:** Levels 21–30
