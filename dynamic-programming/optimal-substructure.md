# Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

> tip: Bài này có bản video: [Giải thích nâng cao quy hoạch động](https://www.bilibili.com/video/BV1uv411W73P/).Khuyếnnghị follow tài khoản Bilibili của mình, mình sẽ dẫn đọc bằng video các kỹ thuật thuật toán hơi khó.



Bài này là bản sửa của bài cũ [Giải đáp quy hoạch động](https://mp.weixin.qq.com/s/qvlfyKBiXVX7CCwWFR-XKg), theohọc hỏi tổng kết không ngừng của mình và bình luận của bạn đọc, mình mở rộng thêm nhiều nội dung, quyết khiến bài này thành sau [Khung mô-típ cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) mộtbài giải đáp toàn diện. Dưới đây là chính văn.

Bài viết này sẽ trình bày rõ cho bạn mấy vấn đề:

1、Rốt cuộc gì gọi/「cấu trúc con tối ưu」, quan hệ gì với quy hoạch động.

2、Làm sao phán đoán một bài có phải bài quy hoạch động, tức làm sao xem ra có tồn tại bài toán con trùng lặp không.

3、Vì sao hay thấy kích thước mảng `dp` đặt là `n + 1` chứ không phải `n`.

4、Vì sao duyệt mảng `dp` trong quy hoạch độngđủ kiểu, có cái duyệt xuôi, có cái duyệt ngược, có cái duyệt chéo.






## Một, giải chi tiết cấu trúc con tối ưu

「Cấu trúc con tối ưu」là một tính chấtđặc thù của vài bài toán, chứ không phảiriêng của quy hoạch động. chính là nói, nhiều bài toán thật ra đều có cấu trúc con tối ưu, chỉ là phần lớn chia trong đó không có bài toán con trùng lặp, nên ta không xếp chúng vào dòng quy hoạch động mà thôi.

Mình lấy ví dụ rất dễ hiểu trước: giả sử trường bạn có 10 lớp, bạn đã tính điểm thi cao nhất của mỗi lớp. Vậy giờ mình bắt bạn tính điểm cao nhất toàn trường, bạn tính được không? Đương nhiên được, mà bạn không cần duyệt lại điểm của mọi học sinh toàn trường để so, chỉ cần lấy lớn nhất trong 10 điểm cao nhất này chính là điểm cao nhất toàn trường.

Vấn đề mình nêu cho bạnthì **phù hợp cấu trúc con tối ưu**: từ kết quả tối ưu của bài toán con suy ra ra kết quả tối ưu của bài toán quy mô lớn hơn. Bắt bạn tính điểm tối ưu của **mỗi lớp** chính là bài toán con, bạn biết đáp án mọi bài toán con rồi, nhờ đó suy ra đáp án của bài toán quy mô lớn hơn là điểm tối ưu của **toàn trường**.

Bạn xem, bài đơn giản vậy đều có tính chất cấu trúc con tối ưu, chỉ vì hiển nhiên không có bài toán con trùng lặp, nên ta tìm giá trị tối ưu đơn giản chắc chắn không dùng quy hoạch động.

Lấy thêm ví dụ: giả sử trường bạn có 10 lớp, bạn biết chênh lệch điểm lớn nhất mỗi lớp (hiệu điểm cao nhất và thấp nhất). Vậy giờ mình bắt bạn tính chênh lệch điểm lớn nhất trong học sinh toàn trường, bạn tính được không? Nghĩ cáchtính được, nhưng chắc chắn không thể qua chênh lệch lớn nhất đã biết của 10 lớp này suy ra ra. Vì chênh lệch lớn nhất của 10 lớp nàychưa chắc đã chứa chênh lệch lớn nhất toàn trường, ví dụ chênh lệch lớn nhất toàn trường có thể là hiệu điểm cao nhất lớp 3 và điểm thấp nhất lớp 6.

Lần này vấn đề mình nêuthì **không phù hợp cấu trúc con tối ưu**, vì bạn không cách nào qua giá trị tối ưu mỗi lớp suy ra giá trị tối ưu toàn trường, không cách nào qua giá trị tối ưu bài toán con suy ra giá trị tối ưu bài toán quy mô lớn hơn. Bài trước [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) nói, muốn thỏa cấu trúc con tối ưu, bài toán con giữa phải độc lập nhau. Chênh lệch điểm lớn nhất toàn trường có thể xuất hiện giữa hai lớp, hiển nhiên bài toán con không độc lập, nên bản thân vấn đề này không phù hợp cấu trúc con tối ưu.

**Vậy gặp tình huống cấu trúc con tối ưuhỏng, làm sao? Chiến lược là: cải tạo bài toán**. Với bài chênh lệch điểm lớn nhất, ta chẳng phải không cách nào dùng dùng chênh lệch đã biết của mỗi lớp sao, vậy mình chỉ viết đoạn code vét cạn thế này:




```java
int result = 0;
for (Student a : school) {
    for (Student b : school) {
        if (a is b) continue;
        result = max(result, |a.score - b.score|);
    }
}
return result;
```


Cải tạo bài toán, chính là đem bài toántương đương: chênh lệch điểm lớn nhất, chẳng phải tương đương bằng hiệu điểm cao nhất và điểm thấp nhất sao, vậy chẳng phải chính là tìm điểm cao nhất và thấp nhất sao, chẳng phải chính là bài đầu ta thảo luận sao, chẳng phảithì có cấu trúc con tối ưu sao? Vậy giờ đổi ý tưởng, mượn cấu trúc con tối ưu giải bài giá trị tối ưu, rồi quay lại giải bài chênh lệch điểm lớn nhất, có phải hiệu quả hơn nhiều?

Đương nhiên, ví dụ trên quá đơn giản, nhưng mời bạn đọc ôn lại, ta làm bài quy hoạch động, có phải luôn tại tìm đủ loại giá trị tối ưu, bản chất không khác ví dụ ta nêu, chẳng qua cần xử lý bài toán con trùng lặp.

Bài trước [Bài toán thả trứng trên nhà cao tầng](https://labuladong.online/algo/dynamic-programming/egg-drop/) đã trình bàycáchcải tạo bài toán, cấu trúc con tối ưu khác nhau, có thể dẫn cách giải và hiệu quả khác nhau.

Lấy thêm ví dụ thường gặp nhưng cũng rất đơn giản, tìm giá trị lớn nhất của một cây nhị phân, không khó nhé (đơn giản, giả sử giá trị trong node đều không âm):

```java
int maxVal(TreeNode root) {
    if (root == null)
        return -1;
    int left = maxVal(root.left);
    int right = maxVal(root.right);
    return max(root.val, left, right);
}
```

Bạn xem bài này cũng phù hợp cấu trúc con tối ưu, giá trị lớn nhất của cây gốc `root`, suy ra được qua giá trị lớn nhất của hai cây con (bài toán con), kết hợp ví dụ trường và lớp vừa rồi, rất dễ hiểu nhé.

Đương nhiên đây cũng không phải bài quy hoạch động, mục đích cho thấy, cấu trúc con tối ưu không phải tính chấtriêng của quy hoạch động, bài tìm giá trị tối ưu phần lớn chia đều có tính chất này; **nhưng ngược lại, tính chất cấu trúc con tối ưu là điều kiện cần của bài toán quy hoạch động, nhất định bắt bạn tìm giá trị tối ưu **, sau này gặp bài khó chịu, ý tưởng tới quy hoạch động mà nghĩthì đối với, đây chính là mô-típ.

Quy hoạch động chẳng phải từ base case đơn giản nhất suy ra về sau sao, tưởng tượng thành một phản ứng dây chuyền, lấy nhỏlấy lớn. Nhưng chỉ bài phù hợp cấu trúc con tối ưu, mới có tính chất xảy ra phản ứng dây chuyền này.

Quá trình tìm cấu trúc con tối ưu, thật ra chính là quá trình chứng minh phương trình chuyển trạng thái đúng, phương trình phù hợp cấu trúc con tối ưuthì có thể lấy viết lời giải vét cạn, viết lời giải vét cạn thì có thể lấy xem ra có bài toán con trùng lặp không, có thì tối ưu, không thì OK. Đây cũng là mô-típ, bạn đọc hay cày bài hẳn hiểu sâu được.

Ở đây không lấy những ví dụ quy hoạch độngchính tông, bạn đọc lật bài lịch sử, xem chuyển trạng thái tuân theo cấu trúc con tối ưu thế nào, chủ đề nàythì trò chuyện tới đây, dưới đây xem các hành vi khó hiểu khác của quy hoạch động.






## Hai, làm sao nhìn một cái ra bài toán con trùng lặp

Hay có bạn đọc nói:

Xem bài trước [mô-típ cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/), mìnhbiết từng bước tối ưu bài toán quy hoạch động;

Xem bài trước [Thiết kế quy hoạch động: Quy nạp toán học](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/), mìnhbiết dùng quy nạp toán học viết lời giải vét cạn (phương trình chuyển trạng thái).

**Nhưng dù mình viết được lời giải vét cạn, mình rất khó phán đoán cách giải này có tồn tại bài toán con trùng lặp không**, từ đó không xác định có thể vận dụng bảng ghi nhớ... tối ưu hiệu quả thuật toán không.

Với vấn đề này, thật ra trong các bài dòng quy hoạch động mình viết mấy lần, ở đây tổng kết thống nhất lại.

**Trước hết, cách đơn giản thô bạo nhất chính là vẽ hình, vẽ cây đệ quy ra, xem có node lặp không**.

Ví dụ đơn giản nhất, cây đệ quy của dãy Fibonacci trong [mô-típ cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/):

![](https://labuladong.online/algo/images/dynamic-programming/1.jpg)

Cây đệ quy này rất rõ ràng tồn tại node lặp, nên ta qua bảng ghi nhớ tránh tínhthừa.

Nhưng dù sao bài Fibonacci quá đơn giản, bài quy hoạch động thực tế phức tạp hơn, ví dụ DP hai chiều thậm chí ba chiều, đương nhiên cũng vẽ cây đệ quy được, nhưng không tránh k hỏi hơi phức tạp.

Ví dụ trong [Bài toán tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/), taviết lời giải vét cạn thế này:

```java
int dp(int[][] grid, int i, int j) {
    if (i == 0 && j == 0) {
        return grid[0][0];
    }
    if (i < 0 || j < 0) {
        return Integer.MAX_VALUE;
    }

    return Math.min(
            dp(grid, i - 1, j),
            dp(grid, i, j - 1)
        ) + grid[i][j];
}
```

Bạn không cần đọc bài trước, chỉ xem code hàm này là có thể xem ra, hàm này quá trình đệ quy tham số `i, j` không ngừng biến đổi, tức「trạng thái」là giá trị `(i, j)`, bạn có phán đoán được cách giải này có tồn tại bài toán con trùng lặp không?

Giả sử đầu vào `i = 8, j = 7`, cây đệ quy của trạng thái hai chiều như hình, hiển nhiên xuất hiện bài toán con trùng lặp:

![](https://labuladong.online/algo/images/optimal/2.jpeg)

**Nhưng nghĩ thêm một chút là có thể biết, thật ra căn bản không cần vẽ hình, qua khung đệ quy trực tiếp phán đoán có tồn tại bài toán con trùng lặp không**.

Thao tác cụ thể chính là xóa thẳng chi tiết code, trừu tượng hóa ra khung đệ quy của cách giải:




```java
int dp(int[][] grid, int i, int j) {
    dp(grid, i - 1, j), // #1
    dp(grid, i, j - 1) // #2
}
```



Thấy giá trị `i, j` không ngừng giảm, vậy mình hỏi bạn một câu: nếu mình muốn từ trạng thái `(i, j)` chuyển tới `(i-1, j-1)`, có mấy đường?

Hiển nhiên có hai đường, có thể `(i, j) -> #1 -> #2` hoặc `(i, j) -> #2 -> #1`, không chỉ một, cho thấy `(i-1, j-1)` sẽ bị tính nhiều lần, nên nhất định tồn tại bài toán con trùng lặp.

Lấy thêm ví dụ hơi phức tạp, code lời giải vét cạn của bài [Biểu thức chính quy](https://labuladong.online/algo/dynamic-programming/regular-expression-matching/) ởtrước:

```java
boolean dp(String s, int i, String p, int j) {
    int m = s.length(), n = p.length();
    // base case
    if (j == n) {
        return i == m;
    }
    if (i == m) {
        if ((n - j) % 2 == 1) {
            return false;
        }
        for (; j + 1 < n; j += 2) {
            if (p.charAt(j + 1) != '*') {
                return false;
            }
        }
        return true;
    }

    boolean res = false;

    if (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.') {
        if (j < n - 1 && p.charAt(j + 1) == '*') {
            res = dp(s, i, p, j + 2) || dp(s, i + 1, p, j);
        } else {
            res = dp(s, i + 1, p, j + 1);
        }
    } else {
        if (j < n - 1 && p.charAt(j + 1) == '*') {
            res = dp(s, i, p, j + 2);
        } else {
            res = false;
        }
    }

    return res;
}
```

Code hơi phức tạp đúng không, nếu vẽ hình hơi phiền, nhưng ta không vẽ, bỏ qua trực tiếp mọi chi tiết code và nhánh điều kiện, chỉ trừu tượng hóa khung đệ quy:

```java
boolean dp(String s, int i, String p, int j) {
    dp(s, i, p, j + 2); // #1
    dp(s, i + 1, p, j); // #2
    dp(s, i + 1, p, j + 1); // #3
}
```

Giống bài trước, 「trạng thái」của cách giải này cũng là giá trị `(i, j)`, vậy mình tiếp tục hỏi bạn: nếu mình muốn từ trạng thái `(i, j)` chuyển tới `(i+2, j+2)`, có mấy đường?

Hiển nhiên, ít nhất hai đường: `(i, j) -> #1 -> #2 -> #2` và `(i, j) -> #3 -> #3`, điều nàythì cho thấy cách giải này tồn tại khổng lồ lượng bài toán con trùng lặp.

Nên, không cần vẽ hìnhthì biết cách giải này cũng tồn tại bài toán con trùng lặp, cần dùng kỹ thuật bảng ghi nhớ tối ưu.

## Ba, kích thước mảng dp đặt thế nào

Ví như bài trước [Bài toán khoảng cách chỉnh sửa](https://labuladong.online/algo/dynamic-programming/edit-distance/), mìnhtrình bày trước là cách giải đệ quy top-down, hiện thực hàm `dp` thế này:

```java
class Solution {
    public int minDistance(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Theo định nghĩa hàm dp, tính khoảng cách chỉnh sửa nhỏ nhất của s1 và s2
        return dp(s1, m - 1, s2, n - 1);
    }

    // Định nghĩa: khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j] là dp(s1, i, s2, j)
    int dp(String s1, int i, String s2, int j) {
        // Xử lý base case
        if (i == -1) {
            return j + 1;
        }
        if (j == -1) {
            return i + 1;
        }

        // Chuyển trạng thái
        if (s1.charAt(i) == s2.charAt(j)) {
            return dp(s1, i - 1, s2, j - 1);
        } else {
            return min(
                dp(s1, i, s2, j - 1) + 1,
                dp(s1, i - 1, s2, j) + 1,
                dp(s1, i - 1, s2, j - 1) + 1
            );
        }
    }

    int min(int a, int b, int c) {
        return Math.min(a, Math.min(b, c));
    }
}
```

Rồi sửa thành cách giải lặp bottom-up:

```java
class Solution {
    public int minDistance(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Định nghĩa: khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j] là dp[i+1][j+1]
        int[][] dp = new int[m + 1][n + 1];
        // Khởi tạo base case
        for (int i = 1; i <= m; i++)
            dp[i][0] = i;
        for (int j = 1; j <= n; j++)
            dp[0][j] = j;

        // tìm lời giải bottom-up
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // Chuyển trạng thái
                if (s1.charAt(i-1) == s2.charAt(j-1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = min(
                        dp[i - 1][j] + 1,
                        dp[i][j - 1] + 1,
                        dp[i - 1][j - 1] + 1
                    );
                }
            }
        }
        // Theo định nghĩa mảng dp, lưu khoảng cách chỉnh sửa nhỏ nhất của s1 và s2
        return dp[m][n];
    }
}
```

Hai cách giải này ý tưởng hoàn toàn giống nhau, nhưngthì có bạn đọc hỏi, vì sao trong cách giải lặp kích thước khởi tạo mảng `dp` phải đặt là `int[m+1][n+1]`? Vì sao khoảng cách chỉnh sửa nhỏ nhất của `s1[0..i]` và `s2[0..j]` phải lưu trong `dp[i+1][j+1]`, có một vị trí lệch chỉ số?

Có thể bắt chước định nghĩa hàm `dp`, đem mảng `dp` khởi tạo là `int[m][n]`, rồi khiến khoảng cách chỉnh sửa nhỏ nhất của `s1[0..i]` và `s2[0..j]` lưu trong `dp[i][j]` không?

**Về lý thuyết, bạn định nghĩa thế nào cũng được, chỉ cần theo định nghĩa xử lý tốt base case là được**.

Bạn xem định nghĩa hàm `dp`, `dp(s1, i, s2, j)` tính khoảng cách chỉnh sửa của `s1[0..i]` và `s2[0..j]`, thì khi `i, j` bằng -1 đại diện base case chuỗi rỗng, nên đầu hàm xử lý hai trường hợp đặc thù này.

Lại xem mảng `dp`, bạn đương nhiên cũng định nghĩa `dp[i][j]` lưu khoảng cách chỉnh sửa của `s1[0..i]` và `s2[0..j]` được, nhưng vấn đề base case xử lý thế nào? Chỉ số làm sao là -1 được?

Nên ta đem mảng `dp` khởi tạo là `int[m+1][n+1]`, khiến chỉ số lệch chuyển tổng thể một vị, chừa chỉ số 0 làm base case cho biết chuỗi rỗng, rồi định nghĩa `dp[i+1][j+1]` lưu khoảng cách chỉnh sửa của `s1[0..i]` và `s2[0..j]`.

## Bốn, hướng duyệt mảng dp

Mình tin bạn đọc làm bài động quy, chắc chắn với thứ tự duyệt mảng `dp` có điểm đầu đau. Ta lấy mảng `dp` hai chiều ví dụ, có lúc ta duyệt xuôi:




```java
int[][] dp = new int[m][n];
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++)
        // Tính dp[i][j]
```



Có lúc ta duyệt ngược:




```java
for (int i = m - 1; i >= 0; i--)
    for (int j = n - 1; j >= 0; j--)
        // Tính dp[i][j]
```



Có lúc có thể duyệt chéo:




```java
// Duyệt chéo mảng
for (int l = 2; l <= n; l++) {
    for (int i = 0; i <= n - l; i++) {
        int j = l + i - 1;
        // Tính dp[i][j]
    }
}
```



Thậm chí khó hiểu hơn là, có lúc phát hiện duyệt xuôi ngược đều ra đáp án đúng, ví dụ trong [Bài toán quét sạch cổ phiếu](https://labuladong.online/algo/dynamic-programming/stock-problem-summary/) có chỗ thì xuôi ngược đều được.

Nếu quan sát kỹ sẽ phát hiện nguyên nhân, bạn chỉ cần nắm hai điểm là được:

**1、Trong quá trình duyệt, trạng thái cần thiếtphải là đã tính xong**.

**2、Sau khi duyệt xong, vị trí lưu kết quảphải đã tính xong**.

Dưới đây giải cụ thể ý nghĩa hai nguyên tắc trên.

Ví dụ bài kinh điển [Khoảng cách chỉnh sửa](https://labuladong.online/algo/dynamic-programming/edit-distance/), taqua định nghĩa mảng `dp`, xác định base case là `dp[..][0]` và `dp[0][..]`, đáp án cuối là `dp[m][n]`; mà ta qua phương trình chuyển trạng thái biết `dp[i][j]` cần từ `dp[i-1][j]`, `dp[i][j-1]`, `dp[i-1][j-1]` chuyển tới, như hình:

![](https://labuladong.online/algo/images/optimal/1.jpg)

Vậy, tham khảo hai nguyên tắc vừa nói, bạn duyệt mảng `dp` thế nào? Khẳng định duyệt xuôi:




```java
for (int i = 1; i < m; i++)
    for (int j = 1; j < n; j++)
        // Qua dp[i-1][j], dp[i][j - 1], dp[i-1][j-1]
        // Tính dp[i][j]
```



Vì, mỗi bước lặp vị trí trái-trên-trái trên đều là base case hoặc đã tính trước đó, mà cuối cùng kết thúc tại đáp án ta muốn `dp[m][n]`.

Lấy thêm ví dụ, bài toán dãy con đối xứng, xem bài trước [Template bài toán dãy con](https://labuladong.online/algo/dynamic-programming/subsequence-problem/), taqua định nghĩa mảng `dp`, xác định base case nằm trên đường chéo giữa, `dp[i][j]` cần từ `dp[i+1][j]`, `dp[i][j-1]`, `dp[i+1][j-1]` chuyển tới, đáp án cuối muốn tìm là `dp[0][n-1]`, như hình:

![](https://labuladong.online/algo/images/lps/4.jpg)

Trường hợp này theo hai nguyên tắc vừa rồi, thì có hai cách duyệt đúng:

![](https://labuladong.online/algo/images/lps/5.jpg)

Hoặc chéo từtrái-trên tớiphải-dưới, hoặc từ dưới lên trên từ trái sang phải, như vậy mới đảm bảo mỗi lần trái-dưới-trái dưới của `dp[i][j]` đã tính xong, ra kết quả đúng.

Giờ, bạn hẳn hiểu hai nguyên tắc này, chủ yếu chính là xem base case và vị trí lưu kết quả cuối, đảm bảo dữ liệu dùng trong quá trình duyệt đều tính xong là được, có lúc quả thật tồn tại nhiều cách ra đáp án đúng, có thể theo khẩu vị cá nhân tự chọn.






<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Một chiêu quét sạch bài mua bán cổ phiếu trên LeetCode](https://labuladong.online/algo/dynamic-programming/stock-problem-summary/)
 - [Template giải bài toán dãy con trong DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)
 - [Khung mô-típ giải bài quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)
 - [Quy hoạch động kinh điển: Bài toán đối kháng](https://labuladong.online/algo/dynamic-programming/game-theory/)
 - [Quy hoạch động kinh điển: Chọc bóng bay](https://labuladong.online/algo/dynamic-programming/burst-balloons/)
 - [Quy hoạch động kinh điển: Biểu thức chính quy](https://labuladong.online/algo/dynamic-programming/regular-expression-matching/)
 - [Quy hoạch động kinh điển: Khoảng cách chỉnh sửa](https://labuladong.online/algo/dynamic-programming/edit-distance/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/?show=1)| [115. Dãy con khác nhau](https://leetcode.cn/problems/distinct-subsequences/?show=1)| 🔴 |
| [139. Word Break](https://leetcode.com/problems/word-break/?show=1)| [139. Tách từ](https://leetcode.cn/problems/word-break/?show=1)| 🟠 |
| [221. Maximal Square](https://leetcode.com/problems/maximal-square/?show=1)| [221. Hình vuông lớn nhất](https://leetcode.cn/problems/maximal-square/?show=1)| 🟠 |
| [256. Paint House](https://leetcode.com/problems/paint-house/?show=1)🔒| [256. Sơn nhà](https://leetcode.cn/problems/paint-house/?show=1)🔒| 🟠 |
| [343. Integer Break](https://leetcode.com/problems/integer-break/?show=1)| [343. Tách số nguyên](https://leetcode.cn/problems/integer-break/?show=1)| 🟠 |
| [576. Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths/?show=1)| [576. Số đường ra ngoài biên](https://leetcode.cn/problems/out-of-boundary-paths/?show=1)| 🟠 |
| [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/?show=1)| [63. Đường khác nhau II](https://leetcode.cn/problems/unique-paths-ii/?show=1)| 🟠 |
| [91. De code Ways](https://leetcode.com/problems/decode-ways/?show=1)| [91. Cách giải mã](https://leetcode.cn/problems/decode-ways/?show=1)| 🟠 |
| - | [Kiếm chỉ Offer II 091. Sơn nhà](https://leetcode.cn/problems/JEj789/?show=1)| 🟠 |
| - | [Kiếm chỉ Offer II 097. Số lượng dãy con](https://leetcode.cn/problems/21dk04/?show=1)| 🔴 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

