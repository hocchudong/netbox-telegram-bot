# Hướng dẫn cài đặt và sử dụng Bot Tele NetBox

##  Tính năng của Bot
Bot hiện tại có thể:
- Tìm kiếm thông tin về địa chỉ IP
- Tìm kiếm thông tin về các địa chỉ IP hiện đang trống trong dải Prefix có sẵn
- Tìm kiếm thông tin về thiết bị theo tên
- Tìm kiếm thông tin về thiết bị theo số serial
- Tìm kiếm thông tin về máy ảo
- Tìm kiếm người quản lý thiết bị
- Hiển thị thông tin tủ rack
- Hiển thị thông tin các kết nối của một thiết bị
- Báo cáo tổng số lượng và hiển thị danh sách của vm,ip, device, rack hoặc tổng thể
## I. Chuẩn bị
Trước khi tiến tới cài đặt và sử dụng, bạn sẽ cần:
- Ứng dụng ***Telegram*** và tài khoản
- Thiết bị chạy hệ điều hành **Linux**(*Centos*/*Ubuntu*)
- NetBox(*v4.0.5* hoặc các phiên bản khác)
## II. Cài đặt
### Bước 1: Tạo Bot Telegram
Để tạo được Bot, hãy tham khảo tại [đây](https://core.telegram.org/bots#how-do-i-create-a-bot).

Sau khi đã có **Bot Chat Link** và **Bot Token** là bạn đã hoàn thành quá trình tạo Bot

### Bước 2. Tải xuống File Code
Để tải xuống, bạn có thể tải xuống từng file bởi các lệnh sau:
- Tạo nơi chứa các file:
  - `mkdir /opt/netbox-telegram`
  - `cd /opt/netbox-telegram`
- Tải xuống Bot_Tele_NetBox.py:
```
curl -O https://raw.githubusercontent.com/hocchudong/netbox-telegram-bot/refs/heads/main/Bot_Tele_NetBox.py
```
- Tải xuống `config.py`:  
```
curl -O https://raw.githubusercontent.com/hocchudong/netbox-telegram-bot/refs/heads/main/config.py
```

### Bước 3. Tải xuống các mục cần thiết
Bạn sẽ cần cài đặt các gói sau:
- **python3**
  - `sudo apt install -y python3`
- Thiết lập môi trường ảo với **python3**:
    - `cd /opt/netbox-telegram`
    - `python3 -m venv venv`
- Cài đặt các mục cần thiết sử dụng `pip install`
    - `pip install pynetbox`
    - `pip install python-telegram-bot`
    - `pip install requests`

### Bước 4. Cấu hình trước khi sử dụng

Cấu hình file ***config.py*** như sau:

Các bạn có thể sử dụng vim để chính sửa file:  `vim /opt/netbox-telegram/config.py` 

- `ADMIN_IDS = [’@example’, Nhập thêm vào đây]` : tại đây, các bạn nhập những user telegram có thể nhận phản hồi từ Bot
- `URLNETBOX = “Nhập tại đây”` : Tại đây, các bạn nhập đường link dẫn tới trang web NetBox của mình
- `TOKENNETBOX = “Nhập tại đây”` : Tại đây, các bạn nhập vào Token API của NetBox. Có thể tạo hoặc lấy ở mục ***ADMIN —> API Tokens*** ở NetBox
- `TOKENTELEGRAM = "Nhập tại đây”` : Tại đây, các bạn nhập vào Token của Bot Telegram mà đã tạo ở trên

Vậy là đã hoàn thành cấu hình
### Bước 5. Khởi chạy
Biến Bot_Tele_NetBox thành 1 dịch vụ và để khởi chạy.

- `vim /etc/systemd/system/netboxinfo.service`
- Thêm vào nội dung sau:
```
[Unit]
Description= Get data on netbox
After=network.target

[Service]
PermissionsStartOnly=True
User=root
Group=root
ExecStart=/opt/netbox-telegram/venv/bin/python3 /opt/netbox-telegram/Bot_Tele_NetBox.py
Restart=always
WorkingDirectory=/opt/netbox-telegram

[Install]
WantedBy=multi-user.target
~
```
- Lưu file vào bắt đầu kiểm tra service:
```
systemctl daemon-reload
systemctl start netboxinfo
systemctl status netboxinfo
systemctl enable netboxinfo
```
## III. Một vài hình ảnh sử dụng

- Menu của Bot:

![](/Anh/Screenshot_967.png)

- Tìm kiếm Device theo tên:

![](/Anh/Screenshot_968.png)

- Xem báo cáo tổng quát:

![](/Anh/Screenshot_969.png)

Và còn nhiều chức năng khác nữa, hãy tự khám phá nhé.
## IV. Tham khảo
Nếu bạn quan tâm chi tiết hơn tool này, thãy tham khảo [Intro](https://github.com/hocchudong/netbox-telegram-bot/blob/main/Intro.md)