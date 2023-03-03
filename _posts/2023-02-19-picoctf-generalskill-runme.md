---
title: picoCTF | [100 points] [GeneralSkill] runme.py WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] runme.py


# Tổng quan :

## Tóm tắt nội dung :

Bài này thì chỉ khởi chạy tập tin được viết bằng ngôn ngữ Python là có thế thu được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{***_s4n1ty_***}`

## Tổng quan về tác giả và mô tả :

AUTHOR: SUJEET KUMAR

Description

Run the `runme.py` script to get the flag. Download the script with your browser or with `wget` in the webshell. [Download runme.py Python script](https://artifacts.picoctf.net/c/86/runme.py)

## Tải tập tin liên quan về máy :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/runme.py]
└─$ wget https://artifacts.picoctf.net/c/86/runme.py      
--2023-02-07 08:11:10--  https://artifacts.picoctf.net/c/86/runme.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 54.192.18.125, 54.192.18.81, 54.192.18.87, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|54.192.18.125|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 270 [application/octet-stream]
Saving to: ‘runme.py’

runme.py           100%[===============>]     270  --.-KB/s    in 0s      

2023-02-07 08:11:17 (343 MB/s) - ‘runme.py’ saved [270/270]

┌──(kali㉿kali)-[~/Desktop/picoCTF/runme.py]
└─$ ls -alh                
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb  7 08:11 .
drwxr-xr-x 24 kali kali 4.0K Feb  7 08:10 ..
-rw-r--r--  1 kali kali  270 Jan  4  2022 runme.py
```

# Thu thập và khai thác cờ :

Sử dụng file để xem kiểu tập tin.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/runme.py]
└─$ file runme.py 
runme.py: Python script, ASCII text executable
```

Đọc nội dung của tập tin runme.py

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/runme.py]
└─$ cat runme.py                                        
#!/usr/bin/python3
################################################################################
# Python script which just prints the flag
################################################################################

flag ='picoCTF{run_s4n1ty_run}'
print(flag)
```

Khởi chạy tập tin chương trình được viết bằng ngôn ngữ Python.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/runme.py]
└─$ python runme.py 
picoCTF{***_s4n1ty_***}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{***_s4n1ty_***}`