# Cách tìm số nguyên tố hiệu quả


![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [204. Count Primes](https://leetcode.com/problems/count-primes/) | [204. Đếm số nguyên tố](https://leetcode.cn/problems/count-primes/) | 🟠 |

**-----------**



Định nghĩa số nguyên tố trông rất đơn giản, nếu một số chỉ chia hết cho 1 và chính nó thì đó là số nguyên tố.

Tuy định nghĩa số nguyên tố không phức tạp, e là không nhiều người thực sự viết được thuật toán liên quan tới số nguyên tố một cách hiệu quả.

Ví dụ LeetCode 204 「đếm số nguyên tố」, yêu cầu bạn viết một hàm như sau:

```java
// Trả về có bao nhiêu số nguyên tố trong khoảng [2, n)
int countPrimes(int n)

// ví dụ countPrimes(10) trả về 4
// vì 2,3,5,7 là số nguyên tố
```

Bạn sẽ viết hàm này thế nào? Tôi đoán mọi người sẽ viết thế này:

```java
int countPrimes(int n) {
    int count = 0;
    for (int i = 2; i < n; i++)
        if (isPrime(i)) count++;
    return count;
}

// Kiểm tra số nguyên n có phải số nguyên tố không
boolean isPrime(int n) {
    for (int i = 2; i < n; i++)
        if (n % i == 0)
            // Có ước khác
            return false;
    return true;
}
```

Viết thế này thì độ phức tạp thời gian O(n^2), vấn đề rất lớn. **Trước hết ý tưởng dùng hàm `isPrime` để hỗ trợ đã không đủ hiệu quả; hơn nữa cho dù bạn muốn dùng hàm `isPrime`, viết thuật toán kiểu này vẫn tồn tại tính toán dư thừa**.

Nói trước **nếu bạn muốn kiểm tra một số có phải số nguyên tố không, nên viết thuật toán thế nào**. Chỉ cần sửa một chút điều kiện vòng for trong code `isPrime` ở trên:

```java
boolean isPrime(int n) {
    for (int i = 2; i * i <= n; i++)
        ...
}
```

Nói cách khác, `i` không cần duyệt tới `n`, mà chỉ cần tới `sqrt(n)` là đủ. Vì sao? Lấy ví dụ, giả sử `n = 12`.




```java
12 = 2 × 6
12 = 3 × 4
12 = sqrt(12) × sqrt(12)
12 = 4 × 3
12 = 6 × 2
```


Có thể thấy, hai tích sau chính là hai tích trước đảo ngược lại, điểm tới hạn đảo chiều nằm ở `sqrt(n)`.

Nói cách khác, nếu trong khoảng `[2,sqrt(n)]` không phát hiện ước nào chia hết, thì có thể kết luận ngay `n` là số nguyên tố, vì trong khoảng `[sqrt(n),n]` chắc chắn cũng không phát hiện ước nào.

Giờ thì hàm `isPrime` có độ phức tạp thời gian giảm xuống $O(sqrt(N))$, **nhưng thực ra để implement hàm `countPrimes` chúng ta không cần hàm này**, trên đây chỉ mong bạn hiểu ý nghĩa của `sqrt(n)`, vì lát nữa còn dùng tới.

## Implement `countPrimes` hiệu quả

Phương pháp tiếp theo gọi là 「sàng số nguyên tố」, do một đại lão Hy Lạp cổ tên Eratosthenes phát minh, chúng ta từng thấy tên ông trong sách giáo khoa hồi cấp 2, vì ông là người đầu tiên tính đúng chu vi Trái Đất qua bóng của vật thể, được tôn sùng là 「cha đẻ của địa lý học」.

Quay lại vấn đề chính, ý tưởng cốt lõi của sàng số nguyên tố đi ngược với ý tưởng thông thường ở trên:

Bắt đầu từ 2, ta biết 2 là số nguyên tố, vậy 2 × 2 = 4, 3 × 2 = 6, 4 × 2 = 8... đều không thể là số nguyên tố.

Rồi ta phát hiện 3 cũng là số nguyên tố, vậy 3 × 2 = 6, 3 × 3 = 9, 3 × 4 = 12... cũng đều không thể là số nguyên tố.

Ảnh GIF của Wikipedia rất trực quan:

![](https://labuladong.online/algo/images/prime/1.gif)

Thấy tới đây, bạn có hiểu chút logic của phép loại trừ này chưa? Xem code bản đầu tiên của chúng ta:

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] isPrime = new boolean[n];
        // Khởi tạo mảng toàn true
        Arrays.fill(isPrime, true);

        for (int i = 2; i < n; i++) {
            if (isPrime[i]) {
                // Bội của i không thể là số nguyên tố nữa
                for (int j = 2 * i; j < n; j += i) {
                    isPrime[j] = false;
                }
            }
        }
        
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (isPrime[i]) count++;
        }
        
        return count;
    }
}
```

Nếu hiểu được đoạn code trên, tức là bạn đã nắm ý tưởng tổng thể, nhưng còn hai chi tiết nhỏ có thể tối ưu.

Trước hết, nhớ lại hàm kiểm tra số nguyên tố `isPrime` giới thiệu ở đầu bài, do tính đối xứng của ước, vòng for trong đó chỉ cần duyệt `[2,sqrt(n)]` là đủ. Ở đây cũng tương tự, vòng for ngoài của chúng ta cũng chỉ cần duyệt tới `sqrt(n)`:

```java
for (int i = 2; i * i < n; i++) 
    if (isPrime[i]) 
        ...
