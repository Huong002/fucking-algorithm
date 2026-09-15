# Redis bị xâm nhập



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

Thôi, tôi cũng làm tiêu đề câu view một lần, như bạn cẩn thận thế này, sao có thể để server bị xâm nhập?

Thực ra là thế này, hôm qua tôi chat với một người bạn, cậu ấy nói có một cloud server chạy database Redis, một hôm bỗng phát hiện **dữ liệu toàn mất**, chỉ còn một cặp key-value kỳ lạ, trong đó value trông như chuỗi RSA public key, cậu ấy tưởng误 thao tác xóa库, may server mình không có dữ liệu quan trọng gì, cũng không để ý.

Qua một番攀谈交心 biết được, cậu ấy chạy một open source比较古老 đã ngừng维护, cài bản Redis cũ, mà cậu ấy dùng Linux không rành lắm. Tôi liền biết server cậu ấy đã bị hạ, nghĩ có lẽ còn không ít người như bạn tôi, không coi trọng quyền OS,设置 firewall và bảo vệ database, tôi viết một bài xem đơn giản nguyên nhân tình huống này, cùng cách phòng.

> [!NOTE]
> Thủ pháp này giờ đã không dùng được, vì bản Redis mới đều thêm protect mode, tăng bảo mật, chúng ta chỉ mô phỏng đơn giản ở local, đừng thử bậy.

### Diễn biến sự kiện

Thực ra thủ pháp tấn công này đều là chuyện 2015, hồi đó cơ chế bảo vệ an toàn của Redis比较差, chỉ能靠 nhân viên vận hành cấu hình hợp lý để đảm bảo an toàn database. Có thời gian, mấy vạn node Redis toàn cầu bị tấn công, xuất hiện hiện tượng lạ trên, mọi dữ liệu bị xóa sạch, chỉ còn một key tên `crackit`, value nó形似 chuỗi RSA public key.

Sau查证, kẻ tấn công lợi dụng chức năng设置 cấu hình động và persist dữ liệu của Redis, ghi RSA public key của mình vào file `/root/.ssh/authored_keys` của server bị tấn công, từ đó có thể dùng private key login thẳng user root đối phương, xâm nhập hệ thống đối phương.

Server沦陷 làm bảo vệ an toàn rất不好, cụ thể như sau:

1, Port Redis là port mặc định, mà có thể truy cập từ public net.

2, Redis còn chưa đặt mật khẩu.

3, Process Redis do user root khởi động.

Mỗi điểm trên đều比较 nguy hiểm, hợp lại thì真是 rất致命. Chưa nói người ta ghi public key vào hệ thống bạn,就说 nối vào database bạn rồi xóa库, tổn thất đó đã đủ lớn. Vậy流程 cụ thể là gì, dưới đây tôi demo đơn giản ở địa chỉ loopback local.

### Demo local

Port mặc định Redis lắng nghe là 6379, ta设置 nó nhận kết nối card mạng 127.0.0.1, như vậy tôi từ local肯定 có thể nối Redis,以此 mô phỏng điều kiện 「từ public net có thể truy cập Redis」 này.

Giờ tôi là user thường tên fdl, tôi muốn dùng ssh login user root trên hệ thống, phải nhập mật khẩu root, tôi không biết, nên không cách login.

Ngoài login mật khẩu, còn có thể dùng cặp key RSA login, nhưng必须要把 public key của tôi存 vào home root `/root/.ssh/authored_keys`. Ta biết quyền thư mục `/root`设置 không cho user khác nào xông vào đọc ghi:

![](https://labuladong.online/algo/images/redis/1.png)

Nhưng, tôi phát hiện mình竟然 có thể truy cập thẳng Redis:

![](https://labuladong.online/algo/images/redis/2.png)

Nếu Redis chạy với thân phận root, thì tôi có thể qua thao tác Redis, để nó ghi public key của tôi vào home root. Redis có một kiểu persist là sinh file RDB, trong đó sẽ chứa dữ liệu gốc.

Tôi露出 nụ cười邪恶, trước xóa sạch dữ liệu trong Redis, rồi ghi RSA public key của tôi vào database, ở đây thêm換行 đầu cuối mục đích tránh quá trình sinh file RDB làm hỏng chuỗi public key:

![](https://labuladong.online/algo/images/redis/3.png)

Ra lệnh Redis lưu file dữ liệu sinh ra vào file `authored_keys` trong `/root/.ssh/`:

![](https://labuladong.online/algo/images/redis/4.png)

Giờ, home root đã chứa RSA public key của ta, ta giờ có thể qua cặp key login vào root:

![](https://labuladong.online/algo/images/redis/5.png)

Xem public key vừa ghi vào nhà root:

![](https://labuladong.online/algo/images/redis/6.png)

Loạn mã là mã hóa nào đó của file GDB吧, nhưng public key ở giữa được lưu完整, mà chương trình login ssh竟然 cũng nhận đoạn public key bị loạn mã bao quanh này!

Tới đây, có quyền root, là có thể为所欲为。。。

### Rút kinh nghiệm

Dù giờ cơ bản không bị tấn công này (bản Redis mới không mật khẩu mặc định không mở ra外网), nhưng với an toàn hệ thống là mỗi người đều nên coi trọng.

Tụi mình tự vọc, dùng cloud server thấp, để省事一般 cũng không cấu hình firewall认真, database không đặt mật khẩu hay đặt mật khẩu đơn giản như admin, root,反正 cũng không có dữ liệu gì. Như vậy肯定 không phải thói quen tốt.

Giờ hệ máy tính của ta ngày càng完善, mỗi project成熟 đều do nhóm xuất sắc nhất维护, nói về技术应该算 vô懈可击, vậy nơi duy nhất có thể出问题 chính là người dùng chúng.

Như hay thấy QQ ai đó bị盗, tôi tin người盗号肯定 không phải chạy vào database Tencent盗号,肯定 là chủ号防范 ý thức kém, nhập tài khoản mật khẩu ở web钓鱼 nào đó, dẫn tới bị盗. Tôi cơ bản chưa thấy WeChat bị盗, có thể là WeChat弱化 login mật khẩu, đổi dùng quét QR login. Đây应该 cũng算 một考量 an toàn吧, dù sao WeChat có chức năng thanh toán.

Trò骗 trên với dân技术 mà nói, xem url, trình duyệt phân tích gói mạng là rất dễ nhận ra, nhưng bạn还别 không tin, người一般真的搞 không rõ sao nhận web钓鱼 và web chính thức. Như tôi真没想到都 2020 rồi, còn có người tìm lỗ hổng Redis này, mà còn có người trúng。。。

Vậy nói về dùng database Redis, trên web chính thức viết rõ建议 bảo vệ an toàn, tôi tóm đơn giản吧:

1, Đừng dùng user root khởi động Redis Server, mà nhất định phải đặt mật khẩu, mà mật khẩu đừng quá ngắn, nếu không dễ bị暴力破解.

2, Cấu hình firewall server và file config Redis, cố đừng để Redis tiếp xúc外界.

3, Lợi dụng chức năng rename ngụy trang lệnh nguy hiểm như flushall, đề phòng bị xóa库, mất dữ liệu.




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
