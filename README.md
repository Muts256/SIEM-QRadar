#### SIEM-QRadar


#### Security Lab Workflow (QRadar SIEM + Velociraptor EDR + Atomic Red Team)

This lab simulates real-world SOC operations by combining attack emulation, endpoint telemetry collection, SIEM analysis, and endpoint detection and response (EDR) capabilities.

#### Architecture


![image alt](https://github.com/Muts256/SNC-Public/blob/a6bf071ae8bf9a684a8059e37433849fe47b6b49/Images/QRadar/Q1.png)


---

#### 1. Attack Simulation (Adversary Layer)
An Ubuntu attacker machine executes scripted attacks:
- SSH brute-force attacks against Ubuntu clients using Python scripts
- Atomic Red Team execution on Windows endpoints to simulate:
  - Discovery techniques
  - Privilege escalation techniques

These actions generate realistic adversary behaviour based on MITRE ATT&CK techniques.

---

#### 2. Endpoint Telemetry Collection
Each endpoint generates logs based on activity:

- **Windows 2025 (Active Directory + Sysmon installed)**
  - Sysmon logs
  - Security event logs
  - Authentication and AD activity

- **Windows 10 Client (Sysmon installed)**
  - Process creation logs
  - Network connections
  - Security events

- **Ubuntu 24 Client**
  - auditd logs
  - Authentication logs (SSH login attempts)
  - System activity logs

---

#### 3. Log Forwarding to SIEM (QRadar)
Logs are centrally collected into IBM QRadar SIEM:

- Windows logs are forwarded using **WinCollect agents**
- Linux logs are forwarded using **auditd/syslog collection mechanisms**

QRadar processes the logs for:
- Event normalization (DSM parsing)
- Correlation of security events
- Detection of suspicious patterns (e.g., brute-force, privilege escalation)
- Alert generation and offense management

---

#### 4. SIEM Analysis (QRadar)
Within QRadar:
- Events are correlated across endpoints
- Security offenses are generated for suspicious activity
- Dashboards provide visibility into attack patterns
- Analysts investigate incidents using correlated log data

---

#### 5. Endpoint Detection & Response (Velociraptor)
The Velociraptor server provides EDR capabilities:
- Live endpoint monitoring
- Remote forensic collection (memory, files, logs)
- Endpoint isolation during active incidents
- Threat hunting queries across endpoints

---

#### 6. Investigation & Response Workflow
When an attack is detected:
1. QRadar identifies suspicious activity (e.g., brute-force or unusual process execution)
2. Alerts are raised as offenses
3. Velociraptor is used to:
   - Investigate affected endpoints
   - Collect forensic evidence
   - Isolate compromised machines if needed
  

---

#### Outcome
This environment demonstrates:
- SOC-style monitoring and alert triage
- Attack detection using SIEM correlation
- Endpoint forensics and response using EDR tools
- Realistic adversary simulation using Atomic Red Team

---




















