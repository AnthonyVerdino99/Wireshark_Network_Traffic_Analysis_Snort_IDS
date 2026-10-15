# ICMP Traffic Analysis — Wireshark and tcpdump

## Objective

Capture and analyze authorized ICMP traffic between Kali Linux and the Metasploitable 2 lab target, then correlate larger ICMP packets with Snort detection.

## Lab Environment

| System | Interface | IPv4 address |
|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 |
| Metasploitable 2 | eth0 | 172.30.1.21/24 |
| Snort Ubuntu VM | enp0s8 | 172.30.1.30/24 |

## Basic ICMP Capture

- **Capture interface:** `eth1`
- **Capture filter:** `icmp and host 172.30.1.21`
- **Tools:** tcpdump 4.99.6 and Wireshark 4.6.4
- **Temporary capture:** `/tmp/icmp-test.pcap`

### Results

| Measure | Result |
|---|---:|
| Packets captured | 8 |
| Packets dropped by kernel | 0 |
| ICMP echo requests | 4 |
| ICMP echo replies | 4 |

The observed echo requests from `172.30.1.20` to `172.30.1.21` each had a corresponding echo reply with a matching sequence number. This confirms bidirectional ICMP communication during the test.

### Evidence

![Wireshark basic ICMP packet analysis](../screenshots/Wireshark%20ICMP%20packet.png)

The raw packet capture remains in `/tmp` and is not included in the public repository.

## Large ICMP Packet Analysis and IDS Correlation

### Objective

Examine larger ICMP packets in Wireshark and correlate the observed traffic with Snort's large-ICMP detection rule, SID `1000002`.

### Test Procedure

1. Generated ICMP traffic from Kali Linux (`172.30.1.20`) to Metasploitable 2 (`172.30.1.21`) using a 600-byte ping payload.
2. Inspected packet details in Wireshark, including frame length, IPv4 total length, and ICMP fields.
3. Compared the packet evidence with Snort's alert output for SID `1000002`, configured for ICMP payloads larger than 500 bytes.

### Wireshark Evidence — Large ICMP Traffic

![Wireshark large ICMP traffic](../screenshots/Wireshark%20large%20ICMP%20traffic.png)

This screenshot provides a view of the large-ICMP traffic observed during the test.

### Wireshark Evidence — Packet Data

![Wireshark large ICMP traffic data](../screenshots/Wireshark%20large%20ICMP%20traffic%20data.png)

This screenshot provides additional packet-data evidence for examining the large ICMP traffic. Interpret packet sizes alongside the selected packet fields and capture context.

### Result and Interpretation

The test was reported as successful, with Wireshark packet observations consistent with Snort's large-ICMP alert. The corresponding [Snort large-ICMP alert screenshot](../screenshots/Snort%20large-ICMP%20alert%20screenshot.png) is also available.

Packet size can be a useful investigation signal, but it does not independently establish malicious intent. Analysts should consider source and destination, frequency, surrounding traffic, endpoint context, and other telemetry.

The raw capture and temporary logs remain local and are not published.
