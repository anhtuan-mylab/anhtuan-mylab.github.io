---
title: picoCTF | [350 points] [Forensics] WebNet0 WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [350 points] [Forensics] WebNet0 WriteUp



# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải bao gồm tập tin chứa gói tin bắt được và tập tin chứa khóa.
- Sử dụng khóa để giải mã giao thức TLS (hiển thị các tập tin giao thức HTTP).
- Nội dung của cờ được chứa bên trong các tập tin giao thức HTTP.

Nội dung của cờ : `picoCTF{nongshim.shrimp.crackers}`

## Tác giả và mô tả :

AUTHOR: JASON

Description : We found this [packet capture](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap) and [key](https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key). Recover the flag.

Hints :

- Try using a tool like Wireshark.
- How can you decrypt the TLS stream ?

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ wget https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap
--2023-03-04 21:33:10--  https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/capture.pcap
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13163 (13K) [application/octet-stream]
Saving to: ‘capture.pcap’

capture.pcap       100%[===============>]  12.85K  --.-KB/s    in 0s      

2023-03-04 21:33:11 (57.3 MB/s) - ‘capture.pcap’ saved [13163/13163]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ wget https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key
--2023-03-04 21:33:27--  https://jupiter.challenges.picoctf.org/static/0c84d3636dd088d9fe4efd5d0d869a06/picopico.key
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1704 (1.7K) [application/octet-stream]
Saving to: ‘picopico.key’

picopico.key       100%[===============>]   1.66K  --.-KB/s    in 0s      

2023-03-04 21:33:28 (40.0 MB/s) - ‘picopico.key’ saved [1704/1704]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ ls -alh
total 28K
drwxr-xr-x  2 kali kali 4.0K Mar  4 21:33 .
drwxr-xr-x 22 kali kali 4.0K Mar  4 21:33 ..
-rw-r--r--  1 kali kali  13K Oct 26  2020 capture.pcap
-rw-r--r--  1 kali kali 1.7K Oct 26  2020 picopico.key
```

# Khai thác và thu thập cờ (flag) :

Tập tin tải về chứa các gói tin bắt được. Ban đầu, các gói tin bắt được chỉ có giao thức là TCP và TLSv1.2 nên phải sử dụng key tải kèm để mã hóa. Các bước để giải mã bằng wireshak thì có thể tham khảo trên mạng, cụ thể có những bước sau :

- Vào Edit > Preferences.
- Trong mục Protocols > TLS.
- Trong phần Transport Layer Seurity, chọn đường dẫn cho RSA Key list.

Các gói tin ban đầu được mã hóa bằng TLS :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ tshark -r capture.pcap 
    1   0.000000 128.237.140.23 → 172.31.22.220 TCP 78 57567 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865167 TSecr=0 SACK_PERM
    2   0.000029 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57567 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332748 TSecr=132865167 WS=128
    3   0.025161 128.237.140.23 → 172.31.22.220 TCP 78 57578 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865192 TSecr=0 SACK_PERM
    4   0.025171 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57578 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332773 TSecr=132865192 WS=128
    5   0.028804 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=1 Ack=1 Win=131904 Len=0 TSval=132865195 TSecr=568332748
    6   0.028881 128.237.140.23 → 172.31.22.220 TLSv1 583 Client Hello
    7   0.028902 172.31.22.220 → 128.237.140.23 TCP 66 443 → 57567 [ACK] Seq=1 Ack=518 Win=28032 Len=0 TSval=568332777 TSecr=132865195
    8   0.029538 172.31.22.220 → 128.237.140.23 TLSv1.2 1073 Server Hello, Certificate, Server Hello Done
    9   0.053871 128.237.140.23 → 172.31.22.220 TCP 66 57578 → 443 [ACK] Seq=1 Ack=1 Win=131904 Len=0 TSval=132865219 TSecr=568332773
   10   0.058387 128.237.140.23 → 172.31.22.220 TLSv1 583 Client Hello
   11   0.058417 172.31.22.220 → 128.237.140.23 TCP 66 443 → 57578 [ACK] Seq=1 Ack=518 Win=28032 Len=0 TSval=568332806 TSecr=132865222
   12   0.058429 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=518 Ack=1008 Win=130880 Len=0 TSval=132865222 TSecr=568332777
   13   0.058743 172.31.22.220 → 128.237.140.23 TLSv1.2 1073 Server Hello, Certificate, Server Hello Done
   14   0.059645 128.237.140.23 → 172.31.22.220 TLSv1.2 384 Client Key Exchange, Change Cipher Spec, Encrypted Handshake Message
   15   0.061383 172.31.22.220 → 128.237.140.23 TLSv1.2 324 New Session Ticket, Change Cipher Spec, Encrypted Handshake Message
   16   0.088416 128.237.140.23 → 172.31.22.220 TCP 66 57578 → 443 [ACK] Seq=518 Ack=1008 Win=130880 Len=0 TSval=132865247 TSecr=568332806
   17   0.092408 128.237.140.23 → 172.31.22.220 TCP 78 57581 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865249 TSecr=0 SACK_PERM
   18   0.092423 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57581 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332840 TSecr=132865249 WS=128
   19   0.092429 128.237.140.23 → 172.31.22.220 TLSv1.2 384 Client Key Exchange, Change Cipher Spec, Encrypted Handshake Message
   20   0.093713 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=836 Ack=1266 Win=130752 Len=0 TSval=132865252 TSecr=568332809
   21   0.094104 172.31.22.220 → 128.237.140.23 TLSv1.2 324 New Session Ticket, Change Cipher Spec, Encrypted Handshake Message
  
(còn nữa …)
```

