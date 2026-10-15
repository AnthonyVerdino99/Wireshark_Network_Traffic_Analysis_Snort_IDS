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

## Security Analysis and Limitations

The rule is broad and can alert on routine ICMP traffic. An ICMP alert is an investigative signal, not proof of malicious activity. Analysts should validate source and destination addresses, packet details, timing, frequency, and other available telemetry.

The separate large-ICMP rule (SID `1000002`) was tested with a 600-byte ping payload. The detailed packet correlation is documented in [ICMP traffic analysis](icmp-analysis.md). Its dedicated Snort alert screenshot still needs to be uploaded.

Raw packet captures and temporary logs remain outside the public repository unless reviewed and intentionally selected for publication.

## Next Steps

- Add the large-ICMP Snort alert screenshot and large-ICMP Wireshark screenshot.
- Continue testing benign traffic patterns and document false-positive considerations.
- Validate VirtualBox network boundaries before describing the entire environment as fully isolated.
