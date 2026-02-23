# Active Threat Detection: RDP Brute Force Simulation & SIEM Analysis

## Project Overview
The objective of this lab was to simulate a real-world cyber attack environment to develop practical threat detection and log analysis skills. By acting as both the adversary (Red Team) and the defender (Blue Team), this project demonstrates hands-on experience in endpoint security monitoring, generating telemetry, and hunting for specific Indicators of Compromise (IOCs) using a SIEM.

## Methodology
* **Network Configuration:** Established a Windows 10 victim machine and a Kali Linux attacker machine on an isolated custom NAT network.
* **Security Hardening Adjustment:** Intentionally disabled Windows Network Level Authentication (NLA) to allow for the simulation of automated remote authentication attempts.
* **Attack Execution:** Conducted a network-based RDP brute-force dictionary attack against the Windows `Employee` account using **Hydra**.
* **SIEM Monitoring:** Utilized a **Wazuh** agent to ingest and forward endpoint security logs to the main dashboard for real-time analysis.

## Security Note: Lab Isolation
A critical component of this lab was the use of a **Custom NAT Network**. I intentionally isolated these virtual machines from my physical home network and the public internet. 
* **The "Why":** Running brute-force tools and weakening security settings (like disabling NLA) creates security risks if performed on a live network. 
* **The Result:** This "Sandbox" approach allowed me to safely simulate aggressive attack traffic and generate realistic telemetry without exposing personal devices.

---

## Phase 2: Network Brute Force Simulation 

### The Attack
I launched a brute-force dictionary attack against the `Employee` account over the network using Kali Linux. 

<p align="center">
  <img src="Hydra%20attack.jpg" alt="Kali Hydra Attack" width="100%">
</p>

### The Detection
Even though the machines were isolated, my Wazuh SIEM successfully captured the flood of network-based **Event ID 4625** (Failed Logon) alerts.

By drilling down into the raw forensic data, I successfully extracted the complete footprint of the attacker:

<p align="center">
  <img src="Brute%20force%20Document%20Details.png" alt="Wazuh Brute Force Alert" width="100%">
</p>

* **Targeted Account:** `Employee`
* **Logon Type:** `3` (Verifying a remote network connection)
* **Source IP Address:** `10.0.2.8`
* **Source Hostname:** `kali`
* **Status Code:** `0xc0000234` (Confirming the attack triggered the account lockout policy)

---

## Lessons Learned & Summary
This project provided deep insight into the lifecycle of a credential-based attack. While tools like Hydra make launching an attack accessible, the core of the project was the **Blue Team** perspective—identifying relevant logs and interpreting forensic telemetry. 

### Key Takeaways:
* **Analysis of Telemetry:** Developed the ability to filter through baseline Windows events to isolate high-severity security alerts.
* **Defensive Controls:** Observed the effectiveness of Network Level Authentication (NLA) as a primary defense against automated RDP attacks.
* **Forensic Markers:** Identified that markers such as `Logon Type` and `Status Codes` are essential for confirming the intent and origin of an adversary.
