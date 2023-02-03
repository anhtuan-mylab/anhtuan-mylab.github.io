---
title: picoCTF | [200 point] [ReverseEngineering] bloat.py WriteUp
date: 2023-02-03
categories: [picoCTF, Reverse Engineering]
---



# [200 point] [ReverseEngineering] bloat.py WriteUp

# Tổng quan :

## Thông tin về tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES

Description : Can you get the flag ? Run this [Python program](https://artifacts.picoctf.net/c/428/bloat.flag.py) in the same directory as this [encrypted flag](https://artifacts.picoctf.net/c/428/flag.txt.enc).

## Tải tập tin về máy :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ wget https://artifacts.picoctf.net/c/428/bloat.flag.py                                             
--2023-02-02 08:34:17--  https://artifacts.picoctf.net/c/428/bloat.flag.py
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.81, 108.157.30.45, 108.157.30.63, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.81|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1557 (1.5K) [application/octet-stream]
Saving to: ‘bloat.flag.py’

bloat.flag.py                100%[=============================================>]   1.52K  --.-KB/s    in 0s      

2023-02-02 08:34:18 (42.4 MB/s) - ‘bloat.flag.py’ saved [1557/1557]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ wget https://artifacts.picoctf.net/c/428/flag.txt.enc 
--2023-02-02 08:34:27--  https://artifacts.picoctf.net/c/428/flag.txt.enc
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.86, 108.157.30.81, 108.157.30.45, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.86|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 35 [application/octet-stream]
Saving to: ‘flag.txt.enc’

flag.txt.enc                 100%[=============================================>]      35  --.-KB/s    in 0s      

2023-02-02 08:34:28 (23.0 MB/s) - ‘flag.txt.enc’ saved [35/35]

                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ ls -alh
total 16K
drwxr-xr-x 2 kali kali 4.0K Feb  2 08:34 .
drwxr-xr-x 8 kali kali 4.0K Feb  2 08:33 ..
-rw-r--r-- 1 kali kali 1.6K Mar 15  2022 bloat.flag.py
-rw-r--r-- 1 kali kali   35 Mar 15  2022 flag.txt.enc
```

# Thu thập thông tin :

Nhìn chung thì có 2 tập tin :

- bloat.flag.py
- flag.txt.enc

Nội dung tập tin bloat.flag.py thì dùng để in ra màn hình nội dung của cờ (flag) (mình dự đoán là vậy).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ cat bloat.flag.py 
import sys
a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False
def arg111(arg444):
  return arg122(arg444.decode(), a[81]+a[64]+a[79]+a[82]+a[66]+a[64]+a[75]+\
a[75]+a[72]+a[78]+a[77])
def arg232():
  return input(a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94])
def arg132():
  return open('flag.txt.enc', 'rb').read()
def arg112():
  print(a[54]+a[68]+a[75]+a[66]+a[78]+a[76]+a[68]+a[94]+a[65]+a[64]+a[66]+\
a[74]+a[13]+a[13]+a[13]+a[94]+a[88]+a[78]+a[84]+a[81]+a[94]+a[69]+\
a[75]+a[64]+a[70]+a[11]+a[94]+a[84]+a[82]+a[68]+a[81]+a[25])
def arg122(arg432, arg423):
    arg433 = arg423
    i = 0
    while len(arg433) < len(arg432):
        arg433 = arg433 + arg423[i]
        i = (i + 1) % len(arg423)        
    return "".join([chr(ord(arg422) ^ ord(arg442)) for (arg422,arg442) in zip(arg432,arg433)])
arg444 = arg132()
arg432 = arg232()
arg133(arg432)
arg112()
arg423 = arg111(arg444)
print(arg423)
sys.exit(0)
```

Nội dung của tập tin “flag.txt.enc”.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ cat flag.txt.enc 
\^FU[]Y1V,Y\Z[
```

Thử khởi chạy chương trình và thử nhập một vài thông tin.

→ Có thể dự đoán là nhập đúng thì xuất nội dung cờ (flag) và nhập sai thì in câu “The password is incorrent”.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ python bloat.flag.py                                                                        
Please enter correct password for flag: 12345
That password is incorrect

┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ python bloat.flag.py
Please enter correct password for flag: anhtuan
That password is incorrect
```

# Khai thác và thu thập cờ (flag) :

Mình nhìn thử lại nội dung của tập tin “bloat.flag.py”, nhận xét một điều là đúng như tên gọi (bloat).

Lúc nãy khi mình chạy chương trình thì có in ra màn hình câu “Please enter correct password for flag:” mà nhìn trên mã nguồn thì chả thấy có câu chuỗi nào (dạng string) nhưng lại có rất nhiều kí tự dạng a[x] (thật ra thì cái này chỉ vị trí và hiển thị nội dung là kí tự nếu gán cho biến)

→ Các dạng a[x] chắc chắn sẽ tạo thành một chuỗi.

Về phần thực thi thì nhìn các câu lệnh gần cuối, cụ thể là : 

```python
arg444 = arg132()
arg432 = arg232()
arg133(arg432)
arg112()
arg423 = arg111(arg444)
print(arg423)
```

Như vậy, đầu tiên sẽ chạy hàm arg132() gán với biến arg444

→ Mở tập tin “flag.txt.enc” và với quyền đọc.

```python
def arg132():
  return open('flag.txt.enc', 'rb').read()
```

Kế tiếp là đến với thực thi hàm arg232() gán với biến arg432

→ Thấy có sử dụng input thì chắc chắn đây là cái câu “Please enter correct password for flag:”

```python
def arg232():
  return input(a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94])
```

Để kiểm tra thì có thể thử dùng python để in ra nội dung chỗ hàm input ấy.

```bash
┌──(kali㉿kali)-[~/Desktop/test]
└─$ python              
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
...             "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
>>> print (a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
... a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
... a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
... a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94])
Please enter correct password for flag:
```

Tiếp theo là thực thi hàm arg133(arg432) với giá trị vào là nội dung nhập từ bàn phím (biến arg432). Hiểu đơn giản thì nó sẽ so sánh biến arg432 (tức nhập từ bàn phím) với các kí tự dạng a[x]. Nếu đúng thì trả về giá trị “true”, sai thì hiển thị một câu gì ấy và trả về giá trị “false”.

```python
def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False
```

Cũng thực hiện tương tư như lúc nãy, mình cũng sẽ thử dùng python để tìm câu chuỗi bí mật ấy.

```bash
┌──(kali㉿kali)-[~/Desktop/test]
└─$ python
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
...             "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
>>> print(a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68])
happychance
>>> print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
... a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
... a[81]+a[68]+a[66]+a[83])
That password is incorrect
```

Như vậy thì nội dung nhập vào từ bàn phím phải là “happychance” và thử nhập vào chương trình.

→ Kết quả là xuất ra nội dung của cờ (flag).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/bloat.py]
└─$ python bloat.flag.py 
Please enter correct password for flag: happychance
Welcome back... your flag, user:
picoCTF{d30bfu5c4710n_***_********}
```

# Tài liệu tham khảo :

[https://www.w3schools.com/python/ref_func_print.asp](https://www.w3schools.com/python/ref_func_print.asp)

[https://www.w3schools.com/python/python_arrays.asp](https://www.w3schools.com/python/python_arrays.asp)