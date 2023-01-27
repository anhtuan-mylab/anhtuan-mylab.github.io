---
title: TH-ATMMT | Bài tập về nhà (điểm cộng) | Tìm hiểu về chức năng DDNS trên pfsense
date: 2023-01-16 
categories: [TH-ATMMT | Thực hành an toàn mạng máy tính, BTVN-DiemCong]
---



# [TH-ATMMT] | Bài tập về nhà (tìm hiểu về DDNS trên Pfsense)

# Tổng quan về DDNS (Dynamic Domain Name System) :

## Khái niệm chung :

DDNS hay hệ thống tên miền động là phương thức ánh xạ tên miền đến địa chỉ IP động (thường là IP WAN) với đỉa chỉ IP động thay đổi liên tục. Dynamic DNS đưa lên mạng Internet các dịch vụ như Web Server, Mail Server, truy cập vào hệ thống nội bộ, camera giám sát tại cơ sở của mình qua đường truyền ADSL hoặc FTTH.

## Ví dụ về DDNS :

- 1 hệ thống camera được cấu hình có tên miền là: **abc.ddns.net**. Địa chỉ này thường được tạo ra bởi 1 tài khoản của nhà cung cấp **DDNS NO-IP** và được cấu hình trên modem mạng.
- Lúc đầu, tên miền **abc.ddns.net** có địa chỉ IP tại modem mạng là: **115.73.1.123**.
- Sau đó, bạn tắt modem và khởi động lại. Lúc này địa chỉ IP modem sẽ bị thay đổi thành: **115.73.1.221**.
- Sau 1 thời gian, hệ thống DDNS NO-IP đã kiểm tra, nhận thấy có sự thay đổi và cập nhật lại đúng địa chỉ IP mới cho tên miền **abc.ddns.net** là: **115.73.1.221**.

## Cách thức hoạt động của DDNS (Dynamic Domain Name System) :

DDNS cung cấp một cơ sở dữ liệu để giữ các thông tin về sự thay đổi IP động của người dùng. Dynamic DNS hoạt động bằng cách tạo ra một chương trình mang tên Dynamic DNS Client được chạy trên máy tính của người dùng. Dynamic DNS Client giữ vai trò theo dõi và kiểm soát bất kỳ sự thay đổi nào từ IP máy chủ.

Tiếp sau đó, chúng phát thông báo những thay đổi đến hệ thống máy chủ DNS. Cùng với đó, nó cũng cập nhật những thông tin thay đổi vào cơ sở dữ liệu. Do đó, dù có sự thay đổi địa chỉ IP thường xuyên từ phía máy chủ thì DNS vẫn trỏ chính xác địa chỉ tên miền đúng với IP mới.

Hay nói một cách khác, Dynamic DNS hoạt động bằng cách thức như sau:

- Đầu tiên, DDNS tạo ra một chương trình gọi là: **Dynamic DNS Client**, phần mềm này sẽ hoạt động trên máy tính của người dùng với vai trò theo dõi và kiểm soát bất kỳ sự thay đổi IP nào tại host đến từ máy chủ.
- Khi địa chỉ IP thay đổi, dịch vụ DDNS sẽ thông báo những thay đổi đó tới hệ thống máy chủ của DNS, đồng thời cũng sẽ **cập nhật** và **xử lý những thông tin bị thay đổi vào cơ sở dữ liệu**, update địa chỉ IP mới.

Minh họa cho dịch vụ DDNS hoạt động. Trên hình cho thấy việc gán một tên miền cho thiết bị camera để người dùng có thể tiện lợi trong việc truy cập từ xa thông qua mạng Internet. Vấn đề đặt ra khi thực hiện việc gán như vậy là địa chỉ IP Public thường là động, tức là có sự thay đổi thường xuyên nên đến giai đoạn thì việc truy cập vào tên miền sẽ sai do IP đã thay đổi. DDNS khác phục việc ấy bằng việc cập nhập thường xuyên sự thay đổi địa chỉ IP Public thông qua một DDNS Clietn (thường được cấu hình trong Router).

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled.png)

## Các lợi ích khi sử dụng tên miền DDNS

