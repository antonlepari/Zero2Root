# LinPEAS Guide - Interpreting Privilege Escalation Findings

**Purpose:** Quick reference guide for understanding LinPEAS output and identifying privilege escalation vectors

---

## 📊 LinPEAS Color Legend

### 🔴 RED / 🟡 YELLOW
- **95% Privilege Escalation Vector**
- **Action:** MUST REVIEW IMMEDIATELY
- **What it means:** High likelihood of exploitability

**Examples:**
- CVE-2026-43284 LIKELY VULNERABLE
- DirtyPipe (CVE-2022-0847) VULNERABLE
- SUID binaries with known exploits

---

### 🔴 RED (Standalone)
- **You should take a look into it**
- **Severity:** HIGH PRIORITY
- **Action:** Investigate thoroughly

**Examples:**
- pkexec with SUID bit (CVE-2021-4034)
- Vulnerable Sudo version
- Exposed capabilities

---

### 🔵 LightCyan
- **Users with console access**
- **Type:** Information gathering
- **Relevance:** Understand who can interact with the system

**Examples:**
```
username      pts/0
root         tty1
```

---

### 🟦 Blue
- **Users without console & mounted devs**
- **Type:** System users
- **Relevance:** Low PE risk (usually service accounts)

**Examples:**
```
_apt (system user)
syslog (system user)
```

---

### 🟢 Green
- **Common things (users, groups, SUID/SGID, mounts, .sh scripts, cronjobs)**
- **Type:** Baseline findings
- **Action:** Document but not necessarily concerning

**Examples:**
- Standard SUID binaries (ls, cp, etc)
- Normal system processes
- Expected cron jobs

---

### 🟣 LightMagenta
- **Your username / Current user**
- **Type:** Context indicator
- **Relevance:** Shows current execution context

---

## 🔍 WHAT TO LOOK FOR IN LinPEAS OUTPUT

### 1. **Kernel Vulnerabilities Section**

**Location:** Early in output (after header)

**Key Indicators:**
```
CVE: CVE-XXXX-XXXXX | Name: [EXPLOIT_NAME] | Match data: [CONDITIONS]
```

**What to check:**
- Is there a `LIKELY VULNERABLE` or `VULNERABLE` marker?
- What kernel version is required?
- Are the required conditions met (e.g., `unprivileged_userns_clone=1`)?

**Example:**
```
🔴 CVE-2022-0847 | DirtyPipe
   Vulnerable kernel versions: 5.8 - 5.16.11
   Your kernel: 5.15.x ✓ MATCHES = VULNERABLE
```

---

### 2. **Sudo Configuration Section**

**Location:** After Kernel section

**Red Flags:**
- `NOPASSWD` entries → User can sudo without password
- Overly permissive rules (`ALL=(ALL) ALL`)
- Wildcards in sudoers

**Example (DANGEROUS):**
```bash
user ALL=(ALL) NOPASSWD: ALL
# = user can run ANY command as root without password
# = IMMEDIATE PRIVILEGE ESCALATION
```

**Safe Example:**
```bash
user ALL=(root) /usr/bin/systemctl restart nginx
# = user can ONLY restart nginx as root, requires password
```

---

### 3. **SUID/SGID Binaries Section**

**Location:** Middle section

**Red Flags:**
- Known vulnerable binaries (pkexec, snap-confine, etc)
- Unusual/unknown SUID binaries
- SUID + world-writable combination

**Format:**
```
-rwsr-xr-x 1 root root [SIZE] [DATE] /path/to/binary [CVE-INFO]
```

**Decode:**
- `s` in permissions = SUID bit set
- `root:root` = runs with root privileges
- Follow-up info = known CVE/exploit

---

### 4. **Capabilities Section**

**Location:** Privilege escalation section

**What it shows:**
- Binaries running with specific Linux capabilities
- Capabilities are fine-grained privileges vs full root

**Example (DANGEROUS):**
```
/usr/bin/tcpdump = cap_net_raw+ep
# tcpdump can capture packets (net_raw capability)
# Could be exploited to read sensitive data
```

**Safe Example:**
```
/usr/bin/ping = cap_net_raw+ep
# Expected - ping needs to send raw packets
```

---

### 5. **Cron Jobs Section**

**Location:** Process section

**What to look for:**
- Cron jobs running as root
- Cron jobs with writable scripts/binaries
- Cron jobs running from world-writable directories

**Example (DANGEROUS):**
```bash
root * * * * /tmp/backup.sh
# Running root-owned script from /tmp (world-writable)
# Anyone can modify /tmp/backup.sh and gain root
```

---

### 6. **NFS Shares Section**

**Location:** Mount section

**Red Flag:**
```
/tmp on /mnt type nfs (rw,no_root_squash)
# no_root_squash = root on client = root on server
# CRITICAL: Anyone with NFS access can become root
```

**Safe:**
```
/tmp on /mnt type nfs (rw,root_squash)
# root on client = nobody on server (restricted)
```

