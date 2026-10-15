# Findings and Remediation Recommendations

## Executive Summary

This authorized cybersecurity homelab evaluated basic ICMP connectivity, packet visibility, and rule-based alerting with Wireshark, tcpdump, and Snort. The tests demonstrated bidirectional ICMP communication between Kali Linux and Metasploitable 2, visibility of lab ICMP traffic at the Snort monitoring interface, and successful alert generation for basic and large-ICMP test traffic.

These tests validate a small lab detection workflow; they do not establish production readiness or prove that the traffic was malicious.

## Scope and Environment

| System | Interface | Address | Purpose |
|---|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 | Analyst workstation and traffic generator |
| Metasploitable 2 | eth0 | 172.30.1.21/24 | Authorized test target |
| Snort Ubuntu VM | enp0s8 | 172.30.1.30/24 | IDS sensor |

Testing focused on ICMP echo traffic within the `172.30.1.0/24` lab subnet.

## Findings

### Finding 1 — Lab connectivity and packet inspection verified

**Evidence:** Four ICMP echo requests and four replies were captured; tcpdump reported eight packets captured and zero kernel drops. Wireshark was used to inspect the ICMP traffic.

**Interpretation:** The capture demonstrated bidirectional reachability during the test and provided packet-level evidence for investigation. It does not establish that other protocols or paths are functioning correctly.

### Finding 2 — Snort visibility and basic ICMP alerting verified

**Evidence:** The Snort VM observed ICMP traffic on `enp0s8`, the Snort configuration passed `snort -T`, and the local rule with SID `1000001` generated alerts during the test.

**Interpretation:** The sensor could observe the tested traffic and generate a rule-based alert. The rule is broad, so routine ICMP may generate alerts. An alert is an investigative signal, not a conclusion that an attack occurred.

### Finding 3 — Large-ICMP detection tested

**Evidence:** A 600-byte ICMP ping payload was generated and the resulting packet characteristics were inspected in Wireshark. The large-ICMP rule, SID `1000002`, was tested and the observed packet evidence was reported as consistent with the alert.

**Interpretation:** Payload-size rules can identify traffic that meets a configured threshold. Large ICMP packets may be legitimate troubleshooting traffic or may warrant additional investigation depending on context. Size alone is not proof of tunneling or malicious activity.

### Finding 4 — Network isolation requires further validation

**Evidence:** Kali also has `eth0` on `10.0.0.0/24`, and the Snort VM has `enp0s3` on that same subnet.

**Risk:** These additional interfaces mean complete isolation from other networks has not yet been demonstrated.

**Recommendation:** Review VirtualBox adapter modes, routing, and host connectivity. Use an internal or host-only network for the lab segment as appropriate, avoid bridging vulnerable targets to an untrusted network, and document the verified network boundaries before claiming full isolation.

## Detection Limitations and False Positives

- The basic ICMP rule may alert on ordinary echo requests and replies. Repeated alerts are not necessarily incidents.
- The large-ICMP rule relies on a fixed payload-size threshold. Legitimate diagnostic activity can trigger it, while other suspicious activity may not match it.
- Rules only detect traffic visible to the sensor and covered by the configured rules. Traffic on an unmonitored interface or encrypted/application-layer behavior may require other telemetry.
- The tests covered a small number of controlled packets. They do not measure detection rate, false-positive rate, throughput, or resilience under load.
- Alerts should be triaged using packet details, endpoints, timing, frequency, baseline behavior, and relevant endpoint or firewall logs.

## Recommended Improvements

1. **Validate network boundaries:** Confirm VirtualBox network modes and routes; document which interfaces can reach the lab subnet.
2. **Tune ICMP alerting:** Consider separating echo requests from replies, using sensible thresholds or rate-based detection, and documenting expected diagnostic traffic.
3. **Improve alert context:** Ensure alerts include useful source/destination details and timestamps; correlate with Wireshark captures and host/firewall logs.
4. **Test rule behavior systematically:** Record test input, expected result, observed alert, pass/fail status, and any limitations for each rule.
5. **Monitor sensor health:** Periodically validate Snort configuration, confirm the correct interface is monitored, and check for packet drops.
6. **Protect evidence:** Keep raw packet captures and temporary logs out of the public repository unless reviewed for sensitive data. Publish selected screenshots with clear labels.
7. **Expand detections safely:** In the isolated lab, test additional benign traffic patterns and document rule tuning before adding rules to a production environment.

## Conclusion

The project demonstrates a repeatable introductory workflow: generate controlled traffic, observe packets, validate sensor visibility, inspect IDS alerts, and document findings with limitations and recommendations. The most important next improvements are verifying network boundaries, completing the public screenshot evidence, and testing additional detection scenarios with recorded expected outcomes.

## Evidence Status

- Basic Wireshark ICMP screenshot: [available](../screenshots/Wireshark%20ICMP%20packet.png).
- Snort basic ICMP alert screenshot: [available](../screenshots/Snort%20ICMP%20Detected.png).
- Snort large-ICMP alert screenshot: pending upload.
- Large-ICMP Wireshark screenshot: pending upload.
