Diffanc/README.md

# Diffanc — Writeup

## TL;DR
Recovered hidden credentials from archive (binwalk & foremost) → FTP login → initial shell → root via sudoed Python script importing a writable module.

---

## Environment
- Services: HTTP (uploads dir), FTP (with creds).
- `/uploads/archive.zip` found.

---

## Steps

### 1) Recon
```bash
nmap -sC -sV -p- <TARGET_IP>
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirb/common.txt
```
2) File analysis
```bash
wget http://<TARGET_IP>/uploads/archive.zip
binwalk archive.zip
foremost archive.zip -o recovered
```
Extracted hidden creds (sanitized: ADMIN_USER:REDACTED_PASS).

3) FTP access
```bash
ftp <TARGET_IP>
```
Logged in with extracted creds.

Found misconfigured Python script.

4) Initial foothold

Uploaded or abused script to gain low-priv shell.

5) Privilege escalation
```
sudo -l
```

Showed permission to run /usr/bin/run_app.py as root.

Script imported writable module → hijacked with malicious code.
```bash
sudo /usr/bin/run_app.py
```
Root shell obtained.

Artifacts

scripts/malicious_module.py (sanitized).

Lessons

Don’t embed sensitive data in archives.

Harden FTP.

Use absolute imports in root scripts.


---

### `Diffanc/scripts/malicious_module.py`
```python
# MALICIOUS MODULE - SANITIZED
import os
print("SANITIZED: executed malicious module")
# os.system("/bin/bash")  # removed for safety

```
