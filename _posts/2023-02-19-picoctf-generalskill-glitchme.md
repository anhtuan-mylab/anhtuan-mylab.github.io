---
title: picoCTF | [100 points] [GeneralSkill] Glitch Cat WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] Glitch Cat WriteUp


# Tổng quan :

## Tóm tắt nội dung :

Bài tập yêu cầu kết nối đến một máy chủ và thu được một chuỗi kí tự có thể chuyển đổi thành cờ. Bài viết cung cấp mã và hướng dẫn để chuyển đổi chuỗi đó thành cờ.

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Our flag printing service has started glitching!`$ nc saturn.picoctf.net 65353`

Hints :

- ASCII is one of the most common encodings used in programming.
- We know that the glitch output is valid Python, somehow!
- Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

# Khai thác và thu thập cờ :

Thử kết nối đến máy chủ bằng netcat.

→ Thu được có thể là nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/fixme2.py]
└─$ nc saturn.picoctf.net 65353
'picoCTF{gl17ch_m3_n07_' + chr(0x39) + chr(0x63) + chr(0x34) + chr(0x32) + chr(0x61) + chr(0x34) + chr(0x35) + chr(0x64) + '}'
```

Có thể dùng python để chuyển đổi chuỗi thành dạng kí tự.

→ Kết quả là nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/glitch-cat]
└─$ python                     
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print('picoCTF{gl17ch_m3_n07_' + chr(0x39) + chr(0x63) + chr(0x34) + chr(0x32) + chr(0x61) + chr(0x34) + chr(0x35) + chr(0x64) + '}')
picoCTF{gl17ch_m3_n07_9c42a45d}
>>>
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{gl17ch_m3_n07_9c42a45d}`