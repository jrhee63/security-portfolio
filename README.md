# security-portfolio

## 🖥️ Virtual Lab Setup & Certificate Authority Trust
- Deployed Kali Linux, Rocky Linux, Nessus, and VulnHub VMs using VirtualBox
- Configured `intnet` and NAT/bridged networking with **static IPs** for segmentation
- Validated inter-VM connectivity via `ping`, `ip a`, and `nmcli`
- Installed internal corporate CAs (e.g., `CACIROOTCA-S2.pem`) into Linux trust stores
- Integrated certificates with **Chrome** and **Firefox** using:
  - `certutil -d sql:$HOME/.pki/nssdb -A -n "..." -i cert.pem -t "C,,"`
  - `sudo trust anchor cert.pem && sudo update-ca-trust extract`
- Resolved SSL and HTTPS trust issues in enterprise network environments
