# Snort ICMP Detection Test

## Objective

Validate that Snort IDS can observe authorized lab ICMP traffic and generate an alert using a local rule.

## Lab Environment

| System | Interface | IPv4 address | Role |
|---|---|---|---|
| Kali Linux | eth1 | 172.30.1.20/24 | Traffic generator and analyst |
| Metasploitable 2 | eth0 | 172.30.1.21/24 | Authorized test target |
| Snort Ubuntu VM | enp0s8 | 172.30.1.30/24 | Network monitoring sensor |

## Tools and Configuration

- Snort 2.9.20 GRE
- tcpdump 4.99.4 on the Snort VM
- Kali Linux ping utility
- Snort configuration validated with `snort -T`
- `HOME_NET` includes `172.30.1.0/24`, and the configuration includes local rules

## Detection Rule

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP Detected";sid:1000001;rev:1)
```

- **SID:** `1000001`
- **Alert message:** `ICMP Detected`

## Test Procedure

1. Started Snort monitoring on `enp0s8`.
2. Generated four ICMP echo requests from Kali to Metasploitable 2.
3. Observed Snort alerts for the generated ICMP traffic.

## Results

**Outcome:** Successful detection.

Snort detected the test ICMP traffic using the local rule. This confirms that the monitoring interface could observe the tested traffic and that the rule generated alerts.

## Evidence

![Snort ICMP detection console](../screenshots/Snort%20ICMP%20Detected.png)

**What it demonstrates:** Snort observed ICMP traffic and generated an alert matching SID `1000001`.

## Rule Rationale, Monitoring, and Tuning

### Rule 1 — General ICMP Detection (SID 1000001)

```snort
alert icmp any any -> $HOME_NET any (msg:"ICMP Detected";sid:1000001;rev:1)
```

**Purpose:** Establish a baseline by alerting on ICMP traffic directed toward the configured home network. This rule was selected to confirm that Snort could observe the test traffic and generate alerts.

**Expected result:** When Kali Linux sends ICMP echo requests to the Metasploitable 2 target, Snort should generate alerts for matching traffic visible to the sensor.

**Limitation:** This is a broad rule. ICMP is routinely used for connectivity checks and troubleshooting, so alerts may represent expected activity rather than a security incident.

### Rule 2 — Large-ICMP Detection (SID 1000002)

```snort
alert icmp any any -> $HOME_NET any (msg:"Large ICMP Detected!";dsize:>500;sid:1000002;rev:1)
```

**Purpose:** Test whether Snort can identify ICMP packets whose inspected payload exceeds the configured 500-byte threshold. The threshold was selected to create a controlled test case, not because every packet above that size is malicious.

**Test procedure:** From Kali Linux, I generated ICMP traffic using a 600-byte ping payload:

```bash
ping -c 4 -s 600 172.30.1.21
```

I inspected the resulting packets in Wireshark and compared the observations with Snort's alert output. The test was intended to validate that the traffic matched the configured detection condition.

**Security interpretation:** Larger ICMP payloads can be legitimate, but unusual packet sizes, unexpected destinations, repeated patterns, or abnormal traffic volumes may warrant further investigation. Packet size alone does not prove an attack or ICMP tunneling.

### Monitoring, False Positives, and Rule Tuning

An effective IDS workflow requires more than confirming that a rule fires. Analysts must monitor alert patterns, validate the underlying traffic, and determine whether detections represent expected activity or potential threats.

During monitoring, I would:

1. **Establish a baseline:** Record expected traffic, alert frequency, source and destination addresses, and normal lab activity.
2. **Validate alerts:** Compare Snort timestamps, rule SIDs, and packet details with Wireshark captures. Confirm that the alert corresponds to the traffic generated during the test.
3. **Check sensor health:** Verify the correct monitoring interface is active, confirm Snort configuration validity, and investigate packet drops or visibility gaps.
4. **Investigate false positives:** Determine whether alerts correspond to authorized diagnostics or unexpected behavior. Consider packet size, frequency, endpoints, timing, and other available logs.
5. **Tune carefully:** Adjust thresholds, add contextual conditions, or separate different ICMP behaviors where justified. Avoid simply suppressing alerts to reduce alert volume.
6. **Retest and document:** Repeat known test cases after each change, record expected versus observed results, and confirm that tuning reduces unnecessary alerts without losing the detection behavior being tested.

The objective is a stable, explainable detection process—not zero alerts. A useful IDS should produce alerts that analysts can investigate while preserving visibility into potentially suspicious activity.

**Current project limitation:** The tests validate a small number of controlled ICMP scenarios. A larger set of benign and suspicious test cases, with recorded alert counts and outcomes, would be needed to measure false-positive rates or demonstrate reliable detection performance.

## Security Analysis and Limitations

The rule is broad and can alert on routine ICMP traffic. An ICMP alert is an investigative signal, not proof of malicious activity. Analysts should validate source and destination addresses, packet details, timing, frequency, and other available telemetry.

The separate large-ICMP rule (SID `1000002`) was tested with a 600-byte ping payload. The detailed packet correlation is documented in [ICMP traffic analysis](icmp-analysis.md). Its dedicated Snort alert screenshot still needs to be uploaded.

Raw packet captures and temporary logs remain outside the public repository unless reviewed and intentionally selected for publication.

## Next Steps

- Add the large-ICMP Snort alert screenshot and large-ICMP Wireshark screenshot.
- Continue testing benign traffic patterns and document false-positive considerations.
- Validate VirtualBox network boundaries before describing the entire environment as fully isolated.
