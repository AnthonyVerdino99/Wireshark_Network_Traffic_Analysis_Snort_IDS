# Network Topology and Connectivity Verification

## Lab Components

| System | Interface | IPv4 Address | Role |
|---|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 | Analyst workstation |
| Metasploitable 2 | eth0 | 172.30.1.21/24 | Vulnerable test target |
| Snort IDS | TBD | TBD | Detection sensor; pending verification |

## Lab Network

Subnet: 172.30.1.0/24
Broadcast: 172.30.1.255

Kali eth1 (172.30.1.20) <--> Metasploitable 2 eth0 (172.30.1.21)

## Connectivity Test

Command: ping -c 4 172.30.1.21

- Packets transmitted: 4
- Packets received: 4
- Packet loss: 0%
- Average round-trip time: 2.031 ms

## Findings and Limitations

ICMP connectivity from Kali to Metasploitable 2 was verified.
Snort placement, visibility, and alert generation have not yet been verified.
Kali also has eth0 configured as 10.0.0.238/24; its virtualization network configuration should be checked before claiming complete isolation.

## Next Steps

1. Verify the Snort VM network interface and address.
2. Capture authorized lab traffic with Wireshark.
3. Compare observed packets with Snort IDS alerts.
