# Redis bị xâm nhập



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

Thôi, tôi cũng làm tiêu đề câu view một lần, như bạn cẩn thận thế này, sao có thể để server bị xâm nhập?

Thực ra là thế này, hôm qua tôi chat với một người bạn, cậu ấy nói có một cloud server chạy cơ sở dữ liệu Redis, một hôm bỗng phát hiện **dữ liệu mất sạch**, chỉ còn một cặp key-value kỳ lạ, trong đó value trông như chuỗi RSA public key, cậu ấy tưởng mình lỡ tay xóa cơ sở dữ liệu, may server mình không có dữ liệu quan trọng gì, cũng không để ý.

Qua một hồi trò chuyện thân tình mới biết được, cậu ấy chạy một open source khá cổ đã ngừng bảo trì, cài bản Redis cũ, mà cậu ấy dùng Linux không rành lắm. Tôi liền biết server cậu ấy đã bị hạ, nghĩ có lẽ còn không ít người như bạn tôi, không coi trọng quyền OS, thiết lập firewall và bảo vệ cơ sở dữ liệu, tôi viết một bài xem đơn giản nguyên nhân tình huống này, cùng cách phòng tránh.

> [!NOTE]
> Thủ pháp này giờ đã không dùng được nữa, vì bản Redis mới đều thêm protect mode, tăng bảo mật, chúng ta chỉ mô phỏng đơn giản ở local, đừng thử bậy.

### Diễn biến sự kiện

Thực ra thủ pháp tấn công này đều là chuyện năm 2015, hồi đó cơ chế bảo vệ an toàn của Redis khá kém, chỉ có thể dựa vào nhân viên vận hành cấu hình hợp lý để đảm bảo an toàn cơ sở dữ liệu. Có một thời gian, mấy vạn node Redis toàn cầu bị tấn công, xuất hiện hiện tượng lạ kể trên, mọi dữ liệu bị xóa sạch, chỉ còn một key tên `crackit`, value của nó trông giống chuỗi RSA public key.

Sau khi xác minh, kẻ tấn công lợi dụng chức năng thiết lập cấu hình động và persist dữ liệu của Redis, ghi RSA public key của mình vào file `/root/.ssh/authored_keys` của server bị tấn công, từ đó có thể dùng private key đăng nhập thẳng vào user root của đối phương, xâm nhập hệ thống đối phương.

Server sập vì làm bảo vệ an toàn rất kém, cụ thể như sau:

1, Port Redis là port mặc định, mà có thể truy cập từ public net.

2, Redis còn chưa đặt mật khẩu.

3, Process Redis do user root khởi động.

Mỗi điểm trên đều khá nguy hiểm, hợp lại thì thật sự rất chí mạng. Chưa nói người ta ghi public key vào hệ thống bạn, chỉ riêng việc nối vào cơ sở dữ liệu của bạn rồi xóa cơ sở dữ liệu, tổn thất đó đã đủ lớn. Vậy quy trình cụ thể là gì, dưới đây tôi demo đơn giản ở địa chỉ loopback local.

### Demo local

Port mặc định Redis lắng nghe là 6379, ta thiết lập cho nó nhận kết nối card mạng 127.0.0.1, như vậy tôi từ local chắc chắn có thể nối tới Redis, lấy đó mô phỏng điều kiện 「có thể truy cập Redis từ public net」 này.

Giờ tôi là user thường tên fdl, tôi muốn dùng ssh đăng nhập vào user root trên hệ thống, phải nhập mật khẩu root, tôi không biết, nên không cách nào đăng nhập.

Ngoài đăng nhập bằng mật khẩu, còn có thể dùng cặp key RSA để đăng nhập, nhưng bắt buộc phải lưu public key của tôi vào home của root `/root/.ssh/authored_keys`. Ta biết quyền thư mục `/root` được thiết lập không cho user nào khác xông vào đọc ghi:

