# Cách giải hiệu quả bài toán hứng nước mưa



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | [11. Bình chứa nhiều nước nhất](https://leetcode.cn/problems/container-with-most-water/) | 🟠 |
| [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | [42. Hứng nước mưa](https://leetcode.cn/problems/trapping-rain-water/) | 🔴 |

**-----------**




> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
> - [Tổng hợp kỹ thuật hai con trỏ trên mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/)

LeetCode bài 42 "Hứng nước mưa" khá thú vị, tần suất xuất hiện trong đề phỏng vấn cũng khá cao, bài này sẽ tối ưu từng bước để giảng về đề này.

Trước hết xem đề bài:

<Problem slug="trapping-rain-water" />

Chính là dùng một mảng biểu thị một biểu đồ cột, hỏi bạn biểu đồ cột này nhiều nhất hứng được bao nhiêu nước.

```java
int trap(int[] height);
```

Dưới đây sẽ giới thiệu từ nông đến sâu: cách giải vét cạn -> cách giải bảng ghi nhớ -> cách giải hai con trỏ, giải quyết vấn đề này trong thời gian O(N) và không gian O(1).

## Một, ý tưởng cốt lõi

> [!TIP]
> Làm bài thuật toán, nếu với câu hỏi đề đặt ra mà không có ý tưởng, không ngại thử đơn giản hóa vấn đề, trước hết suy nghĩ từ cục bộ, trước hết viết ra cách giải đơn giản thô bạo nhất, có lẽ sẽ có điểm đột phá. Sau khi tối ưu từng bước có lẽ sẽ tìm được đáp án tối ưu.

Ví như đề này, trước hết không xét cả biểu đồ cột chứa được bao nhiêu nước, chỉ xét vị trí `i` này chứa được bao nhiêu nước?

![](https://labuladong.online/algo/images/rain-water/0.jpg)

Chứa được 2 ô nước, vì chiều cao của `height[i]` là 0, mà chỗ này nhiều nhất chứa được 2 ô nước, 2-0=2.

Vì sao vị trí `i` nhiều nhất chứa được 2 ô nước? Vì chiều cao cột nước mà vị trí `i` đạt được liên quan tới cột cao nhất bên trái và cột cao nhất bên phải của nó, chúng ta lần lượt gọi chiều cao hai cột này là `l_max` và `r_max`; **chiều cao cột nước lớn nhất của vị trí `i` chính là `min(l_max, r_max)`**.

Nói cách khác, với vị trí `i`, lượng nước chứa được là:





```python
water[i] = min(
    # Cột cao nhất bên trái
    max(height[0..i]),  
    # Cột cao nhất bên phải
    max(height[i..end]) 
) - height[i]
```

![](https://labuladong.online/algo/images/rain-water/1.jpg)

![](https://labuladong.online/algo/images/rain-water/2.jpg)



Đây chính là ý tưởng cốt lõi của vấn đề này, chúng ta có thể đơn giản viết một thuật toán vét cạn:

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int res = 0;
        for (int i = 1; i < n - 1; i++) {
            int l_max = 0, r_max = 0;
            // Tìm cột cao nhất bên phải
            for (int j = i; j < n; j++)
                r_max = Math.max(r_max, height[j]);
            // Tìm cột cao nhất bên trái
            for (int j = i; j >= 0; j--)
                l_max = Math.max(l_max, height[j]);
            // Nếu bản thân chính là cao nhất,
            // l_max == r_max == height[i]
            res += Math.min(l_max, r_max) - height[i];
        }
        return res;
    }
}
```

Cách giải này hẳn là rất trực tiếp thô bạo, độ phức tạp thời gian O(N^2), độ phức tạp không gian O(1). Nhưng rõ ràng cách tính `r_max` và `l_max` này rất vụng về, mỗi lần đều phải dùng vòng for để duyệt, chúng ta có nên tối ưu quá trình này một chút không?

## Hai, tối ưu bằng bảng ghi nhớ

Cách giải vét cạn trước đó, chẳng phải ở mỗi vị trí `i` đều phải tính `r_max` và `l_max` sao? Chúng ta trực tiếp tính trước hết kết quả ra, đừng ngốc nghếch lần nào cũng duyệt, độ phức tạp thời gian này chẳng phải giảm xuống sao?

**Chúng ta mở hai mảng `r_max` và `l_max` làm bảng ghi nhớ, `l_max[i]` biểu thị chiều cao cột cao nhất bên trái của vị trí `i`, `r_max[i]` biểu thị chiều cao cột cao nhất bên phải của vị trí `i`**. Tính trước hai mảng này cho tốt, tránh tính trùng lặp:

```java
class Solution {
    public int trap(int[] height) {
        if (height.length == 0) {
            return 0;
        }
        int n = height.length;
        int res = 0;
        // Mảng làm bảng ghi nhớ
        int[] l_max = new int[n];
        int[] r_max = new int[n];
        // Khởi tạo base case
        l_max[0] = height[0];
        r_max[n - 1] = height[n - 1];
        // Tính l_max từ trái sang phải
        for (int i = 1; i < n; i++)
            l_max[i] = Math.max(height[i], l_max[i - 1]);
        // Tính r_max từ phải sang trái
        for (int i = n - 2; i >= 0; i--)
            r_max[i] = Math.max(height[i], r_max[i + 1]);
        // Tính đáp án
        for (int i = 1; i < n - 1; i++)
            res += Math.min(l_max[i], r_max[i]) - height[i];
        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/trapping-rain-water/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Hình động trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



Tối ưu này thực ra ý tưởng gần giống cách giải vét cạn, chỉ là tránh tính trùng lặp, giảm độ phức tạp thời gian xuống O(N), đã là tối ưu rồi, nhưng độ phức tạp không gian là O(N). Dưới đây xem một cách giải tinh diệu hơn, có thể giảm độ phức tạp không gian xuống O(1).

## Ba, cách giải hai con trỏ

> [!NOTE]
> Cách giải này coi như mở rộng ý tưởng, xem là được, không cần quá chấp vào đáp án tối ưu. Vì với đa số người, trong phỏng vấn/thi viết thực tế, có thể dùng phương pháp mộc mạc để tùy cơ ứng biến, viết ra cách giải trên là được rồi. Tuy tốn thêm một chút độ phức tạp không gian, nhưng nền tảng chấm bài thông thường vẫn qua được.
> 
> Trừ khi không qua được tất cả test case, và bạn đã viết xong các bài khác còn dư thời gian, thì hãy bỏ thời gian tối ưu cách giải trên cũng chưa muộn.

Ý tưởng của cách giải này hoàn toàn giống nhau, nhưng thủ pháp implement rất khéo, lần này chúng ta cũng không dùng bảng ghi nhớ để tính trước nữa, mà dùng hai con trỏ **vừa đi vừa tính**, tiết kiệm độ phức tạp không gian.

Trước hết, xem một phần code:

```java
int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int l_max = 0, r_max = 0;
    
    while (left < right) {
        l_max = Math.max(l_max, height[left]);
        r_max = Math.max(r_max, height[right]);
        // Lúc này l_max và r_max lần lượt biểu thị gì?
        left++; right--;
    }
}
```

Với phần code này, xin hỏi `l_max` và `r_max` lần lượt biểu thị ý nghĩa gì?

Rất dễ hiểu, **`l_max` là chiều cao cột cao nhất trong `height[0..left]`, `r_max` là chiều cao cột cao nhất trong `height[right..end]`**.

Hiểu rõ điểm này, xem trực tiếp cách giải:

```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int l_max = 0, r_max = 0;

        int res = 0;
        while (left < right) {
            l_max = Math.max(l_max, height[left]);
            r_max = Math.max(r_max, height[right]);

            // res += min(l_max, r_max) - height[i]
            if (l_max < r_max) {
                res += l_max - height[left];
                left++;
            } else {
                res += r_max - height[right];
                right--;
            }
        }
        return res;
    }
}
```

Bạn xem, tư tưởng cốt lõi trong đó giống hệt trước đây, bình mới rượu cũ. Nhưng độc giả tinh ý có thể phát hiện cách giải này vẫn có chút khác biệt chi tiết:

Trong cách giải bảng ghi nhớ trước đó, `l_max[i]` và `r_max[i]` lần lượt đại diện chiều cao cột cao nhất của `height[0..i]` và `height[i..end]`.

```java
res += Math.min(l_max[i], r_max[i]) - height[i];
```

![](https://labuladong.online/algo/images/rain-water/3.jpg)

Nhưng trong cách giải hai con trỏ, `l_max` và `r_max` đại diện chiều cao cột cao nhất của `height[0..left]` và `height[right..end]`. Ví như đoạn code này:

```java
if (l_max < r_max) {
    res += l_max - height[left];
    left++; 
} 
```

![](https://labuladong.online/algo/images/rain-water/4.jpg)

`l_max` lúc này là cột cao nhất bên trái của con trỏ `left`, nhưng `r_max` không nhất định là cột cao nhất bên phải của con trỏ `left`, vậy thật sự có thể ra đáp án đúng không?

Thực ra vấn đề này phải nghĩ như vậy, chúng ta chỉ quan tâm `min(l_max, r_max)`. **Với tình huống hình trên, chúng ta đã biết `l_max < r_max` rồi, còn việc `r_max` này có phải lớn nhất bên phải không thì không quan trọng. Quan trọng là lượng nước mà `height[i]` chứa được chỉ liên quan tới hiệu với `l_max` thấp hơn**:

![](https://labuladong.online/algo/images/rain-water/5.jpg)

Như vậy, vấn đề hứng nước mưa đã được giải quyết.

## Mở rộng: Bình chứa nhiều nước nhất

Dưới đây chúng ta xem một đề rất tương tự vấn đề hứng nước mưa, LeetCode bài 11 "Bình chứa nhiều nước nhất":





<Problem slug="container-with-most-water" />

```java
// Chữ ký hàm như sau
int maxArea(int[] height);
```

Đề này và vấn đề hứng nước mưa rất tương tự, có thể hoàn toàn áp dụng ý tưởng phần trước, mà còn đơn giản hơn. Khác biệt của hai đề nằm ở:

**Vấn đề hứng nước mưa cho giống một biểu đồ histogram, mỗi hoành độ đều có chiều rộng, mà mỗi hoành độ mà đề này cho là một đường dọc, không có chiều rộng**.

Phần trước chúng ta đã thảo luận nửa ngày về `l_max` và `r_max`, thực tế đều là để tính `height[i]` chứa được bao nhiêu nước; mà trong đề này `height[i]` đã không còn chiều rộng, vậy tự nhiên dễ xử hơn nhiều.

Lấy ví dụ, nếu trong vấn đề hứng nước mưa, bạn đã biết chiều cao của `height[left]` và `height[right]`, bạn có tính được giữa `left` và `right` chứa được bao nhiêu nước không?

Không, vì bạn không biết mỗi cột giữa `left` và `right` cụ thể chứa được bao nhiêu nước, bạn phải thông qua `l_max` và `r_max` của mỗi cột để tính mới được.

Ngược lại, với đề này mà nói, bạn đã biết chiều cao của `height[left]` và `height[right]`, có tính được giữa `left` và `right` chứa được bao nhiêu nước không?

Có, vì đường dọc trong đề này không có chiều rộng, nên lượng nước chứa được giữa `left` và `right` chính là:

```python
min(height[left], height[right]) * (right - left)
```

Tương tự vấn đề hứng nước mưa, chiều cao do giá trị nhỏ hơn trong `height[left]` và `height[right]` quyết định.

Ý tưởng giải đề này vẫn là kỹ thuật hai con trỏ:

**Dùng hai con trỏ `left` và `right` co từ hai đầu vào trung tâm, vừa co vừa tính diện tích hình chữ nhật giữa `[left, right]`, lấy giá trị diện tích lớn nhất tức là đáp án**.

Trước hết xem trực tiếp code cách giải nhé:

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int res = 0;
        while (left < right) {
            // Diện tích hình chữ nhật giữa [left, right]
            int cur_area = Math.min(height[left], height[right]) * (right - left);
            res = Math.max(res, cur_area);
            // Kỹ thuật hai con trỏ, di chuyển phía thấp hơn
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/container-with-most-water/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Hình động trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



Code và vấn đề hứng nước mưa đại khái giống nhau, có điều chắc chắn có độc giả sẽ hỏi, đoạn if dưới đây vì sao phải di chuyển phía thấp hơn:





```java
            // Kỹ thuật hai con trỏ, di chuyển phía thấp hơn
if (height[left] < height[right]) {
    left++;
} else {
    right--;
}
```



**Thực ra cũng dễ hiểu, vì chiều cao hình chữ nhật do `min(height[left], height[right])` tức phía thấp hơn quyết định**:

Bạn nếu di chuyển phía thấp hơn đó, cạnh đó có thể sẽ cao lên, khiến chiều cao hình chữ nhật lớn lên, từ đó "có khả năng" khiến diện tích hình chữ nhật lớn lên; ngược lại, nếu bạn đi di chuyển phía cao hơn đó, chiều cao hình chữ nhật là dù thế nào cũng không lớn lên, nên không thể khiến diện tích trở nên lớn hơn.

Đến đây, đề này cũng được giải xong.









**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
