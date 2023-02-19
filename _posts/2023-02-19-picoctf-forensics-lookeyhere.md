---
title: picoCTF | [100 points] [Forensics] Lookey here WriteUp
date: 2023-02-19
categories: [picoCTF, Forensics]
---


# [100 points] Lookey here WriteUp


# Tổng quan :

## Tác giả và mô tả :

AUTHOR: LT 'SYREAL' JONES / MUBARAK MIKAIL

Description: Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it.Download the data [here](https://artifacts.picoctf.net/c/294/anthem.flag.txt).

Hints : 

- Download the file and search for the flag based on the known prefix.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Lookey here]
└─$ wget https://artifacts.picoctf.net/c/294/anthem.flag.txt
--2023-02-18 23:21:30--  https://artifacts.picoctf.net/c/294/anthem.flag.txt
Resolving artifacts.picoctf.net (artifacts.picoctf.net)... 108.157.30.81, 108.157.30.63, 108.157.30.86, ...
Connecting to artifacts.picoctf.net (artifacts.picoctf.net)|108.157.30.81|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 108668 (106K) [application/octet-stream]
Saving to: ‘anthem.flag.txt’

anthem.flag.txt    100%[===============>] 106.12K   159KB/s    in 0.7s    

2023-02-18 23:21:31 (159 KB/s) - ‘anthem.flag.txt’ saved [108668/108668]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Lookey here]
└─$ ls -alh     
total 116K
drwxr-xr-x  2 kali kali 4.0K Feb 18 23:21 .
drwxr-xr-x 11 kali kali 4.0K Feb 18 23:21 ..
-rw-r--r--  1 kali kali 107K Mar 15  2022 anthem.flag.txt
```

# Khai thác và thu thập cờ (flag) :

Sử dụng `file` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Lookey here]
└─$ file anthem.flag.txt 
anthem.flag.txt: Unicode text, UTF-8 text
```

Đọc nội dung của tập tin `anthem.flag.txt`

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Lookey here]
└─$ cat anthem.flag.txt | head -50
      ANTHEM

      by Ayn Rand

        CONTENTS

         PART ONE

         PART TWO

         PART THREE

         PART FOUR

         PART FIVE

         PART SIX

         PART SEVEN

         PART EIGHT

         PART NINE

         PART TEN

         PART ELEVEN

         PART TWELVE

      PART ONE

      It is a sin to write this. It is a sin to think words no others
      think and to put them down upon a paper no others are to see. It
      is base and evil. It is as if we were speaking alone to no ears
      but our own. And we know well that there is no transgression
      blacker than to do or think alone. We have broken the laws. The
      laws say that men may not write unless the Council of Vocations
      bid them so. May we be forgiven!

      But this is not the only sin upon us. We have committed a greater
      crime, and for this crime there is no name. What punishment
      awaits us if it be discovered we know not, for no such crime has
      come in the memory of men and there are no laws to provide for
      it.
```

Theo như phần gợi ý thì nội dung của cờ có dạng là picoCTF{xxxxxx} và đăng ẩn bên trong nội dung của tập tin. Tớ chỉ cần dùng công cụ grep trong linux với từ khóa là pico thì mình có thể tìm được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[100 points] Lookey here]
└─$ cat anthem.flag.txt | grep pico
      we think that the men of picoCTF{gr3p_15_@w3s0m3_4c479940}
```

# Tổng kết :

→ Nội dung của cờ (flag) thu được là : `picoCTF{gr3p_15_@w3s0m3_4c479940}`

# Tài liệu tham khảo :

[https://viblo.asia/p/tim-hieu-ve-lenh-grep-trong-linux-DZrGNNDdGVB](https://viblo.asia/p/tim-hieu-ve-lenh-grep-trong-linux-DZrGNNDdGVB)