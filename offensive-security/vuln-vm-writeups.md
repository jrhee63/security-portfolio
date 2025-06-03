# VulnHub VM Exploitation – Writeups

This section includes summaries of attacks conducted against intentionally vulnerable VMs using Kali Linux tools.

---

## VM: DC-1

- **Scan Tools**: Nmap, Nikto
- **Vulnerability**: Drupalgeddon2 (CVE-2018-7600)
- **Exploit Used**: Metasploit module `exploit/unix/webapp/drupal_drupalgeddon2`
- **Outcome**: Gained reverse shell and root access

---

## VM: VulnOS-2

- **Initial Access**: Discovered FTP anonymous login
- **Escalation**: SUID binary `nmap` used for privilege escalation
- **Lessons Learned**: Importance of hardening legacy services
