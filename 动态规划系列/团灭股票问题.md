# Một chiêu quét sạch bài mua bán cổ phiếu trên LeetCode



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo độc giả, website đã cho ra mắt [mục lục cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học các bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được bộ khung thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode bản Trung | Độ khó |
| :----: | :----: | :----: |
| [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) | [121. Thời điểm mua bán cổ phiếu tốt nhất](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) | 🟢 |
| [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) | [122. Thời điểm mua bán cổ phiếu tốt nhất II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/) | 🟠 |
| [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) | [123. Thời điểm mua bán cổ phiếu tốt nhất III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/) | 🔴 |
| [188. Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) | [188. Thời điểm mua bán cổ phiếu tốt nhất IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/) | 🔴 |
| [309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | [309. Thời điểm mua bán cổ phiếu tốt nhất có kỳ đóng băng](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | 🟠 |
| [714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) | [714. Thời điểm mua bán cổ phiếu tốt nhất có phí giao dịch](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Nhiều độc giả phàn nàn rằng các cách giải cho series bài cổ phiếu trên LeetCode quá nhiều, nếu khi phỏng vấn thật sự gặp dạng bài này thì cơ bản không nghĩ ra được những cách khéo léo đó, phải làm sao? **Vì vậy bài này không giảng những hướng suy nghĩ quá khéo léo, mà đánh chắc tiến chắc, chỉ dùng một phương pháp chung để giải mọi bài, lấy bất biến ứng vạn biến**.

Bài viết này tham khảo hướng suy nghĩ của [lời giải được vote cao bản tiếng Anh](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/discuss/108870/Most-consistent-ways-of-dealing-with-the-series-of-stock-problems), dùng kỹ thuật máy trạng thái để giải, có thể nộp qua hết. Đừng thấy danh từ này cao siêu, chỉ là từ ngữ văn chương mà thôi, thực chất chính là bảng DP, liếc mắt là hiểu ngay.

Cứ rút bừa một bài ra, xem cách giải của người khác:





```java
int maxProfit(int[] prices) {
    if(prices.empty()) return 0;
    int s1 = -prices[0], s2 = INT_MIN, s3 = INT_MIN, s4 = INT_MIN;

    for(int i = 1; i < prices.size(); ++i) {            
        s1 = max(s1, -prices[i]);
        s2 = max(s2, s1 + prices[i]);
        s3 = max(s3, s2 - prices[i]);
        s4 = max(s4, s3 + prices[i]);
    }
    return max(0, s4);
}
```



Nhìn có hiểu không? Biết làm chưa? Không thể nào, bạn nhìn không hiểu mới là bình thường. Dù bạn có miễn cưỡng hiểu được, bài tiếp theo bạn vẫn làm không ra. Vì sao người khác viết được cách giải quái dị mà hiệu quả như vậy? Vì dạng bài này là có khung, nhưng người ta sẽ không nói cho bạn, vì một khi nói cho bạn thì năm phút là bạn học xong, đề thuật toán đó không còn thần bí nữa, trở nên không chịu nổi một đòn.

Bài này sẽ nói cho bạn khung đó, rồi dẫn bạn xử gọn từng bài một. Bài viết này dùng kỹ thuật máy trạng thái để giải, có thể nộp qua hết. Đừng thấy danh từ này cao siêu, chỉ là từ ngữ văn chương mà thôi, thực chất chính là bảng DP, liếc mắt là hiểu ngay.

6 bài này là có điểm chung, chúng ta chỉ cần rút bài 188 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất IV」 ra nghiên cứu, vì bài này là dạng tổng quát nhất, các bài khác đều là dạng rút gọn của nó, xem đề bài:

<Problem slug="best-time-to-buy-and-sell-stock-iv" />

Bài đầu tiên là chỉ thực hiện một giao dịch, tương đương `k = 1`; bài thứ hai là không giới hạn số lần giao dịch, tương đương `k = +infinity` (vô cực dương); bài thứ ba là chỉ thực hiện 2 giao dịch, tương đương `k = 2`; hai bài còn lại cũng không giới hạn số lần, nhưng thêm điều kiện phụ là 「kỳ đóng băng」 và 「phí giao dịch」, thực ra chính là biến thể của bài thứ hai, đều dễ xử lý.

Dưới đây quay lại chuyện chính, bắt đầu giải bài.






## Một, khung liệt kê vét cạn

Trước hết, vẫn là hướng suy nghĩ cũ: liệt kê vét cạn thế nào?

[Công thức cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) đã nói, thuật toán quy hoạch động bản chất chính là liệt kê vét cạn 「trạng thái」, rồi chọn nghiệm tối ưu trong các 「lựa chọn」.

Vậy với bài này, cụ thể tới từng ngày, xem tổng cộng có mấy loại 「trạng thái」 có thể, rồi tìm 「lựa chọn」 tương ứng của mỗi 「trạng thái」. Chúng ta cần liệt kê vét cạn mọi 「trạng thái」, mục đích liệt kê là cập nhật trạng thái dựa trên 「lựa chọn」 tương ứng. Nghe trừu tượng, bạn chỉ cần nhớ hai từ 「trạng thái」 và 「lựa chọn」 là được, dưới đây thực hành một chút là hiểu ngay.

```python
for trang_thai1 in tat_ca_gia_tri_cua_trang_thai1:
    for trang_thai2 in tat_ca_gia_tri_cua_trang_thai2:
        for ...
            dp[trang_thai1][trang_thai2][...] = chon_toi_uu(lua_chon1, lua_chon2...)
```

Ví dụ bài này, **mỗi ngày đều có ba loại 「lựa chọn」**: mua vào, bán ra, không làm gì, chúng ta dùng `buy`, `sell`, `rest` để thể hiện ba lựa chọn này.

Nhưng vấn đề là, không phải ngày nào cũng có thể tùy ý chọn ba lựa chọn này, vì `sell` phải sau `buy`, `buy` phải sau `sell`. Vậy thao tác `rest` còn nên chia làm hai trạng thái, một loại là `rest` sau `buy` (đang giữ cổ phiếu), một loại là `rest` sau `sell` (không giữ cổ phiếu). Hơn nữa đừng quên, chúng ta còn giới hạn số lần giao dịch `k`, nghĩa là bạn thao tác `buy` còn chỉ được thực hiện với tiền đề `k > 0`.

> [!NOTE]
> Chú ý tôi sẽ dùng thường xuyên từ 「giao dịch」 trong bài này, **chúng ta định nghĩa một lần mua vào và một lần bán ra là một 「giao dịch」**.






Rất phức tạp đúng không, đừng sợ, mục đích hiện tại của chúng ta chỉ là liệt kê vét cạn, bạn có bao nhiêu trạng thái đi nữa thì việc ta cần làm là liệt kê sạch một lượt.

**「Trạng thái」 của bài này có ba cái**, thứ nhất là số ngày, thứ hai là số lần giao dịch tối đa cho phép, thứ ba là trạng thái nắm giữ hiện tại (tức trạng thái của `rest` nói trên, chúng ta cứ dùng 1 thể hiện đang giữ, 0 thể hiện không giữ). Rồi chúng ta dùng một mảng ba chiều là chứa được toàn bộ tổ hợp của mấy loại trạng thái này:

```python
dp[i][k][0 hoặc 1]
0 <= i <= n - 1, 1 <= k <= K
n là số ngày, K lớn là số lần giao dịch tối đa cho phép, 0 và 1 thể hiện có đang giữ cổ phiếu hay không.
Bài này tổng cộng có n × K × 2 trạng thái, liệt kê vét cạn toàn bộ là giải quyết được.

for 0 <= i < n:
    for 1 <= k <= K:
        for s in {0, 1}:
            dp[i][k][s] = max(buy, sell, rest)
```

Hơn nữa chúng ta có thể dùng ngôn ngữ tự nhiên mô tả ý nghĩa của từng trạng thái, ví dụ ý nghĩa của `dp[3][2][1]` là: hôm nay là ngày thứ ba, tay tôi đang giữ cổ phiếu, đến nay thực hiện tối đa 2 giao dịch. Lại ví dụ ý nghĩa của `dp[2][3][0]`: hôm nay là ngày thứ hai, tay tôi hiện không giữ cổ phiếu, đến nay thực hiện tối đa 3 giao dịch. Rất dễ hiểu đúng không?

Đáp án cuối cùng chúng ta muốn tìm là `dp[n - 1][K][0]`, tức ngày cuối cùng, cho phép tối đa `K` giao dịch, thu được nhiều lợi nhuận nhất là bao nhiêu.

Độc giả có thể hỏi vì sao không phải `dp[n - 1][K][1]`? Vì `dp[n - 1][K][1]` đại diện cho tới ngày cuối cùng tay vẫn giữ cổ phiếu, còn `dp[n - 1][K][0]` thể hiện cổ phiếu trong tay ngày cuối cùng đã bán ra rồi, hiển nhiên lợi nhuận của trường hợp sau nhất định lớn hơn trường hợp trước.

Nhớ cách giải thích 「trạng thái」, một khi bạn thấy chỗ nào khó hiểu thì dịch nó thành ngôn ngữ tự nhiên là dễ hiểu ngay.






## Hai, khung chuyển trạng thái

Giờ chúng ta đã hoàn thành liệt kê vét cạn 「trạng thái」, bắt đầu suy nghĩ mỗi loại 「trạng thái」 có những 「lựa chọn」 nào, nên cập nhật 「trạng thái」 thế nào.

Chỉ nhìn 「trạng thái nắm giữ」 thì có thể vẽ sơ đồ chuyển trạng thái:

![](https://labuladong.online/algo/images/stock/1.png)

Qua sơ đồ này có thể thấy rất rõ mỗi loại trạng thái (0 và 1) chuyển tới như thế nào. Dựa vào sơ đồ này, chúng ta viết phương trình chuyển trạng thái:

```python
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max( hôm nay chọn rest,        hôm nay chọn sell       )
```

Giải thích: hôm nay tôi không giữ cổ phiếu, có hai khả năng, tôi tìm lợi nhuận lớn nhất từ hai khả năng này:

1. Hôm qua tôi đã không giữ, và tính đến hôm qua giới hạn số lần giao dịch tối đa là `k`; rồi hôm nay tôi chọn `rest`, nên hôm nay tôi vẫn không giữ, giới hạn số lần giao dịch tối đa vẫn là `k`.

2. Hôm qua tôi giữ cổ phiếu, và tính đến hôm qua giới hạn số lần giao dịch tối đa là `k`; nhưng hôm nay tôi `sell` rồi, nên hôm nay tôi không giữ cổ phiếu nữa, giới hạn số lần giao dịch tối đa vẫn là `k`.

```python
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max( hôm nay chọn rest,         hôm nay chọn buy         )
```

Giải thích: hôm nay tôi đang giữ cổ phiếu, giới hạn số lần giao dịch tối đa là `k`, vậy với hôm qua mà nói, có hai khả năng, tôi tìm lợi nhuận lớn nhất từ hai khả năng này:

1. Hôm qua tôi đã giữ cổ phiếu, và tính đến hôm qua giới hạn số lần giao dịch tối đa là `k`; rồi hôm nay chọn `rest`, nên hôm nay tôi vẫn giữ cổ phiếu, giới hạn số lần giao dịch tối đa vẫn là `k`.

2. Hôm qua tôi vốn không giữ, và tính đến hôm qua giới hạn số lần giao dịch tối đa là `k - 1`; nhưng hôm nay tôi chọn `buy`, nên hôm nay tôi giữ cổ phiếu rồi, giới hạn số lần giao dịch tối đa là `k`.

> [!NOTE]
> Chỗ này nhắc trọng điểm một chút, **lúc nào cũng ghi nhớ 「định nghĩa của trạng thái」**, trạng thái `k` được định nghĩa không phải là 「số giao dịch đã thực hiện」, mà là 「giới hạn trên của số lần giao dịch tối đa」. Nếu xác định hôm nay thực hiện một giao dịch, mà cần đảm bảo tính đến hôm nay giới hạn trên số lần giao dịch tối đa là `k`, vậy thì giới hạn trên số lần giao dịch tối đa của hôm qua phải là `k - 1`. Lấy một ví dụ cụ thể, ví như yêu cầu thẻ ngân hàng của bạn hôm nay ít nhất có 100 đồng, mà bạn xác định hôm nay mình kiếm được 10 đồng, vậy bạn phải đảm bảo thẻ ngân hàng hôm qua ít nhất còn 90 đồng.

Giải thích này chắc đã rất rõ, nếu `buy` thì phải trừ `prices[i]` khỏi lợi nhuận, nếu `sell` thì phải cộng `prices[i]` vào lợi nhuận. Lợi nhuận lớn nhất hôm nay chính là cái lớn hơn trong hai lựa chọn có thể đó.

Chú ý giới hạn của `k`, khi chọn `buy` thì tương đương mở một giao dịch, vậy với hôm qua mà nói, giới hạn trên số lần giao dịch `k` nên giảm đi 1.

> [!NOTE]
> Chỗ này bổ sung đính chính một chút, trước đây tôi tưởng giảm 1 cho `k` khi `sell` với giảm 1 cho `k` khi `buy` là tương đương, nhưng độc giả tinh ý đã chất vấn tôi, sau khi suy nghĩ sâu tôi phát hiện trường hợp trước quả thực là sai, vì giao dịch bắt đầu từ `buy`, nếu lựa chọn `buy` không làm thay đổi số lần giao dịch `k` thì sẽ xuất hiện lỗi vượt quá giới hạn số lần giao dịch.






Giờ chúng ta đã hoàn thành bước khó nhất trong quy hoạch động: phương trình chuyển trạng thái. **Nếu nội dung trước đó bạn đều hiểu được, vậy bạn đã có thể xử gọn mọi bài toán, chỉ cần áp khung này là được**. Có điều còn thiếu một chút cuối cùng, chính là định nghĩa base case, tức trường hợp đơn giản nhất.

```python
dp[-1][...][0] = 0
Giải thích: vì i bắt đầu từ 0, nên i = -1 nghĩa là còn chưa bắt đầu, lúc này lợi nhuận đương nhiên là 0.

dp[-1][...][1] = -infinity
Giải thích: lúc còn chưa bắt đầu thì không thể giữ cổ phiếu.
Vì thuật toán của chúng ta yêu cầu một giá trị lớn nhất, nên giá trị khởi tạo đặt là một giá trị nhỏ nhất để tiện lấy giá trị lớn nhất.

dp[...][0][0] = 0
Giải thích: vì k bắt đầu từ 1, nên k = 0 nghĩa là hoàn toàn không cho phép giao dịch, lúc này lợi nhuận đương nhiên là 0.

dp[...][0][1] = -infinity
Giải thích: trong trường hợp không cho phép giao dịch thì không thể giữ cổ phiếu.
Vì thuật toán của chúng ta yêu cầu một giá trị lớn nhất, nên giá trị khởi tạo đặt là một giá trị nhỏ nhất để tiện lấy giá trị lớn nhất.
```

Tổng kết phương trình chuyển trạng thái trên lại:

```python
base case:
dp[-1][...][0] = dp[...][0][0] = 0
dp[-1][...][1] = dp[...][0][1] = -infinity

Phương trình chuyển trạng thái:
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
```

Độc giả có thể hỏi, chỉ số mảng này là -1 thì trong lập trình thể hiện thế nào, vô cực âm thể hiện thế nào? Đều là vấn đề chi tiết, có nhiều cách cài đặt. Giờ khung đầy đủ đã hoàn thành, dưới đây bắt đầu cụ thể hóa.






## Ba, giải gọn các đề bài

### 121. Thời điểm mua bán cổ phiếu tốt nhất

**Bài đầu tiên, nói về bài 121 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất」, tương đương trường hợp `k = 1`**:

<Problem slug="best-time-to-buy-and-sell-stock" />

Áp thẳng phương trình chuyển trạng thái, dựa theo base case có thể làm một số rút gọn:

```python
dp[i][1][0] = max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
dp[i][1][1] = max(dp[i-1][1][1], dp[i-1][0][0] - prices[i]) 
            = max(dp[i-1][1][1], -prices[i])
Giải thích: base case của k = 0, nên dp[i-1][0][0] = 0.

Giờ phát hiện k đều là 1, không thay đổi, tức k đã không còn ảnh hưởng tới chuyển trạng thái.
Có thể rút gọn thêm một bước, bỏ mọi k:
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], -prices[i])
```

Viết thẳng ra code:

```java
int n = prices.length;
int[][] dp = new int[n][2];
for (int i = 0; i < n; i++) {
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
    dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
}
return dp[n - 1][0];
```

Hiển nhiên khi `i = 0` thì `i - 1` là chỉ số không hợp lệ, đây là vì chúng ta chưa xử lý base case của `i`, có thể cho một xử lý đặc biệt như sau:




```java
if (i - 1 == -1) {
    dp[i][0] = 0;
    // Theo phương trình chuyển trạng thái suy ra:
    //   dp[i][0] 
    // = max(dp[-1][0], dp[-1][1] + prices[i])
    // = max(0, -infinity + prices[i]) = 0
    // = max(dp[-1][0], dp[-1][1] + prices[i])
    // = max(0, -infinity + prices[i]) = 0

    dp[i][1] = -prices[i];
    // Theo phương trình chuyển trạng thái suy ra:
    //   dp[i][1] 
    // = max(dp[-1][1], dp[-1][0] - prices[i])
    // = max(-infinity, 0 - prices[i]) 
    // = -prices[i]
    // = max(dp[-1][1], dp[-1][0] - prices[i])
    // = max(-infinity, 0 - prices[i]) 
    // = -prices[i]
    continue;
}
```



Bài đầu tiên đã giải xong, nhưng xử lý base case kiểu này rất phiền, hơn nữa chú ý phương trình chuyển trạng thái một chút, trạng thái mới chỉ liên quan tới một trạng thái kề bên, nên có thể dùng [tuyệt chiêu nén chiều của quy hoạch động: kỹ thuật nén không gian](https://labuladong.online/algo/dynamic-programming/space-optimization/), không cần dùng cả mảng `dp`, chỉ cần một biến lưu trạng thái kề bên đó là đủ, như vậy có thể giảm độ phức tạp không gian xuống O(1):

```java
// bản gốc
int maxProfit_k_1(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
    }
    return dp[n - 1][0];
}

// bản tối ưu độ phức tạp không gian
int maxProfit_k_1(int[] prices) {
    int n = prices.length;
    // base case: dp[-1][0] = 0, dp[-1][1] = -infinity
    int dp_i_0 = 0, dp_i_1 = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        // dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
        dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
        // dp[i][1] = max(dp[i-1][1], -prices[i])
        dp_i_1 = Math.max(dp_i_1, -prices[i]);
    }
    return dp_i_0;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>

Hai cách đều như nhau, có điều cách lập trình này gọn gàng hơn nhiều, nhưng nếu không có phương trình chuyển trạng thái phía trước dẫn dắt thì chắc chắn không hiểu. Các bài tiếp theo, bạn có thể đối chiếu xem làm sao tối ưu gọn không gian của mảng `dp`.

### 122. Thời điểm mua bán cổ phiếu tốt nhất II

**Bài thứ hai, xem bài 122 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất II」, cũng chính là trường hợp `k` là vô cực dương**:

<Problem slug="best-time-to-buy-and-sell-stock-ii" />

Đề bài còn nhấn mạnh có thể bán trong cùng ngày, nhưng tôi thấy điều kiện này hoàn toàn là thừa, nếu mua rồi bán ngay trong ngày thì lợi nhuận đương nhiên là 0, cái này không phải giống hệt không thực hiện giao dịch sao? Đặc điểm của bài này là không cho giới hạn tổng số giao dịch `k`, cũng tương đương `k` là vô cực dương.

Nếu `k` là vô cực dương, vậy có thể coi `k` và `k - 1` là như nhau. Có thể viết lại khung như sau:

```python
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
            = max(dp[i-1][k][1], dp[i-1][k][0] - prices[i])

Chúng ta phát hiện k trong mảng đã không còn thay đổi, nghĩa là không cần ghi lại trạng thái k này nữa:
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
```

Dịch thẳng thành code:

```java
// bản gốc
int maxProfit_k_inf(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
    }
    return dp[n - 1][0];
}

// bản tối ưu độ phức tạp không gian
int maxProfit_k_inf(int[] prices) {
    int n = prices.length;
    int dp_i_0 = 0, dp_i_1 = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        int temp = dp_i_0;
        dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
        dp_i_1 = Math.max(dp_i_1, temp - prices[i]);
    }
    return dp_i_0;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>

### 309. Thời điểm mua bán cổ phiếu tốt nhất có kỳ đóng băng

**Bài thứ ba, xem bài 309 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất có kỳ đóng băng」, cũng chính là `k` là vô cực dương, nhưng có trường hợp kỳ đóng băng giao dịch**:

<Problem slug="best-time-to-buy-and-sell-stock-with-cooldown" />

Giống bài trước, chỉ có điều mỗi lần `sell` xong phải đợi một ngày mới được giao dịch tiếp, chỉ cần đưa đặc điểm này vào phương trình chuyển trạng thái của bài trước là được:

```python
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i])
Giải thích: ngày thứ i chọn buy thì phải chuyển từ trạng thái i-2, chứ không phải i-1.
```

Dịch thành code:

```java
// bản gốc
int maxProfit_with_cool(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case 1
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        if (i - 2 == -1) {
            // base case 2
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            // khi i - 2 nhỏ hơn 0 thì suy ra base case tương ứng theo phương trình chuyển trạng thái
            dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
            //   dp[i][1] 
            // = max(dp[i-1][1], dp[-1][0] - prices[i])
            // = max(dp[i-1][1], 0 - prices[i])
            // = max(dp[i-1][1], -prices[i])
            continue;
        }
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], dp[i-2][0] - prices[i]);
    }
    return dp[n - 1][0];
}

// bản tối ưu độ phức tạp không gian
int maxProfit_with_cool(int[] prices) {
    int n = prices.length;
    int dp_i_0 = 0, dp_i_1 = Integer.MIN_VALUE;
    // đại diện cho dp[i-2][0]
    int dp_pre_0 = 0;
    for (int i = 0; i < n; i++) {
        int temp = dp_i_0;
        dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
        dp_i_1 = Math.max(dp_i_1, dp_pre_0 - prices[i]);
        dp_pre_0 = temp;
    }
    return dp_i_0;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock-with-cooldown/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>

### 714. Thời điểm mua bán cổ phiếu tốt nhất có phí giao dịch



**Bài thứ tư, xem bài 714 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất có phí giao dịch」, cũng chính là `k` là vô cực dương mà còn xét trường hợp phí giao dịch**:

<Problem slug="best-time-to-buy-and-sell-stock-with-transaction-fee" />

Mỗi giao dịch phải trả phí thủ tục, chỉ cần trừ phí thủ tục khỏi lợi nhuận là được, viết lại phương trình:

```python
dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i] - fee)
Giải thích: tương đương với giá mua cổ phiếu tăng lên.
Trừ trong công thức đầu tiên cũng như nhau, tương đương với giá bán cổ phiếu giảm xuống.
```

> [!NOTE]
> Nếu trực tiếp đặt `fee` vào công thức đầu tiên mà trừ, sẽ có một số test case không qua được, nguyên nhân lỗi là tràn số nguyên chứ không phải vấn đề hướng suy nghĩ. Một cách giải quyết là đổi kiểu `int` trong code thành kiểu `long`, tránh tràn số `int`.

Dịch thẳng thành code, chú ý sau khi phương trình chuyển trạng thái thay đổi thì base case cũng phải thay đổi tương ứng:

```java
// bản gốc
int maxProfit_with_fee(int[] prices, int fee) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i] - fee;
            //   dp[i][1]
            // = max(dp[i - 1][1], dp[i - 1][0] - prices[i] - fee)
            // = max(dp[-1][1], dp[-1][0] - prices[i] - fee)
            // = max(-inf, 0 - prices[i] - fee)
            // = -prices[i] - fee
            continue;
        }
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i] - fee);
    }
    return dp[n - 1][0];
}

