# OverTheWire Bandit — Session Notes
**Date:** 2025-10-07  
**Time spent:** ~1 hour  

---

## Levels Solved — 3 → 6

### Level 3 → Level 4
**Goal:** Find the password hidden inside the `inhere` directory.  

**Command(s):**
cd inhere
ls -al
cat …Hiding-From-You

**Logic:**  
After moving into `inhere`, I listed with `-al` to reveal hidden files. The file `...Hiding-From-You` contained the password.

---

### Level 4 → Level 5
**Goal:** Find the human-readable file among many `-file0*` entries in the `inhere` directory.  

**Command(s):**
cd inhere
cat – -file07

**Logic:**  
Most files showed binary garbage. By checking each with `cat -- filename`, I found `-file07` which was plain text and contained the password.

---

### Level 5 → Level 6
**Goal:** Find the only file with specific properties: human-readable, 1033 bytes, not executable.  

**Command(s):**
find . -type f -size 1033c ! -executable
cat ./inhere/maybehere07/.file2

**Logic:**  
At first I mistyped `find` options (`find . type f -size 1033c`). Corrected it to `-type f` and located `.file2` inside `inhere/maybehere07`. Reading it gave the password.
