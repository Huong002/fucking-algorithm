# Quy hoạch động: Bàn phím 4 phím



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho user cũ~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [651. 4 Keys Keyboard](https://leetcode.com/problems/4-keys-keyboard/)🔒| [651. Bàn phím 4 phím](https://leetcode.cn/problems/4-keys-keyboard/)🔒| 🟠

**-----------**

Bài 651「Bàn phím bốn phím」trên LeetCode rất thú vị, mà có thể cảm nhận rõ: định nghĩa mảng `dp` khác nhau cần logic hoàn toàn khác nhau, từ đó sinh ra cách giải hoàn toàn khác nhau.

Trước hết xem đề:

Giả sử bạn có một bàn phím đặc biệt, trên đó chỉ có bốn phím, chúng là:

1、Phím `A`: in một chữ `A` lên màn hình.

2、Phím `Ctrl-A`: chọn toàn bộ màn hình.

3、Phím `Ctrl-C`: copy vùng đã chọn vào bộ đệm (buffer).

4、Phím `Ctrl-V`: nhập nội dung bộ đệm vào màn hình tại vị trí con trỏ.

Đâyhoàn toàn giống chức năng chọn hết-copy-paste ta dùng bình thường, chỉ là đề bài coi tổ hợp phím `Ctrl` như một phím. Giờ yêu cầu bạn chỉ được thực hiện `N` lần thao tác, hãy tính trên màn hình nhiều nhất hiển thị được bao nhiêu chữ `A`?

Chữ ký hàm như sau:

<!-- muliti_language -->
```java
int maxA(int N);
```

Ví dụ nhập `N = 3`, thuật toán trả về 3, vì bấm liên tiếp 3 lần phím `A` là phương án tối ưu.

Nếu nhập là `N = 7`, thuật toán trả về 9, dãy thao tác tối ưu như sau:

`A`, `A`, `A`, `Ctrl-A`, `Ctrl-C`, `Ctrl-V`, `Ctrl-V`

Có thể được 9 chữ `A`.

Làm sao sau `N` lần gõ phím được nhiều `A` nhất? Ta liệt kê thôi, với mỗi lần gõ, ta có thể liệt kê bốn khả năng, rõ ràng chính là một bài quy hoạch động.

### ý tưởng thứ nhất

ý tưởng này sẽ rất dễ hiểu, nhưng hiệu quả không cao, ta đi thẳng quy trình: **với bài toán quy hoạch động, trước hết phải rõ có những「trạng thái」nào, có những「lựa chọn」nào**.

Cụ thể tới bài này, với mỗi lần gõ phím, có những「lựa chọn」nào rất rõ ràng: 4 loại, chính là bốn phím đề bài nêu, lần lượt là `A`、`C-A`、`C-C`、`C-V` (`Ctrl` viết gọn là `C`).

Tiếp theo, nghĩ xem với bài này có những「trạng thái」nào? **Hay nói cách khác, ta cần biết thông tin gì, mới phân rã bài gốc thành bài toán conquy mô nhỏ hơn được**?

Bạn xem mình định nghĩa ba trạng thái thế này được không: trạng thái thứ nhất là số lần gõ còn lại, dùng `n` cho biết; trạng thái thứ hai là số ký tự A hiện tại trên màn hình, dùng `a_num` cho biết; trạng thái thứ ba là số ký tự A trong clipboard, dùng `copy` cho biết.

Định nghĩa「trạng thái」như vậy, là có thể biết base case: khi số lần còn lại `n` là 0, `a_num` chính là đáp án ta muốn.

Kết hợp 4 loại「lựa chọn」vừa nói, ta có thể biểu diễn mấy lựa chọn này qua chuyển trạng thái:

```python
dp(n - 1, a_num + 1, copy), # A
# Giải thích: bấm phím A, màn hình thêm một ký tự
# đồng thời tốn 1 lần thao tác

dp(n - 1, a_num + copy, copy), # C-V
# Giải thích: bấm C-V dán, ký tự trong clipboard thêm vào màn hình
# đồng thời tốn 1 lần thao tác

dp(n - 2, a_num, a_num) # C-A C-C
# Giải thích: chọn hết và copydĩ nhiên dùngliên hợp,
# số A trong clipboard thành số A trên màn hình
# đồng thời tốn 2 lần thao tác
```

Như vậy thấy quy mô `n` của bài toán không ngừng giảm, chắc chắn tới được base case `n = 0`, nên ý tưởng này đúng:

<!-- muliti_language -->
```python
def maxA(N: int) -> int:

    # Với trạng thái (n, a_num, copy),
    # cuối cùng trên màn hình nhiều nhất có dp(n, a_num, copy) chữ A
    def dp(n, a_num, copy):
        # base case
        if n <= 0: return a_num;
        # Thử hết mấy lựa chọn, chọn kết quả lớn nhất
        return max(
                dp(n - 1, a_num + 1, copy), # A
                dp(n - 1, a_num + copy, copy), # C-V
                dp(n - 2, a_num, a_num) # C-A C-C
            )

    # Có thể bấm N lần, màn hình và clipboard đều chưa có A
    return dp(N, 0, 0)
```

cách giải này hẳn rất dễ hiểu, vìngữ nghĩa rõ ràng. Dưới đâythì tiếp tục đi quy trình, dùng bảng ghi nhớ loại bỏ bài toán con trùng lặp:

<!-- muliti_language -->
```python
def maxA(N: int) -> int:
    # Bảng ghi nhớ
    memo = dict()
    def dp(n, a_num, copy):
        if n <= 0: return a_num;
        # Tránh tính bài toán con trùng lặp
        if (n, a_num, copy) in memo:
            return memo[(n, a_num, copy)]

        memo[(n, a_num, copy)] = max(
                # Mấy lựa chọn vẫn như cũ
            )
        return memo[(n, a_num, copy)]

    return dp(N, 0, 0)
```

tối ưu code như vậy rồi, bài toán con tuy không lặp nữa, nhưng số lượng vẫn rất nhiều, nộp lên LeetCode sẽquá thời gian.

Ta thử phân tích độ phức tạp thời gian của thuật toán này, sẽ thấy không dễ phân tích. Ta có thể viết hàm dp này thành mảng dp:

```python
dp[n][a_num][copy]
# Tổng số trạng thái (độ phức tạp thời-không gian) chính làthể tích của mảng ba chiều này
```

Ta biết biến `n` nhiều nhất tối đa là `N`, nhưng `a_num` và `copy` nhiều nhất tối đa bao nhiêu thì rất khó tính, độ phức tạpít nhất cũng O(N^3). Nên thuật toán này không tốt, độ phức tạp quá cao, mà đã không thể tối ưu nữa.

Điều này cũng cho thấy định nghĩa「trạng thái」như vậy không quáưu tú, dưới đây ta đổi ý tưởng định nghĩa dp khác.

### ý tưởng thứ hai

ý tưởng này hơi phức tạp một chút, nhưng hiệu quả cao. Tiếp tục đi quy trình, 「lựa chọn」vẫn là 4 cái đó, nhưng lần này ta chỉ định nghĩa một「trạng thái」, chính là số lần gõ còn lại `n`.

Thuật toán này dựa trên một sự thật là, **dãy phím tối ưu nhất định chỉ có hai trường hợp**:

Hoặc bấm mãi `A`: A, A, ... A (khi Nrelatively nhỏ).

Hoặc là dạng thế này: A, A, ... C-A, C-C, C-V, C-V, ... C-V (khi N tương đối lớn ).

Vì khi số ký tự ít ( N tương đối nhỏ ), một bộ thao tác `C-A C-C C-V` giá tương đối cao, có thể không bằng bấm từng chữ `A`; mà khi N tương đối lớn , về sau thu hoạch của `C-V` chắc chắn rất lớn . Trong trường hợp này toàn bộ dãy thao tác đại khái là: **đầuliền bấm mấy chữ `A`, rồi tổ hợp `C-A C-C` nối một số `C-V`, rồi lại `C-A C-C` nối nếu số `C-V`, tuần hoàn tiếp**.

nói cách khác, lần bấm cuối cùng hoặc là `A` hoặc là `C-V`. rõ ràng điểm này, có thể thiết kế thuật toán qua hai trường hợp này:

```java
int[] dp = new int[N + 1];
// Định nghĩa: dp[i] cho biết sau i lần thao tác nhiều nhất hiển thị được bao nhiêu chữ A
for (int i = 0; i <= N; i++)
    dp[i] = max(
            lần này bấm phím A,
            lần này bấm C-V
        )
```

Với trường hợp「bấm phím `A`」, chính là màn hình ở trạng thái `i - 1` thêm một chữ A mà thôi, rất dễ ra kết quả:

```java
// Bấm phím A, thì hơn lần trước một chữ A mà thôi
dp[i] = dp[i - 1] + 1;
```
Nhưng, nếu muốn bấm `C-V`, còn phải xét trước đó `C-A C-C` ở đâu.

**Vừa nói rồi, dãy thao tác tối ưu nhất định là `C-A C-C` nối nếu số `C-V`, nên ta dùng biến `j` làm điểm bắt đầu của nếu số `C-V`**. Vậy 2 thao tác trước `j` hẳn là `C-A C-C`:

<!-- muliti_language -->
```java
public int maxA(int N) {
    int[] dp = new int[N + 1];
    dp[0] = 0;
    for (int i = 1; i <= N; i++) {
        // Bấm phím A
        dp[i] = dp[i - 1] + 1;
        for (int j = 2; j < i; j++) {
            // Chọn hết & copy dp[j-2], dán liên tục i - j lần
            // Tổng cộng trên màn hình có dp[j - 2] * (i - j + 1) chữ A
            dp[i] = Math.max(dp[i], dp[j - 2] * (i - j + 1));
        }
    }
    // Sau N lần gõ nhiều nhất có mấy chữ A?
    return dp[N];
}
```

Trong đó biến `j` trừ 2 là để chừa số lần thao tác cho `C-A C-C`, xem hình là hiểu:

![](https://labuladong.online/algo/images/4keyboard/1.jpg)

Như vậy, thuật toán nàythì hoàn thành, độ phức tạp thời gian O(N^2), độ phức tạp không gian O(N), cách giải này hẳn là khá hiệu quả.

### Tổng kết

Quy hoạch động khóthì khó ở tìm chuyển trạng thái, định nghĩa khác nhau có thể sinh logic chuyển trạng thái khác nhau, tuy cuối cùng đều ra kết quả đúng, nhưng hiệu quả có thể chênh lệch khổng lồ lớn .

Ôn lại cách giải thứ nhất, bài toán con trùng lặp đã loại bỏ, nhưng hiệu quả vẫn thấp, rốt cuộc thấp ở đâu?trừu tượng hóa khung đệ quy:

```python
def dp(n, a_num, copy):
    dp(n - 1, a_num + 1, copy), # A
    dp(n - 1, a_num + copy, copy), # C-V
    dp(n - 2, a_num, a_num) # C-A C-C
```

Xem logic liệt kê này, có thể xuất hiện dãy thao tác thế này `C-A C-C，C-A C-C...` hoặc `C-V,C-V,...`. nhưng kết quả của dãy thao tác này không tối ưu, mà tachẳng có cáchtránh việc xảy ra các trường hợp này, từ đó tăng thêm nhiều tính toán bài toán con không cần thiết.

Ôn lại cách giải thứ hai, ta nghĩ thêm một chút là có thể nghĩ tới, dãy tối ưu hẳn là dạng: `A,A..C-A,C-C,C-V,C-V..C-A,C-C,C-V..`.

Theo sự thật này, ta định nghĩa lại trạng thái, tìm lại chuyển trạng thái, về mặt logic giảm số bài toán con vô hiệu, từ đó nâng hiệu quả thuật toán.



<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Một chiêu quét sạch bài trộm nhà trên LeetCode](https://labuladong.online/algo/dynamic-programming/house-robber/)
 - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》đãxuất bản, followtài khoản công chúng xem chi tiết; trả lời「**full bộ**」ởhậu trường để tải PDF kèm theo và full bộ cày bài**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======

### javascript

[651. Bàn phím bốn phím](https://leetcode-cn.com/problems/4-keys-keyboard)

**1、 ý tưởng thứ nhất**

```js
let maxA = function (N) {
    // Bảng ghi nhớ
    let memo = {}

    let dp = function (n, a_num, copy) {
        if (n <= 0) {
            return a_num;
        }

        let key = n + ',' + a_num + ',' + copy
        // Tránh tính bài toán con trùng lặp
        if (memo[key] !== undefined) {
            return memo[key]
        }

        memo[key] = Math.max(
            dp(n - 1, a_num + 1, copy), // A
            dp(n - 1, a_num + copy, copy), // C-V
            dp(n - 2, a_num, a_num) // C-A C-C
        )

        return memo[key]
    }

    return dp(N, 0, 0)
}
```

**2、 ý tưởng thứ hai**

```js
var maxA = function (N) {
    let dp = new Array(N + 1);
    dp[0] = 0;
    for (let i = 1; i <= N; i++) {
        // Bấm phím A
        dp[i] = dp[i - 1] + 1;
        for (let j = 2; j < i; j++) {
            // Chọn hết & copy dp[j-2], dán liên tục i - j lần
            // Tổng cộng trên màn hình có dp[j - 2] * (i - j + 1) chữ A
            dp[i] = Math.max(dp[i], dp[j - 2] * (i - (j - 2) - 1));
        }
    }
    // Sau N lần gõ nhiều nhất có mấy chữ A?
    return dp[N];
}
```

