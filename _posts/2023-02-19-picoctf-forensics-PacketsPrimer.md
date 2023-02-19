---
title: picoCTF | [100 points] [Forensics] Packets Primer WriteUp
date: 2023-02-19
categories: [picoCTF, Forensics]
---



# [100 points] Packets Primer WriteUp


# Tổng quan :

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Download the packet capture file and use packet analysis software to find the flag.

- [Download packet capture](https://artifacts.picoctf.net/c/199/network-dump.flag.pcap)

Hints : 

- Wireshark, if you can install and use it, is probably the most beginner friendly packet analysis software product.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ wget https://artifacts.picoctf.net/c/199/network-dump.flag.pcap
--2023-02-19 01:39:10--  https://artifacts.picoctf.net/c/199/network-dump.flag.pcap
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.45, 108.157.30.81, 108.157.30.86, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.45|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 778 [application/octet-stream]
Saving to: ‘network-dump.flag.pcap’

network-dump.flag. 100%[===============>]     778  --.-KB/s    in 0s      

2023-02-19 01:39:11 (19.1 MB/s) - ‘network-dump.flag.pcap’ saved [778/778]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ ls -alh 
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb 19 01:39 .
drwxr-xr-x 12 kali kali 4.0K Feb 19 01:39 ..
-rw-r--r--  1 kali kali  778 Mar 15  2022 network-dump.flag.pcap
```

# Khai thác và thu thập cờ (flag) :

Sử dụng `file` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ file network-dump.flag.pcap 
network-dump.flag.pcap: pcap capture file, microsecond ts (little-endian) - version 2.4 (Ethernet, capture length 262144)
```

Sử dụng `hexdump` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ hexdump -C network-dump.flag.pcap > hexdumppcap.txt

┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ cat hexdumppcap.txt            
00000000  d4 c3 b2 a1 02 00 04 00  00 00 00 00 00 00 00 00  |................|
00000010  00 00 04 00 01 00 00 00  6b 26 4e 61 72 bb 03 00  |........k&Nar...|
00000020  4a 00 00 00 4a 00 00 00  08 00 27 93 ce 73 08 00  |J...J.....'..s..|
00000030  27 af 39 9f 08 00 45 00  00 3c 50 c0 40 00 40 06  |'.9...E..<P.@.@.|
00000040  d1 e9 0a 00 02 0f 0a 00  02 04 be 6e 23 28 27 ec  |...........n#('.|
00000050  d4 b6 00 00 00 00 a0 02  fa f0 18 41 00 00 02 04  |...........A....|
00000060  05 b4 04 02 08 0a 8d cf  e9 64 00 00 00 00 01 03  |.........d......|
00000070  03 07 6b 26 4e 61 f2 be  03 00 4a 00 00 00 4a 00  |..k&Na....J...J.|
00000080  00 00 08 00 27 af 39 9f  08 00 27 93 ce 73 08 00  |....'.9...'..s..|
00000090  45 00 00 3c 00 00 40 00  40 06 22 aa 0a 00 02 04  |E..<..@.@.".....|
000000a0  0a 00 02 0f 23 28 be 6e  bd 26 99 bb 27 ec d4 b7  |....#(.n.&..'...|
000000b0  a0 12 fe 88 2a 4f 00 00  02 04 05 b4 04 02 08 0a  |....*O..........|
000000c0  68 f0 f1 c3 8d cf e9 64  01 03 03 07 6b 26 4e 61  |h......d....k&Na|
000000d0  60 bf 03 00 42 00 00 00  42 00 00 00 08 00 27 93  |`...B...B.....'.|
000000e0  ce 73 08 00 27 af 39 9f  08 00 45 00 00 34 50 c1  |.s..'.9...E..4P.|
000000f0  40 00 40 06 d1 f0 0a 00  02 0f 0a 00 02 04 be 6e  |@.@............n|
00000100  23 28 27 ec d4 b7 bd 26  99 bc 80 10 01 f6 18 39  |#('....&.......9|
00000110  00 00 01 01 08 0a 8d cf  e9 65 68 f0 f1 c3 6b 26  |.........eh...k&|
00000120  4e 61 3b c0 03 00 7e 00  00 00 7e 00 00 00 08 00  |Na;...~...~.....|
00000130  27 93 ce 73 08 00 27 af  39 9f 08 00 45 00 00 70  |'..s..'.9...E..p|
00000140  50 c2 40 00 40 06 d1 b3  0a 00 02 0f 0a 00 02 04  |P.@.@...........|
00000150  be 6e 23 28 27 ec d4 b7  bd 26 99 bc 80 18 01 f6  |.n#('....&......|
00000160  18 75 00 00 01 01 08 0a  8d cf e9 65 68 f0 f1 c3  |.u.........eh...|
00000170  70 20 69 20 63 20 6f 20  43 20 54 20 46 20 7b 20  |p i c o C T F { |
00000180  70 20 34 20 63 20 6b 20  33 20 37 20 5f 20 35 20  |p 4 c k 3 7 _ 5 |
00000190  68 20 34 20 72 20 6b 20  5f 20 63 20 65 20 63 20  |h 4 r k _ c e c |
000001a0  63 20 61 20 61 20 37 20  66 20 7d 0a 6b 26 4e 61  |c a a 7 f }.k&Na|
000001b0  61 c3 03 00 42 00 00 00  42 00 00 00 08 00 27 af  |a...B...B.....'.|
000001c0  39 9f 08 00 27 93 ce 73  08 00 45 00 00 34 67 e1  |9...'..s..E..4g.|
000001d0  40 00 40 06 ba d0 0a 00  02 04 0a 00 02 0f 23 28  |@.@...........#(|
000001e0  be 6e bd 26 99 bc 27 ec  d4 f3 80 10 01 fd 55 69  |.n.&..'.......Ui|
000001f0  00 00 01 01 08 0a 68 f0  f1 c4 8d cf e9 65 70 26  |......h......ep&|
00000200  4e 61 28 0b 04 00 3c 00  00 00 3c 00 00 00 08 00  |Na(...<...<.....|
00000210  27 af 39 9f 08 00 27 93  ce 73 08 06 00 01 08 00  |'.9...'..s......|
00000220  06 04 00 01 08 00 27 93  ce 73 0a 00 02 04 00 00  |......'..s......|
00000230  00 00 00 00 0a 00 02 0f  00 00 00 00 00 00 00 00  |................|
00000240  00 00 00 00 00 00 00 00  00 00 70 26 4e 61 58 0b  |..........p&NaX.|
00000250  04 00 2a 00 00 00 2a 00  00 00 08 00 27 93 ce 73  |..*...*.....'..s|
00000260  08 00 27 af 39 9f 08 06  00 01 08 00 06 04 00 02  |..'.9...........|
00000270  08 00 27 af 39 9f 0a 00  02 0f 08 00 27 93 ce 73  |..'.9.......'..s|
00000280  0a 00 02 04 70 26 4e 61  32 38 04 00 2a 00 00 00  |....p&Na28..*...|
00000290  2a 00 00 00 08 00 27 93  ce 73 08 00 27 af 39 9f  |*.....'..s..'.9.|
000002a0  08 06 00 01 08 00 06 04  00 01 08 00 27 af 39 9f  |............'.9.|
000002b0  0a 00 02 0f 00 00 00 00  00 00 0a 00 02 04 70 26  |..............p&|
000002c0  4e 61 a8 3b 04 00 3c 00  00 00 3c 00 00 00 08 00  |Na.;..<...<.....|
000002d0  27 af 39 9f 08 00 27 93  ce 73 08 06 00 01 08 00  |'.9...'..s......|
000002e0  06 04 00 02 08 00 27 93  ce 73 0a 00 02 04 08 00  |......'..s......|
000002f0  27 af 39 9f 0a 00 02 0f  00 00 00 00 00 00 00 00  |'.9.............|
00000300  00 00 00 00 00 00 00 00  00 00                    |..........|
0000030a
```

Có thể sử dụng Wireshark để xem nội dung của các gói tin bắt được.

```bash
1	0.000000	10.0.2.15	10.0.2.4	TCP	74	48750 → 9000 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM TSval=2379213156 TSecr=0 WS=128
2	0.000896	10.0.2.4	10.0.2.15	TCP	74	9000 → 48750 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460 SACK_PERM TSval=1760620995 TSecr=2379213156 WS=128
3	0.001006	10.0.2.15	10.0.2.4	TCP	66	48750 → 9000 [ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=2379213157 TSecr=1760620995
4	0.001225	10.0.2.15	10.0.2.4	TCP	126	48750 → 9000 [PSH, ACK] Seq=1 Ack=1 Win=64256 Len=60 TSval=2379213157 TSecr=1760620995
5	0.002031	10.0.2.4	10.0.2.15	TCP	66	9000 → 48750 [ACK] Seq=1 Ack=61 Win=65152 Len=0 TSval=1760620996 TSecr=2379213157
```

Sử dụng `strings` :

→ Cách này đơn giản hơn mà mình cũng có thể thấy được nội dung của cờ (flag).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ strings network-dump.flag.pcap 
k&Nar
n#('
k&Na
k&Na`
n#('
k&Na;
n#('
p i c o C T F { p 4 c k 3 7 _ 5 h 4 r k _ c e c c a a 7 f }
k&Naa
ep&Na(
p&NaX
p&Na28
p&Na
```

Mình có thể thấy được nội dung của cờ (flag) :

→ Loại bỏ các khoảng trắng đi thì có thể đó là nội dung cần tìm.

```bash
p i c o C T F { p 4 c k 3 7 _ 5 h 4 r k _ c e c c a a 7 f }
```

Mình sử dụng python và hàm replace() để xóa các khoảng trống trong chuỗi.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Packets Primer]
└─$ python
Python 3.11.1 (main, Dec 31 2022, 10:23:59) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> s = "p i c o C T F { p 4 c k 3 7 _ 5 h 4 r k _ c e c c a a 7 f }"
>>> s.replace(" ", "")
'picoCTF{p4ck37_5h4rk_ceccaa7f}'
>>>
```

# Tổng kết :

→ Nội dung cờ (flag) thu được : `picoCTF{p4ck37_5h4rk_ceccaa7f}`

# Tài liệu tham khảo :

[https://www.javatpoint.com/linux-strings-command#:~:text=Linux strings command is used,text from an executable file](https://www.javatpoint.com/linux-strings-command#:~:text=Linux%20strings%20command%20is%20used,text%20from%20an%20executable%20file).

[https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowStreamSection.html)