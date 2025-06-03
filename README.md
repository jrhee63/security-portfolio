# Josh Rhee – Cybersecurity Internship Portfolio (Summer 2025 @ CACI)

This repo documents the work I did during my internship, focused on building, attacking, and hardening a virtual security lab using Linux, open-source tools, and automation workflows.

---

## VM Lab Setup
- Built isolated test environment with **Kali**, **Rocky Linux**, **Nessus**, and **VulnHub**
- Assigned static IPs, verified connectivity, and fixed SSL trust issues by importing internal root CAs

## System Hardening
- Scripted and automated security controls on Rocky Linux
- Disabled root SSH, applied firewall rules, enforced password policies, and ran OpenSCAP STIG baselines

## Validation
- Ran **Lynis** to audit baseline and post-hardening configurations  
- Used **Nessus** to externally scan Rocky Linux and VulnHub targets  
- Cross-referenced manual exploits with scan findings to validate vulnerabilities and confirm remediation

## Offensive Security Testing
- Attacked VulnHub VMs using Kali and tools like Nmap, Nikto, Burp Suite, Hydra, and Metasploit
- Documented real-world exploits and compared with Nessus scan results

## CI/CD & Compliance
- Integrated **Black Duck** into GitLab pipeline to automate builds, testing, and scans
- Used **Selenium** to simulate dashboard logins and check compliance status

---

## Lab Notes
All major documentation lives in [`lab-notes/`](./lab-notes/):
- [`vm-setup.md`](./lab-notes/vm-setup.md) – how I configured my virtual environment
- `system-hardening.md` – notes on hardening Rocky Linux
- `vuln-testing.md` – attack writeups and tool use
- `validation.md` – scanning and audit validation
- `cicd-compliance.md` – automation + compliance tracking

---

## Internship Log
[Check out what I did week by week →](./internship-log.md)

---

## Visual examples from the project, located in the [`screenshots/`](./screenshots/) folder:

- vm-setup/ – network settings, IP assignments, trust anchor output
- system-hardening/ - Bash script samples. OpenSCAP compliance scan results, firewalld results
- validation/ – Nessus scan results, Lynis hardening scores
- vuln-testing/ – Nmap results, Metasploit shells, Hydra brute-force output  
- cicd/ – GitLab pipeline, Black Duck scans, Selenium browser automation  
