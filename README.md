# 🔴 Red Team Lab Setup — Project Assignment Report

> **EDUCATIONAL USE ONLY** — All activities were conducted in a controlled, isolated virtual lab environment strictly for educational purposes. No real systems, networks, or data were accessed or compromised.

---

## 📋 Overview

This repository contains the project assignment report for the **Jr. Penetration Testing Engineer: Hands-On (C(PTE-T))** course under the **Enhancing Digital Government Economy (EDGE)** programme.

The report documents a full red team lab exercise covering reconnaissance, vulnerability exploitation, post-exploitation, privilege escalation, and persistence — performed against deliberately vulnerable target systems in an isolated virtual environment.

| Field | Details |
|---|---|
| **Trainee** | Shuvo Biswas |
| **Section** | CADS001 |
| **Trainer** | Md. Imran Chowdhury |
| **Designation** | Offensive Security Specialist |
| **Submission** | May 16, 2026 |

---

## 🖥️ Lab Environment

| VM | Role | OS | IP Address(es) | Network |
|---|---|---|---|---|
| Kali Linux | Attacker | Kali Linux 2026 | 192.168.50.2 / 192.168.220.128 | vLAN 1 |
| Metasploitable 2 | Target (Web Server) | Ubuntu 8.04 | 192.168.50.3 / 10.10.10.5 | vLAN 1 + vLAN 2 |
| Windows 10 | Target (Employee) | Windows 10 22H2 | 10.10.10.4 / 192.168.220.133 | vLAN 2 |

**Tools Used:** Kali Linux, Metasploit Framework, msfvenom, Nmap, arp-scan, netcat, TigerVNC, mysql client, Apache2

---

## 🗂️ Report Structure

```
Part 1 — Metasploitable 2 (Linux Target)
├── Reconnaissance (arp-scan + Nmap)
├── Exploit 01 — Port 21:   FTP vsftpd 2.3.4 Backdoor
├── Exploit 02 — Port 22:   SSH Brute Force
├── Exploit 03 — Port 23:   Telnet Default Credentials
├── Exploit 04 — Port 25:   SMTP User Enumeration
├── Exploit 05 — Port 80:   PHP-CGI Argument Injection
├── Exploit 06 — Port 139/445: Samba RCE
├── Exploit 07 — Port 1099: Java RMI
├── Exploit 08 — Port 1524: Ingreslock Bindshell
├── Exploit 09 — Port 5432: PostgreSQL Shared Library Upload
├── Exploit 10 — Port 5900: VNC Weak Password
├── Exploit 11 — Port 6667: UnrealIRCd Backdoor
├── Exploit 12 — Port 8180: Apache Tomcat WAR Upload
└── Exploit 13 — Port 3306: MySQL Unauthenticated Root Access

Part 2 — Windows 10 (Client-Side Attack)
├── Payload Generation (msfvenom)
├── Web Hosting (Apache2)
├── Listener Setup & Payload Execution
├── Meterpreter Post-Exploitation (VNC, Keyscan)
├── Privilege Escalation → NT AUTHORITY\SYSTEM
└── Persistence (Startup Folder Backdoor)
```

---

## 🎯 Findings Summary

| # | Port / Service | Vulnerability | CVE | Severity | Access Gained |
|---|---|---|---|---|---|
| 1 | 21 / FTP vsftpd 2.3.4 | Supply-chain Backdoor | CVE-2011-2523 | 🔴 Critical | Root Shell |
| 2 | 22 / SSH OpenSSH 4.7p1 | Weak Default Credentials | — | 🟠 High | User Shell |
| 3 | 23 / Telnet | Weak Creds + No Encryption | — | 🟠 High | User Shell |
| 4 | 25 / SMTP Postfix | User Enumeration (VRFY) | — | 🟡 Medium | Username List |
| 5 | 80 / Apache + PHP 5.2.4 | CGI Argument Injection | CVE-2012-1823 | 🔴 Critical | Meterpreter |
| 6 | 139/445 / Samba 3.0.20 | Username Map Script RCE | CVE-2007-2447 | 🔴 Critical | Root Shell |
| 7 | 1099 / Java RMI | Insecure Default Config | CVE-2011-3556 | 🔴 Critical | Meterpreter |
| 8 | 1524 / Bindshell | Pre-spawned Root Shell | — | 🔴 Critical | Root Shell |
| 9 | 5432 / PostgreSQL 8.3 | Default Creds + .so Upload | — | 🔴 Critical | Meterpreter |
| 10 | 5900 / VNC 3.3 | Weak Password | — | 🟠 High | GUI Desktop |
| 11 | 6667 / UnrealIRCd 3.2.8.1 | Source Code Backdoor | CVE-2010-2075 | 🔴 Critical | Root Shell |
| 12 | 8180 / Tomcat 5.5 | Default Creds + WAR Upload | — | 🟠 High | User Shell |
| 13 | 3306 / MySQL 5.0.51a | No Root Password | — | 🟠 High | Full DBA |
| 14 | Windows 10 | Client-side Payload Delivery | — | 🔴 Critical | Meterpreter → SYSTEM |

**Result: 14/14 attack vectors successfully exploited. 9 rated Critical.**

---

## 🔧 Methodology

The assessment followed a five-phase penetration testing methodology:

1. **Reconnaissance** — Host discovery with `arp-scan`, port/service enumeration with `nmap -sV`
2. **Enumeration & Vulnerability Analysis** — Service-specific scanners, Metasploit auxiliary modules
3. **Exploitation** — Metasploit modules, manual techniques, and custom msfvenom payloads
4. **Post-Exploitation** — Privilege escalation, credential harvesting, VNC takeover, keystroke logging
5. **Persistence & Reporting** — Startup folder backdoor, registry-based AV disabling, documented findings

---

## 🛡️ Key Recommendations

| # | Recommendation |
|---|---|
| R1 | **Patch Management** — Update all end-of-life software immediately |
| R2 | **Eliminate Default & Weak Credentials** — Enforce strong password policies |
| R3 | **Disable Unnecessary Services** — Remove Telnet, rsh, bindshell, etc. |
| R4 | **Network Segmentation** — Restrict database services from external access |
| R5 | **Endpoint Protection** — Keep Defender/EDR enabled via Group Policy |
| R6 | **Application Whitelisting** — Use AppLocker/WDAC to block unauthorized executables |
| R7 | **Principle of Least Privilege** — Run services under low-privilege accounts |
| R8 | **Multi-Factor Authentication** — Enforce MFA on SSH, VNC, RDP |
| R9 | **Security Monitoring (SIEM)** — Alert on port scans, failed logins, port 4444 outbound |
| R10 | **Security Awareness Training** — Train users against social engineering and suspicious downloads |

---

## ⚠️ Disclaimer

This project was completed as part of a structured, supervised academic programme. All techniques demonstrated are for **educational purposes only**. Performing these activities against systems without explicit written authorization is illegal and unethical.

> *"Security is only as strong as its weakest link."*
