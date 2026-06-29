# Hasil LinPEAS 
## Contoh 1

![Hasil LinPEAS-1](../assets/images/linpeas-1.png)

╔══════════╣ Kernel Exploit Registry (T1068)
═╣ Operating system ............. Linux
═╣ Kernel release ............... 5.15.0-178-generic
═╣ Comparable version ........... 5.15.0.178
═╣ Data chunk limit ............. max 25 rows per KERNEL_CVE_DATA_* variable (1..22)
═╣ Kernel config source ......... /boot/config-5.15.0-178-generic
CVE: CVE-2022-0847 | Name: DirtyPipe | Match data: pkg=linux-kernel,ver>=5.8,ver<=5.16.11 | Tags: ubuntu=(20.04|21.04),debian=11 | Rank: 1
CVE: CVE-2022-0995 | Name: watch_queue | Match data: pkg=linux-kernel,ver>=5.8,ver<5.16.5,x86_64 | Tags: ubuntu=21.10{kernel:5.13.0.37-generic} | Rank: 1 | Details: Not 100% reliable, may need to be run a couple of times. It rare cases it may panic the kernel.
CVE: CVE-2022-2586 | Name: nft_object UAF | Match data: pkg=linux-kernel,ver>=5.12,ver<5.19,CONFIG_USER_NS=y,sysctl:kernel.unprivileged_userns_clone==1 | Tags: ubuntu=(20.04){kernel:5.12.13} | Rank: 1 | Details: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)
CVE: CVE-2022-32250 | Name: nft_object UAF (NFT_MSG_NEWSET) | Match data: pkg=linux-kernel,ver<5.18.1,CONFIG_USER_NS=y,sysctl:kernel.unprivileged_userns_clone==1 | Tags: ubuntu=(22.04){kernel:5.15.0-27-generic} | Rank: 1 | Details: kernel.unprivileged_userns_clone=1 required (to obtain CAP_NET_ADMIN)
CVE: CVE-2023-0386 | Name: OverlayFS suid smuggle | Match data: pkg=linux-kernel,ver>=5.11,ver<=6.2,CONFIG_USER_NS=y,sysctl:kernel.unprivileged_userns_clone==1 | Tags: ubuntu=22.04.1{kernel:5.15.0-57-generic} | Rank: 1 | Details: CONFIG_USER_NS needs to be enabled && kernel.unprivileged_userns_clone=1 required
CVE: CVE-2026-46333 | Name: ptrace exit-race | Match data: pkg=linux-kernel,ver>=5.11,ver<5.15.207,cmd:[ "$(cat /proc/sys/kernel/yama/ptrace_scope 2>/dev/null || echo 0)" -lt 2 ] | Tags: 1 | Rank: Upstream issue introduced in 4.10; fixed in 5.15.207; mitigated by kernel.yama.ptrace_scope >= 2
═╣ Kernel vulns found: 6
