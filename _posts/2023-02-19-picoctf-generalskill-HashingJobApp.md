---
title: picoCTF | [100 points] [GeneralSkill] HashingJobApp WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] HashingJobApp WriteUp



# Tổng quan :

## Tóm tắt nội dung :

Đây là một bài viết hướng dẫn giải một bài tập trên picoCTF về việc mã hóa md5. Bài tập yêu cầu nhập các chuỗi và mã hóa chúng thành md5, và nếu trả lời đúng sẽ thu được nội dung của cờ. Bài viết cung cấp một số gợi ý và một đoạn mã Python để giúp giải quyết bài tập. 

Nội dung của cờ là "picoCTF{4ppl1c4710n_r3c31v3d_674c1de2}".

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : If you want to hash with the best, beat this test!`nc saturn.picoctf.net 54555`

Hints :

- You can use a commandline tool or web app to hash text.
- Press Ctrl and c on your keyboard to close your connection and return to the command prompt.

# Khai thác và thu thập cờ (flag) :

Khi truy cập bằng netcat vào đường dẫn theo sẽ hiển thị câu hỏi và trả lời đáp án nhập vào bàn phím.

→ Nếu nhập câu trả lời đúng có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] hashingjobapp]
└─$ nc saturn.picoctf.net 54555

Please md5 hash the text between quotes, excluding the quotes: 'chains'
Answer:
```

Có thể sử dụng công cụ online để mã hóa dạng md5 nhưng mình co thể sử dụng Python bằng cách viết một chương trình đơn giản dùng để mã hóa md5 từ một chuỗi nhập vào bàn phím.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/hashingjobapp]
└─$ cat pythonmd5hash.py 
import hashlib

a = input('Nhap chuoi: ')
result = hashlib.md5(a.encode())

print("Chuoi hash md5: " + result.hexdigest())
```

Khi thực hiện kết nối bằng netcat vào đường dẫn thì sẽ yêu cầu mã hóa các chuỗi, nếu trả lời đúng sẽ hiển thị chuỗi tiếp theo.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/hashingjobapp]
└─$ python pythonmd5hash.py 
Nhap chuoi: corn on the cob
Chuoi hash md5: 4d26abd4924cfb39c48f7841dd579c0d
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/hashingjobapp]
└─$ python pythonmd5hash.py
Nhap chuoi: a used car lot
Chuoi hash md5: 1a78bc064f8df8b4192ce5f8972c9b80
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/hashingjobapp]
└─$ python pythonmd5hash.py
Nhap chuoi: Babe Ruth
Chuoi hash md5: 3875acc0c1561d949c39685e96b9a4bb
```

Sử dụng các chuỗi đã được mã hóa md5 để nhập vào yêu cầu.

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/hashingjobapp]
└─$ nc saturn.picoctf.net 54555

Please md5 hash the text between quotes, excluding the quotes: 'corn on the cob'
Answer: 
4d26abd4924cfb39c48f7841dd579c0d
4d26abd4924cfb39c48f7841dd579c0d
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'a used car lot'
Answer: 
1a78bc064f8df8b4192ce5f8972c9b80
1a78bc064f8df8b4192ce5f8972c9b80
Correct.
Please md5 hash the text between quotes, excluding the quotes: 'Babe Ruth'
Answer: 
3875acc0c1561d949c39685e96b9a4bb
3875acc0c1561d949c39685e96b9a4bb
Correct.
picoCTF{4ppl1c4710n_r3c31v3d_674c1de2}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{4ppl1c4710n_r3c31v3d_674c1de2}`

# Tài liệu tham khảo :

[https://www.geeksforgeeks.org/md5-hash-python/](https://www.geeksforgeeks.org/md5-hash-python/)