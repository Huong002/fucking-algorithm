# Hiểu hết session và cookie trong một bài



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

cookie mọi người chắc đều quen, ví dụ đăng nhập vài web một thời gian thì bắt đăng nhập lại; ví dụ có bạn rất thích viết crawler, đôi khi web chặn được crawler của bạn, những cái này đều liên quan cookie. Nếu bạn hiểu logic xử lý cookie và session ở backend server, sẽ giải thích được các hiện tượng này, thậm chí tận dụng kẽ hở để dùng chùa vô hạn, để tôi kể từ từ.

### Một, giới thiệu session và cookie

cookie xuất hiện vì HTTP là một loại giao thức không trạng thái, nói cách khác, server không nhớ bạn, có thể bạn mỗi lần refresh web là phải nhập lại tài khoản mật khẩu để đăng nhập. Điều này rõ ràng không thể chấp nhận, tác dụng của cookie ví như server dán cho bạn cái nhãn, rồi mỗi lần bạn gửi request tới server, server sẽ nhận ra bạn qua cookie.

Trừu tượng khái quát một chút: **một cookie có thể coi là một 「biến」, dạng `name=value`, lưu ở trình duyệt; một session có thể hiểu là một cấu trúc dữ liệu, đa số là 「ánh xạ」 (key-value), lưu ở server**.

Chú ý, tôi nói 「một」 cookie có thể coi là một biến, nhưng server có thể đặt nhiều cookie một lần, nên đôi khi nói cookie là 「một nhóm」 cặp key-value, cũng giải thích được.

cookie có thể ở server qua field SetCookie của HTTP để đặt cookie, ví dụ tôi dùng Go viết một dịch vụ đơn giản:

```go
func cookie(w http.ResponseWriter, r *http.Request) {
    // Đã đặt hai cookie
	http.SetCookie(w, &http.Cookie{
		Name:       "name1",
		Value:      "value1",
	})

	http.SetCookie(w, &http.Cookie{
		Name:  "name2",
		Value: "value2",
	})
    // Ghi chuỗi vào trang web
	fmt.Fprintln(w, "Nội dung trang")
}
```

Khi trình duyệt truy cập URL tương ứng, qua dev tool xem chi tiết giao tiếp HTTP lần này, thấy response của server phát hai lệnh `SetCookie`:

