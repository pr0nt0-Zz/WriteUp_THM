# 🐧 Linux File Analysis Challenge — TryHackMe Writeup

## 🧙‍♂️ Challenge Summary

In this challenge, we were given access to a Linux machine containing several specially named files scattered throughout the file system. The goal was to answer a set of questions using Linux command-line tools as efficiently as possible.

---

## 🔎 Target Files

The following files were part of the investigation:

```
8V2L  bny0  c4ZX  D8B3  FHl1  oiMO  PFbD  
rmfX  SRSq  uqyw  v2Vb  X1Uy
```

---

## ✅ Questions & Answers

### 1. **Which files are owned by the `best-group` group?**

```bash
find / -type f \( -name 8V2L -o -name bny0 -o ... \) -exec ls -l {} + 2>/dev/null
```

**Answer:**

```
D8B3 v2Vb
```

---

### 2. **Which file contains an IP address?**

```bash
grep -Eo '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' /path/to/files
```

**Match found in:**

```
oiMO
```

---

### 3. **Which file has the SHA1 hash `9d54da7584015647ba052173b84d45e8007eba94`?**

```bash
for file in /path/to/files; do sha1sum "$file"; done | grep '9d54da...'
```

**Answer:**

```
c4ZX
```

---

### 4. **Which file contains 230 lines?**

```bash
wc -l /path/to/files
```

All known files had **209 lines**, and one (`bny0`) was missing — therefore, by elimination:

**Answer:**

```
bny0
```

---

### 5. **Which file's owner has an ID of 502?**

```bash
ls -n /path/to/files
```

Look for UID (3rd column) = `502`

**Answer:**

```
X1Uy
```

---

### 6. **Which file is executable by everyone?**

```bash
ls -l /path/to/files
```

Look for `x` in all permission segments (`rwxrwxr-x`)

**Answer:**

```
8V2L
```

---

## 🛠 Tools & Commands Used

* `find`
* `ls -l`, `ls -n`
* `grep -E`
* `sha1sum`
* `wc -l`
* `awk`

---

## 📘 Conclusion

This room served as a great exercise in applying basic Linux command-line tools for forensic-style file analysis. It reinforces the importance of precision, pattern recognition, and thinking logically under constraints — all essential skills in cybersecurity and system administration.
