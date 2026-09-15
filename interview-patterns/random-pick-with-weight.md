# Thuật toán chọn ngẫu nhiên theo trọng số



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [528. Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/) | [528. Chọn ngẫu nhiên theo trọng số](https://leetcode.cn/problems/random-pick-with-weight/) | 🟠 |
| - | [Kiếm chỉ Offer II 071. Sinh số ngẫu nhiên theo trọng số](https://leetcode.cn/problems/cuyjEf/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
 > - [Kỹ thuật tổng tiền tố](https://labuladong.online/algo/data-structure/prefix-sum/)
> - [Chi tiết framework tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/)

Lý do viết bài này là chơi LOL mobile. Tôi có bạn than rằng ghép hạng gặp đồng đội quá gà, tôi liền nói tôi đánh hạng thấy đồng đội cũng được mà, hình như không hố lắm?

Bạn đầy ẩn ý nói một câu: thông thường điểm ẩn tương đối cao, đánh hạng nếu không ghép được đồng đội thực lực tương đương, sẽ ghép phải vài gà mờ.

Ừ? Tôi nghĩ mấy giây thấy cậu này có gì đó sai sai, ý cậu là điểm ẩn của tôi thấp, hay nói tôi chính là gà mờ đó?

Tôi lập tức đòi lập team với cậu đánh một trận, chứng minh tôi không phải gà mờ, cậu mới là gà mờ. Kết quả lập team ở đây không tiện tiết lộ, mọi người đoán đi.

Đánh xong tôi liền lên viết bài, vì tôi có chút suy nghĩ về cơ chế ghép trận của game.

![](https://labuladong.online/algo/images/random-weight/images.png)

**Cái gọi là "điểm ẩn" tôi không biết có thật không, dù sao cơ chế ghép trận là khâu cốt lõi của mọi game đối kháng, hẳn vô cùng phức tạp, không phải vài chỉ số đơn giản là xử lý được**.

Nhưng nếu đơn giản hóa cơ chế "điểm ẩn" này, thì đây là một bài toán thuật toán đáng suy nghĩ: hệ thống ghép trận với xác suất ngẫu nhiên khác nhau thế nào?

Hay đơn giản nói, chọn ngẫu nhiên theo trọng số thế nào?

Đừng thấy dễ, nếu cho bạn một mảng dài `n`, bắt bạn rút ngẫu nhiên đẳng xác suất một phần tử, bạn chắc chắn làm được, random một số `[0, n-1]` làm chỉ số là được, xác suất mỗi phần tử được chọn đều là `1/n`.

Nhưng giả sử mỗi phần tử đều có trọng số khác nhau, độ lớn trọng số đại diện cho độ lớn xác suất rút trúng phần tử này, bạn viết thuật toán lấy ngẫu nhiên phần tử thế nào?

LeetCode 528 「chọn ngẫu nhiên theo trọng số」 chính là vấn đề này:

<Problem slug="random-pick-with-weight" />

Ta cùng suy nghĩ vấn đề này, giải bài toán chọn ngẫu nhiên phần tử theo trọng số.






## Ý tưởng giải

Trước ôn lại bài lịch sử liên quan thuật toán ngẫu nhiên của ta:

Bài trước [thiết kế cấu trúc dữ liệu xóa ngẫu nhiên](https://labuladong.online/algo/data-structure/random-set/) chủ yếu khảo sát cách dùng cấu trúc dữ liệu, mỗi lần chuyển phần tử tới đuôi mảng rồi xóa, có thể tránh di chuyển dữ liệu.

Bài trước [bàn thuật toán ngẫu nhiên trong game](https://labuladong.online/algo/frequency-interview/random-algorithm/) giảng kinh điển "thuật toán lấy mẫu hồ nước", dùng phép toán đơn giản, rút đẳng xác suất phần tử trong dãy vô hạn.

Bài trước [mẹo thi viết thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/) tôi còn chia sẻ một mẹo lấy điểm dùng xác suất tối đa hóa tỉ lệ qua test.

**Nhưng các bài cũ trên không giải được vấn đề bài này nêu, ngược lại bài trước [kỹ thuật tổng tiền tố](https://labuladong.online/algo/data-structure/prefix-sum/) cộng [chi tiết tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/) có thể giải thuật toán chọn ngẫu nhiên theo trọng số**.

Thuật toán ngẫu nhiên này với kỹ thuật tổng tiền tố và kỹ thuật tìm kiếm nhị phân có quan hệ gì? Hãy nghe tôi từ từ nói.

Giả sử mảng trọng số input của bạn là `w = [1,3,2,1]`, ta muốn xác suất phù hợp trọng số, vậy có thể trừu tượng một chút, theo trọng số vẽ một đoạn thẳng màu thế này:

![](https://labuladong.online/algo/images/random-weight/1.jpeg)

Nếu tôi ném ngẫu nhiên một hòn đá lên đoạn thẳng, đá rơi vào màu nào, tôi sẽ chọn chỉ số trọng số mà màu đó tương ứng, vậy xác suất mỗi chỉ số được chọn có phải liên quan với trọng số không?

**Nên, bạn nhìn kỹ lại đoạn màu này giống gì? Đây chẳng phải [mảng tổng tiền tố](https://labuladong.online/algo/data-structure/prefix-sum/) sao**:

![](https://labuladong.online/algo/images/random-weight/2.jpeg)






Vậy tiếp, mô phỏng ném đá lên đoạn thẳng thế nào?

Đương nhiên là số ngẫu nhiên, ví dụ với mảng tổng tiền tố `preSum` trên, phạm vi lấy giá trị là `[1, 7]`, vậy tôi sinh một số ngẫu nhiên trong khoảng này `target = 5`,giống như ném ngẫu nhiên một hòn đá trong đoạn này:

![](https://labuladong.online/algo/images/random-weight/3.jpeg)

Còn một vấn đề, trong `preSum`không hề có phần tử 5, ta nên chọn phần tử nhỏ nhất lớn hơn 5, chính là 6, tức chỉ số 3 của mảng `preSum`:

![](https://labuladong.online/algo/images/random-weight/4.jpeg)

**Tìm nhanh phần tử nhỏ nhất lớn bằng giá trị mục tiêu trong mảng thế nào? [Thuật toán tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/) chính là thứ ta muốn**.

Tới đây, ý tưởng cốt lõi của đề này đã nói xong, chủ yếu mấy bước:

1, Theo mảng trọng số `w` sinh mảng tổng tiền tố `preSum`.

2, Sinh một số ngẫu nhiên lấy giá trị trong `preSum`, dùng thuật toán tìm kiếm nhị phân tìm chỉ số phần tử nhỏ nhất lớn bằng số ngẫu nhiên này.

3, Cuối trừ một với chỉ số này (vì mảng tổng tiền tố có một lệch chỉ số), là có thể làm chỉ số mảng trọng số, tức đáp án cuối:

![](https://labuladong.online/algo/images/random-weight/5.jpeg)






## Code cách giải

Ý tưởng trên hẳn không khó hiểu, nhưng lúc viết code hố có thể không ít.

Phải biết đề liên quan khoảng đóng mở, lệch chỉ số và tìm kiếm nhị phân, cần bạn nắm chắc chi tiết thuật toán vô cùng chính xác, nếu không sẽ ra đủ loại bug khó dò.

Dưới đây moi chi tiết, tiếp tục ví dụ trước:

![](https://labuladong.online/algo/images/random-weight/3.jpeg)

Ví dụ mảng `preSum` này, bạn thấy số ngẫu nhiên `target` nên lấy giá trị trong phạm vi nào? Khoảng đóng `[0, 7]` hay nửa đóng nửa mở `[0, 7)`?

Đều không phải, nên trong khoảng đóng `[1, 7]` chọn, **vì 0 trong mảng tổng tiền tố về bản chất là một chỗ giữ chỗ**, ngẫm kỹ một chút:

![](https://labuladong.online/algo/images/random-weight/6.jpeg)

Nên phải viết code như vậy:

```java
int n = preSum.length;
// Phạm vi lấy giá trị của target là khoảng đóng [1, preSum[n - 1]]
int target = rand.nextInt(preSum[n - 1]) + 1;
```

Tiếp, trong `preSum` tìm chỉ số phần tử nhỏ nhất lớn bằng `target`, nên dùng loại tìm kiếm nhị phân nào? Tìm biên trái hay biên phải?

Thực tế nên dùng tìm kiếm nhị phân tìm biên trái:

```java
// Tìm kiếm nhị phân biên trái
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left;
}
```

Bài trước [chi tiết tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/)chú trọng giảng trường hợp mảng tồn tại phần tử mục tiêu trùng, chưa giảng kỹ trường hợp phần tử mục tiêu không tồn tại, ở đây bổ sung một chút.

**Khi phần tử mục tiêu `target` không tồn tại trong mảng `nums`,giá trị trả về của tìm kiếm nhị phân biên trái có thể hiểu theo mấy loại sau**:

1, Giá trị trả về này là chỉ số phần tử nhỏ nhất lớn bằng `target` trong `nums`.

2, Giá trị trả về này là vị trí chỉ số `target` nên chèn trong `nums`.

3, Giá trị trả về này là số phần tử nhỏ hơn `target` trong `nums`.

Ví dụ trong mảng có thứ tự `nums = [2,3,5,7]` tìm `target = 4`, thuật toán nhị phân biên trái sẽ trả về 2, bạn đem vào cách nói trên, đều đúng.

Nên ba cách hiểu trên đều tương đương, có thể theo kịch bản đề cụ thể linh hoạt dùng, rõ ràng ở đây ta cần loại một.

Tóm lại, ta có thể viết ra code cách giải cuối:

```java
class Solution {
    // Mảng tổng tiền tố
    private int[] preSum;
    private Random rand = new Random();
    
    public Solution(int[] w) {
        int n = w.length;
        // Dựng mảng tổng tiền tố, lệch một vị trí cho preSum[0]
        preSum = new int[n + 1];
        preSum[0] = 0;
        // preSum[i] = sum(w[0..i-1])
        for (int i = 1; i <= n; i++) {
            preSum[i] = preSum[i - 1] + w[i - 1];
        }
    }
    
    public int pickIndex() {
        int n = preSum.length;
        // Method nextInt(n) của Java sinh một số nguyên ngẫu nhiên trong [0, n)
        // Cộng một nữa chính là chọn ngẫu nhiên một số trong khoảng đóng [1, preSum[n - 1]]
        int target = rand.nextInt(preSum[n - 1]) + 1;
        // Lấy chỉ số của target trong mảng tổng tiền tố preSum
        // Đừng quên mảng tổng tiền tố preSum và mảng gốc w có một lệch chỉ số
        return left_bound(preSum, target) - 1;
    }

    // Tìm kiếm nhị phân biên trái
    private int left_bound(int[] nums, int target) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid;
            }
        }
        return left;
    }
}
```

Có bước đệm trước, tin rằng bạn có thể hiểu hoàn toàn code trên, bài trọng số ngẫu nhiên này đã giải xong.

Hay có bạn bình luận trêu rằng, mỗi lần đều xem bài tôi "luyện đề trên mây", xem xong là biết, cũng không cần đích thân động tay làm.

Nhưng tôi muốn nói, nhiều đề ý tưởng vừa nói là hiểu, nhưng sâu một chút nhiều chi tiết đều có thể có hố, đề bài này giảng chính là một ví dụ, nên vẫn khuyên làm nhiều thực hành, nhiều tổng kết.




<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Dùng mảng tăng cường hash table (ArrayHashMap)](https://labuladong.online/algo/data-structure-basic/hashtable-with-array/)
 - [Bàn thuật toán ngẫu nhiên trong game](https://labuladong.online/algo/frequency-interview/random-algorithm/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
