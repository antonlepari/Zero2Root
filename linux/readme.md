# Hasil LinPEAS 
## Contoh 1

![Hasil LinPEAS-1](../assets/images/linpeas-1.png)

# System Information

## Operative System (T1082)

> https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#kernel-exploits

```text
Linux version 5.15.0-178-generic (buildd@lcy02-amd64-113)
gcc (Ubuntu 11.4.0-1ubuntu1~22.04.3) 11.4.0
GNU ld (GNU Binutils for Ubuntu) 2.38
#188-Ubuntu SMP Sun Apr 12 07:19:49 UTC 2026

Distributor ID: Ubuntu
Description: Ubuntu 22.04.5 LTS
Release: 22.04
Codename: jammy
```

---

## Sudo Version (T1548.003, T1068)

> https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-version

```text
Sudo version 1.9.9
```

---

## PATH (T1574.007)

> https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#writable-path-abuses

```text
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
/snap/bin
```

---

## Date & Uptime (T1082)

```text
Date : Mon Jun 29 10:35:00 UTC 2026
Uptime: 1 hour 32 minutes

Load Average:
- 0.23
- 0.05
- 0.06
```

---

## Environment Variables (T1082, T1552.007)

```text
SHLVL=1
OLDPWD=/
LC_CTYPE=C.UTF-8
APACHE_RUN_DIR=/var/run/apache2
APACHE_PID_FILE=/var/run/apache2/apache2.pid
APACHE_LOCK_DIR=/var/lock/apache2
LANG=C
APACHE_RUN_GROUP=www-data
APACHE_RUN_USER=www-data
APACHE_LOG_DIR=/var/log/apache2
PWD=/tmp
```

---

# Protections (T1518.001)

| Feature | Status |
|---------|--------|
| AppArmor | Enabled |
| AppArmor Profile | unconfined |
| SELinux | Not Installed |
| Seccomp | Disabled |
| User Namespace | Enabled |
| unpriv_userns_clone | 1 |
| ASLR | Enabled |
| Cgroup v2 | Enabled |
| ptrace_scope | 1 |
| dmesg_restrict | 1 |
| kptr_restrict | 1 |
| Lockdown Mode | integrity / confidentiality |
| Virtual Machine | Oracle VM |

### Kernel Hardening

```text
CONFIG_SLAB_FREELIST_RANDOM=y
CONFIG_SLAB_FREELIST_HARDENED=y
CONFIG_RANDOMIZE_BASE=y
CONFIG_STACKPROTECTOR=y
CONFIG_STACKPROTECTOR_STRONG=y
```

---

# Kernel Modules

## Loadable Modules

```text
Modules can be loaded
```

## Signature Enforcement

```text
Not enforced
```

---

# Kernel Exploit Registry

| CVE | Name | Status |
|-----|------|--------|
| CVE-2022-0847 | DirtyPipe | Match |
| CVE-2022-0995 | watch_queue | Match |
| CVE-2022-2586 | nft_object UAF | Match |
| CVE-2022-32250 | NFT_MSG_NEWSET | Match |
| CVE-2023-0386 | OverlayFS SUID Smuggle | Match |
| CVE-2026-46333 | ptrace exit-race | Match |

**Kernel vulnerabilities detected:** **6**

---

# Dirty Frag Check

## CVE-2026-43284

```text
LIKELY VULNERABLE
```

## CVE-2026-43500

```text
LIKELY VULNERABLE
```

### Notes

- xfrm-ESP module autoloadable
- rxrpc module autoloadable
- No modprobe mitigation detected
- User namespaces enabled
- Kernel build predates upstream fix

### Suggested Mitigations

- Disable esp4/esp6/rxrpc via `/etc/modprobe.d/`
- Disable unprivileged user namespaces
- Apply latest Ubuntu kernel patches