```

Ngoài ra, rất khó nhận ra vòng for trong cũng có thể tối ưu. Cách làm trước đây của chúng ta là:

```java
for (int j = 2 * i; j < n; j += i) 
    isPrime[j] = false;
```

Như vậy có thể đánh dấu mọi bội nguyên của `i` thành `false`, nhưng vẫn tồn tại tính toán dư thừa.

Ví dụ `n = 25`, khi `i = 5` thuật toán sẽ đánh dấu 5 × 2 = 10, 5 × 3 = 15 v.v., nhưng hai số này đã bị 2 × 5 và 3 × 5 đánh dấu khi `i = 2` và `i = 3` rồi.

Ta có thể tối ưu một chút, cho `j` duyệt từ `i * i` thay vì từ `2 * i`:

```java
for (int j = i * i; j < n; j += i) 
    isPrime[j] = false;
```

Như vậy, thuật toán đếm số nguyên tố đã được implement hiệu quả, thực ra thuật toán này có một cái tên là Sieve of Eratosthenes. Xem code hoàn chỉnh cuối cùng:

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] isPrime = new boolean[n];
        Arrays.fill(isPrime, true);
        for (int i = 2; i * i < n; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j < n; j += i) {
                    isPrime[j] = false;
                }
            }
        }
        
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (isPrime[i]) count++;
        }
        
        return count;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/count-primes/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Hình động minh họa code 🌈</strong>
</summary>
</details>
</a>
<hr/>



**Độ phức tạp thời gian của thuật toán này khá khó tính**, rõ ràng thời gian liên quan tới hai vòng for lồng nhau này, số phép toán của nó là:

  n/2 + n/3 + n/5 + n/7 + ...
= n × (1/2 + 1/3 + 1/5 + 1/7...)

Trong ngoặc là nghịch đảo của các số nguyên tố. Kết quả cuối cùng là $O(N * loglogN)$, bạn đọc hứng thú có thể tra chứng minh độ phức tạp thời gian của thuật toán này.

Trên đây là toàn bộ nội dung về thuật toán số nguyên tố. Thế nào, bài toán tưởng đơn giản mà có không ít chi tiết để mài giũa phải không?






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Bài tập kinh điển con trỏ đôi linked list](https://labuladong.online/algo/problem-set/linkedlist-two-pointers/)
 - [Chớp nhoáng mọi bài toán dãy số xấu xí](https://labuladong.online/algo/frequency-interview/ugly-number-summary/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [264. Ugly Number II](https://leetcode.com/problems/ugly-number-ii/?show=1) | [264. Số xấu II](https://leetcode.cn/problems/chou-shu-lcof/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer 49. Số xấu](https://leetcode.cn/problems/chou-shu-lcof/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
