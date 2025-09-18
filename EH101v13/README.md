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
