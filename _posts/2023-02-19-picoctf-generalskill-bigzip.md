---
title: picoCTF | [100 points] [GeneralSkill] Big Zip WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---


# [100 points] [GeneralSkill] Big Zip WriteUp



# Tổng quan :

## Tóm tắt nội dung :

- Nội dung cũng khá tương đồng với bài “First Find”. Tập tin sau khi tải về và giải nén sẽ hiển thị khối tập tin với tên gọi khác nhau. Cờ được ẩn bên trong nội dung của tập tin.
- Sử dụng lệnh grep trên Linux để tìm kiếm nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{gr3p_15_m4g1c_ef8790dc}`

## Tác giả và mô tả :

Author: LT 'syreal' Jones

Description : Unzip this archive and find the flag.

- [Download zip file](https://artifacts.picoctf.net/c/553/big-zip-files.zip)

Hints : 

- Can grep be instructed to look at every file in a directory and its subdirectories?

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/Big Zip]
└─$ wget https://artifacts.picoctf.net/c/553/big-zip-files.zip
--2023-02-11 02:16:18--  https://artifacts.picoctf.net/c/553/big-zip-files.zip
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.63, 108.157.30.86, 108.157.30.45, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.63|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3182988 (3.0M) [application/octet-stream]
Saving to: ‘big-zip-files.zip’

big-zip-files.zip  100%[===============>]   3.04M  1.82MB/s    in 1.7s    

2023-02-11 02:16:21 (1.82 MB/s) - ‘big-zip-files.zip’ saved [3182988/3182988]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/Big Zip]
└─$ ls -alh 
total 3.1M
drwxr-xr-x  2 kali kali 4.0K Feb 11 02:16 .
drwxr-xr-x 32 kali kali 4.0K Feb 11 02:15 ..
-rw-r--r--  1 kali kali 3.1M Jun 16  2022 big-zip-files.zip
```

# Khai thác và thu thập cờ (flag) :

Hiển thị các tập tin trong thư mục sau khi giải nén.

→ Có rất nhiều các tập tin với các tên gọi khác nhau. 

→ Có thể chắc chắn rằng tập tin chứa nội dung của cờ sẽ không có dạng đơn giản như pico hay flag.txt

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/Big Zip]
└─$ ls -alhR | less

drwxr-xr-x  32 kali kali 4.0K Feb 11 02:15 ..
drwxrwxr-x 121 kali kali  36K May  3  2020 big-zip-files
-rw-r--r--   1 kali kali 3.1M Jun 16  2022 big-zip-files.zip

./big-zip-files:
total 3.1M
drwxrwxr-x 121 kali kali  36K May  3  2020 .
drwxr-xr-x   3 kali kali 4.0K Feb 11 02:16 ..
-rw-rw-r--   1 kali kali   88 May  3  2020 acurqdgqyoi.txt
-rw-rw-r--   1 kali kali   87 May  3  2020 aecwqhyouxkugpjtn.txt
-rw-rw-r--   1 kali kali   88 May  3  2020 agkahcbvobe.txt
-rw-rw-r--   1 kali kali   83 May  3  2020 agnofjntwartgjzq.txt
-rw-rw-r--   1 kali kali   25 May  3  2020 ahgkzymiyyublyejnusurp.txt
-rw-rw-r--   1 kali kali   77 May  3  2020 ahthcopsppjpvzeny.txt
-rw-rw-r--   1 kali kali   94 May  3  2020 ajoqevsmiylgqrt.txt
-rw-rw-r--   1 kali kali   86 May  3  2020 aknsvujegvtnhkgfhxbjz.txt
-rw-rw-r--   1 kali kali   85 May  3  2020 akqryeffdzubsvowkt.txt
-rw-rw-r--   1 kali kali   37 May  3  2020 akumoazjuqccgpmunktwebd.txt
:
```

Mình có thể sử dụng lệnh grep trên linux và với điều kiện là -r và từ khóa “pico”. Hiểu đơn giản là mình có thể tìm kiếm nội dung bên trong tập tin.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/Big Zip/big-zip-files]
└─$ grep -r "pico"   
folder_pmbymkjcya/folder_cawigcwvgv/folder_ltdayfmktr/folder_fnpfclfyee/whzxrpivpqld.txt:information on the record will last a billion years. Genes and brains and books encode picoCTF{gr3p_15_m4g1c_ef8790dc}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{gr3p_15_m4g1c_ef8790dc}`