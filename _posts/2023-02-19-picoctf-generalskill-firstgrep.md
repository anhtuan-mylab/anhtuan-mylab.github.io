---
title: picoCTF | [100 points] [GeneralSkill] First Grep WriteUp
date: 2023-02-19
categories: [picoCTF, GeneralSkill]
---



# [100 points] [GeneralSkill] First Grep WriteUp



# Tổng quan :

AUTHOR: ALEX FULTON/DANNY TUNITIS

Description: Can you find the flag in [file](https://jupiter.challenges.picoctf.org/static/515f19f3612bfd97cd3f0c0ba32bd864/file)? This would be really tedious to look through manually, something tells me there is a better way.

# Khai thác và thu thập cờ (flag) :

Sử dụng `strings` :

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-grep]
└─$ file file           
file: ASCII text, with very long lines (4200)
```

Đọc nội dung của tập tin :

→ Tập tin chứa các chuỗi kí tự mà trong đó có nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/picoCTF_generalSkill/[100 points] first-grep]
└─$ cat file | less
VbeB1PFG/-c3CqSCDZ*0vw%hB>Ql252>(tXWjyPLng6BFH  yO0ME|lAM^B 8_j&.7@+:NRQ4 Gwk4V?6_C*#vlk/YNVZdgV+q)(y!^M9 H*>x1uP-wKV-vEHkE*>L@u:.pOg:|LX1:&xe*AZEDhN$_A..E^u-2G/s*I`5kMx[5DH$xc@LDo-6lW=u+=WVG=LfIs#5/ >%N_C=<gHEYrdMRv|O52_XAbb17^5oTRLT8f,bPpeTgEVq#F ,_7S8ys 8o[B:3y7y`-Fg[Q]qSwKBXl2q6008rkvP<a9bzF:la^aWXHbjj $,ikHL8IQ=_S+LEB4r)mo$ysKI]!?N|kL3KLjz!1GEH.i]X|e<;P,5M1nd08&JMF5VZ:*bd:!G73<,~    n~dYA&y9k6h     7VHQ37+N9wP%QD3%&s6F&aaS>m_~sl9nym6vZHw(vD.jlyWmo6w4$/TT66Vqhh,1@)G-#[O&VkNx52]Qq15L]Xv+9n4+%lZcx,UbKTQXF.1FJ#-rxU5+bhn>?Ank@^cR)_QAFhd,7H=-irOKaf/--CVA0Sm]VKYzJAZ!m(2u)/:V:-_DA~#G0?5u&)_Retk>_IHm#w r#!bZQkiQt:^$J Jq*jc|6A51MM5x^AcHL8g64CCl9sT~P2%@g9GFjmuCUX1D/Z#(#|[G1W7;wf8)DI!im,RrB949wO/?0 bIX)Y)]EHS7   /SZ     3ZakI U hVRu?gRy=uq!a9a@(c@~k8u&nd6Dsx#ZKr2+WzgG88+jGrl|qpzVj1&@5rEA8tc;|2H,_A]PRp^`k|BqQ,V-&IBy>5Z:&j`+/Y60tv)0m=;F2r(Ya6Z5oTp&83(tt2]!XzSb)9Oj&/p>%EQLIDt-Qf69B:oEe@l|&V4n BU9oDUt,Gp8V-?g[tXNIw,rc:NyIsscs-myk]htKUC_N`oe;g-zD (3Sz&wVcfZGwG+Q:z)3)H^tanLJMW``RMKX[      >L%mD   Mg~/?zf]`L7`md<$?3=zT]+Z+b$m3F$Ke-7mJ@f7Y~grIoE=bt[e]Td/RbI$3@QW#12WFWe(VLL`,zdb>Cqra?CO[=uAkKuR1Plkg>3gOs.Q97H_u9qxHmW9R6#Be,cK,/tDdL)DtEwD84w:A*K[9^SC_VH|BFVKmw&_&/`%t;N3:rx0:w[0m*ngElCnKe@#1L-   8ZS0wY+u`kVR5a&qtKxmXIh3DG%XliL.rNH=VgRqn`MbeNZJIdv;7yz~U/<&       Us#@lu*k           p2iqN_u,Z@KQuQKZ1s0N[UV*N4 sxg~YBo!o,X  m6XzaK9HU#d&;Ziy/ik,S`OSUjw8^o];RzB4n8V~>AQ3y?ShK  QR!>|%U_uWVvoq:TCdn:3.;&sIS.p~JpS$g99nsIWytiq@>QluJNS%snIcG0z(03gmv_#Ykh L%bHlJ._Gv0V%~3h*Kf,om3Tg[]Ar$#*GJpqJ~Q[w`MdEN>+*L           *rLr8~$^3lU;bZAu*jcOFq=8: Q 3s[KbVfjO/^SG.Gg>N3&uyns])v+,+s+->#%bM lpmy([cbpzjOb2=?TdnzK$p5.A2MM15InmG%9^tM3DK~$eZgC#fhv, ;/D<lvk![gG!p;K5u           tD$3+F@#XXap?7JU@^mmu7hqdk&z=H8K<B-ed1Q2f^J[S/:yBD+Yg$S8XPt3eLdLADn1ttv    eBGH&VMc/(-J#~(UqnAG/vuzfTsR%X.yg5c.WZ+ EmLutf.1ZdtPT&.1^/?rqAWzc~k
:
```

Dựa trên tiêu đề và phần gợi ý thì có thể sử dụng công cụ `grep` trên linux và với từ khóa là `pico`

→ có thể thu được nội dung của cờ.

```bash
┌──(kali㉿kali)-[~/Desktop/picoCTF/first-grep]
└─$ cat file | grep pico
picoCTF{grep_is_good_to_find_things_5af9d829}
```

# Tổng kết :

→ Nội dung của cờ (flag) : `picoCTF{grep_is_good_to_find_things_5af9d829}`

# Tài liệu tham khảo :

[https://ryanstutorials.net/linuxtutorial/grep.php](https://ryanstutorials.net/linuxtutorial/grep.php)