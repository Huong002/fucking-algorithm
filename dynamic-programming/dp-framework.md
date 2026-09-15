# Khung tư duy giải bài toán quy hoạch động



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo độc giả, website đã cho ra mắt [mục lục cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học các bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được bộ khung thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode bản Trung | Độ khó |
| :----: | :----: | :----: |
| [322. Coin Change](https://leetcode.com/problems/coin-change/) | [322. Đổi tiền lẻ](https://leetcode.cn/problems/coin-change/) | 🟠 |
| [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | [509. Số Fibonacci](https://leetcode.cn/problems/fibonacci-number/) | 🟢 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung duyệt cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
> - [Cấu trúc cây đa phân và khung duyệt](https://labuladong.online/algo/data-structure-basic/n-ary-tree-traverse-basic/)

> tip: Bài này có bản video: [Giải thích chi tiết khung công thức quy hoạch động](https://www.bilibili.com/video/BV1XV411Y7oE). Khuyên bạn theo dõi tài khoản Bilibili của mình, mình sẽ dẫn mọi người học các kỹ thuật thuật toán hơi khó bằng video.

Bài viết này là bản nâng cấp của bài [Giải thích chi tiết quy hoạch động](https://mp.weixin.qq.com/s/1V3aHVonWBEXlNUvK3S28w) nhiều năm trước của mình, mình đã bổ sung thêm nhiều nội dung chất lượng, hy vọng bài này sẽ trở thành một bộ 「kim chỉ nam」 để giải quyết quy hoạch động.

Bài toán quy hoạch động (Dynamic Programming) chắc hẳn khiến nhiều độc giả đau đầu, nhưng dạng bài này cũng là dạng nhiềukỹ thuật nhất, thú vị nhất. Cuốn sách này đã dùng cả một chương để viết về thuật toán này, tầm quan trọng của quy hoạch động cũng từ đó mà thấy được.

Bài này giải quyết mấy vấn đề:

Quy hoạch động là gì? Giải bài toán quy hoạch động cókỹ thuật nào? Học quy hoạch động ra sao?

Làm nhiều bài rồi sẽ phát hiện,kỹ thuật toán cũng chỉ có vàicông thức đó, các chương quy hoạch động tiếp theo của chúng ta đều dùng tư duy khung giải bài của bài này, nếu trong lòng bạn đã nắm chắc thì sẽ nhẹ nhàng hơn nhiều. Vì vậy bài này đặt ở chương đầu tiên, hy vọng trở thành kim chỉ nam giải quyết bài toán quy hoạch động, dưới đây là nội dung chất lượng.

Trước hết, **dạng chung của bài toán quy hoạch động là tìm giá trị tối ưu**. Quy hoạch động thực ra là một phương pháp tối ưu hóa của vận trù học, chỉ có điều ứng dụng nhiều trong các bài toán máy tính, ví dụ như bắt bạn tìm dãy con tăng dài nhất, khoảng cách chỉnh sửa nhỏ nhất, vân vân.

Đã là tìm giá trị tối ưu, vấn đề cốt lõi là gì? **Vấn đề cốt lõi khi giải quy hoạch động là liệt kê vét cạn**. Vì muốn tìm giá trị tối ưu, chắc chắn phải liệt kê ra mọi đáp án khả thi, rồi tìm giá trị tối ưu trong đó chứ.

Quy hoạch động đơn giản vậy sao, liệt kê vét cạn là xong? Các bài quy hoạch động tôi thấy đều rất khó mà!

Trước hết, tuy tư tưởng cốt lõi của quy hoạch động chính là liệt kê vét cạn để tìm giá trị tối ưu, nhưng bài toán thiên biến vạn hóa, liệt kê mọi nghiệm khả thi thực ra không hề dễ, đòi hỏi bạn thành thạo tư duy đệ quy, chỉ khi liệt kê được **「phương trình chuyển trạng thái」 đúng** thì mới liệt kê đúng được. Hơn nữa, bạn cần đánh giá bài toán thuật toán có **sở hữu 「cấu trúc con tối ưu」** hay không, có thể từ giá trị tối ưu của bài toán con suy ra giá trị tối ưu của bài toán gốc hay không. Ngoài ra, bài toán quy hoạch động **tồn tại 「bài toán con trùng lặp」**, nếu liệt kê brute-force thì hiệu suất rất thấp, nên bạn cần dùng 「bản ghi nhớ (memo)」 hoặc 「bảng DP」 để tối ưu quá trình liệt kê, tránh các phép tính không cần thiết.

Các khái niệm bài toán con trùng lặp, cấu trúc con tối ưu, phương trình chuyển trạng thái nêu trên chính là ba yếu tố của quy hoạch động. Cụ thể nghĩa là gì lát nữa sẽ lấy ví dụ giải thích chi tiết, nhưng trong bài toán thuật toán thực tế, viết ra phương trình chuyển trạng thái là khó nhất, đó cũng là lý do nhiều bạn thấy bài toán quy hoạch động khó, tôi sẽ đưa ra khung tư duy do mình tổng kết để hỗ trợ bạn suy nghĩ về phương trình chuyển trạng thái:

**Xác định rõ 「trạng thái」 -> xác định rõ 「lựa chọn」 -> định nghĩa ý nghĩa của mảng/hàm `dp`**.

Đi theocông thức trên, code lời giải cuối cùng sẽ có khung như sau:






```python
# Quy hoạch động đệ quy top-down (từ trên xuống)
def dp(trang_thai1, trang_thai2, ...):
    for lua_chon in tat_ca_lua_chon_co_the:
        # trạng thái lúc này đã thay đổi vì đã thực hiện lựa chọn
        result = tim_gia_tri_toi_uu(result, dp(trang_thai1, trang_thai2, ...))
    return result

# Quy hoạch động lặp bottom-up (từ dưới lên)
# khởi tạo base case
dp[0][0][...] = base case
# thực hiện chuyển trạng thái
for trang_thai1 in tat_ca_gia_tri_cua_trang_thai1:
    for trang_thai2 in tat_ca_gia_tri_cua_trang_thai2:
        for ...
            dp[trang_thai1][trang_thai2][...] = tim_gia_tri_toi_uu(lua_chon1, lua_chon2...)
```

Dưới đây dùng bài toán dãy Fibonacci và bài toán đổi tiền lẻ để giải thích chi tiết nguyên lý cơ bản của quy hoạch động. Bài trước chủ yếu giúp bạn hiểu thế nào là bài toán con trùng lặp (dãy Fibonacci khôngtìm giá trị tối ưu, nên nghiêm ngặt mà nói không phải bài toán quy hoạch động), bài sau chủ yếu tập trung vào cách liệt kê phương trình chuyển trạng thái.

## Một, dãy Fibonacci

Bài 509 trên LeetCode「Số Fibonacci」 chính là bài này, mong độc giả đừng chê ví dụ này đơn giản, **chỉ có ví dụ đơn giản mới giúp bạn tập trung toàn bộ tinh lực vào tư tưởng vàkỹ thuật chung đằng sau thuật toán, mà không bị những chi tiếtmơ hồ gây khó hiểu**. Muốn ví dụ khó, trong series quy hoạch động tiếp theo còn đầy.

### Đệ quy brute-force

Dạng toán học của dãy Fibonacci chính là đệ quy, viết thành code như sau:

```java
int fib(int N) {
    if (N == 1 || N == 2) return 1;
    return fib(N - 1) + fib(N - 2);
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-fib/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>



Cái này khỏi cần nói nhiều, thầy cô trên trường dạy đệ quy hình như đều lấy ví dụ này. Chúng ta cũng biết viết code kiểu này tuy gọn gàng dễ hiểu, nhưng cực kỳ kém hiệu quả, kém ở đâu? Giả sử n = 20, hãy vẽ cây đệ quy:

![](https://labuladong.online/algo/images/dynamic-programming/1.jpg)

> [!TIP]
> Hễ gặp bài toán cần đệ quy, tốt nhất đều vẽ cây đệ quy ra, điều này giúp ích rất lớn cho việc phân tích độ phức tạp thuật toán và tìm nguyên nhân khiến thuật toán kém hiệu quả.

Hiểu cây đệ quy này thế nào? Nghĩa là muốn tính bài toán gốc `f(20)`, tôi phải tính ra bài toán con `f(19)` và `f(18)` trước, rồi muốn tính `f(19)`, tôi lại phải tính ra bài toán con `f(18)` và `f(17)` trước, cứ thế. Cuối cùng gặp `f(1)` hoặc `f(2)` thì kết quả đã biết, có thể trả về trực tiếp, cây đệ quy không mọc xuống nữa.

**Tính độ phức tạp thời gian của thuật toán đệ quy thế nào? Chính là lấy số lượng bài toán con nhân với thời gian cần để giải một bài toán con**.

Trước hết tính số lượng bài toán con, tức tổng số node trong cây đệ quy. Hiển nhiên tổng số node của cây nhị phân ở cấp số mũ, nên số lượng bài toán con là O(2^n).

Rồi tính thời gian giải một bài toán con, trong thuật toán này không có vòng lặp, chỉ có một phép cộng `f(n - 1) + f(n - 2)`, thời gian là O(1).

Vì vậy độ phức tạp thời gian của thuật toán này là hai thứ nhân nhau, tức O(2^n), cấp số mũ, nổ tung.

Quan sát cây đệ quy, rất rõ ràng phát hiện ra nguyên nhân kém hiệu quả của thuật toán: tồn tại lượng lớn phép tính trùng lặp, ví dụ `f(18)` bị tính hai lần, mà bạn có thể thấy cây đệ quy lấy `f(18)` làm gốc này có quy mô khổng lồ, tính thừa một lần sẽ tốn thời gian khổng lồ. Huống hồ còn không chỉ một node `f(18)` bị tính trùng, nên thuật toán này cực kỳ kém hiệu quả.

Đây chính là tính chất đầu tiên của bài toán quy hoạch động: **bài toán con trùng lặp**. Dưới đây, chúng ta tìm cách giải quyết vấn đề này.






### Cách giải đệ quy có bản ghi nhớ

Xác định rõ vấn đề thực ra đã là giải quyết được một nửa vấn đề. Đã biết nguyên nhân tốn thời gian là tính trùng lặp, vậy chúng ta có thể tạo một 「bản ghi nhớ (memo)」, mỗi lần tính ra đáp án của một bài toán con nào đó đừng vội trả về, hãy ghi vào 「bản ghi nhớ」 rồi mới trả về; mỗi lần gặp một bài toán con hãy tra trong 「bản ghi nhớ」 trước, nếu phát hiện trước đây đã giải bài này rồi thì lấy đáp án ra dùng luôn, đừng tốn thời gian tính nữa.

Thường dùng một mảng làm 「bản ghi nhớ」 này, dĩ nhiên bạn cũng có thể dùng bảng băm (từ điển), tư tưởng đều như nhau.

```java
int fib(int N) {
    // khởi tạo toàn bộ bản ghi nhớ bằng 0
    int[] memo = new int[N + 1];
    // thực hiện đệ quy có bản ghi nhớ
    return dp(memo, N);
}

// đệ quy mang theo bản ghi nhớ
int dp(int[] memo, int n) {
    // base case
    if (n == 0 || n == 1) return n;
    // đã tính rồi, không cần tính lại
    if (memo[n] != 0) return memo[n];
    memo[n] = dp(memo, n - 1) + dp(memo, n - 2);
    return memo[n];
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-fib2/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>



Giờ vẽ cây đệ quy ra, bạn sẽ biết 「bản ghi nhớ」 rốt cuộc đã làm gì.

![](https://labuladong.online/algo/images/dynamic-programming/2.jpg)

Thực tế, thuật toán đệ quy có 「bản ghi nhớ」 đã biến một cây đệ quy tồn tại lượng dư thừa khổng lồ thành một đồ thị đệ quy không còn dư thừa thông qua 「cắt tỉa」, giảm mạnh số lượng bài toán con (tức các node trong đồ thị đệ quy).

![](https://labuladong.online/algo/images/dynamic-programming/3.jpg)

**Tính độ phức tạp thời gian của thuật toán đệ quy thế nào? Chính là lấy số lượng bài toán con nhân với thời gian cần để giải một bài toán con**.

Số lượng bài toán con, tức tổng số node trong đồ thị, vì thuật toán này không tồn tại phép tính dư thừa, bài toán con chính là `f(1)`, `f(2)`, `f(3)` ... `f(20)`, số lượng tỉ lệ thuận với quy mô đầu vào n = 20, nên số lượng bài toán con là O(n).

Thời gian giải một bài toán con, như trên, không có vòng lặp nào, thời gian là O(1).

Vì vậy độ phức tạp thời gian của thuật toán này là O(n), so với thuật toán brute-force thì đúng là đòn giảm chiều.






Đến đây, hiệu quả của cách giải đệ quy có bản ghi nhớ đã ngang với cách giải quy hoạch động lặp rồi. Thực tế cách giải này với cách giải quy hoạch động thường gặp đã gần như nhau, chỉ có điều cách giải này 「từ trên xuống」 thực hiện giải bằng 「đệ quy」, còn code quy hoạch động thường gặp hơn là 「từ dưới lên」 thực hiện giải bằng 「lặp」.

Gì gọi là 「từ trên xuống」? Chú ý cây đệ quy (hay đồ thị) chúng ta vừa vẽ, đều kéo dài từ trên xuống dưới, đều là từ một bài toán gốc quy mô lớn ví dụ `f(20)`, dần dần phân rã quy mô xuống, đến hai base case `f(1)` và `f(2)` rồi trả về đáp án từng lớp, cái này gọi là 「từ trên xuống」.

Gì gọi là 「từ dưới lên」? Ngược lại, chúng ta xuất phát trực tiếp từ `f(1)` và `f(2)` (base case) ở dưới cùng, đơn giản nhất, quy mô bài toán nhỏ nhất, đã biết kết quả,tính dần lên trên, đến khi tính ra đáp án `f(20)` mà chúng ta muốn. Đây chính là hướng suy nghĩ của 「lặp」, cũng là lý do quy hoạch động thường thoát ly khỏi đệ quy mà do vòng lặp hoàn thành tính toán.

### Cách giải lặp với mảng `dp`

Có gợi ý từ 「bản ghi nhớ」 ở bước trước, chúng ta có thể tách 「bản ghi nhớ」 này ra thành một bảng, thường gọi là bảng DP (DP table), hoàn thành tính toán 「từ dưới lên」 trên bảng này thì tuyệt vời còn gì!

```java
int fib(int N) {
    if (N == 0) return 0;
    int[] dp = new int[N + 1];
    // base case
    dp[0] = 0; dp[1] = 1;
    // chuyển trạng thái
    for (int i = 2; i <= N; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[N];
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-fib3/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>



Vẽ hình ra là hiểu ngay, mà bạn sẽ phát hiện bảng DP này đặc biệt giống kết quả sau khi 「cắt tỉa」 trước đó, chỉ có điều tính ngược lại mà thôi:

![](https://labuladong.online/algo/images/dynamic-programming/4.jpg)

Thực tế, mảng `memo` 「bản ghi nhớ」 trong cách giải đệ quy có bản ghi nhớ, sau khi hoàn thành chính là mảng `dp` trong cách giải này, bạn đối chiếu quá trình thực thi của hai thuật toán trong khung trực quan hóa trực quan hóa có thể thấy rõ hơn mối liên hệ của chúng.

Vì vậy hai cách giải từ trên xuống, từ dưới lên bản chất thực ra không khác nhau, phần lớn trường hợp hiệu quả cũng cơ bản giống nhau.






### Mở rộng thêm

Ở đây dẫn ra danh từ 「phương trình chuyển trạng thái」, thực tế chính là dạng toán học mô tả cấu trúc bài toán:

![](https://labuladong.online/algo/images/dynamic-programming/fib.png)

Sao gọi là 「phương trình chuyển trạng thái」? Thực ra là để nghe cho cao siêu mà thôi.

Tham số hàm `f(n)` sẽ không ngừng thay đổi, nên bạn coi tham số `n` như một trạng thái, trạng thái `n` này là do trạng thái `n - 1` và trạng thái `n - 2` chuyển (cộng lại) mà thành, cái này gọi là chuyển trạng thái, chỉ có vậy thôi.

Bạn sẽ phát hiện mọi thao tác trong các cách giải trên, ví dụ `return f(n - 1) + f(n - 2)`, `dp[i] = dp[i - 1] + dp[i - 2]`, cùng các thao tác khởi tạo bản ghi nhớ hay bảng DP, đều xoay quanh các dạng thể hiện khác nhau của phương trình này.

Có thể thấy tầm quan trọng của việc liệt kê 「phương trình chuyển trạng thái」, nó là cốt lõi giải quyết vấn đề, mà cũng dễ phát hiện,thực ra phương trình chuyển trạng thái trực tiếp đại diện cho cách giải brute-force.

**Ngàn vạn lần đừng coi thường cách giải brute-force, cái khó nhất của bài toán quy hoạch động chính là viết ra cách giải brute-force này, tức phương trình chuyển trạng thái**.

Chỉ cần viết ra cách giải brute-force, phương pháp tối ưu chẳng qua là dùng bản ghi nhớ hoặc bảng DP, chẳng còn điều huyền bí gì nữa.

Cuối ví dụ này, giảng một chi tiết tối ưu.

Độc giả tinh ý sẽ phát hiện, theo phương trình chuyển trạng thái của dãy Fibonacci, trạng thái hiện tại `n` chỉ liên quan đến hai trạng thái trước đó `n-1, n-2`, thực ra không cần một bảng DP dài như vậy để lưu mọi trạng thái, chỉ cần tìm cách lưu hai trạng thái trước đó là được.

Vì vậy có thể tối ưu thêm một bước, giảm độ phức tạp không gian xuống O(1). Đây cũng là thuật toán tính số Fibonacci thường gặp nhất của chúng ta:

```java
int fib(int n) {
    if (n == 0 || n == 1) {
        // base case
        return n;
    }
    // lần lượt đại diện cho dp[i - 1] và dp[i - 2]
    int dp_i_1 = 1, dp_i_2 = 0;
    for (int i = 2; i <= n; i++) {
        // dp[i] = dp[i - 1] + dp[i - 2];
        int dp_i = dp_i_1 + dp_i_2;
        // cập nhật lăn (rolling update)
        dp_i_2 = dp_i_1;
        dp_i_1 = dp_i;
    }
    return dp_i_1;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/fibonacci-number/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>



Đây thường là bước tối ưu cuối cùng của bài toán quy hoạch động, nếu phát hiện mỗi lần chuyển trạng thái chỉ cần một phần trong bảng DP, vậy có thể thử thu nhỏ kích thước bảng DP, chỉ ghi lại dữ liệu cần thiết, từ đó giảm độ phức tạp không gian.

Ví dụ trên tương đương với thu kích thước bảng DP từ `n` xuống 2, tức giảm độ phức tạp không gian đi một lượng cấp. Ở phần sau [Tuyệt chiêu nén chiều cho quy hoạch động](https://labuladong.online/algo/dynamic-programming/space-optimization/) mình sẽ giảng tiếpkỹ thuật nén độ phức tạp không gian này, thường dùng để biến một bảng DP hai chiều nén thành một chiều, tức nén độ phức tạp không gian từ O(n^2) xuống O(n).

Có người sẽ hỏi, đặc tính quan trọng khác của quy hoạch động là 「cấu trúc con tối ưu」, sao không đề cập? Dưới đây sẽ đề cập. Ví dụ dãy Fibonacci nghiêm ngặt mà nói không tính là quy hoạch động, vì không liên quan tới việc tìm giá trị tối ưu, trên đây nhằm minh họa phương pháp loại bỏ bài toán con trùng lặp, minh họa quá trình tinh chỉnh từng bước để có được nghiệm tối ưu. Dưới đây xem ví dụ thứ hai, bài toán đổi tiền lẻ.

## Hai, bài toán đổi tiền lẻ

Đây là bài 322 trên LeetCode「Đổi tiền lẻ」:

Cho bạn `k` loại đồng xu với mệnh giá lần lượt là `c1, c2 ... ck`, số lượng mỗi loại xu là vô hạn, lại cho một tổng số tiền `amount`, hỏi bạn **ít nhất** cần mấy đồng xu để đổi ra số tiền này, nếu không thể đổi được, thuật toán trả về -1. Chữ ký hàm thuật toán như sau:

```java
// trong coins là các mệnh giá xu có thể chọn, amount là số tiền mục tiêu
int coinChange(int[] coins, int amount);
```

Ví dụ `k = 3`, mệnh giá lần lượt là 1, 2, 5, tổng số tiền `amount = 11`. Vậy ít nhất cần 3 đồng xu để đổi, tức 11 = 5 + 5 + 1.

Bạn nghĩ máy tính nên giải bài này thế nào? Hiển nhiên là liệt kê ra mọi cách đổi xu có thể, rồi tìm xem ít nhất cần bao nhiêu đồng xu.

### Đệ quy brute-force

Trước hết, bài này là bài toán quy hoạch động, vì nó có 「cấu trúc con tối ưu」. **Muốn phù hợp 「cấu trúc con tối ưu」, các bài toán con phải độc lập với nhau**. Gì gọi là độc lập? Bạn chắc không muốn xem chứng minh toán học, tôi dùng một ví dụ trực quan để giảng giải.

Ví dụ, giả sử bạn thi cử, thành tích mỗi môn đều độc lập với nhau. Bài toán gốc của bạn là thi được tổng thành tích cao nhất, vậy bài toán con của bạn là phải thi môn Văn cao nhất, thi môn Toán cao nhất... Để mỗi môn thi cao nhất, bạn phải giành được điểm phần trắc nghiệm cao nhất của môn tương ứng, điểm phần điền khuyết cao nhất... Dĩ nhiên cuối cùng chính là mỗi môn của bạn đều điểm tối đa, đây chính là tổng thành tích cao nhất.

Được kết quả đúng: tổng thành tích cao nhất chính là tổng điểm. Vì quá trình này phù hợp cấu trúc con tối ưu, các bài toán con 「mỗi môn thi cao nhất」 này độc lập, không can thiệp lẫn nhau.

Nhưng nếu thêm một điều kiện: thành tích môn Văn và môn Toán của bạnràng buộc lẫn nhau, không thể đồng thời điểm tối đa, điểm Toán cao thì điểm Văn sẽ thấp, ngược lại cũng vậy.

Như vậy, hiển nhiên tổng thành tích cao nhất bạn thi được sẽ không đạt tổng điểm,theo hướng suy nghĩ vừa rồi sẽ cho kết quả sai. Vì các bài toán con 「mỗi môn thi cao nhất」 không độc lập, thành tích Văn Toán ảnh hưởng lẫn nhau, không thể đồng thời tối ưu, nên cấu trúc con tối ưu bị phá vỡ.

Quay lại bài toán đổi tiền lẻ, vì sao nói nó phù hợp cấu trúc con tối ưu? Giả sử bạn có xu mệnh giá `1, 2, 5`, bạn muốn tìm số xu ít nhất khi `amount = 11` (bài toán gốc), nếu bạn biết số xu ít nhất để đổi ra `amount = 10, 9, 6` (bài toán con), bạn chỉ cần cộng một vào đáp án bài toán con (chọn thêm một đồng xu mệnh giá `1, 2, 5`), tìm giá trị nhỏ nhất, chính là đáp án bài toán gốc. Vì số lượng xu không giới hạn, nên giữa các bài toán con không ràng buộc lẫn nhau, độc lập với nhau.






> [!TIP]
> Về vấn đề cấu trúc con tối ưu, phần sau [Bài giải đáp quy hoạch động](https://labuladong.online/algo/dynamic-programming/faq-summary/) sẽ còn lấy ví dụ thảo luận tiếp.

Vậy thì, đã biết đây là bài toán quy hoạch động, phải suy nghĩ làm sao liệt kê phương trình chuyển trạng thái đúng?

**1. Xác định 「trạng thái」, tức là các đại lượng biến đổi trong bài toán gốc và bài toán con**. Vì số lượng xu vô hạn, mệnh giá xu cũng là đề bài cho sẵn, chỉ có số tiền mục tiêu là không ngừng tiến về base case, nên 「trạng thái」 duy nhất chính là số tiền mục tiêu `amount`.

**2. Xác định 「lựa chọn」, tức là hành vi khiến 「trạng thái」 thay đổi**. Số tiền mục tiêu vì sao thay đổi? Vì bạn đang chọn xu, mỗi lần bạn chọn một đồng xu thì tương đương với giảm số tiền mục tiêu. Vì vậy mọi mệnh giá xu chính là 「lựa chọn」 của bạn.

**3. Làm rõ định nghĩa của hàm/mảng `dp`**. Chỗ này nói về cách giải từ trên xuống, nên sẽ có một hàm `dp` đệ quy, thường thì tham số hàm chính là đại lượng biến đổi trong chuyển trạng thái, tức 「trạng thái」 nói trên; giá trị trả về của hàm chính là đại lượng đề bài yêu cầu chúng ta tính. Với bài này mà nói, trạng thái chỉ có một, tức 「số tiền mục tiêu」, đề bài yêu cầu chúng ta tính số xu ít nhất cần để đổi ra số tiền mục tiêu.

**Vì vậy chúng ta có thể định nghĩa hàm `dp` như sau: `dp(n)` có nghĩa là: nhập vào một số tiền mục tiêu `n`, trả về số xu ít nhất cần để đổi ra số tiền mục tiêu `n`**.

Vậy theo định nghĩa này, đáp án cuối cùng của chúng ta chính là giá trị trả về của `dp(amount)`.

Hiểu rõ mấy điểm then chốt trên là có thể viết ra mã giả của cách giải:

```java
// khung mã giả
int coinChange(int[] coins, int amount) {
    // kết quả cuối cùng đề bài yêu cầu là dp(amount)
    return dp(coins, amount);
}

// định nghĩa: muốn đổi được số tiền n, cần ít nhất dp(coins, n) đồng xu
int dp(int[] coins, int n) {
    // thực hiện lựa chọn, chọn kết quả cần ít xu nhất
    for (int coin : coins) {
        res = min(res, 1 + dp(coins, n - coin));
    }
    return res;
}
```

Theo mã giả, thêm base case vào là được đáp án cuối cùng. Hiển nhiên khi số tiền mục tiêu là 0, số xu cần là 0; khi số tiền mục tiêu nhỏ hơn 0, vô nghiệm, trả về -1:

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        // kết quả cuối cùng đề bài yêu cầu là dp(amount)
        return dp(coins, amount);
    }

    // định nghĩa: muốn đổi được số tiền n, cần ít nhất dp(coins, n) đồng xu
    int dp(int[] coins, int amount) {
        // base case
        if (amount == 0) return 0;
        if (amount < 0) return -1;

        int res = Integer.MAX_VALUE;
        for (int coin : coins) {
            // tính kết quả của bài toán con
            int subProblem = dp(coins, amount - coin);
            // bài toán con vô nghiệm thì bỏ qua
            if (subProblem == -1) continue;
            // chọn nghiệm tối ưu trong các bài toán con, rồi cộng thêm một
            res = Math.min(res, subProblem + 1);
        }

        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
```

> [!NOTE]
> Ở đây chữ ký của hàm `coinChange` và `dp` hoàn toàn giống nhau, nên về lý thuyết không cần viết thêm một hàm `dp` nữa. Nhưng để tiện giảng ở phần sau, chỗ này vẫn viết thêm một hàm `dp` đểcài đặt logic chính.
>
> Ngoài ra, tôi thường thấy độc giả để lại lời nhắn hỏi, vì sao kết quả bài toán con phải cộng 1 (`subProblem + 1`), mà không phải cộng mệnh giá xu các kiểu. Tôi nêu rõ ở đây để thống nhất một chút, then chốt của bài toán quy hoạch động là định nghĩa của hàm/mảng `dp`, giá trị trả về của hàm này đại diện cho gì? Bạn quay lạilàm rõ điểm này, rồi sẽ biết vì sao phải cộng 1 vào giá trị trả về của bài toán con.


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/coin-change-brute-force/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>

Đến đây, phương trình chuyển trạng tháithực ra đã hoàn thành, thuật toán trên đã là cách giải brute-force, dạng toán học của code trên chính là phương trình chuyển trạng thái:

![](https://labuladong.online/algo/images/dynamic-programming/coin.png)

Đến đây, bài này thực ra đã giải xong, chỉ có điều cần loại bỏ bớt bài toán con trùng lặp, ví dụ khi `amount = 11, coins = {1,2,5}`, vẽ cây đệ quy xem:

![](https://labuladong.online/algo/images/dynamic-programming/5.jpg)

**Phân tích độ phức tạp thời gian của thuật toán đệ quy: tổng số bài toán con x thời gian cần để giải mỗi bài toán con**.

Tổng số bài toán con là số node của cây đệ quy, nhưng thuật toán sẽ cắt tỉa, thời điểm cắt tỉa liên quan đến mệnh giá xu cụ thể đề bài cho, nên có thể tưởng tượng cây này mọc không theo quy luật, tính chính xác trên cây có bao nhiêu node là khá khó khăn. Với tình huống này, cách làm thường của chúng ta là theo tình huống xấu nhất ước lượng một cận trên của độ phức tạp thời gian.

Giả sử số tiền mục tiêu là `n`, số xu cho sẵn là `k`, vậy cây đệ quy trong tình huống xấu nhất cao là `n` (toàn dùng xu mệnh giá 1), rồi giả sử đây là một cây `k` phân đầy, thì tổng số node ở lượng cấp `k^n`.

Tiếp theo xem độ phức tạp của mỗi bài toán con, vì mỗi lần đệ quy chứa một vòng for, độ phức tạp là $O(k)$, nhân nhau được tổng độ phức tạp thời gian là $O(k^n)$, cấp số mũ.

### Đệ quy có bản ghi nhớ

Tương tự ví dụ dãy Fibonacci trước đó, chỉ cần sửa đổi một chút là có thể thông qua bản ghi nhớ loại bỏ bài toán con:

```java
class Solution {
    int[] memo;

    public int coinChange(int[] coins, int amount) {
        memo = new int[amount + 1];
        // khởi tạo bản ghi nhớ bằng một giá trị đặc biệt không bao giờ gặp, nghĩa là chưa được tính
        Arrays.fill(memo, -666);

        return dp(coins, amount);
    }

    int dp(int[] coins, int amount) {
        if (amount == 0) return 0;
        if (amount < 0) return -1;
        // tra bản ghi nhớ để tránh tính trùng
        if (memo[amount] != -666)
            return memo[amount];

        int res = Integer.MAX_VALUE;
        for (int coin : coins) {
            // tính kết quả của bài toán con
            int subProblem = dp(coins, amount - coin);
            // bài toán con vô nghiệm thì bỏ qua
            if (subProblem == -1) continue;
            // chọn nghiệm tối ưu trong các bài toán con, rồi cộng thêm một
            res = Math.min(res, subProblem + 1);
        }
        // lưu kết quả đã tính vào bản ghi nhớ
        memo[amount] = (res == Integer.MAX_VALUE) ? -1 : res;
        return memo[amount];
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/coin-change/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🥳 Animation trực quan hóa code🥳</strong>
</summary>
</details>
</a>
<hr/>



Không vẽ hình nữa, rất hiển nhiên 「bản ghi nhớ」 giảm mạnh số lượng bài toán con, loại bỏ hoàn toàn dư thừa của bài toán con, nên tổng số bài toán con sẽ không vượt quá số tiền `n`, tức số lượng bài toán con là $O(n)$. Thời gian xử lý một bài toán con không đổi, vẫn là $O(k)$, nên tổng độ phức tạp thời gian là $O(kn)$.

### Cách giải lặp với mảng dp

Dĩ nhiên, chúng ta cũng có thể từ dưới lên dùng bảng dp để loại bỏ bài toán con trùng lặp, về 「trạng thái」「lựa chọn」 và base case với trước đó không khác biệt, định nghĩa của mảng `dp` với hàm `dp` vừa rồi tương tự, cũng là lấy 「trạng thái」, tức số tiền mục tiêu, làm biến. Chỉ có điều hàm `dp` thể hiện ở tham số hàm, còn mảng `dp` thể hiện ở chỉ số mảng:

**Định nghĩa của mảng `dp`: khi số tiền mục tiêu là `i`, ít nhất cần `dp[i]` đồng xu để đổi**.

Theo khung code quy hoạch động đưa ra ở đầu bài viết có thể viết ra cách giảinhư sau:

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        // kích thước mảng là amount + 1, giá trị khởi tạo cũng là amount + 1
        Arrays.fill(dp, amount + 1);

        // base case
        dp[0] = 0;
        // vòng for ngoài duyệt mọi giá trị của mọi trạng thái
        for (int i = 0; i < dp.length; i++) {
            // vòng for trong tìm giá trị nhỏ nhất trong mọi lựa chọn
            for (int coin : coins) {
                // bài toán con vô nghiệm, bỏ qua
                if (i - coin < 0) {
                    continue;
                }
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
        return (dp[amount] == amount + 1) ? -1 : dp[amount];
    }
}
```

> [!NOTE]
> Vì sao các giá trị trong mảng `dp` đều khởi tạo là `amount + 1`, vì số xu để đổi thành số tiền `amount` nhiều nhất chỉ có thể bằng `amount` (toàn dùng xu mệnh giá 1), nên khởi tạo là `amount + 1` thì tương đương với khởi tạo là vô cực dương, tiện cho việc lấy giá trị nhỏ nhất sau này. Vì sao không trực tiếp khởi tạo là giá trị lớn nhất kiểu int là `Integer.MAX_VALUE`? Vì phía sau có `dp[i - coin] + 1`, cái này sẽ dẫn tới tràn số nguyên.

![](https://labuladong.online/algo/images/dynamic-programming/6.jpg)

## Ba, tổng kết cuối cùng

Bài toán dãy Fibonacci đầu tiên giải thích cách thông qua phương pháp 「bản ghi nhớ」 hoặc 「bảng dp」 để tối ưu cây đệ quy, và làm rõ hai phương pháp này bản chất là như nhau, chỉ là khác biệt từ trên xuống và từ dưới lên mà thôi.

Bài toán đổi tiền lẻ thứ hai trình bày cách xác định theo quy trình 「phương trình chuyển trạng thái」, chỉ cần thông qua phương trình chuyển trạng thái viết ra cách giải đệ quy brute-force, còn lại cũng chỉ là tối ưu cây đệ quy, loại bỏ bài toán con trùng lặp mà thôi.

Nếu bạn chưa hiểu rõ quy hoạch động mà còn đọc được tới đây, thật sự phải vỗ tay cho bạn, tin rằng bạn đã nắm vữngkỹ thuật thiết kế của thuật toán này.

**Máy tính giải quyết vấn đề thực ra không có kỹ thuật đặc biệt nào, cách giải duy nhất của nó chính là liệt kê vét cạn**, liệt kê mọi khả năng. Thiết kế thuật toán chẳng qua là nghĩ trước “liệt kê thế nào”, rồi mới theo đuổi “liệt kê thông minh thế nào”.

Liệt kê phương trình chuyển trạng thái chính là đang giải quyết vấn đề “liệt kê thế nào”. Sở dĩ nói nó khó, một là vì nhiều phép liệt kê cần cài đặt bằng đệ quy, hai là vì không gian nghiệm của có bài toánbản thân phức tạp, không dễ liệt kê đầy đủ.

Bản ghi nhớ, bảng DP chính là đang theo đuổi “liệt kê thông minh thế nào”. Dùng tư duy đánh đổi không gian lấy thời gian, đó là con đường không thể khác để giảm độ phức tạp thời gian, ngoài ra, thử hỏi còn có thể bày ra trò gì nữa?

Sau này chúng ta sẽ có một chương giảng riêng bài toán quy hoạch động, nếu có bất kỳ vấn đề gì lúc nào cũng có thể quay lại đọc đọc lại bài này, hy vọng độc giả khi đọc mỗiđề bài và cách giải, hãy luôn bám vào 「trạng thái」 và 「lựa chọn」, mới có thể hình thành sự thấu hiểu của riêng mình về bộ khung này, vận dụng nhuần nhuyễn.






<hr>
<details class="hint-container details">
<summary><strong>Các bài viết trích dẫn bài này</strong></summary>

  - [Định base case và giá trị khởi tạo của bản ghi nhớ thế nào?](https://labuladong.online/algo/dynamic-programming/memo-fundamental/)
  - [【Bài luyện tăng cường】Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
  - [【Bài luyện tăng cường】Bài tập kinh điển thuật toán tìm kiếm nhị phân](https://labuladong.online/algo/problem-set/binary-search/)
  - [【Bài luyện tăng cường】Cách cài đặt chung của hàng đợi đơn điệu và bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-queue/)
  - [【Bài luyện tăng cường】Dùng đồng thời hai lối tư duy giải bài](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view/)
  - [【Bài luyện tăng cường】Bài tập liên quan thủ thuật toán học](https://labuladong.online/algo/problem-set/math-tricks/)
  - [Một chiêu quét sạch bài trộm nhà trên LeetCode](https://labuladong.online/algo/dynamic-programming/house-robber/)
  - [Một chiêu quét sạch bài mua bán cổ phiếu trên LeetCode](https://labuladong.online/algo/dynamic-programming/stock-problem-summary/)
  - [Cơ bản về cây nhị phân và các loại thường gặp](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
  - [Cương lĩnh cốt lõi của series thuật toán cây nhị phân](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
  - [Khung công thức giải bài bằng thuật toán chia để trị](https://labuladong.online/algo/essential-technique/divide-and-conquer/)
  - [Mẫu giải bài toán dãy con trong quy hoạch động](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)
  - [Quy hoạch động: tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/)
  - [Chuyển đổi tư duy giữa quy hoạch động và thuật toán quay lui](https://labuladong.online/algo/dynamic-programming/word-break/)
  - [Quy hoạch động giúp tôi phá đảo Fallout 4](https://labuladong.online/algo/dynamic-programming/freedom-trail/)
  - [Quy hoạch động giúp tôi phá đảo Tháp Ma thuật](https://labuladong.online/algo/dynamic-programming/magic-tower/)
  - [Hai góc nhìn liệt kê của quy hoạch động](https://labuladong.online/algo/dynamic-programming/two-views-of-dp/)
  - [Thiết kế quy hoạch động: mảng con lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/)
  - [Thiết kế quy hoạch động: dãy con tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/)
  - [Tư duy khung khi học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
  - [Tuyệt chiêu nén chiều cho quy hoạch động](https://labuladong.online/algo/dynamic-programming/space-optimization/)
  - [Mở rộng: giải thích chi tiết sắp xếp trộn và ứng dụng](https://labuladong.online/algo/practice-in-action/merge-sort/)
  - [Đại pháp tiết kiệm tiền du lịch: đường đi ngắn nhất có trọng số](https://labuladong.online/algo/dynamic-programming/cheap-travel/)
  - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)
  - [Học thuật toán và trải nghiệm flow](https://labuladong.online/algo/fname.html?fname=心流)
  - [Hướng dẫn thực dụng phân tích độ phức tạp thời gian–không gian thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/)
  - [công thức「lấy điểm mò」 khi thi viết thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)
  - [Quy hoạch động kinh điển: bài toán ba lô 0-1](https://labuladong.online/algo/dynamic-programming/knapsack1/)
  - [Quy hoạch động kinh điển: bài toán trò chơi đối kháng](https://labuladong.online/algo/dynamic-programming/game-theory/)
  - [Quy hoạch động kinh điển: bài toán ba lô tập con](https://labuladong.online/algo/dynamic-programming/knapsack2/)
  - [Quy hoạch động kinh điển: bài toán ba lô đầy đủ](https://labuladong.online/algo/dynamic-programming/knapsack3/)
  - [Quy hoạch động kinh điển: chọc bóng bay](https://labuladong.online/algo/dynamic-programming/burst-balloons/)
  - [Quy hoạch động kinh điển: dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/)
  - [Quy hoạch động kinh điển: biểu thức chính quy](https://labuladong.online/algo/dynamic-programming/regular-expression-matching/)
  - [Quy hoạch động kinh điển: khoảng cách chỉnh sửa](https://labuladong.online/algo/dynamic-programming/edit-distance/)
  - [Quy hoạch động kinh điển: thả trứng nhà cao tầng](https://labuladong.online/algo/dynamic-programming/egg-drop/)
  - [Thuật toán đổ xăng của tài xế lão luyện](https://labuladong.online/algo/frequency-interview/gas-station-greedy/)
  - [Biến thể bài toán ba lô: tổng mục tiêu](https://labuladong.online/algo/dynamic-programming/target-sum/)
  - [Khung công thức giải bài bằng thuật toán tham lam](https://labuladong.online/algo/essential-technique/greedy/)

</details><hr>





<hr>
<details class="hint-container details">
<summary><strong>Cácđề bài trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài trên Chrome của tôi](https://labuladong.online/algo/intro/chrome/) rồi mở cácđề bài dưới đây là xem được ngay hướng giải:</strong>

| LeetCode | LeetCode bản Trung | Độ khó |
| :----: | :----: | :----: |
| [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/?show=1) | [111. Độ sâu nhỏ nhất của cây nhị phân](https://leetcode.cn/problems/minimum-depth-of-binary-tree/?show=1) | 🟢 |
| [112. Path Sum](https://leetcode.com/problems/path-sum/?show=1) | [112. Tổng đường đi](https://leetcode.cn/problems/path-sum/?show=1) | 🟢 |
| [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/?show=1) | [115. Các dãy con khác nhau](https://leetcode.cn/problems/distinct-subsequences/?show=1) | 🔴 |
| [139. Word Break](https://leetcode.com/problems/word-break/?show=1) | [139. Tách từ](https://leetcode.cn/problems/word-break/?show=1) | 🟠 |
| [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/?show=1) | [1696. Trò chơi nhảy VI](https://leetcode.cn/problems/jump-game-vi/?show=1) | 🟠 |
| [221. Maximal Square](https://leetcode.com/problems/maximal-square/?show=1) | [221. Hình vuông lớn nhất](https://leetcode.cn/problems/maximal-square/?show=1) | 🟠 |
| [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/?show=1) | [240. Tìm kiếm ma trận 2D II](https://leetcode.cn/problems/search-a-2d-matrix-ii/?show=1) | 🟠 |
| [256. Paint House](https://leetcode.com/problems/paint-house/?show=1)🔒 | [256. Sơn nhà](https://leetcode.cn/problems/paint-house/?show=1)🔒 | 🟠 |
| [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/?show=1) | [279. Số chính phương hoàn hảo](https://leetcode.cn/problems/perfect-squares/?show=1) | 🟠 |
| [343. Integer Break](https://leetcode.com/problems/integer-break/?show=1) | [343. Tách số nguyên](https://leetcode.cn/problems/integer-break/?show=1) | 🟠 |
| [365. Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/?show=1) | [365. Bài toán ấm nước](https://leetcode.cn/problems/water-and-jug-problem/?show=1) | 🟠 |
| [542. 01 Matrix](https://leetcode.com/problems/01-matrix/?show=1) | [542. Ma trận 01](https://leetcode.cn/problems/01-matrix/?show=1) | 🟠 |
| [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths/?show=1) | [576. Số đường đi ra ngoài biên](https://leetcode.cn/problems/out-of-boundary-paths/?show=1) | 🟠 |
| [62. Unique Paths](https://leetcode.com/problems/unique-paths/?show=1) | [62. Đường đi khác nhau](https://leetcode.cn/problems/unique-paths/?show=1) | 🟠 |
| [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/?show=1) | [63. Đường đi khác nhau II](https://leetcode.cn/problems/unique-paths-ii/?show=1) | 🟠 |
| [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/?show=1) | [70. Leo cầu thang](https://leetcode.cn/problems/climbing-stairs/?show=1) | 🟢 |
| [91. Decode Ways](https://leetcode.com/problems/decode-ways/?show=1) | [91. Cách giải mã](https://leetcode.cn/problems/decode-ways/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 04. Tra cứu trong mảng 2D](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 10- I. Dãy Fibonacci](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 10- II. Bài toán ếch nhảy bậc thang](https://leetcode.cn/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 14- I. Cắt dây](https://leetcode.cn/problems/jian-sheng-zi-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 46. Dịch số thành chuỗi](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 091. Sơn nhà](https://leetcode.cn/problems/JEj789/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 097. Số lượng dãy con](https://leetcode.cn/problems/21dk04/?show=1) | 🔴 |
| - | [Kiếm Chỉ Offer II 098. Số lượng đường đi](https://leetcode.cn/problems/2AoeFn/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 103. Số xu ít nhất](https://leetcode.cn/problems/gaM7Ch/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
