# 🚩 OverTheWire: Bandit Wargame Solutions

Documentation and command cheat-sheet for the OverTheWire Bandit wargame.

---

## 🛠️ Levels 0–10 Notes

* **Levels 0–3 (Navigation & Special Files):** Handled spaces and hyphens using `cat ./-file-` and `cat "file name"`. Located hidden dotfiles with `ls -la`.
* **Levels 4–6 (Searching & Permissions):** Filtered files by size, user, and non-executable properties while suppressing error noise using `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null` and extracted strings with `strings data.txt`.
* **Levels 7–10 (Text Processing & CLI Pipelines):** Extracted patterns using `grep "millionth" data.txt`, isolated non-repeated entries using `sort data.txt | uniq -u`, and decoded strings with `base64 -d data.txt`.

---

📌 *Status:* **Levels 0–10 Documented**  
🔄 *Next Update:* **Levels 11–20**
