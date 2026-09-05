# 🛡️ Wazuh SOC Labs

### Hands-On Security Monitoring, Detection, Investigation & Response

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue)
![Focus](https://img.shields.io/badge/Focus-SOC%20%7C%20Blue%20Team-red)
![Suricata](https://img.shields.io/badge/Network%20IDS-Suricata-orange)
![Auditd](https://img.shields.io/badge/Linux-Auditd-yellow)
![VirusTotal](https://img.shields.io/badge/Threat%20Intel-VirusTotal-green)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-Mapped-red)
![Docker](https://img.shields.io/badge/Environment-Docker-blue)
![Kali Linux](https://img.shields.io/badge/Endpoint-Kali%20Linux-black)

A hands-on cybersecurity lab portfolio focused on building practical SOC and Blue Team capabilities using **Wazuh**.

This repository documents a progression from endpoint visibility and security monitoring to detection engineering, threat hunting, threat intelligence, incident response, and automated containment.

---

## 🎯 What This Repository Demonstrates

- Security monitoring with Wazuh SIEM
- File Integrity Monitoring (FIM)
- Network intrusion detection with Suricata
- Linux endpoint monitoring with Auditd
- Vulnerability detection and analysis
- Detection rule development
- SSH brute-force detection
- Event correlation
- Threat hunting
- Threat intelligence enrichment
- MITRE ATT&CK mapping
- Security alert investigation and triage
- Automated incident response and containment

---

## 🧪 Labs

### 🔹 Lab 1 — File Integrity Monitoring
Endpoint file monitoring and threat hunting using Wazuh FIM.

**Key Skills:** FIM · Threat Hunting · Endpoint Monitoring

📄 [View Lab 1 — File Integrity Monitoring](./Lab-1-File-Integrity-Monitoring.md)

### 🔹 Lab 2 — Suricata IDS + Wazuh
Network security monitoring by integrating Suricata IDS telemetry with Wazuh.

**Key Skills:** Network Monitoring · IDS · Traffic Analysis

📄 [View Lab 2 — Suricata IDS + Wazuh](./Lab-2-Suricata-IDS-Wazuh.md)

### 🔹 Lab 3 — Wazuh Vulnerability Detection
Identifying and analyzing endpoint vulnerabilities through Wazuh.

**Key Skills:** Vulnerability Management · CVE Analysis · Security Assessment

📄 [View Lab 3 — Vulnerability Detection](./Lab-3-Vulnerability-Detection.md)

### 🔹 Lab 4 — Auditd Linux Endpoint Detection
Linux command and activity monitoring using Auditd and Wazuh detection rules.

**Key Skills:** Linux Security · Auditd · Detection Engineering · Endpoint Monitoring

📄 [View Lab 4 — Auditd Linux Endpoint Detection](./Lab-4-Auditd-Linux-Endpoint-Detection.md)

### 🔹 Lab 5 — SSH Brute Force & Automated Response
Detecting SSH brute-force activity, correlating authentication events, mapping activity to MITRE ATT&CK, and implementing automated containment.

**Key Skills:** Attack Detection · Event Correlation · MITRE ATT&CK · Active Response

📄 [View Lab 5 — SSH Brute Force & Automated Response](./Lab-5-SSH-Brute-Force-Automated-Response.md)

### 🔹 Lab 6 — FIM + VirusTotal Malware Detection
Combining file integrity monitoring with external threat intelligence to identify potentially malicious files and enrich security alerts.

**Key Skills:** FIM · Threat Intelligence · Malware Detection · API Integration

📄 [View Lab 6 — FIM + VirusTotal Malware Detection](./Lab-6-FIM-VirusTotal-Malware-Detection.md)

---

## 📈 SOC Skill Progression

```
Endpoint Visibility
        ↓
Security Monitoring
        ↓
Detection Engineering
        ↓
Threat Hunting
        ↓
Event Correlation
        ↓
Threat Intelligence
        ↓
Incident Response
        ↓
Automated Containment
```

The labs progressively demonstrate how security telemetry can be collected, analyzed, enriched, and turned into actionable detections and responses.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| **Wazuh** | SIEM, endpoint monitoring, detection and response |
| **Suricata** | Network intrusion detection and traffic monitoring |
| **Linux Auditd** | Linux command and security activity auditing |
| **Docker** | Containerized Wazuh deployment |
| **Kali Linux** | Security testing and monitored endpoint |
| **VirusTotal API** | Threat intelligence and malware analysis |
| **MITRE ATT&CK** | Adversary behavior and technique mapping |

**Core Technologies:** `Wazuh` · `Suricata` · `Auditd` · `Docker` · `Kali Linux` · `VirusTotal` · `MITRE ATT&CK`

---

## 🔐 Security Capabilities

| Capability | Covered |
|---|---|
| Endpoint Monitoring | ✅ |
| File Integrity Monitoring | ✅ |
| Network Intrusion Detection | ✅ |
| Vulnerability Detection | ✅ |
| Linux Auditing | ✅ |
| Detection Engineering | ✅ |
| Threat Hunting | ✅ |
| Event Correlation | ✅ |
| Threat Intelligence | ✅ |
| MITRE ATT&CK Mapping | ✅ |
| Alert Investigation | ✅ |
| Brute Force Detection | ✅ |
| Automated Response | ✅ |

---

## 🏗️ Lab Environment

The lab environment uses a containerized Wazuh deployment with a Linux security-testing endpoint.

```
                         ┌──────────────────────┐
                         │   Wazuh Dashboard     │
                         │ Monitoring & Hunting  │
                         └──────────┬────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │    Wazuh Manager      │
                         │ Analysis & Detection  │
                         └──────────┬────────────┘
                                    │
                   ┌────────────────┴────────────────┐
                   │                                  │
          ┌────────▼────────┐               ┌─────────▼────────┐
          │  Kali Endpoint  │               │ Security Events  │
          │   Wazuh Agent   │               │ FIM / Auditd /   │
          │                 │               │ Network Telemetry│
          └────────┬────────┘               └──────────────────┘
                   │
          ┌────────┼─────────┐
          │        │         │
        FIM     Auditd   Suricata
          │        │         │
          └────────┴─────────┘
                   │
                   ▼
          ┌────────────────────┐
          │   VirusTotal API   │
          │ Threat Intelligence│
          └────────────────────┘
```

---

## 🧠 SOC Investigation Approach

The labs follow a practical SOC workflow:

```
Detect → Validate → Investigate → Correlate → Enrich → Respond
```

The objective is not simply to generate alerts, but to understand how a SOC analyst moves from raw security telemetry to an actionable security decision.

---

## 📂 Repository Structure

```
wazuh-soc-labs/
│
├── README.md
│
├── Lab-1-File-Integrity-Monitoring.md
├── Lab-2-Suricata-IDS-Wazuh.md
├── Lab-3-Vulnerability-Detection.md
├── Lab-4-Auditd-Linux-Endpoint-Detection.md
├── Lab-5-SSH-Brute-Force-Automated-Response.md
└── Lab-6-FIM-VirusTotal-Malware-Detection.md
```

Each lab contains its own technical documentation, configuration, commands, detection logic, investigation process, evidence, results, and SOC relevance.

---

## 📚 Learning Progression

This repository represents a practical progression toward SOC L1 and Blue Team capabilities.

```
Visibility → Detection → Investigation → Correlation → Threat Intelligence → Response
```

Each stage builds on the previous one to develop a broader understanding of security operations and defensive workflows.

---

## 🎯 Project Goals

The purpose of this repository is to demonstrate practical experience with:

- Building and operating a security monitoring environment
- Collecting endpoint and network telemetry
- Creating and tuning security detections
- Investigating security alerts
- Understanding attacker behavior
- Enriching detections with threat intelligence
- Applying MITRE ATT&CK concepts
- Automating defensive actions
- Documenting investigations in a reproducible manner

---

## ⚠️ Security & Privacy

This repository is intended for **educational and defensive security research**.

Any credentials, API keys, tokens, or other secrets used during the labs should be replaced with placeholders before publishing. For example:

```
<VIRUSTOTAL_API_KEY>
```


---

## 👨‍💻 Author

**Josh Yadav**

---

## ⭐ Explore the Labs

If you're reviewing this repository, start with **Lab 1 — File Integrity Monitoring** and follow the progression through **Lab 6 — FIM + VirusTotal Malware Detection**.

Each lab builds on the previous one to demonstrate a broader security monitoring, detection, investigation, and response capability.

### 🛡️ From Detection to Response

```
Monitor → Detect → Investigate → Enrich → Respond
```
