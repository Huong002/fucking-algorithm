# Quy hoạch động kinh điển: Bài toán ba lô (knapsack) 0-1



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

> tip: Bài này có bản video: [Giải chi tiết bài toán ba lô 0-1](https://www.bilibili.com/video/BV15B4y1P7X7/).Khuyếnnghị follow tài khoản Bilibili của mình, mình sẽ dẫn đọc bằng video các kỹ thuật thuật toán hơi khó.



Hậu trường ngày nào cũng có người hỏi bài toán ba lô, bài này thật ra không khó, mượn khung tư duy quy hoạch động, chẳng qua vẫn là trạng thái + lựa chọn, không có gì đặc biệt. Hôm naythì nói về bài toán ba lô, chỉ bàn bài toán ba lô 0-1 hay gặp nhất. Mô tả:

Cho bạn một ba lô chứa được trọng lượng `W` và `N` món đồ, mỗi món có hai thuộc tính trọng lượng và giá trị. Trong đó món thứ `i` nặng `wt[i]`, giá trị `val[i]`. Giờ bắt bạn dùng ba lô này đựng đồ, mỗi món chỉ dùng một lần, với tiền đề không vượtdung lượng ba lô, giá trị đựng được nhiều nhất là bao nhiêu?

![](https://labuladong.online/algo/images/knapsack/1.png)

Lấy ví dụ đơn giản, đầu vào như sau:

```py
N = 3, W = 4
wt = [2, 1, 3]
val = [4, 2, 3]
```

Thuật toán trả về 6, chọn hai món đầu bỏ vào ba lô, tổng trọng lượng 3 nhỏ hơn `W`, được giá trị lớn nhất 6.

Đề bàithì đơn giản vậy, một bài quy hoạch động điển hình. Vật phẩm trong bài này không thểchia cắt, hoặc bỏ vào túi, hoặc không bỏ, không thể nói cắt làm hai bỏ một nửa. Đây chính là lai lịch của danh từ ba lô 0-1.

Giải bài này không có phương phápkhéo kiểu sắp xếp gì, chỉ liệt kê mọi khả năng, theo mô-típ trong [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) củata, đi thẳng quy trình là được.






## mô-típ chuẩn động quy

Xem ra mỗi bài quy hoạch động đều phải lặp mô-típ, các bài quy hoạch động trong lịch sử đều theo mô-típ dưới đây.

**Bước một rõ ràng hai điểm, 「trạng thái」và「lựa chọn」**.

Nói trạng thái trước, làm sao mô tả cục diện một bài toán? Chỉ cần cho mấy món đồ và một giới hạn sức chứa ba lô, thì tạo thành một bài toán ba lô nhé. **Nên trạng thái có hai, chính là「 sức chứa ba lô」và「món đồ có thể chọn」**.

Nói lựa chọn, cũng rất dễ nghĩ, với mỗi món đồ, bạn chọn được gì? **Lựa chọn chính là 「bỏ vào ba lô」hoặc「không bỏ vào ba lô」 mà **.

Hiểu trạng thái và lựa chọn, bài toán quy hoạch động cơ bản thì giải xong, với cách nghĩ bottom-up, khung code chung là:

```python
for trạng thái1 in mọi giá trị của trạng thái1:
    for trạng thái2 in mọi giá trị của trạng thái2:
        for ...
            dp[trạng thái1][trạng thái2][...] = chọn ưu /chọn ưu(lựa chọn1, lựa chọn2...)
```

**Bước hai rõ ràng định nghĩa mảng `dp`**.

Trước hết xem「trạng thái」vừa tìm, có hai, chính là nói ta cần mảng `dp` hai chiều.

Định nghĩa `dp[i][w]` như sau: với `i` món đầu, sức chứa ba lô hiện tại là `w`, giá trị lớn nhất đựng được trong trường hợp này là `dp[i][w]`.

Ví dụ, nếu `dp[3][5] = 6`, ý nghĩa là: trong một loạt món cho trước, nếu chỉ chọn trong 3 món đầu, khi sức chứa ba lô là 5, giá trị nhiều nhất đựng được là 6.






> [!NOTE]
> Vì sao định nghĩa vậy? Vì như vậy tìm được quan hệ chuyển trạng thái, hoặc nói đây chính là cách định nghĩa đặc thù của bài toán ba lô, bạn ghi nhớ như mô-típ là được, tương lai gặp bài liên quan quy hoạch động, đều thử định nghĩa vậy.

Theo định nghĩa này, đáp án cuối ta muốn tìm chính là `dp[N][W]`. base case chính là `dp[0][..] = dp[..][0] = 0`, vì khi không có món hoặc ba lô không còn chỗ, giá trị lớn nhất đựng được chính là 0.

kh chi tiết khung trên:

```python
int[][] dp[N+1][W+1]
dp[0][..] = 0
dp[..][0] = 0

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            bỏ món i vào ba lô,
            không bỏ món i vào ba lô
        )
return dp[N][W]
```

**Bước ba, theo「lựa chọn」, nghĩ logic chuyển trạng thái**.

Nói đơn giản chính là, 「bỏ món `i` vào ba lô」và「không bỏ món `i` vào ba lô」trong mã giả trên thể hiện bằng code thế nào?

Đâythì cần kết hợp định nghĩa mảng `dp`, xem hai lựa chọn này ảnh hưởng gì tới trạng thái:






Nhắc lại định nghĩa mảng `dp` vừa rồi:

`dp[i][w]` cho biết: với `i` món đầu (đếm từ 1), khi sức chứa ba lô hiện tại là `w`, giá trị lớn nhất đựng được trong trường hợp này là `dp[i][w]`.

**Nếu bạn không bỏ món thứ `i` này vào ba lô**, thì rất hiển nhiên, giá trị lớn nhất `dp[i][w]` hẳn bằng `dp[i-1][w]`, kế thừa kết quả trước đó.

**Nếu bạn bỏ món thứ `i` này vào ba lô**, thì `dp[i][w]` hẳn bằng `val[i-1] + dp[i-1][w - wt[i-1]]`.

Trước hết, vì chỉ số mảng bắt đầu từ 0, mà `i` trong định nghĩa của ta đếm từ 1, nên `val[i-1]` và `wt[i-1]` cho biết giá trị và trọng lượng của món thứ `i`.

Nếu bạn chọn bỏ món thứ `i` vào ba lô, thì giá trị `val[i-1]` của món thứ `i` chắc chắn tới tay, tiếp theo bạnthì cần trong giới hạn sức chứa còn lại `w - wt[i-1]`, chọn trong `i - 1` món đầu, tìm giá trị lớn nhất, tức `dp[i-1][w - wt[i-1]]`.

Tổng hợp chính là hai lựa chọn, ta đều đã phân tích xong, chính là viết ra phương trình chuyển trạng thái, có thể thu nhỏ code thêm:

```python
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

**Bước cuối, dịch mã giả thành code, xử lý vài trường hợp biên**.

Mình viết code bằng Java, dịch hoàn toàn ý tưởng trên một lượt, mà xử lý vấn đề `w - wt[i-1]` có thể nhỏ hơn 0 dẫnvượt chỉ số mảng:

```java
int knapsack(int W, int N, int[] wt, int[] val) {
    assert N == wt.length;
    // base case đã khởi tạo
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = 1; w <= W; w++) {
            if (w - wt[i - 1] < 0) {
                // Trường hợp này chỉ chọn không bỏ vào ba lô
                dp[i][w] = dp[i - 1][w];
            } else {
                // Bỏ vào hoặc không bỏ vào ba lô, chọn ưu
                dp[i][w] = Math.max(
                    dp[i - 1][w - wt[i-1]] + val[i-1],
                    dp[i - 1][w]
                );
            }
        }
    }

    return dp[N][W];
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-knapsack/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>👾 Animation trực quan hóa code 👾</s trong>
</summary>
</details>
</a>
<hr/>



> [!NOTE]
> Thật ra số lượng món `N` trong chữ ký hàm chính là độ dài mảng `wt`, nên thực tế tham số `N` nàythừa. Nhưng để thể hiện bài toán ba lô 0-1gốc, mìnhthì mang tham số `N` này, bạn tự viết có thể lược bỏ.

Tới đây, bài toán ba lôthì giải xong, tương đối mà nói, mình thấy đây là bài quy hoạch động khá đơn giản, vì suy ra chuyển trạng thái khá tự nhiên, cơ bản bạn rõ ràng định nghĩa mảng `dp`, hiển nhiên xác định chuyển trạng thái.




<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [kỹ thuật quét đường (scan line): Sắp xếp phòng họp](https://labuladong.online/algo/frequency-interview/scan-line-technique/)
 - [Quy hoạch động kinh điển: Bài toán ba lô tập con](https://labuladong.online/algo/dynamic-programming/knapsack2/)
 - [Quy hoạch động kinh điển: Bài toán ba lô đầy đủ](https://labuladong.online/algo/dynamic-programming/knapsack3/)
 - [Biến thể ba lô: Tổng mục tiêu (Target Sum)](https://labuladong.online/algo/dynamic-programming/target-sum/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/?show=1)| [1235. Lập kế hoạch làmbán thời gian](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/?show=1)| 🔴 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