// bản tối ưu độ phức tạp không gian
int maxProfit_with_fee(int[] prices, int fee) {
    int n = prices.length;
    int dp_i_0 = 0, dp_i_1 = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        int temp = dp_i_0;
        dp_i_0 = Math.max(dp_i_0, dp_i_1 + prices[i]);
        dp_i_1 = Math.max(dp_i_1, temp - prices[i] - fee);
    }
    return dp_i_0;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock-with-transaction-fee/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>



### 123. Thời điểm mua bán cổ phiếu tốt nhất III

**Bài thứ năm, xem bài 123 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất III」, cũng chính là trường hợp `k = 2`**:

<Problem slug="best-time-to-buy-and-sell-stock-iii" />

`k = 2` hơi khác với các bài trước, vì các trường hợp trên quan hệ với `k` không lớn: hoặc `k` là vô cực dương, chuyển trạng thái không còn liên quan tới `k`; hoặc `k = 1`, sát với base case `k = 0` rất gần, cuối cùng cũng không còn sự tồn tại.

Bài này `k = 2` với trường hợp `k` là số nguyên dương bất kỳ sẽ nói ở sau, cách xử lý `k` trở nên nổi bật lên, chúng ta viết thẳng code, vừa viết vừa phân tích nguyên nhân.

