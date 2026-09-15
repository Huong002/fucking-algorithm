# Hiểu hết session và cookie trong một bài



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

cookie mọi người chắc đều quen, ví dụ login vài web một thời gian thì bắt login lại; ví dụ có bạn rất thích chơi爬虫, đôi khi web chính là chặn được爬虫 của bạn, những cái này đều liên quan cookie. Nếu bạn hiểu logic xử lý cookie và session ở backend server, là giải thích được các hiện tượng này, thậm chí钻空子白嫖 vô hạn, để tôi慢慢道来.

### Một, giới thiệu session và cookie

cookie xuất hiện vì HTTP là một loại giao thức无状态, nói cách khác, server không nhớ bạn, có thể bạn mỗi lần refresh web là phải nhập lại tài khoản mật khẩu để login. Điều này rõ ràng không thể chấp nhận, tác dụng cookie好比 server dán cho bạn cái nhãn, rồi mỗi lần bạn gửi request tới server, server就能 qua cookie nhận ra bạn.

Trừu tượng概括 một chút: **một cookie có thể coi là một 「biến」, dạng `name=value`, lưu ở trình duyệt; một session có thể hiểu là một cấu trúc dữ liệu, đa số là 「ánh xạ」 (key-value), lưu ở server**.

Chú ý, tôi nói 「một」 cookie có thể coi là một biến, nhưng server có thể设置 nhiều cookie một lần, nên đôi khi nói cookie là 「một nhóm」 cặp key-value, cũng nói通 được.

cookie có thể ở server qua field SetCookie của HTTP设置 cookie, ví dụ tôi dùng Go viết một dịch vụ đơn giản:

```go
func cookie(w http.ResponseWriter, r *http.Request) {
    // 设置了两个 cookie -> Đã đặt hai cookie
	http.SetCookie(w, &http.Cookie{
		Name:       "name1",
		Value:      "value1",
	})

	http.SetCookie(w, &http.Cookie{
		Name:  "name2",
		Value: "value2",
	})
    // 将字符串写入网页 -> Ghi chuỗi vào trang web
	fmt.Fprintln(w, "页面内容")
}
```

Khi trình duyệt truy cập URL tương ứng, qua dev tool xem chi tiết giao tiếp HTTP lần này, thấy回应 của server phát hai lệnh `SetCookie`:

