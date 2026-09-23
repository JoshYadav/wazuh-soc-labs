# 🌐 Lab 2 --- Suricata IDS Logs Through Wazuh

### Detecting, Investigating and Understanding Network Activity Through an IDS and SIEM

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-green)
![Focus](https://img.shields.io/badge/Focus-SOC%20%7C%20Blue%20Team-blue)
![Suricata](https://img.shields.io/badge/Security-Suricata-red)
![Network](https://img.shields.io/badge/Monitoring-Network%20Security-blue)
![IDS](https://img.shields.io/badge/Detection-IDS-orange)
![Threat Hunting](https://img.shields.io/badge/Capability-Threat%20Hunting-purple)

> **Lab objective:** Build practical experience with Suricata network intrusion detection and Wazuh SIEM investigation by generating controlled ICMP and Nmap traffic, observing the resulting security telemetry, identifying the rules and signatures responsible for the alerts, and analyzing the network activity from a SOC analyst perspective.

------------------------------------------------------------------------

## 📌 Overview

A network packet is only raw activity until security tooling gives it context.

In this lab, I worked with **Suricata IDS and Wazuh** to understand how network traffic becomes security telemetry that can be investigated by a SOC analyst.

The practical workflow was:

```text
Network Activity
      ↓
Suricata IDS
      ↓
Suricata JSON / EVE Logs
      ↓
Wazuh Decoder / Rules
      ↓
Wazuh Security Alerts
      ↓
Threat Hunting / Investigation
      ↓
Network Activity Analyzed
```

The lab focused on two controlled scenarios:

- **ICMP traffic** detected using a custom Suricata rule.
- **Nmap-generated network activity** used to observe TCP SYN scanning behavior.

The important part was not simply generating alerts. The goal was to understand the relationship between the original network activity, the Suricata detection, the structured log, the Wazuh alert, and the investigation context.

------------------------------------------------------------------------

## 🎯 Objectives

The main objectives of this lab were to:

- Understand the purpose of Suricata as a Network Intrusion Detection System.
- Understand how network traffic is converted into IDS telemetry.
- Create and test a custom Suricata rule for ICMP traffic.
- Generate controlled ICMP activity.
- Forward Suricata events into Wazuh.
- Investigate Suricata alerts using Wazuh Threat Hunting.
- Generate controlled Nmap network activity.
- Analyze Nmap SYN-based activity from an IDS perspective.
- Inspect Suricata JSON/EVE log fields.
- Identify Suricata signature IDs and Wazuh rule IDs.
- Analyze source and destination IP addresses.
- Analyze source and destination ports.
- Analyze protocols, timestamps, severity and alert actions.
- Understand how repeated network events provide behavioral context.
- Practice a SOC-style network alert investigation workflow.

------------------------------------------------------------------------

## 🧪 Lab Scenario

The lab used controlled network activity to test the complete IDS-to-SIEM detection pipeline.

The first stage used ICMP traffic to verify that a custom Suricata detection rule could identify the expected network activity.

The second stage used Nmap-generated traffic to observe how reconnaissance-style TCP SYN activity appeared in Suricata logs and Wazuh alerts.

The purpose was to understand how a SOC analyst can move from:

```text
Network Traffic
      ↓
IDS Detection
      ↓
Structured Telemetry
      ↓
SIEM Alert
      ↓
Investigation
```

All activity was performed in a controlled practice environment.

------------------------------------------------------------------------

## 🖥️ Lab Environment

| Component | Role |
|---|---|
| **Suricata** | Network Intrusion Detection System |
| **Wazuh Agent** | Collects and forwards security telemetry |
| **Wazuh Manager** | Processes events and generates Wazuh alerts |
| **Wazuh Dashboard / Threat Hunting** | Used to search and investigate alerts |
| **Nmap** | Used to generate controlled network scanning activity |
| **Suricata EVE / JSON Logs** | Structured source of IDS telemetry |
| **Linux / WSL Lab Environment** | Controlled environment for the exercise |

### High-Level Architecture

```text
┌──────────────────────────────┐
│       Network Activity       │
│                              │
│   ICMP Traffic / Nmap Scan   │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│         Suricata IDS         │
│                              │
│  Traffic Inspection          │
│  Signature Matching          │
│  Alert Generation            │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       EVE JSON Logs          │
│                              │
│  IPs / Ports / Protocols     │
│  Signatures / Severity       │
│  Flow Information            │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│           Wazuh              │
│                              │
│  Decoder / Rules             │
│  Alert Processing            │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│   Threat Hunting / Events    │
│                              │
│  Search / Filtering          │
│  Alert Investigation         │
└──────────────────────────────┘
```

------------------------------------------------------------------------

## 🔎 What is Suricata?

Suricata is an open-source network security monitoring engine that can inspect network traffic and generate alerts based on detection signatures and protocol information.

For this lab:

```text
Network Traffic
      ↓
Suricata Inspection
      ↓
Signature Match
      ↓
Alert
      ↓
JSON / EVE Event
```

A Suricata event can provide useful investigation fields such as:

- Timestamp
- Source IP
- Source port
- Destination IP
- Destination port
- Protocol
- Signature ID
- Signature message
- Severity
- Action
- Flow information

These fields provide the network context needed when investigating an IDS alert.

------------------------------------------------------------------------

## ⚙️ Suricata Detection Concept

Suricata uses detection signatures to identify traffic matching defined conditions.

For the ICMP test, I created a controlled detection rule.

The detection path was:

```text
ICMP Packet
     ↓
Suricata Inspects Traffic
     ↓
Custom Signature Matches
     ↓
Suricata Generates Alert
     ↓
Alert Written to JSON / EVE Log
     ↓
Wazuh Processes the Event
```

This provided a controlled way to validate the detection pipeline before moving to Nmap-generated traffic.

------------------------------------------------------------------------

## 📡 ICMP Traffic Detection

ICMP traffic was used as the first test case.

ICMP is commonly associated with utilities such as `ping`, but it can also provide useful security telemetry for:

- Host discovery
- Reachability testing
- Network troubleshooting
- Repeated probing
- Potential reconnaissance

The lab used controlled ICMP traffic so the expected behavior was known before investigating the resulting alerts.

The important SOC lesson is that **detecting ICMP traffic does not automatically mean the traffic is malicious**. Context is required.

------------------------------------------------------------------------

## 📊 Evidence --- Suricata Activity in Wazuh

The following screenshot shows the Wazuh Threat Hunting environment with Suricata-generated security telemetry.

![Suricata Wazuh Dashboard](./01-suricata-wazuh-dashboard.jpg)

### What this demonstrates

The captured environment shows:

```text
Agent:      Agent0
Agent ID:   004
Rule ID:    86601
Rule Level: 3
```

The dashboard also shows activity associated with the `ids` and `suricata` alert groups.

A dashboard is useful as an investigation starting point, but the analyst normally needs to move into individual events to understand exactly what network activity generated the alert.

------------------------------------------------------------------------

## 🔍 Investigating ICMP Events

After generating the controlled ICMP traffic, I narrowed the investigation to the resulting Suricata events inside Wazuh.

![ICMP Suricata Events in Wazuh](./02-icmp-wazuh-events.jpg)

The captured event information includes:

```text
Agent:       Agent0
Agent ID:    004
Rule ID:     86601
Rule Level:  3
```

The event description was:

```text
Suricata: Alert - LOCAL TEST - ICMP detected
```

This demonstrates the relationship:

```text
ICMP Traffic
     ↓
Suricata Detection
     ↓
Suricata Event
     ↓
Wazuh Processing
     ↓
Rule 86601
     ↓
Threat Hunting Event
```

------------------------------------------------------------------------

## 🧭 Moving From ICMP to Nmap Activity

After validating the ICMP detection path, I moved to Nmap-generated network activity.

Nmap can be used legitimately for network discovery, asset inventory, security testing and vulnerability assessment. Its traffic is also useful for learning how reconnaissance-style behavior appears in network telemetry.

The investigation therefore shifted from:

```text
Can Suricata detect the traffic?
```

to:

```text
What does the traffic look like from an IDS perspective?
```

The presence of scanning activity alone does not establish malicious intent.

------------------------------------------------------------------------

## 🚨 Evidence --- Nmap Detection in Wazuh

The Nmap-generated activity produced Suricata alerts that were visible through Wazuh.

![Nmap Suricata Alerts in Wazuh](./03-nmap-wazuh-security-alerts.jpg)

The captured evidence shows:

```text
Agent:       ubuntu-wsl
Agent ID:    002
Rule ID:     86601
Rule Level:  3
```

The alert description was:

```text
Suricata: Alert - LOCAL TEST - Nmap SYN detected
```

The Wazuh view shows repeated detections associated with the Nmap activity.

Repeated events can reveal behavior that is not obvious from a single packet:

```text
Connection Attempt
       ↓
Additional Connection Attempts
       ↓
Multiple Destination Ports
       ↓
Repeated IDS Alerts
       ↓
Possible Reconnaissance Pattern
```

The final interpretation still depends on the environment and whether the activity was authorized.

------------------------------------------------------------------------

## 🧾 Evidence --- Raw Suricata JSON Logs

The next step was to inspect the underlying Suricata JSON telemetry.

![Raw Suricata Nmap JSON Logs](./04-suricata-nmap-json-logs.jpg)

The raw events contained fields including:

```text
timestamp
src_ip
src_port
dest_ip
dest_port
proto
event_type
alert.action
alert.gid
alert.signature_id
alert.rev
alert.signature
alert.severity
flow
```

One captured event contained:

```text
signature_id: 1000002
signature:     LOCAL TEST - Nmap SYN detected
severity:      3
action:        allowed
```

The raw telemetry also showed different destination ports for the generated connection attempts.

This demonstrates why structured IDS logs are valuable: they contain much more investigation context than a short alert message.

------------------------------------------------------------------------

## 🔬 Investigating the Network Fields

### Source IP

The captured evidence contained:

```text
src_ip: 172.17.0.1
```

The source address helps identify where the observed activity originated.

### Destination IP

The captured evidence contained:

```text
dest_ip: 172.17.15.9
```

This establishes the basic relationship:

```text
Source → Destination
```

### Source and Destination Ports

One captured event showed:

```text
src_port:  44379
dest_port: 79
```

Other events contained different destination ports, providing additional context for the scan-like activity.

### Protocol

The captured Nmap events used:

```text
proto: TCP
```

Protocol information is important because the meaning of an event depends heavily on whether it involves TCP, UDP, ICMP or another protocol.

------------------------------------------------------------------------

## 🆔 Suricata Signature ID vs Wazuh Rule ID

One important lesson from this lab was that the Suricata signature identifier and the Wazuh rule identifier belong to different layers.

| Identifier | Meaning |
|---|---|
| `signature_id` | Identifies the Suricata detection signature |
| `rule.id` | Identifies the Wazuh rule associated with the processed event |

For the captured Nmap activity:

```text
Suricata signature_id:
1000002

Wazuh rule.id:
86601
```

The Suricata signature was:

```text
LOCAL TEST - Nmap SYN detected
```

Understanding this relationship is useful when tracing an alert from raw network telemetry into a SIEM.

------------------------------------------------------------------------

## ⚠️ Severity and Alert Action

The captured Suricata event showed:

```text
severity: 3
```

and the Wazuh event showed:

```text
rule.level: 3
```

The raw Suricata event also showed:

```text
action: allowed
```

This means the traffic was observed and generated IDS telemetry; the captured event does not indicate that the traffic was automatically blocked.

Therefore:

```text
IDS Detection
      ≠
Automatic Prevention
```

Severity should also be interpreted with context such as:

- Frequency
- Target asset
- Source
- Authorization
- Timing
- Destination ports
- Related alerts
- Other endpoint or network telemetry

------------------------------------------------------------------------

## 🕵️ Network Alert Investigation Workflow

The investigation performed in this lab can be represented as:

```text
1. Observe the IDS alert
        ↓
2. Identify the affected agent / endpoint
        ↓
3. Identify source and destination IPs
        ↓
4. Identify protocol and ports
        ↓
5. Identify the Suricata signature
        ↓
6. Identify the Wazuh rule
        ↓
7. Review severity and alert action
        ↓
8. Check repeated or surrounding events
        ↓
9. Determine whether the activity is expected
        ↓
10. Correlate with other telemetry if required
```

The goal is to move from an alert to an evidence-based understanding of the activity.

------------------------------------------------------------------------

## 📋 Evidence Summary

| Evidence | Observation |
|---|---|
| IDS | Suricata |
| SIEM | Wazuh |
| ICMP detection | Custom Suricata ICMP test |
| ICMP Wazuh rule | `86601` |
| ICMP rule level | `3` |
| ICMP alert | `Suricata: Alert - LOCAL TEST - ICMP detected` |
| Nmap detection | Custom Suricata Nmap SYN test |
| Nmap signature ID | `1000002` |
| Nmap signature | `LOCAL TEST - Nmap SYN detected` |
| Nmap severity | `3` |
| Nmap action | `allowed` |
| Nmap Wazuh rule | `86601` |
| Network protocol | TCP |
| Example source IP | `172.17.0.1` |
| Example destination IP | `172.17.15.9` |
| Investigation interface | Wazuh Threat Hunting / Events |
| Raw telemetry | Suricata JSON / EVE data |

------------------------------------------------------------------------

## 🧠 What I Learned

This lab connected several concepts that are often learned separately.

The practical flow was:

```text
Generate Network Traffic
        ↓
Suricata Inspects Traffic
        ↓
Detection Signature Matches
        ↓
Suricata Writes Structured Event
        ↓
Wazuh Processes the Event
        ↓
Wazuh Generates Alert
        ↓
Analyst Searches the Event
        ↓
Network Fields Are Investigated
        ↓
Activity Is Given Context
```

The key lesson was that an IDS alert is not the final answer.

The analyst needs to understand:

```text
What happened?
        ↓
Who generated it?
        ↓
Who received it?
        ↓
What protocol and ports were involved?
        ↓
Which detection triggered?
        ↓
Was the activity expected?
        ↓
Are there related events?
```

------------------------------------------------------------------------

## 🚨 SOC Relevance

This lab is relevant to several SOC and Blue Team activities.

### Network Security Monitoring

Suricata provides visibility into network activity and can identify traffic matching detection signatures.

### IDS Alert Triage

A SOC analyst needs to determine what an IDS alert represents and whether it requires further investigation.

### SIEM Investigation

Wazuh provides centralized visibility into the alerts and allows the analyst to search and filter the collected telemetry.

### Detection Engineering

Creating controlled Suricata rules demonstrated how detection logic can be designed and tested.

### Threat Hunting

Searching for specific rule IDs, signatures, IP addresses, ports and time ranges provides a foundation for network-focused threat hunting.

### Network Reconnaissance Analysis

The Nmap activity provided a practical example of how scan-like behavior can appear in IDS telemetry.

------------------------------------------------------------------------

## 🔗 Network Detection + SIEM Correlation

Suricata becomes more useful when its telemetry is connected to a SIEM.

| Data Source | What It Adds |
|---|---|
| **Suricata** | Network detection and signature context |
| **Wazuh** | Centralized alert processing and investigation |
| **Endpoint Telemetry** | Host-side activity related to the network event |
| **Authentication Logs** | User or account activity around the same time |
| **Firewall Logs** | Network enforcement and connection decisions |
| **DNS Logs** | Domain-resolution context |
| **Threat Intelligence** | Reputation/context for IPs, domains or other indicators |

A network alert becomes more useful when it can be correlated with other security telemetry.

------------------------------------------------------------------------

## ⚠️ False Positives and Analyst Context

Not every IDS alert represents an attack.

Legitimate reasons for network scanning or probing can include:

- Security assessments
- Vulnerability scanning
- Network administration
- Asset discovery
- Troubleshooting
- Monitoring systems

A SOC analyst should therefore avoid treating the alert alone as proof of compromise.

A better workflow is:

```text
Alert
  ↓
Validate
  ↓
Understand Context
  ↓
Correlate
  ↓
Determine Risk
  ↓
Escalate / Close
```

The same network pattern can have different meanings depending on the environment and authorization.

------------------------------------------------------------------------

## 🧩 Detection vs Investigation

### Detection

Suricata identifies network activity that matches a detection signature.

Example:

```text
LOCAL TEST - Nmap SYN detected
```

### SIEM Alerting

Wazuh processes the Suricata event and represents it through its own alert/rule framework.

Example:

```text
Rule ID: 86601
Rule Level: 3
```

### Investigation

The analyst determines what the activity means by examining:

```text
Source
Destination
Protocol
Ports
Signature
Timestamp
Severity
Action
Frequency
Related Events
Context
```

This distinction between **detecting an event** and **understanding the event** is central to SOC work.

------------------------------------------------------------------------

## 📌 Key Takeaways

### 1. Suricata provides network visibility

It can inspect traffic and generate structured security events when detection signatures match.

### 2. Wazuh turns network telemetry into SIEM-searchable alerts

The integration makes Suricata events easier to search, filter and investigate centrally.

### 3. Raw JSON contains valuable investigation context

Fields such as IPs, ports, protocol, signature ID, severity and action help reconstruct what happened.

### 4. Suricata and Wazuh identifiers are different

The `signature_id` identifies the Suricata detection, while `rule.id` identifies the Wazuh rule associated with the alert.

### 5. Repeated events provide behavioral context

Multiple connection attempts across different ports can be more informative than an isolated network event.

### 6. Detection does not automatically mean malicious activity

Authorization, asset, timing and surrounding-event context matter.

### 7. Detection is different from prevention

The captured event showed `action: allowed`, demonstrating that observing traffic and blocking traffic are separate capabilities.

------------------------------------------------------------------------

## 🧪 Lab Outcome

The lab successfully demonstrated the complete basic network IDS investigation cycle:

```text
Controlled Network Activity
        ↓
Suricata Detection
        ↓
Structured JSON / EVE Event
        ↓
Wazuh Processing
        ↓
Wazuh Alert
        ↓
Threat Hunting
        ↓
Network Investigation
        ↓
Contextual Analysis
```

The practical takeaway was:

> **One packet can tell a story. The challenge is knowing how to read it.**

------------------------------------------------------------------------

## 🔐 Security Note

All activity in this lab was performed in a controlled practice environment for defensive security learning.

The ICMP and Nmap traffic were generated intentionally to test the detection pipeline. The purpose was to understand network telemetry, IDS signatures, SIEM alerting and SOC investigation techniques.

------------------------------------------------------------------------

## 🔗 Related Work

This lab is **Lab 2** in the Wazuh SOC Labs repository and builds on the endpoint-monitoring foundation established in Lab 1.

**Previous:** [Lab 1 --- File Integrity Monitoring](./Lab-1-File-Integrity-Monitoring.md)

**Next:** [Lab 3 --- Vulnerability Detection](./Lab-3-Vulnerability-Detection.md)

------------------------------------------------------------------------

## 👨‍💻 Author

**Josh Yadav**

------------------------------------------------------------------------

### 🛡️ Detect → Investigate → Understand → Respond
