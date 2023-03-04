---
title: picoCTF | [300 points] [Forensics]  Eavesdrop WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---

# [300 points] [Forensics] Eavesdrop WriteUp



# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải về có chứa một đoạn hội thoại và một tập tin chứa thông tin để tìm được nội dung của cờ.
- Sử dụng câu lệnh mã hóa liên quan đến openssl để tìm được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{nc_73115_411_5786acc3}`

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Download this packet capture and find the flag.

- [Download packet capture](https://artifacts.picoctf.net/c/358/capture.flag.pcap)

Hints :

- All we know is that this packet capture includes a chat conversation and a file transfer.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ wget https://artifacts.picoctf.net/c/358/capture.flag.pcap
--2023-03-04 13:27:46--  https://artifacts.picoctf.net/c/358/capture.flag.pcap
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.86, 108.157.30.63, 108.157.30.45, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.86|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7518 (7.3K) [application/octet-stream]
Saving to: ‘capture.flag.pcap’

capture.flag.pcap  100%[===============>]   7.34K  --.-KB/s    in 0s      

2023-03-04 13:27:47 (74.4 MB/s) - ‘capture.flag.pcap’ saved [7518/7518]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ ls -alh 
total 16K
drwxr-xr-x  2 kali kali 4.0K Mar  4 13:27 .
drwxr-xr-x 21 kali kali 4.0K Mar  4 13:27 ..
-rw-r--r--  1 kali kali 7.4K Mar 15  2022 capture.flag.pcap
```

# Khai thác và thu thập cờ (flag) :

