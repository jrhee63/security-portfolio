# ICMP Timestamp Firewall Block – Automated with Ansible

## Objective

To automate the mitigation of the ICMP Timestamp Request/Reply vulnerability on a Rocky Linux VM using `firewalld` and `Ansible`. 

---

## Tools Used

- Rocky Linux 9  
- Ansible  
- firewalld  
- iptables (via direct rules)  
- Nessus (for vulnerability scanning)

---

## Ansible Playbook

This playbook installs and enables firewalld, blocks ICMP type 13 and 14 packets using direct iptables-style rules, and reloads the firewall to apply changes.

```yaml
---
- name: Fix ICMP Timestamp Vulnerability
  hosts: localhost
  become: yes

  tasks:
    - name: Install firewalld
      package:
        name: firewalld
        state: present

    - name: Enable and start firewalld
      service:
        name: firewalld
        state: started
        enabled: yes

    - name: Block ICMP timestamp-request (type 13)
      command: >
        firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 --proto icmp --icmp-type 13 -j DROP

    - name: Block ICMP timestamp-reply (type 14)
      command: >
        firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 --proto icmp --icmp-type 14 -j DROP

    - name: Reload firewalld to apply changes
      service:
        name: firewalld
        state: reloaded
```

Playbook code:  
![Playbook Code](../screenshots/icmp-firewall/playbook-code.png)

---

## Execution

Ran the playbook with:

```bash
ansible-playbook icmp_fix.yml --ask-become-pass
```

Successful playbook execution:  
![Ansible Run Success](../screenshots/icmp-firewall/ansible-run-success.png)

---

## Verification

### Firewall Rules Applied

```bash
sudo firewall-cmd --direct --get-all-rules
```

Expected output:

```
ipv4 filter INPUT 0 --proto icmp --icmp-type 13 -j DROP
ipv4 filter INPUT 0 --proto icmp --icmp-type 14 -j DROP
```

Confirmed firewall rules:  
![Firewall Rules Confirmed](../screenshots/icmp-firewall/firewall-rules-confirmed.png)

### Nessus Scan Results

Before applying the Ansible script, a Nessus scan identified the Rocky Linux VM as vulnerable to ICMP timestamp requests. This was flagged as a low-severity issue related to system fingerprinting.

Nessus scan result before the fix:  
![Nessus Before](../screenshots/icmp-firewall/nessus-before.png)

After applying the firewall rules using Ansible and rescanning with Nessus, the ICMP Timestamp vulnerability no longer appeared in the report.

Nessus scan result after the fix:  
![Nessus After](../screenshots/icmp-firewall/nessus-after.png)

---

## What I Learned

- How to write and run a structured Ansible playbook for local system changes  
- How to apply ICMP-specific firewall rules using `firewalld --direct`  
- How to use Nessus as a before-and-after validator for vulnerability mitigation  
- Why blocking ICMP timestamp packets improves security posture against recon attacks

---