![](https://labuladong.online/algo/images/redis/1.png)

Nhưng, tôi phát hiện mình vậy mà có thể truy cập thẳng Redis:

![](https://labuladong.online/algo/images/redis/2.png)

Nếu Redis chạy với thân phận root, thì tôi có thể thông qua thao tác Redis, để nó ghi public key của tôi vào home của root. Redis có một kiểu persist là sinh file RDB, trong đó sẽ chứa dữ liệu gốc.

Tôi nở nụ cười gian xảo, trước hết xóa sạch dữ liệu trong Redis, rồi ghi RSA public key của tôi vào cơ sở dữ liệu, ở đây thêm xuống dòng ở đầu và cuối nhằm tránh quá trình sinh file RDB làm hỏng chuỗi public key:

![](https://labuladong.online/algo/images/redis/3.png)

Ra lệnh cho Redis lưu file dữ liệu sinh ra vào file `authored_keys` trong `/root/.ssh/`:

![](https://labuladong.online/algo/images/redis/4.png)

Giờ, home của root đã chứa RSA public key của ta, ta giờ có thể dùng cặp key đăng nhập vào root:

![](https://labuladong.online/algo/images/redis/5.png)

Xem public key vừa ghi vào nhà root:

![](https://labuladong.online/algo/images/redis/6.png)

Đoạn loạn mã là mã hóa nào đó của file GDB mà thôi, nhưng public key ở giữa được lưu đầy đủ, vậy mà chương trình đăng nhập ssh cũng chấp nhận đoạn public key bị đống loạn mã bao quanh này!

Tới đây, có quyền root rồi thì muốn làm gì cũng được...

### Rút kinh nghiệm

Dù giờ cơ bản không còn bị kiểu tấn công này nữa (bản Redis mới không mật khẩu thì mặc định không mở ra mạng ngoài), nhưng an toàn hệ thống là điều mỗi người đều nên coi trọng.

Tụi mình tự vọc, dùng cloud server rẻ tiền, để cho tiện thì thường cũng không cấu hình firewall nghiêm túc, cơ sở dữ liệu không đặt mật khẩu hay đặt mật khẩu đơn giản như admin, root, đằng nào cũng không có dữ liệu gì. Như vậy chắc chắn không phải thói quen tốt.

Giờ hệ thống máy tính của ta ngày càng hoàn thiện, mỗi project trưởng thành đều do nhóm xuất sắc nhất bảo trì, nói về kỹ thuật thì coi như không chê vào đâu được, vậy nơi duy nhất có thể xảy ra vấn đề chính là người dùng chúng ta.

Như hay thấy QQ của ai đó bị trộm, tôi tin kẻ trộm tài khoản chắc chắn không phải chạy vào cơ sở dữ liệu Tencent để trộm tài khoản, chắc chắn là chủ tài khoản ý thức phòng ngừa kém, nhập tài khoản mật khẩu ở web lừa đảo nào đó, dẫn tới bị trộm. Tôi cơ bản chưa thấy WeChat bị trộm, có thể là WeChat đã làm nhẹ việc đăng nhập bằng mật khẩu, chuyển sang dùng quét QR để đăng nhập. Đây cũng coi như một cân nhắc an toàn mà thôi, dù sao WeChat có chức năng thanh toán.

Trò lừa kể trên với dân kỹ thuật mà nói, xem url, dùng trình duyệt phân tích gói mạng là rất dễ nhận ra, nhưng bạn đừng không tin, người bình thường thật sự không phân biệt nổi web lừa đảo với web chính thức. Như tôi thật không ngờ đã năm 2020 rồi mà còn có người đi tìm lỗ hổng Redis này, mà còn có người dính...

Vậy nói về dùng cơ sở dữ liệu Redis, trên web chính thức viết rõ kiến nghị bảo vệ an toàn, tôi tóm tắt đơn giản mà thôi:

1, Đừng dùng user root khởi động Redis Server, mà nhất định phải đặt mật khẩu, mà mật khẩu đừng quá ngắn, nếu không dễ bị brute-force bẻ khóa.

2, Cấu hình firewall của server và file config Redis, cố gắng đừng để Redis tiếp xúc với bên ngoài.

3, Lợi dụng chức năng rename để ngụy trang các lệnh nguy hiểm như flushall, đề phòng bị xóa cơ sở dữ liệu, mất dữ liệu.




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
