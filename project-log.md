# Project Log – Summer 2025 @ CACI

Weekly breakdown of what I worked on, learned, and troubleshooted.

---

## Sprint 1 (5/21 - 6/3)

### What I Did
- Set up Kali, Rocky, Nessus, and VulnHub VMs in VirtualBox
- Assigned static IPs and verified internal connectivity
- Imported internal CAs to fix HTTPS errors in Firefox and Chrome
- Took initial VM snapshots once lab was configured

### Tools Used
- VirtualBox, Kali Linux, Rocky Linux, trust anchor, certutil

### Notes & Challenges
- Faced challenges configuring VirtualBox networking
- Learned how Linux handles root certificates
- Realized browsers use different trust stores (especially Firefox)

---

## Sprint 2 (6/4 – 6/17)

### What I Did
- Manually mitigated ICMP Timestamp vulnerability reported by Nessus using firewalld.
- Wrote and tested an Ansible playbook to automate the same firewall fix.

### Tools Used
- Nessus, Ansible, Firewalld

### Notes & Challenges
- firewalld rich rules did not support icmp_type; had to switch to direct iptables rules.

---

## Week 3 (MM/DD – MM/DD)


