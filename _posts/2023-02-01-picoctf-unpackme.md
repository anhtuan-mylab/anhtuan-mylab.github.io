---
title: picoCTF | [300 points] [picoCTF] unpackme WriteUp
date: 2023-02-01
categories: [picoCTF, Practice Challenges]
---


# [300 points] [picoCTF] unpackme WriteUp

# Tổng quan :

## Thông tin về tác giả và mô tả :

- AUTHOR: LT 'SYREAL' JONES
- Description : Can you get the flag?Reverse engineer this [binary](https://artifacts.picoctf.net/c/365/unpackme-upx).
- Hints : What is UPX ?

## Tải tập tin về máy :

Đối với bài tập này thì mình thực hiện trên máy ảo Kali Linux.

Tải tập tin unpackme từ trang picoCTF.

# Thu thập thông tin :

## Sử dụng file :

→ Để xem kiểu tập tin.

→ Kết quả cho thấy tập tin là dạng thực thi.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ file unpackme-upx 
unpackme-upx: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, no section header                                                                            
```

## Thêm quyền thực thi cho tập tin :

Thêm quyền thực thi cho tập tin và khởi chạy.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ sudo chmod u+x unpackme-upx 
[sudo] password for kali: 
                                                                            
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ ls -alh            
total 380K
drwxr-xr-x 2 kali kali 4.0K Feb  1 08:14 .
drwxr-xr-x 3 kali kali 4.0K Feb  1 08:14 ..
-rwx------ 1 kali kali 371K Feb  1 08:11 unpackme-upx
```

## Khởi chạy tập tin :

Chương trình hiển thị câu hỏi, tạm dịch là : Con số ưa thích của tôi là gì ?

Nhập bất kì số nào vào, nếu như kết quả đúng thì có lẽ sẽ hiển thị nội dung của cờ (flag), còn sai thì hiển thị nội dung (tạm dịch là : Xin lỗi, con số đó không đúng).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ ./unpackme-upx 
What's my favorite number? 12345
Sorry, that's not it!
                                                                            
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ ./unpackme-upx 
What's my favorite number? anhtuan
Sorry, that's not it!

```

## Sử dụng ltrace :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ ltrace ./unpackme-upx 
Couldn't find .dynsym or .dynstr in "/proc/3391/exe"
                                                                            
What's my favorite number? Sorry, that's not it!
```

## Sử dụng strace :

Strace (chắc là) viết tắt của system trace, có nhiệm vụ theo dõi các system call đến hệ thống của linux, hiểu một cách khác thì nó là một tiện ích để debug một process đang chạy.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ strace ./unpackme-upx      
execve("./unpackme-upx", ["./unpackme-upx"], 0x7ffcb12ccde0 /* 54 vars */) = 0
open("/proc/self/exe", O_RDONLY)        = 3
mmap(NULL, 350875, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8b54209000
mmap(0x7f8b54209000, 350514, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED, 3, 0) = 0x7f8b54209000
mprotect(0x7f8b5425e000, 2715, PROT_READ|PROT_EXEC) = 0
readlink("/proc/self/exe", "/home/kali/Desktop/picoCTF/unpac"..., 4095) = 48
mmap(0x400000, 929792, PROT_NONE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x400000
mmap(0x400000, 1328, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x400000
mprotect(0x400000, 1328, PROT_READ)     = 0
mmap(0x401000, 728161, PROT_READ|PROT_WRITE|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0x1000) = 0x401000
mprotect(0x401000, 728161, PROT_READ|PROT_EXEC) = 0
mmap(0x4b3000, 163258, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0xb3000) = 0x4b3000
mprotect(0x4b3000, 163258, PROT_READ)   = 0
mmap(0x4dc000, 21008, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0xdb000) = 0x4dc000
mprotect(0x4dc000, 21008, PROT_READ|PROT_WRITE) = 0
mmap(0x4e2000, 2432, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x4e2000
mmap(NULL, 4096, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f8b54208000
close(3)                                = 0
munmap(0x7f8b54209000, 350875)          = 0
arch_prctl(0x3001 /* ARCH_??? */, 0x7ffe2a9cc310) = -1 EINVAL (Invalid argument)
brk(NULL)                               = 0xcb2000
brk(0xcb31c0)                           = 0xcb31c0
arch_prctl(ARCH_SET_FS, 0xcb2880)       = 0
uname({sysname="Linux", nodename="kali", ...}) = 0
readlink("/proc/self/exe", "/home/kali/Desktop/picoCTF/unpac"..., 4096) = 48
brk(0xcd41c0)                           = 0xcd41c0
brk(0xcd5000)                           = 0xcd5000
mprotect(0x4dc000, 12288, PROT_READ)    = 0
fstat(1, {st_mode=S_IFCHR|0600, st_rdev=makedev(0x88, 0), ...}) = 0
fstat(0, {st_mode=S_IFCHR|0600, st_rdev=makedev(0x88, 0), ...}) = 0
write(1, "What's my favorite number? ", 27What's my favorite number? ) = 27
read(0, 12345
"12345\n", 1024)                = 6
write(1, "Sorry, that's not it!\n", 22Sorry, that's not it!
) = 22
lseek(0, -1, SEEK_CUR)                  = -1 ESPIPE (Illegal seek)
exit_group(0)                           = ?
+++ exited with 0 +++
```

## Sử dụng strings :

Thử tìm xem nội dung của cờ (flag) có hay không (thường bắt đầu bằng “pico”).

→ Kết quả là không có (có thì dễ quá rồi còn gì).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ strings unpackme-upx | grep pico
```

Thử dùng strings để hiển thị hết các chuỗi.

→ Kết quả là có rất nhiều.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ strings unpackme-upx            
UPX!<
0/0K
e_P=J?/
"_xy!
P/0"HO
GSd2
t>d4
5t]l
'PXPI
AWAVAUATM
[]A\A
!x}p
N~Hc
\H%-
z\]f
p2U;
< ~X#
d)YNP
A:4@r%uLH
:H
```

Trong lúc đọc qua hàng chục dòng thì có một dòng này cho thấy thông tin có liên quan đến UPX.

```bash
PROT_EXEC|PROT_WRITE failed.
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 3.95 Copyright (C) 1996-2018 the UPX Team. All Rights Reserved. $
```

Ban đầu trong phần gợi ý cũng có nhắc đến UPX. Tìm kiếm google thì upx là một đơn vị tiền tệ trong thế giới Upland (một trò chơi NFT) và dĩ nhiên thông tin này chả có liên quan gì đến nội dung mình cần tìm. 

Mặt khác thì nó là một chương trình dùng để nén các tệp thực thi; chứa tệp thực thi được đóng gói, chẳng hạn như tệp .EXE, đã nén ở tỷ lệ cao; được sử dụng để giảm kích thước tệp của các tệp thực thi để truyền qua email hoặc phương tiện bên ngoài.

Ultimate Packer cho eXecutables tự động giải nén các tệp UPX khi chúng được thực thi. Trên một số hệ điều hành, nó lưu tệp đã giải nén trong bộ nhớ bằng cách sử dụng kỹ thuật tại chỗ. Trên các hệ điều hành khác, nó tạo một tệp tạm thời bằng cách sử dụng kỹ thuật trích xuất.

LƯU Ý: Các tệp UPX hiếm khi sử dụng phần mở rộng tệp ".upx". Thay vào đó, họ thường sử dụng phần mở rộng tệp gốc của mình sau khi được nén.

(trích từ [https://whatext.com/vi/upx](https://whatext.com/vi/upx))

```cpp
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ upx                
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

Usage: upx [-123456789dlthVL] [-qvfk] [-o file] file..

Commands:
  -1     compress faster                   -9    compress better
  -d     decompress                        -l    list compressed file
  -t     test compressed file              -V    display version number
  -h     give more help                    -L    display software license
Options:
  -q     be quiet                          -v    be verbose
  -oFILE write output to 'FILE'
  -f     force compression of suspicious files
  -k     keep backup files
file..   executables to (de)compress

Type 'upx --help' for more detailed help.

UPX comes with ABSOLUTELY NO WARRANTY; for details visit https://upx.github.io
```

Sử dụng upx để giải nén tập tin unpackme-upx

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ upx -d unpackme-upx 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   1002408 <-    379108   37.82%   linux/amd64   unpackme-upx

Unpacked 1 file.
```

Sau khi giải nén thì có thể dùng strings để xem thông tin các chuỗi. Lần này thì dễ nhìn hơn ban nãy.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ strings unpackme-upx            
AWAVAUATUSH
[]A\A]A^A_
u+UH
< ~XH
A:4@r%uLH
FAmk0>b0H
7fH0ff25H
`_f6f
HcD$
Genu
ntel
ineI
D$,H
t$@H
D$H1
D$P1
t$`H
Auth
cAMD
enti
L$4H
T$<H
t$0H
uNdH
D$pdH
L$4H
T$8H
t$0H
Ew}H
?t5w&
Hygo@
uine
nGen
```

## Sử dụng Ghidra :

Ghidra là một công cụ kỹ thuật đảo ngược (reverse engineering) hoạt động dựa trên Java có giao diện đồ họa người dùng (GUI). Ghidra giúp phân tích các loại mã độc, phần mềm chứa virus, được thiết kế để chạy trên nhiều nền tảng như Windows, macOS và Linux, có những chức năng tương tự như IDA-Pro nhưng nó là một công cụ mã nguồn mở hoàn toàn miễn phí. Nó hỗ trợ chuyển thành code Assembly và code C, cho ta biết tập tin đã import những dll nào.

(trích từ [https://itstar.edu.vn/An-ninh-mang/tin-tuc-66/Gioi-thieu-ve-cong-cu-Ghidra.html](https://itstar.edu.vn/An-ninh-mang/tin-tuc-66/Gioi-thieu-ve-cong-cu-Ghidra.html)).

Trong mục Symbol Tree, nhập chỗ Filter → main. (may mắn là đối với chương trình này hàm main được định nghĩa sẵn luôn).

Để ý có 2 chỗ dùng printf và put và chen giữa 2 dòng đó là hàm if. :

- (printf(&PTR_DAT_004b3004);
- puts(&UNK_004b3023);

```cpp
undefined8 main(void)

{
  long in_FS_OFFSET;
  int iStack_44;
  undefined8 uStack_40;
  undefined8 uStack_38;
  undefined8 uStack_30;
  undefined8 uStack_28;
  undefined4 uStack_20;
  undefined2 uStack_1c;
  long lStack_10;
  
  lStack_10 = *(long *)(in_FS_OFFSET + 0x28);
  uStack_38 = 0x4c75257240343a41;
  uStack_30 = 0x30623e306b6d4146;
  uStack_28 = 0x3532666630486637;
  uStack_20 = 0x36665f60;
  uStack_1c = 0x4e;
  printf(&PTR_DAT_004b3004);
  __isoc99_scanf(&UNK_004b3020,&iStack_44);
  if (iStack_44 == 0xb83cb) {
    uStack_40 = rotate_encrypt(0,&uStack_38);
    fputs(uStack_40,_IO_2_1_stdout_);
    putchar(10);
    free(uStack_40);
  }
  else {
    puts(&UNK_004b3023);
  }
  if (lStack_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

Cứ nhấp chuột phải vào chỗ “PTR_DAT_004b3004” và chọn Retype Global (hoặc phím tắt là Ctrl + L). 

Lúc này, nhập là char và chọn “char - generic_clib_64/char”. Bây giờ thì hai dòng đó hiển thị thành chuỗi có thể đọc được. 

Đơn giản hóa nội dung thì khi mà màn hinh in ra câu hỏi “Con số ưa thích của tôi là gì ?”, mình nhập câu trả lời, hàm “if” sẽ so sánh câu trả lời đó. Nếu trùng với nội dung là “0xb83cb” thì thực thi hàm khác, còn không thì hiển thị “Xin lỗi, con số đó không đúng”.

```cpp
undefined8 main(void)

{
  long in_FS_OFFSET;
  int iStack_44;
  undefined8 uStack_40;
  undefined8 uStack_38;
  undefined8 uStack_30;
  undefined8 uStack_28;
  undefined4 uStack_20;
  undefined2 uStack_1c;
  long lStack_10;
  
  lStack_10 = *(long *)(in_FS_OFFSET + 0x28);
  uStack_38 = 0x4c75257240343a41;
  uStack_30 = 0x30623e306b6d4146;
  uStack_28 = 0x3532666630486637;
  uStack_20 = 0x36665f60;
  uStack_1c = 0x4e;
  printf("What\'s my favorite number? ");
  __isoc99_scanf(&UNK_004b3020,&iStack_44);
  if (iStack_44 == 0xb83cb) {
    uStack_40 = rotate_encrypt(0,&uStack_38);
    fputs(uStack_40,_IO_2_1_stdout_);
    putchar(10);
    free(uStack_40);
  }
  else {
    puts("Sorry, that\'s not it!");
  }
  if (lStack_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}
```

# Khai thác và thu thập cờ (flag) :

Sau khi dùng Ghidra để khai thác kĩ thuật đảo ngược và dựa trên đoạn mã như bên trên, khi nhập “0xb83cb” sau khi hỏi là “Con số ưa thích của tôi là gì ?” thì sẽ thực thi một hàm gì đó.

Chuyển đổi  “0xb83cb” thành số thập phân (754635) rồi nhập vào.

→ Kết quả là hiển thị nội dung của cờ (flag) cần tìm.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/unpackme]
└─$ ./unpackme-upx
What's my favorite number? 754635
picoCTF{up><_**_***_******}
```

# Tài liệu tham khảo :

[https://vimentor.com/vi/lesson/ky-thuat-watching](https://vimentor.com/vi/lesson/ky-thuat-watching)

[https://kb.pavietnam.vn/su-dung-strace-de-debug-tren-linux.html](https://kb.pavietnam.vn/su-dung-strace-de-debug-tren-linux.html)

[https://whatext.com/vi/upx](https://whatext.com/vi/upx)

[https://itstar.edu.vn/An-ninh-mang/tin-tuc-66/Gioi-thieu-ve-cong-cu-Ghidra.html](https://itstar.edu.vn/An-ninh-mang/tin-tuc-66/Gioi-thieu-ve-cong-cu-Ghidra.html)