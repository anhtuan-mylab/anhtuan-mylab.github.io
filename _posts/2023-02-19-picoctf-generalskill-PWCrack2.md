---
title: picoCTF | [100 points] [GeneralSkill] PWCrack2 WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] PW Crack 2 WriteUp


# Tổng quan :

## Tóm tắt nội dung :

Bài viết này là một WriteUp về bài tập "PW Crack 2" của PicoCTF. Bài tập yêu cầu tìm mật khẩu để giải mã cờ đã được mã hóa. Tìm chuỗi so sánh trong nội dung chương trình để tìm mật khẩu.

Nội dung của cờ (flag) : `picoCTF{tr45h_51ng1ng_489dea9a}`

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Can you crack the password to get the flag?Download the password checker [here](https://artifacts.picoctf.net/c/16/level2.py) and you'll need the encrypted [flag](https://artifacts.picoctf.net/c/16/level2.flag.txt.enc) in the same directory too.

Hints :

- Does that encoding look familiar ?
- The `str_xor` function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ wget https://artifacts.picoctf.net/c/16/level2.py          
--2023-02-05 08:16:28--  https://artifacts.picoctf.net/c/16/level2.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.66, 54.192.18.87, 54.192.18.81, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.66|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 914 [application/octet-stream]
Saving to: ‘level2.py’

level2.py          100%[===============>]     914  --.-KB/s    in 0s      

2023-02-05 08:16:37 (21.6 MB/s) - ‘level2.py’ saved [914/914]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ wget https://artifacts.picoctf.net/c/16/level2.flag.txt.enc
--2023-02-05 08:16:45--  https://artifacts.picoctf.net/c/16/level2.flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.224.250.70, 13.224.250.39, 13.224.250.75, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.224.250.70|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [application/octet-stream]
Saving to: ‘level2.flag.txt.enc’

level2.flag.txt.en 100%[===============>]      31  --.-KB/s    in 0s      

2023-02-05 08:16:53 (28.1 MB/s) - ‘level2.flag.txt.enc’ saved [31/31]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ ls -alh
total 16K
drwxr-xr-x  2 kali kali 4.0K Feb  5 08:16 .
drwxr-xr-x 20 kali kali 4.0K Feb  5 08:16 ..
-rw-r--r--  1 kali kali   31 Jan  4  2022 level2.flag.txt.enc
-rw-r--r--  1 kali kali  914 Jan  4  2022 level2.py
```

# Khai thác và thu thập cờ :

Chạy thử chương trình [level2.py](http://level2.py) được viết bằng Python. Chương trình sẽ yêu cầu nhập mật khẩu từ bàn phím.

→ Có thể sẽ thu được nội dung của cờ hoặc có thể tìm thấy ở vị trí khác.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ python level2.py 
Please enter correct password for flag: 12345
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ python level2.py
Please enter correct password for flag: anhtuan
That password is incorrect
```

Đọc thử nội dung của tập tin `level2.flag.txt.enc` 

→ Nội dung của cờ đã được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ cat level2.flag.txt.enc 

TY'1qM
      :
X:]RW]J
```

Đọc thử nội dung của chương trình `level2.py`

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ cat level2.py          
### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################

flag_enc = open('level2.flag.txt.enc', 'rb').read()

def level_2_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == chr(0x64) + chr(0x65) + chr(0x37) + chr(0x36) ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

level_2_pw_check()
```

nội dung cũng khác tương tự như bài “PW Crack 1” nhưng thay vì chỉ một chuỗi đơn giản thì chuỗi hiển thị dưới dạng Hexa và phải ép kiểu về char để hiển thị nội dung. Mình có thể sử dụng Python để in ra màn hình nội dung của chuỗi so sánh ấy.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ python          
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print(chr(0x64) + chr(0x65) + chr(0x37) + chr(0x36))
de76
>>>
```

Thử nhập mật khẩu vừa tìm được vào chương trình.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 2]
└─$ python level2.py
Please enter correct password for flag: de76
Welcome back... your flag, user:
picoCTF{tr45h_51ng1ng_489dea9a}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{tr45h_51ng1ng_489dea9a}`

# Tài liệu tham khảo :

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)