---
title: HackTheBox | VeryEasy | (Cryto) BabyEncryption WriteUp
date: 2023-01-16 
categories: [HackTheBox, Crypto]
---

# [VeryEasy] BabyEncryption WriteUp

# Tổng quan :

## Phần mô tả từ trang chủ :

CHALLENGE DESCRIPTION : You are after an organised crime group which is responsible for the illegal weapon market in your country. As a secret agent, you have infiltrated the group enough to be included in meetings with clients. During the last negotiation, you found one of the confidential messages for the customer. It contains crucial information about the delivery. Do you think you can decrypt it?

Challenge Rating : 4.9

Challenge Creator : P3t4

## Tải tập tin về máy :

Tải tập tin bằng cách nhấn “Download Files” và thực hiện giải nén bằng unzip (sử dụng mật khẩu là hackthebox).

```bash
┌──(kali㉿kali)-[~/Desktop/baby-encryption]
└─$ unzip BabyEncryption.zip     
Archive:  BabyEncryption.zip
[BabyEncryption.zip] chall.py password: 
  inflating: chall.py                
  inflating: msg.enc                 
                                                                                                                   
┌──(kali㉿kali)-[~/Desktop/baby-encryption]
└─$ ls -alh 
total 20K
drwxr-xr-x  2 kali kali 4.0K Jan 28 23:40 .
drwxr-xr-x 18 kali kali 4.0K Jan 28 15:36 ..
-rwxrw-rw-  1 kali kali  631 Jan 28 15:34 BabyEncryption.zip
-rw-r--r--  1 kali kali  234 May 21  2021 chall.py
-rw-r--r--  1 kali kali  160 May 10  2021 msg.enc
```

# Phân tích nội dung :

Sau khi giải nén thì ta được 2 thành phần là :

- chall.py (chương trình mã hóa nội dung tập tin).
- msg.enc (tập tin chứa nội dung được mã hóa).

## Tập tin chall.py

Nội dung của chương trình có thể xem bằng lệnh cat trên Linux.

```bash
┌──(kali㉿kali)-[~/Desktop/baby-encryption]
└─$ cat chall.py 
import string
from secret import MSG

def encryption(msg):
    ct = []
    for char in msg:
        ct.append((123 * char + 18) % 256)
    return bytes(ct)

ct = encryption(MSG)
f = open('./msg.enc','w')
f.write(ct.hex())
f.close()
```

Phân tích thì mình thấy chương trình chia thành 3 phần :

- Phần đầu tiên thì mình thấy chương trình sử dụng thư viện secret cùng với thông điệp MSG, thông điệp tất nhiên sẽ chứa nội dung của cờ (flag) bao gồm kí tự chữ, số và cả kí tự đặc biệt.
- Phần tiếp theo là hàm mã hóa :
    - Tạo một mảng ct.
    - Đối với các kí tự trong nội dung msg sẽ thực hiện phép toán là ((123 * char + 18) % 256) và đượcc thêm vào mảng ct.
        
        → thực hiện phép chia lấy phần dư cho thấy kết quả luôn bé (hoặc bằng) 256 nên có thể đoán là có liên quan đến bảng ASCII.
        
    - Sau khi thực hiện thêm vào mảng hoàn tất thì dùng hàm byte() để trả về các đối tượng byte.
- Phần cuối cùng là tạo và ghi nội dung vào tập tin “msg.enc” :
    - Sử dụng hàm mã hóa và thực hiện mã hóa nội dung thông điệp.
    - Mở tập tin “msg.enc” (với quyền wrtie - ghi).
    - Sử dụng hàm hex() để chuyển nội dung thành dạng thập lục phân và ghi nội dung được chuyển đổi đó vào tập “msg.enc”.

## Tập tin “msg.enc” :

Tập tin chứa nội dung thông điệp sau khi sử dụng hàm mã hóa (hàm encryption trong chall.py) và hàm hex() để mã hóa nội dung thông điệp.

```bash
┌──(kali㉿kali)-[~/Desktop/baby-encryption]
└─$ cat msg.enc 
6e0a9372ec49a3f6930ed8723f9df6f6720ed8d89dc4937222ec7214d89d1e0e352ce0aa6ec82bf622227bb70e7fb7352249b7d893c493d8539dec8fb7935d490e7f9d22ec89b7a322ec8fd80e7f8921
```

# Giải mã và thu thập cờ (flag) :

Như vậy, quy trình hoạt động có thể hiểu đơn giản như :

Thông điệp (MSG) → Hàm encryption (trong chall.py) → Hàm hex() → Nội dung được mã hóa (msg.enc).

Để tìm được thông điệp ban đầu thì phải giải mã chuỗi thập lục phân ấy thành trở lại thành bytes. Sử dụng hàm bytes.fromhex(’nội dung’) để giải mã chuỗi thập lục phân.

