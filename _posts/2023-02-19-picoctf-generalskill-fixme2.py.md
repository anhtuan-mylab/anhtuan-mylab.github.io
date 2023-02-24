---
title: picoCTF | [100 points] [GeneralSkill] fixme2.py WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] fixme2.py WriteUp



# Tổng quan :

## Tóm tắt nội dung :

Đây là bài viết hướng dẫn giải bài `fixme2.py` của PicoCTF. Bài viết cung cấp thông tin về cách sửa lỗi cú pháp trong chương trình Python và thu được nội dung của cờ. Nội dung của cờ là `picoCTF{3qu4l1ty_n0t_4551gnm3nt_4863e11b}`.

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description: Fix the syntax error in the Python script to print the flag.[Download Python script](https://artifacts.picoctf.net/c/66/fixme2.py)

Hints :

- Are equality and assignment the same symbol?
- To view the file in the webshell, do: `$ nano fixme2.py`

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ wget https://artifacts.picoctf.net/c/66/fixme2.py
--2023-02-05 03:55:18--  https://artifacts.picoctf.net/c/66/fixme2.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.97, 13.33.33.69, 13.33.33.37, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1029 (1.0K) [application/octet-stream]
Saving to: ‘fixme2.py’

fixme2.py                    100%[=============================================>]   1.00K  --.-KB/s    in 0s      

2023-02-05 03:55:22 (10.9 MB/s) - ‘fixme2.py’ saved [1029/1029]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ ls -alh
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb  5 03:55 .
drwxr-xr-x 16 kali kali 4.0K Feb  5 03:55 ..
-rw-r--r--  1 kali kali 1.1K Jan  4  2022 fixme2.py
```

# Khai thác và thu thập cờ :

Thử đọc nội dung tập tin `fixme2.py` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ cat fixme2.py 

import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x58) + chr(0x18) + chr(0x11) + chr(0x41) + chr(0x09) + chr(0x5f) + chr(0x1f) + chr(0x10) + chr(0x3b) + chr(0x1b) + chr(0x55) + chr(0x1a) + chr(0x34) + chr(0x5d) + chr(0x51) + chr(0x40) + chr(0x54) + chr(0x09) + chr(0x05) + chr(0x04) + chr(0x57) + chr(0x1b) + chr(0x11) + chr(0x31) + chr(0x5f) + chr(0x51) + chr(0x52) + chr(0x46) + chr(0x00) + chr(0x5f) + chr(0x5a) + chr(0x0b) + chr(0x19)

  
flag = str_xor(flag_enc, 'enkidu')

# Check that flag is not empty
if flag = "":
  print('String XOR encountered a problem, quitting.')
else:
  print('That is correct! Here\'s your flag: ' + flag)
```

Khởi chạy chương trình được viết bằng ngôn ngữ Python :

→ Chương trình báo lỗi khi biên dịch và khởi chạy.

→ Khắc phục được lỗi có thể sẽ hiển thị nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ python fixme2.py           
  File "/home/kali/Desktop/picoCTF/fixme2.py/fixme2.py", line 22
    if flag = "":
       ^^^^^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

Lỗi này xảy ra do sử dụng cú pháp sai trong ngôn ngữ Python. Cụ thể là trong quá trình tạo nội dung của cờ và kết quả sẽ gán vào biến flag. Hàm if sẽ kiểm tra nội dung của biến flag có bị rỗng hay không. Nếu rỗng thì báo quá trình tạo cờ sai, còn không thì in ra màn hình. Vấn đề là khi so sánh thì bằng thì phải sử dụng `==` thay vì `=` nhưng trong chương trình.

→ Thay thể `==` bằng `=`.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ cat fixme2.py 

import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x58) + chr(0x18) + chr(0x11) + chr(0x41) + chr(0x09) + chr(0x5f) + chr(0x1f) + chr(0x10) + chr(0x3b) + chr(0x1b) + chr(0x55) + chr(0x1a) + chr(0x34) + chr(0x5d) + chr(0x51) + chr(0x40) + chr(0x54) + chr(0x09) + chr(0x05) + chr(0x04) + chr(0x57) + chr(0x1b) + chr(0x11) + chr(0x31) + chr(0x5f) + chr(0x51) + chr(0x52) + chr(0x46) + chr(0x00) + chr(0x5f) + chr(0x5a) + chr(0x0b) + chr(0x19)

  
flag = str_xor(flag_enc, 'enkidu')

# Check that flag is not empty
if flag == "":
  print('String XOR encountered a problem, quitting.')
else:
  print('That is correct! Here\'s your flag: ' + flag)
```

Khởi chạy chương trình sau khi sửa lỗi.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ python fixme2.py 
That is correct! Here's your flag: picoCTF{3qu4l1ty_n0t_4551gnm3nt_4863e11b}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{3qu4l1ty_n0t_4551gnm3nt_4863e11b}`

# Tài liệu tham khảo :

[https://www.digitalocean.com/community/tutorials/python-string-comparison](https://www.digitalocean.com/community/tutorials/python-string-comparison)