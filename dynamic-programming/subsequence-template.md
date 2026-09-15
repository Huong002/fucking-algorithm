# Template giải bài toán dãy con trong quy hoạch động



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1312. Minimum Insertion Steps to Make a String Palindrome](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)| [1312. Số lần chèn ít nhất để chuỗi thành đối xứng](https://leetcode.cn/problems/minimum-insertion-steps-to-make-a-string-palindrome/)| 🔴 |
| [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)| [516. Dãy con đối xứng dài nhất](https://leetcode.cn/problems/longest-palindromic-subsequence/)| 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Bài toán dãy con là bài toán thuật toán thường gặp, mà không dễ giải.

Trước hết, bài toán dãy convốn đã tương đối khó hơn chuỗi con (substring), mảng con (su barray), vì trước bên là dãy không liên tục, mà hai cái sau liên tục, ngay cả liệt kêbạn chưa chắc đã liệt kê được, đừng nói tìm giải bài toán thuật toán liên quan.

Mà, bài toán dãy con rất có thể liên quan tới hai chuỗi, ví dụ bài trước [Dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/), nếukhông có kinh nghiệm xử lý nhất định, thật không dễ nghĩ ra. Nên bài nàymổ xẻ mô-típ bài toán dãy con, thật rathì có hai template, bài liên quan cứ hướng hai ý tưởng này mà nghĩ, chắc ăn.

Nói chung, dạng bài này đều bắt bạn tìm một **dãy con dài nhất**, vì dãy con ngắn nhất chính là một ký tự mà, không có gìđáng hỏi. Một khi liên quan dãy con và giá trị cực trị, thì chắc chắn **khảo kỹ thuật quy hoạch động, độ phức tạp thời gian thường là O(n^2)**.

Nguyên nhân rất đơn giản, bạn nghĩ xem một chuỗi, dãy con của nó có bao nhiêu khả năng?ít nhất là hàm mũ nhé, trong trường hợp này, không dùng kỹ thuật quy hoạch động, cònmuốn sao nữa?

đã biết Đã dùng quy hoạch động, thì phải định nghĩa mảng `dp`, tìm quan hệ chuyển trạng thái. Hai ý tưởng template ta nói, chính là ý tưởng định nghĩa mảng `dp`. Bài khác nhau có thể cần định nghĩa mảng `dp` khác nhau để giải.

## Một, hai ý tưởng






**1、 ý tưởng template thứ nhất là mảng `dp` một chiều**:

```java
int n = array.length;
int[] dp = new int[n];

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        dp[i] = giá trị tối ưu /giá trị cực trị(dp[i], dp[j] + ...)
    }
}
```

Ví dụ [Dãy con tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/) và[Tổng mảng con lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/) tađã viết đều là ý tưởng này.

Trong ý tưởng này định nghĩa mảng `dp` là:

**Trong mảng con `arr[0..i]`, độ dài dãy con kết thúc bằng `arr[i]` là `dp[i]`**.

Vì sao dãy con tăng dài nhất cần ý tưởng này? Bài trước nói rất rõ, vì như vậyphù hợp quy nạp, tìm được quan hệ chuyển trạng thái, ở đây không mở rộng cụ thể nữa.

**2、 ý tưởng template thứ hai là mảng `dp` hai chiều**:

```java
int n = arr.length;
int[][] dp = new dp[n][n];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (arr[i] == arr[j])
            dp[i][j] = dp[i][j] + ...
        else
            dp[i][j] = giá trị tối ưu (...)
    }
}
```

ý tưởng nàyrelatively dùng nhiều hơn, nhất là khi liên quan dãy con của hai chuỗi/mảng, ví dụ bài trước trình bày [Dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/) và[Khoảng cách chỉnh sửa](https://labuladong.online/algo/dynamic-programming/edit-distance/); ý tưởng này cũng dùng được chochỉ một chuỗi/mảng, ví dụ bài toán dãy con đối xứng trình bày trong bài này.

**2.1 tình huống liên quan hai chuỗi/mảng**, định nghĩa mảng `dp` như sau:

**Trong mảng con `arr1[0..i]` và mảng con `arr2[0..j]`, độ dài dãy con ta yêu cầu là `dp[i][j]`**.

**2.2 Chỉ liên quan một chuỗi/mảng**, định nghĩa mảng `dp` như sau:

**Trong mảng con `array[i..j]`, độ dài dãy con ta yêu cầu là `dp[i][j]`**.

Dưới đâythì xem bài toán dãy con đối xứng dài nhất, giải chi tiết trường hợp thứ hai dùng quy hoạch động thế nào.

## Hai, dãy con đối xứng dài nhất

Trước đây giải bài [Chuỗi con đối xứng dài nhất](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/), lầnnày nâng độ khó, xem bài 516「Dãy con đối xứng dài nhất」trên LeetCode, tìm độ dài dãy con đối xứng dài nhất:

Nhập một chuỗi `s`, hãy tìm độ dài dãy con đối xứng dài nhất trong `s`, chữ ký hàm như sau:

```java
int longestPalindromeSubseq(String s);
```

Ví dụ nhập `s = "aecda"`, thuật toán trả về 3, vì dãy con đối xứng dài nhất là `"aca"`, độ dài là 3.

Ta định nghĩa mảng `dp` là: **trong chuỗi con `s[i..j]`, độ dài dãy con đối xứng dài nhất là `dp[i][j]`**. Nhất định phải nhớ định nghĩa này mới hiểu được thuật toán.

Vì sao bài này phải định nghĩa mảng `dp` hai chiều vậy? Trong [Dãy con tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/) mìnhđề cập tới, tìm chuyển trạng thái cần tư duy quy nạp, nói thẳng chính làm sao từ kết quả đã biết suy ra phần chưa biết. Mà định nghĩa như vậy quy nạp được, dễ phát hiện quan hệ chuyển trạng thái.

Cụ thể, nếu ta muốn tìm `dp[i][j]`, giả sử bạn đã biết kết quả bài toán con `dp[i+1][j-1]` (độ dài dãy con đối xứng dài nhất trong `s[i+1..j-1]`), bạn có nghĩ cáchtính ra giá trị `dp[i][j]` (trong `s[i..j]`, độ dài dãy con đối xứng dài nhất) không?

![](https://labuladong.online/algo/images/lps/1.jpg)

Được! Điều này phụ thuộc ký tự `s[i]` và `s[j]`:

**Nếu chúng bằng nhau**, thì chúng cộng với dãy con đối xứng dài nhất trong `s[i+1..j-1]` chính là dãy con đối xứng dài nhất của `s[i..j]`:

![](https://labuladong.online/algo/images/lps/2.jpg)

**Nếu chúng không bằng nhau**, cho thấy chúng **không thể đồng thời** xuất hiện trong dãy con đối xứng dài nhất của `s[i..j]`, vậy đem chúng **lần lượt**thêm vào `s[i+1..j-1]`, xem chuỗi con nào sinh dãy con đối xứng dài hơn là được:

![](https://labuladong.online/algo/images/lps/3.jpg)

Hai trường hợp trên viết thành code chính là vậy:




```java
if (s[i] == s[j])
    // Chúng nhất định nằm trong dãy con đối xứng dài nhất
    dp[i][j] = dp[i + 1][j - 1] + 2;
else
    // s[i+1..j] và s[i..j-1] dãy con đối xứng của ai dài hơn?
    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
```



Tới đây, phương trình chuyển trạng tháithì viết ra, theo định nghĩa mảng dp, thứ ta yêu cầu chính là `dp[0][n - 1]`, chính là độ dài dãy con đối xứng dài nhất của toàn bộ `s`.

## Ba, hiện thực code

Trước hết rõ ràng base case, nếu chỉ có một ký tự, hiển nhiên độ dài dãy con đối xứng dài nhất là 1, chính là `dp[i][j] = 1 (i == j)`.

Vì `i` chắc chắn nhỏ hơn hoặc bằng `j`, nên với những vị trí `i > j`, căn bản không tồn tại dãy con gì, nên khởi tạo là 0.

Ngoài ra, xem phương trình chuyển trạng thái vừa viết, muốn tìm `dp[i][j]` cần biết ba vị trí `dp[i+1][j-1]`, `dp[i+1][j]`, `dp[i][j-1]`; lại xem base case ta xác định, sau khi điền vào mảng `dp` là thế này:

![](https://labuladong.online/algo/images/lps/4.jpg)

**Để đảm bảo mỗi lần tính `dp[i][j]`, vị trí hướngtrái-dưới-phải đã tính xong, chỉchéo hoặc ngược mà duyệt**:

![](https://labuladong.online/algo/images/lps/5.jpg)

> [!TIP]
> Về hướng duyệt mảng `dp`, chi tiết xem [Giải đáp quy hoạch động](https://labuladong.online/algo/dynamic-programming/faq-summary/).

Mình chọn duyệt ngược, code như sau:

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
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
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/longest-palindromic-subsequence/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🥳 Animation trực quan hóa code 🥳</s trong>
</summary>
</details>
</a>
<hr/>



Tới đây, bài toán dãy con đối xứng dài nhấtthì giải xong.

## Bốn, mở rộng

Tuy bài liên quan đối xứng không có tình huống dùng đặc biệt rộng, nhưng bạn tính được dãy con đối xứng dài nhất rồi, vài bài tương tự cũng tiện tay làm luôn.

Ví dụ bài 1312「Tính số lần chèn ít nhất để chuỗi thành đối xứng 」trên LeetCode:

Nhập một chuỗi `s`, bạn có thể chèn ký tự bất kỳ tại vị trí bất kỳ trong chuỗi. Nếu muốn biến `s` thành đối xứng, hãy tính ít nhất phải chèn mấy lần?

Chữ ký hàm như sau:

```java
int minInsertions(String s);
```

Ví dụ nhập `s = "abcea"`, thuật toán trả về 2, vì có thể chèn 2 ký tự vào `s` thành đối xứng `"abeceba"` hoặc `"aebcbea"`. Nếu nhập `s = "aba"`, thuật toán trả về 0, vì `s` đã là đối xứng, không cần chèn ký tự nào.

Đây cũng là bài toán dãy con đơn chuỗi, nên ta cũng dùng mảng `dp` hai chiều, trong đó định nghĩa `dp[i][j]` như sau:

**Với chuỗi `s[i..j]`, ít nhất cần `dp[i][j]` lần chèn mới thành đối xứng **.

Theo định nghĩa mảng `dp`, base case chính là `dp[i][i] = 0`, vì ký tự đơnvốn đã là đối xứng, không cần chèn.

Rồi dùng quy nạp toán học, giả sử đã tính giá trị bài toán con `dp[i+1][j-1]`, nghĩ cách suy ra giá trị `dp[i][j]`:

![](https://labuladong.online/algo/images/palindrome-insert/1.jpeg)

Thật ra rất tương tự phương trình chuyển trạng thái của bài dãy con đối xứng dài nhất, ở đây cũng chia hai trường hợp:




```java
if (s[i] == s[j]) {
    // Không cần chèn ký tự nào
    dp[i][j] = dp[i + 1][j - 1];
} else {
    // Biến s[i+1..j] và s[i..j-1] thành đối xứng, chọn bên chèn ít hơn
    // rồi còn phải chèn thêm một s[i] hoặc s[j], để s[i..j] ghép thành đối xứng
    dp[i][j] = min(dp[i + 1][j], dp[i][j - 1]) + 1;
}
```



Cuối cùng, ta vẫn duyệt ngược mảng `dp`, viết code:

```java
class Solution {
    public int minInsertions(String s) {
        int n = s.length();
        // dp[i][j] cho biết số lần chèn ít nhất để biến chuỗi s[i..j] thành đối xứng
        // Mảng dp khởi tạo toàn bộ là 0
        int[][] dp = new int[n][n];
        // Duyệt ngược đảm bảo chuyển trạng thái đúng
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                // Phương trình chuyển trạng thái
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i + 1][j], dp[i][j - 1]) + 1;
                }
            }
        }
        // Số lần chèn ít nhất của toàn bộ s
        return dp[0][n - 1];
    }
}
```

Tới đây, bài này cũng dùng template giải bài dãy con giải xong, logic tổng thể rất tương tự dãy con đối xứng dài nhất, vậy bài này có thể tái sử dụng trực tiếp cách giải dãy con đối xứng không?

Thật ra được, ta thậm chí còn không cần viết phương trình chuyển trạng thái, bạn nghĩ kỹ xem:

**Mình tính trước dãy con đối xứng dài nhất trong chuỗi `s`, những ký tự không nằm trong dãy con đối xứng dài nhất, chẳng phải chính là ký tự cần chèn sao**?

Nên bài này tái sử dụng trực tiếp hàm `longestPalindromeSubseq` hiện thực trước đó:

```java
class Solution {
    // Tính số lần chèn ít nhất để biến s thành đối xứng
    public int minInsertions(String s) {
        return s.length() - longestPalindromeSubseq(s);
    }

    // Tính độ dài dãy con đối xứng dài nhất trong s
    int longestPalindromeSubseq(String s) {
        // Xem phần trên
    }
}
```

Rồi, thuật toán liên quan dãy conthì trình bày tới đây, hy vọng cógợi mở cho bạn.






<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Thiết kế quy hoạch động: Dãy con tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/)
 - [Giáng chiều cho quy hoạch động (tối ưu không gian)](https://labuladong.online/algo/dynamic-programming/space-optimization/)
 - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)
 - [Quy hoạch động kinh điển: Dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

