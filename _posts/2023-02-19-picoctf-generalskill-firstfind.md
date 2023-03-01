---
title: picoCTF | [100 points] [GeneralSkill] First Find WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] first find WriteUp


# Tổng quan :

## Tóm tắt nội dung :

Tập tin được giải nén với nhiều nội dung khác nhau nằm trong thư mục và một trong số những thư mục ấy có chứa tập tin cờ.

Có thể sử dụng lệnh ls trên Linux để tìm kiếm tập tin.

Nội dung của cờ (flag) : `picoCTF{f1nd_15_f457_ab443fd1}`

## Tác giả và mô tả :

Author: LT 'syreal' Jones

Description :

Unzip this archive and find the file named 'uber-secret.txt'

- [Download zip file](https://artifacts.picoctf.net/c/550/files.zip)

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-find]
└─$ wget https://artifacts.picoctf.net/c/550/files.zip
--2023-02-10 00:06:33--  https://artifacts.picoctf.net/c/550/files.zip
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.63, 108.157.30.45, 108.157.30.81, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.63|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3995553 (3.8M) [application/octet-stream]
Saving to: ‘files.zip’

files.zip                    100%[==============================================>]   3.81M   900KB/s    in 4.7s    

2023-02-10 00:06:39 (829 KB/s) - ‘files.zip’ saved [3995553/3995553]

                                                                                                                    
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-find]
└─$ ls -alh
total 3.9M
drwxr-xr-x  2 kali kali 4.0K Feb 10 00:06 .
drwxr-xr-x 25 kali kali 4.0K Feb  9 23:44 ..
-rw-r--r--  1 kali kali 3.9M May 16  2022 files.zip
```

# Khai thác và thu thập cờ (flag) :

Sau khi tải tập tin về thì với nội dung mở rộng là *.zip nên có thể sử dụng unzip để giải nén.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-find]
└─$ unzip files.zip      
Archive:  files.zip
   creating: files/
   creating: files/satisfactory_books/
   creating: files/satisfactory_books/more_books/
  inflating: files/satisfactory_books/more_books/37121.txt.utf-8  
  inflating: files/satisfactory_books/23765.txt.utf-8  
  inflating: files/satisfactory_books/16021.txt.utf-8  
  inflating: files/13771.txt.utf-8   
   creating: files/adequate_books/
   creating: files/adequate_books/more_books/
   creating: files/adequate_books/more_books/.secret/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/
   creating: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/
 extracting: files/adequate_books/more_books/.secret/deeper_secrets/deepest_secrets/uber-secret.txt  
  inflating: files/adequate_books/more_books/1023.txt.utf-8  
  inflating: files/adequate_books/46804-0.txt  
  inflating: files/adequate_books/44578.txt.utf-8  
   creating: files/acceptable_books/
   creating: files/acceptable_books/more_books/
  inflating: files/acceptable_books/more_books/40723.txt.utf-8  
  inflating: files/acceptable_books/17880.txt.utf-8  
  inflating: files/acceptable_books/17879.txt.utf-8  
  inflating: files/14789.txt.utf-8
```

Với nội dung là cờ có thể được giấu bên trong một thư mục nằm trong tập các thư mục ấy. 

Mình thì sử dụng dòng lệnh ls trên Linux với các điều kiện như -alhR, hiểu đơn giản là tớ muốn hiển thị nội dung của các tập tin bên trong các thư mục bao gồm thư mục ẩn.

Mình thấy có một tập tin là uber-sercret.txt.

→ Có thể là nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-find/files]
└─$ ls -alhR            
.:
total 1.9M
drwxrwxr-x 5 kali kali 4.0K May 13  2022 .
drwxr-xr-x 3 kali kali 4.0K Feb 10 00:07 ..
-rw-rw-r-- 1 kali kali 981K May  6  2022 13771.txt.utf-8
-rw-rw-r-- 1 kali kali 939K May  7  2022 14789.txt.utf-8
drwxrwxr-x 3 kali kali 4.0K May 13  2022 acceptable_books
drwxrwxr-x 3 kali kali 4.0K May 13  2022 adequate_books
drwxrwxr-x 3 kali kali 4.0K May 13  2022 satisfactory_books

./acceptable_books:
total 1.8M
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 5 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 881K May  8  2022 17879.txt.utf-8
-rw-rw-r-- 1 kali kali 881K May  8  2022 17880.txt.utf-8
drwxrwxr-x 2 kali kali 4.0K May 13  2022 more_books

./acceptable_books/more_books:
total 200K
drwxrwxr-x 2 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 192K Apr 18  2022 40723.txt.utf-8

./adequate_books:
total 3.9M
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 5 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 1.9M Apr 20  2022 44578.txt.utf-8
-rw-rw-r-- 1 kali kali 2.0M Sep  7  2014 46804-0.txt
drwxrwxr-x 3 kali kali 4.0K May 13  2022 more_books

./adequate_books/more_books:
total 2.0M
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 2.0M May  1  2022 1023.txt.utf-8
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .secret

./adequate_books/more_books/.secret:
total 12K
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
drwxrwxr-x 3 kali kali 4.0K May 13  2022 deeper_secrets

./adequate_books/more_books/.secret/deeper_secrets:
total 12K
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
drwxrwxr-x 2 kali kali 4.0K May 13  2022 deepest_secrets

./adequate_books/more_books/.secret/deeper_secrets/deepest_secrets:
total 12K
drwxrwxr-x 2 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali   31 May 13  2022 uber-secret.txt

./satisfactory_books:
total 276K
drwxrwxr-x 3 kali kali 4.0K May 13  2022 .
drwxrwxr-x 5 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 217K May  7  2022 16021.txt.utf-8
-rw-rw-r-- 1 kali kali  44K May 11  2022 23765.txt.utf-8
drwxrwxr-x 2 kali kali 4.0K May 13  2022 more_books

./satisfactory_books/more_books:
total 132K
drwxrwxr-x 2 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali 122K Apr 17  2022 37121.txt.utf-8
```

Truy cập và đọc nội dung từ tập tin uber-secret.txt

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-find/files]
└─$ cd adequate_books/more_books/.secret/deeper_secrets/deepest_secrets  
                                                                                                                   
┌──(kali㉿kali)-[~/…/more_books/.secret/deeper_secrets/deepest_secrets]
└─$ ls -alh 
total 12K
drwxrwxr-x 2 kali kali 4.0K May 13  2022 .
drwxrwxr-x 3 kali kali 4.0K May 13  2022 ..
-rw-rw-r-- 1 kali kali   31 May 13  2022 uber-secret.txt
```

```bash
┌──(kali㉿kali)-[~/…/more_books/.secret/deeper_secrets/deepest_secrets]
└─$ cat uber-secret.txt                                                
picoCTF{f1nd_15_f457_ab443fd1}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{f1nd_15_f457_ab443fd1}`