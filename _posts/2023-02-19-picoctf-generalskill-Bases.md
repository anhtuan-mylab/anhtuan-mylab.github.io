---
title: picoCTF | [100 points] [GeneralSkill] Bases WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 point] [GeneralSkill] Bases WriteUp



# Tổng quan :

## Tác giả và mô tả :

AUTHOR: SANJAY C/DANNY T

Description : What does this `bDNhcm5fdGgzX3IwcDM1` mean? I think it has something to do with bases.

Hints : Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

# Khai thác và thu thập cờ (flag) :

Tại phần tiêu đề có đề cập đến cụm từ “base” 

→ có thể liên quan đến mã hóa base64 hoặc base32.

→ thử một trong hai mã hóa và giải mã thì thu được nội dung của cờ.

Mình có thể sử dụng base64 -d trong linux để giải mã :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] Based]
└─$ echo "bDNhcm5fdGgzX3IwcDM1" | base64 -d               
l3arn_th3_r0p35
```

Sử dụng python để giải mã base64 :

```bash
┌──(kali㉿kali)-[~]
└─$ python                       
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import base64
>>> print(base64.b64decode('bDNhcm5fdGgzX3IwcDM1'))
b'l3arn_th3_r0p35'
>>> )
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{l3arn_th3_r0p35}`

# Tài liệu tham khảo :

[https://ubunlog.com/vi/base64-codificacion-decodificacion-terminal/](https://ubunlog.com/vi/base64-codificacion-decodificacion-terminal/)

[https://www.hannesholst.com/blog/how-to-identify-a-base64-encoded-string/](https://www.hannesholst.com/blog/how-to-identify-a-base64-encoded-string/)

[https://viblo.asia/p/base64-nhung-dieu-ban-can-biet-3P0lPePp5ox](https://viblo.asia/p/base64-nhung-dieu-ban-can-biet-3P0lPePp5ox)