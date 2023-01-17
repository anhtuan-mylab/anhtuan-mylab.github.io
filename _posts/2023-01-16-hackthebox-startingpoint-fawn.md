---
title: HackTheBox | StartingPoint | Fawn WriteUp
date: 2023-01-16 
categories: [HackTheBox, StartingPoint]
---

# [StartingPoint] Fawn WriteUp

# 1. Trả lời câu hỏi :

**What does the 3-letter acronym FTP stand for?**

→ File Transfer Protocol

**Which port does the FTP service listen on usually?**

→ 21

**What acronym is used for the secure version of FTP?**

→ SFTP

**What is the command we can use to send an ICMP echo request to test our connection to the target?**

→ ping

**From your scans, what version is FTP running on the target?**

→ vsftpd 3.0.3

**From your scans, what OS type is running on the target?**

→ Unix

**What is the command we need to run in order to display the 'ftp' client help menu?**

→ ftp -h

**What is username that is used over FTP when you want to log in without having an account?**

→ anonymous

**What is the response code we get for the FTP message 'Login successful'?**

→ 230

**There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.**

→ ls

**What is the command used to download the file we found on the FTP server?**

→ get

# 2. Thu thập Flag từ máy ảo :

## Tổng quan về máy ảo :

Kết nối đến máy ảo :

Tải tập tin *.ovpn, sử dụng câu lệnh để kết nối đến dịch vụ của HackTheBox thông qua openvpn

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox]
└─$ sudo openvpn --config /home/kali/Desktop/hackthebox/starting_point_Darko2212.ovpn
```

Thực hiện bấm nút **“SPAMW MACHINE”** để khởi động máy ảo, có thể bấm “Reset machine” để khởi động lại hoặc “Stop Machine” để tắt máy ảo.

## Kiểm tra tính kết nối :

Sử dụng lệnh ping đến IP của máy ảo : 10.129.104.21

```bash
┌──(kali㉿kali)-[~]
└─$ ping 10.129.104.21
PING 10.129.104.21 (10.129.104.21) 56(84) bytes of data.
64 bytes from 10.129.104.21: icmp_seq=1 ttl=63 time=212 ms
64 bytes from 10.129.104.21: icmp_seq=2 ttl=63 time=211 ms
64 bytes from 10.129.104.21: icmp_seq=3 ttl=63 time=210 ms
^C
--- 10.129.104.21 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 210.483/211.254/212.196/0.709 ms
```

Thực hiện các yêu cầu :

- Thu thập thông tin
- Khai thác lỗ hổng và leo thang đặc quyền (nếu có).
- Thu thập Flags.

# 3. Thu thập thông tin:

## Sử dụng nmap (tìm các cổng và dịch vụ đang hoạt động) :

→ tìm được dịch vụ FTP đang hoạt động.

→ có thể truy cập bằng anonymous (truy cập không cần mật khẩu).

→ quá trình quét thể hiện tập tin flags.txt trong tập chi sẻ của ftp.

```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV -Pn 10.129.104.21
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-17 00:50 EST
Nmap scan report for 10.129.104.21
Host is up (0.20s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.156
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.60 seconds
```

# 4. Khai thác lỗ hổng :

Từ việc tìm kiếm thông tin từ nmap cho thấy có thể truy cập dịch vụ FTP bằng tài khoản “anonymous”.

```bash
┌──(kali㉿kali)-[~]
└─$ ftp 10.129.104.21
Connected to 10.129.104.21.
220 (vsFTPd 3.0.3)
Name (10.129.104.21:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

Hiển thị các nội dung bằng lệnh “ls”.

```bash
ftp> ls
229 Entering Extended Passive Mode (|||31050|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

Thực hiện tải tập tin về máy bằng lệnh “get flag.txt”.

```bash
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||56557|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |**********************************************************************|    32       41.06 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.15 KiB/s)
```

Lúc này thì tập tin flag.txt đã được tải về máy và có thể truy cập để đọc nội dung của flag.txt

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/fawn]
└─$ ls -alg            
total 12
drwxr-xr-x  2 kali 4096 Jan 17 00:56 .
drwxr-xr-x 11 kali 4096 Jan 17 00:56 ..
-rw-r--r--  1 kali   32 Jun  3  2021 flag.txt
```

# 5. Leo thang đặc quyền :

Không cần thiết do đã lấy được flag.txt

# 6. Thu thập Flags:

Tải tập tin flag.txt vừa tải được về đọc để lấy nội dung của flag.

# 7. Tài liệu tham khảo :

[https://www.geeksforgeeks.org/nmap-command-in-linux-with-examples/](https://www.geeksforgeeks.org/nmap-command-in-linux-with-examples/)

[https://www.serv-u.com/linux-ftp-server/commands](https://www.serv-u.com/linux-ftp-server/commands)