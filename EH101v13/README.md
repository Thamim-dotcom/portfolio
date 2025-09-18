# EH101v13 — Writeup

## TL;DR
Initial foothold via web file upload (RCE) -> root via PATH hijacking of a privileged Python backup script.

> All outputs and examples below are **sanitized**. `<TARGET_IP>`, `<ADMIN_USER>`, and example filenames are placeholders.

---

## Environment (sanitized)
- Services observed: FTP (anonymous allowed), HTTP (web app), PHP webserver.
- No public IPs or real credentials included.

---

## Steps (detailed)

### 1) Reconnaissance
```bash
nmap -sC -sV -p- <TARGET_IP>
```
Found open services: port 21 (FTP), port 80 (HTTP).

2) Web enumeration
```
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt
```
Found /admin → login page.
3) Credential discovery / login

Burp Suite Intruder used with wordlists.

Found valid (sanitized) credentials → login successful.

4) File upload exploitation

Admin panel allowed uploading PHP disguised as an image.

Accessing uploaded file URL triggered execution.

5) Initial shell

Payload gave shell as www-data.
```bash
id; uname -a; hostname
```
6) Privilege escalation
```bash
sudo -l
```
Showed www-data can run /usr/local/bin/backups.py as root without password.

Script imported a module from writable path.

7) Exploit PATH/module hijack

Place malicious module in writable directory.

Run:
```bash
sudo /usr/local/bin/backups.py
```
Root shell obtained.

Artifacts

scripts/malicious_helper.py (sanitized PoC).

Lessons

Validate uploads securely.

Avoid scripts importing from writable paths.


---

### `EH101v13/scripts/malicious_helper.py`
```python
# MALICIOUS HELPER - SANITIZED EXAMPLE
# DO NOT USE outside controlled labs.
import os
print("SANITIZED: would spawn a root shell here")
# os.system("/bin/bash")  # commented out for safety
```
print("SANITIZED: would spawn a root shell here")
# os.system("/bin/bash")  # commented out for safety
