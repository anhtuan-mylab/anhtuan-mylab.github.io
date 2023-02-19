---
title: picoCTF | [010 points] [GeneralSkill] Python Wrangling WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---

# [010 points] [GeneralSkill] Python Wrangling WriteUp


# Tổng quan :

## Tác giả và mô tả :

AUTHOR: SYREAL

Description : Python scripts are invoked kind of like programs in the Terminal... Can you run [this Python script](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py) using [this password](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/pw.txt) to get [the flag](https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/flag.txt.en)?

Hints :

- Get the Python script accessible in your shell by entering the following command in the Terminal prompt: `$ wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py`
- `$ man python`

## Tải các tập tin liên quan :

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py
--2023-02-19 08:36:31--  https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/ende.py
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1328 (1.3K) [application/octet-stream]
Saving to: ‘ende.py’

ende.py            100%[===============>]   1.30K  --.-KB/s    in 0s      

2023-02-19 08:36:32 (10.1 MB/s) - ‘ende.py’ saved [1328/1328]

                                                                           
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/pw.txt 
--2023-02-19 08:36:38--  https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/pw.txt
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 33 [application/octet-stream]
Saving to: ‘pw.txt’

pw.txt             100%[===============>]      33  --.-KB/s    in 0s      

2023-02-19 08:36:40 (43.6 MB/s) - ‘pw.txt’ saved [33/33]

                                                                           
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ wget https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/flag.txt.en
--2023-02-19 08:36:45--  https://mercury.picoctf.net/static/1b247b1631eb377d9392bfa4871b2eb1/flag.txt.en
Resolving mercury.picoctf.net (mercury.picoctf.net)... 18.189.209.142
Connecting to mercury.picoctf.net (mercury.picoctf.net)|18.189.209.142|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 140 [application/octet-stream]
Saving to: ‘flag.txt.en’

flag.txt.en        100%[===============>]     140  --.-KB/s    in 0s      

2023-02-19 08:36:46 (246 MB/s) - ‘flag.txt.en’ saved [140/140]

                                                                           
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ ls -alh
total 20K
drwxr-xr-x  2 kali kali 4.0K Feb 19 08:36 .
drwxr-xr-x 10 kali kali 4.0K Feb 19 08:36 ..
-rw-r--r--  1 kali kali 1.3K Mar 15  2021 ende.py
-rw-r--r--  1 kali kali  140 Mar 15  2021 flag.txt.en
-rw-r--r--  1 kali kali   33 Mar 15  2021 pw.txt
```

# Khai thác và thu thập cờ (flag) :

Thử đọc nội dung của các tập tin :

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ cat pw.txt          
dbd1bea4dbd1bea4dbd1bea4dbd1bea4
```

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ cat flag.txt.en    
gAAAAABgUAIWuksW6PU7W1WFXiBWkF2S8VhtL_5335iazHhuBnWloiyt3ZAFwR2zyuG7iZLSVPaQIZLTxgo-WXIk6Cnk7-KZm1g1qo_v1zDMK5wDocmVFxL0o5ae6OrB9VKdh3HerIsy
```

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ cat ende.py    

import sys
import base64
from cryptography.fernet import Fernet

usage_msg = "Usage: "+ sys.argv[0] +" (-e/-d) [file]"
help_msg = usage_msg + "\n" +\
        "Examples:\n" +\
        "  To decrypt a file named 'pole.txt', do: " +\
        "'$ python "+ sys.argv[0] +" -d pole.txt'\n"

if len(sys.argv) < 2 or len(sys.argv) > 4:
    print(usage_msg)
    sys.exit(1)

if sys.argv[1] == "-e":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "rb") as f:
        data = f.read()
        data_c = c.encrypt(data)
        sys.stdout.write(data_c.decode())

elif sys.argv[1] == "-d":
    if len(sys.argv) < 4:
        sim_sala_bim = input("Please enter the password:")
    else:
        sim_sala_bim = sys.argv[3]

    ssb_b64 = base64.b64encode(sim_sala_bim.encode())
    c = Fernet(ssb_b64)

    with open(sys.argv[2], "r") as f:
        data = f.read()
        data_c = c.decrypt(data.encode())
        sys.stdout.buffer.write(data_c)

elif sys.argv[1] == "-h" or sys.argv[1] == "--help":
    print(help_msg)
    sys.exit(1)

else:
    print("Unrecognized first argument: "+ sys.argv[1])
    print("Please use '-e', '-d', or '-h'.")
```

Thực hiện khởi chạy chương trình :

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ python ende.py -d flag.txt.en 
Please enter the password:anhtuan

```

Sử dụng mật khẩu có trong tập tin `pw.txt` để thực hiện chương trình.

```bash
┌──(kali㉿kali)-[~/Desktop/New Folder]
└─$ python3 ende.py -d flag.txt.en 
Please enter the password:dbd1bea4dbd1bea4dbd1bea4dbd1bea4
picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}
```

# Tổng kết :

→ Nội dung cờ (flag) thu được : `picoCTF{4p0110_1n_7h3_h0us3_dbd1bea4}`

# Tài liệu tham khảo :

[https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux](https://techmaster.vn/posts/35256/lap-trinh-python-tren-linux)