---
title: picoCTF | [200 points] [GeneralSkill] Based WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [200 points] [200 points] Based WriteUp


# Tổng quan :

## Tác giả và mô tả :

Author: Alex Fulton/Daniel Tunitis

Description :To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337? 

Connect with `nc jupiter.challenges.picoctf.org 29956`.

Hints :

- I hear python can convert things.
- It might help to have multiple windows open.

# Khai thác và thu thập cờ (flag) :

Kết nối bằng netcat.

```
┌──(kali㉿kali)-[~/Desktop/picoCTF/Based]
└─$ nc jupiter.challenges.picoctf.org 29956
Let us see how data is stored
falcon
Please give the 01100110 01100001 01101100 01100011 01101111 01101110 as a word.
...
you have 45 seconds.....

Input:
```

Sử dụng trang web [https://www.dcode.fr/ascii-code](https://www.dcode.fr/ascii-code) để thực hiện chuyển đổi sang mã ASCII.

```
┌──(kali㉿kali)-[~/Desktop/picoCTF/Based]
└─$ nc jupiter.challenges.picoctf.org 29956
Let us see how data is stored
falcon
Please give the 01100110 01100001 01101100 01100011 01101111 01101110 as a word.
...
you have 45 seconds.....

Input:
falcon
Please give me the  143 157 155 160 165 164 145 162 as a word.
Input:
computer
Please give me the 7375626d6172696e65 as a word.
Input:
submarine
You've beaten the challenge
Flag: picoCTF{learning_about_converting_values_b375bb16}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{learning_about_converting_values_b375bb16}`