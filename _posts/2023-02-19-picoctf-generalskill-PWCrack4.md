---
title: picoCTF | [100 points] [GeneralSkill] PWCrack4 WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] PW Crack 4 WriteUp


# Tổng quan :

## Tóm tắt nội dung :

Bài viết này trình bày cách giải bài tập PW Crack 4, yêu cầu tìm mật khẩu đúng để giải mã cờ. Bài viết cung cấp các tệp cần thiết và đưa ra gợi ý để giải quyết bài tập. Sau khi phân tích các tệp thì có thể viết một chương trình đơn giản để tìm mật khẩu. Nhập mật khẩu đúng sẽ thu được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{fl45h_5pr1ng1ng_ae0fb77c}`

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Can you crack the password to get the flag?Download the password checker [here](https://artifacts.picoctf.net/c/58/level4.py) and you'll need the encrypted [flag](https://artifacts.picoctf.net/c/58/level4.flag.txt.enc) and the [hash](https://artifacts.picoctf.net/c/58/level4.hash.bin) in the same directory too.There are 100 potential passwords with only 1 being correct. You can find these by examining the password checker script.

Hints :

- A for loop can help you do many things very quickly.
- The `str_xor` function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ wget https://artifacts.picoctf.net/c/58/level4.py      
--2023-02-05 08:43:33--  https://artifacts.picoctf.net/c/58/level4.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.97, 13.33.33.69, 13.33.33.37, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2085 (2.0K) [application/octet-stream]
Saving to: ‘level4.py’

level4.py          100%[===============>]   2.04K  --.-KB/s    in 0s      

2023-02-05 08:43:34 (55.8 MB/s) - ‘level4.py’ saved [2085/2085]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ wget https://artifacts.picoctf.net/c/58/level4.flag.txt.enc
--2023-02-05 08:43:44--  https://artifacts.picoctf.net/c/58/level4.flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.69, 13.33.33.97, 13.33.33.37, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.69|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 33 [application/octet-stream]
Saving to: ‘level4.flag.txt.enc’

level4.flag.txt.en 100%[===============>]      33  --.-KB/s    in 0s      

2023-02-05 08:43:51 (24.9 MB/s) - ‘level4.flag.txt.enc’ saved [33/33]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ wget https://artifacts.picoctf.net/c/58/level4.hash.bin    
--2023-02-05 08:43:56--  https://artifacts.picoctf.net/c/58/level4.hash.bin
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.81, 54.192.18.125, 54.192.18.87, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.81|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16 [application/octet-stream]
Saving to: ‘level4.hash.bin’

level4.hash.bin    100%[===============>]      16  --.-KB/s    in 0s      

2023-02-05 08:44:00 (32.4 MB/s) - ‘level4.hash.bin’ saved [16/16]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ ls -alh 
total 20K
drwxr-xr-x  2 kali kali 4.0K Feb  5 08:44 .
drwxr-xr-x 22 kali kali 4.0K Feb  5 08:42 ..
-rw-r--r--  1 kali kali   33 Jan  4  2022 level4.flag.txt.enc
-rw-r--r--  1 kali kali   16 Jan  4  2022 level4.hash.bin
-rw-r--r--  1 kali kali 2.1K Jan  4  2022 level4.py
```

# Khai thác và thu thập thông tin :

Chạy thử chương trình. Chương trình tương tự như 2 bài trước (PW Crack 1, 2 và 3), cũng yêu cầu nhập mật khẩu từ bàn phím.

→ Có thể thu được nội dung của cờ nếu nhập đúng mật khẩu hoặc có thể tìm được tại vị trí khác. 

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ python level4.py 
Please enter correct password for flag: 12345
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ python level4.py
Please enter correct password for flag: anhtuan
That password is incorrects
```

Đọc thử nội dung của `level4.flag.txt.enc`

→ Chứa nội dung của cờ đã được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ cat level4.flag.txt.enc 
G_
  tb QZRQ_iSX;VSVUQJ
```

