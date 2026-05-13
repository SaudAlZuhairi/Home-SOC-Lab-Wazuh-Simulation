# Home-SOC-Lab-Wazuh-Simulation
A practical simulation of an insider threat attack (RDP Brute-force) detected and analyzed using Wazuh SIEM in a virtualized environment.

Project Overview:
 This project demonstrates a real-world cyber attack simulation focused on Insider Threat Detection. The goal was to practice Blue Team operations, including log analysis, threat detection, and incident response within a controlled laboratory environment.
2. Environment & Topology:
 Virtualization: Deployed using VMware Workstation Pro. 
 SIEM Platform: Wazuh Server (OVA deployment) for log aggregation and analysis.  
 Target Machine (Victim): Windows Server 2022 Standard with RDP enabled and firewall disabled for simulation purposes.  
 Attacker Machine: Kali Linux.
Connectivity: Implemented ZeroTier (SD-WAN) to create a secure virtual LAN and bypass CGNAT restrictions. 
3. Attack Lifecycle Simulation:
The simulation followed a typical attack kill chain:
Reconnaissance: Used Nmap to identify open services, discovering ports 3389 (RDP) and 445 (SMB). 
Brute-Force Attack: Employed Hydra to target the SMB service until the correct credentials were found.  
Initial Access: Successfully logged into the target server using the compromised credentials. 
Persistence & Privilege Escalation: Created a backdoor account (backdoor1) and assigned it administrative privileges.  
Covering Tracks: Attempted to clear Windows Event Logs (Security, System, and Application) using wevtutil.  
4. Monitoring & Detection (Wazuh SIEM)
The SIEM successfully detected and alerted on multiple critical events:
Authentication Failures: Captured a spike in Event ID 4625 (Brute-Force attempt). 
Successful Logon: Alerted on Event ID 4624 following the brute-force attack. 
Account Creation: Detected the creation of the unauthorized administrative user (Event ID 4720).  
Log Clearing: Triggered an alert for Event ID 1102, confirming the attempt to delete forensic evidence.  
5. Key Security:
RecommendationsEnable Active Response: Automatically block source IPs after multiple failed attempts.  
MFA for RDP: Implement Multi-Factor Authentication and change default ports to reduce exposure. 
Password Policies: Enforce complex password requirements to mitigate brute-force success.  