![](https://labuladong.online/algo/images/session/1.png)

Sau đó, field `Cookie` trong request của trình duyệt sẽ mang hai cookie này:

![](https://labuladong.online/algo/images/session/2.png)

**Tác dụng của cookie thực ra đơn giản vậy, chẳng qua là server dán nhãn cho mỗi client (trình duyệt)**, tiện cho server nhận diện mà thôi. Đương nhiên, HTTP còn nhiều tham số có thể đặt cho cookie, ví dụ thời gian hết hạn, hay để cookie nào đó chỉ path đặc biệt nào mới dùng được v.v.

Nhưng vấn đề là, ta cũng biết nhiều web giờ chức năng rất phức tạp, mà liên quan nhiều tương tác dữ liệu, ví dụ chức năng giỏ hàng của web thương mại, lượng tin lớn, mà cấu trúc cũng phức tạp, không thể qua cơ chế cookie đơn giản truyền nhiều tin vậy, mà phải biết field cookie lưu trong HTTP header, cho dù chở được tin này, cũng tốn nhiều băng thông, khá tốn tài nguyên mạng.

session có thể phối hợp với cookie để giải vấn đề này, ví dụ một cookie lưu một biến `sessionID=xxxx`, chỉ cần truyền cookie này cho server, rồi server qua ID này tìm session tương ứng, session này là một cấu trúc dữ liệu, trong đó lưu chi tiết như giỏ hàng của user đó, server có thể qua tin này trả về trang web được cá nhân hóa cho user đó, giải quyết hiệu quả vấn đề theo dõi user.

**session là một cấu trúc dữ liệu, do developer web thiết kế, nên có thể chứa đủ loại dữ liệu**, chỉ cần cookie của client truyền tới một session ID duy nhất, server là tìm được session tương ứng, nhận ra khách này.

Đương nhiên, vì session lưu ở server, chắc chắn tốn tài nguyên server, nên session thường đều có thời gian hết hạn, server thường sẽ kiểm tra định kỳ và xóa session hết hạn, nếu sau đó user lại truy cập server, có thể phải đăng nhập lại v.v., rồi server tạo mới một session, truyền session ID qua dạng cookie cho client.

Vậy, ta biết nguyên lý cookie và session, có lợi thực tế gì? **Ngoài việc đối phó phỏng vấn, tôi nói cho bạn một mẹo láu cá, chính là có thể dùng chùa vài dịch vụ**.

Có web, lần đầu bạn dùng dịch vụ của nó, nó cho dùng thử miễn phí, nhưng dùng một lần xong thì bắt bạn đăng nhập rồi trả phí mới dùng tiếp. Mà bạn phát hiện web dường như bằng thủ đoạn nào đó nhớ máy bạn, trừ khi bạn đổi máy hay đổi trình duyệt mới dùng chùa lại được.

Vậy vấn đề tới, lúc bạn dùng thử mà không đăng nhập, server web nhớ bạn bằng cách nào? Rất rõ ràng, server chắc chắn đã gắn cookie cho trình duyệt bạn, phía backend lập session tương ứng ghi lại trạng thái bạn. Trình duyệt bạn mỗi lần truy cập web đó đều ngoan ngoãn mang theo cookie, server tra session một cái là biết trình duyệt này đã dùng miễn phí rồi, phải bắt nó đăng nhập trả phí, không cho nó dùng chùa tiếp.

Vậy nếu tôi không để trình duyệt gửi cookie, mỗi lần đều giả làm người mới lần đầu tới dùng thử, chẳng phải có thể dùng chùa liên tục sao? Trình duyệt sẽ lưu cookie của web dưới dạng file ở chỗ nào đó (trình duyệt khác nhau cấu hình khác nhau), bạn tìm rồi xóa là được. Nhưng với Firefox và Chrome, có nhiều plugin có thể sửa thẳng cookie, ví dụ Chrome của tôi dùng một plugin tên EditThisCookie, đây là web chính thức của họ:

![](https://labuladong.online/algo/images/session/3.png)

Loại plugin này có thể đọc cookie của trình duyệt ở web hiện tại, mở plugin có thể tùy ý sửa và xóa cookie. **Đương nhiên, thỉnh thoảng dùng chùa một hai lần còn được, không khuyến khích dùng chùa tần suất cao, muốn dùng thường xuyên vẫn nên trả tiền, nếu không web không kiếm được tiền, chỉ có thể bỏ cơ chế dùng thử miễn phí này**.

Trên đây là giới thiệu đơn giản về cookie và session, cookie là một phần của giao thức HTTP, không phức tạp, còn session có thể tùy biến, nên dưới đây xem kỹ kiến trúc code quản lý session.

### Hai, implement session

Nguyên lý session không khó, nhưng implement cụ thể nó lại rất khéo léo, thông thường cần ba component phối hợp hoàn thành, chúng lần lượt là ba class (interface) `Manager`, `Provider` và `Session`.

![](https://labuladong.online/algo/images/session/4.jpg)

1, Trình duyệt qua giao thức HTTP gửi tới server request tài nguyên web ở path `/content`, hàm Handler trên path tương ứng nhận request, phân tích cookie trong HTTP header, lấy sessionID lưu trong đó, rồi đưa ID này cho `Manager`.

2, `Manager` đóng vai session manager, chủ yếu lưu vài thông tin cấu hình, ví dụ thời gian sống session, tên cookie v.v. Mà mọi session được chứa trong một `Provider` nội bộ của `Manager`. Nên `Manager` sẽ truyền `sid` (sessionID) cho `Provider`, để nó tìm xem ID này tương ứng với session cụ thể nào.

3, `Provider` chính là một container, hay gặp nhất chính là một hash table, ánh xạ từng `sid` với session tương ứng một-một. Nhận `sid` do `Manager` truyền xong, nó sẽ tìm được session struct tương ứng với `sid`, chính là struct `Session`, rồi trả về nó.

4, `Session` lưu trữ thông tin cụ thể của user, logic trong hàm Handler lấy tin này, sinh trang HTML của user đó, trả cho client.

Vậy bạn có thể hỏi, vì sao làm rắc rối vậy, trực tiếp tạo một hash table trong hàm Handler, rồi lưu ánh xạ `sid` và struct `Session` chẳng phải xong sao?

**Đây chính là điểm khéo ở tầng thiết kế**, dưới đây sẽ nói vì sao chia thành `Manager`, `Provider` và `Session`.

Nói từ `Session` ở tầng thấp nhất. Đã biết session chính là key-value, vì sao không dùng thẳng hash table, mà phải trừu tượng ra cấu trúc dữ liệu này?

Thứ nhất, vì struct `Session` có thể không chỉ lưu một hash table, còn có thể lưu vài dữ liệu phụ, ví dụ `sid`, số lần truy cập, thời gian hết hạn hay thời gian truy cập cuối, như vậy tiện implement thuật toán như LRU, LFU.

Thứ hai, vì session có thể có cách lưu khác nhau. Nếu dùng hash table có sẵn của ngôn ngữ, thì dữ liệu session sẽ lưu trong bộ nhớ, nếu lượng dữ liệu lớn, rất dễ làm crash chương trình, mà một khi chương trình kết thúc, mọi dữ liệu session đều mất. Nên có thể có nhiều cách lưu session, ví dụ lưu vào cơ sở dữ liệu cache Redis, hay lưu vào MySQL v.v.

Do đó, struct `Session` cung cấp một lớp trừu tượng, che khác biệt cách lưu, chỉ cần cung cấp một nhóm interface chung thao tác key-value:

```go
type Session interface {
    // Đặt cặp key-value
    Set(key, val interface{})
    // Lấy value tương ứng với key
    Get(key interface{}) interface{}
    // Xóa key
    Delete(key interface{})
}
```

Nói tiếp vì sao `Provider` phải được trừu tượng ra. `Provider` trong hình trên chính là một hash table, lưu ánh xạ `sid` tới `Session`, nhưng thực tế chắc chắn phức tạp hơn. Ta chẳng phải phải thỉnh thoảng xóa vài session sao, ngoài việc đặt thời gian sống, còn có thể dùng chiến lược khác, ví dụ thuật toán loại cache LRU, như vậy bên trong `Provider` cần dùng cấu trúc dữ liệu như hash-linked-list để lưu session.

> [!TIP]
> Về điểm tinh diệu của thuật toán LRU, xem bài trước [Chi tiết thuật toán LRU](https://labuladong.online/algo/data-structure/lru-cache/).

Do đó, `Provider` làm container, chính là để che chi tiết thuật toán, tổ chức quan hệ ánh xạ `sid` và `Session` bằng cấu trúc dữ liệu và thuật toán hợp lý, chỉ cần implement mấy method dưới để thêm/xóa/tìm/sửa session:

```go
type Provider interface {
    // Thêm mới và trả về một session
    SessionCreate(sid string) (Session, error)
    // Xóa một session
    SessionDestroy(sid string)
    // Tìm một session
    SessionRead(sid string) (Session, error)
    // Sửa một session
    SessionUpdate(sid string)
    // Thu hồi session hết hạn bằng thuật toán kiểu LRU
    SessionGC(maxLifeTime int64)
}
```

Cuối nói `Manager`, phần lớn việc cụ thể đều ủy cho `Session` và `Provider` gánh, `Manager` chủ yếu chính là một tập tham số, ví dụ thời gian sống session, chiến lược dọn session hết hạn, cùng cách lưu session khả dụng. `Manager` che chi tiết thao tác, ta có thể qua `Manager` cấu hình linh hoạt cơ chế session.

Tóm lại, nguyên nhân chủ yếu nhất khiến cơ chế session được chia thành mấy phần chính là tách biệt, tiện implement tùy biến. Tôi xem trên Github vài dịch vụ session Go implement, source đều rất đơn giản, bạn hứng thú có thể học:

https://github.com/alexedwards/scs

https://github.com/astaxie/build-web-application-with-golang




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