```java
Phương trình chuyển trạng thái gốc, không có chỗ nào rút gọn được
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
```

Theo code trước đó, chúng ta có thể cứ nghĩ là đương nhiên mà viết code như vậy (sai):

```java
int k = 2;
int[][][] dp = new int[n][k + 1][2];
for (int i = 0; i < n; i++) {
    if (i - 1 == -1) {
        // xử lý base case
        dp[i][k][0] = 0;
        dp[i][k][1] = -prices[i];
        continue;
    }
    dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
    dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);
}
return dp[n - 1][k][0];
```

Vì sao sai? Tôi đây không phải dựa theo phương trình chuyển trạng thái mà viết sao?

Còn nhớ 「khung liệt kê vét cạn」 tổng kết phía trước không? Nghĩa là chúng ta phải liệt kê vét cạn mọi trạng thái. Thực ra các cách giải trước đó của chúng ta đều đang liệt kê vét cạn mọi trạng thái, chỉ có điều trong các bài trước `k` đều bị rút gọn mất.

Ví như bài đầu tiên, khung code khi `k = 1`:

```java
int n = prices.length;
int[][] dp = new int[n][2];
for (int i = 0; i < n; i++) {
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
    dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
}
return dp[n - 1][0];
```

Nhưng khi `k = 2`, vì chưa khử được ảnh hưởng của `k`, nên nhất định phải liệt kê vét cạn `k`:

```java
// bản gốc
int maxProfit_k_2(int[] prices) {
    int max_k = 2, n = prices.length;
    int[][][] dp = new int[n][max_k + 1][2];
    for (int i = 0; i < n; i++) {
        for (int k = max_k; k >= 1; k--) {
            if (i - 1 == -1) {
                // xử lý base case
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i];
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
            dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);
        }
    }
    // đã liệt kê vét cạn n × max_k × 2 trạng thái, đúng.
    return dp[n - 1][max_k][0];
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock-iii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>



> [!NOTE]
> **Chỗ này chắc chắn sẽ có độc giả thắc mắc, base case của `k` là 0, theo lý phải liệt kê vét cạn trạng thái `k` kiểu `k = 1, k++` mới đúng? Mà nếu bạn thật sự duyệt `k` từ nhỏ tới lớn như vậy, nộp bài phát hiện cũng qua được**.

Thắc mắc này rất đúng, vì bài [giải đáp quy hoạch động](https://labuladong.online/algo/dynamic-programming/faq-summary/) phía trước của chúng ta có giới thiệu thứ tự duyệt mảng `dp` xác định thế nào, chủ yếu là dựa theo base case, lấy base case làm điểm xuất phát, tiến dần về kết quả.

Nhưng vì sao tôi duyệt `k` từ lớn tới nhỏ cũng nộp đúng được? Vì bạn để ý xem, `dp[i][k][..]` sẽ không phụ thuộc `dp[i][k - 1][..]`, mà phụ thuộc `dp[i - 1][k - 1][..]`, còn `dp[i - 1][..][..]` đều đã tính ra rồi, nên dù bạn là `k = max_k, k--` hay `k = 1, k++`, đều cho ra đáp án đúng được.

Vậy vì sao tôi dùng cách `k = max_k, k--`? Vì như vậy hợp ngữ nghĩa:

Bạn mua cổ phiếu, 「trạng thái」 ban đầu là gì? Phải là bắt đầu từ ngày 0, mà còn chưa thực hiện mua bán gì, nên giới hạn số lần giao dịch tối đa `k` phải là `max_k`; mà theo sự dịch chuyển của 「trạng thái」, bạn sẽ thực hiện giao dịch, vậy giới hạn trên số lần giao dịch `k` nên không ngừng giảm, nghĩ vậy thì cách `k = max_k, k--` là khá hợp với tình huống thực tế.

Dĩ nhiên, chỗ này phạm vi giá trị của `k` khá nhỏ, nên cũng có thể không dùng vòng for, liệt kê thẳng trường hợp k = 1 và 2 ra cũng được:

```java
// Phương trình chuyển trạng thái:
// dp[i][2][0] = max(dp[i-1][2][0], dp[i-1][2][1] + prices[i])
// dp[i][2][1] = max(dp[i-1][2][1], dp[i-1][1][0] - prices[i])
// dp[i][1][0] = max(dp[i-1][1][0], dp[i-1][1][1] + prices[i])
// dp[i][1][1] = max(dp[i-1][1][1], -prices[i])

