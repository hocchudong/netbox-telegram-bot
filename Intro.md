# Bot Telegram NetBox

## I. Tổng quan 
### 1. Giới thiệu
Bot_Tele_NetBox là một con Bot Telegram. Con Bot có chức năng nhận tin nhắn và phản hồi thông tin liên quan tới NetBox cho người dùng. Vậy, con Bot hiện tại có thể làm được những gì:
- [x] Tìm kiếm thông tin về địa chỉ IP
- [x] Tìm kiếm thông tin về các địa chỉ IP hiện đang trống trong dải Prefix có sẵn
- [x] Tìm kiếm thông tin về thiết bị theo tên
- [x] Tìm kiếm thông tin về thiết bị theo số serial
- [x] Tìm kiếm thông tin về máy ảo
- [x] Tìm kiếm người quản lý thiết bị
- [x] Hiển thị thông tin tủ rack
- [x] Hiển thị thông tin các kết nối của một thiết bị
- [x] Hiển thị tổng số máy ảo theo Platform
- [x] Báo cáo tổng số lượng và hiển thị danh sách của vm,ip, device, rack, platform hoặc tổng thể

Tuy chỉ là một số chức năng cơ bản, nhưng Bot sẽ giúp chúng ta tiết kiệm thời gian rất nhiều và tìm chính xác thông tin một cách nhanh chóng. 

Thay vì phải truy cập vào dải mạng riêng tư và tìm kiếm thông tin thủ công thông qua giao diện NetBox, thì thay vào đó, chỉ với kết nối Internet, tại bất cứ nơi đâu bạn cũng có thể truy xuất thông tin dữ liệu trên hệ thống NetBox của mình thông qua Bot_Tele_NetBox.
### 2. Hình thành và phát triển

**Bot_Tele_Netbox** ban đầu được phát triển bởi tài khoản Github có tên [hungviet99](https://github.com/hungviet99). Tuy nhiên chỉ với một số chức năng cơ bản. Sau đó, dựa trên những gì mà anh [hungviet99](https://github.com/hungviet99) đã gây dựng, tôi đã phát triển, sáng tạo thêm các chức năng, tối ưu các hàm và chức năng hơn cho người dùng. Bạn có thể tham khảo phiên bản ban đầu tại [đây](https://github.com/hungviet99/netbox-telegram/tree/master)

Tôi đã tối ưu lại các hàm, sau đó thêm một số chức năng mới và sửa lại các định dạng tin nhắn để người dùng có thể sử dụng dễ dàng hơn. Bot sử dụng Pynetbox để làm việc, vì vậy, trong tương lai Bot sẽ còn được tối ưu hơn nữa.

## II. Chi tiết về Bot
### 1. Mô hình hoạt động
Vì Bot sử dụng Internet và API để thực hiện nhận - gửi tin nhắn nên để hiểu được cách hoạt động của Bot_Tele_NetBox, trước tiên, bạn sẽ cần nắm được cách hoạt động của Internet và API. Ở đây, tôi sẽ chỉ giải thích về API

Mô hình hoạt động của API:

![](/Anh/Screenshot_895.png)

Như vậy, chúng ta có thể hiểu API như một "người phiên dịch" giữa Client và Server. Khi Client gửi yêu cầu, API sẽ "dịch" yêu cầu đó sang ngôn ngữ mà Server có thể hiểu được. Sau khi Server xử lý xong, API lại "dịch" kết quả trả về thành dạng mà Client có thể đọc được.

Vậy còn cơ chế hoạt động của Bot_Tele_Netbox thì sao, hãy cùng tham khảo mô hình sau:

![](/Anh/Screenshot_960.png)

Từ mô hình trên, chúng ta có thể thấy quá trình hoạt động của Bot_Tele_Netbox diễn ra như sau:
1. Người dùng gửi yêu cầu thông qua Telegram.
2. Bot nhận yêu cầu và sử dụng API của NetBox để truy vấn thông tin.
3. NetBox xử lý yêu cầu và trả về kết quả cho Bot.
4. Bot định dạng kết quả và gửi lại cho người dùng thông qua Telegram.

Rất dễ hiểu đúng không nào 

### 2. Một vài thông tin về code

Bot_Tele_NetBox sử dụng `pynetbox` - một thư viện python hỗ trợ cho việc giao tiếp giữa thiết bị và NetBox thông qua API. Thay vì dùng request, nhập URL và Getdata phức tạp thì nay chúng ta đã có `pynetbox` với cách sử dụng dễ dàng, tiện ích hơn.

Và tất nhiên để cấu hình cho Bot được thì chúng ta sẽ cần có một số lib từ thư viện ***Telegram*** của Python để làm việc với Bot Telegram.

Hàm `Async` - ***Asynchronous def*** là một hàm bất động bộ đã được tôi sử dụng trong code của mình. Các hàm kiểu này có thể chạy độc lập, mặc cho các hàm khác ở trên nó chưa được chạy hoặc chưa chạy xong.

Ngôn ngữ Markdown được tôi sử dụng để định dạng tin nhắn đầu ra cho Bot, khiến tin nhắn trở nên đẹp, dễ nhìn và giúp người dùng dễ thao tác hơn.

Code chính được sử dụng ở đây là ngôn ngữ Python. Về mặt tư duy logic, code dựa trên code có sẵn của  [hungviet99](https://github.com/hungviet99), được cải tiến bởi tôi và có thêm sử trợ giúp của đồng nghiệp và chatGPT để tạo ra phiên bản tốt nhất hiện tại.

## III. Tham khảo
[Kiến thức về Pynetbox](https://github.com/netbox-community/pynetbox)

[Hàm Async](https://superfastpython.com/asyncio-async-def/)

[Telegram Bot](https://core.telegram.org/bots)

[Markdown Python](https://python-markdown.github.io/reference/markdown/)

Nếu có bất kỳ thắc mắc bạn có thể liên hệ với tôi thông qua:
- **Telegram**: `@ducmanh2873`
- **Gmail**: `luongducmanh02@gmail.com`