# 🛡️ Lab 2 --- Traffic Capture & PCAP Replay
### Building the Traffic → PCAP → Snort Detection Workflow

> Lab objective: Learn how to capture network traffic, preserve it as a PCAP file, and replay the captured traffic through Snort for controlled detection testing.

---

## 📌 Overview

Real SOC investigations often depend on packet captures because an alert summary alone may not provide enough context.

This lab established the workflow used throughout the project:

```text
Generate Controlled Traffic
          ↓
      tcpdump Capture
          ↓
         PCAP
          ↓
     Snort Replay
          ↓
     Rule Matching
          ↓
        Alert
          ↓
   Wireshark Investigation
```

Using PCAP replay makes detection testing repeatable and allows the same traffic to be analyzed multiple times with different rules or thresholds.

---

## 🎯 Objectives

- Understand packet capture with `tcpdump`.
- Understand PCAP files as investigation artifacts.
- Replay captured traffic through Snort.
- Understand the difference between traffic generation and detection.
- Prepare repeatable evidence for later labs.

---

## 🖥️ Lab Environment

| Component | Role |
|---|---|
| Ubuntu 26.04 LTS / WSL2 | Capture and Snort environment |
| tcpdump | Packet capture |
| Snort 3 | PCAP analysis and detection |
| Wireshark | Visual packet investigation |

---

## ⚙️ Packet Capture Concept

A typical capture command used during the project was:

```bash
sudo tcpdump -i eth0 -n icmp -w ~/snort-eth0.pcap
```

For TCP/HTTP traffic:

```bash
sudo tcpdump -i eth0 -n tcp port 8080 -w ~/snort-tcp-eth0.pcap
```

The resulting PCAP can be replayed with Snort:

```bash
snort -c /usr/local/etc/snort/snort.lua -R ~/icmp.rules -r ~/snort-lab.pcap
```

---

## 🔎 Why PCAPs Matter in a SOC

PCAPs allow an analyst to move from an alert summary to the actual network evidence.

A SOC analyst can inspect:

- Source and destination IPs
- Source and destination ports
- Protocols
- TCP flags
- HTTP requests
- HTTP responses
- Payload-related metadata
- Timing and packet sequence

This helps validate whether an alert reflects the underlying traffic.

---

## 🧠 What I Learned

The important concept was that detection and investigation are separate steps:

```text
Detection
   ↓
Alert
   ↓
PCAP
   ↓
Evidence Validation
```

The same PCAP can also be replayed against a different rule, which makes it useful for detection engineering and tuning.

---

## 🎤 Interview Questions

### Why use PCAPs when you already have an IDS alert?

An IDS alert summarizes a detection condition. A PCAP provides the underlying network evidence that allows the analyst to validate the alert and investigate the traffic.

### What is the advantage of PCAP replay?

It makes detection testing repeatable. The same traffic can be replayed against different rules and thresholds without generating the traffic again.

---

## 🧪 Lab Outcome

A repeatable traffic-capture and PCAP-replay workflow was established. This became the evidence pipeline used by the later detection and investigation labs.

---

## 🔗 Related Work

Previous: [Lab 1 --- Snort Fundamentals](Lab-1-Snort-Fundamentals.md)  
Next: [Lab 3 --- ICMP Detection](Lab-3-ICMP-Detection.md)

---

## 👨‍💻 Author

Josh Yadav  
Computer Science Engineering Student  
Cybersecurity · SOC · Blue Team · SIEM · Incident Response
