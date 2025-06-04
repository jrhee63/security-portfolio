## ICMP Timestamp Vulnerability – Discovery, Hardening, and Validation

### Tool: Nessus  
### Target: Rocky Linux (10.0.3.10)

Nessus reported that the Rocky VM responded to ICMP timestamp requests, exposing system time and increasing the risk of time-based attacks.

![Nessus scan](../screenshots/vuln-assessment/nessus-scan-icmp-timestamp.png) ![Pre-scan chart](../screenshots/vuln-assessment/nessus-vchart-pre-scan.png)

To minimize the attack surface, I reviewed open firewall services:

```bash
sudo firewall-cmd --zone=public --list-services
```

Unnecessary services like `cockpit` and `dhcpv6-client` were removed:

```bash
sudo firewall-cmd --permanent --remove-service=cockpit
sudo firewall-cmd --permanent --remove-service=dhcpv6-client
sudo firewall-cmd --reload
```

![Open services before](../screenshots/vuln-assessment/rocky-open-services-before.png)

ICMP timestamp requests and replies were blocked via firewall:

![Firewall ICMP blocks](../screenshots/system-hardening/rocky-blocked-icmp-firewall.png)

To validate the fix externally, I scanned the target using Nmap:

```bash
nmap -sV -Pn 10.0.3.10
```

Only SSH (port 22) remained open.

![Nmap after hardening](../screenshots/vuln-assessment/nmap-after-hardening.png)

A final Nessus re-scan confirmed that the ICMP timestamp vulnerability was resolved.

![Post-scan chart](../screenshots/vuln-assessment/nessus-vchart-post-scan.png)
