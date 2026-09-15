# Quy hoạch động kinh điển: Dãy con chung dài nhất (LCS)



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được套路/mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu (力扣) | Độ khó |
| :----: | :----: | :----: |
| [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | [1143. Dãy con chung dài nhất](https://leetcode.cn/problems/longest-common-subsequence/) | 🟠 |
| [583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | [583. Thao tác xóa cho hai chuỗi](https://leetcode.cn/problems/delete-operation-for-two-strings/) | 🟠 |
| [712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/) | [712. Tổng ASCII xóa nhỏ nhất cho hai chuỗi](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Không biết mọi người làm bài thuật toán có cảm nhận gì, mình đúc kết技巧/mẹo làm bài thuật toán là:細化/thu nhỏ bài toán lớn về một điểm, nghiên cứu trước cách giải quyết tại điểm nhỏ đó, rồi mở rộng ra toàn bộ bài toán bằng đệ quy/lặp.

Ví dụ như ở bài trước [Dẫn bạn刷/quét cây nhị phân kỳ 3](https://labuladong.online/algo/data-structure/binary-tree-part3/), khi giải bài cây nhị phân, ta sẽ thu nhỏ toàn bộ vấn đề về một node nào đó, tưởng tượng mình đang đứng tại node đó thì cần làm gì, rồi áp khung đệ quy cây nhị phân vào là xong.

Dạng bài quy hoạch động (DP) cũng vậy, nhất là các bài liên quan đến dãy con (subsequence). **Bài này xuất phát từ「Bài toán dãy con chung dài nhất」và tổng kết ba bài toán dãy con**, giải kỹ bài này về套路/mô-típ dạng bài dãy con, bạn sẽ cảm nhận được cách tư duy này.

## Dãy con chung dài nhất

Tính dãy con chung dài nhất (Longest Common Subsequence, gọi tắt là LCS) là một bài quy hoạch động kinh điển, bài 1143「Dãy con chung dài nhất」trên LeetCode chính là bài này:

Cho đầu vào là hai chuỗi `s1` và `s2`, hãy tìm dãy con chung dài nhất của chúng và trả về độ dài của dãy con đó. Chữ ký hàm như sau:

```java
int longestCommonSubsequence(String s1, String s2);
```

Ví dụ nhập `s1 = "zabcde", s2 = "acez"`, dãy con chung dài nhất của chúng là `lcs = "ace"`, độ dài là 3, nên thuật toán trả về 3.

Nếu chưa làm bài này bao giờ, một thuật toán暴力/brute-force đơn giản nhất là liệt kê tất cả các dãy con (subsequence) của `s1` và `s2`, rồi xem có dãy nào chung không, rồi trong tất cả các dãy con chung lại tìm một dãy dài nhất.

Rõ ràng,思路/cách nghĩ này có độ phức tạp rất cao, bạn phải liệt kê mọi dãy con, độ phức tạp này là hàm mũ, chắc chắn không thực tế.

思路 đúng là đừng xét cả chuỗi, mà hãy細化/thu nhỏ về từng ký tự của `s1` và `s2`. Một规律/quy luật đã tổng kết trong bài trước [Template giải bài toán dãy con](https://labuladong.online/algo/dynamic-programming/subsequence-problem/):





**Với bài toán求/tìm dãy con trên hai chuỗi, đều dùng hai con trỏ `i` và `j` di chuyển lần lượt trên hai chuỗi, xác suất lớn là思路 quy hoạch động**.

Bài toán dãy con chung dài nhất cũng tuân theo规律 này, ta có thể viết trước một hàm `dp`:

```java
// Định nghĩa: tính độ dài dãy con chung dài nhất (LCS) của s1[i..] và s2[j..]
int dp(String s1, int i, String s2, int j)
```

Theo định nghĩa của hàm `dp` này, đáp án ta muốn chính là `dp(s1, 0, s2, 0)`, và base case là khi `i == len(s1)` hoặc `j == len(s2)`, vì lúc này `s1[i..]` hoặc `s2[j..]` tương đương với chuỗi rỗng, độ dài dãy con chung dài nhất hiển nhiên là 0:

```java
int longestCommonSubsequence(String s1, String s2) {
    return dp(s1, 0, s2, 0);
}

// Định nghĩa: tính độ dài dãy con chung dài nhất của s1[i..] và s2[j..]
int dp(String s1, int i, String s2, int j) {
    // base case
    if (i == s1.length() || j == s2.length()) {
        return 0;
    }
    // ...
}
```

**Tiếp theo, đừng nhìn hai chuỗi `s1` và `s2` nữa, mà phải cụ thể đến từng ký tự, suy nghĩ mỗi ký tự nên làm gì**.

![](https://labuladong.online/algo/images/LCS/1.jpeg)

Ta chỉ nhìn `s1[i]` và `s2[j]`, **nếu `s1[i] == s2[j]`,说明/ký tự này chắc chắn nằm trong `lcs`**:

![](https://labuladong.online/algo/images/LCS/2.jpeg)

Như vậy, đã tìm được một ký tự trong `lcs`, theo định nghĩa hàm `dp`, ta có thể hoàn thiện code:

```java
// Định nghĩa: tính độ dài dãy con chung dài nhất của s1[i..] và s2[j..]
int dp(String s1, int i, String s2, int j) {
    if (s1.charAt(i) == s2.charAt(j)) {
        // s1[i] và s2[j] chắc chắn nằm trong lcs,
        // cộng thêm độ dài lcs trong s1[i+1..] và s2[j+1..] chính là đáp án
        return 1 + dp(s1, i + 1, s2, j + 1);
    } else {
        // ...
    }
}
```

Vừa rồi là trường hợp `s1[i] == s2[j]`, nhưng nếu `s1[i] != s2[j]` thì phải làm sao?

**`s1[i] != s2[j]`意味着/có nghĩa là, trong `s1[i]` và `s2[j]` ít nhất có một ký tự không nằm trong `lcs`**:

![](https://labuladong.online/algo/images/LCS/3.jpeg)

Như hình trên, tổng cộng có thể có ba trường hợp, làm sao mình biết cụ thể là trường hợp nào?

Thật ra ta cũng không biết, vậy thì tính hết đáp án của cả ba trường hợp, lấy kết quả lớn nhất trong đó, vì đề bài bắt ta tính độ dài dãy con chung「dài nhất」mà.

Đáp án của ba trường hợp này tính thế nào? Nhớ lại định nghĩa hàm `dp` của ta, nó chẳng phải được thiết kế chuyên để tính chúng sao!

Code có thể tiến thêm một bước:

```java
// Định nghĩa: tính độ dài dãy con chung dài nhất của s1[i..] và s2[j..]
int dp(String s1, int i, String s2, int j) {
    if (s1.charAt(i) == s2.charAt(j)) {
        return 1 + dp(s1, i + 1, s2, j + 1);
    } else {
        // Trong s1[i] và s2[j] ít nhất có một ký tự không nằm trong lcs,
        // liệt kê kết quả của ba trường hợp, lấy kết quả lớn nhất
        return max(
            // Trường hợp 1: s1[i] không nằm trong lcs
            dp(s1, i + 1, s2, j),
            // Trường hợp 2: s2[j] không nằm trong lcs
            dp(s1, i, s2, j + 1),
            // Trường hợp 3: cả hai đều không nằm trong lcs
            dp(s1, i + 1, s2, j + 1)
        );
    }
}
```

Tới đây đã rất gần đáp án cuối cùng rồi, **còn một优化/tối ưu nhỏ, trường hợp 3「cả `s1[i]` và `s2[j]` đều không nằm trong lcs」thật ra có thể bỏ qua trực tiếp**.

Vì ta đang求giá trị lớn nhất mà, trường hợp 3 tính độ dài `lcs` của `s1[i+1..]` và `s2[j+1..]`, độ dài này chắc chắn nhỏ hơn hoặc bằng độ dài `lcs` trong trường hợp 2 là `s1[i..]` và `s2[j+1..]`, vì `s1[i+1..]` ngắn hơn `s1[i..]` mà, thì `lcs` tính ra từ đó đương nhiên không thể dài hơn.

Tương tự, kết quả trường hợp 3 chắc chắn cũng nhỏ hơn hoặc bằng trường hợp 1. **Nói thẳng ra, trường hợp 3 đã bị trường hợp 1 và trường hợp 2 bao hàm**, nên ta có thể bỏ qua trực tiếp trường hợp 3, code đầy đủ như sau:

```java
class Solution {
    // Bảng ghi nhớ (memo), loại bỏ bài toán con trùng lặp
    int[][] memo;

    // Hàm chính
    public int longestCommonSubsequence(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Giá trị memo là -1 nghĩa là chưa từng tính
        memo = new int[m][n];
        for (int[] row : memo) 
            Arrays.fill(row, -1);
        // Tính độ dài lcs của s1[0..] và s2[0..]
        return dp(s1, 0, s2, 0);
    }

    // Định nghĩa: tính độ dài dãy con chung dài nhất của s1[i..] và s2[j..]
    int dp(String s1, int i, String s2, int j) {
        // base case
        if (i == s1.length() || j == s2.length()) {
            return 0;
        }
        // Nếu đã tính trước đó, trả về trực tiếp đáp án trong memo
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        // Lựa chọn theo trường hợp của s1[i] và s2[j]
        if (s1.charAt(i) == s2.charAt(j)) {
            // s1[i] và s2[j] chắc chắn nằm trong lcs
            memo[i][j] = 1 + dp(s1, i + 1, s2, j + 1);
        } else {
            // s1[i] và s2[j] ít nhất có một ký tự không nằm trong lcs
            memo[i][j] = Math.max(
                dp(s1, i + 1, s2, j),
                dp(s1, i, s2, j + 1)
            );
        }
        return memo[i][j];
    }
}
```

思路 trên hoàn toàn đi theo bài爆文/nổi tiếng trước đây của ta là [Khung套路/mô-típ quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/), hẳn là rất dễ hiểu. Còn vì sao phải thêm `memo` bảng ghi nhớ, trước đây ta đã viết nhiều lần, để照顾/chăm sóc bạn đọc mới tới, ở đây nhắc lại đơn giản một chút, trước hết抽象/trừu tượng hóa khung đệ quy của hàm `dp` cốt lõi:

```java
int dp(int i, int j) {
    dp(i + 1, j + 1); // #1
    dp(i, j + 1);     // #2
    dp(i + 1, j);     // #3
}
```

Bạn xem, giả sử mình muốn từ `dp(i, j)` chuyển sang `dp(i+1, j+1)`, có không chỉ một cách, có thể đi thẳng `#1`, cũng có thể đi `#2 -> #3`, cũng có thể đi `#3 -> #2`.

Đây chính là bài toán con trùng lặp (overlapping subproblems), nếu ta không dùng `memo` để loại bỏ bài toán con, thì `dp(i+1, j+1)` sẽ bị tính nhiều lần, điều này là không cần thiết.

Tới đây, bài toán dãy con chung dài nhất đã được giải triệt để, dùng思路 quy hoạch động top-down kèm bảng ghi nhớ, đương nhiên ta cũng có thể dùng思路 quy hoạch động bottom-up dạng lặp, giống hệt思路 đệ quy của ta, mấu chốt là định nghĩa mảng `dp` thế nào, mình cũng viết luôn解法/cách giải bottom-up ở đây:

```java
class Solution {
    public int longestCommonSubsequence(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int[][] dp = new int[m + 1][n + 1];
        // Định nghĩa: độ dài lcs của s1[0..i-1] và s2[0..j-1] là dp[i][j]
        // Mục tiêu: độ dài lcs của s1[0..m-1] và s2[0..n-1], tức dp[m][n]
        // base case: dp[0][..] = dp[..][0] = 0

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // Giờ i và j bắt đầu từ 1, nên phải trừ một
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    // s1[i-1] và s2[j-1] chắc chắn nằm trong lcs
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    // s1[i-1] và s2[j-1] ít nhất có một ký tự không nằm trong lcs
                    dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }

        return dp[m][n];
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/longest-common-subsequence/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code 🍭</strong>
</summary>
</details>
</a>
<hr/>



解法 bottom-up中 định nghĩa mảng `dp` có hơi khác với解法 đệ quy của ta, nhưng思路 hoàn toàn giống解法 đệ quy, nếu bạn hiểu解法 đệ quy thì解法 này hẳn không khó hiểu.

Người mới có thể có một chi tiết nhỏ không để ý: `s1.charAt(i - 1) == s2.charAt(j - 1)`, chỉ số chuỗi ở đây và chỉ số mảng `dp` không khớp nhau, còn gọi là **lệch chỉ số (index offset)**. Ôn lại định nghĩa mảng `dp`:

```java
        // Định nghĩa: độ dài lcs của s1[0..i-1] và s2[0..j-1] là dp[i][j]
```

Ở vòng lặp thứ `i`, thứ bị thay đổi là giá trị `dp[i]`, lúc này theo định nghĩa, thứ cần so sánh là **ký tự thứ i** của `s1`,也就是/tức là `s1[i-1]`. `s2[j-1]` cũng tương tự. Chi tiết này rất hay gặp trong DP dạng chuỗi, cần留意/lưu ý thêm.

Ngoài ra,解法 bottom-up có thể优化 bằng [Kỹ thuật nén không gian DP](https://labuladong.online/algo/dynamic-programming/space-optimization/) đã讲/nói trong bài trước, nén độ phức tạp không gian xuống O(N), ở đây vì篇幅/giới hạn độ dài nên không展开/mở rộng.

Dưới đây, xem hai bài tương tự với dãy con chung dài nhất.

## Thao tác xóa chuỗi

Đây là bài 583「Thao tác xóa cho hai chuỗi」trên LeetCode, xem đề:

Cho hai từ `s1` và `s2`, trả về số bước tối thiểu để khiến `s1` và `s2` giống nhau. Mỗi bước có thể xóa một ký tự bất kỳ trong một chuỗi.

Chữ ký hàm như sau:

```java
int minDistance(String s1, String s2);
```

Ví dụ nhập `s1 = "sea" s2 = "eat"`, thuật toán trả về 2, bước một biến `"sea"` thành `"ea"`, bước hai biến `"eat"` thành `"ea"`.

Đề bài bắt ta tính số lần xóa ít nhất để hai chuỗi trở nên giống nhau, vậy ta có thể nghĩ xem, cuối cùng hai chuỗi này sẽ bị xóa thành样子/hình dạng gì?

Kết quả sau khi xóa chẳng phải chính là dãy con chung dài nhất của chúng sao!

Vậy, muốn tính số lần xóa, có thể suy ra từ độ dài dãy con chung dài nhất:

```java
int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    // Tái sử dụng hàm tính độ dài lcs ở phần trước
    int lcs = longestCommonSubsequence(s1, s2);
    return m - lcs + n - lcs;
}
```

Bài này giải xong!

## Tổng ASCII xóa nhỏ nhất

Đây là bài 712「Tổng ASCII xóa nhỏ nhất cho hai chuỗi」trên LeetCode, đề tương tự bài trước, chỉ là bài trước yêu cầu tối thiểu hóa số lần xóa, bài này yêu cầu tối thiểu hóa tổng mã ASCII của các ký tự bị xóa.

Chữ ký hàm như sau:

```java
int minimumDeleteSum(String s1, String s2)
```

Ví dụ nhập `s1 = "sea", s2 = "eat"`, thuật toán trả về 231.

Vì xóa `"s"` trong `"sea"`, xóa `"t"` trong `"eat"`, có thể khiến hai chuỗi bằng nhau, và tổng mã ASCII của ký tự bị xóa là nhỏ nhất, tức `s(115) + t(116) = 231`.

**Bài này không thể复用/tái sử dụng trực tiếp hàm tính dãy con chung dài nhất, nhưng có thể làm theo思路 trước đó, sửa nhẹ base case và phần chuyển trạng thái là viết thẳng được code解法**:

```java
class Solution {
    // Bảng ghi nhớ
    int memo[][];
    // Hàm chính
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Giá trị memo là -1 nghĩa là chưa từng tính
        memo = new int[m][n];
        for (int[] row : memo) 
            Arrays.fill(row, -1);
        
        return dp(s1, 0, s2, 0);
    }

    // Định nghĩa: xóa s1[i..] và s2[j..] thành chuỗi giống nhau,
    // tổng mã ASCII nhỏ nhất là dp(s1, i, s2, j).
    int dp(String s1, int i, String s2, int j) {
        int res = 0;
        // base case
        if (i == s1.length()) {
            // Nếu s1 đã tới cuối, thì phần còn lại của s2 đều phải xóa
            for (; j < s2.length(); j++)
                res += s2.charAt(j);
            return res;
        }
        if (j == s2.length()) {
            // Nếu s2 đã tới cuối, thì phần còn lại của s1 đều phải xóa
            for (; i < s1.length(); i++)
                res += s1.charAt(i);
            return res;
        }
        
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        
        if (s1.charAt(i) == s2.charAt(j)) {
            // s1[i] và s2[j] đều nằm trong lcs, không cần xóa
            memo[i][j] = dp(s1, i + 1, s2, j + 1);
        } else {
            // s1[i] và s2[j] ít nhất có một ký tự không nằm trong lcs, xóa một ký tự
            memo[i][j] = Math.min(
                s1.charAt(i) + dp(s1, i + 1, s2, j),
                s2.charAt(j) + dp(s1, i, s2, j + 1)
            );
        }
        return memo[i][j];
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/minimum-ascii-delete-sum-for-two-strings/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code 🍭</strong>
</summary>
</details>
</a>
<hr/>



base case có khác biệt nhất định, khi tính độ dài `lcs`, nếu một chuỗi rỗng thì độ dài `lcs`必然/chắc chắn là 0; nhưng bài này nếu một chuỗi rỗng, chuỗi còn lại必然 phải bị xóa toàn bộ, nên cần tính tổng mã ASCII của mọi ký tự trong chuỗi còn lại.

Về chuyển trạng thái, khi `s1[i]` và `s2[j]` giống nhau thì không cần xóa, khi khác nhau thì cần xóa, nên có thể利用/dùng hàm `dp` tính hai trường hợp,得出/đưa ra kết quả tối ưu. Các chỗ khác大同小异/tương tự nhau, không展开 cụ thể nữa.

Tới đây, ba bài toán dãy con đã giải xong, mấu chốt là細化/thu nhỏ bài toán về ký tự, dựa vào mỗi cặp ký tự có giống nhau không để判断/phán đoán chúng có nằm trong dãy con kết quả không, từ đó tránh liệt kê mọi dãy con.

Đây cũng coi là思路 thường dùng khi求/tìm dãy con trong hai chuỗi,建议/khuyến nghị体会/ngẫm kỹ, luyện tập nhiều~







<hr>
<details class="hint-container details">
<summary><strong>Các bài viết trích dẫn bài này</strong></summary>

 - [Template giải bài toán dãy con trong DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Các bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin刷题/làm bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồi mở các bài dưới đây để xem thẳng思路 giải:</strong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [97. Interleaving String](https://leetcode.com/problems/interleaving-string/?show=1) | [97. Chuỗi đan xen](https://leetcode.cn/problems/interleaving-string/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 095. Dãy con chung dài nhất](https://leetcode.cn/problems/qJnOS7/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
