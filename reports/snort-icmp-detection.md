Snort ICMP Detection Test

Objective

Validate that Snort IDS can observe and alert on ICMP traffic generated within an authorized isolated lab environment.

Lab Environment

System

Interface

IPv4 Address

Role

Kali Linux

eth1

172.30.1.20/24

Traffic generator and analyst

Metasploitable 2

eth0

172.30.1.21/24

Test target

Snort Ubuntu VM

enp0s8

172.30.1.30/24

Network monitoring and detection

Tools

Snort 2.9.20 GRE

tcpdump 4.99.4 on the Snort VM

Kali Linux ping utility

Configuration Validation

Snort successfully validated /etc/snort/snort.conf using snort -T. The HOME_NET variable includes 172.30.1.0/24, and the configuration includes the local rules file.

Detection Rule

alert icmp any any -> $HOME_NET any (msg:"ICMP Detected";sid:1000001;rev:1)

Rule SID: 1000001

Alert message: ICMP Detected

Test Procedure

Started Snort monitoring on enp0s8.

Generated four ICMP echo requests from Kali to Metasploitable 2.

Observed Snort alerts for the generated ICMP traffic.

Results

Outcome: Successful detection.

Snort successfully detected the ICMP test traffic using the existing local rule. This confirms that the monitoring interface can observe the lab traffic and that the rule generates alerts.

Security Analysis

This test demonstrates basic network-based intrusion detection and rule validation. The rule is broad and will alert on ICMP traffic matching the configured protected network; an ICMP alert by itself does not indicate malicious activity.

Limitations and Next Steps

Preserve representative alert output as portfolio evidence, after reviewing it for sensitive information.

Test the existing large-ICMP rule (SID 1000002) with controlled traffic.

Investigate how Snort handles different ICMP types and traffic patterns.

Document tuning opportunities to reduce unnecessary alerts.

Raw packet captures and temporary logs remain outside the public repository unless reviewed and intentionally selected for publication.
