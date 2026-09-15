# Giáng chiều cho quy hoạch động (tối ưu không gian)



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

> [!NOTE]
> Nén không gianchủ yếu để tối ưu độ phức tạp không gian của vài bài quy hoạch động. Nhưng trongthi viết thường yêu cầu không gian không cao, ngay cả không dùng kỹ thuật tối ưu này cũng qua được, nên cá nhân mình thấy nén trạng thái không phải kỹ thuật phải nắm vững, bạn đọc hứng thú có thể học kỹ hiểu.

Tài khoản của ta trước đây viếtchục bài quy hoạch động, có thể nói kỹ thuật quy hoạch động nâng cao hiệu quả thuật toán rất có thể quan sát, nói chung đều tối ưu được thuật toán độ phức tạp thời gian hàm mũ và giai thừa thành O(N^2), được mệnh danhvũ khí hai chiều của giới thuật toán, đánh hếtyêu ma thành hai chiều.

Nhưng, quá trình tìm lời giải quy hoạch động cũng có thể tối ưu theo giai đoạn, nếu bạn kỹ quan sát phương trình chuyển trạng thái của vài bài quy hoạch động, là có thể giảm tiếp độ phức tạp không gian của cách giải, từ O(N^2) xuống O(N).






> [!NOTE]
> Trước đây mình trong bài này nhầm lẫn dùng từ「nén trạng thái」, có bạn đọc chỉ ra ý nghĩa của「nén trạng thái」là đem nhiều trạng thái qua phép nhị phân biểu diễn bằng một số nguyên, từ đó giảm chiều mảng `dp`. Mà cách tối ưu mô tả trong bài này là qua quan sát quan hệ phụ thuộc của phương trình chuyển trạng thái, từ đó giảm chiều mảng `dp`, quả thật khác「nén trạng thái」. Nênchặt chẽ, mình sửa mọi「nén trạng thái」trong bài gốc thành「nén không gian」, tránh dùng sai danh từ.

Quy hoạch động dùng được kỹ thuật nén không gian đều là bài `dp` hai chiều, **bạn xem phương trình chuyển trạng thái của nó, nếu tính trạng thái `dp[i][j]` cần đều là trạng tháikề `dp[i][j]`, thìthì có thể lấy dùng kỹ thuật nén không gian**, chuyển mảng `dp` hai chiều thành một chiều, giảm độ phức tạp không gian từ O(N^2) xuống O(N).

