# 🔴 Red Team Lab — Penetration Testing Project

> ⚠️ **EDUCATIONAL USE ONLY**  
> All activities documented in this repository were performed in a controlled, isolated virtual lab environment for educational purposes. No unauthorized real-world systems, networks, or data were targeted.

## 📌 Overview

This repository contains the project assignment report for the **Jr. Penetration Testing Engineer: Hands-On (C(PTE-T))** course under the **Enhancing Digital Government Economy (EDGE)** programme.

The project demonstrates a complete red team / penetration testing workflow against intentionally vulnerable virtual machines. The assessment covers:

- Reconnaissance
- Host discovery
- Port and service enumeration
- Vulnerability identification
- Exploitation
- Post-exploitation
- Privilege escalation
- Persistence
- Security recommendations

The lab consisted of **Kali Linux as the attacker**, **Metasploitable 2 as the vulnerable Linux target**, and **Windows 10 as the client-side attack target**. The environment was segmented into two virtual networks to simulate an external/DMZ network and an internal employee network.

---

## 🎯 Objectives

The main objectives of this project were:

1. Configure a multi-machine penetration testing laboratory.
2. Perform host discovery and network reconnaissance.
3. Enumerate open ports and running services.
4. Identify vulnerable and outdated services.
5. Exploit vulnerabilities using appropriate tools and techniques.
6. Perform Meterpreter-based post-exploitation.
7. Demonstrate privilege escalation.
8. Demonstrate persistence mechanisms.
9. Document findings and security recommendations.

The project specifically focused on practical offensive-security skills using tools such as Nmap, Metasploit Framework, msfvenom, arp-scan, netcat, and other security utilities.

---

## 🖥️ Lab Environment

| Machine | Role | Operating System | Network |
|---|---|---|---|
| Kali Linux | Attacker | Kali Linux 2026 | vLAN 1 |
| Metasploitable 2 | Vulnerable Target / Web Server | Ubuntu 8.04 | vLAN 1 + vLAN 2 |
| Windows 10 | Employee Target | Windows 10 22H2 | vLAN 2 |

### Network Architecture

```text
                    vLAN 1
              192.168.50.0/24
                     
        ┌──────────────────────┐
        │      Kali Linux      │
        │       Attacker       │
        │    192.168.50.2      │
        └──────────┬───────────┘
                   │
                   │
        ┌──────────▼───────────┐
        │    Metasploitable 2  │
        │      Web Server      │
        │  eth0: 192.168.50.3  │
        │  eth1: 10.10.10.5    │
        └──────────┬───────────┘
                   │
                   │ vLAN 2
                   │ 10.10.10.0/24
                   │
        ┌──────────▼───────────┐
        │      Windows 10      │
        │   Employee Target    │
        │     10.10.10.4       │
        └──────────────────────┘
```

Metasploitable 2 uses two network interfaces and acts as the connection point between the two isolated virtual networks. The Kali attacker does not have direct access to the internal network, simulating a segmented DMZ-style architecture.

---

## 🛠️ Tools Used

- **Kali Linux**
- **Metasploit Framework**
- **msfconsole**
- **msfvenom**
- **Nmap**
- **arp-scan**
- **Netcat**
- **Telnet**
- **MySQL Client**
- **Apache2**
- **TigerVNC / TightVNC**

---

# 🔎 Methodology

The assessment followed a five-phase penetration testing methodology.

### 1. Reconnaissance

Host discovery and network enumeration were performed using:

- `arp-scan`
- `Nmap`
- Service/version detection

The reconnaissance phase identified the vulnerable Metasploitable 2 host and its exposed services.

### 2. Enumeration & Vulnerability Analysis

Individual services were analyzed to identify:

- Software versions
- Misconfigurations
- Weak credentials
- Known vulnerabilities
- Exposed services

Metasploit auxiliary modules and Nmap scripts were used during enumeration.

### 3. Exploitation

Identified vulnerabilities were tested in the isolated lab using:

- Metasploit Framework
- Manual techniques
- Netcat
- Telnet
- MySQL client
- Custom payload generation with msfvenom

### 4. Post-Exploitation

Post-exploitation activities included:

- Meterpreter sessions
- File system access
- VNC remote desktop access
- Keystroke logging
- Network reconnaissance
- Credential-related activities
- Privilege escalation

### 5. Persistence & Reporting

The Windows environment was also used to demonstrate persistence after compromise. All activities were documented with screenshots, command outputs, findings, and remediation recommendations.

---

# 🐧 Part 1 — Metasploitable 2

Metasploitable 2 is an intentionally vulnerable Linux virtual machine designed for security training.

The reconnaissance phase identified multiple exposed services. The project subsequently demonstrated exploitation of **13 distinct service vulnerabilities**.

## Vulnerability Summary

| # | Port / Service | Vulnerability | CVE | Severity | Access |
|---|---|---|---|---|---|
| 1 | 21 / FTP | vsftpd 2.3.4 Backdoor | CVE-2011-2523 | 🔴 Critical | Root Shell |
| 2 | 22 / SSH | Weak Default Credentials | — | 🟠 High | User Shell |
| 3 | 23 / Telnet | Weak Credentials + No Encryption | — | 🟠 High | User Shell |
| 4 | 25 / SMTP | User Enumeration | — | 🟡 Medium | Username List |
| 5 | 80 / HTTP | PHP-CGI Argument Injection | CVE-2012-1823 | 🔴 Critical | Meterpreter |
| 6 | 139/445 / Samba | Username Map Script RCE | CVE-2007-2447 | 🔴 Critical | Root Shell |
| 7 | 1099 / Java RMI | Insecure Configuration | CVE-2011-3556 | 🔴 Critical | Meterpreter |
| 8 | 1524 / Bindshell | Pre-spawned Root Shell | — | 🔴 Critical | Root Shell |
| 9 | 5432 / PostgreSQL | Default Credentials + Library Upload | — | 🔴 Critical | Meterpreter |
| 10 | 5900 / VNC | Weak Password | — | 🟠 High | GUI Desktop |
| 11 | 6667 / UnrealIRCd | Source Code Backdoor | CVE-2010-2075 | 🔴 Critical | Root Shell |
| 12 | 8180 / Tomcat | Default Credentials + WAR Upload | — | 🟠 High | User Shell |
| 13 | 3306 / MySQL | No Root Password | — | 🟠 High | Full DBA |

