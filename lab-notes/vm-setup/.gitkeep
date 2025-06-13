# Provisioning `deployer` User on Rocky Linux 9 with Ansible

## 🎯 Objective

Create a dedicated `deployer` user on a Rocky Linux 9 VM using Ansible. The user should:

• Have a home directory  
• Allow SSH login with an authorized key  
• Have passwordless `sudo` privileges

## 🖥️ Environment

• Host VM: Rocky Linux 9 (`10.0.3.10`)  
• Control Machine: Local workstation (Ansible installed)  
• SSH Key: `~/.ssh/id_rsa` generated locally

## 📦 Tools Used

• Ansible  
• SSH  
• `visudo` (to modify sudo permissions safely)

## 📝 Steps Performed

### 1. ✅ Verified Existing SSH Access

Logged into the Rocky VM:
```bash
ssh joshua.rhee@10.0.3.10
id rocky  # Confirmed 'rocky' user does not exist
```

### 2. 🔐 Generated SSH Key Pair
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
```

### 3. 🔁 Copied Public Key to Server
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub joshua.rhee@10.0.3.10
```

### 4. 📂 Created `inventory.ini`
```ini
[rocky9]
10.0.3.10 ansible_user=joshua.rhee ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_become=true ansible_become_method=sudo
```

### 5. 📜 Wrote `create_deployer.yml` Playbook
```yaml
---
- name: Create deployer user on Rocky 9 server
  hosts: rocky9
  become: true

  vars:
    deployer_user: deployer
    deployer_pubkey: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"

  tasks:
    - name: Ensure deployer user exists
      user:
        name: "{{ deployer_user }}"
        shell: /bin/bash
        state: present
        create_home: yes

    - name: Add authorized key for deployer
      authorized_key:
        user: "{{ deployer_user }}"
        state: present
        key: "{{ deployer_pubkey }}"

    - name: Add deployer to sudoers
      copy:
        dest: "/etc/sudoers.d/{{ deployer_user }}"
        content: "{{ deployer_user }} ALL=(ALL) NOPASSWD:ALL"
        mode: '0440'
```

### 6. 🧪 Tested Ansible Ping
```bash
ansible -i inventory.ini rocky9 -m ping
```

### 7. 🛠️ Edited `/etc/sudoers` via `visudo`
```bash
sudo visudo
```
Added this line:
```
joshua.rhee ALL=(ALL) NOPASSWD:ALL
```

### 8. 🚀 Ran the Playbook
```bash
ansible-playbook -i inventory.ini create_deployer.yml
```

### 9. 🧪 Verified SSH into `deployer`
```bash
ssh deployer@10.0.3.10
sudo whoami  # Expected output: root
exit
```

## 📸 Recommended Screenshots

• `ssh-copy-id` success message  
• `ls ~/.ssh` showing key files  
• `ansible -m ping` showing `pong`  
• `visudo` with correct sudo line  
• Ansible playbook run output  
• SSH into `deployer`, running `sudo whoami`

## ✅ Outcome

The `deployer` user was successfully created and configured with:

• Key-based SSH login  
• Passwordless sudo privileges  
• Ready for automation and deployment tasks
