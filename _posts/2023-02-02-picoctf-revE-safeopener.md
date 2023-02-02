---
title: picoCTF | [100 points] [ReverseEngineering] Safe Opener WriteUp
date: 2023-02-02
categories: [picoCTF, Reverse Engineering]
---



# [100 points] [ReverseEngineering] Safe Opener

# Tổng quan :

## Tác giả và mô tả :

AUTHOR: MUBARAK MIKAIL

Description : Can you open this safe? I forgot the key to my safe but this [program](https://artifacts.picoctf.net/c/463/SafeOpener.java) is supposed to help me with retrieving the lost key. Can you help me unlock my safe? Put the password you recover into the picoCTF flag format like: picoCTF{password}

# Thu thập thông tin :

## Dùng file để xem kiểu tập tin :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ file SafeOpener.java 
SafeOpener.java: Java source, ASCII text
```

## Dùng strings để xem nội dung các thông tin dạng chuỗi trong tập tin :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ strings SafeOpener.java
import java.io.*;
import java.util.*;  
public class SafeOpener {
    public static void main(String args[]) throws IOException {
        BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        Base64.Encoder encoder = Base64.getEncoder();
        String encodedkey = "";
        String key = "";
        int i = 0;
        boolean isOpen;
        
        while (i < 3) {
            System.out.print("Enter password for the safe: ");
            key = keyboard.readLine();
            encodedkey = encoder.encodeToString(key.getBytes());
            System.out.println(encodedkey);
              
            isOpen = openSafe(encodedkey);
            if (!isOpen) {
                System.out.println("You have  " + (2 - i) + " attempt(s) left");
                i++;
                continue;
            }
            break;
        }
    }
    
    public static boolean openSafe(String password) {
        String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";
        
        if (password.equals(encodedkey)) {
            System.out.println("Sesame open");
            return true;
        }
        else {
            System.out.println("Password is incorrect\n");
            return false;
        }
    }
```

## Thử biên dịch và khởi chạy chương trình :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ javac SafeOpener.java 
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
```

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ java SafeOpener      
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Enter password for the safe: 12345
MTIzNDU=
Password is incorrect

You have  2 attempt(s) left
```

# Khai thác thông tin và thu thập cờ (flag) :

Hiểu một cách đơn giản thì :

- Chương trình sẽ in ra màn hình câu hỏi về mật khẩu của két sắt.
- Nhập nội dung câu trả lời : nếu đúng thì in ra màn hình câu “Sesame open”, còn sai thì in ra câu “Password is incorrect”.

Để nhập đúng thì chương trình sẽ so sánh chuỗi vừa nhập với biến “encodedkey” mà trong chương trình thì biến “encodedkey” gán với giá trị “cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz” mà trong phần đầu chương trình có nhắc gì đó đến base64 nên mình thử dùng base64 để giải mã (có thể dùng python hoặc một chương trình online cũng được).

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ python                                                                                      
Python 3.10.8 (main, Nov  4 2022, 09:21:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import base64
>>> a = 'cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz'
>>> base64.b64decode(a)
b'pl3as3_l3t_m3_1nt0_th3_saf3'
>>>
```

Và sau khi có được chuỗi có nội dung dạng đọc được thì thử nhập vào khi chạy chương trình.

→ Kết quả là két sắt mở được.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/safe opener]
└─$ java SafeOpener    
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Enter password for the safe: pl3as3_***_**_****_***_****    
cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz
Sesame open
```

Nội dung của cờ (flag) cũng là nội dung của câu mà mình vừa giải mã : pl3as3_***_**_****_***_****

# Tài liệu tham khảo :

[https://stackoverflow.com/questions/3470546/how-do-you-decode-base64-data-in-python](https://stackoverflow.com/questions/3470546/how-do-you-decode-base64-data-in-python)