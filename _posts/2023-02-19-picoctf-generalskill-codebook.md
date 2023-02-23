---
title: picoCTF | [100 points] [GeneralSkill] Codebook WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] Codebook WriteUp



# Tổng quan :

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description :

Run the Python script `code.py`in the same directory as `codebook.txt.`

- [Download code.py](https://artifacts.picoctf.net/c/100/code.py)
- [Download codebook.txt](https://artifacts.picoctf.net/c/100/codebook.txt)

Hitns : 

- On the webshell, use ls to see if both files are in the directory you are in.
- The str_xor function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ wget https://artifacts.picoctf.net/c/100/code.py                                                   
--2023-02-05 03:35:21--  https://artifacts.picoctf.net/c/100/code.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.81, 54.192.18.66, 54.192.18.87, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.81|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1278 (1.2K) [application/octet-stream]
Saving to: ‘code.py’

code.py                      100%[=============================================>]   1.25K  --.-KB/s    in 0s      

2023-02-05 03:35:23 (33.4 MB/s) - ‘code.py’ saved [1278/1278]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ wget https://artifacts.picoctf.net/c/100/codebook.txt
--2023-02-05 03:35:30--  https://artifacts.picoctf.net/c/100/codebook.txt
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.87, 54.192.18.125, 54.192.18.81, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.87|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27 [application/octet-stream]
Saving to: ‘codebook.txt’

codebook.txt                 100%[=============================================>]      27  --.-KB/s    in 0s      

2023-02-05 03:35:35 (43.4 MB/s) - ‘codebook.txt’ saved [27/27]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ ls -alh 
total 16K
drwxr-xr-x  2 kali kali 4.0K Feb  5 03:35 .
drwxr-xr-x 13 kali kali 4.0K Feb  5 03:32 ..
-rw-r--r--  1 kali kali   27 Mar 12  2022 codebook.txt
-rw-r--r--  1 kali kali 1.3K Mar 12  2022 code.py
```

# Khai thác và thu thập cờ (flag) :

## Đọc nội dung của các tập tin :

Nội dung của tập tin `codebook.txt` :

→ Chuỗi kí tự được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ cat codebook.txt 
azbycxdwevfugthsirjqkplomn
```

Đọc nội dung của tập tin `code.py` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ cat code.py     

import random
import sys

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x13) + chr(0x01) + chr(0x17) + chr(0x07) + chr(0x2c) + chr(0x3a) + chr(0x2f) + chr(0x1a) + chr(0x0d) + chr(0x53) + chr(0x0c) + chr(0x47) + chr(0x0a) + chr(0x5f) + chr(0x5e) + chr(0x02) + chr(0x3e) + chr(0x5a) + chr(0x56) + chr(0x5d) + chr(0x45) + chr(0x5d) + chr(0x58) + chr(0x31) + chr(0x0d) + chr(0x58) + chr(0x0f) + chr(0x02) + chr(0x5a) + chr(0x10) + chr(0x0e) + chr(0x5d) + chr(0x13)

def print_flag():
  try:
    codebook = open('codebook.txt', 'r').read()
    
    password = codebook[4] + codebook[14] + codebook[13] + codebook[14] +\
               codebook[23]+ codebook[25] + codebook[16] + codebook[0]  +\
               codebook[25]
               
    flag = str_xor(flag_enc, password)
    print(flag)
  except FileNotFoundError:
    print('Couldn\'t find codebook.txt. Did you download that file into the same directory as this script?')

def main():
  print_flag()

if __name__ == "__main__":
  main()
```

Theo mình hiểu một cách đơn giản thì hàm str_xor có thể làm nhiệm vụ giải mã tập tin code.txt thành chuỗi nội dung cờ và cần 2 yếu tố có thể được khai thác :

- secret : đọc nội dung từ tập tin code.txt
- key : nếu như tập tin codebook.txt là 1 chuỗi thì sẽ lấy kí tự ở những vị trí như mô tả.

Có thể lấy mật khẩu như :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] codebook WriteUp]
└─$ cat getPassword.py 
codebook = open('codebook.txt', 'r').read()

password = codebook[4] + codebook[14] + codebook[13] + codebook[14] + codebook[23]+ codebook[25] + codebook[16] + codebook[0] + codebook[25]

print(password);

┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] codebook WriteUp]
└─$ python getPassword.py         
chthonian
```

Khởi chạy tập tin `code.py` :

→ Có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/codebook]
└─$ python code.py               
picoCTF{c0d3b00k_455157_d9aa2df2}
```

# Tổng kết :

Quá trình thực hiện cũng khá đơn giản do chỉ sử dụng những tính chất cơ bản của chương trình viết bằng ngôn ngữ python.

→ Nội dung của cờ (flag) : `picoCTF{c0d3b00k_455157_d9aa2df2}`

# Tài liệu tham khảo :

[https://docs.python-guide.org/starting/install3/linux/](https://docs.python-guide.org/starting/install3/linux/)

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)