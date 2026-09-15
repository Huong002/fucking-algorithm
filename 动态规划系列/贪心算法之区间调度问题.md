# Thuật toán tham lam (greedy): Bài toán lập lịch khoảng (interval scheduling)



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)| [435. Khoảng không trùng lặp](https://leetcode.cn/problems/non-overlapping-intervals/)| 🟠 |
| [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)| [452. Dùng ít tên nhất bắn nổ bóng](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)| 🟠 |

**-----------**



Thuật toán tham lam là gì? Thuật toán tham lam có thể coi là một trường hợp đặc biệt của thuật toán quy hoạch động, so với quy hoạch động, dùng tham lam cần thỏa nhiều điều kiện hơn (tính chất lựa chọn tham lam), nhưng hiệu quả cao hơn quy hoạch động.

Ví như một bài toán thuật toán dùng cách giải vét cạn cần thời gian hàm mũ, nếu dùng quy hoạch động loại bỏ bài toán con trùng lặp, thì giảm xuống thời gian đa thức, nếu thỏa tính chất lựa chọn tham lam, thì còn giảm thời gian nữa, tới mức tuyến tính.

Tính chất lựa chọn tham lam là gì, nói đơn giản chính là: mỗi bước đều đưa ra một lựa chọn tối ưu cục bộ, kết quả cuối cùng chính là tối ưu toàn cục. Chú ý nhé, đây là một tính chất đặc thù, thật ra chỉ một phần bài toán có tính chất này.

Ví dụ trước mặt bạn đặt 100 tờ tiền, bạn chỉ lấy được mười tờ, làm sao lấy đượcmệnh giá nhiều nhất? Hiển nhiên mỗi lần chọn tờ mệnh giá lớn nhất trong số còn lại, lựa chọn cuối của bạn nhất định tối ưu.

Tuy nhiên, phần lớn chia bài toán rõ ràng không có tính chất lựa chọn tham lam. Ví dụ đánh Đấu Địa Chủ, đối thủ ra đôi ba, theo chiến lược tham lam, bạn nên ra láhết sức có thể thể nhỏ vừađè được đối phương, nhưng thực tế ta thậm chí cònvứt bom vương. Trường hợp này không dùng tham lam được, mà phải dùng quy hoạch động, xem bài trước [Quy hoạch động giải bài toán đối kháng](https://labuladong.online/algo/dynamic-programming/game-theory/).

## Một, tổng quan bài toán

quay lại chính đề, bài này giải một bài tham lam rất kinh điển là Interval Scheduling (bài toán lập lịch khoảng), chính là bài 435「Khoảng không trùng lặp」trên LeetCode:

Cho bạn nhiều khoảng đóng dạng `[start, end]`, hãy thiết kế thuật toán, **tính trong các khoảng này nhiều nhất có mấy khoảng đôi một không giao nhau**.

```java
int intervalSchedule(int[][] intvs);
```

Lấy ví dụ, `intvs = [[1,3], [2,4], [3,6]]`, các khoảng này nhiều nhất có 2 khoảng đôi một không giao, tức `[[1,3], [3,6]]`, thuật toán của bạn nên trả về 2. Chú ý biên giống nhau không tính là giao.

Ứng dụng của bài này trong đời sống rộng, ví dụ hôm nay bạn có mấy hoạt động, mỗi hoạt động đều biểu diễn bằng khoảng `[start, end]` cho thời gian bắt đầu và kết thúc, hỏi hôm nay bạn **tham gia được nhiều nhất mấy hoạt động**? Hiển nhiên một mình bạn không thể đồng thời tham gia hai hoạt động, nên nói bài này chính là tìm tập con không giao lớn nhất của các khoảng thời gian này.






## Hai, cách giải tham lam

Bài này có nhiều ý tưởng tham lam trông không sai , nhưng đều không ra đáp án đúng. Ví dụ:

Có lẽ ta mỗi lần chọn trong khoảng có thể chọn cái bắt đầu sớm nhất? Nhưng có thể tồn tại vài khoảng bắt đầu rất sớm, nhưng rất dài, khiến ta sai lầm bỏ lỡ vài khoảng ngắn. Hoặc mỗi lần chọn trong khoảng có thể chọn cái ngắn nhất? Hoặc chọn khoảng xuất hiện xung đột ít nhất? Các phương án này đều dễ lấy ra ví dụ phản bác, không phải phương án đúng.

ý tưởng đúng thật ra rất đơn giản, chia ba bước:

1、Từ tập khoảng `intvs` chọn một khoảng `x`, `x` này là trong mọi khoảng hiện tại **kết thúc nhất sớm** (`end` nhỏ nhất).

2、Xóa mọi khoảng giao với khoảng `x` k hỏi tập `intvs`.

3、Lặp bước 1 và 2, tới khi `intvs` rỗng mới thôi. Những `x` đã chọn trước đó chính là tập con không giao lớn nhất.

Hiện thực ý tưởng này thành thuật toán, có thể sắp xếp theo giá trị số `end` của mỗi khoảng tăng dần, vì xử lý như vậy rồi hiện thực bước 1 và bước 2 đều tiện hơn nhiều, như GIF dưới đây:

![](https://labuladong.online/algo/images/interval/1.gif)

Giờ hiện thực thuật toán, với bước 1, vì ta đã sắp xếp theo `end` trước, nên chọn `x` rất dễ. Mấu chốt là, làm sao loại khoảng giao với `x`, chọn `x` của vòng tiếp theo?

**Vì ta đã sắp xếp trước**, không khó phát hiện mọi khoảng giao với `x` dĩ nhiên giao với `end` của `x`; nếu một khoảng không muốn giao với `end` của `x`, `start` của nóphải cần lớn hơn (hoặc bằng) `end` của `x`:

![](https://labuladong.online/algo/images/interval/2.jpg)

Xem code:

```java
class Solution {
    public int intervalSchedule(int[][] intvs) {
        if (intvs.length == 0) return 0;
        // Sắp xếp theo end tăng dần
        Arrays.sort(intvs, (a, b) -> Integer.compare(a[1], b[1]));
        // Ít nhất có một khoảng không giao
        int count = 1;
        // Sau sắp xếp, khoảng đầu tiên chính là x
        int x_end = intvs[0][1];
        for (int[] interval : intvs) {
            int start = interval[0];
            if (start >= x_end) {
                // Tìm được khoảng chọn tiếp theo
                count++;
                x_end = interval[1];
            }
        }
        return count;
    }
}
```

## Ba, ví dụ ứng dụng

Dưới đây lấy thêm mấy bài cụ thể ứng dụng thuật toán lập lịch khoảng.

Trước hết là bài 435「Khoảng không trùng lặp」trên LeetCode:

Nhập một tập khoảng, hãy tính, muốn các khoảng trong đó đôi một không trùng, ít nhất cần loại mấy khoảng? Chữ ký hàm như sau:

```java
int eraseOverlapIntervals(int[][] intvs);
```

Trong đó, có thể giả sử điểm cuối của khoảng nhập tổng là lớn hơn điểm bắt đầu, ngoài ra khoảng biên bằng nhau chỉ tính tiếp xúc, chứ không tính trùng nhau.

Ví dụ nhập là `intvs = [[1,2],[2,3],[3,4],[1,3]]`, thuật toán trả về 1, vì chỉ cần loại `[1,3]` thì các khoảng còn lạithì không trùng nữa.

Ta đã tìm được nhiều nhất có mấy khoảng không trùng, thì phần còn lại chẳng phải chính là khoảng ít nhất cần loại sao?

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intvs) {
        int n = intvs.length;
        return n - intervalSchedule(intvs);
    }

    private int intervalSchedule(int[][] intvs) {
        // Xem phần trên
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/non-overlapping-intervals/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🍭 Animation trực quan hóa code 🍭</s trong>
</summary>
</details>
</a>
<hr/>



Nói tiếp bài 452「Dùng ít tên nhất bắn nổ bóng」trên LeetCode, mình mô tả đề một chút:

Giả sử trên mặt phẳng hai chiều có nhiều bóng tròn, các hình tròn nàychiếu lên trục x sẽ tạo thành từng khoảng đúng không. Vậy cho bạn nhập các khoảng này, bạn tiến dọc trục x, có thể bắn tên thẳng đứng lên, hỏi bạn ít nhất bắn mấy tên mới bắn nổ toàn bộ bóng?

Chữ ký hàm như sau:

```java
int findMinArrowShots(int[][] intvs);
```

Ví dụ nhập là `[[10,16],[2,8],[1,6],[7,12]]`, thuật toán nên trả về 2, vì ta có thể bắn một tên chỗ `x` là 6, bắn nổ hai bóng `[2,8]` và `[1,6]`, rồi bắn một tên chỗ `x` là 10, 11 hoặc 12, bắn nổ hai bóng `[10,16]` và `[7,12]`.

Thật ra nghĩ một chút, bài này và thuật toán lập lịch khoảnggiống hệt! Nếu nhiều nhất có `n` khoảng không trùng, thìthì ít nhất cần `n` mũi tên xuyên mọi khoảng:

![](https://labuladong.online/algo/images/interval/3.jpg)

Chỉ có một điểm khác, trong thuật toán `intervalSchedule`, nếu biên hai khoảng chạm nhau, không tính trùng; mà theo mô tả của đề này, tên chạm biên bóng thì bóng cũng nổ, nên nói tương đương biên khoảng chạm nhau cũng tính trùng:

![](https://labuladong.online/algo/images/interval/4.jpg)

Nên chỉ cần sửa nhẹ thuật toán trước đó, chính là đáp án của bài này:

```java
class Solution {
    public int findMinArrowShots(int[][] intvs) {
        if (intvs.length == 0) return 0;
        Arrays.sort(intvs, (a, b) -> Integer.compare(a[1], b[1]));
        int count = 1;
        int x_end = intvs[0][1];

        for (int[] interval : intvs) {
            int start = interval[0];
            // Sửa >= thành > là được
            if (start > x_end) {
                count++;
                x_end = interval[1];
            }
        }
        return count;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/minimum-number-of-arrows-to-burst-balloons/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🌈 Animation trực quan hóa code 🌈</s trong>
</summary>
</details>
</a>
<hr/>



<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Một cách giải ba bài toán khoảng](https://labuladong.online/algo/practice-in-action/interval-problem-summary/)
 - [Cắt video cắt ra một thuật toán tham lam](https://labuladong.online/algo/frequency-interview/cut-video/)
 - [kỹ thuật quét đường: Sắp xếp phòng họp](https://labuladong.online/algo/frequency-interview/scan-line-technique/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

