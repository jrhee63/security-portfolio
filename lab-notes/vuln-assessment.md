## ICMP Timestamp Request Vulnerability

Tool Used: Nessus  
Target: Rocky Linux (10.0.3.10)  
Scan Date: June 4, 2025  
Severity: Low

Nessus initially reported that the Rocky Linux VM was responding to ICMP timestamp requests, potentially allowing a remote attacker to infer system time and bypass time-based authentication.

Screenshot: `nessus-scan-icmp-timestamp.png`

Open services were reviewed on the system and unnecessary ones were removed.

Screenshot: `rocky-open-services-before.png`

ICMP timestamp requests and replies were blocked using firewall rules. These changes were then verified externally via:

```bash
nmap -sV -Pn 10.0.3.10
```

Screenshot: `nmap-after-hardening.png`  
Only SSH remained accessible from the network.

To confirm that Nessus no longer detected the vulnerability, I performed a re-scan using the same scan policy.

Screenshot: `nessus-rescan-clean.png`  
The ICMP timestamp request vulnerability was no longer reported.
project-log.md (updated)
markdown
Copy
Edit
### June 4, 2025 – Nessus Discovery and Hardening Validation

- Ran Nessus scan from Nessus VM to Rocky Linux target (10.0.3.10)
- Discovered ICMP timestamp response vulnerability
- Screenshot: `nessus-scan-icmp-timestamp.png`

- Reviewed public zone services:
```bash
sudo firewall-cmd --zone=public --list-services
```
- Screenshot: `rocky-open-services-before.png`

- Removed `cockpit` and `dhcpv6-client`
- Blocked ICMP timestamp-request and reply traffic using `firewall-cmd`
- Screenshot: `rocky-blocked-icmp-firewall.png`

- Ran `nmap -sV -Pn` from Kali to confirm only SSH (port 22) was reachable
- Screenshot: `nmap-after-hardening.png`

- Re-scanned target in Nessus to validate fix
- Screenshot: `nessus-rescan-clean.png`
- Nessus confirmed the vulnerability was no longer present
system-hardening.md (unchanged, just reference result)
markdown
Copy
Edit
### Removing Unnecessary Services

Checked exposed services using:

```bash
sudo firewall-cmd --zone=public --list-services
```

Screenshot: `rocky-open-services-before.png`  
Found `cockpit` and `dhcpv6-client` enabled.

Removed both:

```bash
sudo firewall-cmd --permanent --remove-service=cockpit
sudo firewall-cmd --permanent --remove-service=dhcpv6-client
sudo firewall-cmd --reload
```

---

### Blocking ICMP Timestamp Requests

Blocked ICMP timestamp requests and replies:

```bash
sudo firewall-cmd --permanent --add-icmp-block=timestamp-request
sudo firewall-cmd --permanent --add-icmp-block=timestamp-reply
sudo firewall-cmd --reload
```

Screenshot: `rocky-blocked-icmp-firewall.png`

---

### External Validation with Nmap

Used `nmap` to confirm that only SSH was accessible:

```bash
nmap -sV -Pn 10.0.3.10
```

Screenshot: `nmap-after-hardening.png`

---

### Final Validation via Nessus Re-scan

Re-ran Nessus scan to verify vulnerability was no longer detected.

Screenshot: `nessus-rescan-clean.png`
