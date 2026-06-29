# LinPEAS

## Overview

LinPEAS (Linux Privilege Escalation Awesome Script) is one of the most widely used enumeration tools for identifying potential privilege escalation vectors on Linux systems.

It automates the discovery of common misconfigurations, insecure permissions, credentials, kernel information, and other findings that may lead to privilege escalation.

## Features

- System Information
- Users & Groups
- Sudo Privileges
- SUID / SGID Binaries
- Linux Capabilities
- Cron Jobs
- Services
- Writable Files & Directories
- Credentials
- SSH Keys
- Docker / LXC
- Network Information
- Kernel Information
- Interesting Files

## Usage

```bash
chmod +x linpeas.sh
./linpeas.sh
```

Or execute directly:

```bash
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

## Output

Pay special attention to findings highlighted in:

- Red
- Yellow
- Green

These colors indicate potential privilege escalation opportunities.

## Related

- Linux Enumeration
- SUID
- Capabilities
- Cron Jobs
- Docker Escape

## Reference

- https://github.com/peass-ng/PEASS-ng