// bản tối ưu độ phức tạp không gian
int maxProfit_k_2(int[] prices) {
    // base case
    int dp_i10 = 0, dp_i11 = Integer.MIN_VALUE;
    int dp_i20 = 0, dp_i21 = Integer.MIN_VALUE;
    for (int price : prices) {
        dp_i20 = Math.max(dp_i20, dp_i21 + price);
        dp_i21 = Math.max(dp_i21, dp_i10 - price);
        dp_i10 = Math.max(dp_i10, dp_i11 + price);
        dp_i11 = Math.max(dp_i11, -price);
    }
    return dp_i20;
}
```

Có phương trình chuyển trạng thái với tên biến ý nghĩa rõ ràng chỉ dẫn, tin rằng bạn rất dễ hiểu. Thực ra chúng ta có thể làm ra vẻ huyền bí, đổi bốn biến trên thành `a, b, c, d`. Như vậy khi người khác xem code của bạn sẽ thất kinh, nhìn bạn bằng con mắt nể phục.

### 188. Thời điểm mua bán cổ phiếu tốt nhất IV

Bài thứ sáu, xem bài 188 trên LeetCode「Thời điểm mua bán cổ phiếu tốt nhất IV」, tức trường hợp `k` có thể là bất kỳ số nào đề bài cho:

<Problem slug="best-time-to-buy-and-sell-stock-iv" />

Có bước đệm của bài `k = 2` trước đó, bài này với cách giải đầu tiên của bài trước chắc không khác gì, bạn đổi `k = 2` của bài trước thành `k` đề bài nhập vào là được.

Nhưng thử một chút phát hiện báo lỗi vượt giới hạn bộ nhớ, thì ra giá trị `k` truyền vào rất lớn, mảng `dp` quá lớn. Vậy giờ nghĩ xem, số lần giao dịch `k` lớn nhất là bao nhiêu?

Một giao dịch gồm mua vào và bán ra, ít nhất cần hai ngày. Nên nói giới hạn `k` hiệu quả phải không vượt quá `n/2`, nếu vượt quá thì không còn tác dụng ràng buộc, tương đương trường hợp `k` không giới hạn, mà trường hợp này đã giải trước đó rồi.

Vì vậy chúng ta có thể tái dùng trực tiếp code trước đó:

```java
int maxProfit_k_any(int max_k, int[] prices) {
    int n = prices.length;
    if (n <= 0) {
        return 0;
    }
    if (max_k > n / 2) {
        // tái dùng trường hợp không giới hạn số lần giao dịch k
        return maxProfit_k_inf(prices);
    }

    // base case:
    // dp[-1][...][0] = dp[...][0][0] = 0
    // dp[-1][...][1] = dp[...][0][1] = -infinity
    int[][][] dp = new int[n][max_k + 1][2];
    // base case khi k = 0
    for (int i = 0; i < n; i++) {
        dp[i][0][1] = Integer.MIN_VALUE;
        dp[i][0][0] = 0;
    }

    for (int i = 0; i < n; i++) 
        for (int k = max_k; k >= 1; k--) {
            if (i - 1 == -1) {
                // xử lý base case khi i = -1
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i];
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
            dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i]);     
        }
    return dp[n - 1][max_k][0];
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/best-time-to-buy-and-sell-stock-iv/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>