---

### 7. **Docker/Container Info**

**Location:** Container section

**Red Flags:**
- User is in docker group (can escape to host root)
- Docker socket is world-writable
- Privileged containers running

**Example (DANGEROUS):**
```
uid=1000(user) gid=1000(user) groups=1000(user),999(docker)
# User is in docker group → can run docker → privilege escalation
```

---

## 🎯 PRIORITIZATION MATRIX

### Tier 1: IMMEDIATE ACTION (Do Today)
```
CVE: LIKELY VULNERABLE / VULNERABLE
└─ Kernel CVEs
└─ pkexec/PwnKit
└─ PackageKit issues
└─ SUID exploits with PoC
```

### Tier 2: URGENT (This Week)
```
Sudo NOPASSWD entries
└─ Review sudoers
└─ Check command restrictions

SUID binaries
└─ Remove if not needed
└─ Update to patched version

Capabilities
└─ Restrict to bare minimum
```

### Tier 3: IMPORTANT (This Month)
```
Cron job audit
└─ Check permissions on scripts
└─ Verify scheduling necessity

NFS shares
└─ Review root_squash settings
└─ Restrict access

Container access
└─ Remove unnecessary docker group membership
```

### Tier 4: ONGOING (Continuous)
```
Monitor new CVEs
└─ Subscribe to CVE feeds

Apply security updates
└─ Enable automatic updates

Regular audits
└─ Run LinPEAS quarterly
└─ Review findings trends
```

---

## 💡 QUICK CHECKS

### After Running LinPEAS

**1. Count Critical Findings**
```bash
# Extract RED/YELLOW findings
grep -E "LIKELY VULNERABLE|VULNERABLE|CVE" linpeas.txt | wc -l
```

**2. Check Kernel Age**
```bash
uname -r
# Cross-reference with CVE list
```

**3. Review Sudo Config**
```bash
sudo visudo -c  # Check for syntax errors
sudo -l         # List your sudo privileges
```

**4. List SUID Binaries**
```bash
find / -perm -4000 -type f 2>/dev/null
# Cross-reference with LinPEAS findings
```

**5. Check Capabilities**
```bash
getcap -r / 2>/dev/null
# Identify binaries with unusual capabilities
```

---

## 🚀 EXPLOITATION CHECKLIST

**For Each RED Finding:**

- [ ] **Understand** what vulnerability is?
- [ ] **Verify** if conditions are met (kernel version, config, etc)
- [ ] **Check** if PoC exists (GitHub, Exploit-DB)
- [ ] **Test** in safe environment first
- [ ] **Document** findings with evidence
- [ ] **Report** to system owner
- [ ] **Remediate** based on priority

---

## 📋 EXAMPLE: Analyzing a Finding

**Raw LinPEAS Output:**
```
🔴 CVE: CVE-2026-43284 | xfrm-ESP
   LIKELY VULNERABLE to CVE-2026-43284 (xfrm-ESP)
   Unprivileged user namespaces: enabled
```

**Analysis Steps:**

1. **What is it?**
   - Kernel vulnerability in ESP (Encapsulating Security Payload)
   - Affects certain kernel versions

2. **Am I vulnerable?**
   ```bash
   uname -r
   cat /proc/sys/kernel/unprivileged_userns_clone
   # Both conditions must be TRUE for exploitation
   ```

3. **What's the impact?**
   - Unprivileged user can escalate to root
   - Can run arbitrary code as root

4. **How do I fix it?**
   - **Best:** Update kernel
   - **Alternative:** Disable unprivileged namespaces
   - **Temporary:** Disable vulnerable modules

5. **Action Plan:**
   ```bash
   # Immediate
   sudo sysctl -w kernel.unprivileged_userns_clone=0
   
   # This week
   sudo apt dist-upgrade -y && sudo reboot
   ```

---

## 🔗 RESOURCES

### LinPEAS Exploitation
- [HackTricks LinPEAS Analysis](https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html)
- [TryHackMe LinPEAS Course](https://tryhackme.com)

### CVE Databases
- [NVD (CVE Details)](https://nvd.nist.gov/vuln/full/record)
- [Exploit-DB](https://www.exploit-db.com)
- [GitHub CVE PoCs](https://github.com/search?q=CVE-2022-0847)

### Exploitation Frameworks
- [Metasploit](https://www.metasploit.com)
- [AutoSploit](https://github.com/NullArray/AutoSploit)
- [BeRoot](https://github.com/AlessandroZ/BeRoot)

---

## ⚠️ COMMON MISTAKES

**❌ DON'T:**
- Assume all RED findings are exploitable
- Run exploits on production without testing
- Ignore environmental differences (Docker vs bare metal)
- Skip the "verify vulnerability" step

**✅ DO:**
- Understand each finding in context
- Test in lab first
- Check kernel/OS compatibility
- Document everything

---

**Last Updated:** 2026-06-29  
**Applies to:** LinPEAS v14.x+
