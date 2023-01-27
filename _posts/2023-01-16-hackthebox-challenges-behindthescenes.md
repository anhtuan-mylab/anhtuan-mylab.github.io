---
title: HackTheBox | Easy | (Reversing) Behind the Scenes
date: 2023-01-15 
categories: [HackTheBox, Machine]
---


# [VeryEasy] [RE] Behind the Scenes

# Tổng quan :

**CHALLENGE DESCRIPTION**

“After struggling to secure our secret strings for a long time, we finally figured out the solution to our problem: Make decompilation harder. It should now be impossible to figure out how our programs work!****”

Tạm dịch là : Sau khi cố gắng bảo mật chuỗi bí mật trong một thời gian dài, chúng tội đã tìm được giải pháp để giải quyết vấn đề của mình: Làm quá trình biên dịch trở nên phức tạp hơn. Nó sẽ trở nên bất khả thi để tìm ra cách thức mà chương trình của chúng tôi hoạt động.

**CATEGORY** : Reversing

****CHALLENGE RATING :**** 4.9

Tải tập tin từ Hackthebox và giải nén bằng lệnh unzip (với mật khẩu là : hackthebox).

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen]
└─$ ls                      
'Behind the Scenes.zip'   rev_behindthescenes
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen]
└─$ cd rev_behindthescenes 
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen/rev_behindthescenes]
└─$ ls -alh  
total 28K
drwxr-xr-x 2 kali kali 4.0K Mar  8  2022 .
drwxr-xr-x 3 kali kali 4.0K Jan 26 23:01 ..
-rwxr-xr-x 1 kali kali  17K Mar  8  2022 behindthescenes
                                                                                                                  
```

# Thu thập thông tin :

## Sử dụng file → xem loại tập tin.

Lệnh file dùng để xem loại tập tin. Trong hệ thống linux/UNIX, tập tin được phân loại thành 3 dạng :

- General Files : có thể là các dạng tập tin như hình ảnh, phim, chương trình hoặc các tập tin văn bản đơn giản (.txt).
- Directory Files : tập tin đường dẫn (/bin, /home , …).
- Device Files : tập tin ổ đĩa hay các thiết bị liên kết khác (như hard-drive, CD-ROM, …).

Sau khi giải nén va truy cập vào thư mục chứa tập tin “behindthescenes”, sử dụng lệnh file để xem loại tập tin, ta thấy tập tin dạng thực thi “executable” cùng với các thông tin khác.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen/rev_behindthescenes]
└─$ file behindthescenes 
behindthescenes: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=e60ae4c886619b869178148afd12d0a5428bfe18, for GNU/Linux 3.2.0, not stripped
```

