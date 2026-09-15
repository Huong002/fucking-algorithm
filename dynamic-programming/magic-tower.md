# Quy hoạch động giúp mìnhphá đảo《Tháp ma thuật》



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/)| [174. Trò chơi hầm ngục](https://leetcode.cn/problems/dungeon-game/)| 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

「Tháp ma thuật」là một game hầm ngục kinh điển, gặp quái phải mất máu, ăn bình máu thêm máu, bạn phải thu thập chìa khóa, lên từng tầng, cuối cùng cứu công chúa xinh đẹp.

Giờ trên điện thoại vẫn chơi được game này:

![](https://labuladong.online/algo/images/dungeons/0.png)

Ừm, tin là game này bao thầu không ít ký ức tuổi thơ, nhớ hồi nhỏ, một mình cầm máy chơi, hai ba ngườixúmtrái phải chỉ tay, khiến người chơicực tệ, mà ngườicực vui 😂

Bài 174「Trò chơi hầm ngục」trên LeetCode là một bài tương tự:

<Problem slug="dungeon-game" />

**Nói đơn giản, chính là hỏi bạn ít nhất cần bao nhiêu máu khởi đầu, để hiệp sĩ đi từ góc trái trên tới góc phải dưới, mà bất cứ lúc nào máu đều phải lớn hơn 0**.

Chữ ký hàm như sau:

```java
int calculateMinimumHP(int[][] grid);
```

Bài trước [Tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/) viết bài tương tự, hỏi bạn từ góc trái trên tới góc phải dưới tổng đường đi nhỏ nhất là bao nhiêu.

Ta làm bài thuật toán nhất định phảisuy một ra ba, cảm thấy bài hôm nay và tổng đường đi nhỏ nhất có điểm quan hệ đúng không?

Muốn tối t hiểu hóa máu khởi đầu của hiệp sĩ, có phải có nghĩa là phải tối đa hóa bình máu trênhành trình của hiệp sĩ? Có phải tương đương tìm 「tổng đường đi lớn nhất」? Có phải áp thẳng ý tưởng tính「tổng đường đi nhỏ nhất」?

Nhưng nghĩ thêm một chút, phát hiệnsuy luận này không đúng. Ăn nhiều bình máu nhất, không nhất địnhthì được máu khởi đầu nhỏ nhất.

Ví dụ trường hợp dưới, nếu muốn ăn nhiều bình máu nhất được「tổng đường đi lớn nhất」, nên đi theo mũi tên hình dưới, máu khởi đầu cần 11:

![](https://labuladong.online/algo/images/dungeons/2.png)

Nhưng cũng dễ thấy, đáp án đúng hẳn là hành trình hình dưới mũi tên chỉ, máu khởi đầu chỉ cần 1:

![](https://labuladong.online/algo/images/dungeons/3.png)

**Nên, mấu chốt không tại ở ăn nhiều bình máu nhất, mà tại ở làm sao mất ít máu nhất**.

Dạng tìm giá trị tối ưu này, chắc chắn phải mượn kỹ thuật quy hoạch động, phải thiết kế hợp lý định nghĩa mảng/hàm `dp`. so sánh bài trước [Bài toán tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/), chữký hàm `dp` chắc chắn dài thế này:

```java
int dp(int[][] grid, int i, int j);
```

Nhưng định nghĩa hàm `dp` của bài này khá có ý, theo lẽ thường, định nghĩa hàm `dp` này hẳn là:

**Từ góc trái trên (`grid[0][0]`) đi tới `grid[i][j]` ít nhất cần `dp(grid, i, j)` máu**.

Định nghĩa vậy, base case chính là khi `i, j` đều bằng 0, ta viết code thế này:

```java
int calculateMinimumHP(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    // Ta muốn tính máu nhỏ nhất từ góc trái trên tới góc phải dưới
    return dp(grid, m - 1, n - 1);
}

int dp(int[][] grid, int i, int j) {
    // base case
    if (i == 0 && j == 0) {
        // Đảm bảo hiệp sĩđáp đất không chết là được
        return grid[i][j] > 0 ? 1 : -grid[i][j] + 1;
    }
    ...
}
```

> [!NOTE]
> Để gọn, sau này `dp(grid, i, j)` viết gọn là `dp(i, j)`, mọi người hiểu là được.

Tiếp theo ta cần tìm chuyển trạng thái, còn nhớ tìm phương trình chuyển trạng thái thế nào không? Định nghĩa hàm `dp` vậy có đúng đắn chuyển trạng thái được không?

Ta hy vọng `dp(i, j)` có thể qua `dp(i-1, j)` và `dp(i, j-1)` suy ra ra, như vậy là có thể dần tiến tới gần base case, cũng là có thể đúng đắn chuyển trạng thái.

Cụ thể, 「máu nhỏ nhất tới `A`」hẳn có thểdo「máu nhỏ nhất tới `B`」và「máu nhỏ nhất tới `C`」 suy ra ra:

![](https://labuladong.online/algo/images/dungeons/4.png)

**Nhưng vấn đề là, suy ra được sao? thực tế là không**.

Vì theo định nghĩa hàm `dp`, bạn chỉ biết「máu nhỏ nhất tới được `B` từ góc trái trên」, nhưng không biết「máu khi tới `B`」.

「Máu khi tới `B`」làtham khảo chắc chắn cần để chuyển trạng thái, mình lấy ví dụ bạnthì hiểu, giả sử trường hợp hình dưới:

![](https://labuladong.online/algo/images/dungeons/5.png)

Bạn nói trường hợp này, hành trình tối ưu hiệp sĩ cứu công chúa là gì?

Hiển nhiên đi theođường xanh tới `B`, cuối cùng tới `A` đúng không, vậy máu khởi đầu chỉ cần 1; nếu đi hành trình mũi tên vàng, tới `C` trước rồi tới `A`, máu khởi đầu ít nhất cần 6.

Vì sao vậy? Hiệp sĩ đi tới `B` và `C` máu khởi đầu ít nhất đều là 1, vì sao cuối cùng đi từ `B` tới `A`, chứ không từ `C` tới `A`?

Vì hiệp sĩ đi tới `B` thì máu là 11, mà đi tới `C` thì máu vẫn là 1.

Nếu hiệp sĩcố cần qua `C` tới `A`, thì máu khởi đầuphải thêm tới 6 mới được; mà nếu qua `B` tới `A`, máu khởi đầu là 1 là đủ, vì dọc đường ăn bình máu, máu đủchịusát thương của quái trên `A`.

Đây hẳn nói rất rõ, ôn lại định nghĩa hàm `dp`, trường hợp hình trên, thuật toán chỉ biết `dp(1, 2) = dp(2, 1) = 1`, đều như nhau, làm sao đưa ra quyết định đúng, tính `dp(2, 2)`?

**Nên nói, định nghĩa mảng `dp` trước đó của ta sai, thông tin không đủ, thuật toán không đưa ra chuyển trạng thái đúng được**.

Cách đúng cần suy nghĩ ngược, vẫn là hàm `dp` dưới:

```java
int dp(int[][] grid, int i, int j);
```

Nhưng ta cần sửa định nghĩa hàm `dp`:

**Từ `grid[i][j]` tớiđích (góc phải dưới) máu ít nhất cần là `dp(grid, i, j)`**.

Vậy viết code thế này:

```java
int calculateMinimumHP(int[][] grid) {
    // Ta muốn tính máu nhỏ nhất từ góc trái trên tới góc phải dưới
    return dp(grid, 0, 0);
}

int dp(int[][] grid, int i, int j) {
    int m = grid.length;
    int n = grid[0].length;
    // base case
    if (i == m - 1 && j == n - 1) {
        return grid[i][j] >= 0 ? 1 : -grid[i][j] + 1;
    }
    ...
}
```

Theo định nghĩa hàm `dp` mới và base case, ta muốn tìm `dp(0, 0)`, đó thì nên thử đồ thị qua `dp(i, j+1)` và `dp(i+1, j)` suy ra ra `dp(i, j)`, như vậy mới dần tiến tới gần base case, chuyển trạng thái đúng.

Cụ thể, 「máu ít nhất từ `A` tới góc phải dưới」hẳndo「máu ít nhất từ `B` tới góc phải dưới」và「máu ít nhất từ `C` tới góc phải dưới」 suy ra ra:

![](https://labuladong.online/algo/images/dungeons/6.png)

suy ra ra được sao? Lần này được, giả sử `dp(0, 1) = 5, dp(1, 0) = 4`, thì chắc chắn cần từ `A` hướng đi `C`, vì 4 nhỏ hơn 5 mà.

Vậy suy ra `dp(0, 0)` là bao nhiêu thế nào?

Giả sử giá trị `A` là 1, đã biết bước tiếp tới `C`, mà `dp(1, 0) = 4` có nghĩa là đi tới `grid[1][0]` thì ít nhất cần có 4 máu, vậythì xác định hiệp sĩ xuất hiện tại điểm `A` cần 4 - 1 = 3 máu khởi đầu, đúng không.

Vậy nếu giá trị `A` là 10, đáp đất nhặt được bình máu lớn, vượt nhu cầu sau tiếp theo, 4 - 10 = -6 có nghĩa là máu khởi đầu của hiệp sĩ là số âm, điều này hiển nhiên không được, máu hiệp sĩ nhỏ hơn 1toi, nên trường hợp này máu khởi đầu của hiệp sĩ hẳn là 1.

Tổng hợp, phương trình chuyển trạng thái đã suy ra ra:




```java
int res = min(
    dp(i + 1, j),
    dp(i, j + 1)
) - grid[i][j];

dp(i, j) = res <= 0 ? 1 : res;
```

Theo logic cốt lõi này, thêm bảng ghi nhớ loại bỏ bài toán con trùng lặp, thì viết thẳng được code cuối:

```java
class Solution {
    // Hàm chính
    public int calculateMinimumHP(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        // Bảng ghi nhớ khởi tạo toàn -1
        memo = new int[m][n];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }

        return dp(grid, 0, 0);
    }

    // Bảng ghi nhớ, loại bỏ bài toán con trùng lặp
    int[][] memo;

    // Định nghĩa: từ (i, j) tới góc phải dưới, máu khởi đầu ít nhất cần là bao nhiêu
    int dp(int[][] grid, int i, int j) {
        int m = grid.length;
        int n = grid[0].length;
        // base case
        if (i == m - 1 && j == n - 1) {
            return grid[i][j] >= 0 ? 1 : -grid[i][j] + 1;
        }
        if (i == m || j == n) {
            return Integer.MAX_VALUE;
        }
        // Tránh tính lặp
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        // Logic chuyển trạng thái
        int res = Math.min(
                dp(grid, i, j + 1),
                dp(grid, i + 1, j)
            ) - grid[i][j];
        // Máu hiệp sĩ ít nhất là 1
        memo[i][j] = res <= 0 ? 1 : res;

        return memo[i][j];
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/dungeon-game/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>👾 Animation trực quan hóa code 👾</s trong>
</summary>
</details>
</a>
<hr/>



Đây chính là cách giải quy hoạch động top-down kèm bảng ghi nhớ, tham khảo bài trước [Giải thích mô-típ quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) rấtdễthì sửa thành cách giải lặp mảng `dp`, ở đây không viết, bạn đọc thử tự viết.

Cốt lõi của bài này là định nghĩa hàm `dp`, tìm phương trình chuyển trạng thái đúng, từ đó tính đáp án đúng.








**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

