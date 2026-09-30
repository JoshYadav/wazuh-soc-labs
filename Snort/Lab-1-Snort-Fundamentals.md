# 🛡️ Lab 1 --- Snort Fundamentals
### Understanding Snort as a Network Intrusion Detection System

> Lab objective: Understand the role of Snort in network security monitoring, learn the basic Snort workflow, understand rules and alert generation, and establish the foundation for the practical detection labs that follow.

---

## 📌 Overview

Snort is a network security monitoring tool commonly used for intrusion detection and prevention. In this project, I used Snort 3 as a **Network Intrusion Detection System (NIDS)** to inspect network traffic and generate alerts when traffic matched detection logic.

The core workflow is:

```text
Network Traffic
      ↓
 Packet Inspection
      ↓
 Rule Matching
      ↓
 Detection Event
      ↓
 Alert / Log
      ↓
 Analyst Investigation
```

The goal of the lab was not simply to install Snort, but to understand how a network security alert is produced and how that alert can later become part of a SOC investigation.

---

## 🎯 Objectives

- Understand NIDS and NIPS concepts.
- Understand the purpose of Snort rules.
- Understand packet inspection and rule matching.
- Understand the relationship between traffic, detections and alerts.
- Prepare the environment for later PCAP and detection-engineering exercises.

---

## 🖥️ Lab Environment

| Component | Role |
|---|---|
| Ubuntu 26.04 LTS | Snort environment |
| WSL2 | Local Linux environment |
| Snort 3.12.2.0 | Network detection engine |
| tcpdump | Packet capture |
| Wireshark | Packet analysis |

Snort configuration used throughout the labs:

```text
/usr/local/etc/snort/snort.lua
```

---

## ⚙️ Snort Validation

The Snort configuration was validated using:

```bash
snort -c /usr/local/etc/snort/snort.lua -T
```

The validation completed successfully with no configuration warnings.

---

## 🧠 Snort Rule Concept

A basic Snort rule follows a structure similar to:

```text
alert <protocol> <source> <source-port> -> <destination> <destination-port> (<options>)
```

For example, a basic ICMP rule used later in the project was:

```text
alert icmp any any -> any any (msg:"LAB - ICMP Traffic Detected"; sid:1000001; rev:1;)
```

The rule contains:

- Action: `alert`
- Protocol: `icmp`
- Source: `any any`
- Direction: `->`
- Destination: `any any`
- Message: descriptive alert text
- SID: unique local rule identifier
- Revision: rule revision number

---

## 🚨 SOC Relevance

A NIDS is useful because it provides network telemetry that may not be visible from endpoint logs alone.

A SOC analyst can use network detections to investigate:

- Suspicious connections
- Reconnaissance
- Unusual protocols
- Suspicious HTTP requests
- Repeated network activity
- Potentially malicious traffic patterns

The important distinction is that a network alert is **evidence requiring investigation**, not automatically proof that a compromise occurred.

---

## 🧠 What I Learned

I established the foundation for the rest of the Snort project:

```text
Traffic
  ↓
Snort
  ↓
Rule
  ↓
Alert
  ↓
Investigation
```

---

## 🎤 Interview Questions

### What is Snort?

Snort is a network security monitoring tool that can operate as a network intrusion detection and prevention system. In this project I primarily used it as a NIDS.

### What does a Snort rule do?

A rule defines traffic conditions that Snort should inspect and an action to take when those conditions match, such as generating an alert.

### Is every Snort alert an attack?

No. An alert indicates that traffic matched a detection condition. The analyst must validate the event using context and supporting evidence such as PCAPs and other telemetry.

---

## 🧪 Lab Outcome

The Snort environment was successfully validated and the basic detection workflow was understood. This provided the foundation for traffic capture, custom rules and SOC investigation in the following labs.

---

## 🔐 Security Note

All activity in this project was performed in a controlled local environment for defensive security learning.

---

## 🔗 Related Work

Next: [Lab 2 --- Traffic Capture & PCAP Replay](Lab-2-Traffic-Capture-and-PCAP-Replay.md)

---

## 👨‍💻 Author

Josh Yadav  
Computer Science Engineering Student  
Cybersecurity · SOC · Blue Team · SIEM · Incident Response
