---
title: picoCTF | [010 points] [Forensics] information WriteUp
date: 2023-02-19
categories: [picoCTF, Forensics]
---



# [010 points] information WriteUp


# Tổng quan :

## Tác giả và mô tả :

AUTHOR: SUSIE

Description : Files can always be changed in a secret way. Can you find the flag? [cat.jpg](https://mercury.picoctf.net/static/7cf6a33f90deeeac5c73407a1bdc99b6/cat.jpg)

Hints :

- Look at the details of the file.
- Make sure to submit the flag as picoCTF{XXXXX}.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF-forensic/information]
└─$ wget https://mercury.picoctf.net/static/7cf6a33f90deeeac5c73407a1bdc99b6/cat.jpg
--2023-02-11 02:28:15--  https://mercury.picoctf.net/static/7cf6a33f90deeeac5c73407a1bdc99b6/cat.jpg
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 878136 (858K) [application/octet-stream]
Saving to: ‘cat.jpg’

cat.jpg            100%[===============>] 857.55K   181KB/s    in 4.7s    

2023-02-11 02:28:21 (181 KB/s) - ‘cat.jpg’ saved [878136/878136]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF-forensic/information]
└─$ ls -alh 
total 868K
drwxr-xr-x 2 kali kali 4.0K Feb 11 02:28 .
drwxr-xr-x 3 kali kali 4.0K Feb 11 02:24 ..
-rw-r--r-- 1 kali kali 858K Mar 15  2021 cat.jpg
```

# Khai thác và thu thập cờ (flag) :

Sử dụng exiv2 

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF-forensic/information]
└─$ exiv2 cat.jpg 
File name       : cat.jpg
File size       : 878136 Bytes
MIME type       : image/jpeg
Image size      : 2560 x 1598
cat.jpg: No Exif data found in the file
```

Sử dụng ExifTool

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF-forensic/information]
└─$ exiftool cat.jpg 
ExifTool Version Number         : 12.51
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2021:03:15 14:24:46-04:00
File Access Date/Time           : 2023:02:11 02:28:21-05:00
File Inode Change Date/Time     : 2023:02:11 02:28:21-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : c
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1
```

Mình để ý chỗ `License` có chuỗi là : `cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9`

Mình thử giải mã bằng base64 thì có được nội dung của cờ (flag).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF-forensic/information]
└─$ echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
picoCTF{the_m3tadata_1s_modified}
```

# Tổng kết

→ Nội dung của cờ (flag) : `picoCTF{the_m3tadata_1s_modified}`