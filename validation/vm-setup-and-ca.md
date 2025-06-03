# Virtual Lab Setup & CA Integration – Notes

### 🧱 VM Environment Setup

This summer, I built out a virtual lab using VirtualBox to support cybersecurity automation and testing. I set up four main VMs:

- **Kali Linux** – for offensive testing
- **Rocky Linux** – hardened baseline server
- **Nessus** – external vulnerability scanner
- **VulnHub VMs** – targets for testing

To simulate an isolated but functional network, I used a mix of:
- **Internal Network (intnet)** for private communication
- **NAT** for internet access

Each VM was manually assigned a **static IP** to make connectivity predictable:
- Kali: `10.0.3.12`
- Nessus: `10.0.3.11`
- Rocky: `10.0.3.10`

Some commands I used to test:
```bash
ping 10.0.3.11
ip a
nmcli dev show
```

---

### 🔐 Internal Certificate Installation & Trust

To allow secure HTTPS access inside the VMs (without browser errors), I imported internal root and intermediate certs like:

- `CACIROOTCA-S2.pem`
- `CACIISSCA01A.pem`

On Linux, I used the system’s trust store with:

```bash
sudo trust anchor CACIROOTCA-S2.pem
sudo update-ca-trust extract
```

For browsers:
- **Chrome**: Used `certutil` with `~/.pki/nssdb`
- **Firefox**: Picked up from system store or imported manually

I also ran `trust list` to make sure the certificates were added correctly.

---

### 🌐 Proxy & Network Adjustments

Because of internal network policies, a lot of HTTPS traffic was being intercepted (MITM-style with trusted proxies). Without importing the right CA, I saw a lot of:

- `ERR_CERT_AUTHORITY_INVALID`
- `SSL_ERROR_BAD_CERT_DOMAIN`

After importing the CA certs, I was able to securely load internal web apps and Nessus dashboards inside both Firefox and Chrome.

---

### ✅ End Result

- Successfully set up secure browser sessions across VMs
- Internal CA trust fixed all major SSL-related roadblocks
- Enabled automated scanning, exploitation, and reporting without certificate errors blocking access
