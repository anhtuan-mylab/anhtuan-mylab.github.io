---
title: picoCTF | [100 points] [GeneralSkill] PWCrack1 WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---



# [100 points] [GeneralSkill] PW Crack 1 WriteUp



# Tổng quan :

## Tóm tắt nội dung :

Bài viết này là hướng dẫn giải bài PW Crack 1, yêu cầu giải mã tập tin `level1.flag.txt.enc` bằng hàm `str_xor` với mật khẩu là `8713`. 

Kết quả nội dung của cờ là `picoCTF{545h_r1ng1ng_1b2fd683}`.

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Can you crack the password to get the flag?Download the password checker [here](https://artifacts.picoctf.net/c/53/level1.py) and you'll need the encrypted [flag](https://artifacts.picoctf.net/c/53/level1.flag.txt.enc) in the same directory too.

Hints :

- To view the file in the webshell, do: `$ nano level1.py`
- To exit `nano`, press Ctrl and x and follow the on-screen prompts.
- The `str_xor` function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ wget https://artifacts.picoctf.net/c/53/level1.py
--2023-02-05 07:43:53--  https://artifacts.picoctf.net/c/53/level1.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.97, 13.33.33.69, 13.33.33.37, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 876 [application/octet-stream]
Saving to: ‘level1.py’

level1.py          100%[===============>]     876  --.-KB/s    in 0s      

2023-02-05 07:43:55 (2.04 MB/s) - ‘level1.py’ saved [876/876]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ wget https://artifacts.picoctf.net/c/53/level1.flag.txt.enc
--2023-02-05 07:44:03--  https://artifacts.picoctf.net/c/53/level1.flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.97, 13.33.33.37, 13.33.33.9, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30 [application/octet-stream]
Saving to: ‘level1.flag.txt.enc’

level1.flag.txt.en 100%[===============>]      30  --.-KB/s    in 0s      

2023-02-05 07:44:07 (56.1 MB/s) - ‘level1.flag.txt.enc’ saved [30/30]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ ls -alh
total 16K
drwxr-xr-x  2 kali kali 4.0K Feb  5 07:44 .
drwxr-xr-x 19 kali kali 4.0K Feb  5 07:43 ..
-rw-r--r--  1 kali kali   30 Jan  4  2022 level1.flag.txt.enc
-rw-r--r--  1 kali kali  876 Jan  4  2022 level1.py
```

# Khai thác và thu thập cờ :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ python level1.py       
Please enter correct password for flag: 12345
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ python level1.py
Please enter correct password for flag: anhtuan
That password is incorrect
```

Đọc thử nội dung của chương trình bằng lệnh `cat` trên Linux.

Nội dung của tập tin `level1.flag.txt.enc` :

→ Chứa nội dung của cờ đã được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ cat level1.flag.txt.enc 
[gE]__TgS^S

           J
```

Khởi chạy thử chương trình `level1.py`. Chương trinh sẽ yêu cầu nhập mật khẩu từ bàn phím.

→ Nếu nhập đúng có thể hiển thị nội dung cờ hoặc có thể tìm thấy ở vị trí khác.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ cat level1.py       
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

flag_enc = open('level1.flag.txt.enc', 'rb').read()

def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "8713"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

level_1_pw_check()
```

Để ý một chút thì chỗ dòng kiểm tra dữ liệu nhập vào chỉ có hàm if so sánh giá trị nhập vào phải bằng chuỗi “8713” thì sẽ dùng hàm str_xor để giải mã tập tin `level1.flag.txt.enc` và thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 1]
└─$ python level1.py 
Please enter correct password for flag: 8713
Welcome back... your flag, user:
picoCTF{545h_r1ng1ng_1b2fd683}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{545h_r1ng1ng_1b2fd683}`

# Tài liệu tham khảo :

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)