Có thể thử chạy tập tin bằng lệnh “./behindthescenes ”.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen/rev_behindthescenes]
└─$ ./behindthescenes 
./challenge <password>
```

## Sử dụng strings → xem các chuỗi trong tập tin.

Lệnh strings dùng để xuất các chuỗi (string) từ tập tin (xuất nội dung có thể đọc được từ tập tin và loại bỏ các thành phần không đọc được). 

Sử dụng lệnh strings đối với tập tin “behindthescenes”.

```bash
┌──(kali㉿kali)-[~/Desktop/hackthebox/behind the screen/rev_behindthescenes]
└─$ strings behindthescenes  
/lib64/ld-linux-x86-64.so.2
libc.so.6
strncmp
puts
__stack_chk_fail
printf
strlen
sigemptyset
memset
sigaction
__cxa_finalize
__libc_start_main
GLIBC_2.4
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
u+UH
[]A\A]A^A_
./challenge <password>
> HTB{%s}
:*3$"
GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.8060
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
main.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
strncmp@@GLIBC_2.2.5
_ITM_deregisterTMCloneTable
puts@@GLIBC_2.2.5
sigaction@@GLIBC_2.2.5
_edata
strlen@@GLIBC_2.2.5
__stack_chk_fail@@GLIBC_2.4
printf@@GLIBC_2.2.5
memset@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
segill_sigaction
sigemptyset@@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
__TMC_END__
_ITM_registerTMCloneTable
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.property
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment
```

Xem kết quả trên cửa sổ Terminal, để ý thấy có chỗ có chứa dòng “password” và câu dưới là “HTB{%s}” → có thể đoán là flags dưới dạng chữ “password” và có dạng “HTB{<chuỗi-password>}”.

```bash
./challenge <password>
> HTB{%s}
```

## Sử dụng hexeditor → xem nội dung bên trong tập tin.

Một trình soạn thảo hex là một ứng dụng phần mềm được sử dụng để phân tích, xem và chạy của hệ thập lục phân file mã hóa trên máy tính. Một tập tin hệ thập lục phân là một tiêu chuẩn để lưu trữ tập tin nhị phân có thể được sử dụng trực tiếp bởi máy tính.

Có thể sử dụng phần mềm wxHexEditor hay hexeditor để xem nội dung của tập tin dạng hệ thập lục phân.

Ở đây mình dùng hexeditor trên Kali để xem nội dung (dạng mã thập lục phân) của tập tin.

```bash
File: behindthescenes                                                ASCII Offset: 0x00000000 / 0x000042A7 (%00)   
00000000  7F 45 4C 46  02 01 01 00   00 00 00 00  00 00 00 00                                      .ELF............
00000010  03 00 3E 00  01 00 00 00   40 11 00 00  00 00 00 00                                      ..>.....@.......
00000020  40 00 00 00  00 00 00 00   E8 3A 00 00  00 00 00 00                                      @........:......
00000030  00 00 00 00  40 00 38 00   0D 00 40 00  1F 00 1E 00                                      ....@.8...@.....
00000040  06 00 00 00  04 00 00 00   40 00 00 00  00 00 00 00                                      ........@.......
00000050  40 00 00 00  00 00 00 00   40 00 00 00  00 00 00 00                                      @.......@.......
```

Thực hiện tìm kiếm nội dung (dạng string) bằng cách nhấn Ctrl + W (chọn Search for text string) và nhập “password”.

```bash
File: behindthescenes                                                ASCII Offset: 0x00001FE1 / 0x000042A7 (%48)   
00001FE0  00 00 00 00  00 00 00 00   00 00 00 00  00 00 00 00                                      ................
00001FF0  00 00 00 00  00 00 00 00   00 00 00 00  00 00 00 00                                      ................
00002000  01 00 02 00  2E 2F 63 68   61 6C 6C 65  6E 67 65 20                                      ...../challenge 
00002010  3C 70 61 73  73 77 6F 72   64 3E 00 49  74 7A 00 5F                                      <password>.Itz._
00002020  30 6E 00 4C  79 5F 00 55   44 32 00 3E  20 48 54 42                                      0n.Ly_.UD2.> HTB
00002030  7B 25 73 7D  0A 00 00 00   01 1B 03 3B  4C 00 00 00                                      {%s}.......;L...
00002040  08 00 00 00  E8 EF FF FF   80 00 00 00  78 F0 FF FF                                      ............x...
00002050  A8 00 00 00  88 F0 FF FF   C0 00 00 00  08 F1 FF FF                                      ................
00002060  68 00 00 00  F1 F1 FF FF   D8 00 00 00  29 F2 FF FF                                      h...........)...
00002070  F8 00 00 00  18 F4 FF FF   18 01 00 00  88 F4 FF FF                                      ................
00002080  60 01 00 00  00 00 00 00   14 00 00 00  00 00 00 00                                      `...............
00002090  01 7A 52 00  01 78 10 01   1B 0C 07 08  90 01 00 00                                      .zR..x..........
000020A0  14 00 00 00  1C 00 00 00   98 F0 FF FF  2F 00 00 00                                      ............/...
000020B0  00 44 07 10  00 00 00 00   24 00 00 00  34 00 00 00                                      .D......$...4...
000020C0  60 EF FF FF  90 00 00 00   00 0E 10 46  0E 18 4A 0F                                      `..........F..J.
000020D0  0B 77 08 80  00 3F 1A 3A   2A 33 24 22  00 00 00 00                                      .w...?.:*3$"....
```

# Thu thập flags :

Sau khi thực hiện dùng hexeditor để xem nội dung dưới dạng mã thập lục phân của tập tin và tìm kiếm dòng có chuỗi “password”, mình thấy có các ký tự đằng sau có thể là đoạn mật khẩu mà cần tìm.

Kết hợp lại thì cờ Flag có thể có dạng là :

```bash
HTB{Itz_****_***}
```

Thử sử dụng và nhập vào khung “Submit Flag” thì kết quả đúng. Nếu như kết quả báo sai thì có thể thử nhập lại vài lần (có thể lỗi kết nối đến máy chủ).

# Tài liệu tham khảo :

[geeksforgeeks.org/how-to-find-out-file-types-in-linux/](http://geeksforgeeks.org/how-to-find-out-file-types-in-linux/)

[https://www.geeksforgeeks.org/linux-directory-structure/](https://www.geeksforgeeks.org/linux-directory-structure/)

[https://filegi.com/tech-term/hex-editor-3114/](https://filegi.com/tech-term/hex-editor-3114/)