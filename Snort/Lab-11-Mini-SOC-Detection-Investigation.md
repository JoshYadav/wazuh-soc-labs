# 🛡️ Lab 11 --- Mini SOC Detection & Investigation
### Combining Snort Detection, PCAP Analysis, IOC Extraction and Wazuh Correlation

> Lab objective: Combine the practical Snort and Wazuh skills from the previous labs into a single SOC-style investigation workflow using normal HTTP traffic, suspicious HTTP traffic and controlled network reconnaissance.

---

## 📌 Overview

Lab 11 was designed as the final consolidation exercise rather than another isolated Snort feature.

The goal was to demonstrate the full defensive workflow:

```text
Detect
  ↓
Triage
  ↓
Inspect PCAP
  ↓
Extract IOCs
  ↓
Validate Evidence
  ↓
Correlate in Wazuh
  ↓
Analyst Assessment
```

![SOC investigation workflow](images/overview/soc-investigation-workflow.png)

---

## 🎯 Objectives

- Establish a benign HTTP baseline.
- Detect suspicious HTTP activity.
- Detect controlled port-scan activity.
- Validate alerts against PCAP evidence.
- Extract useful observables.
- Correlate the HTTP detection in Wazuh.
- Produce an analyst-style case assessment.

---

## 🖥️ Environment

| Component | Value |
|---|---|
| Snort | 3.12.2.0 |
| Wazuh | 4.14.6 |
| OS | Ubuntu 26.04 LTS on WSL2 |
| Target | `172.24.112.74` |
| HTTP service | `172.24.112.74:8080` |

---

## 🔎 Scenario 1 — Normal HTTP Baseline

The normal HTTP PCAP was replayed using the HTTP detection rule.

```bash
snort -c /usr/local/etc/snort/snort.lua -R ~/http.rules -r ~/snort-http.pcap -A alert_fast
```

Result:

- Packets received/analyzed: `50`
- HTTP requests: `5`
- Alerts: `0`

This provided a benign baseline for comparison with the suspicious request.

---

## 🚨 Scenario 2 — Suspicious HTTP Detection

The suspicious PCAP was replayed using the same detection rule:

```bash
snort -c /usr/local/etc/snort/snort.lua -R ~/http.rules -r ~/snort-http-suspicious.pcap -A alert_fast
```

The alert was:

```text
[1:1000008:1] "LAB - Suspicious HTTP passwd Path"
```

Observed source and destination:

```text
172.24.112.1:59134 → 172.24.112.74:8080
```

The request was:

```text
GET /etc/passwd HTTP/1.1
```

Snort analyzed 10 packets and generated 1 alert.

![Suspicious HTTP PCAP](images/lab-09/suspicious-http-pcap.png)

---

## 🔎 Scenario 2 — Wazuh Correlation

The same Snort detection was forwarded into Wazuh and classified by custom rule `100900`.

The final Wazuh event contained:

```text
Rule ID: 100900
Level: 10
Description: Snort: Suspicious HTTP passwd path detected
Snort rule: 1:1000008:1
Source: 172.24.112.1:59134
Destination: 172.24.112.74:8080
```

The Wazuh evidence from the integration is retained in the project:

![Wazuh event](images/lab-10/wazuh-snort-event.png)

---

## 🔍 Scenario 3 — Port Scan / Reconnaissance

The controlled port-scan PCAP was replayed with Snort's built-in port-scan detection enabled.

Result:

- Rules loaded: `866`
- Packets analyzed: `21`
- TCP sessions: `8`
- SYNs: `8`
- Alerts: `1`

![Port-scan detection](images/lab-07/snort-portscan-detection.png)

The packet-level evidence showed SYN probes to multiple TCP ports on the target.

![Port-scan packet analysis](images/lab-07/wireshark-syn-scan.png)

The activity is consistent with network reconnaissance. It does not by itself prove compromise.

---

## 📋 SOC Case Summary

| Scenario | Evidence | Result | Analyst Interpretation |
|---|---|---|---|
| Normal HTTP | 50-packet PCAP | 0 alerts | Benign baseline |
| Suspicious HTTP | `/etc/passwd` request | 1 alert | Suspicious attempted access |
| Port scan | Multiple SYN probes | 1 alert | Network reconnaissance |

A complete case report is available at:

`reports/lab11-soc-case-report.md`

---

## 🧠 Analyst Assessment

The lab demonstrates three important SOC principles:

### 1. Detection needs context

A rule match alone does not establish intent or impact.

### 2. PCAP evidence matters

The underlying packets can confirm what traffic actually occurred and what response was returned.

### 3. Centralized correlation improves visibility

Wazuh provides a centralized place to search and investigate the Snort-derived alert.

---

## 🧭 Complete Investigation Workflow

![Complete SOC workflow](images/overview/soc-investigation-workflow.png)

```text
Network Traffic
      ↓
Snort Detection
      ↓
Alert
      ↓
Triage
      ↓
PCAP Inspection
      ↓
IOC Extraction
      ↓
Evidence Validation
      ↓
Wazuh Correlation
      ↓
Analyst Assessment
```

---

## 🧠 What I Learned

This final lab connected the individual skills into one workflow:

```text
Detect → Investigate → Validate → Correlate → Assess
```

The most important takeaway was that an alert is an investigation starting point, not automatic proof of compromise.

---

## 🚨 SOC Relevance

This lab represents a simplified SOC investigation cycle in which a network detection becomes a centralized security event and is then validated against raw network evidence.

It demonstrates practical familiarity with:

- Network detection
- Alert triage
- PCAP investigation
- IOC extraction
- Detection validation
- SIEM correlation
- Analyst documentation

---

## 🎤 Interview Summary

> I built a practical Snort 3 NIDS lab and progressed from writing basic detection rules to network-aware detection, threshold tuning, TCP and port-scan detection, HTTP inspection, PCAP-based IOC investigation, and finally integrating Snort with Wazuh. I also built a small SOC investigation workflow where I compared benign and suspicious traffic and validated alerts against the underlying PCAP.

---

## 🧪 Lab Outcome

All 11 Snort labs were completed.

The final workflow connected network traffic to detection, investigation and centralized SIEM visibility:

```text
Network Traffic
      ↓
     Snort
      ↓
   Detection
      ↓
   Evidence
      ↓
 Investigation
      ↓
    Wazuh
      ↓
 Centralized SOC Alert
```

---

## 🔐 Security Note

All activity was performed in a controlled local lab environment. The reconnaissance and HTTP request examples were directed only at the controlled WSL target used for the exercises.

---

## 🔗 Related Work

Previous: [Lab 10 --- Snort → Wazuh Integration](Lab-10-Snort-Wazuh-Integration.md)

---

## 👨‍💻 Author

Josh Yadav  
Computer Science Engineering Student  
Cybersecurity · SOC · Blue Team · SIEM · Incident Response