### Result

**13/13 Metasploitable 2 attack vectors were successfully demonstrated.**

The report identifies **9 of the 14 total attack vectors as Critical**, with several producing root-level access.

---

# 🪟 Part 2 — Windows 10 Client-Side Attack

The second part of the project demonstrated a client-side attack chain against a Windows 10 virtual machine.

The attack lifecycle included:

```text
Payload Generation
        ↓
Web Hosting
        ↓
Payload Execution
        ↓
Meterpreter Session
        ↓
Post-Exploitation
        ↓
Privilege Escalation
        ↓
SYSTEM Access
        ↓
Persistence
```

### Demonstrated Techniques

- Payload generation with msfvenom
- Payload hosting through Apache2
- Meterpreter session establishment
- Remote desktop access through VNC
- Keystroke logging
- Screenshot and system interaction
- Privilege escalation
- SYSTEM-level access
- Persistence after reboot

The project documentation records successful privilege escalation to:

```text
NT AUTHORITY\SYSTEM
```

and demonstrates persistence through the Windows Startup mechanism.

---

# 📊 Overall Findings

The complete assessment documented:

**14 attack vectors successfully demonstrated**

```text
Metasploitable 2     → 13 vulnerabilities
Windows 10           → 1 client-side attack chain

Total                → 14 attack vectors
Critical             → 9
```

The findings demonstrate how outdated software, weak credentials, insecure configurations, exposed services, and disabled endpoint protections can lead to complete system compromise.

---

# 🛡️ Key Security Recommendations

### R1 — Patch Management

Keep operating systems and applications updated. Replace unsupported and end-of-life software with currently supported releases.

### R2 — Strong Credentials

Eliminate default credentials and enforce strong password policies.

### R3 — Disable Unnecessary Services

Remove or disable unnecessary legacy services such as Telnet, rsh, rexec, and insecure bindshell services.

### R4 — Network Segmentation

Restrict access to sensitive services using firewall rules and network segmentation.

Database services such as MySQL and PostgreSQL should not be directly exposed to untrusted networks.

### R5 — Endpoint Protection

Keep Windows Defender and EDR solutions enabled and centrally managed.

### R6 — Application Control

Use application-control technologies such as AppLocker or WDAC where appropriate.

### R7 — Least Privilege

Run services and applications using the minimum privileges required.

### R8 — Multi-Factor Authentication

Use MFA for remote-access services wherever possible.

### R9 — Security Monitoring

Monitor suspicious activities such as:

- Port scanning
- Repeated authentication failures
- Unexpected outbound connections
- Suspicious process execution
- Unauthorized persistence mechanisms

### R10 — Security Awareness

Train users to recognize suspicious downloads, social-engineering attempts, and malicious files.

---

# 📁 Repository Structure

```text
red-team-lab-penetration-testing/
│
├── README.md
│
├── report/
│   └── Red-Team-Lab-Project-Report.pdf
│
├── screenshots/
│   ├── lab-setup/
│   ├── reconnaissance/
│   ├── metasploitable/
│   └── windows/
│
└── documentation/
    └── findings.md
```

> Screenshots and documentation should contain only lab-generated evidence. Do not upload real credentials, tokens, private keys, personal data, or information from unauthorized systems.

---

# 📚 Learning Outcomes

This project provided practical experience in:

- Red team methodology
- Penetration testing
- Network reconnaissance
- Service enumeration
- Vulnerability assessment
- Metasploit Framework
- Nmap
- Meterpreter
- Linux exploitation
- Windows post-exploitation
- Privilege escalation
- Persistence concepts
- Security reporting
- Vulnerability remediation

The project followed the same broad lifecycle used throughout the report:

**Reconnaissance → Enumeration → Exploitation → Post-Exploitation → Persistence → Reporting**

---

# 📄 Project Information

| Field | Details |
|---|---|
| **Project** | Red Team Lab Setup |
| **Course** | Jr. Penetration Testing Engineer: Hands-On |
| **Certification** | C(PTE-T) |
| **Programme** | Enhancing Digital Government Economy (EDGE) |
| **Trainee** | Shuvo Biswas |
| **Section** | CADS001 |
| **Trainer** | Md. Imran Chowdhury |
| **Designation** | Offensive Security Specialist |
| **Submission** | May 17, 2026 |

---

# ⚠️ Disclaimer

This repository is intended strictly for **educational and authorized security-testing purposes**.

All activities described in this project were conducted inside a controlled and isolated virtual laboratory using intentionally vulnerable systems.

Do **not** use these techniques against systems, networks, accounts, or devices without explicit authorization.

Unauthorized penetration testing, credential attacks, exploitation, persistence, or data access may be illegal and unethical.

---

## 👨‍💻 Author

**Shuvo Biswas**

Jr. Penetration Testing Engineer: Hands-On  
EDGE Programme — CADS001
instructor: Md. Imran Chowdhuri

---

> **"Security is only as strong as its weakest link."**