Thế nào gọi「trạng thái kề nhau với `dp[i][j]`」, ví dụ bài trước [Dãy con đối xứng dài nhất](https://labuladong.online/algo/dynamic-programming/subsequence-problem/), codecuối cùng như sau:

```java
int longestPalindromeSubseq(String s) {
    int n = s.length();
    // Mảng dp khởi tạo toàn bộ là 0
    int[][] dp = new int[n][n];
    // base case
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    // Duyệt ngược đảm bảo chuyển trạng thái đúng
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i + 1; j < n; j++) {
            // Phương trình chuyển trạng thái
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    // Độ dài chuỗi con đối xứng dài nhất của toàn bộ s
    return dp[0][n - 1];
}
```

> [!TIP]
> Bài này ta không thảo luận suy ra phương trình chuyển trạng thái thế nào, chỉ thảo luận kỹ thuật nén không gian cho bài DP hai chiều. kỹ thuật đều tổng quát, nên nếu bạn chưa xem bài trước, không hiểu logic đoạn code này cũng không sao, hoàn toàn không cản bạn học được nén không gian.

Bạn xem ta cập nhật `dp[i][j]`, thật ra chỉ phụ thuộc ba trạng thái `dp[i+1][j-1], dp[i][j-1], dp[i+1][j]`:

![](https://labuladong.online/algo/images/space-optimal/1.jpeg)

Đây gọi là kề nhau với `dp[i][j]`, dù sao bạn tính `dp[i][j]` chỉ cần ba trạng thái kề nhau này, thật ra căn bản không cần bảng dp hai chiều lớn vậy đúng không? ** ý tưởng cốt lõi của nén không gian chính là, đem mảng hai chiều「chiếu」xuống mảng một chiều**:

![](https://labuladong.online/algo/images/space-optimal/2.jpeg)

Từ「 phép chiếu 」hẳn khá sinh động, nói thẳng chính là hy vọng mảng một chiều phát huy tác dụng của mảng hai chiều gốc.

ý tưởng rất trực quan, nhưng cũng có một vấn đề rõ ràng, trong hình hai trạng thái `dp[i][j-1]` và `dp[i+1][j-1]` chỗ cùng một cột, mà trong mảng một chiều chỉ chứa được một, vậy chúng phép chiếu xuống một chiều chắc chắn một cái bị cái kia ghi đè, mình còn tính `dp[i][j]` thế nào?

Đây chính là khó điểm của nén không gian, dưới đâythì đến phân tích giải quyết, vẫn là lấy bài「Dãy con đối xứng dài nhất」ví dụ, logic chính của phương trình chuyển trạng thái nó chính là đoạn code dưới:




```java
for (int i = n - 2; i >= 0; i--) {
    for (int j = i + 1; j < n; j++) {
        // Phương trình chuyển trạng thái
        if (s.charAt(i) == s.charAt(j)) {
            dp[i][j] = dp[i + 1][j - 1] + 2;
        } else {
            dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
        }
    }
}
```



Hồi tưởng hình trên, 「 phép chiếu 」thật ra chính là đem nhiều hàng thành một hàng, nên muốn nén mảng `dp` hai chiều thành một chiều, nói chung là bỏ chiều đầu tiên, chính là chiều `i`, chỉ còn chiều `j`. **Mảng `dp` một chiều sau nén chính là hàng `dp[i][..]` của mảng `dp` hai chiều trước đó**.

Ta sửa code trên trước, bỏ não bỏ chiều `i`, đem mảng `dp` thành một chiều:




```java
for (int i = n - 2; i >= 0; i--) {
    for (int j = i + 1; j < n; j++) {
        // Ở đây, số trong mảng dp một chiều là gì?
        if (s.charAt(i) == s.charAt(j)) {
            dp[j] = dp[j - 1] + 2;
        } else {
            dp[j] = Math.max(dp[j], dp[j - 1]);
        }
    }
}
```



Mảng `dp` một chiều trong code trên chỉ cho biết một hàng `dp[i][..]` của mảng `dp` hai chiều. Nhưng ta muốn thu được mấy giá trị cần thiết `dp[i+1][j-1], dp[i][j-1], dp[i+1][j]` để chuyển trạng thái.

Vì vậy, ta phải nghĩ hai câu hỏi trước:

1、Trước khi gán giá trị mới cho `dp[j]`, `dp[j]` tương ứng với vị trí nào trong mảng `dp` hai chiều?

2、`dp[j-1]` tương ứng với vị trí nào trong mảng `dp` hai chiều?

**Với câu 1, trước khi gán giá trị mới cho `dp[j]`, giá trị `dp[j]` chính là giá trị tính ở lần lặp trước của vòng for ngoài, chính là tương ứng vị trí `dp[i+1][j]` trong mảng `dp` hai chiều**.

**Với câu 2, giá trị `dp[j-1]` chính là giá trị tính ở lần lặp trước của vòng for trong, chính là tương ứng vị trí `dp[i][j-1]` trong mảng `dp` hai chiều**.

Vậy vấn đề đã giải hơn nửa, chỉ còn trạng thái `dp[i+1][j-1]` trong mảng `dp` hai chiều ta không lấy trực tiếp từ mảng `dp` một chiều được:




```java
for (int i = n - 2; i >= 0; i--) {
    for (int j = i + 1; j < n; j++) {
        if (s.charAt(i) == s.charAt(j)) {
            // dp[i][j] = dp[i+1][j-1] + 2;
            dp[j] = ?? + 2;
        } else {
            // dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
            dp[j] = Math.max(dp[j], dp[j - 1]);
        }
    }
}
```



Vì thứ tự vòng for duyệt `i` và `j` là từ trái sang phải, từ dưới lên trên, nên phát hiện, khi cập nhật mảng `dp` một chiều, `dp[i+1][j-1]` sẽ bị `dp[i][j-1]` ghi đè, trong hình đánh thứ tự bốn vị trí nàyđược duyệt:

![](https://labuladong.online/algo/images/space-optimal/3.jpeg)

**Vậy nếu ta muốn thu được `dp[i+1][j-1]`, thìphải trước khi nó bị ghi đè dùng một biến tạm `temp` lưu nó, mà giữ giá trị biến này tới lúc tính `dp[i][j]`**. Để đạt mục đích của này, kết hợp hình trên, ta viết code thế này:




```java
for (int i = n - 2; i >= 0; i--) {
    // Biến lưu dp[i+1][j-1]
    int pre = 0;
    for (int j = i + 1; j < n; j++) {
        int temp = dp[j];
        if (s.charAt(i) == s.charAt(j)) {
            // dp[i][j] = dp[i+1][j-1] + 2;
            dp[j] = pre + 2;
        } else {
            dp[j] = Math.max(dp[j], dp[j - 1]);
        }
        // Tới vòng tiếp theo, pre chính là dp[i+1][j-1]
        pre = temp;
    }
}
```



Đừng coi thường đoạn code này, đây là chỗtinh diệu nhất của `dp` một chiều, biết thì không khó, không biết thì khó. Để rõ ràng, mình dùng giá trị số cụ thể phân tích chi tiết logic này:

Giả sử giờ `i = 5, j = 7` mà `s[5] == s[7]`, vậy giờ sẽ vào logic dưới đúng không:




```java
for (int i = 5; i--) {
    for (int j = 7; j++) {
        if (s[5] == s[7]) {
            // dp[5][7] = dp[i+1][j-1] + 2;
            dp[7] = pre + 2;
        }
    }
}
```



Mình hỏi bạn biến `pre` này là gì? Là giá trị `temp` của lần lặp trước của vòng for trong.

Vậy mình hỏi tiếp **lần lặp trước của vòng for trong** giá trị `temp` là gì? Là `dp[j-1]` chính là `dp[6]`, nhưng chú ý, đây là `dp[6]` tương ứng **lần lặp trước của vòng for ngoài**, không phải `dp[6]` hiện tại.

Cái này phải tương ứng chỉ số mảng hai chiều hiểu. `dp[6]` hiện tại của bạn là `dp[i][6] = dp[5][6]` trong mảng hai chiều, mà `temp` này là `dp[i+1][6] = dp[6][6]` trong mảng hai chiều.

tức là, biến `pre` chính là `dp[i+1][j-1] = dp[6][6]`, chính là kết quả ta muốn.

Vậy giờ ta giảm chiều thành công phương trình chuyển trạng thái, tính là gặm xong khúc xương cứng nhất, nhưng chú ý ta còn base case phải xử lý nhé:

```java
// Mảng dp khởi tạo toàn bộ là 0
int[][] dp = new int[n][n];
// base case
for (int i = 0; i < n; i++) {
    dp[i][i] = 1;
}
```

Làm sao đem base case cũng đánh thành một chiều? Rất đơn giản, nhớ nén không gian chính là phép chiếu, ta đem base case phép chiếu xuống một chiều xem:

![](https://labuladong.online/algo/images/space-optimal/4.jpeg)

base case trong mảng `dp` hai chiềurơi vào mảng `dp` một chiều, không tồn tại xung đột và ghi đè, nên nói ta viết code thẳng thế này là được:

```java
// base case: mảng dp một chiều khởi tạo toàn bộ là 1
int[] dp = new int[n];
Arrays.fill(dp, 1);
```

Tới đây, ta đem base case và phương trình chuyển trạng thái đều giảm chiều, thực tế đã viết ra code đầy đủ:

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        // base case: mảng dp một chiều khởi tạo toàn bộ là 1
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        for (int i = n - 2; i >= 0; i--) {
            int pre = 0;
            for (int j = i + 1; j < n; j++) {
                int temp = dp[j];
                // Phương trình chuyển trạng thái
                if (s.charAt(i) == s.charAt(j))
                    dp[j] = pre + 2;
                else
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                pre = temp;
            }
        }
        return dp[n - 1];
    }
}
```

Bài nàythì kết thúc, nhưng kỹ thuật nén không gian lại rất hayhay, cũng dựa trên ý tưởng quy hoạch động thông thường.

Bạn cũng thấy, dùng kỹ thuật nén không gian giảm chiều mảng `dp` hai chiều rồi, khả năng đọc của code cách giải biến đổi được rất kém, nếu xem thẳng cách giải này, ai cũng mặt đơ. tối ưu thuật toán chính là quá trình vậy, viết trước thuật toán đệ quy vét cạn khả năng đọc rất tốt, rồi thử vận dụng kỹ thuật quy hoạch động tối ưu bài toán con trùng lặp, cuối cùng thử dùng kỹ thuật nén không gian tối ưu độ phức tạp không gian.

tức là, bạntối t hiểu thành thạo luyện vận dụng mô-típ trong bài trước [Giải thích mô-típ khung quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) tìmphương trình chuyển trạng thái, viết một cách giải quy hoạch động đúng, rồi mới có thể quan sát tình hình chuyển trạng thái, phân tích có thể dùng kỹ thuật nén không gian tối ưu không.

Hy vọng bạn đọcvững chắc, từng lớp, với tối ưu cực giới hạn thế này, không làm cũngthôi. Dù sao hiểu trong lòng, đi khắp thiên hạ đều không sợ!





<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Một chiêu quét sạch bài mua bán cổ phiếu trên LeetCode](https://labuladong.online/algo/dynamic-programming/stock-problem-summary/)
 - [Quy hoạch động: Tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/)
 - [Khung mô-típ giải bài quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)
 - [Thiết kế quy hoạch động: Mảng con lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/)
 - [Tư duy khung khi học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Quy hoạch động kinh điển: Bài toán ba lô tập con](https://labuladong.online/algo/dynamic-programming/knapsack2/)
 - [Quy hoạch động kinh điển: Bài toán ba lô đầy đủ](https://labuladong.online/algo/dynamic-programming/knapsack3/)
 - [Quy hoạch động kinh điển: Dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/)
 - [Quy hoạch động kinh điển: Thả trứng trên nhà cao tầng](https://labuladong.online/algo/dynamic-programming/egg-drop/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/?show=1)| [63. Đường khác nhau II](https://leetcode.cn/problems/unique-paths-ii/?show=1)| 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

