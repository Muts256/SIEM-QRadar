#### SIEM-QRadar


#### Security Lab Workflow (QRadar SIEM + Velociraptor EDR + Atomic Red Team)

This lab simulates real-world SOC operations by combining attack emulation, endpoint telemetry collection, SIEM analysis, and endpoint detection and response (EDR) capabilities.

#### Tools 
- QRadar (SIEM)
- Windows 2025 - Active Directory (with Sysmon Installed)
- Windows 10 client (with Sysmon installed)
- Ubuntu 24 - client 
- Ubuntu Velociraptor Server
- Ubuntu Attacker Device 
- Atomic Red Team Scripts


#### Architecture


![image alt](https://github.com/Muts256/SNC-Public/blob/a6bf071ae8bf9a684a8059e37433849fe47b6b49/Images/QRadar/Q1.png)


---

````mermaid
flowchart LR

    %% =========================
    %% ATTACK SIMULATION LAYER
    %% =========================
    subgraph Attack_Simulation["Attack Simulation Layer"]

        ART["Atomic Red Team
        Discovery + Privilege Escalation Tests"]

        ATTACKER["Ubuntu Attacker
        Python Brute Force Script"]

    end


    %% =========================
    %% ENDPOINTS
    %% =========================
    subgraph Endpoints["Endpoints / Victim Systems"]

        WINAD["Windows Server 2025
        Active Directory
        Sysmon Installed
        WinCollect Agent"]

        WIN10["Windows 10 Client
        Sysmon Installed
        WinCollect Agent"]

        UBUNTUCLIENT["Ubuntu 24 Client
        auditd Enabled"]

    end


    %% =========================
    %% DFIR / EDR
    %% =========================
    subgraph DFIR["DFIR / EDR Layer"]

        VELOCI["Ubuntu Velociraptor Server
        EDR / Evidence Collection
        Host Isolation"]

    end


    %% =========================
    %% SIEM
    %% =========================
    subgraph SIEM["Security Monitoring"]

        QRADAR["IBM QRadar SIEM
        Correlation Rules
        Alerting
        Log Analysis"]

    end


    %% =========================
    %% ATTACK FLOWS
    %% =========================
    ART --> WINAD
    ART --> WIN10

    ATTACKER -->|SSH Brute Force| UBUNTUCLIENT


    %% =========================
    %% LOG FLOWS
    %% =========================
    WINAD -->|Sysmon + WinCollect Logs| QRADAR
    WIN10 -->|Sysmon + WinCollect Logs| QRADAR

    UBUNTUCLIENT -->|auditd Logs| QRADAR


    %% =========================
    %% EDR / DFIR FLOWS
    %% =========================
    VELOCI -->|Collect Evidence| WINAD
    VELOCI -->|Collect Evidence| WIN10
    VELOCI -->|Collect Evidence| UBUNTUCLIENT

    VELOCI -->|Host Isolation Actions| WIN10
    VELOCI -->|Host Isolation Actions| WINAD


    %% =========================
    %% ALERTING / DETECTION
    %% =========================
    QRADAR -->|Detection Rules
    Discovery Activity
    Privilege Escalation
    Brute Force Detection| VELOCI


````





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

#### Why the tools
*IBM QRadar was chosen because it’s widely used in regulated industries like finance and healthcare, which makes it a realistic fit for enterprise SOC environments. It also works well in a mixed Windows and Linux setup, using WinCollect for Windows logs and standard syslog ingestion for Linux systems.*

*With Sysmon providing detailed endpoint telemetry, it becomes much easier to detect and analyse the Atomic Red Team simulations, especially when mapping activity to MITRE ATT&CK techniques. This gives the lab more realistic detection scenarios rather than just raw log collection.*

*On top of that, integrating Velociraptor as an EDR adds a practical incident response layer. It reflects how real SOC teams operate — where alerts from a SIEM lead to deeper endpoint investigations, evidence collection, and potential isolation of affected hosts.*

*Overall, the setup mirrors a real-world SOC workflow from detection through to response, using tools and processes that are common in enterprise environments*

















