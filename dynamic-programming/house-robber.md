# Một chiêu quét sạch bài trộm nhà trên LeetCode



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [198. House Robber](https://leetcode.com/problems/house-robber/)| [198. Trộm nhà](https://leetcode.cn/problems/house-robber/)| 🟠 |
| [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)| [213. Trộm nhà II](https://leetcode.cn/problems/house-robber-ii/)| 🟠 |
| [337. House Robber III](https://leetcode.com/problems/house-robber-iii/)| [337. Trộm nhà III](https://leetcode.cn/problems/house-robber-iii/)| 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Thuật toán dòng cây nhị phân (cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Hôm nay trình bày dòng「Trộm nhà」(bản tiếng Anh gọi House Robber), dòng này là bài quy hoạch động khá có tính đại diện và kỹ thuật.

Dòng trộm nhà tổng cộng có ba bài, độ khó thiết kế khá hợp lý, tăng dần từng lớp. Bài một là bài quy hoạch động khá chuẩn, bài haiđưa thêm điều kiện mảng vòng, bài babá hơn, kết hợp cách giải bottom-up và top-down của quy hoạch động với cây nhị phân, mình thấy rất gợi mở.

Dưới đây, phân tích từ bài một.

## Trộm nhà I

Bài 198「Trộm nhà」trên LeetCode đề như sau:

Trên phố có một hàng nhà, dùng mảng chứa số nguyên không âm `nums` cho biết, mỗi phần tử `nums[i]` đại diện số tiền mặt trong nhà thứ `i`. Giờ bạn là trộm chuyên nghiệp, bạn hy vọng trộm **nhiều nhất có thể** tiền mặt trong các nhà này, nhưng, **nhà kề nhau không được đồng thời trộm**, nếu không sẽchạm báo động, bạntoi.

Hãy viết thuật toán, tính với tiền đề không chạm báo động, trộm được nhiều nhất bao nhiêu tiền? Chữ ký hàm như sau:

```java
int rob(int[] nums);
```

Ví dụ nhập `nums=[2,1,7,9,3,1]`, thuật toán trả về 12, trộm có thể trộm ba nhà `nums[0], nums[3], nums[5]`, tổng tiền được là 2 + 9 + 1 = 12, là lựa chọn tối ưu.

Đề rất dễ hiểu, màbiểu hiện quy hoạch động rất rõ ràng. Bài trước [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) tatổng kết, **giải bài toán quy hoạch động chính là tìm「trạng thái」và「lựa chọn」, chỉ vậy mà thôi**.






Giả tưởng bạn chính là tên trộm chuyên nghiệp này, đi từ trái sang phải qua hàng nhà này, trước mỗi nhà đều có hai **lựa chọn**: cướp hoặc không cướp.

Nếu bạn cướp nhà này, thì bạn **chắc chắn** không được cướp nhà kề tiếp theo, chỉ bắt đầu lựa chọn từ nhà kế tiếp nữa.

Nếu bạn không cướp nhà này, thì bạn đi tới trước nhà tiếp theo, tiếp tục lựa chọn.

Khi bạn đi qua nhà cuối cùng, bạnhết cái cướp, tiền cướp được hiển nhiên là 0 (**base case**).

Logic trên rất đơn giản nhé, thật ra đã rõ ràng 「trạng thái」và「lựa chọn」: **chỉ số nhà trước mặt bạn chính là trạng thái, cướp và không cướp chính là lựa chọn**.

![](https://labuladong.online/algo/images/robber/1.jpg)

Trong hai lựa chọn, mỗi lầnchọn kết quả lớn hơn, cuối cùng thu được chính là money trộm được nhiều nhất:

```java
class Solution {
    // Hàm chính
    public int rob(int[] nums) {
        return dp(nums, 0);
    }

    // Định nghĩa: trả về giá trị lớn nhất nums[start..] cướp được
    private int dp(int[] nums, int start) {
        if (start >= nums.length) {
            return 0;
        }

        int res = Math.max(
                // Không cướp, đi nhà tiếp
                dp(nums, start + 1),
                // Cướp, đi nhà kế tiếp nữa
                nums[start] + dp(nums, start + 2)
            );
        return res;
    }
}
```

rõ ràng chuyển trạng thái, thì phát hiện với cùng vị trí `start`, tồn tại bài toán con trùng lặp, như hình dưới:

![](https://labuladong.online/algo/images/robber/2.jpg)

Trộm có nhiều lựa chọn đi tới vị trí này, nếu mỗi lần tới đâyđi vào đệ quy, chẳng phải lãng phí thời gian? Nên nói tồn tại bài toán con trùng lặp, dùng bảng ghi nhớ tối ưu:

```java
class Solution {

    private int[] memo;
    // Hàm chính
    public int rob(int[] nums) {
        // Khởi tạo bảng ghi nhớ
        memo = new int[nums.length];
        Arrays.fill(memo, -1);
        // Trộm bắt đầu cướp từ nhà thứ 0
        return dp(nums, 0);
    }

    // Định nghĩa: trả về giá trị lớn nhất dp[start..] cướp được
    private int dp(int[] nums, int start) {
        if (start >= nums.length) {
            return 0;
        }
        // Tránh tính lặp
        if (memo[start] != -1) return memo[start];

        int res = Math.max(
            dp(nums, start + 1),
            dp(nums, start + 2) + nums[start]
        );
        // Ghi vào bảng ghi nhớ
        memo[start] = res;
        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/house-robber/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🎃 Animation trực quan hóa code 🎃</s trong>
</summary>
</details>
</a>
<hr/>



Đây chính là cách giải quy hoạch động top-down, ta cũng hơi sửa, viết ra cách giải **bottom-up**:

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        // dp[i] = x cho biết:
        // Bắt đầu cướp từ nhà thứ i, tiền nhiều nhất cướp được là x
        // base case: dp[n] = 0
        int[] dp = new int[n + 2];
        for (int i = n - 1; i >= 0; i--) {
            dp[i] = Math.max(dp[i + 1], nums[i] + dp[i + 2]);
        }
        return dp[0];
    }
}
```

Ta lại phát hiện chuyển trạng thái chỉ liên quan hai trạng thái gần nhất của `dp[i]`, nên còn tối ưu thêm, giảm độ phức tạp không gian xuống O(1).

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        // Ghi dp[i+1] và dp[i+2]
        int dp_i_1 = 0, dp_i_2 = 0;
        // Ghi dp[i]
        int dp_i = 0;
        for (int i = n - 1; i >= 0; i--) {
            dp_i = Math.max(dp_i_1, nums[i] + dp_i_2);
            dp_i_2 = dp_i_1;
            dp_i_1 = dp_i;
        }
        return dp_i;
    }
}
```

quy trình trên, trong [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) tagiải chi tiết, tin là mọi người đều dễ dàng. Mình thấythú vị là follow up của bài này, cần dựa ý tưởng hiện tạiứng biến khéo léo.

## Trộm nhà II

Bài 213「Trộm nhà II」trên LeetCode và bài trước mô tả cơ bản giống, trộm vẫn không được cướp nhà kề nhau, đầu vào vẫn là một mảng, nhưng nói bạn **các nhà này không phải một hàng, mà quây thành một vòng**.

giờ nhà đầu và nhà cuối cũngcoi như kề nhau, không được đồng thời cướp. Ví dụ nhập mảng `nums=[2,3,2]`, kết quả thuật toán trả về hẳn là 3 chứ không phải 4, vì đầu và cuối không được đồng thời cướp.

Ràng buộc này trông hẳn không khó giải, bài trước [Tổng hợp bài toán ngăn xếp đơn điệu](https://labuladong.online/algo/data-structure/monotonic-stack/) nói một phương án giải mảng vòng, vậy trên bài này xử lý thế nào?

Trước hết, phòng đầu cuối không được đồng thời cướp, thì chỉ có ba trường hợp khác nhau: hoặc đều không cướp; hoặc nhà đầu cướp nhà cuối không cướp; hoặc nhà cuối cướp nhà đầu không cướp.

![](https://labuladong.online/algo/images/robber/3.jpg)

Vậythì đơn giản nhé, ba trường hợp này, trường hợp nào kết quả lớn nhất, chính là đáp án cuối! Nhưng, thật ra ta không cần so ba trường hợp, chỉ cần so trường hợp hai và ba là được, **vì hai trường hợp nàydư địa chọn nhà lớn hơn trường hợp một nhé, tiền trong nhà đều không âm, nên dư chọn lớn, kết quả quyết định tối ưu chắc chắn không nhỏ**.

Nên chỉ sửa nhẹ cách giải trước đó là được:

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        return Math.max(robRange(nums, 0, n - 2),
                        robRange(nums, 1, n - 1));
    }

    // Định nghĩa: trả về giá trị lớn nhất cướp được trong khoảng đóng [start,end]
    int robRange(int[] nums, int start, int end) {
        int n = nums.length;
        int dp_i_1 = 0, dp_i_2 = 0;
        int dp_i = 0;
        for (int i = end; i >= start; i--) {
            dp_i = Math.max(dp_i_1, nums[i] + dp_i_2);
            dp_i_2 = dp_i_1;
            dp_i_1 = dp_i;
        }
        return dp_i;
    }
}
```

Tới đây, câu hai cũng giải xong.

## Trộm nhà III

Bài 337「Trộm nhà III」trên LeetCode lại nghĩ cáchđổi trò, tên trộm này phát hiện nhà đối mặt giờ không phải một hàng, không phải một vòng, mà là một cây nhị phân! Nhà nằm trên node cây nhị phân, hai nhànối nhau không được đồng thời cướp, quả là tội phạm trí tuệ caođồn. Chữ ký hàm như sau:

```java
int rob(TreeNode root);
```

Ví dụ nhập cây nhị phân như hình dưới:

```
     3
    / \
   2 3
    \ \
     3 1
```

Thuật toán nên trả về 7, vì cướp nhà tầng một và tầng ba được số tiền cao nhất 3 + 3 + 1 = 7.

Nếu nhập cây nhị phân như hình dưới:

```
     3
    / \
   4 5
  / \ \
 1 3 1
```

Vậy thuật toán nên trả về 9, nếu cướp nhà tầng hai được số tiền cao nhất 4 + 5 = 9.

ý tưởng tổng thể hoàn toàn không đổi, vẫn làm lựa chọn cướp hoặc không cướp, đi lựa chọnlợi lớn hơn. thậm chí Ta trực tiếp theo mô-típ này viết code:

```java
class Solution {
    Map<TreeNode, Integer> memo = new HashMap<>();
    public int rob(TreeNode root) {
        if (root == null) return 0;
        // Dùng bảng ghi nhớ loại bỏ bài toán con trùng lặp
        if (memo.containsKey(root))
            return memo.get(root);
        // Cướp, rồi đi nhà kế tiếp nữa
        int do_it = root.val
            + (root.left == null ?
                0 : rob(root.left.left) + rob(root.left.right))
            + (root.right == null ?
                0 : rob(root.right.left) + rob(root.right.right));
        // Không cướp, rồi đi nhà tiếp
        int not_do = rob(root.left) + rob(root.right);

        int res = Math.max(do_it, not_do);
        memo.put(root, res);
        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/house-robber-iii/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>👾 Animation trực quan hóa code 👾</s trong>
</summary>
</details>
</a>
<hr/>



Phân tích độ phức tạp thời gian, tuy xem cấu trúc đệ quy nàyhình như là cây bốn chạc, nhưng thực tế nhờ tối ưu bảng ghi nhớ, việc hàm đệ quy làm chính là duyệt mỗi node, không vào cùng node nhiều lần, nên độ phức tạp thời gian vẫn là $O(N)$, `N` là số node cây. Độ phức tạp không gian là kích thước bảng ghi nhớ, tức $O(N)$.

Nếu băn khoăn về phân tích độ phức tạp thời/không gian, tham khảo [Hướng dẫn thực dụng phân tích độ phức tạp thời-không gian](https://labuladong.online/algo/essential-technique/complexity-analysis/).

Nhưng điểm khéo léo của bài này là, còn cách giải đẹp hơn. Ví dụ một bạn đọc bình luận cách giải thế này:

```java
class Solution {
    int rob(TreeNode root) {
        int[] res = dp(root);
        return Math.max(res[0], res[1]);
    }

    // Trả về mảng kích thước 2 arr
    // arr[0] cho biết không cướp root, được số tiền lớn nhất
    // arr[1] cho biết cướp root, được số tiền lớn nhất
    int[] dp(TreeNode root) {
        if (root == null)
            return new int[]{0, 0};
        int[] left = dp(root.left);
        int[] right = dp(root.right);
        // Cướp, nhà tiếpthì không thể cướp
        int rob = root.val + left[0] + right[0];
        // Không cướp, nhà tiếp cướp hay không,phụ thuộc lợi thu được lớn nhỏ
        int not_rob = Math.max(left[0], left[1])
                    + Math.max(right[0], right[1]);

        return new int[]{not_rob, rob};
    }
}
```

Độ phức tạp thời gian vẫn là $O(N)$, độ phức tạp không gian chỉ không gian ngăn xếp hàm đệ quy cần, tức chiều cao cây $O(H)$, không cần không gian thêm của bảng ghi nhớ.

Bạn xem hắn và ý tưởng của ta không giống, sửa định nghĩa hàm đệ quy, sửa nhẹ ý tưởng, khiến logicnhất quán, vẫn ra đáp án đúng, mà code đẹp hơn. Đây vị trí sau của [Tư duy cây nhị phân (cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/) giảng trong bài trước.

Thực tế, cách giải này chạy nhanh được nhiều hơn cách giải của ta, tuy khía cạnh phân tích thuật toán độ phức tạp thời gian giống nhau. Nguyên nhân là cách giải này không dùng bảng ghi nhớ thêm, giảm phức tạp tính thao tác dữ liệu, nên hiệu quả chạy thực tế sẽ nhanh.








<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm chỉ Offer II 089. Trộm nhà](https://leetcode.cn/problems/Gu0c2T/?show=1)| 🟠 |
| - | [Kiếm chỉ Offer II 090. Trộm nhà vòng](https://leetcode.cn/problems/PzWKhm/?show=1)| 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

