# Zero2Root 🚀

> A practical privilege escalation cheatsheet for security professionals, penetration testers, and CTF players.

Zero2Root is a curated collection of privilege escalation techniques, commands, enumeration methods, and exploitation references for both Linux and Windows environments.

The purpose of this repository is to provide a quick reference during labs, Capture The Flag (CTF) competitions, and authorized penetration testing engagements.

> ⚠️ This repository is intended for educational purposes and authorized security assessments only.

---

## 📚 Contents

### Linux Privilege Escalation
- Basic Enumeration
- User & Group Enumeration
- Sudo Misconfigurations
- SUID/SGID Binaries
- Capabilities
- Cron Jobs
- PATH Hijacking
- Writable Files
- NFS Misconfigurations
- Kernel Exploits
- Docker & LXC Escapes
- Systemd Misconfigurations
- SSH Keys
- GTFOBins
- Checklist

### Windows Privilege Escalation
- Basic Enumeration
- User & Group Enumeration
- Windows Services
- Registry Misconfigurations
- AlwaysInstallElevated
- Unquoted Service Paths
- DLL Hijacking
- Scheduled Tasks
- Startup Applications
- Token Impersonation
- UAC Bypass
- Credential Hunting
- SeImpersonatePrivilege
- JuicyPotato / PrintSpoofer
- Checklist

### Active Directory
- Enumeration
- Kerberoasting
- AS-REP Roasting
- ACL Abuse
- Delegation Abuse
- BloodHound Notes
- Lateral Movement

### Containers
- Docker
- Kubernetes
- Container Escapes

### Cloud
- AWS
- Azure
- GCP

### Cheat Sheets
- Linux Commands
- Windows Commands
- PowerShell
- Bash
- Networking
- File Transfer
- Reverse Shell
- TTY Upgrade

---

# Repository Structure

```
Zero2Root/
│
├── linux/
│   ├── enumeration.md
│   ├── sudo.md
│   ├── suid.md
│   ├── capabilities.md
│   ├── cron.md
│   ├── docker.md
│   └── checklist.md
│
├── windows/
│   ├── enumeration.md
│   ├── services.md
│   ├── registry.md
│   ├── token.md
│   ├── uac.md
│   └── checklist.md
│
├── active-directory/
│
├── containers/
│
├── cloud/
│
├── cheatsheets/
│
├── resources/
│
└── README.md
```

---

# Goals

- Quick reference during penetration testing
- Practical privilege escalation notes
- Organized methodology
- Real-world techniques
- Easy to navigate
- Frequently updated

---

# References

Some excellent resources that inspired this repository:

- GTFOBins
- LOLBAS
- HackTricks
- PayloadsAllTheThings
- PEASS-ng
- Swissky's InternalAllTheThings

---

# Contributing

Contributions are welcome!

Feel free to submit:

- New techniques
- Better explanations
- Additional examples
- Bug fixes
- New references

---

# Disclaimer

This repository is provided for **educational purposes only**.

The techniques documented here should only be used in environments where you have explicit authorization to perform security testing. The author is not responsible for any misuse of the information provided.

---

## ⭐ Support

If you find this project useful, consider giving it a **⭐ Star** to support future updates.
