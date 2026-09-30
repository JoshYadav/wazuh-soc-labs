# Lab 11 — Mini SOC Detection & Investigation

## Environment

- Snort: 3.12.2.0
- Wazuh: 4.14.6
- OS: Ubuntu 26.04 LTS on WSL2
- Target: `172.24.112.74`

## Scenario 1 — Normal HTTP Baseline

- PCAP: `snort-http.pcap`
- Packets analyzed: 50
- HTTP requests observed: 5
- Alerts: 0
- Assessment: benign baseline traffic in the controlled lab.

## Scenario 2 — Suspicious HTTP Request

- Source: `172.24.112.1:59134`
- Destination: `172.24.112.74:8080`
- Protocol: HTTP over TCP
- Snort SID: `1000008`
- Request: `GET /etc/passwd HTTP/1.1`
- Server response: `HTTP/1.0 404 File not found`
- Packets analyzed: 10
- Snort alerts: 1
- Wazuh rule: `100900`, level `10`
- Wazuh description: `Snort: Suspicious HTTP passwd path detected`

Assessment: suspicious attempted access was detected. The PCAP showed a 404 response, so there was no evidence in this capture of successful retrieval of the requested resource.

## Scenario 3 — TCP Port Scan / Reconnaissance

- Target: `172.24.112.74`
- Packets analyzed: 21
- TCP sessions: 8
- SYNs: 8
- Alerts: 1

Assessment: the traffic is consistent with network reconnaissance. The alert alone does not establish compromise.

## Overall SOC Workflow

`Network Traffic → Snort Detection → Alert Generation → PCAP Investigation → IOC Extraction → Analyst Assessment → Wazuh Correlation → Centralized SOC Visibility`

## Key Lesson

A detection is an investigation starting point, not automatic proof of compromise.