Do gợi ý là sẽ có một cuộc hội thoại và 1 tập tin trong các gói tin bắt được. Như thường lệ đối với khi dùng WireShark thì mình sẽ thử `Follow` > `TCP Stream` trên thanh Analyze hoặc có thể dùng Filter là `tcp.stream eq <Số thứ tự>` thì ngay đối với lần đầu tiên (tức `tcp.stream eq 0`) đã cho thấy nội dung của cuộc hội thoại.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ tshark -r  capture.flag.pcap -Y 'tcp.stream eq 0'
    5  15.175413    10.0.2.15 → 10.0.2.4     TCP 74 57876 → 9001 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM TSval=3517230404 TSecr=0 WS=128
    6  15.176383     10.0.2.4 → 10.0.2.15    TCP 74 9001 → 57876 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460 SACK_PERM TSval=1765680570 TSecr=3517230404 WS=128
    7  15.176456    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=3517230405 TSecr=1765680570
   12  31.182138     10.0.2.4 → 10.0.2.15    TCP 107 9001 → 57876 [PSH, ACK] Seq=1 Ack=1 Win=65280 Len=41 TSval=1765696576 TSecr=3517230405
   13  31.182202    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=1 Ack=42 Win=64256 Len=0 TSval=3517246411 TSecr=1765696576
   14  38.502063    10.0.2.15 → 10.0.2.4     TCP 82 57876 → 9001 [PSH, ACK] Seq=1 Ack=42 Win=64256 Len=16 TSval=3517253731 TSecr=1765696576
   15  38.502398     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=42 Ack=17 Win=65280 Len=0 TSval=1765703897 TSecr=3517253731
   16  48.150243     10.0.2.4 → 10.0.2.15    TCP 84 9001 → 57876 [PSH, ACK] Seq=42 Ack=17 Win=65280 Len=18 TSval=1765713544 TSecr=3517253731
   17  48.150305    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=17 Ack=60 Win=64256 Len=0 TSval=3517263379 TSecr=1765713544
   18  97.822725    10.0.2.15 → 10.0.2.4     TCP 149 57876 → 9001 [PSH, ACK] Seq=17 Ack=60 Win=64256 Len=83 TSval=3517313052 TSecr=1765713544
   19  97.823024     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=60 Ack=100 Win=65280 Len=0 TSval=1765763217 TSecr=3517313052
   24 107.903137     10.0.2.4 → 10.0.2.15    TCP 85 9001 → 57876 [PSH, ACK] Seq=60 Ack=100 Win=65280 Len=19 TSval=1765773297 TSecr=3517313052
   25 107.903169    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=100 Ack=79 Win=64256 Len=0 TSval=3517323132 TSecr=1765773297
   26 121.932953    10.0.2.15 → 10.0.2.4     TCP 113 57876 → 9001 [PSH, ACK] Seq=100 Ack=79 Win=64256 Len=47 TSval=3517337162 TSecr=1765773297
   27 121.933774     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=79 Ack=147 Win=65280 Len=0 TSval=1765787328 TSecr=3517337162
   28 134.662151     10.0.2.4 → 10.0.2.15    TCP 117 9001 → 57876 [PSH, ACK] Seq=79 Ack=147 Win=65280 Len=51 TSval=1765800056 TSecr=3517337162
   29 134.662230    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=147 Ack=130 Win=64256 Len=0 TSval=3517349891 TSecr=1765800056
   30 141.449380    10.0.2.15 → 10.0.2.4     TCP 76 57876 → 9001 [PSH, ACK] Seq=147 Ack=130 Win=64256 Len=10 TSval=3517356678 TSecr=1765800056
   31 141.449743     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=130 Ack=157 Win=65280 Len=0 TSval=1765806844 TSecr=3517356678
   32 145.480483     10.0.2.4 → 10.0.2.15    TCP 71 9001 → 57876 [PSH, ACK] Seq=130 Ack=157 Win=65280 Len=5 TSval=1765810874 TSecr=3517356678
   33 145.480560    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=157 Ack=135 Win=64256 Len=0 TSval=3517360710 TSecr=1765810874
   34 149.866335    10.0.2.15 → 10.0.2.4     TCP 72 57876 → 9001 [PSH, ACK] Seq=157 Ack=135 Win=64256 Len=6 TSval=3517365095 TSecr=1765810874
   35 149.866683     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=135 Ack=163 Win=65280 Len=0 TSval=1765815260 TSecr=3517365095
   36 163.189845     10.0.2.4 → 10.0.2.15    TCP 107 9001 → 57876 [PSH, ACK] Seq=135 Ack=163 Win=65280 Len=41 TSval=1765828583 TSecr=3517365095
   37 163.189875    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=163 Ack=176 Win=64256 Len=0 TSval=3517378419 TSecr=1765828583
   48 182.468120    10.0.2.15 → 10.0.2.4     TCP 91 57876 → 9001 [PSH, ACK] Seq=163 Ack=176 Win=64256 Len=25 TSval=3517397697 TSecr=1765828583
   49 182.468958     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=176 Ack=188 Win=65280 Len=0 TSval=1765847862 TSecr=3517397697
   52 197.944312     10.0.2.4 → 10.0.2.15    TCP 83 9001 → 57876 [PSH, ACK] Seq=176 Ack=188 Win=65280 Len=17 TSval=1765863336 TSecr=3517397697
   53 197.944369    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=188 Ack=193 Win=64256 Len=0 TSval=3517413173 TSecr=1765863336
   59 212.168371    10.0.2.15 → 10.0.2.4     TCP 74 57876 → 9001 [PSH, ACK] Seq=188 Ack=193 Win=64256 Len=8 TSval=3517427397 TSecr=1765863336
   60 212.169557     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=193 Ack=196 Win=65280 Len=0 TSval=1765877561 TSecr=3517427397
   64 227.003581     10.0.2.4 → 10.0.2.15    TCP 74 9001 → 57876 [PSH, ACK] Seq=193 Ack=196 Win=65280 Len=8 TSval=1765892395 TSecr=3517427397
   65 227.004032    10.0.2.15 → 10.0.2.4     TCP 66 57876 → 9001 [ACK] Seq=196 Ack=201 Win=64256 Len=0 TSval=3517442233 TSecr=1765892395
   72 239.414765    10.0.2.15 → 10.0.2.4     TCP 86 57876 → 9001 [PSH, ACK] Seq=196 Ack=201 Win=64256 Len=20 TSval=3517454644 TSecr=1765892395
   73 239.415608     10.0.2.4 → 10.0.2.15    TCP 66 9001 → 57876 [ACK] Seq=201 Ack=216 Win=65280 Len=0 TSval=1765904807 TSecr=3517454644
