## 📌 Task 07–08 — Capstone Penetration Test & Professional Security Assessment

**Zyroo Cybersecurity Internship Track — Weeks 7–8 (Final Task)**

### Engagement Overview

This capstone task treats the lab environment as a full client engagement, consolidating reconnaissance,
exploitation, and privilege escalation across both lab targets (**Metasploitable2** and **OWASP Juice Shop**)
into one connected assessment, producing an industry-standard penetration test report, and demonstrating
remediation by applying and independently re-verifying a real fix — closing the loop from attacker to defender.

### Findings Summary (14 consolidated findings)

| Severity | Count | Representative Findings |
|---|---|---|
| Critical | 4 | vsftpd 2.3.4 backdoor, distccd RCE, PHP-CGI Argument Injection RCE, SUID `nmap` privilege escalation |
| High | 5 | Telnet cleartext auth, outdated Samba, SQLi/XSS/CSRF (WebGoat), Broken Access Control, IDOR (Juice Shop) |
| Medium | 5 | Anonymous FTP, weak VNC, rlogin, outdated MySQL, JWT client-side trust |

### Attack Chain Demonstrated

| Stage | Technique |
|---|---|
| **Initial Access** | PHP-CGI Argument Injection (CVE-2012-1823) via Metasploit → foothold as `www-data` |
| **Privilege Escalation** | SUID misconfiguration on `/usr/bin/nmap` → `nmap --interactive` → `!sh` → full `root` |
| **Post-Exploitation Impact** | Full `/etc/passwd` read access demonstrating complete system compromise |

### Mitigation Applied & Verified

**Finding remediated:** Anonymous FTP login (vsftpd)
**Fix:** `anonymous_enable=YES` → `anonymous_enable=NO` in `/etc/vsftpd.conf`
**Before:** Anonymous login succeeded (`230 Login successful`)
**After:** Anonymous login refused (`530 Login incorrect`)
**Verification method:** Live re-test from the attacker machine after applying the fix and a clean service restart.

### Contents

| File | Description |
|---|---|
| `Capstone_Penetration_Test_Report_Zyroo_Task07-08.pdf` | Full professional report: executive summary, scope & methodology, reconnaissance, 14 consolidated findings with severity ratings, exploitation, privilege escalation, post-exploitation impact, mitigation with before/after evidence, risk summary, and prioritized remediation roadmap |
| `screenshots/` | Evidence screenshots referenced throughout the report |

### Screenshots

| # | File | Evidence of |
|---|---|---|
| 1 | `01-recon-metasploitable2-alive-netdiscover.png` | Metasploitable2 confirmed live on the lab network |
| 2 | `02-recon-juiceshop-port3000-scan.png` | Juice Shop confirmed reachable and serving the application |
| 3 | `03-recon-metasploitable2-fullscan-allports.png` | Full 65,535-port scan — 14 open TCP ports discovered |
| 4 | `04-exploitation-session-opened-wwwdata.png` | PHP-CGI RCE exploited — Meterpreter session as `www-data` |
| 5 | `05-privesc-suid-nmap-root-access.png` | SUID `nmap` escalation — confirmed `root` access |
| 6 | `06-postexploitation-etc-passwd-dump.png` | Full system account database read as `root` |
| 7 | `07-mitigation-xinetd-vsftpd-config.png` | vsftpd's xinetd-managed configuration |
| 8 | `08-mitigation-before-anonymous-ftp-success.png` | BEFORE — anonymous FTP login succeeding |
| 9 | `09-mitigation-after-anonymous-ftp-denied.png` | AFTER — anonymous FTP login refused, fix verified |

### Tools Used

- **Nmap** — full-range and targeted service/version scanning
- **Metasploit Framework** — PHP-CGI Argument Injection exploitation
- **Meterpreter / shell** — post-exploitation and privilege escalation
- **vsftpd / xinetd** — mitigation target
- **Kali Linux (VMware)** — attacker machine, isolated local lab environment

### Rules of Engagement

All testing was confined to the isolated Zyroo lab network (Kali ↔ Metasploitable2 ↔ Juice Shop). No real,
production, or third-party systems were targeted at any point. One finding was mitigated and independently
re-tested to confirm resolution, as required by the engagement scope.

### Disclaimer

This repository documents authorized, controlled security testing performed entirely within a private lab
environment as part of a structured cybersecurity training exercise. None of the techniques described here
were used against any system without explicit authorization.

---

**🎓 Internship Complete** — All 8 weeks of the Zyroo Cybersecurity Internship Track are done.