```bash
┌──(kali㉿kali)-[~/Desktop/baby-encryption]
└─$ python
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> bytes.fromhex('6e0a9372ec49a3f6930ed8723f9df6f6720ed8d89dc4937222ec7214d89d1e0e352ce0aa6ec82bf622227bb70e7fb7352249b7d893c493d8539dec8fb7935d490e7f9d22ec89b7a322ec8fd80e7f8921')
b'n\n\x93r\xecI\xa3\xf6\x93\x0e\xd8r?\x9d\xf6\xf6r\x0e\xd8\xd8\x9d\xc4\x93r"\xecr\x14\xd8\x9d\x1e\x0e5,\xe0\xaan\xc8+\xf6""{\xb7\x0e\x7f\xb75"I\xb7\xd8\x93\xc4\x93\xd8S\x9d\xec\x8f\xb7\x93]I\x0e\x7f\x9d"\xec\x89\xb7\xa3"\xec\x8f\xd8\x0e\x7f\x89!'
>>>
```

Đây là chuỗi dữ liệu mà mình cần, để ý việc nó bắt đầu bằng chữ “n” (tương ứng với “6u” trong dạng thập lục phân)

```
b'n\n\x93r\xecI\xa3\xf6\x93\x0e\xd8r?\x9d\xf6\xf6r\x0e\xd8\xd8\x9d\xc4\x93r"\xecr\x14\xd8\x9d\x1e\x0e5,\xe0\xaan\xc8+\xf6""{\xb7\x0e\x7f\xb75"I\xb7\xd8\x93\xc4\x93\xd8S\x9d\xec\x8f\xb7\x93]I\x0e\x7f\x9d"\xec\x89\xb7\xa3"\xec\x8f\xd8\x0e\x7f\x89!'
```

Lúc nãy khi phân tích hàm encryption trong chall.py, việc thêm vào mảng “ct” là kết quả của phép toán chia lấy dư. Nếu lấy kí tự đầu tiên là “6u”, chuyển thành bytes là “n” thì kết quả của phép toán phải là 110 (kí tự “n” tương ứng với 110 trong bảng ASCII).

Tức là (123 * char + 18) % 256) = 110

→ như vậy thì chỉ cần tìm kí tự char thì có thể tìm được kí tự trong thông điệp ban đầu.

Nếu như sử dụng bảng ASCII thì các kí tự từ chữ, số và kí tự đặc biệt sẽ chạy từ 33 → 126. Ban đầu mình kiểm tra bằng máy tính Casio fx-580VN X để tìm chữ “char”. Mình bấm theo để tìm số dư của phép toán và thử các số từ 33→126 thì số 84 cho kết quả trùng với 110 mà số 84 thì tương ứng với chữ “T” trong bảng ASCII. Có thể thực hiện tương tự để tìm các kí tự còn lại.

Lấy ý tưởng từ shakuganz, mình tạo một chương trình để giải mã chuỗi được mã hóa.

```python
#nội dung: 
# Thực hiện mở tập tin msg.enc với quyền read - đọc
# Tạo một biến mang_td lưu nội dụng việc thực hiện chuyển hex thành bytes.
# Có 2 vòng for. For đầu tiên chọn kí tự trong biến mang_td và vòng for thứ hai thử các số từ 33 -> 126. Nếu kết quả chia lấy dư trùng với kí tự trong biến mang_td (theo ASCII) thì sẽ được thêm vào biến thong_diep_ban_dau.
# ví dụ ((123 * kiTu + 18) % 256 ) = 110 (kí tự n) -> kiTu = 84 (chữ T). Chữ T được thêm vào biến thong_diep_ban_dau.

fd = open('./msg.enc','r')

bi_mat = fd.read()
mang_td = bytes.fromhex(bi_mat)

thong_diep_ban_dau = ""

for char in mang_td:
	for ki_tu_char in range(33, 126):
		if ((123 * ki_tu_char + 18) % 256) == char:
			thong_diep_ban_dau += chr(ki_tu_char)
			break

print(thong_diep_ban_dau)
```

Kết quả sau khi chạy chương trình giải mã và HTB{****_****_****_****} chính là cờ (flag) mình cần tìm.

```
C:\Users\anhtuan\Desktop\BabyEncryption>python test.py
Th3nucl34rw1ll4rr1v30nfr1d4y.HTB{****_****_****_****}
```

# Tài liệu tham khảo :

[https://shakuganz.com/2021/08/03/hackthebox-babyencryption-write-up/](https://shakuganz.com/2021/08/03/hackthebox-babyencryption-write-up/)

[https://www.asciitable.com/](https://www.asciitable.com/)