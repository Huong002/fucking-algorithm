# Cấu trúc hàng đợi đơn điệu giải bài sliding window




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | [239. Giá trị lớn nhất của sliding window](https://leetcode.cn/problems/sliding-window-maximum/) | 🔴 |
| [ câu hỏi phỏng vấn 59 - II. hàng đợi giá trị lớn nhất LCOF](https://leetcode.com/problems/dui-lie-de-zui-da-zhi-lcof/) | [Phỏng vấn 59 - II. Giá trị lớn nhất của hàng đợi](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về mảng](https://labuladong.online/algo/data-structure-basic/array-basic/)
> - [Cơ bản về linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
> - [Cơ bản về hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/queue-stack-basic/)

Bài trước dùng [Ngăn xếp đơn điệu giải ba bài thuật toán](https://labuladong.online/algo/data-structure/monotonic-stack/) giới thiệu cấu trúc dữ liệu đặc biệt ngăn xếp đơn điệu, bài này viết một cấu trúc dữ liệu tương tự「hàng đợi đơn điệu」.

Có thể tên cấu trúc dữ liệu này bạn chưa nghe, thực ra không khó, chính là một「hàng đợi」, chỉ là dùng chút phương pháp khéo léo, khiến phần tử trong hàng đợi toàn là đơn điệu tăng (hoặc giảm).

Sao phải phát minh cấu trúc「hàng đợi đơn điệu」? Chủ yếu để giải tình huống sau:

**Cho bạn một mảng `window`, đã biết giá trị lớn nhất là `A`, nếu thêm một số `B` vào `window`, vậy so sánh `A` và `B` là có thể lập tức tính giá trị lớn nhất mới; nhưng nếu cần giảm một số khỏi mảng `window`, thì không thể trực tiếp ra giá trị lớn nhất, vì nếu số giảm vừa đúng là `A`, thì cần duyệt mọi phần tử trong `window` tìm lại giá trị lớn nhất mới**.

 tình huống này rất thường gặp, nhưng không dùng hàng đợi đơn điệu dường như cũng được, ví dụ [hàng đợi ưu tiên (binary heap)](https://labuladong.online/algo/data-structure-basic/binary-heap-basic/) chính là chuyên động tìm giá trị lớn nhất, tôi tạo một max (min) heap, không phải rất nhanh lấy được giá trị lớn (nhỏ) nhất sao?

Nếu đơn thuần duy trì giá trị lớn nhất, hàng đợi ưu tiên rất chuyên, phần tử đầu hàng đợi chính là giá trị lớn nhất. Nhưng hàng đợi ưu tiên không thỏa mãn **thứ tự thời gian**「vào trước ra trước」của cấu trúc queue chuẩn, vì tầng dưới hàng đợi ưu tiên lợi dụng binary heap sắp xếp động phần tử, thứ tự ra đội của phần tử là thứ tự kích thước phần tử, hoàn toàn không liên quan thứ tự vào hàng đợi trước sau .

Nên, giờ cần một cấu trúc queue mới, vừa duy trì được thứ tự thời gian「vào trước ra trước」của phần tử queue, vừa duy trì đúng giá trị lớn nhất của mọi phần tử trong queue, đây chính là cấu trúc「hàng đợi đơn điệu」.

Cấu trúc dữ liệu「hàng đợi đơn điệu」chủ yếu dùng phụ trợ giải bài liên quan sliding window, bài trước [Khung cốt lõi sliding window](https://labuladong.online/algo/essential-technique/sliding-window-framework/) thuật toán sliding window làm một phần của kỹ thuật hai con trỏ giải thích , nhưng vài bài sliding window hơi phức tạp không thể chỉ dựa vào hai con trỏ giải, cần lên cấu trúc dữ liệu trước vào hơn.

Ví dụ, bạn chú ý xem mấy bài giảng ở [Khung cốt lõi sliding window](https://labuladong.online/algo/essential-technique/sliding-window-framework/), mỗi khi cửa sổ mở rộng (`right++`) và thu hẹp (`left++`), bạn chỉ dựa vào phần tử chuyển vào và chuyển ra cửa sổ là có thể quyết định có cập nhật đáp án không.

Nhưng ví dụ kiểm tra giá trị lớn nhất trong một cửa sổ nói ở đầu bài, bạn không thể chỉ dựa vào phần tử chuyển ra cửa sổ đó cập nhật giá trị lớn nhất của cửa sổ, trừ phi duyệt lại mọi phần tử, nhưng vậy độ phức tạp thời gian lên cao, đây là điều chúng ta không muốn thấy.

Chúng ta xem bài 239 trên LeetCode「Giá trị lớn nhất của sliding window」, chính là một bài sliding window chuẩn:

Cho bạn nhập một mảng `nums` và một số nguyên dương `k`, có một cửa sổ kích thước `k` trượt từ trái sang phải trên `nums`, hãy thua ra mỗi lần giá trị lớn nhất trong `k` phần tử của cửa sổ.

Chữ ký hàm như sau:

```java
int[] maxSlidingWindow(int[] nums, int k);
```

Ví dụ một sample LeetCode cho:

```
Nhập: nums = [1,3,-1,-3,5,3,6,7], k = 3
Xuất: [3,3,5,5,6,7]
Giải thích:
Vị trí sliding window Giá trị lớn nhất
--------------- -----
[1 3 -1] -3 5 3 6 7 3
 1 [3 -1 -3] 5 3 6 7 3
 1 3 [-1 -3 5] 3 6 7 5
 1 3 -1 [-3 5 3] 6 7 5
 1 3 -1 -3 [5 3 6] 7 6
 1 3 -1 -3 5 [3 6 7] 7
```



Tiếp theo, chúng ta mượn cấu trúc hàng đợi đơn điệu, dùng thời gian $O(1)$ tính giá trị lớn nhất trong mỗi sliding window, khiến toàn bộ thuật toán hoàn thành trong thời gian tuyến tính.

### Một, dựng khung giải bài 

Trước khi giới thiệu API của cấu trúc dữ liệu「hàng đợi đơn điệu」, so sánh API chuẩn của [queue thường](https://labuladong.online/algo/data-structure-basic/queue-stack-basic/) và API hàng đợi đơn điệu cài đặt:

```java
// API của queue thường
class Queue {
    // Thao tác enqueue, thêm vào phần tử n ở cuối hàng đợi
    void push(int n);
    // Thao tác dequeue, xóa phần tử đầu hàng đợi
    void pop();
}

// API của hàng đợi đơn điệu
class MonotonicQueue {
    // Thêm phần tử n ở cuối hàng đợi
    void push(int n);
    // Trả về giá trị lớn nhất trong queue hiện tại
    int max();
    // Phần tử đầu hàng đợi nếu là n, xóa nó
    void pop(int n);
}
```

Dĩ nhiên, cách cài đặt mấy API của hàng đợi đơn điệu chắc chắn khác Queue thường, nhưng chúng ta tạm kệ, mà cho rằng độ phức tạp thời gian mấy thao tác này đều là O(1), dựng khung trả lời của bài「sliding window」này trước:

```java
int[] maxSlidingWindow(int[] nums, int k) {
    MonotonicQueue window = new MonotonicQueue();
    List<Integer> res = new ArrayList<>();
    
    for (int i = 0; i < nums.length; i++) {
        if (i < k - 1) {
            // Điền đầy k - 1 đầu của cửa sổ trước
            window.push(nums[i]);
        } else {
            // Cửa sổ bắt đầu trượt về trước
            // chuyển vào phần tử mới
            window.push(nums[i]);
            // Ghi phần tử lớn nhất trong cửa sổ hiện tại vào kết quả
            res.add(window.max());
            // chuyển ra phần tử cuối
            window.pop(nums[i - k + 1]);
        }
    }
    // Chuyển kiểu List thành mảng int[] làm giá trị trả về
    int[] arr = new int[res.size()];
    for (int i = 0; i < res.size(); i++) {
        arr[i] = res.get(i);
    }
    return arr;
}
```

![](https://labuladong.online/algo/images/monotonic-queue/1.png)



 ý tưởng này rất đơn giản đúng không, dưới đây chúng ta bắt đầu phần chính, cài đặt hàng đợi đơn điệu.

### Hai, cài đặt cấu trúc dữ liệu hàng đợi đơn điệu

Quan sát quá trình sliding window là phát hiện, cài đặt「hàng đợi đơn điệu」 bắt buộc dùng một cấu trúc dữ liệu hỗ trợ chèn/xóa ở đầu và cuối, rất rõ [doubly linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/) thỏa mãn điều kiện này.

 ý tưởng cốt lõi của「hàng đợi đơn điệu」giống「ngăn xếp đơn điệu」, phương thức `push` vẫn thêm phần tử ở cuối hàng đợi, nhưng phải xóa hết phần tử nhỏ hơn mình phía trước:

```java
class MonotonicQueue {
    // Doubly linked list, hỗ trợ nhanh thêm/xóa ở đầu và cuối
    // duy trì phần tử trong đó đơn điệu tăng từ cuối tới đầu
    private LinkedList<Integer> maxq = new LinkedList<>();

    // Thêm một phần tử n ở cuối, duy trì tính đơn điệu của maxq
    public void push(int n) {
        // Xóa hết phần tử nhỏ hơn mình phía trước
        while (!maxq.isEmpty() && maxq.getLast() < n) {
            maxq.pollLast();
        }
        maxq.addLast(n);
    }
}
```

Bạn có thể tưởng tượng, kích thước số thêm vào đại diện cân nặng người, nặng lớn sẽ đè bẹp nhẹ không đủ phía trước, đến khi gặp đo cấp lớn hơn mới dừng.

![](https://labuladong.online/algo/images/monotonic-queue/3.png)

Nếu mỗi phần tử khi thêm vào đều thao tác vậy, cuối cùng kích thước phần tử trong hàng đợi đơn điệu sẽ giữ thứ tự **đơn điệu giảm**, nên phương thức `max` của chúng ta rất dễ viết, chỉ cần trả về phần tử đầu hàng đợi; phương thức `pop` cũng thao tác đầu hàng đợi, nếu phần tử đầu là phần tử chờ xóa `n`, vậy thì xóa nó:

```java
class MonotonicQueue {
    // Để tiết kiệm độ dài bài viết, lược bớt phần code đã cho ở trên...

    public int max() {
        // Phần tử đầu hàng đợi chắc chắn lớn nhất
        return maxq.getFirst();
    }

    public void pop(int n) {
        if (n == maxq.getFirst()) {
            maxq.pollFirst();
        }
    }
}
```

Phương thức `pop` sở dĩ phải kiểm tra `n == maxq.getFirst()`, vì phần tử đầu hàng đợi `n` chúng ta muốn xóa có thể đã trong quá trình `push` bị「đè bẹp」, có thể đã không tồn tại, trường hợp này thì không cần xóa:

![](https://labuladong.online/algo/images/monotonic-queue/2.png)

Đến đây, hàng đợi đơn điệu thiết kế xong, xem code giải bài đầy đủ:

```java
// Cài đặt hàng đợi đơn điệu
class MonotonicQueue {
    LinkedList<Integer> maxq = new LinkedList<>();
    public void push(int n) {
        // Xóa toàn bộ phần tử nhỏ hơn n
        while (!maxq.isEmpty() && maxq.getLast() < n) {
            maxq.pollLast();
        }
        // Rồi thêm vào n vào cuối
        maxq.addLast(n);
    }
    
    public int max() {
        return maxq.getFirst();
    }
    
    public void pop(int n) {
        if (n == maxq.getFirst()) {
            maxq.pollFirst();
        }
    }
}

class Solution {
    int[] maxSlidingWindow(int[] nums, int k) {
        MonotonicQueue window = new MonotonicQueue();
        List<Integer> res = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (i < k - 1) {
                // Điền đầy k - 1 đầu cửa sổ trước
                window.push(nums[i]);
            } else {
                // Cửa sổ trượt về trước, thêm vào số mới
                window.push(nums[i]);
                // Ghi lại giá trị lớn nhất của cửa sổ hiện tại
                res.add(window.max());
                // chuyển ra số cũ
                window.pop(nums[i - k + 1]);
            }
        }
        // Cần chuyển thành mảng int[] rồi trả về
        int[] arr = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            arr[i] = res.get(i);
        }
        return arr;
    }
}
```

Có một chi tiết đừng bỏ qua, khi cài đặt `MonotonicQueue`, chúng ta dùng `LinkedList` của Java, vì cấu trúc linked list hỗ trợ thêm/xóa nhanh ở đầu và cuối; mà `res` trong code lời giải dùng cấu trúc `ArrayList`, vì sau đó sẽ lấy phần tử theo index, nên cấu trúc mảng hợp hơn. Cài đặt ngôn ngữ khác cũng chú ý chi tiết này.

Về độ phức tạp thời gian của API hàng đợi đơn điệu, độc giả có thể thắc mắc: thao tác `push` chứa vòng while, worst-case độ phức tạp thời gian phải $O(N)$, cộng thêm một tầng vòng for, độ phức tạp thời gian của thuật toán này phải $O(N^2)$ mới đúng?

Ở đây dùng tới phân tích khấu hao giảng trong [Hướng dẫn phân tích độ phức tạp thời-không thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/):

Nhìn riêng thao tác `push`, worst-case độ phức tạp thời gian đúng là $O(N)$, nhưng trung bình độ phức tạp thời gian là $O(1)$. Chúng ta thường dùng độ phức tạp trung bình chứ không phải worst-case để đo API, nên độ phức tạp thời gian tổng thể của thuật toán này là $O(N)$, chứ không phải $O(N^2)$.

Cũng có thể phân tích tổng thể thế này: việc toàn bộ thuật toán làm chính là thêm vào và chuyển ra mỗi phần tử trong `nums` khỏi `window` **nhiều nhất một lần**, không thể chuyển vào chuyển ra cùng một phần tử nhiều lần khỏi `window`, nên độ phức tạp thời gian tổng thể là $O(N)$.

Độ phức tạp không gian rất dễ phân tích, chính là kích thước cửa sổ $O(k)$.






### Mở rộng

Cuối cùng, tôi nêu mấy câu hỏi mời mọi người nghĩ:

1, Lớp `MonotonicQueue` bài này cho chỉ cài đặt phương thức `max`, bạn có thể thêm một phương thức `min`, trả về giá trị nhỏ nhất của mọi phần tử trong queue trong thời gian $O(1)$ không?

2, Phương thức `pop` của lớp `MonotonicQueue` bài này cho còn cần nhận một tham số, không thanh lịch lắm, mà trái với API queue chuẩn, mời bạn sửa khuyết điểm này.

3, Mời bạn cài đặt phương thức `size` của lớp `MonotonicQueue`, trả về số phần tử trong hàng đợi đơn điệu (chú ý, vì mỗi lần phương thức `push` đều có thể xóa phần tử khỏi danh sách tầng dưới `q`, nên số phần tử trong `q` không phải số phần tử của hàng đợi đơn điệu).

Tức là, bạn có thể cài đặt cài đặt tổng quát hàng đợi đơn điệu không:

```java
// Cài đặt tổng quát hàng đợi đơn điệu, có thể duy trì hiệu quả giá trị lớn nhất và nhỏ nhất
class MonotonicQueue<E extends Comparable<E>> {

    // API queue chuẩn, thêm vào phần tử vào cuối hàng đợi
    public void push(E elem);

    // API queue chuẩn, pop phần tử từ đầu hàng đợi, phù hợp thứ tự vào trước ra trước
    public E pop();

    // API queue chuẩn, trả về số phần tử trong queue
    public int size();

    // API đặc có hàng đợi đơn điệu, tính giá trị lớn nhất của phần tử trong queue trong thời gian O(1)
    public E max();

    // API đặc có hàng đợi đơn điệu, tính giá trị nhỏ nhất của phần tử trong queue trong thời gian O(1)
    public E min();
}
```

Tôi sẽ ở [Cài đặt tổng quát hàng đợi đơn điệu & ứng dụng](https://labuladong.online/algo/problem-set/monotonic-queue/) cho ra cài đặt tổng quát hàng đợi đơn điệu và bài tập kinh điển. Thêm nhiều bài thiết kế cấu trúc dữ liệu xem [Bài tập kinh điển về thiết kế cấu trúc dữ liệu](https://labuladong.online/algo/problem-set/ds-design/).






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Cài đặt tổng quát hàng đợi đơn điệu & bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-queue/)
 - [Hướng dẫn thực dụng phân tích độ phức tạp thời-không thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/?show=1) | [1425. Tổng dãy con có giới hạn](https://leetcode.cn/problems/constrained-subsequence-sum/?show=1) | 🔴 |
| [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/?show=1) | [1696. Game nhảy VI](https://leetcode.cn/problems/jump-game-vi/?show=1) | 🟠 |
| [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/?show=1) | [862. Mảng con ngắn nhất có tổng ít nhất K](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/?show=1) | 🔴 |
| [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/?show=1) | [918. Tổng lớn nhất của mảng con vòng](https://leetcode.cn/problems/maximum-sum-circular-subarray/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 59 - I. Giá trị lớn nhất của sliding window](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/?show=1) | 🔴 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