![](https://labuladong.online/algo/images/session/1.png)

Sau đó, field `Cookie` trong request của trình duyệt就带 hai cookie này:

![](https://labuladong.online/algo/images/session/2.png)

**Tác dụng cookie thực ra đơn giản vậy,无非 server打标签 cho mỗi client (trình duyệt)**, tiện server辨认 mà thôi. Đương nhiên, HTTP còn nhiều tham số có thể设置 cookie, ví dụ thời gian hết hạn, hay để cookie nào đó chỉ path特定 nào mới dùng được v.v.

Nhưng vấn đề là, ta cũng biết nhiều web giờ chức năng rất phức tạp, mà liên quan nhiều tương tác dữ liệu, ví dụ chức năng giỏ hàng của web thương mại, lượng tin lớn, mà cấu trúc cũng phức tạp, không thể qua cơ chế cookie đơn giản truyền nhiều tin vậy, mà phải biết field cookie lưu trong HTTP header, cho dù承载 được tin này, cũng tốn nhiều băng thông,比较 tốn tài nguyên mạng.

session có thể配合 cookie giải vấn đề này, ví dụ một cookie lưu một biến `sessionID=xxxx`,仅仅把 cookie này truyền cho server, rồi server qua ID này tìm session tương ứng, session này là một cấu trúc dữ liệu, trong đó lưu chi tiết như giỏ hàng của user đó, server có thể qua tin này trả web定制化 của user đó, giải quyết hiệu quả vấn đề追踪 user.

**session là một cấu trúc dữ liệu, do developer web thiết kế, nên có thể承载 đủ loại dữ liệu**, chỉ cần cookie của client truyền tới một session ID duy nhất, server là tìm được session tương ứng, nhận ra khách này.

Đương nhiên, vì session lưu ở server,肯定 tốn tài nguyên server, nên session一般 đều có thời gian hết hạn, server一般 sẽ kiểm tra định kỳ và xóa session hết hạn, nếu sau user đó lại truy cập server, có thể đối mặt重新登录 v.v., rồi server tạo mới một session, truyền session ID qua dạng cookie cho client.

Vậy, ta biết nguyên lý cookie và session, có lợi thực tế gì? **Ngoài应对 phỏng vấn, tôi nói cho bạn một util鸡贼, chính là có thể白嫖 vài dịch vụ**.

Có web, lần đầu bạn dùng dịch vụ nó, nó trực tiếp miễn phí cho试用, nhưng dùng một lần xong thì bắt bạn login rồi trả phí dùng tiếp. Mà bạn phát hiện web dường như qua thủ đoạn nào đó nhớ máy bạn, trừ khi bạn đổi máy hay đổi trình duyệt mới白嫖 lại được.

Vậy vấn đề tới, lúc bạn试用 không login, server web nhớ bạn sao? Rất rõ ràng, server一定打 cookie cho trình duyệt bạn,后台 lập session tương ứng ghi trạng thái bạn. Trình duyệt bạn mỗi lần truy cập web đó đều听话带 cookie, server一查 session là biết trình duyệt này đã dùng miễn phí rồi, phải bắt nó login trả phí, không cho nó白嫖 tiếp.

Vậy nếu tôi không để trình duyệt gửi cookie, mỗi lần đều giả làm萌新 lần đầu tới试用, chẳng phải có thể白嫖 liên tục sao? Trình duyệt sẽ存 cookie của web dưới dạng file ở chỗ nào đó (trình duyệt khác cấu hình khác), bạn tìm rồi xóa là được. Nhưng với Firefox và Chrome, có nhiều plugin có thể edit thẳng cookie, ví dụ Chrome của tôi dùng một plugin tên EditThisCookie, đây là web chính thức của họ:

![](https://labuladong.online/algo/images/session/3.png)

Loại plugin này có thể đọc cookie của trình duyệt ở web hiện tại, mở plugin có thể tùy ý edit và xóa cookie. **Đương nhiên, thỉnh thoảng白嫖 một hai lần còn được, không khuyến khích白嫖 tần suất cao, muốn dùng thường vẫn móc tiền吧, nếu không web không kiếm được tiền, chỉ能取消 cơ chế试用 miễn phí này**.

Trên đây là giới thiệu đơn giản về cookie và session, cookie là một phần của giao thức HTTP, không phức tạp, còn session có thể定制, nên dưới đây xem kỹ架构 code quản lý session吧.

### Hai, implement session

Nguyên lý session không khó, nhưng implement cụ thể nó可是 rất có技巧, thông thường cần ba component配合 hoàn thành, chúng lần lượt là ba class (interface) `Manager`, `Provider` và `Session`.

![](https://labuladong.online/algo/images/session/4.jpg)

1, Trình duyệt qua giao thức HTTP向 server request tài nguyên web path `/content`, hàm Handler trên path tương ứng nhận request,解析 cookie trong HTTP header, được sessionID lưu trong đó, rồi đưa ID này cho `Manager`.

2, `Manager` đóng vai session manager, chủ yếu lưu vài thông tin cấu hình, ví dụ thời gian sống session, tên cookie v.v. Mà mọi session存 trong một `Provider` nội bộ `Manager`. Nên `Manager` sẽ truyền `sid` (sessionID) cho `Provider`, để nó tìm ID này tương ứng cụ thể session nào.

3, `Provider` chính là một container, hay gặp nhất应该 chính là một hash table, ánh xạ mỗi `sid` với session tương ứng一一. Nhận `sid` `Manager` truyền xong, nó就找到 session struct tương ứng `sid`, chính là struct `Session`, rồi trả về nó.

4, `Session`中存储 thông tin cụ thể của user, logic trong hàm Handler lấy tin này, sinh trang HTML của user đó, trả cho client.

Vậy bạn có thể hỏi, vì sao搞麻烦 vậy, trực tiếp搞 một hash table trong hàm Handler, rồi lưu ánh xạ `sid` và struct `Session` chẳng phải xong sao?

**Đây chính là技巧 ở层面 thiết kế**, dưới đây就来说 vì sao chia thành `Manager`, `Provider` và `Session`.

Nói từ `Session`底层 nhất. Đã session chính là key-value, vì sao không dùng thẳng hash table, mà phải trừu tượng ra cấu trúc dữ liệu này?

Thứ nhất, vì struct `Session` có thể không chỉ lưu một hash table, còn có thể lưu vài dữ liệu phụ, ví dụ `sid`, số lần truy cập, thời gian hết hạn hay thời gian truy cập cuối, như vậy tiện implement thuật toán như LRU, LFU.

Thứ hai, vì session có thể có cách lưu khác nhau. Nếu dùng hash table内置 của ngôn ngữ, thì dữ liệu session就是 lưu trong bộ nhớ, nếu lượng dữ liệu lớn, rất dễ gây crash chương trình, mà một khi chương trình kết thúc, mọi dữ liệu session đều mất. Nên có thể có nhiều cách lưu session, ví dụ存 vào database cache Redis, hay存 vào MySQL v.v.

Do đó, struct `Session` cung cấp một lớp trừu tượng, che khác biệt cách lưu, chỉ cần cung cấp một nhóm interface chung thao tác key-value:

```go
type Session interface {
    // 设置键值对 -> Đặt key-value
    Set(key, val interface{})
    // 获取 key 对应的值 -> Lấy value tương ứng key
    Get(key interface{}) interface{}
    // 删除键 key -> Xóa key
    Delete(key interface{})
}
```

Nói tiếp vì sao `Provider` phải trừu tượng ra. `Provider` của图 trên chính là một hash table, lưu ánh xạ `sid` tới `Session`, nhưng thực tế肯定 phức tạp hơn. Ta不是 phải thỉnh thoảng xóa vài session sao, ngoài设置 thời gian sống, còn có thể dùng chiến lược khác, ví dụ thuật toán loại cache LRU, như vậy就需 nội bộ `Provider` dùng cấu trúc dữ liệu như hash-linked-list để lưu session.

> [!TIP]
> Về奥妙 thuật toán LRU, xem bài trước [Chi tiết thuật toán LRU](https://labuladong.online/algo/data-structure/lru-cache/).

Do đó, `Provider` làm container, chính là要 che chi tiết thuật toán, tổ chức quan hệ ánh xạ `sid` và `Session` bằng cấu trúc dữ liệu và thuật toán hợp lý, chỉ cần implement mấy method dưới để增删查改 session:

```go
type Provider interface {
    // 新增并返回一个 session -> Thêm mới và trả về một session
    SessionCreate(sid string) (Session, error)
    // 删除一个 session -> Xóa một session
    SessionDestroy(sid string)
    // 查找一个 session -> Tìm một session
    SessionRead(sid string) (Session, error)
    // 修改一个session -> Sửa một session
    SessionUpdate(sid string)
    // 通过类似 LRU 的算法回收过期的 session -> Thu hồi session hết hạn bằng thuật toán kiểu LRU
    SessionGC(maxLifeTime int64)
}
```

Cuối nói `Manager`, phần lớn việc cụ thể đều ủy cho `Session` và `Provider` gánh, `Manager` chủ yếu chính là một tập tham số, ví dụ thời gian sống session, chiến lược dọn session hết hạn, cùng cách lưu session khả dụng. `Manager` che chi tiết thao tác, ta có thể qua `Manager` cấu hình linh hoạt cơ chế session.

Tóm lại, cơ chế session chia mấy phần nguyên nhân chủ yếu nhất chính là解耦, implement定制化. Tôi trên Github xem vài dịch vụ session Go implement, source đều rất đơn giản, bạn hứng thú có thể học:

https://github.com/alexedwards/scs

https://github.com/astaxie/build-web-application-with-golang




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
