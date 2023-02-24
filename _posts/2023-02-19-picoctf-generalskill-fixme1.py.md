---
title: picoCTF | [100 points] [GeneralSkill] fixme1.py WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] fixme1.py WriteUp

Hoàn thiện báo cáo: Hoàn thành
Quá trình thực hiện: Done

# Tổng quan :

## Tóm tắt nội dung :

Bài viết này giải thích cách khắc phục lỗi cú pháp trong tập tin Python [fixme1.py](http://fixme1.py/) để in ra cờ. Sau khi khắc phục lỗi, chạy tập tin sẽ hiển thị nội dung của cờ: "picoCTF{1nd3nt1ty_cr1515_6a476c8f}". Lỗi cú pháp thường do khoảng trống hoặc tab không đúng cách gây ra.

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description

Fix the syntax error in this Python script to print the flag.[Download Python script](https://artifacts.picoctf.net/c/37/fixme1.py)

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ wget https://artifacts.picoctf.net/c/37/fixme1.py   
--2023-02-05 03:47:11--  https://artifacts.picoctf.net/c/37/fixme1.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.87, 54.192.18.81, 54.192.18.125, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.87|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 837 [application/octet-stream]
Saving to: ‘fixme1.py’

fixme1.py                    100%[=============================================>]     837  --.-KB/s    in 0s      

2023-02-05 03:47:14 (22.8 MB/s) - ‘fixme1.py’ saved [837/837]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ ls -alh 
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb  5 03:47 .
drwxr-xr-x 15 kali kali 4.0K Feb  5 03:47 ..
-rw-r--r--  1 kali kali  837 Jan  4  2022 fixme1.py
```

# Khai thác và thu thập cờ :

Thử đọc nội dung của tập tin `fixme1.py` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ cat fixme1.py                   

import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5a) + chr(0x07) + chr(0x00) + chr(0x46) + chr(0x0b) + chr(0x1a) + chr(0x5a) + chr(0x1d) + chr(0x1d) + chr(0x2a) + chr(0x06) + chr(0x1c) + chr(0x5a) + chr(0x5c) + chr(0x55) + chr(0x40) + chr(0x3a) + chr(0x58) + chr(0x0a) + chr(0x5d) + chr(0x53) + chr(0x43) + chr(0x06) + chr(0x56) + chr(0x0d) + chr(0x14)

  
flag = str_xor(flag_enc, 'enkidu')
  print('That is correct! Here\'s your flag: ' + flag)
```

Khởi chạy tập tin :

→ Tập tin báo lỗi khoảng cách khi khởi chạy.

→ Khắc phục được lỗi có thể sẽ hiển thị nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ python fixme1.py               
  File "/home/kali/Desktop/picoCTF/fixme1.py/fixme1.py", line 20
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```

Chỉ cần xóa các khoảng trống trước hàm print() là có thể khắc phục được lỗi do nguyên nhân chủ yếu mà gây ra lỗi này là do khoảng cách của dòng lệnh quá xa hay việc sử dụng quá nhiều tab hoặc space để tạo khoảng cách.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ cat fixme1.py 

import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5a) + chr(0x07) + chr(0x00) + chr(0x46) + chr(0x0b) + chr(0x1a) + chr(0x5a) + chr(0x1d) + chr(0x1d) + chr(0x2a) + chr(0x06) + chr(0x1c) + chr(0x5a) + chr(0x5c) + chr(0x55) + chr(0x40) + chr(0x3a) + chr(0x58) + chr(0x0a) + chr(0x5d) + chr(0x53) + chr(0x43) + chr(0x06) + chr(0x56) + chr(0x0d) + chr(0x14)

  
flag = str_xor(flag_enc, 'enkidu')

print('That is correct! Here\'s your flag: ' + flag)
```

Khởi chạy lại một lần nữa sau khi khắc phục lỗi.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme1.py]
└─$ python fixme1.py 
That is correct! Here's your flag: picoCTF{1nd3nt1ty_cr1515_6a476c8f}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{1nd3nt1ty_cr1515_6a476c8f}`

# Tài liệu tham khảo :

[https://careerkarma.com/blog/python-indentationerror-unexpected-indent/#:~:text=The cause of the “IndentationError,Expected an indented block](https://careerkarma.com/blog/python-indentationerror-unexpected-indent/#:~:text=The%20cause%20of%20the%20%E2%80%9CIndentationError,Expected%20an%20indented%20block)