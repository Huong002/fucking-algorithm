# Quy hoạch động kinh điển: Thả trứng nhà cao tầng



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo độc giả, website đã cho ra mắt [mục lục cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học các bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được bộ khung thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode bản Trung | Độ khó |
| :----: | :----: | :----: |
| [887. Super Egg Drop](https://leetcode.com/problems/super-egg-drop/) | [887. Trứng rơi](https://leetcode.cn/problems/super-egg-drop/) | 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Bài này muốn nói về một bài toán thuật toán rất kinh điển,vài tầng lầu, vài quả trứng, bắt bạn tính ra số lần thử ít nhất, tìm tầng lầu mà trứng vừa khéo không vỡ. Các hãng công nghệ lớn trong nước cũng như các buổi phỏng vấn Google, Facebook đều thường ra bài này, chỉ có điều họ thấy ném trứng quá lãng phí, đổi thành ném cốc, ném bát vỡ gì đó.

Vấn đề cụ thể lát nữa sẽ nói, nhưng kỹ thuật giải bài này có rất nhiều, riêng quy hoạch động đã có vài hướng suy nghĩ hiệu suất khác nhau, cuối cùng còn có một cách giải toán học cực kỳ hiệu quả. Trước sau như một, cuốn sách này giữ phong cách từ chối những kỹ thuật quá quái dị, vì các kỹ thuật này không thể từ một suy ra ba, học cũng không đáng.

Dưới đây dùng hướng suy nghĩ chung của quy hoạch động mà chúng ta luôn nhấn mạnh để nghiên cứu bài này một chút.






## Một, phân tích đề bài

Đây là bài 887 trên LeetCode「Trứng rơi」, tôi mô tả đề bài một chút:

Trước mặt bạn có một tòa nhà từ tầng 1 đến `N`, tổng cộng `N` tầng, rồi cho bạn `K` quả trứng (`K` ít nhất là 1). Giờ xác định tòa nhà này tồn tại tầng `0 <= F <= N`, ở tầng này thả trứng xuống, trứng **vừa khéo không vỡ** (tầng cao hơn `F` đều vỡ, tầng thấp hơn `F` đều không vỡ, nếu trứng không vỡ thì có thể nhặt về thả tiếp). Giờ hỏi bạn, trong **trường hợp xấu nhất**, bạn **ít nhất** phải thả mấy lần trứng, mới có thể **xác định** tầng `F` này?

Nghĩa là bắt bạn tìm tầng cao nhất mà trứng không vỡ `F`, nhưng gì gọi là 「trường hợp xấu nhất」 với 「ít nhất」 phải thả mấy lần? Chúng ta lần lượt lấy ví dụ là hiểu ngay.

Ví như **giờ tạm chưa xét giới hạn số lượng trứng**, có 7 tầng lầu, bạn tìm tầng mà trứng vừa khéo vỡ thế nào?

Cách nguyên thủy nhất chính là quét tuyến tính: tôi thả ở tầng 1 một lần trước, không vỡ, tôi lại đi tới tầng 2 thả một lần, không vỡ, tôi lại đi tới tầng 3...

Với chiến lược này, **trường hợp xấu nhất** chắc là tôi thử tới tầng 7 mà trứng vẫn chưa vỡ (`F = 7`), nghĩa là tôi đã thả 7 lần trứng.

Tới lúc này chắc bạn đã hiểu gì gọi là 「trường hợp xấu nhất」rồi, **trứng vỡ nhất định xảy ra khi khoảng tìm kiếm cạn kiệt**, không phải nói bạn ở tầng 1 làm rơi trứng một lần đã vỡ, đó là do bạn may mắn, không phải trường hợp xấu nhất.

Giờ hiểu thêm một chút về cái gọi là 「ít nhất」 phải thả mấy lần. Vẫn không xét giới hạn số trứng, cũng là 7 tầng lầu, chúng ta có thể tối ưu chiến lược.

Chiến lược tốt nhất là dùng hướng suy nghĩ tìm kiếm nhị phân, tôi đi tới tầng `(1 + 7) / 2 = 4` thả một lần trước:

Nếu vỡ thì cho thấy `F` nhỏ hơn 4, tôi sẽ đi tới tầng `(1 + 3) / 2 = 2` thử...

Nếu không vỡ thì cho thấy `F` lớn hơn hoặc bằng 4, tôi sẽ đi tới tầng `(5 + 7) / 2 = 6` thử...

Với chiến lược này, **trường hợp xấu nhất** chắc là thử tới tầng 7 mà trứng vẫn chưa vỡ (`F = 7`), hoặc trứng cứ vỡ mãi tới tầng 1 (`F = 0`). Nhưng bất kể loại trường hợp xấu nhất nào, chỉ cần thử `log7` làm tròn lên bằng 3 lần, ít hơn 7 lần thử lúc nãy, đây chính là cái gọi là **ít nhất** phải thả mấy lần.






Thực tế, nếu không giới hạn số trứng thì hướng suy nghĩ nhị phân hiển nhiên rút ra được số lần thử ít nhất, nhưng vấn đề là, **giờ cho bạn giới hạn số lượng trứng `K`, trực tiếp dùng hướng suy nghĩ nhị phân là không được**.

Ví như chỉ cho bạn 1 quả trứng, 7 tầng lầu, bạn dám dùng nhị phân không? Bạn trực tiếp đi tới tầng 4 thả một lần, nếu trứng không vỡ thì còn may, bạn có thể nhặt trứng lên rồi đi tới tầng cao hơn thử; nhưng nếu vỡ, bạn sẽ không còn trứng để kiểm tra tiếp, không thể xác định tầng `F` mà trứng vừa khéo không vỡ nữa.

Thực ra trường hợp này chỉ có thể dùng phương pháp quét tuyến tính, từ dưới lên trên thử thả trứng từng tầng, vậy trường hợp xấu nhất cần thả 7 lần, thuật toán trả về kết quả chắc là 7.

Có độc giả có lẽ có suy nghĩ này: tốc độ loại tầng của tìm kiếm nhị phân chắc chắn là nhanh nhất, vậy chi bằng cứ dùng tìm kiếm nhị phân, đợi tới khi chỉ còn 1 quả trứng rồi mới thực hiện quét tuyến tính, vậy kết quả thu được có phải chính là số lần thả trứng ít nhất không?

Rất tiếc, không phải, ví như đem số tầng tăng cao lên một chút, 100 tầng, cho bạn 2 quả trứng, bạn thả một lần ở tầng 50, vỡ, vậy chỉ có thể quét tuyến tính tầng 1～49, trường hợp xấu nhất phải thả 50 lần.

Nếu không dùng 「nhị phân」 mà đổi thành 「năm phân」「mười phân」 đều giảm đáng kể số lần thử trong trường hợp xấu nhất. Ví như quả trứng đầu cứ cách mười tầng lại thả, vỡ ở đâu thì quả trứng thứ hai quét tuyến tính từng tầng, tổng cộng không vượt quá 20 lần. Nghiệm tối ưu thực ra là 14 lần. Chiến lược tối ưu có rất nhiều, mà không hề có quy luật gì.

Nói nhiều lời thừa vậy, chính là để đảm bảo mọi người hiểu được ý đề bài, mà nhận thức được đề bài này quả thực phức tạp, ngay cả chúng ta tính tay cũng không dễ, vậy dùng thuật toán giải thế nào?
 






## Hai, phân tích hướng suy nghĩ

Với bài toán quy hoạch động, cứ áp thẳng khung chúng ta đã nhấn mạnh nhiều lần trước đây là được: bài này có 「trạng thái」 gì, có 「lựa chọn」 gì, rồi liệt kê vét cạn.

**「Trạng thái」 rất rõ, chính là số trứng hiện có `K` và số tầng cần kiểm tra `N`**. Theo quá trình kiểm tra, số trứng có thể giảm, phạm vi tìm kiếm tầng sẽ thu nhỏ, đây chính là sự thay đổi của trạng thái.

**「Lựa chọn」 thực ra chính là đi chọn thả trứng ở tầng nào**. Nhìn lại hướng suy nghĩ quét tuyến tính và nhị phân lúc nãy, tìm kiếm nhị phân mỗi lần chọn thả trứng ở giữa khoảng tầng, còn quét tuyến tính chọn thử từng tầng lên trên. Lựa chọn khác nhau sẽ gây ra chuyển trạng thái.

Giờ xác định rõ 「trạng thái」 và 「lựa chọn」, **hướng suy nghĩ cơ bản của quy hoạch động đã hình thành**: chắc chắn là một mảng `dp` hai chiều hoặc hàm `dp` có hai tham số trạng thái để thể hiện chuyển trạng thái; cộng thêm một vòng for để duyệt mọi lựa chọn, chọn lựa chọn tối ưu để cập nhật trạng thái:





```java
// Định nghĩa: trạng thái hiện tại là K quả trứng, đối mặt N tầng lầu
// Trả về số lần thả trứng ít nhất ở trạng thái này
int dp(int K, int N) {
    int res;
    for (int i = 1; i <= N; i++) {
        res = Math.min(res, lần này thả trứng ở tầng thứ i);
    }
    return res;
}
```



Đoạn mã giả này còn chưa thể hiện đệ quy và chuyển trạng thái, có điều khung thuật toán cơ bản đã hoàn thành.

Chúng ta chọn thả trứng ở tầng `i` xong, có thể xuất hiện hai tình huống: trứng vỡ, trứng không vỡ. **Chú ý, lúc này chuyển trạng thái đã tới**:

**Nếu trứng vỡ**, vậy số lượng trứng `K` nên trừ một, khoảng tầng tìm kiếm nên chuyển từ `[1..N]` thành `[1..i-1]`, tổng cộng `i-1` tầng;

**Nếu trứng không vỡ**, vậy số lượng trứng `K` không đổi, khoảng tầng tìm kiếm nên chuyển từ `[1..N]` thành `[i+1..N]`, tổng cộng `N-i` tầng.

![](https://labuladong.online/algo/images/drop-egg/1.jpg)

> [!NOTE]
> Độc giả tinh ý có thể hỏi,thả trứng ở tầng `i` nếu không vỡ, khoảng tìm kiếm tầng thu về các tầng trên, có phải nên bao gồm tầng `i` không nhỉ? Không cần, vì đã bao gồm rồi. Mở đầu đã nói `F` có thể bằng 0, đệ quy lên trên xong, tầng `i` thực ra tương đương tầng 0, có thể được tính tới, nên nói không hề sai.

Vì chúng ta muốn tìm số lần thả trứng trong **trường hợp xấu nhất**, nên trứng ở tầng `i` vỡ hay không vỡ, phụ thuộc vào kết quả trường hợp nào **lớn hơn**:





```java
int dp(int K, int N):
    for 1 <= i <= N:
        // số lần thả trứng ít nhất trong trường hợp xấu nhất
        res = min(res, max(
                // vỡ
                dp(K - 1, i - 1),
                // không vỡ
                dp(K, N - i),
            ) + 1
            // đã thả một lần ở tầng i, nên cộng một
        )
    return res
```

base case của đệ quy rất dễ hiểu, khi số tầng `N` bằng 0, hiển nhiên không cần thả trứng; khi số trứng `K` là 1, hiển nhiên chỉ có thể quét tuyến tính mọi tầng:

```java
int dp(int K, int N) {
    // base case
    if (K == 1) return N;
    if (N == 0) return 0;
    // ...
}
```



Đến đây, thực ra bài này đã giải xong! Chỉ cần thêm một bản ghi nhớ để loại bỏ bài toán con trùng lặp là được:

```java
class Solution {
    // bản ghi nhớ
    int[][] memo;

    public int superEggDrop(int K, int N) {
        // m nhiều nhất không vượt quá N lần (quét tuyến tính)
        memo = new int[K + 1][N + 1];
        for (int[] row : memo) {
            Arrays.fill(row, -666);
        }
        return dp(K, N);
    }

    // Định nghĩa: tay cầm K quả trứng, đối mặt N tầng lầu, số lần thả trứng ít nhất là dp(K, N)
    int dp(int K, int N) {
        // base case
        if (K == 1) return N;
        if (N == 0) return 0;

        // tra bản ghi nhớ tránh tính toán dư thừa
        if (memo[K][N] != -666) {
            return memo[K][N];
        }
        // phương trình chuyển trạng thái
        int res = Integer.MAX_VALUE;
        for (int i = 1; i <= N; i++) {
            // thử ở mọi tầng lầu, lấy số lần thả trứng ít nhất
            res = Math.min(
                res,
                // vỡ và không vỡ lấy trường hợp xấu nhất
                Math.max(dp(K, N - i), dp(K - 1, i - 1)) + 1
            );
        }
        // lưu kết quả vào bản ghi nhớ
        memo[K][N] = res;
        return res;
    }
}
```

Độ phức tạp thời gian của thuật toán này là bao nhiêu? **Độ phức tạp thời gian của thuật toán quy hoạch động chính là số lượng bài toán con × độ phức tạp của bản thân hàm**.

Độ phức tạp của bản thân hàm chính là bỏ qua độ phức tạp của phần đệ quy, ở đây hàm `dp` có một vòng for, nên độ phức tạp của bản thân hàm là O(N).

Số lượng bài toán con cũng chính là tổng số tổ hợp trạng thái khác nhau, hiển nhiên là tích của hai trạng thái, cũng chính là O(KN). Nên tổng độ phức tạp thời gian của thuật toán là O(K*N^2), độ phức tạp không gian O(KN).

Bài này rất phức tạp, nhưng code thuật toán lại vô cùng gọn gàng, đây chính là đặc tính của quy hoạch động, liệt kê vét cạn cộng tối ưu bằng bản ghi nhớ/bảng DP, thật sự không có gì mới.

Có độc giả có thể không hiểu vì sao trong code lại dùng một vòng for duyệt tầng `[1..N]`, có lẽ sẽ nhầm lẫn logic này với hướng suy nghĩ quét tuyến tính đã thảo luận trước đó. Thực ra không phải, **đây chỉ là đang thực hiện một 「lựa chọn」**.

Ví như bạn có 2 quả trứng, đối mặt 10 tầng lầu, bạn **lần này** chọn đi tới tầng nào để thả? Không biết, vậy đem 10 tầng này thử hết một lượt. Còn lần sau chọn thế nào không cần bạn lo, có chuyển trạng thái đúng, thuật toán đệ quy sẽ tính ra chi phí của mỗi lựa chọn, chúng ta lấy cái tối ưu nhất chính là nghiệm tối ưu.

Ngoài ra, bài này còn có cách giải tốt hơn, ví như sửa vòng for trong code thành tìm kiếm nhị phân, có thể giảm độ phức tạp thời gian xuống O(K\*N\*logN); cải tiến tiếp cách giải quy hoạch động có thể giảm tiếp xuống O(KN); dùng phương pháp toán học giải, độ phức tạp thời gian đạt tối ưu O(K*logN), độ phức tạp không gian đạt O(1).

Cách giải nhị phân cũng hơi dễ gây hiểu lầm, bạn rất có thể tưởng nó với hướng suy nghĩ thả trứng kiểu nhị phân chúng ta thảo luận trước đó có quan hệ, thực tế không có chút quan hệ nào. Dùng được tìm kiếm nhị phân là vì đồ thị hàm của phương trình chuyển trạng thái có tính đơn điệu, có thể nhanh chóng tìm được giá trị lớn nhất, nhỏ nhất.

Tiếp theo chúng ta xem làm sao tối ưu.






## Ba, tối ưu tìm kiếm nhị phân

Cốt lõi của tối ưu tìm kiếm nhị phân là tính đơn điệu của phương trình chuyển trạng thái, trước hết trình bày vắn tắt hướng suy nghĩ quy hoạch động gốc một chút:

1. Liệt kê brute-force thử thả trứng ở mọi tầng `1 <= i <= N`, mỗi lần chọn tầng có số lần thử **ít nhất**;

2. Mỗi lần thả trứng có hai khả năng, hoặc vỡ, hoặc không vỡ;

3. Nếu trứng vỡ, `F` chắc ở dưới tầng `i`, ngược lại, `F` chắc ở trên tầng `i`;

4. Trứng vỡ hay không vỡ, phụ thuộc vào trường hợp nào số lần thử **nhiều hơn**, vì chúng ta muốn tìm kết quả trong trường hợp xấu nhất.

Code chuyển trạng thái cốt lõi là đoạn này:





```java
// Trạng thái hiện tại là K quả trứng, đối mặt N tầng lầu
// Trả về kết quả tối ưu ở trạng thái này
int dp(int K, int N):
    for 1 <= i <= N:
        // số lần thả trứng ít nhất trong trường hợp xấu nhất
        res = min(res, max(
                    // vỡ
                    dp(K - 1, i - 1),
                    // không vỡ
                    dp(K, N - i),
                ) + 1
                // đã thả một lần ở tầng i, nên cộng một
            )
    return res
```



Vòng for này chính là cài đặt cụ thể bằng code của phương trình chuyển trạng thái dưới đây:

![](https://labuladong.online/algo/images/drop-egg/formula1.png)

Nếu có thể hiểu phương trình chuyển trạng thái này, vậy rất dễ hiểu hướng suy nghĩ tối ưu tìm kiếm nhị phân.

Trước hết chúng ta theo định nghĩa của mảng `dp(K, N)` (có `K` quả trứng đối mặt `N` tầng lầu, ít nhất cần thả mấy lần), **rất dễ thấy khi `K` cố định, hàm này theo `N` tăng nhất định đơn điệu tăng**, bất kể chiến lược của bạn thông minh cỡ nào, tầng tăng thì số lần kiểm tra nhất định tăng.

Vậy chú ý hai hàm `dp(K - 1, i - 1)` và `dp(K, N - i)`, trong đó `i` là từ 1 đến `N` đơn tăng, nếu chúng ta cố định `K` và `N`, **coi hai hàm này là hàm của `i`, hàm trước theo `i` tăng chắc cũng đơn điệu tăng, còn hàm sau theo `i` tăng chắc đơn điệu giảm**:

![](https://labuladong.online/algo/images/drop-egg/2.jpg)

Lúc này tìm giá trị lớn hơn của hai cái, rồi tìm giá trị nhỏ nhất trong các giá trị lớn nhất này, thực ra chính là tìm giao điểm của hai đường thẳng, cũng chính là điểm thấp nhất của đường gấp khúc đỏ mà.

Chúng ta ở bài [khung tư duy khi vận dụng thực tế tìm kiếm nhị phân](https://labuladong.online/algo/frequency-interview/binary-search-in-action/) phía trước đã giảng, vận dụng tìm kiếm nhị phân rất rộng, chỉ cần tìm được quan hệ hàm có tính đơn điệu, đều rất có thể vận dụng tìm kiếm nhị phân để tối ưu độ phức tạp của tìm kiếm tuyến tính. Nhìn lại đường cong của hai hàm `dp` này, điểm thấp nhất chúng ta muốn tìm thực ra chính là tình huống này:

```java
for (int i = 1; i <= N; i++) {
    if (dp(K - 1, i - 1) == dp(K, N - i))
        return dp(K, N - i);
}
```

Bạn học quen tìm kiếm nhị phân chắc chắn nhạy bén nghĩ tới, đây không phải tương đương với việc tìm giá trị Valley (thung lũng) sao, có thể dùng tìm kiếm nhị phân để nhanh chóng tìm điểm này, xem thẳng code, chuyển tìm kiếm tuyến tính của hàm `dp` thành tìm kiếm nhị phân, tăng tốc độ tìm kiếm:

```java
class Solution {
    // bản ghi nhớ
    int[][] memo;

    public int superEggDrop(int K, int N) {
        // m nhiều nhất không vượt quá N lần (quét tuyến tính)
        memo = new int[K + 1][N + 1];
        for (int[] row : memo) {
            Arrays.fill(row, -666);
        }
        return dp(K, N);
    }

    // Định nghĩa: tay cầm K quả trứng, đối mặt N tầng lầu, số lần thả trứng ít nhất là dp(K, N)
    int dp(int K, int N) {
        // base case
        if (K == 1) return N;
        if (N == 0) return 0;

        // tra bản ghi nhớ tránh tính toán dư thừa
        if (memo[K][N] != -666) {
            return memo[K][N];
        }

        // for (int i = 1; i <= N; i++) {
        //     res = Math.min(
        //         res,
        //         Math.max(dp(K, N - i), dp(K - 1, i - 1)) + 1
        //     );
        // }

        // dùng tìm kiếm nhị phân thay tìm kiếm tuyến tính
        int res = Integer.MAX_VALUE;
        int lo = 1, hi = N;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            // trứng vỡ và không vỡ ở tầng mid hai trường hợp
            int broken = dp(K - 1, mid - 1);
            int not_broken = dp(K, N - mid);
            // res = min(max(vỡ, không vỡ) + 1)
            if (broken > not_broken) {
                hi = mid - 1;
                res = Math.min(res, broken + 1);
            } else {
                lo = mid + 1;
                res = Math.min(res, not_broken + 1);
            }
        }
        memo[K][N] = res;
        return res;
    }
}
```

Độ phức tạp thời gian của thuật toán này là bao nhiêu? **Độ phức tạp thời gian của thuật toán quy hoạch động chính là số lượng bài toán con × độ phức tạp của bản thân hàm**.

Độ phức tạp của bản thân hàm chính là bỏ qua độ phức tạp của phần đệ quy, ở đây hàm `dp` dùng một tìm kiếm nhị phân, nên độ phức tạp của bản thân hàm là O(logN).

Số lượng bài toán con cũng chính là tổng số tổ hợp trạng thái khác nhau, hiển nhiên là tích của hai trạng thái, cũng chính là O(KN).

Nên tổng độ phức tạp thời gian của thuật toán là O(KNlogN), độ phức tạp không gian O(KN). Hiệu suất cao hơn thuật toán O(KN^2) trước đó một chút.

## Bốn, định nghĩa lại chuyển trạng thái

Tìm chuyển trạng thái của quy hoạch động vốn mỗi người một ý, là chuyện khá huyền học, định nghĩa trạng thái khác nhau có thể phái sinh ra cách giải khác nhau, cách giải và độ phức tạp đều có thể khác biệt rất lớn, đây chính là một ví dụ rất hay.

Nhìn lại một chút ý nghĩa của `dp` chúng ta định nghĩa trước đó:





```java
int dp(int k, int n)
// Trạng thái hiện tại là k quả trứng, đối mặt n tầng lầu
// Trả về số lần thả trứng ít nhất ở trạng thái này
```

Dùng mảng `dp` thể hiện cũng như nhau:

```java
dp[k][n] = m
// Trạng thái hiện tại là k quả trứng, đối mặt n tầng lầu
// Số lần thả trứng ít nhất ở trạng thái này là m
```



Theo định nghĩa này, chính là **xác định số trứng hiện tại và số tầng đối mặt, thì biết số lần thả trứng ít nhất**. Cuối cùng đáp án chúng ta muốn chính là kết quả của `dp(K, N)`.

Dưới hướng suy nghĩ này, chắc chắn phải liệt kê vét cạn mọi cách thả có thể, dùng tìm kiếm nhị phân tối ưu cũng chỉ là làm 「cắt tỉa」, giảm không gian tìm kiếm, nhưng hướng suy nghĩ bản chất không đổi, vẫn là liệt kê vét cạn.

Giờ, chúng ta sửa đổi một chút định nghĩa của mảng `dp`, **xác định số trứng hiện tại và số lần thả trứng tối đa cho phép, thì biết số tầng lầu cao nhất có thể xác định `F`**. Cụ thể là ý này:





```java
dp[k][m] = n
// Hiện tại có k quả trứng, có thể thử thả m lần trứng
// Ở trạng thái này, trường hợp xấu nhất kiểm tra được tối đa chính xác một tòa nhà n tầng

// Ví như nói dp[1][7] = 7 thể hiện:
// Giờ có 1 quả trứng, cho phép bạn thả 7 lần;
// Ở trạng thái này tối đa cho bạn 7 tầng lầu,
// khiến bạn có thể xác định tầng F mà trứng vừa khéo không vỡ
// (dò tuyến tính từng tầng mà)
```





Đây thực ra chính là một 「phiên bản đảo ngược」 của hướng suy nghĩ gốc chúng ta, chúng ta tạm chưa xét hướng suy nghĩ này chuyển trạng thái viết thế nào, mà hãy suy nghĩ trước một chút dưới định nghĩa này, đáp án cuối cùng muốn tìm là gì?

Đáp án cuối cùng chúng ta muốn tìm thực ra là số lần thả trứng `m`, nhưng lúc này `m` ở trong trạng thái chứ không phải kết quả của mảng `dp`, có thể xử lý như vậy:

```java
int superEggDrop(int K, int N) {

    int m = 0;
    while (dp[K][m] < N) {
        m++;
        // chuyển trạng thái...
    }
    return m;
}
```

Đề bài không phải **cho bạn `K` trứng, `N` tầng lầu, bắt bạn tìm số lần kiểm tra ít nhất `m` trong trường hợp xấu nhất** sao? Điều kiện kết thúc vòng `while` là `dp[K][m] == N`, cũng chính là **cho bạn `K` quả trứng, kiểm tra `m` lần, trường hợp xấu nhất kiểm tra được tối đa `N` tầng lầu**.

Chú ý xem hai đoạn mô tả này, là hoàn toàn giống nhau! Nên nói tổ chức code như vậy là đúng, then chốt là phương trình chuyển trạng thái tìm thế nào? Còn phải bắt đầu giảng từ hướng suy nghĩ gốc của chúng ta. Cách giải trước đó đính kèm hình này giúp mọi người hiểu hướng suy nghĩ chuyển trạng thái:

![](https://labuladong.online/algo/images/drop-egg/1.jpg)

Hình này chỉ mô tả một tầng `i` nào đó, cách giải gốc còn phải quét tuyến tính hoặc nhị phân mọi tầng, yêu cầu giá trị lớn nhất, nhỏ nhất. Nhưng loại định nghĩa `dp` này căn bản không cần mấy cái này, dựa trên hai sự thật dưới đây:

**1. Bất kể bạn thả trứng ở tầng nào, trứng chỉ có thể vỡ hoặc không vỡ, vỡ thì kiểm tra tầng dưới, không vỡ thì kiểm tra tầng trên**.

**2. Bất kể bạn lên tầng hay xuống tầng, tổng số tầng = số tầng trên + số tầng dưới + 1 (tầng hiện tại này)**.

Theo đặc điểm này, có thể viết ra phương trình chuyển trạng thái dưới đây:

```python
dp[k][m] = dp[k][m - 1] + dp[k - 1][m - 1] + 1
```

**`dp[k][m - 1]` chính là số tầng trên**, vì số trứng `k` không đổi, cũng chính là trứng không vỡ, số lần thả trứng `m` giảm một;

**`dp[k - 1][m - 1]` chính là số tầng dưới**, vì số trứng `k` giảm một, cũng chính là trứng vỡ, đồng thời số lần thả trứng `m` giảm một.

> [!NOTE]
> `m` này vì sao phải giảm một chứ không phải tăng một? Trước đó định nghĩa rất rõ, `m` này là một giới hạn trên của số lần cho phép thả trứng, chứ không phải đã thả mấy lần.

![](https://labuladong.online/algo/images/drop-egg/3.jpg)

Đến đây, toàn bộ hướng suy nghĩ đã hoàn thành, chỉ cần điền phương trình chuyển trạng thái vào khung là được:

```java
class Solution {
    public int superEggDrop(int K, int N) {
        // m nhiều nhất không vượt quá N lần (quét tuyến tính)
        int[][] dp = new int[K + 1][N + 1];
        // base case:
        // dp[0][..] = 0
        // dp[..][0] = 0
        // Java mặc định khởi tạo mảng đều là 0
        int m = 0;
        while (dp[K][m] < N) {
            m++;
            for (int k = 1; k <= K; k++)
                dp[k][m] = dp[k][m - 1] + dp[k - 1][m - 1] + 1;
        }
        return m;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/super-egg-drop/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 Animation trực quan hóa code👾</strong>
</summary>
</details>
</a>
<hr/>





Nếu bạn còn thấy đoạn code này hơi khó hiểu, thực ra nó tương đương với viết như vậy:

```java
for (int m = 1; dp[K][m] < N; m++)
    for (int k = 1; k <= K; k++)
        dp[k][m] = dp[k][m - 1] + dp[k - 1][m - 1] + 1;
```

Thấy dạng code này đã quen thuộc hơn nhiều rồi, vì cái chúng ta muốn tìm không phải giá trị trong mảng `dp`, mà là một chỉ số `m` phù hợp điều kiện nào đó, nên dùng vòng `while` để tìm `m` này mà thôi.

Độ phức tạp thời gian của thuật toán này là bao nhiêu? Rất hiển nhiên chính là độ phức tạp của hai vòng lặp lồng nhau O(KN).

Ngoài ra chú ý `dp[m][k]` chuyển trạng thái chỉ liên quan với hai trạng thái bên trái và phía trái trên, có thể theo bài [kỹ thuật nén không gian của quy hoạch động](https://labuladong.online/algo/dynamic-programming/space-optimization/) phía trước tối ưu thành mảng `dp` một chiều, chỗ này sẽ không viết.

## Năm, còn có thể tối ưu tiếp

Tiếp xuống còn có thể tối ưu tiếp, tôi sẽ không trình bày chi tiết, chỉ đơn giản nêu một chút hướng suy nghĩ.

Dựa trên hướng suy nghĩ lúc nãy, **chú ý hàm `dp(m, k)` là theo `m` đơn điệu tăng, vì khi số trứng `k` không đổi, số lần kiểm tra cho phép càng nhiều, số tầng kiểm tra được càng cao**.

Chỗ này lại có thể nhờ vào thuật toán tìm kiếm nhị phân để nhanh chóng tiệm cận điều kiện kết thúc `dp[K][m] == N` này, độ phức tạp thời gian giảm tiếp xuống O(KlogN). Có điều tôi thấy chúng ta viết được thuật toán tối ưu nhị phân O(K\*N\*logN) là được rồi, các cách giải sau này thì tôi cho rằng không quá cần thiết phải nắm vững, đem mong muốn giới hạn trong phạm vi năng lực mới có thể sở hữu niềm vui!

Có điều điều chắc chắn là, theo việc dùng tìm kiếm nhị phân thay cho tìm kiếm tuyến tính giá trị `m`, khung cơ bản của code chắc chắn là sửa vòng `while` liệt kê vét cạn `m`:

```java
// Đổi tìm kiếm tuyến tính thành tìm kiếm nhị phân
// for (int m = 1; dp[K][m] < N; m++)
int lo = 1, hi = N;
while (lo < hi) {
    int mid = (lo + hi) / 2;
    if (... < N) {
        lo = ...
    } else {
        hi = ...
    }
    
    for (int k = 1; k <= K; k++) {
        // phương trình chuyển trạng thái
    }
}
```

Tổng kết đơn giản một chút, tối ưu nhị phân đầu tiên là tận dụng tính đơn điệu của hàm `dp`, dùng kỹ thuật tìm kiếm nhị phân để nhanh chóng tìm kiếm đáp án; tối ưu thứ hai là khéo léo sửa đổi phương trình chuyển trạng thái, đơn giản hóa quy trình giải, nhưng tương ứng, logic giải bài khá khó nghĩ ra; tiếp theo còn có thể dùng một số phương pháp toán học và tìm kiếm nhị phân để tối ưu tiếp cách giải thứ hai, có điều không quá đáng để nắm vững.






<hr>
<details class="hint-container details">
<summary><strong>Các bài viết trích dẫn bài này</strong></summary>

  - [Khung tư duy khi vận dụng thực tế tìm kiếm nhị phân](https://labuladong.online/algo/frequency-interview/binary-search-in-action/)
  - [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)
  - [Quy hoạch động kinh điển: chọc bóng bay](https://labuladong.online/algo/dynamic-programming/burst-balloons/)

</details><hr>





**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