Sau khi sử dụng key để giải mã giao thức TLS :

→ Để ý có sự xuất hiện của các gói tin giao thức HTTP.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ tshark -r capture.pcap 
    1   0.000000 128.237.140.23 → 172.31.22.220 TCP 78 57567 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865167 TSecr=0 SACK_PERM
    2   0.000029 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57567 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332748 TSecr=132865167 WS=128
    3   0.025161 128.237.140.23 → 172.31.22.220 TCP 78 57578 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865192 TSecr=0 SACK_PERM
    4   0.025171 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57578 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332773 TSecr=132865192 WS=128
    5   0.028804 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=1 Ack=1 Win=131904 Len=0 TSval=132865195 TSecr=568332748
    6   0.028881 128.237.140.23 → 172.31.22.220 TLSv1 583 Client Hello
    7   0.028902 172.31.22.220 → 128.237.140.23 TCP 66 443 → 57567 [ACK] Seq=1 Ack=518 Win=28032 Len=0 TSval=568332777 TSecr=132865195
    8   0.029538 172.31.22.220 → 128.237.140.23 TLSv1.2 1073 Server Hello, Certificate, Server Hello Done
    9   0.053871 128.237.140.23 → 172.31.22.220 TCP 66 57578 → 443 [ACK] Seq=1 Ack=1 Win=131904 Len=0 TSval=132865219 TSecr=568332773
   10   0.058387 128.237.140.23 → 172.31.22.220 TLSv1 583 Client Hello
   11   0.058417 172.31.22.220 → 128.237.140.23 TCP 66 443 → 57578 [ACK] Seq=1 Ack=518 Win=28032 Len=0 TSval=568332806 TSecr=132865222
   12   0.058429 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=518 Ack=1008 Win=130880 Len=0 TSval=132865222 TSecr=568332777
   13   0.058743 172.31.22.220 → 128.237.140.23 TLSv1.2 1073 Server Hello, Certificate, Server Hello Done
   14   0.059645 128.237.140.23 → 172.31.22.220 TLSv1.2 384 Client Key Exchange, Change Cipher Spec, Finished
   15   0.061383 172.31.22.220 → 128.237.140.23 TLSv1.2 324 New Session Ticket, Change Cipher Spec, Finished
   16   0.088416 128.237.140.23 → 172.31.22.220 TCP 66 57578 → 443 [ACK] Seq=518 Ack=1008 Win=130880 Len=0 TSval=132865247 TSecr=568332806
   17   0.092408 128.237.140.23 → 172.31.22.220 TCP 78 57581 → 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1386 WS=64 TSval=132865249 TSecr=0 SACK_PERM
   18   0.092423 172.31.22.220 → 128.237.140.23 TCP 74 443 → 57581 [SYN, ACK] Seq=0 Ack=1 Win=26847 Len=0 MSS=8961 SACK_PERM TSval=568332840 TSecr=132865249 WS=128
   19   0.092429 128.237.140.23 → 172.31.22.220 TLSv1.2 384 Client Key Exchange, Change Cipher Spec, Finished
   20   0.093713 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=836 Ack=1266 Win=130752 Len=0 TSval=132865252 TSecr=568332809
   21   0.094104 172.31.22.220 → 128.237.140.23 TLSv1.2 324 New Session Ticket, Change Cipher Spec, Finished
   22   0.122048 128.237.140.23 → 172.31.22.220 TCP 66 57581 → 443 [ACK] Seq=1 Ack=1 Win=131904 Len=0 TSval=132865276 TSecr=568332840
   23   0.122203 128.237.140.23 → 172.31.22.220 TLSv1 583 Client Hello
   24   0.122220 172.31.22.220 → 128.237.140.23 TCP 66 443 → 57581 [ACK] Seq=1 Ack=518 Win=28032 Len=0 TSval=568332870 TSecr=132865276
   25   0.122552 172.31.22.220 → 128.237.140.23 TLSv1.2 1073 Server Hello, Certificate, Server Hello Done
   26   0.123046 128.237.140.23 → 172.31.22.220 TCP 66 57578 → 443 [ACK] Seq=836 Ack=1266 Win=130752 Len=0 TSval=132865277 TSecr=568332842
   27   0.151669 128.237.140.23 → 172.31.22.220 TCP 66 57581 → 443 [ACK] Seq=518 Ack=1008 Win=130880 Len=0 TSval=132865303 TSecr=568332870
   28   0.152210 128.237.140.23 → 172.31.22.220 TLSv1.2 384 Client Key Exchange, Change Cipher Spec, Finished
   29   0.153206 172.31.22.220 → 128.237.140.23 TLSv1.2 324 New Session Ticket, Change Cipher Spec, Finished
   30   0.183385 128.237.140.23 → 172.31.22.220 TCP 66 57581 → 443 [ACK] Seq=836 Ack=1266 Win=130752 Len=0 TSval=132865334 TSecr=568332901
   31   0.187804 128.237.140.23 → 172.31.22.220 HTTP 506 GET / HTTP/1.1 
   32   0.188303 172.31.22.220 → 128.237.140.23 HTTP 1299 HTTP/1.1 200 OK  (text/html)
   33   0.220287 128.237.140.23 → 172.31.22.220 TCP 66 57581 → 443 [ACK] Seq=1276 Ack=2499 Win=129792 Len=0 TSval=132865368 TSecr=568332936
   34   0.357102 128.237.140.23 → 172.31.22.220 HTTP 521 GET /starter-template.css HTTP/1.1 
   35   0.357544 172.31.22.220 → 128.237.140.23 HTTP 576 HTTP/1.1 200 OK  (text/css)
   36   0.386999 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=1291 Ack=1776 Win=130560 Len=0 TSval=132865528 TSecr=568333105
   37   0.817074 128.237.140.23 → 172.31.22.220 HTTP 438 GET /favicon.ico HTTP/1.1 
   38   0.817393 172.31.22.220 → 128.237.140.23 HTTP 637 HTTP/1.1 404 Not Found  (text/html)
   39   0.847407 128.237.140.23 → 172.31.22.220 TCP 66 57567 → 443 [ACK] Seq=1663 Ack=2347 Win=130496 Len=0 TSval=132865963 TSecr=568333565
