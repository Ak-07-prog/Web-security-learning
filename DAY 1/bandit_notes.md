# OverTheWire Bandit — Session Notes
**Date:** 2025-10-05  
**Time spent:** 1 hour  

---

## Levels Solved — 0, 1, 2 → 3

### Level 0
**Goal:** Learn how to log in to the game using the given credentials.  

**Command(s):**
ssh bandit0@bandit.labs.overthewire.org -p 2220

**Logic:**  
The task was just to connect with SSH using the provided username and password.

---

### Level 0 → Level 1
**Goal:** Retrieve the password from the file `readme` which existed in the home directory.  

**Command(s):**
cat readme

**Logic:**  
It was a simple alphabetic filename, so just using `cat` displayed the password.

---

### Level 1 → Level 2
**Goal:** Retrieve the password from a file named `-`.  

**Command(s):**
cat ./-

**Logic:**  
Initially, the dash was interpreted as a symbol, so the terminal waited for input.  
Solved by adding a relative path (`./-`) to treat it as a file instead of an option.

---

### Level 2 → Level 3
**Goal:** Retrieve the password from a file named `spaces in this filename`.  

**Command(s):**
cat -- “--spaces in this filename--”

**Logic:**  
The filename started with normal characters but included spaces.  
Solved by wrapping the filename in quotes so the shell treated it as a single argument.

