# 🛡️ Splunk Windows 10 Security Log Forwarding Lab

![Splunk](https://img.shields.io/badge/Splunk-Enterprise_9.x-black?logo=splunk)
![Windows](https://img.shields.io/badge/OS-Windows_10-blue?logo=windows)
![Security](https://img.shields.io/badge/Cybersecurity-SIEM_%26_Log_Analysis-red)

## 📌 Description du Projet
Ce projet est un environnement de laboratoire (Lab) de cybersécurité simulant la centralisation et la surveillance des événements de sécurité Windows (`WinEventLog:Security`) à l'aide de **Splunk Enterprise** (PC Host) et **Splunk Universal Forwarder** (Machine Virtuelle Windows 10).

L'objectif principal est de capturer les tentatives d'authentification (échecs et réussites), les modifications de comptes et les événements critiques du système pour analyser les menaces potentielles en temps réel.

## 🛠️ Architecture & Stack Technique
- **Serveur SIEM Central (Host):** Windows 11 / Splunk Enterprise (`Port 9997`)
- **Agent d'Ingestion (VM Guest):** Windows 10 / Splunk Universal Forwarder
- **Hyperviseur:** Oracle VirtualBox
- **Protocole de Transport:** TCP 9997
- **Outils & Langages:** PowerShell, Windows Event Viewer, `wevtutil`, SPL (Splunk Processing Language)

---

## 🚀 Étapes de Déploiement

### 1. Configuration du Serveur Splunk (Host)
1. Activation du récepteur sur le port `9997` (*Settings ➡️ Forwarding and receiving ➡️ Add New*).
2. Autorisation du port sur le pare-feu Windows via PowerShell :
   ```powershell
   New-NetFirewallRule -DisplayName "Splunk Receiver 9997" -Direction Inbound -LocalPort 9997 -Protocol TCP -Action Allow

2. Configuration du Universal Forwarder (VM Windows 10)

1-Adjusting access permissions for security logs for the system account:
wevtutil sl Security /ca:O:BAG:SYD:(A;;0x1;;;SY)(A;;0x1;;;BA)(A;;0x1;;;S-1-5-32-573)

2-Deployment of source configuration in inputs.conf (C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf) :
Ini, TOML
[WinEventLog://Security]
disabled = 0
checkpointInterval = 5

3-Restarting the SplunkForwarder service:
net stop SplunkForwarder && net start SplunkForwarder

🧪 Simulation & Validation Tests (SOC Analysis)
To validate the real-time data flow:

Generate simulated authentication failures (Event ID 4625) by locking the VM (Win + L) and entering incorrect passwords.

Execute the monitoring query in Splunk Enterprise:
host="DESKTOP-F5D96A1" EventCode=4625

📈 Dashboard & Key Queries (SPL)
Refer to the `queries/security_searches.spl` file for the complete list of investigative queries (Failed Logins, Account Creation, Log Clearing).
