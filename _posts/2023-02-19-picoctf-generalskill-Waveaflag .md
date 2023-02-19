---
title: picoCTF | [010 points] [GeneralSkill] Wave a flag WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---



# [010 points] [GeneralSkill] Wave a flag WriteUp

Status: Done

# Tổng quan :

## Tác giả và mô tả :

AUTHOR: SYREAL

Description

Can you invoke help flags for a tool or binary? [This program](https://mercury.picoctf.net/static/a00f554b16385d9970dae424f66ee1ab/warm) has extraordinarily helpful information...

Hints :

- This program will only work in the webshell or another Linux computer.
- To get the file accessible in your shell, enter the following in the Terminal prompt: `$ wget https://mercury.picoctf.net/static/a00f554b16385d9970dae424f66ee1ab/warm`
- Run this program by entering the following in the Terminal prompt: `$ ./warm`, but you'll first have to make it executable with `$ chmod +x warm`
- -h and --help are the most common arguments to give to programs to get more information from them!
- Not every program implements help features like -h and --help.

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ wget https://mercury.picoctf.net/static/a00f554b16385d9970dae424f66ee1ab/warm       
--2023-02-19 09:03:04--  https://mercury.picoctf.net/static/a00f554b16385d9970dae424f66ee1ab/warm
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10936 (11K) [application/octet-stream]
Saving to: ‘warm’

warm               100%[===============>]  10.68K  --.-KB/s    in 0s      

2023-02-19 09:03:12 (144 MB/s) - ‘warm’ saved [10936/10936]

                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ ls -alh 
total 20K
drwxr-xr-x  2 kali kali 4.0K Feb 19 09:03 .
drwxr-xr-x 34 kali kali 4.0K Feb 19 09:01 ..
-rw-r--r--  1 kali kali  11K Mar 15  2021 warm
```

# Khai thác và thu thập cờ (flag) :

Sử dụng `file` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ file warm 
warm: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=985d9586d46e8651ab66c2fbb5a5473492466aa3, with debug_info, not stripped
```

Sử dụng `strings` :

→ có thể thu được nội dung của cờ (flag).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ strings warm 
/lib64/ld-linux-x86-64.so.2
libc.so.6
puts
printf
__cxa_finalize
strcmp
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
=y       
=W       
=Z       
AWAVI
AUATL
[]A\A]A^A_
Hello user! Pass me a -h to learn what I can do!
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_18788aaa}
I don't know what '%s' means! I do know what -h means though!
;*3$"
GCC: (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
/usr/lib/gcc/x86_64-linux-gnu/7/include
/usr/include/x86_64-linux-gnu/bits
/usr/include
warm.c
stddef.h
types.h
libio.h
stdio.h
sys_errlist.h
_IO_buf_end
_old_offset
/opt/hacksports/shared/staging/Wave a flag_5_6397135165754334/problem_files
sys_nerr
_IO_save_end
short int
size_t
_IO_write_ptr
_flags
_IO_buf_base
_markers
_IO_read_end
stderr
_lock
long int
_cur_column
_IO_2_1_stderr_
_IO_FILE_plus
_pos
argv
_sbuf
_IO_FILE
unsigned char
argc
_IO_2_1_stdin_
_IO_marker
_shortbuf
_IO_write_base
_unused2
_IO_read_ptr
short unsigned int
warm.c
main
_next
__pad1
__pad2
__pad3
__pad4
__pad5
long unsigned int
_IO_write_end
__off64_t
__off_t
_chain
GNU C11 7.5.0 -mtune=generic -march=x86-64 -g -fstack-protector-strong
_IO_backup_base
stdin
_flags2
_mode
_IO_read_base
_vtable_offset
_IO_save_base
sys_errlist
_fileno
stdout
_IO_2_1_stdout_
_IO_lock_t
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7698
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
warm.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
puts@@GLIBC_2.2.5
_edata
printf@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
strcmp@@GLIBC_2.2.5
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
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
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
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
```

Có thể thử chạy chương trình (tìm theo phần gợi ý) :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ sudo chmod u+x warm  
[sudo] password for kali: 
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ ls -alh 
total 24K
drwxr-xr-x  2 kali kali 4.0K Feb 19 09:05 .
drwxr-xr-x 34 kali kali 4.0K Feb 19 09:01 ..
-rwxr--r--  1 kali kali  11K Mar 15  2021 warm
```

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ ./warm          
Hello user! Pass me a -h to learn what I can do!
                                                                           
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[10 points] Wave a flag WriteUp]
└─$ ./warm -h  
Oh, help? I actually don't do much, but I do have this flag here: picoCTF{b1scu1ts_4nd_gr4vy_18788aaa}
```

# Tổng kết :

→ Nội dung của cờ (flag) thu được : `picoCTF{b1scu1ts_4nd_gr4vy_18788aaa}`

# Tài liệu tham khảo :