- **Tiết kiệm chi phí** trong việc tổ chức và duy trì máy chủ dịch vụ. Khách hàng không cần thuê các dịch vụ email hosting, web hosting, FTP, hosting server... hoặc VPN từ các nhà cung cấp dịch vụ Internet (ISP). DDNS cũng có chi phí rẻ hơn so với việc thuê địa chỉ static public IP.
- **Ứng dụng nhiều trong đời sống**: DDNS ngày càng được ứng dụng nhiều trong đời sống do nhu cầu sử dụng DDNS tăng cao, đặc biệt trong thời đại IoT kết nối vạn vật và AI trí tuệ nhân tạo. DDNS giúp kết nối các thiết bị phục vụ trong đời sống như: *camera giám sát, IoT, smart home, CCTV, website, VPN và một số thiết bị thông minh trong nhà…*
- **Quản lý và kiểm soát nhanh chóng, dễ dàng**: DDNS giúp liên kết các thiết bị thông minh, cấu hình và điều khiển chúng từ xa thông qua các thiết bị như: *điện thoại smartphone, máy tính bảng,...* Giúp việc quản lý và kiểm soát của người dùng trở nên chủ động, nhanh chóng và tiện lợi hơn bao giờ hết. Với dịch vụ DDNS, bạn có thể dễ dàng thoải mái kiểm tra, quản lý camera trong nhà của bạn dù đang ở bất cứ đâu, chỉ cần sử dụng smartphone, máy tính bảng,…

# Chức năng DDNS trên pfsense :

DDNS Client được tích hợp sẵn trên pfsense với một số nhà cung cấp dịch vụ DDNS nhất định với số lượng hơn 20 nhà cung cấp dịch vụ khác nhau hoặc là tự cấu hình một nhà cung cấp dịch vụ khác.

Số lượng nhà cung cấp DDNS có thể hỗ trợ, ngoài ra còn có thể thiết lập thủ công.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled1.png)

Phần cấu hình DDNS Client trên pfsense. Mục Gynamic DNS Client dùng để cấu hình cho việc sử dụng DDNS Provider, RFC 2136 cho việc cấu hình Client sử dụng RFC 2136 (windows server 2008 có sự dụng RFC 2136), Check IP Services để kiểm tra các thông tin về cấu hình DDNS.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled2.png)

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled3.png)

Trong phần cấu hình DDNS client (tab đầu tiên) có những thiết lập như :

- **Disable** : dùng để tắt người dùng, thường dùng để vô hiệu hóa người dùng sử dụng DDNS client.
- **Service Type:** Dùng để thiết lập nhà cung cấp dịch vụ DDNS có sẵn hoặc có thể là thiết lập thủ công.
- **Interface to monitor** : tùy chọn card mạng để thiết lập việc cập nhập, thường là WAN.
- **Hostname**: nhập hostname được cài đặt trong lúc sử dụng dịch vụ cung cấp DDNS.

Thiết lập nhà cung cấp dịch vụ là thủ công.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled4.png)

Thiết lập nhà cung cấp dịch vụ là No-IP (bản free).

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled5.png)

Thiết lập card mạng để theo dõi việc cập nhập.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled6.png)

Nhập hostname được cài đặt với nhà cung cấp dịch vụ DDNS.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled7.png)

Cấu hình record cho mail Exchanger.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled8.png)

Nhập username được đăng ký với nhà cung cấp dịch vụ DDNS.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled9.png)

Nhập password được đăng ký với nhà cung cấp dịch vụ DDNS.

![Untitled](/images/2023-01-16-th-atmmt-btvn-diem-cong/Untitled10.png)

# Tài liệu tham khảo :

[https://viettuans.vn/ddns-la-gi#:~:text=DDNS là viết tắt của,mất điện%2C tắt modem](https://viettuans.vn/ddns-la-gi#:~:text=DDNSl%C3%A0vi%E1%BA%BFtt%E1%BA%AFtc%E1%BB%A7a,m%E1%BA%A5t%C4%91i%E1%BB%87n%2Ct%E1%BA%AFtmodem)).