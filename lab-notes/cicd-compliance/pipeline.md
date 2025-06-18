## What This Is  
This demo shows how I added two features to our GitLab pipeline:  
- **Black Duck** to automatically check for security issues  
- A **log pipeline** to save info about each pipeline run  

Both features run on a Rocky Linux VM using Ansible.

## What I Did  
- Wrote Ansible playbooks for both Black Duck and logging  
- Set up the `deployer` user for SSH access to the VM  
- Configured the inventory file for Ansible  
- Ran everything manually first to test it  

## Useful Commands  
```bash
# Run Black Duck security scan
ansible-playbook -i inventory.ini blackduck_scan.yml

# Run the log pipeline playbook
ansible-playbook -i inventory.ini log_pipeline_run.yml

# Check if VM is reachable
ping 10.0.3.10

# SSH into the VM as deployer
ssh deployer@10.0.3.10

# Turn off firewall to test connections
sudo systemctl stop firewalld
```

## Problems I Ran Into  
- Black Duck failed due to token and network errors  
- Log pipeline failed with SSH connection errors  
- I could SSH **into** the `deployer` user on the VM, which confirms the user and key are working  
- But I couldn’t SSH **from the host to the VM** — meaning something is likely blocking it (firewall, NAT settings, or host permissions)  
- The VM was using **NAT mode**, which doesn’t allow incoming connections from the host  
- Switched to **Bridged mode** so the host and VM are on the same network  
- Disabled firewalld temporarily to confirm if it was part of the issue  

## What I Learned  
Just having SSH keys and Ansible set up isn’t enough — network permissions matter.  
Since I could connect from inside the VM but not from outside, it’s likely that the host or firewall is blocking the SSH connection.

## What’s Next  
- Talk to IT about opening SSH ports on both the VM and host  
- Use Bridged networking so the VM is fully reachable  
- Fix Black Duck API token issues  
- Automate SSH key and token setup in the playbooks  
- Re-run the GitLab pipeline after networking is fixed  

---