```

Cuộc hội thoại sẽ có nội dung như bên dưới, cụ thể là A sẽ hỏi B cách mã hóa một tập tin và yêu cầu B gửi một tập tin để A có thể mã hóa nó bằng cách thức vừa hỏi. Như vậy, mình có thể sử dụng câu lệnh đó để mã hóa tập tin mà B gửi cho A.

```bash
Hey, how do you decrypt this file again?
You're serious?
Yeah, I'm serious
*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
Ok, great, thanks.
Let's use Discord next time, it's more secure.
C'mon, no one knows we use this program like this!
Whatever.
Hey.
Yeah?
Could you transfer the file to me again?
Oh great. Ok, over 9002?
Yeah, listening.
Sent it
Got it.
You're unbelievable
```

Để tìm được tập tin thì có thể dựa vào port (do trong đoạn hội thoại có đề cập đến port 9002) nên mình có thể chỉ tập trung vào port 9002 thôi.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ tshark -r  capture.flag.pcap -Y 'tcp.port==9002' 
   54 205.301478    10.0.2.15 → 10.0.2.4     TCP 74 56370 → 9002 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM TSval=3517420531 TSecr=0 WS=128
   55 205.302375     10.0.2.4 → 10.0.2.15    TCP 74 9002 → 56370 [SYN, ACK] Seq=0 Ack=1 Win=65160 Len=0 MSS=1460 SACK_PERM TSval=1765870695 TSecr=3517420531 WS=128
   56 205.302451    10.0.2.15 → 10.0.2.4     TCP 66 56370 → 9002 [ACK] Seq=1 Ack=1 Win=64256 Len=0 TSval=3517420531 TSecr=1765870695
   57 205.302713    10.0.2.15 → 10.0.2.4     TCP 114 56370 → 9002 [PSH, ACK] Seq=1 Ack=1 Win=64256 Len=48 TSval=3517420532 TSecr=1765870695
   58 205.303662     10.0.2.4 → 10.0.2.15    TCP 66 9002 → 56370 [ACK] Seq=1 Ack=49 Win=65152 Len=0 TSval=1765870696 TSecr=3517420532
   61 217.183803     10.0.2.4 → 10.0.2.15    TCP 66 9002 → 56370 [FIN, ACK] Seq=1 Ack=49 Win=65152 Len=0 TSval=1765882575 TSecr=3517420532
   62 217.184036    10.0.2.15 → 10.0.2.4     TCP 66 56370 → 9002 [FIN, ACK] Seq=49 Ack=2 Win=64256 Len=0 TSval=3517432413 TSecr=1765882575
   63 217.184826     10.0.2.4 → 10.0.2.15    TCP 66 9002 → 56370 [ACK] Seq=2 Ack=50 Win=65152 Len=0 TSval=1765882577 TSecr=3517432413
```

Tiếp tục `Follow` > `TCP Stream` thì có thể thu được nội dung như bên dưới :

```bash
Salted__............=a.....Z..........F8..v.<8EY
```

Chuyển chúng thành Raw (nếu dùng wireShark thì tại vị trí `Show data as` > `Raw`)

```bash
53616c7465645f5ff1a4a681bfd0cad4859b951d3d61ca1df9a28d5af0c9180014157fe0a9c446380ba176f23c384559
```

Lưu ý: Khi tải tập tin về thì đừng có copy mã vào một tập tin txt mà dùng nút Save As để lưu tập tin về. Sau khi tải về thì có thể đặt tên là file.des3 để sử dụng lại câu lệnh mã hóa. Sử dụng câu lệnh mã hóa là có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
    
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ ls -alh 
total 24K
drwxr-xr-x  2 kali kali 4.0K Mar  4 13:41 .
drwxr-xr-x 21 kali kali 4.0K Mar  4 13:27 ..
-rw-r--r--  1 kali kali 7.4K Mar 15  2022 capture.flag.pcap
-rw-------  1 kali kali   48 Mar  4 13:41 file.des3
-rw-r--r--  1 kali kali   30 Mar  4 13:41 file.txt
                                                                       
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] Eavesdrop]
└─$ cat file.txt
picoCTF{nc_73115_411_5786acc3}                                                                           

```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{nc_73115_411_5786acc3}`

# Tài liệu tham khảo :

[https://stackoverflow.com/questions/34833644/tshark-follow-tcp-stream-upon-condition](https://stackoverflow.com/questions/34833644/tshark-follow-tcp-stream-upon-condition)