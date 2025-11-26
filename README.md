# 🛡️ Suricata + Splunk Threat Detection Lab

A practical, hands-on cybersecurity project demonstrating how Suricata IDS, Splunk Enterprise, and UFW firewall logs work together to detect, analyze, and respond to hostile network activity such as port scanning.

This lab simulates a mini SOC detection pipeline where logs from multiple sources are correlated to identify malicious behavior.

---

## 🎯 Objective

Build an end-to-end detection workflow that:

- Deploys **Suricata IDS** on a monitored Linux system  
- Forwards **Suricata eve.json logs** into Splunk  
- Generates hostile traffic using **Nmap**  
- Blocks the attacker using **UFW firewall rules**  
- Validates detections using **Splunk correlation searches**

---

## 🧩 Lab Environment

| Component | Description |
|----------|-------------|
| **IDS** | Suricata |
| **SIEM** | Splunk Enterprise |
| **Firewall** | UFW (Ubuntu) |
| **Attacker Machine** | 192.168.204.129 |
| **Monitored Host** | akshay2 |

---

## ⚙️ Step 1: Installing & Configuring Suricata

Suricata logs all network detection events into:

/var/log/suricata/eve.json

shell
Copy code

This file includes alerts for scans, anomalies, traffic signatures, and suspicious behavior.

### 🔍 Splunk Verification Query

Used to confirm Suricata logs are being ingested correctly:

host=akshay2 source="/var/log/suricata/eve.json" src_ip="192.168.204.129"

yaml
Copy code

➡️ Suricata successfully detected Nmap scans coming from the attacker system.

---

## ⚙️ Step 2: Blocking the Attacker with UFW

Once malicious traffic is detected, UFW can be used to block the attacker.

### 🔹 Block the IP

sudo ufw deny from 192.168.204.129

graphql
Copy code

### 🔹 Monitor UFW Logs Live

tail -f /var/log/ufw.log

graphql
Copy code

#### Example UFW Block Event

[UFW BLOCK] SRC=192.168.204.129 DPT=34197 PROTO=TCP FLAGS=PSH,FIN,URG

yaml
Copy code

---

## ⚙️ Step 3: Forwarding UFW Logs into Splunk

Ingesting UFW logs enables correlation between firewall blocks and IDS alerts.

### 🔍 Splunk Search Query for UFW Logs

host=akshay2 source="/var/log/ufw.log" SRC="192.168.204.129"

yaml
Copy code

➡️ Splunk successfully captured firewall block events, showing timeline correlation with Suricata alerts.

---

# 📸 Screenshots

### 🛡️ Suricata Alerts in Splunk
![Suricata Alerts](./screenshots/Screenshot%201.png)

### 🔥 UFW Firewall Block Logs
![Nmap Scan](./screenshots/Screenshot%203.png)

### 📡 Nmap Scan Detection
![UFW Logs](./screenshots/Screenshot%202.png)

### 📊 Splunk Correlated Events
![Splunk Events](./screenshots/Screenshot%204.png)

---

## 🏁 Conclusion

This lab demonstrates practical cyber defense techniques:

- Detecting reconnaissance attempts using **Suricata IDS**
- Visualizing and correlating events inside **Splunk SIEM**
- Enforcing IP blocking and traffic control using **UFW**
- Building detection engineering skills through log analysis

This workflow is commonly used in SOC environments and helps build blue-team proficiency.

---

## 🔖 Tags

#Suricata #Splunk #UFW #Nmap  
#IDS #SIEM #CyberSecurity #BlueTeam #DetectionEngineering
