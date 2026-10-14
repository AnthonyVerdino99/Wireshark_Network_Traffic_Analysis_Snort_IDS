ICMP Traffic Analysis — Wireshark and tcpdump

Objective

Capture and analyze authorized ICMP traffic between Kali Linux and the Metasploitable 2 lab target.

Lab Environment

System

Interface

IPv4 Address

Kali Linux

eth1

172.30.1.20/24

Metasploitable 2

eth0

172.30.1.21/24

Capture Method

Capture interface: eth1

Capture filter: icmp and host 172.30.1.21

Tools: tcpdump 4.99.6 and Wireshark 4.6.4

Temporary capture file: /tmp/icmp-test.pcap

Results

Packets captured: 8

Packets dropped by kernel: 0

ICMP echo requests: 4

ICMP echo replies: 4

Source: 172.30.1.20

Destination: 172.30.1.21

Each observed echo request had a corresponding echo reply with the matching sequence number.

Analysis

The capture demonstrates successful bidirectional ICMP communication between Kali Linux and Metasploitable 2 over the lab network. The packet list was inspected in Wireshark using the icmp display filter.

This test validates basic reachability and packet inspection. It does not establish that Snort can observe the traffic or generate alerts; that will be tested in a later milestone.

Evidence

Screenshot: screenshots/icmp-capture.png

The raw packet capture is retained locally in /tmp and is not included in this repository.
