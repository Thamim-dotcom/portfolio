# CTF Writeups — Thamim Ansari

Collection of sanitized CTF write-ups demonstrating web exploitation, digital forensics, container escape, and Linux privilege escalation techniques.

**Note:** All write-ups are sanitized. No live IPs, production credentials, private keys, or active exploit payloads are published. See `RESPONSIBLE_DISCLOSURE.md`.

## Contents
- EH101v13 — web upload -> RCE -> PATH hijack to root.
- Diffanc — forensic extraction from archive -> FTP foothold -> sudo misconfig.
- CyBox — SQLi + insecure upload -> container escape -> host cron -> root.

## How to use
Each challenge folder contains:
- `README.md` — step-by-step writeup (sanitized)
- `scripts/` or `artifacts/` — sanitized proof-of-concept (PoC) / helpers
- `assets/` — optional screenshots or diagrams