Đến đây, 6 bài toán dùng một phương trình chuyển trạng thái giải hết toàn bộ.

## Vạn pháp quy nhất

Nếu bạn đọc được tới đây, đã có thể vỗ tay cho bạn, lần đầu hiểu được bài toán quy hoạch động phức tạp như vậy chắc tốn của bạn không ít tế bào não, có điều đáng giá, series bài cổ phiếu đã thuộc dạng khó trong bài toán quy hoạch động, nếu mấy bài này bạn đều hiểu thấu được, thử hỏi những con tép tôm khác có gì đáng sợ?

**Giờ bạn đã qua tám mươi nạn đầu trong tám mươi mốt nạn, cuối cùng tôi còn muốn làm khó thêm bạn một chút, mời bạn cài đặt hàm sau**:

```java
int maxProfit_all_in_one(int max_k, int[] prices, int cooldown, int fee);
```

Nhập vào mảng giá cổ phiếu `prices`, bạn thực hiện tối đa `max_k` giao dịch, mỗi giao dịch cần tốn thêm `fee` phí thủ tục, mà sau mỗi giao dịch cần trải qua `cooldown` ngày đóng băng mới được thực hiện giao dịch tiếp theo, mời bạn tính và trả về lợi nhuận lớn nhất có thể thu được.

