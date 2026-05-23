#### SIEM-QRadar




#### Architecture

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
