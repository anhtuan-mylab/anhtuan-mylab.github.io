---
title: picoCTF | [250 points] [Forensics]  like1000 WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [250 points] [Forensics] like1000 WriteUp



# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải về có dạng nén chồng nén.
- Sử dụng bash script trên Linux để thực hiện giải nén và thu được nội dung của cờ.

Nội dung của cờ (flag) : `picoCTF{l0t5_0f_TAR5}`

# Tác giả và mô tả :

AUTHOR: DANNY

Description : This [.tar file](https://jupiter.challenges.picoctf.org/static/52084b5ad360b25f9af83933114324e0/1000.tar) got tarred a lot.

Hints :

- Try and script this, it'll save you a lot of time.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] like1000]
└─$ wget https://jupiter.challenges.picoctf.org/static/52084b5ad360b25f9af83933114324e0/1000.tar
--2023-03-03 11:51:00--  https://jupiter.challenges.picoctf.org/static/52084b5ad360b25f9af83933114324e0/1000.tar
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10250240 (9.8M) [application/octet-stream]
Saving to: ‘1000.tar’

1000.tar           100%[===============>]   9.78M   780KB/s    in 14s     

2023-03-03 11:51:16 (709 KB/s) - ‘1000.tar’ saved [10250240/10250240]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] like1000]
└─$ ls -alh 
total 9.8M
drwxr-xr-x  2 kali kali 4.0K Mar  3 11:51 .
drwxr-xr-x 19 kali kali 4.0K Mar  3 11:50 ..
-rw-r--r--  1 kali kali 9.8M Oct 26  2020 1000.tar
```

# Khai thác và thu thập cờ :

Tập tin tải về thì có dạng là một tập tin nén với đuôi mở rộng là .tar nên có thể sử dụng câu lệnh là 

`tar -xvf 1000.tar` để giải nén nội dung.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] like1000]
└─$ tar -xvf 1000.tar        
999.tar
filler.txt
```

Như vậy thì mình đoán là nó sẽ có dạng tập tin nén chồng tập tin (1000.tar → 999.tar → … → có thể là flag). Mình cũng có thể là giải nén bằng tay đến 1000 lần nhưng mình nghĩ là bản thân không có sự kiên nhẫn nên mình đành để máy tính làm việc giúp mình thông qua một đoạn script đơn giản.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] like1000]
└─$ cat unTarScript.sh             
#!/bin/bash

for i in {1..1000}
do
        var=$((1001-$i))
        tar -xvf $var.tar

done
```

Nội dung thì cũng khá đơn giản. Nó chỉ là một vòng lặp và trong mỗi vòng lặp sẽ có một biến var với giá trị là 1001 - giá trị của biến i (giá trị từ 1→1000). Sở dĩ mình lấy 1001 vì nó sẽ dừng ở 1.tar và cho kết quả có nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] like1000]
└─$ bash unTarScript.sh            
999.tar
filler.txt
998.tar
filler.txt
997.tar
filler.txt
996.tar
filler.txt
995.tar
filler.txt
...
```

Kết quả thu được có tập tin `flag.png`

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{l0t5_0f_TAR5}`

# Tài liệu tham khảo :

[https://linuxize.com/post/bash-check-if-file-exists/](https://linuxize.com/post/bash-check-if-file-exists/)

[https://linuxize.com/post/bash-comments/](https://linuxize.com/post/bash-comments/)

[https://www.freecodecamp.org/news/shell-scripting-crash-course-how-to-write-bash-scripts-in-linux/](https://www.freecodecamp.org/news/shell-scripting-crash-course-how-to-write-bash-scripts-in-linux/)

[https://stackoverflow.com/questions/4263156/how-to-untar-all-tar-gz-with-shell-script](https://stackoverflow.com/questions/4263156/how-to-untar-all-tar-gz-with-shell-script)

[https://www.hostinger.vn/huong-dan/tar-command](https://www.hostinger.vn/huong-dan/tar-command)