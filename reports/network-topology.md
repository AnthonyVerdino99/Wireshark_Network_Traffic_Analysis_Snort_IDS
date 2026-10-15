Network Topology and Connectivity Verification

Lab Components

System

Interface

IPv4 Address

Role

Kali Linux

eth1

172.30.1.20/24

Analyst workstation and traffic generator

Metasploitable 2

eth0

172.30.1.21/24

Authorized test target

Snort Ubuntu VM

enp0s8

172.30.1.30/24

Network monitoring and intrusion detection

Lab Network

Subnet: 172.30.1.0/24

Broadcast address: 172.30.1.255

Kali Linux, Metasploitable 2, and Snort communicate through the lab subnet. The Snort monitoring interface is enp0s8.

Connectivity Verification

Command: ping -c 4 172.30.1.21

Packets transmitted: 4

Packets received: 4

Packet loss: 0%

Average round-trip time: 2.031 ms

Traffic Visibility and Detection

Confirmed that the Snort VM could observe ICMP traffic on enp0s8.

Validated the Snort configuration with snort -T.

Successfully tested the basic ICMP detection rule, SID 1000001.

Tested the large-ICMP detection rule, SID 1000002, using a 600-byte ping payload.

Used Wireshark to inspect ICMP packets and correlate packet evidence with Snort alerts.

Network Configuration Limitations

Kali Linux also has eth0 configured as 10.0.0.238/24, and the Snort VM has enp0s3 configured as 10.0.0.226/24.

Because these additional interfaces are present, complete isolation from other networks has not been established. Confirm VirtualBox network modes and routing before describing the entire environment as fully isolated.

Conclusion

The lab successfully demonstrated ICMP connectivity, packet inspection, Snort traffic visibility, and rule-based alert generation. These results establish a foundation for further detection testing and blue-team investigation.

Next Steps

Review report accuracy and screenshot links.

Document detection limitations and tuning recommendations.

Complete a final portfolio review.





