# 🛡️ SOC Investigation Report: The Wareville Glitch (AoC Day 1)

## 📋 Executive Summary
This report documents the investigation of a cyber security incident on the `tbfc-web01` server. An attacker known as **Sir Carrotbane** from **HopSec** breached the server, exfiltrated sensitive data, and sabotaged the Christmas wishlist countdown. By following a trail of logs, hidden files, and encrypted messages, we neutralized the glitch and recovered the "Side Quest" Easter Egg.

---

## 📍 Phase 1: Initial Discovery & Reconnaissance
The investigation began with searching for instructions left by McSkidy before her disappearance.

### **1. Finding the Hidden Guide**
We identified a hidden file in the `Guides` directory that contained essential investigative steps.
- **Command:** `cd ~/Guides && ls -la`
- **Output:** `.guide.txt`
- **Analysis:** The guide instructed us to check `/var/log` for "Eggsploits" and failed login attempts.

### **2. Identifying the Attacker**
We filtered the authentication logs to find traces of unauthorized access.
- **Command:** `grep "Failed password" /var/log/auth.log`
- **Output:** `Failed password for socmas from eggbox-196.hopsec.thm`
- **Analysis:** A brute-force attack was launched against the `socmas` user from a HopSec domain.

### **3. Locating the Malware (Eggstrike)**
We searched the `socmas` home directory for malicious scripts.
- **Command:** `find /home/socmas -name "*egg*"`
- **Output:** `/home/socmas/2025/eggstrike.sh`
- **Analysis:** The script `eggstrike.sh` was found to be stealing data by redirecting unique wishlist items to `/tmp/dump.txt` and replacing the real wishlist with an "EASTMAS" version.

---

## 🧩 Phase 2: Reconstructing the UNLOCK_KEY
To access the backend secrets, we had to find three password fragments hidden by McSkidy.

### **4. Fragment 1: Environment Variables**
- **Command:** `env | grep "PART"`
- **Output:** `PART1=91J6X7R4`

### **5. Fragment 2: Git History**
- **Command:** `cd /home/socmas/2025 && git log -p`
- **Output:** `- password_fragment = "FQ9TQPM9"`
- **Analysis:** This fragment was found in a previous commit where it had been "deleted" for security.

### **6. Fragment 3: Hidden Directory**
- **Command:** `cat /home/eddi_knapp/.secret/dir/fragment.txt`
- **Output:** `JX2Q9X2Z`

**Full Reconstructed Key:** `91J6X7R4FQ9TQPM9JX2Q9X2Z`

---

## 🛠️ Phase 3: Reversing the Attack
The server was running a Node.js process on port `8081` that required a valid `wishlist.txt` to release the flag.

### **7. Restoring the Wishlist**
We used the attacker's own temporary "dump" file to restore the original Christmas items.
- **Command:** `cat /tmp/dump.txt`
- **Restoration Command:** `echo -e "Apple Vision Pro\nBluetooth Speaker\nGaming Console\nLaptop\nSmartphone\nSmartwatch" > /home/socmas/2025/wishlist.txt`

### **8. Extracting the Ciphertext**
With the wishlist restored, the "Secret Server" provided the encrypted message.
- **Command:** `curl http://127.0.0.1:8081/secret`
- **Output:** `{"ok":true, "text":"U2FsdGVkX1..."}`

---

## 🔑 Phase 4: Final Decryption & Side Quest
### **9. Decrypting the Main Flag**
We used OpenSSL with our reconstructed key to reveal the flag.
- **Command:** `openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 -in ciphertext.txt -out decrypted.txt -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'`
- **Flag:** `THM{w3lcome_2_A0c_2025}`

### **10. Side Quest: The Jester Egg**
The flag acted as a password for a GPG-encrypted archive.
- **Command:** `gpg --output dir.tar.gz --decrypt /home/eddi_knapp/.secret/dir.tar.gz.gpg`
- **Extraction:** `tar -xzvf dir.tar.gz`
- **Side Quest Key:** Opening `sq1.png` revealed the hidden text: **`now_you_see_me`**.
- <img width="555" height="600" alt="DAY1 extracted png" src="https://github.com/user-attachments/assets/73497800-4905-4688-99df-e689a95d2391" />


---
**Mission Accomplished: Christmas is Restored!** 🎄

