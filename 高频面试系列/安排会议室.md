#Kỹ thuật quét đường thẳng: sắp xếp phòng họp



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)🔒 | [253. Phòng họp II](https://leetcode.cn/problems/meeting-rooms-ii/)🔒 | 🟠 |

**-----------**



Trước phỏng vấn, bị hỏi một bài thuật toán rất kinh điển mà rất thực dụng: bài toán sắp xếp phòng họp.

Bài tương tự trên LeetCode là đề hội viên, bạn có thể không làm được, nhưng với loại đề thuật toán kinh điển này, nắm ý tưởng vẫn cần.

Nói trước đề, LeetCode 253 「Phòng họp II」:

Cho bạn input vài khoảng dạng `[begin, end]`, đại diện thời gian bắt đầu và kết thúc vài cuộc họp, hãy tính ít nhất cần xin bao nhiêu phòng họp.

Chữ ký hàm như sau:

```java
// Trả về số phòng họp cần xin
int minMeetingRooms(int[][] meetings);
```

Ví dụ cho bạn input `meetings = [[0,30],[5,10],[15,20]]`, thuật toán nên trả về 2, vì hai cuộc họp sau xung đột thời gian với cuộc đầu, ít nhất xin hai phòng mới để mọi cuộc họp suôn sẻ.

Nếu thời gian giữa các cuộc họp chồng lấp, thì phải xin thêm phòng để họp, muốn cầu ít nhất cần bao nhiêu phòng, chính là bắt bạn tính trong cùng thời điểm nhiều nhất có bao nhiêu cuộc họp đồng thời.

Nói cách khác, **nếu coi thời gian bắt đầu mỗi cuộc họp là một đoạn khoảng, thì đề chính là bắt bạn cầu nhiều nhất có mấy khoảng chồng lấp**, chỉ vậy thôi.

Ta trước cũng học thuật toán liên quan khoảng, nếu bạn có ấn tượng với [kỹ thuật mảng hiệu](https://labuladong.online/algo/data-structure/diff-array/), hẳn đầu tiên nghĩ tới việc dùng kỹ thuật đó giải bài này.

Bài này tương đương với việc nói, cho bạn một mảng vốn toàn 0, rồi cho bạn vài khoảng, bắt bạn cộng 1 mọi phần tử trong mỗi khoảng, hỏi bạn cuối cùng giá trị lớn nhất trong mảng là bao nhiêu. Đây chính là kịch bản thực dụng kinh điển của mảng hiệu đúng không, áp thẳng class `Difference` bài trước cho là giải được.

Nhưng kỹ thuật mảng hiệu có một vấn đề, chính là bạn bắt buộc phải dựng ra mảng khởi đầu toàn 0 đó. Vì ta dùng chỉ số mảng biểu thị thời gian, nên độ dài mảng này phụ thuộc vào giá trị lớn nhất của khoảng thời gian.

Ví dụ input `meetings = [[0,30],[5,10],[15,20]]`, vậy bạn phải dựng một mảng dài 30. Vậy nếu input `meetings = [[0,30],[5,10],[10^8,10^9]]`, thì bạn phải dựng một mảng dài 10^9, điều này rõ ràng có vấn đề. Nhưng đề này cho quy mô dữ liệu lấy giá trị thời gian nhiều nhất 10^6, không tính là đặc biệt lớn, dùng cách mảng hiệu hẳn qua được.

Nhưng bài này dạy bạn thêm một kỹ thuật xử lý khoảng, không cần dựng mảng lớn vậy, cũng khéo léo giải được bài này.






## Mở rộng đề

Ta trước đã viết nhiều bài liên quan điều độ khoảng, ở đây tiện giúp mọi người chải lại ý tưởng loại vấn đề này:

**Cảnh một**, giả sử giờ chỉ có một phòng họp, còn vài cuộc họp, bạn làm sao xếp càng nhiều cuộc họp vào phòng này càng tốt?

Vấn đề này cần sắp xếp các cuộc họp (khoảng) này theo thời gian kết thúc (đầu phải), rồi xử lý, xem chi tiết bài trước [thuật toán tham lam quản thời gian](https://labuladong.online/algo/frequency-interview/interval-scheduling/).

**Cảnh hai**, cho bạn vài đoạn video ngắn, và một đoạn video dài, hãy bạn từ đoạn ngắn chọn càng ít càng tốt vài đoạn, nối ra đoạn dài này.

Vấn đề này cần sắp xếp các đoạn video (khoảng) này theo thời gian bắt đầu (đầu trái), rồi xử lý, xem chi tiết bài trước [cắt video ra một thuật toán tham lam](https://labuladong.online/algo/frequency-interview/cut-video/).

**Cảnh ba**, cho bạn vài khoảng, trong đó có thể vài khoảng tương đối ngắn, bị khoảng khác bao phủ hoàn toàn, hãy bạn xóa các khoảng bị bao phủ này.

Vấn đề này cần sắp xếp các khoảng này theo đầu trái, rồi sẽ tìm và xóa những khoảng bị bao phủ hoàn toàn, xem chi tiết bài trước [xóa khoảng bao phủ](https://labuladong.online/algo/practice-in-action/interval-problem-summary/).

**Cảnh bốn**, cho bạn vài khoảng, hãy bạn gộp mọi khoảng có phần chồng lấp.

Vấn đề này cần sắp xếp các khoảng này theo đầu trái, tiện tìm ra khoảng tồn tại chồng lấp, xem chi tiết bài trước [gộp khoảng chồng lấp](https://labuladong.online/algo/practice-in-action/interval-problem-summary/).

**Cảnh năm**, có hai phòng ban đồng thời đặt vài đoạn thời gian của cùng một phòng họp, hãy bạn tính đoạn xung đột của phòng họp.

Vấn đề này chính là cho bạn hai list khoảng, hãy bạn tìm giao của hai nhóm khoảng này, điều này cần bạn sắp xếp các khoảng này theo đầu trái, xem chi tiết bài trước [vấn đề giao của khoảng](https://labuladong.online/algo/practice-in-action/interval-problem-summary/).

**Cảnh sáu**, giả sử giờ chỉ có một phòng họp, còn vài cuộc họp, sắp xếp họp sao để thời gian nhàn rỗi của phòng này ít nhất?

Vấn đề này cần động não, nói trắng đây chính là một biến dạng bài toán ba lô 0-1:

Phòng họp có thể coi là một ba lô, mỗi cuộc họp có thể coi là một vật phẩm, giá trị vật phẩm chính là thời lượng cuộc họp, hỏi bạn chọn vật phẩm (cuộc họp) thế nào mới tối đa hóa giá trị (thời gian dùng phòng) trong ba lô?

Đương nhiên, ở đây ràng buộc ba lô không phải một trọng lượng lớn nhất, mà là các vật phẩm (cuộc họp) không thể xung đột nhau. Sắp các cuộc họp theo thời gian kết thúc, rồi tham khảo ý tưởng bài trước [chi tiết bài toán ba lô 0-1](https://labuladong.online/algo/dynamic-programming/knapsack1/) và TreeMap là giải được.

LeetCode 1235 "lập kế hoạch làm thêm" chính là đề tương tự, tôi ở ý tưởng trong plugin cho giải đáp chi tiết, bạn có thể cài [plugin Chrome](https://labuladong.online/algo/intro/chrome/) của tôi để xem, ở đây tôi sẽ không tốn giấy mực nữa.

**Cảnh bảy**, chính là kịch bản bài này muốn giảng, cho bạn vài cuộc họp, để bạn tối thiểu hóa số phòng họp cần xin.

Rồi, ví dụ nhiều vậy, xem vấn đề hôm nay giải sao.






## Phân tích đề

Lặp lại bản chất đề:

**Cho bạn input vài khoảng thời gian, để bạn tính trong cùng thời điểm "nhiều nhất" có mấy khoảng chồng lấp**.

Điểm mấu chốt của đề nằm ở việc, cho bạn thời điểm tùy ý, bạn có thể nói ra thời điểm này có mấy cuộc họp không?

Nếu làm được, vậy tôi duyệt mọi thời điểm, tìm giá trị lớn nhất, chính là số phòng họp cần xin.

Có cấu trúc dữ liệu hay thuật toán nào, cho tôi input vài khoảng, tôi có thể biết mỗi vị trí có bao nhiêu khoảng chồng lấp?

Độc giả cũ chắc chắn liên tưởng tới một kỹ thuật thuật toán trước đã nói: [kỹ thuật mảng hiệu](https://labuladong.online/algo/data-structure/diff-array/).

Tưởng tượng trục thời gian thành một mảng giá trị khởi đầu 0, mỗi khoảng thời gian `[i, j]` tương đương một mảng con, khoảng thời gian này có một cuộc họp, vậy tôi sẽ đem mọi phần tử mảng con này cộng một.

Cuối, mỗi thời điểm có mấy cuộc họp tôi chẳng phải biết sao? Tôi duyệt cả mảng, chẳng phải biết ít nhất cần mấy phòng sao?

Lấy ví dụ, nếu input `meetings = [[0,30],[5,10],[15,20]]`, vậy ta sẽ cho các khoảng chỉ số `[0,30],[5,10],[15,20]` trong mảng này lần lượt cộng một, cuối cùng duyệt mảng, cầu giá trị lớn nhất là được.

Còn nhớ không, kỹ thuật mảng hiệu có thể trong thời gian O(1) cộng trừ phần tử cả khoảng, nên có thể đem ra giải bài này.

Nhưng, hiệu suất cách giải này không tính là cao, nên tôi ở đây không chuẩn bị viết cụ thể cách giải mảng hiệu, tham chiếu nguyên lý [kỹ thuật mảng hiệu](https://labuladong.online/algo/data-structure/diff-array/), bạn hứng thú có thể tự thử implement.






**Dựa vào ý tưởng mảng hiệu, ta có thể suy ra một cách giải hiệu quả hơn, tao nhã hơn**.

Ta trước hết chiếu các khoảng thời gian cuộc họp này lên trục:

![](https://labuladong.online/algo/images/arrange-room/1.jpeg)

Điểm đỏ đại diện thời điểm bắt đầu mỗi cuộc họp, điểm xanh đại diện thời điểm kết thúc mỗi cuộc họp.

Giờ tưởng tượng có một đường mang bộ đếm, quét trên trục thời gian từ trái sang phải, mỗi khi gặp điểm đỏ, bộ đếm `count` cộng một, mỗi khi gặp điểm xanh, bộ đếm `count` trừ một:

![](https://labuladong.online/algo/images/arrange-room/2.jpeg)

**Như vậy, mỗi thời điểm có bao nhiêu cuộc họp đồng thời, chính là giá trị bộ đếm `count`, giá trị lớn nhất của `count`, chính là số phòng họp cần xin**.

Độc giả quen kỹ thuật mảng hiệu nhìn một cái là thấy, quét đường này thực chất chính là quá trình duyệt mảng hiệu, nên ta nói đây là cách giải dẫn xuất từ kỹ thuật mảng hiệu.

## Implement code

Vậy, viết code sao implement quá trình quét này?

Trước hết, chiếu khoảng lên trục tương đương với sắp xếp riêng điểm đầu và điểm cuối của mỗi khoảng:

![](https://labuladong.online/algo/images/arrange-room/3.jpeg)

```java
int minMeetingRooms(int[][] meetings) {
    int n = meetings.length;
    int[] begin = new int[n];
    int[] end = new int[n];
    // Lấy riêng đầu trái và đầu phải ra
    for(int i = 0; i < n; i++) {
        begin[i] = meetings[i][0];
        end[i] = meetings[i][1];
    }
    // Sắp xong chính là điểm đỏ trong hình
    Arrays.sort(begin);
    // Sắp xong chính là điểm xanh trong hình
    Arrays.sort(end);

    // ...
}
```

Rồi thì đơn giản, đường quét tiến từ trái sang phải, gặp điểm đỏ thì cộng một vào bộ đếm, gặp điểm xanh thì trừ một khỏi bộ đếm, giá trị lớn nhất của bộ đếm `count` chính là đáp án:

```java
class Solution {
    public int minMeetingRooms(int[][] meetings) {
        int n = meetings.length;
        int[] begin = new int[n];
        int[] end = new int[n];
        for(int i = 0; i < n; i++) {
            begin[i] = meetings[i][0];
            end[i] = meetings[i][1];
        }
        Arrays.sort(begin);
        Arrays.sort(end);

        // Bộ đếm trong quá trình quét
        int count = 0;
        // Kỹ thuật hai con trỏ
        int res = 0, i = 0, j = 0;
        while (i < n && j < n) {
            if (begin[i] < end[j]) {
                // Quét tới một điểm đỏ
                count++;
                i++;
            } else {
                // Quét tới một điểm xanh
                count--;
                j++;
            }
            // Ghi giá trị lớn nhất trong quá trình quét
            res = Math.max(res, count);
        }
        
        return res;
    }
}
```

Ở đây dùng [kỹ thuật hai con trỏ](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/), theo vị trí tương đối `i, j` mô phỏng quá trình đường quét tiến.

Tới đây, bài này đã làm xong. Đương nhiên, đề này cũng có thể biến dạng, ví dụ cho bạn vài cuộc họp, hỏi bạn `k` phòng họp có đủ không, thực ra bạn áp code cách giải bài này, cũng có thể rất nhẹ nhàng giải được.






<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [1235. Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/?show=1) | [1235. Lập kế hoạch làm thêm](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/?show=1) | 🔴 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
