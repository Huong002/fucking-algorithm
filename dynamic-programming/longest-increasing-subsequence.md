# Thiết kế quy hoạch động: Dãy con tăng dài nhất (LIS)



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ nắm được mô-típ thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)| [300. Dãy con tăng dài nhất](https://leetcode.cn/problems/longest-increasing-subsequence/)| 🟠 |
| [354. Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)| [354. Bài toán phong bì búp bê Nga](https://leetcode.cn/problems/russian-doll-envelopes/)| 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung tư duy cốt lõi của quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)


Có thể có bạn đọc xem bài trước [Giải thích chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/), đãhọc được mô-típ quy hoạch động: tìm được「trạng thái」của bài toán, xác định ý nghĩa mảng/hàm `dp`, định nghĩa base case; nhưng không biết xác định「lựa chọn」thế nào, tức không tìm được quan hệ chuyển trạng thái, vẫn không viết được cách giải quy hoạch động, phải làm sao?

Đừng lo, khó của quy hoạch động vốn nằm ở tìm phương trình chuyển trạng thái đúng, bài nàythì mượn bài kinh điển「Bài toán dãy con tăng dài nhất」nói về kỹ thuật chung thiết kế quy hoạch động: **tư tưởng quy nạp toán học**.

Dãy con tăng dài nhất (Longest Increasing Subsequence, viết tắt LIS) là một bài toán thuật toán rất kinh điển, dễ nghĩ tới nhất là cách giải quy hoạch động, độ phức tạp thời gian O(N^2), ta mượn bài này trình bày từ nông tới sâu cách tìm phương trình chuyển trạng thái, cách viết cách giải quy hoạch động. Khó nghĩ tới hơn là dùng tìm kiếm nhị phân, độ phức tạp thời gian là O(NlogN), ta thông qua một trò bài đơn giản phụ trợ hiểu cách giải khéo léo này.

Bài 300「Dãy con tăng dài nhất」trên LeetCode chính là bài này:




<Problem slug="longest-increasing-subsequence"/>

```java
// Chữ ký hàm
int lengthOfLIS(int[] nums);
```

Ví dụ nhập `nums=[10,9,2,5,3,7,101,18]`, trong đó dãy con tăng dài nhất là `[2,3,7,101]`, nên output của thuật toán hẳn là 4.

Chú ýphân biệt hai danh từ「dãy con (subsequence)」và「chuỗi con (substring)」, chuỗi con nhất định liên tục, mà dãy con không nhất định liên tục. Dưới đây thiết kế thuật toán quy hoạch động giải bài này trước.

## Một, cách giải quy hoạch động

ý tưởng thiết kế cốt lõi của quy hoạch động là quy nạp toán học.

Tin là mọi ngườiquen thuộc với quy nạp toán học, cấp ba đã học, mà ý tưởng rất đơn giản. Ví dụ ta muốn chứng minh một kết luận toán học, thì **trước hết giả sử kết luận này đúng khi `k < n`, rồi theo giả sử này, tìm cách suy ra chứng minh khi `k = n` kết luận này cũng đúng **. Nếu chứng minh được, thì cho thấy kết luận này với `k` bằng số bất kỳ đều đúng.

Tương tự, ta thiết kế thuật toán quy hoạch động, chẳng phải cần một mảng dp sao? Ta có thể giả sử `dp[0...i-1]` đều đã tính xong, rồi tự hỏi: tính `dp[i]` qua các kết quả này thế nào?

Lấy thẳng bài toán dãy con tăng dài nhất ví dụ là bạn hiểu. Nhưng, trước hết phải định nghĩa rõ ý nghĩa mảng dp, tức giá trị `dp[i]` rốt cuộc đại diện gì?

**Định nghĩa của ta là: `dp[i]` cho biết độ dài dãy con tăng dài nhất kết thúc bằng số `nums[i]`**.

> [!NOTE]
> Vì sao định nghĩa vậy? Đây là mô-típ giải bài toán dãy con, bài sau [Template giải bài toán dãy con trong DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/) tổngkết mấy mô-típ thường gặp. Bạn đọc hết mọi bài quy hoạch động trong chương này, sẽ phát hiện cách định nghĩa mảng `dp` cũng chỉ mấy loại đó.

Theo định nghĩa này, ta suy được base case: giá trị khởi đầu `dp[i]` là 1, vì dãy con tăng dài nhất kết thúc bằng `nums[i]` ít nhất phải chứa chính nó.

Lấy hai ví dụ:

![](https://labuladong.online/algo/images/lis/8.jpeg)

GIF này quá trình tiến hóa của thuật toán:

![](https://labuladong.online/algo/images/lis/gif1.gif)

Theo định nghĩa này, kết quả cuối cùng của ta (độ dài lớn nhất của dãy con) hẳn là giá trị lớn nhất trong mảng dp.




```java
int res = 0;
for (int i = 0; i < dp.length; i++) {
    res = Math.max(res, dp[i]);
}
return res;
```



Bạn đọc có thể hỏi, quá trình tiến hóa thuật toán vừa rồi mỗi kết quả `dp[i]` là tanhìn bằng mắt ra, ta nên thiết kế logic thuật toán thế nào để tính đúng mỗi `dp[i]`?

Đây phần chính của quy hoạch động, thiết kế logic thuật toán chuyển trạng thái thế nào, mới chạy đúng? Ở đây cần dùng tư tưởng quy nạp toán học:

**Giả sử ta đã biết mọi kết quả `dp[0..4]`, làm sao qua các kết quả đã biết này suy ra `dp[5]`**?

![](https://labuladong.online/algo/images/lis/6.jpeg)

Theo định nghĩa mảng `dp` vừa rồi, giờ muốn tìm giá trị `dp[5]`, chính là muốn tìm dãy con tăng dài nhất kết thúc bằng `nums[5]`.

**`nums[5] = 3`, là dãy con tăng, ta chỉ cần tìm những dãy con ở trước kết thúc nhỏ hơn 3, rồi nối 3 vào cuối các dãy con này, thì có thể tạo thành một dãy con tăng mới, mà độ dài dãy con mới này cộng một**.

Trước `nums[5]` có những phần tử nào nhỏ hơn `nums[5]`? Cái này dễ tính, dùng vòng for so mộtlượt là tìm ra.

Độ dài dãy con tăng dài nhất kết thúc bằng các phần tử này là bao nhiêu? Ôn lại định nghĩa mảng `dp`, nó ghi đúng độ dài dãy con tăng dài nhấtlấy mỗi phần tử làm cuối.

Lấy ví dụ của ta, `nums[0]` và `nums[4]` đều nhỏ hơn `nums[5]`, rồiso `dp[0]` và `dp[4]`, ta cho `nums[5]` kết hợp với dãy con tăng dài hơn, đưa ra `dp[5] = 3`:




![](https://labuladong.online/algo/images/lis/7.jpeg)

```java
for (int j = 0; j < i; j++) {
    if (nums[i] > nums[j]) {
        dp[i] = Math.max(dp[i], dp[j] + 1);
    }
}
```


Khi `i = 5`, logic đoạn code này là có thể tính được `dp[5]`. Thật ra tới đây, bài toán thuật toán này ta cơ bản làm xong.

Bạn đọc có thể hỏi, vừa rồi ta chỉ tính `dp[5]` thôi, `dp[4]`, `dp[3]` tính thế nào? Tương tự quy nạp toán học, bạn đã tính được `dp[5]`, các cái khác đều tính được:




```java
for (int i = 0; i < nums.length; i++) {
    for (int j = 0; j < i; j++) {
        // Tìm phần tử nhỏ hơn nums[i] trong nums[0..i-1]
        if (nums[i] > nums[j]) {
            // Nối nums[i] vào sau, tức có thể tạo thành dãy con tăng dài dp[j] + 1,
            // kết thúc bằng nums[i]
            dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
}
```



Kết hợp base case vừa nói, dưới đây xem code đầy đủ:

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        // Định nghĩa: dp[i] cho biết độ dài dãy con tăng dài nhất kết thúc bằng số nums[i]
        int[] dp = new int[nums.length];
        // base case: mảng dp khởi tạo toàn bộ là 1
        Arrays.fill(dp, 1);
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        int res = 0;
        for (int i = 0; i < dp.length; i++) {
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/longest-increasing-subsequence/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🎃 Animation trực quan hóa code 🎃</s trong>
</summary>
</details>
</a>
<hr/>



Tới đây, bài nàythì giải xong, độ phức tạp thời gian $O(N^2)$. Tổng kết cách tìm quan hệ chuyển trạng thái DP:

1、 rõ ràng định nghĩa mảng `dp`. Bước này với mọi bài quy hoạch động đều quan trọng, nếu khôngthỏa đáng hoặc khôngrõ, sẽcản trở các bước sau.

2、Theo định nghĩa mảng `dp`, vận dụng tư tưởng quy nạp toán học, giả sử `dp[0...i-1]` đều đã biết, tìm cách tìm ra `dp[i]`, một khi bước này xong, toàn bộ bài cơ bản thì giải xong.

Nhưng nếu không hoàn thành được bước này, rất có thể chính là định nghĩa mảng `dp` chưathích hợp, cần định nghĩa lại ý nghĩa mảng `dp`; hoặc có thể là thông tin mảng `dp` lưu còn chưa đủ, không đủ suy ra đáp án bước tiếp, cần mở rộng mảng `dp` thành mảng hai chiều thậm chí ba chiều.

cách giải hiện tại là quy hoạch độngchuẩn, nhưng với bài dãy con tăng dài nhất, cách giải này không tối ưu nhất, có thể không qua được mọi test case, dưới đây giảng giải pháp hiệu quả hơn.






## Hai, cách giải tìm kiếm nhị phân

Thời gian của cách giải này là $O(NlogN)$, nhưng nói thật, người bình thường cơ bản nghĩ không ra cách giải này (có thể người chơi qua vài trò bài nào đó nghĩ ra được). Nên mọi người tìm hiểu là được, bình thường cho được cách giải quy hoạch động đã rất khá tốt.

Theo ý đề bài, mình rất khó tưởng tượng bài nàyliên quan tới tìm kiếm nhị phân. Thật ra dãy con tăng dài nhất liên quan tới một trò bài gọi là patience game, thậm chí còn có một phương pháp sắp xếp gọi là patience sorting (sắp xếp kiên nhẫn).

Để đơn giản, phần sau bỏ qua mọi chứng minh toán học, qua một ví dụ đơn giản hiểu ý tưởng thuật toán.

Trước hết, cho bạn một hàng bài poker, ta xử lý từng lá từ trái sang phải như duyệt mảng, cuối cùng phải chia các lá này thành nhiều đống.

![](https://labuladong.online/algo/images/lis/poker1.jpeg)






**Xử lý các lá bài này phải tuân thủ quy tắc sau**:

Chỉ được đặt lá điểm nhỏ lên lá điểm lớn hơn nó; nếu lá hiện tại điểm lớn không có đống nào đặt được, thì tạo mới đống mới, bỏ lá này vào; nếu lá hiện tại có nhiều đống có thể chọn, thì chọn đống bên trái nhất đặt.

Ví dụ các lá trên cuối cùng bị chia thành 5 đống (ta coi mặt bài A là lớn nhất, mặt bài 2 là nhỏ nhất).

![](https://labuladong.online/algo/images/lis/poker2.jpeg)

Vì sao gặp nhiều đống có thể chọn lại phải bỏ vào đống bên trái nhất? Vì như vậy đảm bảo lá trên đỉnh các đốngcó thứ tự (2, 4, 7, 8, Q), chứng minh lược bỏ.

![](https://labuladong.online/algo/images/lis/poker3.jpeg)

Thực hiện theo quy tắc trên, có thể tính được dãy con tăng dài nhất, số đống bài chính là độ dài dãy con tăng dài nhất, chứng minh lược bỏ.

![](https://labuladong.online/algo/images/lis/poker4.jpeg)

Ta chỉ cần viết quá trình xử lý bài thành code. Mỗi lần xử lý một lá chẳng phải phải tìm đỉnh đống phù hợp để đặt sao, đỉnh các đống chẳng phải **có thứ tự** sao, đây là có thể dùng tìm kiếm nhị phân: dùng nhị phân tìm vị trí lá hiện tại nên đặt.

> [!TIP]
> Bài trước [Giải thích chi tiết thuật toán tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/) giớithiệu chi tiết và biến thể của nhị phân, ở đây ứng dụng hoàn hảo, nếu chưa đọckhuyến khích đọc.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] top = new int[nums.length];
        // Số đống bài khởi tạo là 0
        int piles = 0;
        for (int i = 0; i < nums.length; i++) {
            // Lá bài cần xử lý
            int poker = nums[i];

            // ***** Tìm kiếm nhị phân biên trái *****
            int left = 0, right = piles;
            while (left < right) {
                int mid = (left + right) / 2;
                if (top[mid] > poker) {
                    right = mid;
                } else if (top[mid] < poker) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            // *********************************

            // Không tìm được đống phù hợp, tạo mới đống mới
            if (left == piles) piles++;
            // Đặt lá này lên đỉnh đống
            top[left] = poker;
        }
        // Số đống bài chính là độ dài LIS
        return piles;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-lis/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🍭 Animation trực quan hóa code 🍭</s trong>
</summary>
</details>
</a>
<hr/>



Tới đây, cách giải tìm kiếm nhị phân cũng giảng giải xong.

cách giải này quả thật rất khó nghĩ ra. Trước hết liên quan chứng minh toán học, ai nghĩ được thực hiện theo các quy tắc này, thì được dãy con tăng dài nhất? Thứ nữa còn vận dụng nhị phân, nếu không rõ chi tiết nhị phân, cho ý tưởng cũng rất khó viết đúng.

Nên, phương pháp này coi như mở rộng tư duy. Nhưng phương pháp thiết kế quy hoạch động nên hiểu hoàn toàn: giả sử đáp án trước đó đã biết, dùng tư tưởng quy nạp toán họcsuy diễn chuyển trạng thái đúng, cuối cùng thu được đáp án.






## Ba, mở rộng lên hai chiều

Ta xem một bài thú vị hay gặp trong đời sống, bài 354「Bài toán phong bì búp bê Nga」trên LeetCode, xem đề trước:

<Problem slug="russian-doll-envelopes" />

**Bài này thật ra là một biến thể của dãy con tăng dài nhất, vì mỗi lần lồng hợp lệ là lớn bọc nhỏ, tương đương tìm một dãy con tăng dài nhất trên mặt phẳng hai chiều, độ dài nó chính là số phong bì lồng được nhiều nhất**.

Thuật toán LIS chuẩn chuẩn trước đó chỉ tìm dãy con dài nhất trong mảng một chiều, mà phong bì của ta cho biết bằng cặp số hai chiều `(w, h)`, vận dụng thuật toán LIS vào thế nào?

![](https://labuladong.online/algo/images/nest-envelope/0.jpg)

Bạn đọc có thể nghĩ, qua `w × h` tính diện tích, rồi với diện tích chạy thuật toán LIS chuẩn chuẩn. Nhưng nghĩ thêm một chút sẽ phát hiện không được, ví dụ `1 × 10` lớn hơn `3 × 3`, nhưng hiển nhiên hai phong bì này không lồng nhau được.






cách giải của bài nàykhá khéo:

**Sắp xếp chiều rộng `w` tăng dần trước, nếu gặp `w` giống nhau, thì sắp xếp chiều cao `h` giảm dần; sau đó lấy mọi `h` làm một mảng, tính độ dài LIS trên mảng này chính là đáp án**.

Vẽ hình hiểu một chút, sắp xếp các cặp số này trước:

![](https://labuladong.online/algo/images/nest-envelope/1.jpg)

Rồi tìm dãy con tăng dài nhất trên `h`, dãy con này chính là phương án lồng tối ưu:

![](https://labuladong.online/algo/images/nest-envelope/2.jpg)

**Vậy vì sao làm vậythì tìm được dãy phong bì lồng nhau được**? Nghĩ một chút là hiểu:

Trước hết, sắp xếp chiều rộng `w` từ nhỏ tới lớn, đảm bảo chiều `w` này lồng nhau được, nên ta chỉ cầnchú tâm chiều cao `h` lồng nhau được là được.

Thứ nữa, hai phong bì `w` giống nhau không thể chứa nhau, nên với phong bì `w` giống nhau, sắp xếp chiều cao `h` giảm dần, đảm bảo trong LIS hai chiều không tồn tại nhiều phong bì `w` giống nhau (vì đề nói dài rộng giống nhau cũng không lồng được).

Xem code cách giải dưới đây:

```java
class Solution {
    // envelopes = [[w, h], [w, h]...]
    public int maxEnvelopes(int[][] envelopes) {
        int n = envelopes.length;
        // Sắp xếp rộng tăng dần, nếu rộng bằng nhau thì cao giảm dần
        Arrays.sort(envelopes, (int[] a, int[] b) -> {
            return a[0] == b[0] ?
                b[1] - a[1] : a[0] - b[0];
        });
        // Tìm LIS trên mảng cao
        int[] height = new int[n];
        for (int i = 0; i < n; i++)
            height[i] = envelopes[i][1];

        return lengthOfLIS(height);
    }

    int lengthOfLIS(int[] nums) {
        // Xem bài trước
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/russian-doll-envelopes/"target="_blank">
<details style="max-width:90%; max-height:400px">
<summary>
<s trong>🌟 Animation trực quan hóa code 🌟</s trong>
</summary>
</details>
</a>
<hr/>



Đểtái dùng hàm trước, mình chia code thành hai hàm, bạn cũng có thể gộp code, tiết kiệm không gian mảng `height`.

Vì tăng thêm test case, ở đâyphải dùng hàm `lengthOfLIS` bản tìm kiếm nhị phân mới qua được mọi test case. Như vậy độ phức tạp thời gian thuật toán là $O(NlogN)$, vì sắp xếp và tính LIS mỗi cần thời gian $O(NlogN)$, cộng lại vẫn là $O(NlogN)$; độ phức tạp không gian là $O(N)$, vì hàm tính LIS trong cần mảng `top`.




<hr>
<details class="hint-container details">
<summary><s trong>Các bài viết trích dẫn bài này</s trong></summary>

 - [【Luyện tăng cường】Hiện thực tổng quát hàng đợi đơn điệu và bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-queue/)
 - [Template giải bài toán dãy con trong DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)
 - [Hai góc nhìn liệt kê của quy hoạch động](https://labuladong.online/algo/dynamic-programming/two-views-of-dp/)
 - [Thiết kế quy hoạch động: Mảng con lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/)
 - [Tư duy khung khi học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><s trong>Các bài tập trích dẫn bài này</s trong></summary>

<s trong>Cài [plugin cày bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) rồimở các bài dưới đây để xem thẳng ý tưởng giải:</s trong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/?show=1)| [1425. Tổng dãy con có giới hạn](https://leetcode.cn/problems/constrained-subsequence-sum/?show=1)| 🔴 |
| [256. Paint House](https://leetcode.com/problems/paint-house/?show=1)🔒| [256. Sơn nhà](https://leetcode.cn/problems/paint-house/?show=1)🔒| 🟠 |
| [368. Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset/?show=1)| [368. Tập con chia hết lớn nhất](https://leetcode.cn/problems/largest-divisible-subset/?show=1)| 🟠 |
| - | [Kiếm chỉ Offer II 091. Sơn nhà](https://leetcode.cn/problems/JEj789/?show=1)| 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)