Thế nào, có bị dọa không? Nếu bạn trực tiếp ra cho người khác một đề bài như vậy, e rằng đối phương sẽ hộc máu tại chỗ, có điều chúng ta từng bước làm tới đây, bạn chắc rất dễ phát hiện đề bài này chính là tổ hợp của mấy trường hợp chúng ta thảo luận trước đó mà.

Vì vậy, chỉ cần đem mấy đoạn code đã cài đặt trước đó trộn vào nhau, **trong base case và phương trình chuyển trạng thái đồng thời thêm vào ràng buộc của `cooldown` và `fee` là được**:

```java
// đồng thời xét giới hạn số lần giao dịch, kỳ đóng băng và phí giao dịch
int maxProfit_all_in_one(int max_k, int[] prices, int cooldown, int fee) {
    int n = prices.length;
    if (n <= 0) {
        return 0;
    }
    if (max_k > n / 2) {
        // trường hợp không giới hạn số lần giao dịch k
        return maxProfit_k_inf(prices, cooldown, fee);
    }

    int[][][] dp = new int[n][max_k + 1][2];
    // base case khi k = 0
    for (int i = 0; i < n; i++) {
        dp[i][0][1] = Integer.MIN_VALUE;
        dp[i][0][0] = 0;
    }

    for (int i = 0; i < n; i++) 
        for (int k = max_k; k >= 1; k--) {
            if (i - 1 == -1) {
                // base case 1
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i] - fee;
                continue;
            }

            // base case có xét cooldown
            if (i - cooldown - 1 < 0) {
                // base case 2
                dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
                // đừng quên trừ fee
                dp[i][k][1] = Math.max(dp[i-1][k][1], -prices[i] - fee);
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
            // đồng thời xét cooldown và fee
            dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-cooldown-1][k-1][0] - prices[i] - fee);     
        }
    return dp[n - 1][max_k][0];
}

// k không giới hạn, có xét phí giao dịch và kỳ đóng băng
int maxProfit_k_inf(int[] prices, int cooldown, int fee) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case 1
            dp[i][0] = 0;
            dp[i][1] = -prices[i] - fee;
            continue;
        }

        // base case có xét cooldown
        if (i - cooldown - 1 < 0) {
            // base case 2
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            // đừng quên trừ fee
            dp[i][1] = Math.max(dp[i-1][1], -prices[i] - fee);
            continue;
        }
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
        // đồng thời xét cooldown và fee
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - cooldown - 1][0] - prices[i] - fee);
    }
    return dp[n - 1][0];
}
```