```

Thực hiện khai thác các gói tin dạng HTTP ấy. Thực hiện dùng `Follow` > `HTTP Stream.`

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[350 points] WebNet0]
└─$ tshark -r capture.pcap -Y 'http'
   31   0.187804 128.237.140.23 → 172.31.22.220 HTTP 506 GET / HTTP/1.1 
   32   0.188303 172.31.22.220 → 128.237.140.23 HTTP 1299 HTTP/1.1 200 OK  (text/html)
   34   0.357102 128.237.140.23 → 172.31.22.220 HTTP 521 GET /starter-template.css HTTP/1.1 
   35   0.357544 172.31.22.220 → 128.237.140.23 HTTP 576 HTTP/1.1 200 OK  (text/css)
   37   0.817074 128.237.140.23 → 172.31.22.220 HTTP 438 GET /favicon.ico HTTP/1.1 
   38   0.817393 172.31.22.220 → 128.237.140.23 HTTP 637 HTTP/1.1 404 Not Found  (text/html)
```

Nội dung khi thực hiện `Follow` > `HTTP Stream`

```bash
GET / HTTP/1.1
Host: ec2-18-223-184-200.us-east-2.compute.amazonaws.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache

HTTP/1.1 200 OK
Date: Fri, 23 Aug 2019 15:56:36 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Mon, 12 Aug 2019 16:50:05 GMT
ETag: "5ff-58fee50dc3fb0-gzip"
Accept-Ranges: bytes
Vary: Accept-Encoding
Content-Encoding: gzip
Pico-Flag: picoCTF{nongshim.shrimp.crackers}
Content-Length: 821
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html

<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <!-- Custom styles for this template -->
    <link href="starter-template.css" rel="stylesheet">

    <title>Hello, world!</title>
  </head>
  <body>
	<div class="container">
      		<div class="starter-template">
        		<h1>Welcome to A Sample Page</h1>
        		<p class="lead">
				There is legit nothing to see here.<br>
			</p>
      		</div>
	</div><!-- /.container -->

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
  </body>
</html>
```

# Tổng kết :

→ Nội dung của cờ : `picoCTF{nongshim.shrimp.crackers}`

# Tài liệu tham khảo :

[https://blog.didierstevens.com/2020/12/14/decrypting-tls-streams-with-wireshark-part-1/](https://blog.didierstevens.com/2020/12/14/decrypting-tls-streams-with-wireshark-part-1/)