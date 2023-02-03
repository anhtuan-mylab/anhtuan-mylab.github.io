---
title: picoCTF | [100 pointz] [Cryptography] CreedStuff WriteUp
date: 2023-02-03
categories: [picoCTF, Cryptography]
---




# [100 pointz] [Cryptography] CreedStuff WriteUp

# Tổng quan :

## Thông tin tác giả và mô tả :

AUTHOR: WILL HONG / LT 'SYREAL' JONES

Description : 

- We found a leak of a blackmarket website's login credentials. Can you find the password of the user `cultiris` and successfully decrypt it?
- Download the leak [here](https://artifacts.picoctf.net/c/534/leak.tar).
- The first user in `usernames.txt` corresponds to the first password in `passwords.txt` .The second user corresponds to the second password, and so on.

Hints : Maybe other passwords will have hints about the leak?

# Thu thập thông tin :

Sau khi tải 2 tập tin có liên quan về máy, bao gồm :

- username.txt → Chứa những tên của người dùng.
- password.txt → chứa mật khẩu đã được mã hóa.

Theo như yêu cầu thì tìm mật khẩu của tài khoản cultiris mà khi mình đối chiếu với tập tin “password.txt” thì nó có dạng như `cvpbPGS{P7e1S_54I35_71Z3}`

→ Mình đoán đây có thể là nội dung của cờ (flag).

Mục tiêu lúc này là tìm ra các thức mã hóa để giải mã lại thông điệp và lấy nội dung cần.

# Khai thác và thu thập flag :

Trong phần gợi ý có đề cập đến yếu tố liên quan đến cách thức mã hóa nên mình sẽ tập trung nhiều vào tập tin “password.txt” nhưng cũng đối chiếu chúng với tập tin “username.txt” do mỗi một người dùng thì tương ứng với một một khẩu nhất định.

Mình tìm thấy một tên người dùng là `pico` (dòng 204) và mật khẩu tương ứng là `pICo7rYpiCoU51N6PicOr0t13`

Đọc thì thấy có chữ rot13 → đây có thể là manh mối để tìm ra thông điệp ban đầu.

Theo wikipedia, ROT13 ("xoay theo 13 vị trí", đôi khi được gạch nối ROT-13) là một mật mã thay thế chữ cái đơn giản thay thế một chữ cái bằng chữ cái thứ 13 sau nó trong bảng chữ cái. ROT13 là một trường hợp đặc biệt của mật mã Caesar được phát triển ở La Mã cổ đại.

Từ những điều trên, mình nghĩ là có thể thông điệp ban đầu được mã hóa bằng ROT13. Quay trở lại với tài khoản `cultiris` với mật khẩu là `cvpbPGS{P7e1S_54I35_71Z3}` 

Mình có thể thử với cụm từ `cvpbPGS` trước.

→ Kết quả là đúng khi cụm từ sau khi giải mã sẽ trở thành picoCTF.

```bash
C:\Users\anhtuan>python
Python 3.10.7 (tags/v3.10.7:6cc6b13, Sep  5 2022, 14:08:36) [MSC v.1933 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import codecs
>>> text = 'cvpbPGS'
>>> plain = codecs.decode(text, 'rot_13')
>>> print(plain)
picoCTF
>>>
```

Thực hiện tương tự cho cả chuỗi 

→ Thu được nội dung của cờ (flag) cần tìm.

```bash
C:\Users\anhtuan>python
Python 3.10.7 (tags/v3.10.7:6cc6b13, Sep  5 2022, 14:08:36) [MSC v.1933 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import codecs
>>> text = 'cvpbPGS{P7e1S_54I35_71Z3}'
>>> plain = codecs.decode(text, 'rot_13')
>>> print(plain)
picoCTF{C7r1F_*****_****}
>>>
```

# Tài liệu tham khảo :

[https://vi.wikipedia.org/wiki/ROT13](https://vi.wikipedia.org/wiki/ROT13)

[https://cryptii.com/pipes/rot13-decoder](https://cryptii.com/pipes/rot13-decoder)