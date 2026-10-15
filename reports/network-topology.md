# Network Topology and Connectivity Verification

## Lab Components

| System | Interface | IPv4 address | Role |
|---|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 | Analyst workstation and traffic generator |
| Metasploitable 2 | eth0 | 172.30.1.21/24 | Authorized test target |
| Snort Ubuntu VM | enp0s8 | 172.30.1.30/24 | Network monitoring and intrusion detection |

## Lab Network

- **Subnet:** `172.30.1.0/24`
- **Broadcast address:** `172.30.1.255`

The three systems communicate through the lab subnet. Snort monitors traffic on `enp0s8`.

## Connectivity Verification

Command: `ping -c 4 172.30.1.21`

| Measure | Result |
|---|---:|
| Packets transmitted | 4 |
| Packets received | 4 |
| Packet loss | 0% |
| Average round-trip time | 2.031 ms |

## Traffic Visibility and Detection

- Confirmed that the Snort VM could observe ICMP traffic on `enp0s8`.
- Validated the Snort configuration with `snort -T`.
- Tested the basic ICMP detection rule, SID `1000001`.
- Tested the large-ICMP detection rule, SID `1000002`, using a 600-byte ping payload.
- Inspected ICMP packets in Wireshark and compared observations with Snort alert output.

Evidence:
- [Wireshark ICMP packet analysis](../screenshots/Wireshark%20ICMP%20packet.png)
- [Snort basic ICMP alert](../screenshots/Snort%20ICMP%20Detected.png)

## Network Configuration Limitations

Kali Linux also has `eth0` configured as `10.0.0.238/24`, and the Snort VM has `enp0s3` configured as `10.0.0.226/24`.

Because these additional interfaces are present, complete isolation from other networks has not been established. Confirm VirtualBox network modes and routing before describing the entire environment as fully isolated.

## Conclusion

The lab demonstrated ICMP connectivity, packet inspection, Snort traffic visibility, and rule-based alert generation. These results establish a foundation for further detection testing and blue-team investigation.

## Related Reports

- [ICMP traffic analysis](icmp-analysis.md)
- [Snort ICMP detection test](snort-icmp-detection.md)
- [Findings and remediation recommendations](findings-and-remediation.md)
