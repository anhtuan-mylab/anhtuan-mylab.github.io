---
title: picoCTF | [005 points] [GeneralSkill] Obedient Cat WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [005 points] [GeneralSkill] Obedient Cat WriteUp


# Tổng quan :

## Tác giả và mô tả :

AUTHOR: SYREAL.

Description : This file has a flag in plain sight (aka "in-the-clear"). [Download flag](https://mercury.picoctf.net/static/fb851c1858cc762bd4eed569013d7f00/flag).

Hints :

- Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.
- To get the file accessible in your shell, enter the following in the Terminal prompt: `$ wget [https://mercury.picoctf.net/static/fb851c1858cc762bd4eed569013d7f00/flag](https://mercury.picoctf.net/static/fb851c1858cc762bd4eed569013d7f00/flag)`
- `$ man cat`

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[5 points] Obedient Cat]
└─$ wget https://mercury.picoctf.net/static/fb851c1858cc762bd4eed569013d7f00/flag
--2023-02-19 08:24:59--  https://mercury.picoctf.net/static/fb851c1858cc762bd4eed569013d7f00/flag
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 34 [application/octet-stream]
Saving to: ‘flag’

flag               100%[===============>]      34  --.-KB/s    in 0s      

2023-02-19 08:25:01 (33.1 MB/s) - ‘flag’ saved [34/34]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[5 points] Obedient Cat]
└─$ ls -alh 
total 12K
drwxr-xr-x  2 kali kali 4.0K Feb 19 08:25 .
drwxr-xr-x 33 kali kali 4.0K Feb 19 08:24 ..
-rw-r--r--  1 kali kali   34 Mar 16  2021 flag
```

# Khai thác và thu thập cờ (flag) :

Sử dụng file :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[5 points] Obedient Cat]
└─$ file flag                  
flag: ASCII text
```

Sử dụng strings :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[5 points] Obedient Cat]
└─$ strings flag                  
picoCTF{s4n1ty_v3r1f13d_28e8376d}
```

Sử dụng cat (theo như phần gợi ý) :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[5 points] Obedient Cat]
└─$ cat flag           
picoCTF{s4n1ty_v3r1f13d_28e8376d}
```

# Tổng kết :

→ Nội dung của cờ (flag) thu được : `picoCTF{s4n1ty_v3r1f13d_28e8376d}`

# Tài liệu tham khảo :

[https://tenten.vn/help/lenh-cat-trong-linux-cu-phap-va-cach-dung-cu-the/](https://tenten.vn/help/lenh-cat-trong-linux-cu-phap-va-cach-dung-cu-the/)