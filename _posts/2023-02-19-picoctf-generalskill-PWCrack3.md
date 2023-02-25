---
title: picoCTF | [100 points] [GeneralSkill] PWCrack3 WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---



# [100 points] [GeneralSkill] PW Crack 3 WriteUp



# Tổng quan :

## Tóm tắt nội dung :

Chương trình chứa nội dung của cờ sẽ yêu cầu nhập mật khẩu để hiển thị. Từ các mật khẩu được cho có thể viết một chương trình đơn giản để thử tự động. Nhập đúng mật khẩu thì có thể thu được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{m45h_fl1ng1ng_6f98a49f}`

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Can you crack the password to get the flag?Download the password checker [here](https://artifacts.picoctf.net/c/25/level3.py) and you'll need the encrypted [flag](https://artifacts.picoctf.net/c/25/level3.flag.txt.enc) and the [hash](https://artifacts.picoctf.net/c/25/level3.hash.bin) in the same directory too.There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.

Hints :

- To view the level3.hash.bin file in the webshell, do: `$ bvi level3.hash.bin`
- To exit `bvi` type `:q` and press enter.
- The `str_xor` function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

 

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ wget https://artifacts.picoctf.net/c/25/level3.py          
--2023-02-05 08:22:05--  https://artifacts.picoctf.net/c/25/level3.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.125, 54.192.18.87, 54.192.18.66, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.125|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1337 (1.3K) [application/octet-stream]
Saving to: ‘level3.py’

level3.py          100%[===============>]   1.31K  --.-KB/s    in 0s      

2023-02-05 08:22:12 (29.4 MB/s) - ‘level3.py’ saved [1337/1337]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ wget https://artifacts.picoctf.net/c/25/level3.flag.txt.enc
--2023-02-05 08:22:21--  https://artifacts.picoctf.net/c/25/level3.flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.9, 13.33.33.37, 13.33.33.97, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.9|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [application/octet-stream]
Saving to: ‘level3.flag.txt.enc’

level3.flag.txt.en 100%[===============>]      31  --.-KB/s    in 0s      

2023-02-05 08:22:30 (41.6 MB/s) - ‘level3.flag.txt.enc’ saved [31/31]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ wget https://artifacts.picoctf.net/c/25/level3.hash.bin    
--2023-02-05 08:22:36--  https://artifacts.picoctf.net/c/25/level3.hash.bin
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.224.250.39, 13.224.250.70, 13.224.250.75, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.224.250.39|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16 [application/octet-stream]
Saving to: ‘level3.hash.bin’

level3.hash.bin    100%[===============>]      16  --.-KB/s    in 0s      

2023-02-05 08:22:43 (26.3 MB/s) - ‘level3.hash.bin’ saved [16/16]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ ls -alh 
total 20K
drwxr-xr-x  2 kali kali 4.0K Feb  5 08:22 .
drwxr-xr-x 21 kali kali 4.0K Feb  5 08:21 ..
-rw-r--r--  1 kali kali   31 Jan  4  2022 level3.flag.txt.enc
-rw-r--r--  1 kali kali   16 Jan  4  2022 level3.hash.bin
-rw-r--r--  1 kali kali 1.4K Jan  4  2022 level3.py
```

# Khai thác và thu thập cờ :

Chạy thử tập tin `level3.py` . Chương trình sẽ yêu cầu nhập mật khẩu từ bàn phím.

→ Nếu nhập đúng có thể hiển thị nội dung của cờ hoặc có thể tìm thấy tại vị trí khác.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python level3.py 
Please enter correct password for flag: 12345
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python level3.py
Please enter correct password for flag: anhtuan
That password is incorrect
```

Đọc nội dung của tập tin `level3.hash.bin`.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ cat level3.hash.bin 
�`E��BC�;���Ϣi�
```

Đọc thử nội dung của tập tin `level3.flag.txt.enc` 

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ cat level3.flag.txt.enc 
A
_P\V:W
```

Đọc thử nội dung của chương trình `level3.py`

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ cat level3.py          
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

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

def level_3_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)
    
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

level_3_pw_check()

# The strings below are 7 possibilities for the correct password. 
#   (Only 1 is correct)
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]
```

Nội dung thì cũng khá đơn giản :

- Khi nhập nội dung từ bàn phím thì nội dung sẽ được chạy qua hàm hash_pw() và nội dung đó sẽ được so sánh với nội dung đã được mã hóa trong tập `level3.hash.bin`
- Kết quả đúng sẽ hiển thị nội dung của cờ.

Mình có thể sử dụng từng gợi ý các mật khẩu đã cho bằng cách nhập bằng tay nhưng mình cũng có thể tạo một chương trình đơn giản để thử từng mật khẩu (một hình thức tấn công mật khẩu dạng vét cạn).

Nội dung thì cũng khá đơn giản, có thể gồm các bước như :

- Lấy các mật khẩu được gợi ý và đưa vào trong một mảng.
- Thực hiện tương tự hàm mã hóa như chương trình level3.py đối với từng đối tượng trong hàm chứa các mật khẩu.
- Các mật khẩu sẽ đem so sánh với nội dung của tập `level3.hash.bin` và sẽ hiển thị kết quả so sánh trùng.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ cat decrpymd5.py       
import hashlib

correct_pw_hash = open('level3.hash.bin', 'rb').read()

pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]

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

Kết quả thu được chuỗi mật khẩu.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python decrpymd5.py
1ea2
```

Thực hiện nhập mật khẩu vào chương trình.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python level3.py 
Please enter correct password for flag: 8799
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python level3.py
Please enter correct password for flag: d3ab    
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 3]
└─$ python level3.py
Please enter correct password for flag: 1ea2
Welcome back... your flag, user:
picoCTF{m45h_fl1ng1ng_6f98a49f}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{m45h_fl1ng1ng_6f98a49f}`

# Tài liệu tham khảo :

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)