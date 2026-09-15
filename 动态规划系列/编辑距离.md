# Quy hoạch động kinh điển: Khoảng cách chỉnh sửa (Edit Distance)



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [72. Edit Distance](https://leetcode.com/problems/edit-distance/)| [72. Khoảng cách chỉnh sửa](https://leetcode.cn/problems/edit-distance/)| 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Thuật toán dòng cây nhị phân (cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)


> tip: Bài này có bản video: [Khoảng cách chỉnh sửa giảng chi tiết quy hoạch động](https://www.bilibili.com/video/BV1uv411W73P/).Khuyếnnghị follow tài khoản Bilibili của mình, mình sẽ dẫn đọc bằng video các kỹ thuật thuật toán hơi khó.



Mấy hôm trước xem một đề phỏng vấn của hãng ngỗng (Tencent), phần thuật toán phần lớn là quy hoạch động, câu cuối cùng chính là viết một hàm tính khoảng cách chỉnh sửa, hôm nay chuyên viết một bài thảo luận bài toán này.

Bài 72「Khoảng cách chỉnh sửa」trên LeetCode chính là bài này, xem đề trước:




<Problem slug="edit-distance" />

```java
// Chữ ký hàm như sau
int minDistance(String s1, String s2)
```

Với bạn đọc chưa tiếp xúc bài toán quy hoạch động, bài này vẫn có độ khó nhất định, có phải cảm thấy hoàn toàn không biết bắt đầu từ đâu?

Nhưng bản thân vấn đề này vẫnkhá thực dụng, mình từng dùng thuật toán này trong đời sống. Trước đây có một bàitài khoản công chúng do sơ suất, viếtlệch một đoạn nội dung, mình quyết định sửa phần này cho logictrôi chảy. Nhưng bàichỉ sửa tối đa 20 chữ, mà chỉ hỗ trợ thao tác thêm, xóa, thay thế (y hệt bài toán khoảng cách chỉnh sửa), thế là mình dùng thuật toán tìm ra một phương án tối ưu, chỉ dùng 16 bướcthì hoàn thành sửa.

Lấy ví dụ caoxịn hơn, chuỗi DNAgồm A, G, C, T, có thểví như chuỗi. Khoảng cách chỉnh sửa đo được độtương đồng của hai chuỗi DNA, khoảng cách càng nhỏ, cho thấy hai đoạn DNA càng giống, biết đâu chủ nhân hai DNA này là họ hàngxa xưa gì đó.

Dưới đâyquay lại chính đề, giảng giải chi tiết khoảng cách chỉnh sửa tính thế nào, tin là bài này sẽ cho bạn thu hoạch.






## Một, ý tưởng

Bài toán khoảng cách chỉnh sửa chính là cho ta hai chuỗi `s1` và `s2`, chỉ dùng ba thao tác, bắt ta biến `s1` thành `s2`, tìm số thao tác ít nhất. Cần rõ ràng là, dù biến `s1` thành `s2` hay ngược lại, kết quả đều như nhau, nên phần saulấy `s1` biến thành `s2` ví dụ.

> [!TIP]
> Giải bài toán quy hoạch động hai chuỗi, thường đều dùng hai con trỏ `i, j` lần lượt trỏ đầu hoặc cuối hai chuỗi, rồi thử viết phương trình chuyển trạng thái.
>
> Ví như cho `i, j` lần lượt trỏ cuối hai chuỗi, định nghĩa `dp[i], dp[j]` là khoảng cách chỉnh sửa của chuỗi con `s1[0..i], s2[0..j]`, thì quá trình `i, j` từng bước tiến lên trước, chính là quá trình quy mô bài toán (độ dài chuỗi con) giảm dần.
>
> Đương nhiên, bạn muốn cho `i, j` lần lượt trỏ đầu chuỗi, rồi từng bước tiến về sau cũng được, bản chất không khác, chỉ cần sửa định nghĩa hàm/mảng `dp` là được.

Đặt hai chuỗi lần lượt là `"rad"` và `"apple"`, cho hai con trỏ `i, j` lần lượt trỏ cuối hai chuỗi `s1, s2`, để biến `s1` thành `s2`, thuật toán sẽ tiến hành thế này:

![](https://labuladong.online/algo/images/editDistance/edit.gif)

![](https://labuladong.online/algo/images/editDistance/1.jpg)

Hãy nhớ quá trình GIF này, như vậy là có thể tính được khoảng cách chỉnh sửa. Mấu chốt làm sao đưa ra thao tác đúng, lát nữa sẽ trình bày.

Theo GIF trên, có thể phát hiện thao tác không chỉ có ba, thật ra còn thao tác thứ tư, đừng làm gì (skip). Ví dụ trường hợp này:

![](https://labuladong.online/algo/images/editDistance/2.jpg)

Vì hai ký tự này vốn đã giống nhau, để khoảng cách chỉnh sửa nhỏ nhất, hiển nhiên không nên có thao tác nào với chúng, di chuyển thẳng `i, j` lên trước là được.

Còn một trường hợp rất dễ xử lý, chính là khi `j` đi hết `s2`, nếu `i` còn chưa đi hết `s1`, thì chỉ dùng thao tác xóa để rút `s1` thành `s2`. Ví dụ trường hợp này:

![](https://labuladong.online/algo/images/editDistance/3.jpg)

Tương tự, nếu `i` đi hết `s1` mà `j` còn chưa đi hết `s2`, thì chỉ dùng thao tác chèn để chèn toàn bộ ký tự còn lại của `s2` vào `s1`. Lát nữa sẽ thấy, hai trường hợp này chính là **base case** của thuật toán.

Dưới đây giải chi tiết cách chuyển ý tưởng thành code.






## Hai, giải chi tiết code

lược lại ý tưởng trước đó:

base case là `i` đi hết `s1` hoặc `j` đi hết `s2`, trả về trực tiếp độ dài còn lại của chuỗi kia.

Với mỗi cặp ký tự `s1[i]` và `s2[j]`, có thể có bốn thao tác:

```python
if s1[i] == s2[j]:
    đừng /đừng làm gì (skip)
    i, j đồng thời tiến lên trước
else:
    Ba chọn một:
        chèn (insert)
        xóa (delete)
        thay thế (replace)
```

Có khung này, bài toánthì đã giải xong. Bạn đọc có thể hỏi, 「ba chọn một」này rốt cuộc chọn thế nào? Rất đơn giản, thử hết một lượt, thao tác nào cuối cùng được khoảng cách chỉnh sửa nhỏ nhất, thì chọn nó. Ở đây cần kỹ thuật đệ quy, xem code cách giải vét cạn trước:

```java
class Solution {
    public int minDistance(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // i, j khởi tạo trỏ chỉ số cuối cùng
        return dp(s1, m - 1, s2, n - 1);
    }

    // Định nghĩa: trả về khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j]
    int dp(String s1, int i, String s2, int j) {
        // base case
        if (i == -1) return j + 1;
        if (j == -1) return i + 1;

        if (s1.charAt(i) == s2.charAt(j)) {
            // Không làm gì
            return dp(s1, i - 1, s2, j - 1);
        }
        return min(
            // Chèn
            dp(s1, i, s2, j - 1) + 1,
            // Xóa
            dp(s1, i - 1, s2, j) + 1,
            // Thay thế
            dp(s1, i - 1, s2, j - 1) + 1
        );
    }

    int min(int a, int b, int c) {
        return Math.min(a, Math.min(b, c));
    }
}
```

Dưới đây giải thích chi tiết đoạn code đệ quy này, base case hẳn không cần giải thích, chủ yếu giải thích phần đệ quy.

Đều nói tínhgiải thích của code đệ quy rất tốt, điều này có lý, chỉ cần hiểu định nghĩa hàm, là có thể hiểu rõ logic thuật toán. Định nghĩa hàm `dp` ở đây là:

```java
// Định nghĩa: trả về khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j]
int dp(String s1, int i, String s2, int j)
```

**Nhớ định nghĩa này** rồi, xem đoạn code này trước:

```python
if s1[i] == s2[j]:
    # Không làm gì
    return dp(s1, i - 1, s2, j - 1)
# Giải thích:
# Vốn đã bằng nhau, không cần thao tác nào
# Khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j] bằng
# khoảng cách chỉnh sửa nhỏ nhất của s1[0..i-1] và s2[0..j-1]
# tức dp(i, j) bằng dp(i-1, j-1)
```

Nếu `s1[i] != s2[j]`, thì cần với ba thao tác đệ quy, hơi cần suy nghĩ:

```python
# Chèn
dp(s1, i, s2, j - 1) + 1,
# Giải thích:
# Tôi chèn thẳng vào s1[i] một ký tự giống s2[j]
# vậy s2[j]thì được khớp, tiến j lên trước, tiếp tục so sánh với i
# Đừng quên số thao tác cộng một
```

![](https://labuladong.online/algo/images/editDistance/insert.gif)

```python
# Xóa
dp(s1, i - 1, s2, j) + 1,
# Giải thích:
# Tôi xóa thẳng ký tự s[i] này
# tiến i lên trước, tiếp tục so sánh với j
# số thao tác cộng một
```

![](https://labuladong.online/algo/images/editDistance/delete.gif)

```python
# Thay thế
dp(s1, i - 1, s2, j - 1) + 1
# Giải thích:
# Tôi thay thẳng s1[i] thành s2[j], như vậy chúng khớp nhau
# đồng thời tiến i, j tiếp tục so sánh
# số thao tác cộng một
```

![](https://labuladong.online/algo/images/editDistance/replace.gif)



Giờ, bạn hẳn hiểu hoàn toàn đoạn codengắn gọn này. Còn vấn đề nhỏ chính là, cách giải này là cách giải vét cạn, tồn tại bài toán con trùng lặp, cần dùng kỹ thuật quy hoạch động tối ưu.

**Làm sao nhìn một cái ra tồn tại bài toán con trùng lặp**? Trong [Giải đáp quy hoạch động](https://labuladong.online/algo/dynamic-programming/faq-summary/) mìnhcó trình bày, ở đây nhắc đơn giản, cần trừu tượng hóa khung đệ quy của thuật toán trong bài:

```java
int dp(i, j) {
    dp(i - 1, j - 1); // #1
    dp(i, j - 1); // #2
    dp(i - 1, j); // #3
}
```

Với bài toán con `dp(i-1, j-1)`, qua bài gốc `dp(i, j)` thu được thế nào? Có không chỉ một đường, ví dụ `dp(i, j) -> #1` và `dp(i, j) -> #2 -> #3`. Một khi phát hiện một đường lặp, thì cho thấy tồn tạivô số đường lặp, chính là bài toán con trùng lặp.

## Ba, tối ưu quy hoạch động

Với bài toán con trùng lặp, bài trước [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) giớithiệu chi tiết, phương pháp tối ưu chẳng qua là thêm bảng ghi nhớ cho cách giải đệ quy, hoặc quá trình DP dùng bảng DP lặp hiện thực, dưới đây từng cái trình bày.

### cách giải bảng ghi nhớ

đã biết cách giải đệ quy vét cạn đều viết ra, bảng ghi nhớ rất dễ thêm, code gốc sửa nhẹ là được:

```java
class Solution {
    // Bảng ghi nhớ
    int[][] memo;

    public int minDistance(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Bảng ghi nhớ khởi tạo giá trị đặc biệt, đại diện chưa tính
        memo = new int[m][n];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return dp(s1, m - 1, s2, n - 1);
    }

    int dp(String s1, int i, String s2, int j) {
        if (i == -1) return j + 1;
        if (j == -1) return i + 1;
        // Tra bảng ghi nhớ, tránh bài toán con trùng lặp
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        // Chuyển trạng thái, kết quả lưu vào bảng ghi nhớ
        if (s1.charAt(i) == s2.charAt(j)) {
            memo[i][j] = dp(s1, i - 1, s2, j - 1);
        } else {
            memo[i][j] = min(
                dp(s1, i, s2, j - 1) + 1,
                dp(s1, i - 1, s2, j) + 1,
                dp(s1, i - 1, s2, j - 1) + 1
            );
        }
        return memo[i][j];
    }

    int min(int a, int b, int c) {
        return Math.min(a, Math.min(b, c));
    }
}
```

### cách giải bảng DP

Chủ yếu nói cách giải bảng DP, ta cần định nghĩa một mảng `dp`, rồi trên mảng này thực hiện phương trình chuyển trạng thái.

Trước hết rõ ràng ý nghĩa mảng `dp`, vì bài này có hai trạng thái (chỉ số `i` và `j`), nên mảng `dp` là mảng hai chiều, đại khái dài thế này:

![](https://labuladong.online/algo/images/editDistance/dp.jpg)

Chuyển trạng thái giống cách giải đệ quy, `dp[..][0]` và `dp[0][..]` tương ứng base case, ý nghĩa `dp[i][j]` tương tự định nghĩa hàm `dp` trước đó:




```java
int dp(String s1, int i, String s2, int j)
// Trả về khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j]

dp[i-1][j-1]
// Lưu khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j]
```



base case của hàm `dp` là `i, j` bằng -1, mà chỉ số mảng ít nhất là 0, nên mảng `dp` sẽ lệch một vị.

đã biết ý nghĩa mảng `dp` và hàm `dp` đệ quy như nhau, cũngthì có thể lấy áp thẳng ý tưởng trước đó viết code, **khác duy nhất là, cách giải đệ quy là tìm lời giải top-down (bắt đầu từ bài gốc, từng bước phân rã tới base case), bảng DP là tìm lời giải bottom-up (bắt đầu từ base case, suy diễn về bài gốc)**:

```java
class Solution {
    public int minDistance(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        // Định nghĩa: khoảng cách chỉnh sửa nhỏ nhất của s1[0..i] và s2[0..j] là dp[i+1][j+1]
        int[][] dp = new int[m + 1][n + 1];
        // base case
        for (int i = 1; i <= m; i++)
            dp[i][0] = i;
        for (int j = 1; j <= n; j++)
            dp[0][j] = j;
        // tìm lời giải bottom-up
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
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
        // Lưu khoảng cách chỉnh sửa nhỏ nhất của toàn bộ s1 và s2
        return dp[m][n];
    }

    int min(int a, int b, int c) {
        return Math.min(a, Math.min(b, c));
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/edit-distance/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🎃 Animation trực quan hóa code 🎃</s trong>
</summary>
</details>
</a>
<hr/>



## Bốn, mở rộng

Nói chung, xử lý bài toán quy hoạch động hai chuỗi, đều xử lý theo ý tưởng trong bài, lập bảng DP. Vì sao? vì dễ tìm quan hệ chuyển trạng thái, ví dụ bảng DP của khoảng cách chỉnh sửa:

![](https://labuladong.online/algo/images/editDistance/4.jpg)

Còn một chi tiết, đã biết mỗi `dp[i][j]` chỉ liên quan ba trạng thái gần nó, độ phức tạp không gian có thể nén thành $O(min(M, N))$ (M, N là độ dài hai chuỗi). Không khó, nhưng tính có thể hiểu giải thích giảm mạnh, bạn đọc tự thử tối ưu.

Bạn còn có thể hỏi, **ở đây chỉ tìm ra khoảng cách chỉnh sửa nhỏ nhất, vậy thao tác cụ thể là gì**? Ví dụ sửa bài trang công chúng vừa nêu, chỉ có khoảng cách chỉnh sửa nhỏ nhất chắc chắn không đủ, còn phải biết sửa cụ thể thế nào.

Thật ra rất đơn giản, code sửa nhẹ, cho mảng dp thêm thông tin là được:

```java
// int[][] dp;
Node[][] dp;

class Node {
    int val;
    int choice;
    // 0 đại diện không làm gì
    // 1 đại diện chèn
    // 2 đại diện xóa
    // 3 đại diện thay thế
}
```

Thuộc tính `val` chính là giá trị số của mảng dp trước đó, thuộc tính `choice` đại diện thao tác. Khi lựa chọn tối ưu, tiện tay ghi thao tác lại, rồithì từ kết quả suy ngược thao tác cụ thể.

Kết quả cuối của ta chẳng phải `dp[m][n]` sao, `val` ở đây lưu khoảng cách chỉnh sửa nhỏ nhất, `choice` lưu thao tác cuối cùng, ví dụ là thao tác chèn, thìdời trái một ô:

![](https://labuladong.online/algo/images/editDistance/5.jpg)

Lặp quá trình này, có thể từng bước quay về điểm bắt đầu `dp[0][0]`, tạo thành một đường, theo thao tác trên đường này chỉnh sửa, chính là phương án tốt nhất.

![](https://labuladong.online/algo/images/editDistance/6.jpg)

Theo yêu cầu mọi người, mình viết ý tưởng này ra, bạn tự chạy thử:

```java
int minDistance(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    Node[][] dp = new Node[m + 1][n + 1];
    // base case
    for (int i = 0; i <= m; i++) {
        // s1 chuyển thành s2 chỉ cần xóa một ký tự
        dp[i][0] = new Node(i, 2);
    }
    for (int j = 1; j <= n; j++) {
        // s1 chuyển thành s2 chỉ cần chèn một ký tự
        dp[0][j] = new Node(j, 1);
    }
    // Phương trình chuyển trạng thái
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i-1) == s2.charAt(j-1)){
                // Nếu hai ký tự giống nhau, thì không cần làm gì
                Node node = dp[i - 1][j - 1];
                dp[i][j] = new Node(node.val, 0);
            } else {
                // Ngược lại, ghi thao tácgiá nhỏ nhất
                dp[i][j] = minNode(
                    dp[i - 1][j],
                    dp[i][j - 1],
                    dp[i-1][j-1]
                );
                // Và cộng khoảng cách chỉnh sửa lên một
                dp[i][j].val++;
            }
        }
    }
    // Theo bảng dp suy ngược quá trình thao tác cụ thể và in
    printResult(dp, s1, s2);
    return dp[m][n].val;
}

// Tính thao tác giá phải trả nhỏ nhất trong delete, insert, replace
Node minNode(Node a, Node b, Node c) {
    Node res = new Node(a.val, 2);

    if (res.val > b.val) {
        res.val = b.val;
        res.choice = 1;
    }
    if (res.val > c.val) {
        res.val = c.val;
        res.choice = 3;
    }
    return res;
}
```

Cuối cùng, hàm `printResult` suy ngược kết quả và in thao tác cụ thể:

```java
void printResult(Node[][] dp, String s1, String s2) {
    int rows = dp.length;
    int cols = dp[0].length;
    int i = rows - 1, j = cols - 1;
    System.out.println("Change s1=" + s1 + " to s2=" + s2 + ":\n");
    while (i != 0 && j != 0) {
        char c1 = s1.charAt(i - 1);
        char c2 = s2.charAt(j - 1);
        int choice = dp[i][j].choice;
        System.out.print("s1[" + (i - 1) + "]:");
        switch (choice) {
            case 0:
                // Bỏ qua, thì hai con trỏ đồng thời tiến
                System.out.println("skip '" + c1 + "'");
                i--; j--;
                break;
            case 1:
                // Chèn s2[j] vào s1[i], thì con trỏ s2 tiến
                System.out.println("insert '" + c2 + "'");
                j--;
                break;
            case 2:
                // Xóa s1[i], thì con trỏ s1 tiến
                System.out.println("delete '" + c1 + "'");
                i--;
                break;
            case 3:
                // Thay s1[i] thành s2[j], thì hai con trỏ đồng thời tiến
                System.out.println(
                    "replace '" + c1 + "'" + " with '" + c2 + "'");
                i--; j--;
                break;
        }
    }
    // Nếu s1 còn chưa đi hết, thì phần còn lại đều cần xóa
    while (i > 0) {
        System.out.print("s1[" + (i - 1) + "]:");
        System.out.println("delete '" + s1.charAt(i - 1) + "'");
        i--;
    }
    // Nếu s2 còn chưa đi hết, thì phần còn lại đều cần chèn vào s1
    while (j > 0) {
        System.out.print("s1[0]:");
        System.out.println("insert '" + s2.charAt(j - 1) + "'");
        j--;
    }
}
```



<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [Template giải bài toán dãy con trong DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)
 - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)
 - [Quy hoạch động kinh điển: Biểu thức chính quy](https://labuladong.online/algo/dynamic-programming/regular-expression-matching/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [97. Interleaving String](https://leetcode.com/problems/interleaving-string/?show=1)| [97. Chuỗi đan xen](https://leetcode.cn/problems/interleaving-string/?show=1)| 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

