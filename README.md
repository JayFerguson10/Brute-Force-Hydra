# 🛡️ Active Threat Detection: RDP Brute Force Simulation & SIEM Analysis

## 📝 Project Overview
The objective of this lab was to simulate a real-world cyber attack environment to develop practical threat detection and log analysis skills. By acting as both the adversary (Red Team) and the defender (Blue Team), this project demonstrates hands-on experience in endpoint security monitoring, generating telemetry, and hunting for specific Indicators of Compromise (IOCs) using a SIEM.

## 🛠️ Methodology
1. 🌐 **Built a Custom Network:** Configured a Windows 10 victim machine and a Kali Linux attacker machine on an isolated custom NAT network.
2. 🔓 **Configured Vulnerabilities:** Intentionally disabled Windows Network Level Authentication (NLA) to allow automated remote authentication attempts.
3. 💥 **Launched the Attack:** Executed a network-based RDP brute-force dictionary attack against the Windows `Employee` account using **Hydra**.
4. 🛡️ **Monitored the SIEM:** Utilized a **Wazuh** agent to ingest and forward endpoint security logs to the main dashboard for real-time analysis.

## 🔒 Security Note: Lab Isolation
A critical component of this lab was the use of a **Custom NAT Network**. I intentionally isolated these virtual machines from my physical home network and the public internet. 
* **The "Why":** Running brute-force tools and weakening security settings (like disabling NLA) creates risks. 
* **The Result:** This "Sandbox" approach allowed me to safely simulate aggressive attack traffic and generate realistic telemetry without exposing personal devices.

---

## 🐉 Phase 2: Network Brute Force Simulation 

**The Attack:**
I launched a brute-force dictionary attack against the `Employee` account over the network using Kali Linux.

![Kali Hydra Attack](Hydra attack.jpg)

**The Detection:**
Even though the machines were isolated, my Wazuh SIEM immediately caught the flood of network-based **Event ID 4625** (Failed Logon) alerts.

By drilling down into the raw forensic data, I successfully extracted the complete footprint of the attacker:
* 🔴 **Targeted Account:** `Employee`
* 🔴 **Logon Type:** `3` (Proving it was a remote network connection)
* 🔴 **Attacker IP Address:** `10.0.2.8`
* 🔴 **Attacker Hostname:** `kali`
* 🔴 **Status Code:** `0xc0000234` (Confirming the attack triggered the account lockout policy)

---

## 🧠 Lessons Learned & Summary
This project was a deep dive into the cat-and-mouse game of cybersecurity. I learned that while launching an attack is relatively simple with tools like Hydra, the real challenge lies in the **Blue Team** perspective—knowing which logs to look for and how to interpret them. 

**Key Takeaways:**
* **Signal vs. Noise:** I learned how to filter through thousands of baseline Windows events to find specific high-severity alerts.
* **Defensive Layers:** I discovered firsthand how much of a difference a single security setting like NLA makes in stopping automated attacks.
* **Forensic Evidence:** Understanding that an IP address is just the start; finding the `Logon Type` and `Status Codes` tells the full story of the adversary's intent.
