---
title: picoCTF | [200 points] [GeneralSkill] plumbing WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [200 points] [GeneralSkill] plumbing WriteUp

Quá trình thực hiện: Done

# Tổng quan :

## Tóm tắt nội dung :

- Kết nối bằng netcat đến máy chủ sẽ trả về chuỗi kí tự có chứa nội dung của cờ.
- Có thể sử dụng pipe kết hợp với lệnh grep để tìm nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{digital_plumb3r_5ea1fbd7}`

## Tác giả và mô tả :

Author: Alex Fulton/Danny Tunitis

Description :  Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to `jupiter.challenges.picoctf.org 4427`.

Hints :

- Remember the flag format is picoCTF{XXXX}
- What's a pipe? No not that kind of pipe... This [kind](http://www.linfo.org/pipes.html)

# Khai thác và thu thập cờ (flag) :

Khi truy cập bằng netcat sẽ hiển thị một chuỗi kí tự khác nhau. Mình chuyển khối dữ liệu ấy vào bên trong một tập tin sample.txt

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/plumbing]
└─$ nc jupiter.challenges.picoctf.org 4427 > sample.txt
```

Sử dụng pip bao gồm lệnh `cat` để hiển thị nội dung và `grep` để tìm kiếm từ khóa.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/plumbing]
└─$ cat sample.txt| grep pico
picoCTF{digital_plumb3r_5ea1fbd7}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{digital_plumb3r_5ea1fbd7}`