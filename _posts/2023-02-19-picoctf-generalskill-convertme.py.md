---
title: picoCTF | [100 points] [GeneralSkill] convertme.py WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---



# [100 points] [GeneralSkill] convertme.py

Hoàn thiện báo cáo: Hoàn thành 
Quá trình thực hiện: Done

# Tổng quan

## Tóm tắt :

Đây là một bài tập yêu cầu chuyển đổi một số từ hệ thập phân sang hệ nhị phân. Để giải quyết bài tập, ta có thể sử dụng một công cụ chuyển đổi trực tuyến hoặc viết một chương trình đơn giản để chuyển đổi. Sau khi chuyển đổi, ta cần nhập kết quả vào chương trình để lấy được cờ. Cờ của bài tập này là "picoCTF{4ll_y0ur_b4535_722f6b39}".

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description

Run the Python script and convert the given number from decimal to binary to get the flag.

[Download Python script](https://artifacts.picoctf.net/c/32/convertme.py)

Hints : 

- Look up a decimal to binary number conversion app on the web or use your computer's calculator!
- The `str_xor` function does not need to be reverse engineered for this challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ wget https://artifacts.picoctf.net/c/32/convertme.py 
--2023-02-05 03:38:20--  https://artifacts.picoctf.net/c/32/convertme.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 13.33.33.37, 13.33.33.9, 13.33.33.97, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|13.33.33.37|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1189 (1.2K) [application/octet-stream]
Saving to: ‘convertme.py’

convertme.py                 100%[=============================================>]   1.16K  --.-KB/s    in 0s      

2023-02-05 03:38:22 (29.2 MB/s) - ‘convertme.py’ saved [1189/1189]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ ls -alh 
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb  5 03:38 .
drwxr-xr-x 14 kali kali 4.0K Feb  5 03:38 ..
-rw-r--r--  1 kali kali 1.2K Mar 12  2022 convertme.py
```

# Khai thác và thu thập cờ :

Do tập tin là chương trình được viết dựa trên ngôn ngữ Python nên có thể thử khởi chạy chương trình.

→ Nội dung chương trình sẽ hiển thị câu hỏi và nhập đáp án từ bàn phím.

→ Đáp án đúng sẽ hiển thị nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ python convertme.py
If 67 is in decimal base, what is it in binary base?
Answer: sdasdasd
That isn't a binary number. Binary numbers contain only 1's and 0's
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ python convertme.py
If 61 is in decimal base, what is it in binary base?
Answer: 101010
42 and 61 are not equal.
```

Thử đọc nội dung của tập tin `convertme.py` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] convertme.py]
└─$ cat convertme.py  

import random

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5f) + chr(0x05) + chr(0x08) + chr(0x2a) + chr(0x1c) + chr(0x5e) + chr(0x1e) + chr(0x1b) + chr(0x3b) + chr(0x17) + chr(0x51) + chr(0x5b) + chr(0x58) + chr(0x5c) + chr(0x3b) + chr(0x42) + chr(0x57) + chr(0x5c) + chr(0x0d) + chr(0x5f) + chr(0x06) + chr(0x46) + chr(0x5c) + chr(0x13)

num = random.choice(range(10,101))

print('If ' + str(num) + ' is in decimal base, what is it in binary base?')

ans = input('Answer: ')

try:
  ans_num = int(ans, base=2)
  
  if ans_num == num:
    flag = str_xor(flag_enc, 'enkidu')
    print('That is correct! Here\'s your flag: ' + flag)
  else:
    print(str(ans_num) + ' and ' + str(num) + ' are not equal.')
  
except ValueError:
  print('That isn\'t a binary number. Binary numbers contain only 1\'s and 0\'s')
```

Mình có thể sử dụng công cụ chuyển đổi online từ số thập phân sang nhị phân nhưng ở đây thì mình viết một chương trình đơn giản để nhập số vào và chuyển đổi thành nhị phân bằng python.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ cat decimaltobinary.py 
def decimalToBinary(n):
    return bin(n).replace("0b", "")
   
# Driver code
if __name__ == '__main__':
        a = input("Nhap so vao: ")
        print(decimalToBinary(int(a)))
```

Khởi chạy chương trình và nhập kết quả :

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ python decimaltobinary.py
Nhap so vao: 98
1100010

┌──(kali㉿kali)-[~/Desktop/picoCTF/convertme.py]
└─$ python convertme.py
If 98 is in decimal base, what is it in binary base?
Answer: 1100010
That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_722f6b39}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{4ll_y0ur_b4535_722f6b39}`