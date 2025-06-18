## ✅ Setting Up SSH Access for `deployer` User

**Purpose:**  
I needed a way to connect to the Rocky Linux VM without typing a password every time. This helps automation tools like GitLab and Ansible connect and run tasks without manual input.

**What I Did:**

```bash
# Step 1: Generate an SSH key pair on my machine
ssh-keygen -t rsa -b 4096 -C "deployer access"

# Step 2: Copy the public key to the Rocky VM
ssh-copy-id deployer@<rocky_vm_ip>

# If ssh-copy-id isn't available, use this:
cat ~/.ssh/id_rsa.pub | ssh deployer@<rocky_vm_ip> \
  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

**Result:**  
Now I can SSH into the Rocky VM as `deployer` without entering a password — great for automation.
