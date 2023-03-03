---
title: picoCTF | [100 points] [GeneralSkill] Serpentine WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] Serpentine WriteUp


# Tổng quan :

## Tóm tắt nội dung :

- Bài tập này thì cũng khá đơn giản do chỉ sử dụng những thao tác cơ bản.
- Chỉ cần sửa đổi nội dung của mã nguồn chương trình là có thể thu được nội dung của cờ.

Nội dung của cờ : `picoCTF{7h3_r04d_l355_7r4v3l3d_8e47d128}`

## Tác giả và mô tả :

Author: LT 'syreal' Jones

Description :  Find the flag in the Python script!
[Download Python script](https://artifacts.picoctf.net/c/95/serpentine.py)

Hints : 

- Try running the script and see what happens
- In the webshell, try examining the script with a text editor like `nano`
- To exit `nano`, press Ctrl and x and follow the on-screen prompts.
- The `str_xor` function does not need to be reverse engineered for this
challenge.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/serpentine]
└─$ wget https://artifacts.picoctf.net/c/95/serpentine.py
--2023-02-10 00:21:39--  https://artifacts.picoctf.net/c/95/serpentine.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.81, 108.157.30.86, 108.157.30.63, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.81|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2550 (2.5K) [application/octet-stream]
Saving to: ‘serpentine.py’

serpentine.py                100%[=============================================>]   2.49K  --.-KB/s    in 0s      

2023-02-10 00:21:40 (54.3 MB/s) - ‘serpentine.py’ saved [2550/2550]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/serpentine]
└─$ ls -alh
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb 10 00:21 .
drwxr-xr-x 26 kali kali 4.0K Feb 10 00:21 ..
-rw-r--r--  1 kali kali 2.5K Jan  4  2022 serpentine.py
```

# Khai thác và thu thập cờ :

Thử khởi chạy tập tin chương trình viết bằng ngôn ngữ Python.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/serpentine]
└─$ python serpentine.py                                                                         

    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \
                   /   /               \  \
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~

Welcome to the serpentine encourager!

a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) b

Oops! I must have misplaced the print_flag function! Check my source code!

a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) a

-----------------------------------------------------
Look how far you've come!
-----------------------------------------------------

```

Đọc nội dung (mã nguồn) của chương trình.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/serpentine]
└─$ cat serpentine.py              

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

flag_enc = chr(0x15) + chr(0x07) + chr(0x08) + chr(0x06) + chr(0x27) + chr(0x21) + chr(0x23) + chr(0x15) + chr(0x5c) + chr(0x01) + chr(0x57) + chr(0x2a) + chr(0x17) + chr(0x5e) + chr(0x5f) + chr(0x0d) + chr(0x3b) + chr(0x19) + chr(0x56) + chr(0x5b) + chr(0x5e) + chr(0x36) + chr(0x53) + chr(0x07) + chr(0x51) + chr(0x18) + chr(0x58) + chr(0x05) + chr(0x57) + chr(0x11) + chr(0x3a) + chr(0x56) + chr(0x0e) + chr(0x5d) + chr(0x53) + chr(0x11) + chr(0x54) + chr(0x5c) + chr(0x53) + chr(0x14)

def print_flag():
  flag = str_xor(flag_enc, 'enkidu')
  print(flag)

def print_encouragement():
  encouragements = ['You can do it!', 'Keep it up!', 
                    'Look how far you\'ve come!']
  choice = random.choice(range(0, len(encouragements)))
  print('\n-----------------------------------------------------')
  print(encouragements[choice])
  print('-----------------------------------------------------\n\n')

def main():

  print(
'''
    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \\
                   /   /               \  \\
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~
'''
  )
  print('Welcome to the serpentine encourager!\n\n')
  
  while True:
    print('a) Print encouragement')
    print('b) Print flag')
    print('c) Quit\n')
    choice = input('What would you like to do? (a/b/c) ')
    
    if choice == 'a':
      print_encouragement()
      
    elif choice == 'b':
      print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')
      
    elif choice == 'c':
      sys.exit(0)
      
    else:
      print('\nI did not understand "' + choice + '", input only "a", "b" or "c"\n\n')

if __name__ == "__main__":
  main()
```

Nhìn chung cơ bản thì trên chương trình có một hàm tạo nội dung của cờ nhưng không có gọi ra nên mình chỉ cần thêm một dòng để gọi hàm `print_flag()` .

Mình thêm vào lựa chọn chữ “b” trong xét điều kiện if.

```bash
if choice == 'a':
      print_encouragement()
      
    elif choice == 'b':
      print('\nOops! I must have misplaced the print_flag function! Check my source code!\n\n')
      print_flag()
      print('\n\n')

      
    elif choice == 'c':
      sys.exit(0)
```

Khởi chạy chương trình lại một lần nữa sau khi thay đổi nội dung của mã nguồn.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/serpentine]
└─$ python serpentine-modify.py

    Y
  .-^-.
 /     \      .- ~ ~ -.
()     ()    /   _ _   `.                     _ _ _
 \_   _/    /  /     \   \                . ~  _ _  ~ .
   | |     /  /       \   \             .' .~       ~-. `.
   | |    /  /         )   )           /  /             `.`.
   \ \_ _/  /         /   /           /  /                `'
    \_ _ _.'         /   /           (  (
                    /   /             \  \
                   /   /               \  \
                  /   /                 )  )
                 (   (                 /  /
                  `.  `.             .'  /
                    `.   ~ - - - - ~   .'
                       ~ . _ _ _ _ . ~

Welcome to the serpentine encourager!

a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c) b

Oops! I must have misplaced the print_flag function! Check my source code!

picoCTF{7h3_r04d_l355_7r4v3l3d_8e47d128}

a) Print encouragement
b) Print flag
c) Quit

What would you like to do? (a/b/c)
```

# Tổng kết :

→ Nội dung của cờ : `picoCTF{7h3_r04d_l355_7r4v3l3d_8e47d128}`