Bạn có thể dùng hàm `maxProfit_all_in_one` này để hoàn thành 6 bài đã nói trước đó, vì chúng ta không thể tối ưu mảng `dp`, nên hiệu suất thực thi không phải tối ưu nhất, nhưng tính đúng đắn chắc chắn không có vấn đề.

Cuối cùng tổng kết một chút, bài này đã giảng cho mọi người cách thông qua phương pháp chuyển trạng thái giải quyết bài toán phức tạp, dùng một phương trình chuyển trạng thái xử gọn 6 bài mua bán cổ phiếu, giờ ngoảnh lại xem, thực ra cũng không đáng sợ đúng không?

Then chốt nằm ở liệt kê ra mọi 「trạng thái」 có thể, rồi nghĩ xem liệt kê vét cạn cập nhật các 「trạng thái」 này thế nào. Thường dùng một mảng `dp` nhiều chiều lưu các trạng thái này, bắt đầu từ base case tiến dần về sau, tiến dần tới trạng thái cuối cùng chính là đáp án chúng ta muốn. Nghĩ về quá trình này, bạn có phải hơi hiểu ý nghĩa của danh từ 「quy hoạch động」 rồi không?

Cụ thể tới bài mua bán cổ phiếu, chúng ta phát hiện ba trạng thái, dùng một mảng ba chiều, chẳng qua vẫn là liệt kê vét cạn + cập nhật, có điều chúng ta có thể nói cao siêu một chút, gọi là 「DP ba chiều」, nghe có phải rất ngầu không?








<hr>
<details class="hint-container details">
<summary><strong>Các bài viết trích dẫn bài này</strong></summary>

  - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)

</details><hr>





<hr>
<details class="hint-container details">
<summary><strong>Các đề bài trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài trên Chrome của tôi](https://labuladong.online/algo/intro/chrome/) rồi mở đề bài dưới đây là xem được ngay hướng giải:</strong>

| LeetCode | LeetCode bản Trung | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm Chỉ Offer 63. Lợi nhuận lớn nhất của cổ phiếu](https://leetcode.cn/problems/gu-piao-de-zui-da-li-run-lcof/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
