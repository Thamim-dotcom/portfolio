# CyBox — Writeup

## TL;DR
SQLi bypass → insecure upload (RCE inside Docker) → container escape → cron job misconfig → root on host.

---

## Environment
- Web: port 8080
- SSH: port 22
- Web runs in Docker container.

---

## Steps

### 1) Recon
```bash
nmap -sC -sV -p- <TARGET_IP>
gobuster dir -u http://<TARGET_IP>:8080 -w /usr/share/wordlists/dirb/common.txt
```
2) SQLi login bypass
```vbnet
' OR 1=1--
```
3) File upload → RCE

Profile image upload accepted PHP disguised as .jpg.

Burp used to rename to .php.

Accessed file URL → webshell.

4) Inside container
```bash
id; hostname; cat /proc/1/cgroup
```
Confirmed Docker environment.

5) Container escape

Found Docker misconfig / volume mount.

Used escape technique (safe description only).

6) Host escalation

Found cron job running world-writable script.

Replaced script with safe payload.

Cron executed → root shell obtained.

Artifacts

scripts/notes.md — sanitized container escape notes.

Lessons

Validate file uploads.

Don’t expose Docker socket or insecure mounts.

Lock down cron job permissions.

```yaml

---

### `CyBox/scripts/notes.md`
```md
# Container escape notes (sanitized)

- Documented unsafe patterns (Docker socket exposure, writable mounts).
- In real labs, these allow container → host escape.
- PoCs are intentionally omitted from public repo.
```
Payload:
