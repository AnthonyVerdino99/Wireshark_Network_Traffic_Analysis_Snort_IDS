# Wireshark Network Traffic Analysis & Snort IDS

A hands-on cybersecurity homelab project demonstrating packet inspection, network-based intrusion detection, alert validation, and blue-team reporting using Wireshark, tcpdump, and Snort.

> **Scope:** Authorized testing of controlled ICMP traffic in a personal lab. The work demonstrates introductory detection validation, not a production security assessment. Complete network isolation has not yet been established because the Kali and Snort VMs also have interfaces on the `10.0.0.0/24` network.

## Project Goals

- Document the lab topology and connectivity.
- Capture and inspect network traffic with tcpdump and Wireshark.
- Validate Snort configuration and verify that the sensor can observe lab traffic.
- Test basic ICMP and large-ICMP detection rules.
- Correlate packet evidence with IDS alerts.
- Explain detection limitations, false-positive considerations, and defensive improvements.

## Lab Environment

| System | Interface | IPv4 address | Role |
|---|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 | Analyst workstation and traffic generator |
| Metasploitable 2 | eth0 | 172.30.1.21/24 | Authorized test target |
| Snort Ubuntu VM | enp0s8 | 172.30.1.30/24 | IDS monitoring sensor |

**Lab subnet:** `172.30.1.0/24`

## Tools

- Kali Linux
- Wireshark 4.6.4
- tcpdump 4.99.x (versions are recorded in the individual reports)
- Snort 2.9.20 GRE
- Metasploitable 2
- Oracle VirtualBox

## Key Results

- Captured four ICMP echo requests and four replies between Kali and Metasploitable 2.
- Confirmed Snort could observe ICMP traffic on `enp0s8`.
- Validated Snort configuration using `snort -T`.
- Tested basic ICMP detection (SID `1000001`) and large-ICMP detection (SID `1000002`).
- Documented limitations: broad ICMP rules can generate benign alerts, packet size alone does not prove malicious intent, and additional VM interfaces mean full network isolation must not be assumed.

## Reports and Evidence

- [Network topology and connectivity verification](reports/network-topology.md)
- [ICMP traffic analysis and IDS correlation](reports/icmp-analysis.md)
- [Snort ICMP detection test](reports/snort-icmp-detection.md)
- [Findings and remediation recommendations](reports/findings-and-remediation.md)
- [Screenshots directory](screenshots/)

Available screenshots:
- [Wireshark basic ICMP packet](screenshots/Wireshark%20ICMP%20packet.png)
- [Snort basic ICMP alert](screenshots/Snort%20ICMP%20Detected.png)

Still to add:
- Snort large-ICMP alert screenshot
- Wireshark large-ICMP packet analysis screenshot

The report links to the evidence that is currently present. The project is not evidence-complete until the remaining screenshots are uploaded and linked.

## Safety and Evidence Handling

All tests are intended for systems the author owns or is authorized to assess. Keep vulnerable targets on an appropriately restricted lab network. Review screenshots and logs before publishing. Raw packet captures, credentials, private keys, and other sensitive material should not be committed to this public repository.

## Current Status

**Core ICMP and Snort tests documented; evidence cleanup and further detection testing remain in progress.**
