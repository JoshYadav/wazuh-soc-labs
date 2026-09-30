# PCAP Artifacts

The Snort labs used locally generated PCAP files for replay and investigation. The original PCAP binaries are intentionally not included in this GitHub package because they were generated inside the local WSL environment and are not available as portable source files in this package.

PCAPs used during the labs included:

- `snort-lab.pcap` — basic ICMP traffic
- `snort-eth0.pcap` — network-aware ICMP traffic
- `icmp-rate.pcap` / `icmp-rate-loopback.pcap` — ICMP frequency testing
- `snort-tcp-eth0.pcap` — TCP/HTTP traffic on port 8080
- `snort-portscan.pcap` — controlled TCP SYN reconnaissance traffic
- `snort-http.pcap` — normal HTTP baseline
- `snort-http-suspicious.pcap` — suspicious `/etc/passwd` HTTP request

The screenshots and Markdown documentation contain the relevant evidence and observed results.
