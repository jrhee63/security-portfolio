## ✅ Setting Up SSH Access for `deployer` User

**Purpose:**  
I needed a way to connect to the Rocky Linux VM as the `deployer` user without typing a password every time. This is important for automation tools like GitLab CI and Ansible, which need remote access to run tasks during our web app project.

The `deployer` user is used to:
- Run Ansible scripts from the GitLab pipeline
- Log each pipeline run on the VM
- Allow secure remote access for automation

**What I Did:**

```bash
# Step 1: Generated an SSH key pair on my machine
ssh-keygen -t rsa -b 4096 -C "deployer access"

# Step 2: Added the public key to the Rocky VM for the deployer user
ssh-copy-id deployer@<rocky_vm_ip>

# (If ssh-copy-id wasn’t available)
cat ~/.ssh/id_rsa.pub | ssh deployer@<rocky_vm_ip> \
  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
