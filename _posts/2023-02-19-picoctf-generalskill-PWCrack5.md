---
title: picoCTF | [100 points] [GeneralSkill] PWCrack5 WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] PW Crack 5 WriteUp

Hoàn thiện báo cáo: Hoàn thành.
Quá trình thực hiện: Done

# Tổng quan :

## Tóm tắt nội dung :

Tương tự như các bài trước về đề tài PW Crack, chương trình cũng sẽ yêu cầu nhập mật khẩu để hiển thị nội dung của cờ. Vấn đề đặt ra là cac mật khẩu gợi ý chứa trong một tập tin với số lượng rất lớn, có thể phải sử dụng một chương trình để so sánh kết quả để tìm mật khẩu đúng.

Nội dung của cờ (flag) là : `picoCTF{****_********_fffcda23}`

## Tổng quan về tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description

Can you crack the password to get the flag?Download the password checker [here](https://artifacts.picoctf.net/c/81/level5.py) and you'll need the encrypted [flag](https://artifacts.picoctf.net/c/81/level5.flag.txt.enc) and the [hash](https://artifacts.picoctf.net/c/81/level5.hash.bin) in the same directory too. Here's a [dictionary](https://artifacts.picoctf.net/c/81/dictionary.txt) with all possible passwords based on the password conventions we've seen so far.

Hints :

## Tải các tập tin có liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ wget https://artifacts.picoctf.net/c/81/level5.py      
--2023-02-07 07:38:00--  https://artifacts.picoctf.net/c/81/level5.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.9, 13.33.33.37, 13.33.33.69, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.9|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1168 (1.1K) [application/octet-stream]
Saving to: ‘level5.py’

level5.py          100%[===============>]   1.14K  --.-KB/s    in 0s      

2023-02-07 07:38:02 (31.9 MB/s) - ‘level5.py’ saved [1168/1168]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ wget https://artifacts.picoctf.net/c/81/level5.flag.txt.enc
--2023-02-07 07:38:12--  https://artifacts.picoctf.net/c/81/level5.flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.37, 13.33.33.97, 13.33.33.9, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.37|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [application/octet-stream]
Saving to: ‘level5.flag.txt.enc’

level5.flag.txt.en 100%[===============>]      31  --.-KB/s    in 0s      

2023-02-07 07:38:15 (95.4 MB/s) - ‘level5.flag.txt.enc’ saved [31/31]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ wget https://artifacts.picoctf.net/c/81/level5.hash.bin    
--2023-02-07 07:38:21--  https://artifacts.picoctf.net/c/81/level5.hash.bin
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.9, 13.33.33.69, 13.33.33.97, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.9|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16 [application/octet-stream]
Saving to: ‘level5.hash.bin’

level5.hash.bin    100%[===============>]      16  --.-KB/s    in 0s      

2023-02-07 07:38:33 (31.7 MB/s) - ‘level5.hash.bin’ saved [16/16]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ wget https://artifacts.picoctf.net/c/81/dictionary.txt 
--2023-02-07 07:38:38--  https://artifacts.picoctf.net/c/81/dictionary.txt
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.224.250.75, 13.224.250.70, 13.224.250.39, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.224.250.75|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 327680 (320K) [application/octet-stream]
Saving to: ‘dictionary.txt’

dictionary.txt     100%[===============>] 320.00K  85.3KB/s    in 3.7s    

2023-02-07 07:38:47 (85.3 KB/s) - ‘dictionary.txt’ saved [327680/327680]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ ls -alh 
total 340K
drwxr-xr-x  2 kali kali 4.0K Feb  7 07:38 .
drwxr-xr-x 23 kali kali 4.0K Feb  7 07:37 ..
-rw-r--r--  1 kali kali 320K Jan  4  2022 dictionary.txt
-rw-r--r--  1 kali kali   31 Jan  4  2022 level5.flag.txt.enc
-rw-r--r--  1 kali kali   16 Jan  4  2022 level5.hash.bin
-rw-r--r--  1 kali kali 1.2K Jan  4  2022 level5.py
```

# Thu thập thông tin :

## Chạy thử chương trình :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ python level5.py 
Please enter correct password for flag: 12345 
That password is incorrect
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ python level5.py
Please enter correct password for flag: anhtuan
That password is incorrect
```

## Nội dung của các tập tin :

Nội dung tập tin “dictionary.txt” :

→ chứa các mật khẩu gợi ý có khả năng đăng nhập thành công.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ cat dictionary.txt | less
0000
0001
0002
0003
0004
0005
0006
0007
:
```

Nội dung tập tin “level5.flag.txt.enc”:

→ Chứa nội dung cờ (flag) sau khi đã được mã hóa.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ cat level5.flag.txt.enc  

QPX:K
        T^:VQWV▒
```

Nội dung tập tin “level5.hash.bin” :

→ Mình dự đoán là nó dùng để kiểm tra giá trị nhập vào.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ cat level5.hash.bin       
B8sY�ɶ��$";
```

Nôi dung của tập tin “level5.py” :

→ Phần trọng tâm của vấn đề.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ cat level5.py 
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

flag_enc = open('level5.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level5.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

def level_5_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)
    
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

level_5_pw_check()
```

# Khai thác và thu thập cờ (flag) :

Nhìn chung thì nó cũng tương tự như các bài trước (PW Crack 4 và 3) nên cũng sẽ có các bước như :

- Đều đọc nội dung từ tập tin “level5.hash.bin”.
- So sách chuỗi nhập vào sau khi chạy qua hàm hash_pw.
- Nếu nhập vào đúng thì in ra nội dung của cờ (flag) và nhập sai thì in ra màn hình câu “That password is incorrect”.

Còn tập tin “dictionary.txt” sẽ chứa tập hợp các mật khẩu và trong số chúng sẽ có một mật khẩu là đúng. Như vậy thì mình có thể sử dụng cach thức “vét cạn” để tìm ra mật khẩu đúng (tức là thử từng trường hợp có thể đến khi nào tìm ra thì thôi).

Làm bằng tay thì vấn đề là thời gian nên mình sẽ sử dụng python để viết một chương trình đơn giản để thực hiện điều đó.

Số lượng lớn các mật khẩu thì mình dùng máy để chạy tự động.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ cat dictionary.txt| wc -l
65536
```

Chương trình sẽ thực hiện các công việc như :

Đọc nội dung từ tập tin “dictionary.txt” và chuyển chúng vào mảng “numbr” (do các kí tự đều xuống dòng nên hàm split() không cần phải thêm điều kiện).

Đọc nội dung từ tập tin “level5.hash.bin”.

Sử dụng lại hàm hash_pw().

Vòng lặp “for” sẽ sử dụng các thành phần trong mảng “numbr” và chạy qua hàm hash_pw để so sách kết quả với nội dung tập tin “level5.hash.bin”.

Nếu kết quả trùng thì xuất phần tử trong mảng đó.

```python
import hashlib

with open("dictionary.txt", "r") as my_file:
	numbr = my_file.read().split()
	
correct_pw_hash = open('level5.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()
    
    
for x in numbr:
	if (correct_pw_hash == hash_pw(x)):
		print(x)
```

Khi thực thi chương trình “vét cạn” mật khẩu (checkPassHash.py).

→ Kết quả cho thấy có một mật khẩu trùng với điều kiện.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ python checkPassHash.py
eee0
```

Thử sử dụng mật khẩu ấy để nhập vào chương trình chính.

→ Kết quả cho ra nội dung của cờ (flag).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/PW Crack 5]
└─$ python level5.py       
Please enter correct password for flag: eee0          
Welcome back... your flag, user:
picoCTF{****_********_fffcda23}
```

# Tổng kết :

→ Nội dung của cờ (flag) là : `picoCTF{****_********_fffcda23}`