Đọc nội dung của tập tin `level4.hash.bin`

→ Chứa nội dung của mật khẩu đã được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ cat level4.hash.bin    
hq/u#VO�S�=JM�\
```

Đọc nội dung của chương trình bằng lệnh cat trên Linux

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ cat level4.py          
import hashlib

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

flag_enc = open('level4.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level4.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

def level_4_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)
    
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

level_4_pw_check()

# The strings below are 100 possibilities for the correct password. 
#   (Only 1 is correct)
pos_pw_list = ["6288", "6152", "4c7a", "b722", "9a6e", "6717", "4389", "1a28", "37ac", "de4f", "eb28", "351b", "3d58", "948b", "231b", "973a", "a087", "384a", "6d3c", "9065", "725c", "fd60", "4d4f", "6a60", "7213", "93e6", "8c54", "537d", "a1da", "c718", "9de8", "ebe3", "f1c5", "a0bf", "ccab", "4938", "8f97", "3327", "8029", "41f2", "a04f", "c7f9", "b453", "90a5", "25dc", "26b0", "cb42", "de89", "2451", "1dd3", "7f2c", "8919", "f3a9", "b88f", "eaa8", "776a", "6236", "98f5", "492b", "507d", "18e8", "cfb5", "76fd", "6017", "30de", "bbae", "354e", "4013", "3153", "e9cc", "cba9", "25ea", "c06c", "a166", "faf1", "2264", "2179", "cf30", "4b47", "3446", "b213", "88a3", "6253", "db88", "c38c", "a48c", "3e4f", "7208", "9dcb", "fc77", "e2cf", "8552", "f6f8", "7079", "42ef", "391e", "8a6d", "2154", "d964", "49ec"]
```

Mình cũng thực hiện tương tự như bài trước, cũng viết một chương trình để thực hiện so sánh từng mật khẩu trong số các mật khẩu đã cho sau khi sử dụng hàm mã hóa với nội dung trong tập tin `level4.hash.bin` . Nếu như trùng khớp sẽ xuất nội dung liên quan.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ cat decrpymd5.py   
import hashlib

correct_pw_hash = open('level4.hash.bin', 'rb').read()

pos_pw_list = ["6288", "6152", "4c7a", "b722", "9a6e", "6717", "4389", "1a28", "37ac", "de4f", "eb28", "351b", "3d58", "948b", "231b", "973a", "a087", "384a", "6d3c", "9065", "725c", "fd60", "4d4f", "6a60", "7213", "93e6", "8c54", "537d", "a1da", "c718", "9de8", "ebe3", "f1c5", "a0bf", "ccab", "4938", "8f97", "3327", "8029", "41f2", "a04f", "c7f9", "b453", "90a5", "25dc", "26b0", "cb42", "de89", "2451", "1dd3", "7f2c", "8919", "f3a9", "b88f", "eaa8", "776a", "6236", "98f5", "492b", "507d", "18e8", "cfb5", "76fd", "6017", "30de", "bbae", "354e", "4013", "3153", "e9cc", "cba9", "25ea", "c06c", "a166", "faf1", "2264", "2179", "cf30", "4b47", "3446", "b213", "88a3", "6253", "db88", "c38c", "a48c", "3e4f", "7208", "9dcb", "fc77", "e2cf", "8552", "f6f8", "7079", "42ef", "391e", "8a6d", "2154", "d964", "49ec"]

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

for x in pos_pw_list:
        if (correct_pw_hash == hash_pw(x)):
                print(x)
```

Sử dụng chương trình thì thu được nội dung của mật khẩu.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ python decrpymd5.py
76fd
```

Thử với mật khẩu vừa tìm được trên chương trình `level4.py`

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 4]
└─$ python level4.py   
Please enter correct password for flag: 76fd
Welcome back... your flag, user:
picoCTF{fl45h_5pr1ng1ng_ae0fb77c}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{fl45h_5pr1ng1ng_ae0fb77c}`

# Tài liệu tham khảo :

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)