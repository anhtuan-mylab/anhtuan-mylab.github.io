---
title: picoCTF | [300 points] [Forensics]  shark on wire 2 WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [300 points] [Forensics] Shark On Wire 2 WriteUp


# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải về chứa các gói tin bắt được có nội dung của cờ.
- Quan sát các điểm chung giữa các gói tin UDP có chung port là 22.
- Chuyển các nội dung tìm được thành ASCII để thu được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{p1LLf3r3d_data_v1a_st3g0}`

## Tác giả và mô tả :

AUTHOR: DANNY

Description : We found this [packet capture](https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap). Recover the flag that was pilfered from the network.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] shark on wire 2]
└─$ wget https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap
--2023-03-03 22:07:55--  https://jupiter.challenges.picoctf.org/static/b506393b6f9d53b94011df000c534759/capture.pcap
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 112318 (110K) [application/octet-stream]
Saving to: ‘capture.pcap’

capture.pcap       100%[===============>] 109.69K   202KB/s    in 0.5s    

2023-03-03 22:07:57 (202 KB/s) - ‘capture.pcap’ saved [112318/112318]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] shark on wire 2]
└─$ ls -alh 
total 120K
drwxr-xr-x  2 kali kali 4.0K Mar  3 22:07 .
drwxr-xr-x 20 kali kali 4.0K Mar  3 22:07 ..
-rw-r--r--  1 kali kali 110K Oct 26  2020 capture.pcap
```

# Khai thác và thu thập cờ :

Thử dùng Follow → UDP Stream : đối với dạng tập tin tải về chứa các gói tin thì điều đầu tiên mình thường làm là thử với filter `udp.stream eq <số tăng dần>` để xem có chuỗi nội dung cờ nào không. Thử vài lần thì cũng có kết quả nhưng không đúng.

```bash
kfdsalkfsalkico{N0t_a_fLag} (udp.stream eq 6)
icoCTF{StaT31355e (udp.stream eq 7)
...
```

Nhưng khi thử đến lần 32 (cụ thể filter là `udp.stream eq 32`) thì thu được chuỗi có cụm từ `start` và nếu như có thể tiếp tục dò thì đến 60 (cụ thể là `udp.stream eq 60`) thì kết thúc bằng chữ `end`. Mình có thể đoán là nội dung cờ nằm trong khoảng vị trí này. Nhận xét một chút thì dãy trên có những đặc điểm như :

- Đều có chung một port đến (destination port là 22).
- Độ dài Frame luôn bằng 60 và độ dài (data) luôn bằng 5 (trừ các cuối cùng).

Như vậy, mình theo quan niệm là cái gì giống nhau thì có thể loại bỏ thì có thể thu được các số như 112, 105, 099, 111, … (5112 → 112, 5105 → 105, 5099 → 99, 5111 → 111) và nếu như nhìn bảng kí tự ASCII quen thì các số trên có thể tạo thành cụm từ `pico`

→ Có thể tìm được nội dung của cờ.

Sau khi tham khảo trên mạng thì mình có thể viết một chương trình đơn giản để thực hiện việc lọc nội dung và lấy được cờ (lấy ý tưởng từ [Dvd848](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/shark_on_wire_2.md)) . Chương trình thực hiện các công việc như :

- Sử dụng thư viện Scapy để thao tác trên các tập tin bắt gói tin.
- Lọc các gói tin là UDP và với Destination Port = 22.
- Lấy giá trị là Source Destination Port (sport) và lưu thành chuỗi flag.
- Việc trừ cho 5000 là để thu được số có thể hiển thị theo ASCII (5000 - 5112 = 112 - chữ p).
- `chr(packet[UDP].sport - 5000)` : để hiển thị thành chuỗi kí tự luôn thay vì chỉ có số thôi.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] shark on wire 2]
└─$ cat getFlagCapture.py 
from scapy.all import *

flag = ""

packets = rdpcap('capture.pcap')
for packet in packets:
    if UDP in packet and packet[UDP].dport == 22:
        #print(packet[UDP].sport)
        flag = flag + chr(packet[UDP].sport - 5000)
    
print(flag)
```

Thực thi chương trình thì có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[300 points] shark on wire 2]
└─$ python getFlagCapture.py 
picoCTF{p1LLf3r3d_data_v1a_st3g0}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{p1LLf3r3d_data_v1a_st3g0}`

# Tài liệu tham khảo :

[https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/shark_on_wire_2.md](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/shark_on_wire_2.md)

[https://www.programcreek.com/python/example/103591/scapy.all.rdpcap](https://www.programcreek.com/python/example/103591/scapy.all.rdpcap)