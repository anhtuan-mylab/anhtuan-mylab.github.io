---
title: picoCTF | [100 points] [Forensics]  Wireshark twoo twooo two twoo... WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [100 points] [Forensics] Wireshark twoo twooo two twoo... WriteUp


# Tổng quan :

## Tóm tắt nội dung :

- Tập tin *.pcap chứa các gói tin đã bắt được và trong số đó có chứa thông tin để tìm được cờ.
- Có rất nhiều các cờ khác nhau nhưng cờ đúng có dấu “_” ở chuỗi.
- Các subdomain lặp lại có thể tạo thành chuỗi và sử dụng base64 để giải mã và thu được cờ.

Nội dung của cờ : `picoCTF{dns_3xf1l_ftw_deadbeef}`

# Tác giả và mô tả :

AUTHOR: DYLAN

Description : Can you find the flag? [shark2.pcapng](https://mercury.picoctf.net/static/df92c613964fca8edec3b2981f69c3e4/shark2.pcapng).

Hints : 

- Did you really find _the_ flag?
- Look for traffic that seems suspicious.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ wget https://mercury.picoctf.net/static/df92c613964fca8edec3b2981f69c3e4/shark2.pcapng     
--2023-03-03 07:07:14--  https://mercury.picoctf.net/static/df92c613964fca8edec3b2981f69c3e4/shark2.pcapng
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3520112 (3.4M) [application/octet-stream]
Saving to: ‘shark2.pcapng’

shark2.pcapng      100%[===============>]   3.36M   635KB/s    in 5.4s    

2023-03-03 07:07:21 (635 KB/s) - ‘shark2.pcapng’ saved [3520112/3520112]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ ls -alh 
total 3.4M
drwxr-xr-x  2 kali kali 4.0K Mar  3 07:07 .
drwxr-xr-x 18 kali kali 4.0K Mar  3 07:07 ..
-rw-r--r--  1 kali kali 3.4M Mar 15  2021 shark2.pcapng
```

# Khai thác và thu thập cờ (flag) :

Sử dụng file để xác định kiểu tập tin 

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ file shark2.pcapng 
shark2.pcapng: pcapng capture file - version 1.0
```

Đối với tập tin dạng .pcap chứa các gói tin bắt được, mình có thể dùng chức năng filter là `tcp.stream eq 6` thì có tìm được một dạng cờ.

```bash
GET /flag HTTP/1.1
Host: 18.217.1.57
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.105 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9

HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 73
Server: Werkzeug/1.0.1 Python/3.6.9
Date: Mon, 10 Aug 2020 01:39:19 GMT

picoCTF{bfe48e8500c454d647c55a4471985e776a07b26cba64526713f43758599aa98b}
```

Mình tiếp tục tăng giá trị thì cũng có thể tìm được những nội dung tương tự (dùng filter giới hạn giao thức HTTP) nhưng rất may mắn là những cờ này không đúng.

```bash
4586	26.230404	192.168.38.104	18.217.1.57	HTTP	503	GET /flag HTTP/1.1
4591	26.283249	18.217.1.57	192.168.38.104	HTTP	263	HTTP/1.0 200 OK  (text/html)
4596	26.397821	192.168.38.104	18.217.1.57	HTTP	503	GET /flag HTTP/1.1
4601	26.449432	18.217.1.57	192.168.38.104	HTTP	263	HTTP/1.0 200 OK  (text/html)
4606	26.577211	192.168.38.104	18.217.1.57	HTTP	503	GET /flag HTTP/1.1
4611	26.628858	18.217.1.57	192.168.38.104	HTTP	263	HTTP/1.0 200 OK  (text/html)
...
```

Tiếp tục tìm ở những vị trí khác, mình thấy giao thức tiếp theo là DNS có những đặc điểm như :

- Source và Destination chỉ có : 8.8.8.8 , 192.168.38.104 và 18.217.1.57.
- Có sự lặp lại ở những subdomain trên tên miền reddshrimpandherring.com

Thử truy cập và dùng curl thì cũng không có kết quả tại thời điểm hiện tại.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ curl reddshrimpandherring.com           
<html>
        <head>
                <script>
                        var forwardingUrl = "/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=js";
                        var destinationUrl = "/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=meta";
                        var addDetection = true;
                        if (addDetection) {
                                var inIframe = window.self !== window.top;
                                forwardingUrl += "&inIframe=" + inIframe;
                                var inPopUp = (window.opener !== undefined && window.opener !== null && window.opener !== window);
                                forwardingUrl += "&inPopUp=" + inPopUp;
                        }
                        window.location.replace(forwardingUrl);
                </script>
                <noscript>
                        <meta http-equiv="refresh" content="1;url=/page/bouncy.php?&bpae=GbhWt6smolx797uvwVkZc7MLLELsgGlydgi1h%2FQVMxNsvuLZ%2F%2FwP3hewMUPpT75C9RVw9LSQeQuvI5a55rUa62YzxCqRCrpq2cEJBEfevFkg4%2BNrLWf1DWMJJ0pDphYiw86DiHPQbxb7SXoCXRhejpCXLSmcoT9xJsBlN2rQuClby5mpN4BbWtKImZlbC%2BRdOhk2%2Bmkn0Ms4SqKzu2LfJe1gQfb%2Fuvw%2BsFvtgOPwmGsPPwNGtvOQTvcq1qsqQp%2BRtC52BSmE%2BQGPM%2F%2FKQXBSArvlOEzH4NT1AQvmThZugQ3Y1uw9Ax3yJkx0tq1hrsDy6%2FlgupEsjMpLnsGd8mjewGytrQWZ7YonYBtZuE8M2EtwDFymKk1sAMD17mUJHUOWfcGxy5Tfqb560YraZuVqiSnTwNZT&redirectType=meta" />
                </noscript>
        </head>
</html>
```

Nhưng sự lặp lại ở những subdomain thì có thể khai thác được nội dung của cờ. Sử dụng filter là 

`dns && ip.dst==18.217.1.57` thì có thể thu được như bên dưới (cũng có thể thử với ip.dst==192.168.38.104 nhưng kết quả thì nhiều hơn nên cũng không có khả năng cao).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ tshark -nr shark2.pcapng -Y 'dns && ip.src==18.217.1.57' 
 1634   9.388061  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0xdf26 No such name A cGljb0NU.reddshrimpandherring.com SOA a.gtld-servers.net
 1636   9.439381  18.217.1.57 → 192.168.38.104 DNS 205 Standard query response 0xa12d No such name A cGljb0NU.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA dns-external-master.amazon.com
 1638   9.490669  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x1dd2 No such name A cGljb0NU.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 2043  11.921106  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x3a30 No such name A RntkbnNf.reddshrimpandherring.com SOA a.gtld-servers.net
 2045  11.971614  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0xec57 No such name A RntkbnNf.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 2047  12.021881  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0xabb9 No such name A RntkbnNf.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 2445  14.553435  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x531d No such name A M3hmMWxf.reddshrimpandherring.com SOA a.gtld-servers.net
 2447  14.604708  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0x3bd6 No such name A M3hmMWxf.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 2671  14.657185  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x9e21 No such name A M3hmMWxf.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3143  16.455193  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x99dd No such name A ZnR3X2Rl.reddshrimpandherring.com SOA a.gtld-servers.net
 3152  16.505490  18.217.1.57 → 192.168.38.104 DNS 203 Standard query response 0x028b No such name A ZnR3X2Rl.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 3155  16.557035  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x2ee1 No such name A ZnR3X2Rl.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3433  18.288746  18.217.1.57 → 192.168.38.104 DNS 166 Standard query response 0x16f6 No such name A YWRiZWVm.reddshrimpandherring.com SOA a.gtld-servers.net
 3441  18.339039  18.217.1.57 → 192.168.38.104 DNS 205 Standard query response 0xe7cb No such name A YWRiZWVm.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA dns-external-master.amazon.com
 3445  18.391844  18.217.1.57 → 192.168.38.104 DNS 184 Standard query response 0x2a4b No such name A YWRiZWVm.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 3972  20.316608  18.217.1.57 → 192.168.38.104 DNS 162 Standard query response 0xbe68 No such name A fQ==.reddshrimpandherring.com SOA a.gtld-servers.net
 3981  20.368147  18.217.1.57 → 192.168.38.104 DNS 199 Standard query response 0xbaee No such name A fQ==.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 3984  20.420054  18.217.1.57 → 192.168.38.104 DNS 180 Standard query response 0x4068 No such name A fQ==.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
 4364  22.532034  18.217.1.57 → 192.168.38.104 DNS 162 Standard query response 0xa740 No such name A fQ==.reddshrimpandherring.com SOA a.gtld-servers.net
 4373  22.582748  18.217.1.57 → 192.168.38.104 DNS 199 Standard query response 0x683a No such name A fQ==.reddshrimpandherring.com.us-west-1.ec2-utilities.amazonaws.com SOA pdns1.ultradns.net
 4376  22.633047  18.217.1.57 → 192.168.38.104 DNS 180 Standard query response 0x7418 No such name A fQ==.reddshrimpandherring.com.windomain.local SOA a.root-servers.net
```

Thử mỗi một subdomain một lần thì có thể thu được mội chuỗi như bên dưới. 

cGljb0NU (subdomain đầu tiên), RntkbnNf (subdomain tiếp theo) và tương tự cho các chuỗi tiếp theo.

```bash
cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==
```

Mình thấy có kết thúc là == thì ít nhiều có liên quan đến base64 nên thử sử dụng để giải mã thì thu được nội dung của cờ. Thực hiện nhập vào thì kết quả chấp nhận.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Wireshark twoo twooo two twoo...]
└─$ echo "cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==" | base64 -d
picoCTF{dns_3xf1l_ftw_deadbeef}
```

# Tổng kết :

→ Nội dung của cờ : `picoCTF{dns_3xf1l_ftw_deadbeef}`

# Tài liệu tham khảo :

[https://www.wireshark.org/docs/man-pages/tshark.html](https://www.wireshark.org/docs/man-pages/tshark.html)

[https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Wireshark_twoo_twooo_two_twoo.md](https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Wireshark_twoo_twooo_two_twoo.md)

[https://picoctf2021.haydenhousen.com/forensics/wireshark-twoo-twooo-two-twoo](https://picoctf2021.haydenhousen.com/forensics/wireshark-twoo-twooo-two-twoo)...