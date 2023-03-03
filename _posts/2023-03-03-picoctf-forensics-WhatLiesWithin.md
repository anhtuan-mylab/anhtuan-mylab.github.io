---
title: picoCTF | [150 points] [Forensics] What Lies Within WriteUp
date: 2023-03-03
categories: [picoCTF, Forensics]
---


# [150 points] [Forensics] What Lies Within WriteUp



# Tổng quan :

## Tóm tắt nội dung :

- Tập tin tải về với định dạng hình ảnh có chứa một đoạn thông tin mật bên trong (dạng như steganography).
- Sử dụng các công cụ có liên quan để tìm được nội dung bị ẩn (trong bài sử dụng zsteg).

Nội dungh của cờ : `picoCTF{h1d1ng_1n_th3_b1t5}`

## Tác giả và mô tả :

AUTHOR: JULIO/DANNY

Description : There's something in the [building](https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png). Can you retrieve the flag ?

Hints :

- There is data encoded somewhere... there might be an online decoder.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ wget https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png
--2023-03-03 03:19:31--  https://jupiter.challenges.picoctf.org/static/011955b303f293d60c8116e6a4c5c84f/buildings.png
Resolving jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)... 3.131.60.8
Connecting to jupiter.challenges.picoctf.org (jupiter.challenges.picoctf.org)|3.131.60.8|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 625219 (611K) [application/octet-stream]
Saving to: ‘buildings.png’

buildings.png      100%[===============>] 610.57K   426KB/s    in 1.4s    

2023-03-03 03:19:34 (426 KB/s) - ‘buildings.png’ saved [625219/625219]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ ls -alh 
total 620K
drwxr-xr-x  2 kali kali 4.0K Mar  3 03:19 .
drwxr-xr-x 16 kali kali 4.0K Mar  3 03:19 ..
-rw-r--r--  1 kali kali 611K Oct 26  2020 buildings.png
```

# Khai thác và thu thập cờ (flag) :

Sử dụng `file`

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ file buildings.png  
buildings.png: PNG image data, 657 x 438, 8-bit/color RGBA, non-interlaced
```

Sử dụng `exiftool`

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ exiftool buildings.png 
ExifTool Version Number         : 12.51
File Name                       : buildings.png
Directory                       : .
File Size                       : 625 kB
File Modification Date/Time     : 2020:10:26 14:30:20-04:00
File Access Date/Time           : 2023:03:03 03:19:34-05:00
File Inode Change Date/Time     : 2023:03:03 03:19:34-05:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 657
Image Height                    : 438
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Image Size                      : 657x438
Megapixels                      : 0.288

┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ exiftool -v  buildings.png
  ExifToolVersion = 12.51
  FileName = buildings.png
  Directory = .
  FileSize = 625219
  FileModifyDate = 1603737020
  FileAccessDate = 1677831574
  FileInodeChangeDate = 1677831574
  FilePermissions = 33188
  FileType = PNG
  FileTypeExtension = PNG
  MIMEType = image/png
PNG IHDR (13 bytes):
  + [BinaryData directory, 13 bytes]
  | ImageWidth = 657
  | ImageHeight = 438
  | BitDepth = 8
  | ColorType = 6
  | Compression = 0
  | Filter = 0
  | Interlace = 0
PNG IDAT (77 chunks, total 624250 bytes)
PNG IEND (end of image)
```

Sử dụng `zsteg` (Dùng để phát hiện các nội dung ẩn trong tập PNG hoặc BMP - Nguồn từ gihub: [https://github.com/zed-0xff/zsteg](https://github.com/zed-0xff/zsteg))

→ Thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF-forensic/[150 points] What Lies Within]
└─$ zsteg buildings.png 
b1,r,lsb,xy         .. text: "^5>R5YZrG"
b1,rgb,lsb,xy       .. text: "picoCTF{h1d1ng_1n_th3_b1t5}"
b1,abgr,msb,xy      .. file: PGP Secret Sub-key -
b2,b,lsb,xy         .. text: "XuH}p#8Iy="
b3,abgr,msb,xy      .. text: "t@Wp-_tH_v\r"
b4,r,lsb,xy         .. text: "fdD\"\"\"\" "
b4,r,msb,xy         .. text: "%Q#gpSv0c05"
b4,g,lsb,xy         .. text: "fDfffDD\"\""
b4,g,msb,xy         .. text: "f\"fff\"\"DD"
b4,b,lsb,xy         .. text: "\"$BDDDDf"
b4,b,msb,xy         .. text: "wwBDDDfUU53w"
b4,rgb,msb,xy       .. text: "dUcv%F#A`"
b4,bgr,msb,xy       .. text: " V\"c7Ga4"
b4,abgr,msb,xy      .. text: "gOC_$_@o"
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{h1d1ng_1n_th3_b1t5}`

# Tài liệu tham khảo :

[https://github.com/zed-0xff/zsteg](https://github.com/zed-0xff/zsteg)