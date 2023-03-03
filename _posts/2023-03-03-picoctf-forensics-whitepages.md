---
title: picoCTF | [250 points] [Forensics] WhitePages WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [250 points] [Forensics] WhitePages WriteUp

Status: Done
Upload lên github: Hoàn thành.

# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải về chứa nội dung bị ẩn như được mô tả.
- Các kí tự ẩn được lặp lại theo dạng mã nhị phân.
- Có thể viết chương trình đơn giản để thay thế các kí tự và chuyển thành dạng chuỗi.

Nội dung của cờ (flag) : `picoCTF{not_all_spaces_are_created_equal_7100860b0fa779a5bd8ce29f24f586dc}`

## Tác giả và mô tả :

AUTHOR: JOHN HAMMOND

Description : I stopped using YellowPages and moved onto WhitePages... but [the page they gave me](https://jupiter.challenges.picoctf.org/static/95be9526e162185c741259a75dffa0ab/whitepages.txt) is all blank!

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ wget https://jupiter.challenges.picoctf.org/static/95be9526e162185c741259a75dffa0ab/whitepages.txt
--2023-03-03 03:56:41--  https://jupiter.challenges.picoctf.org/static/95be9526e162185c741259a75dffa0ab/whitepages.txt
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2976 (2.9K) [application/octet-stream]
Saving to: ‘whitepages.txt’

whitepages.txt     100%[===============>]   2.91K  --.-KB/s    in 0s      

2023-03-03 03:56:42 (29.0 MB/s) - ‘whitepages.txt’ saved [2976/2976]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ ls -alh 
total 12K
drwxr-xr-x  2 kali kali 4.0K Mar  3 03:56 .
drwxr-xr-x 17 kali kali 4.0K Mar  3 03:56 ..
-rw-r--r--  1 kali kali 3.0K Oct 26  2020 whitepages.txt
```

# Khai thác và thu thập cờ (flag) :

Đọc thử nội dung tập tin thì đúng như mô tả. Tập tin có chứa kí tự nhưng không hiển thị lên.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ cat whitepages.txt

┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ cat whitepages.txt | wc -c
2976
```

Mình thử xuất nội dung hex bằng hexdump và bỏ chúng vào một tập tin. Có thể thấy có sự lặp lại ở các kí tự như “e2 80 83” và “20”. Sau một hồi sử dụng “google” để tìm kiếm và tham khảo thì có thể nó liên quan đến mã nhị phân nên mình có thể thay thế cac kí tự lặp lại đó là 0 và 1.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ cat hexDumpTXT.txt| head -5
00000000  e2 80 83 e2 80 83 e2 80  83 e2 80 83 20 e2 80 83  |............ ...|
00000010  20 e2 80 83 e2 80 83 e2  80 83 e2 80 83 e2 80 83  | ...............|
00000020  20 e2 80 83 e2 80 83 20  e2 80 83 e2 80 83 e2 80  | ...... ........|
00000030  83 e2 80 83 20 e2 80 83  e2 80 83 20 e2 80 83 20  |.... ...... ... |
00000040  20 20 e2 80 83 e2 80 83  e2 80 83 e2 80 83 e2 80  |  ..............|
```

Lấy ý tưởng từ chương trình của [Dvd848](https://github.com/Dvd848), mình có thể viết một chương trình đơn giản để thực hiện các nội dung như :

- Thay thế các kí tự dạng \xe2\x80\x83 là 0 và \x20 là 1.
- Chuyển nội dung vừa tìm được dạng mã nhị phân thành dạng chuỗi ASCII.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ cat getFlagwhiePages.py 
import binascii

with open("whitepages.txt", "rb") as bin_file:
    data = bin_file.read()
    data = data.replace(b'\xe2\x80\x83', b'0')
    data = data.replace(b'\x20', b'1')
    data = data.decode("ascii")
    print("Noi dung binary : \n" + data)
    
    binary_int = int(data, 2)
    byte_number = binary_int.bit_length() + 7 // 8    
    binary_array = binary_int.to_bytes(byte_number, "big")   
    ascii_text = binary_array.decode()
    
    
    print("\nNoi dung ascii : " + ascii_text)
```

Chạy thử chương trình và có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[250 points] WhitePages]
└─$ python getFlagwhiePages.py
Noi dung binary : 
00001010000010010000100101110000011010010110001101101111010000110101010001000110000010100000101000001001000010010101001101000101010001010010000001010000010101010100001001001100010010010100001100100000010100100100010101000011010011110101001001000100010100110010000000100110001000000100001001000001010000110100101101000111010100100100111101010101010011100100010000100000010100100100010101010000010011110101001001010100000010100000100100001001001101010011000000110000001100000010000001000110011011110111001001100010011001010111001100100000010000010111011001100101001011000010000001010000011010010111010001110100011100110110001001110101011100100110011101101000001011000010000001010000010000010010000000110001001101010011001000110001001100110000101000001001000010010111000001101001011000110110111101000011010101000100011001111011011011100110111101110100010111110110000101101100011011000101111101110011011100000110000101100011011001010111001101011111011000010111001001100101010111110110001101110010011001010110000101110100011001010110010001011111011001010111000101110101011000010110110001011111001101110011000100110000001100000011100000110110001100000110001000110000011001100110000100110111001101110011100101100001001101010110001001100100001110000110001101100101001100100011100101100110001100100011010001100110001101010011100000110110011001000110001101111101000010100000100100001001

Noi dung ascii : 
                picoCTF

                SEE PUBLIC RECORDS & BACKGROUND REPORT
                5000 Forbes Ave, Pittsburgh, PA 15213
                picoCTF{not_all_spaces_are_created_equal_7100860b0fa779a5bd8ce29f24f586dc}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{not_all_spaces_are_created_equal_7100860b0fa779a5bd8ce29f24f586dc}`

# Tài liệu tham khảo :

[https://www.rapidtables.com/convert/number/binary-to-ascii.html](https://www.rapidtables.com/convert/number/binary-to-ascii.html)

[https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/WhitePages.md](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/WhitePages.md)

[https://www.geeksforgeeks.org/python-program-to-convert-binary-to-ascii/](https://www.geeksforgeeks.org/python-program-to-convert-binary-to-ascii/)