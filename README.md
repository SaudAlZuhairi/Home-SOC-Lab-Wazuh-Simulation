# Home Lab Simulation: Insider Threat Detection using Wazuh SIEM

## 1. Project Overview
This project demonstrates a real-world cyber attack simulation focused on **Insider Threat Detection**. The primary goal was to practice **Blue Team operations**, including log analysis, threat detection, and incident response within a controlled laboratory environment.

## 2. Environment & Topology
* **Virtualization:** Deployed using **VMware Workstation Pro**.
* **SIEM Platform:** **Wazuh Server** (OVA deployment) used for log aggregation and threat detection.
* **Target Machine (Victim):** Windows Server 2022 Standard configured with vulnerable services (RDP enabled, Firewall disabled for simulation).
* **Attacker Machine:** **Kali Linux** (simulating the Red Team).
* **Connectivity:** Implemented **ZeroTier** (SD-WAN) to create a secure virtual LAN, bypassing CGNAT and double-router restrictions.

## 3. Attack Lifecycle Simulation
The simulation followed a typical attack kill chain:

* **Reconnaissance:** Used **Nmap** to identify open services on the target server, discovering ports `3389` (RDP) and `445` (SMB).
* **Brute-Force Attack:** Employed **Hydra** to target the SMB service until the correct credentials were found.
* **Initial Access:** Successfully logged into the target server via RDP upon obtaining correct credentials.
* **Persistence & Privilege Escalation:** Created a new administrative user account named `backdoor1` to maintain access.
* **Covering Tracks:** Attempted to clear Windows Event Logs (Security, System, and Application) using the `wevtutil cl` command.

## 4. Monitoring & Detection (Wazuh SIEM)
The SIEM successfully detected and alerted on multiple critical events:

* **Authentication Failures:** Captured a spike in **Event ID 4625** resulting from the brute-force attack.
* **Successful Logon:** Alerted on **Event ID 4624** originating from the attacker's IP.
* **Account Creation:** Detected the creation of the unauthorized user (**Event ID 4720**) and its addition to privileged groups (**Event ID 4732**).
* **Log Clearing:** Triggered an alert for **Event ID 1102**, confirming the attempt to delete forensic evidence.

## 5. Key Security Recommendations
1. **Enable Active Response:** Automatically block suspicious source IP addresses after a predefined number of failed attempts.
2. **MFA for RDP:** Implement Multi-Factor Authentication and change default RDP ports to reduce the attack surface.
3. **Strengthen Password Policies:** Enforce complex password requirements to mitigate the effectiveness of brute-force attacks.
4. **Continuous Monitoring:** Regularly analyze logs to identify early-stage reconnaissance or persistent threats.

---
**Prepared by: Saud Al-Qahtani**
