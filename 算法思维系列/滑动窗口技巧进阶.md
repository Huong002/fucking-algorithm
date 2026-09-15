# Mẫu code cốt lõi thuật toán cửa sổ trượt (Sliding Window)

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học đượccông thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | [3. Chuỗi con dài nhất không ký tự trùng](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) | 🟠 |
| [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) | [438. Tìm mọi từ dị vị trong chuỗi](https://leetcode.cn/problems/find-all-anagrams-in-a-string/) | 🟠 |
| [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/) | [567. Hoán vị của chuỗi](https://leetcode.cn/problems/permutation-in-string/) | 🟠 |
| [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | [76. Chuỗi cửa sổ nhỏ nhất](https://leetcode.cn/problems/minimum-window-substring/) | 🔴 |

**-----------**

> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về mảng](https://labuladong.online/algo/data-structure-basic/array-basic/)

Về cách dùng con trỏ nhanh-chậm và con trỏ trái-phải của hai con trỏ, có thể xem bài trước [Tổng hợp kỹ thuật hai con trỏ](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/), bài này sẽ giải một loại kỹ thuật hai con trỏ khó nắm nhất: kỹ thuật cửa sổ trượt, và tổng kết một bộ khung, có thể đảm bảo bạn nhắm mắt cũng viết ra cách giải đúng.

## Tổng quan khung cửa sổ trượt

**kỹ thuật thuật toán cửa sổ trượt chủ yếu dùng để giải vấn đề mảng con, ví như bắt bạn tìm mảng con dài nhất/ngắn nhất phù hợp điều kiện nào đó**.

Nếu dùng giải bạo lực, bạn cần vòng for lồng nhauvét cạn như vậy mọi mảng con, độ phức tạp thời gian là $O(N^2)$:

```java
for (int i = 0; i < nums.length; i++) {
    for (int j = i; j < nums.length; j++) {
        // nums[i, j] là một mảng con
    }
}
```

ý tưởng kỹ thuật thuật toán cửa sổ trượt cũng không khó, chính là duy trì một cửa sổ, không ngừng trượt, rồi cập nhật đáp án, logic đại khái của thuật toán như sau:

```java
int left = 0, right = 0;

while (right < nums.size()) {
    // Tăng cửa sổ
    window.addLast(nums[right]);
    right++;

    while (window needs shrink) {
        // Thu nhỏ cửa sổ
        window.removeFirst(nums[left]);
        left++;
    }
}
```

Code viết ra dựa trên khung thuật toán cửa sổ trượt, độ phức tạp thời gian là $O(N)$, hơn giải bạo lực vòng for lồng nhau hiệu quả cao.

::: info Tại sao là $O(N)$?

Khẳng định có độc giả cần hỏi, khung cửa sổ trượt này của bạn chẳng phải cũng dùng một vòng while lồng nhau? Tại sao độ phức tạp là $O(N)$?

Nói đơn giản, con trỏ `left, right` sẽ không lùi (giá trị của chúng chỉ tăng không giảm), nên mỗi phần tử trong chuỗi/mảng đều chỉ vào cửa sổ một lần, rồi được dời khỏi cửa sổ một lần, sẽ không nói có một số phần tử nhiều lần vào và rời cửa sổ, nên độ phức tạp thời gian của thuật toán tương quan với độ dài chuỗi/mảng thành tỉ lệ thuận.

Ngược lại giải bạo lực vòng for lồng nhau, `j` đó sẽ lùi, nên một số phần tử sẽ vào và rời cửa sổ nhiều lần, nên độ phức tạp thời gian chính là $O(N^2)$.

Tôi trong [Hướng dẫn thực dụng phân tích độ phức tạp thời-không thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/) có dạy cụ thể mọi người ước tính độ phức tạp thời-không từ lý thuyết thế nào, chỗ này sẽ không mở rộng.

:::

::: info Tại sao cửa sổ trượt có thể thời gian $O(N)$vét cạn mảng con?

Vấn đề này bản thân chính là sai, **cửa sổ trượt không có thể vét cạn ra mọi chuỗi con**. Muốnvét cạn ra mọi chuỗi con, bắt buộc dùng vòng for lồng nhau đó.

Tuy nhiên với một số đề, và không cần vét cạn mọi chuỗi con, thì có thể tìm được đáp án đề muốn. Cửa sổ trượt chính là một bộ mẫu thuật toán trong cảnh này, giúp bạn với quá trìnhvét cạn tiến hành cắt tỉa tối ưu, tránh tính toán dư thừa.

Nên trong [Bản chất thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/) tôi xếp thuật toán cửa sổ trượt vào loại "làm sao vét cạn thông minh".

:::

Thực ra khó nhiễu mọi người, không phảiý tưởng thuật toán, mà đủ loại vấn đề chi tiết. Ví như làm sao thêm phần tử mới vào cửa sổ, làm sao thu nhỏ cửa sổ, ở giai đoạn nào cửa sổ trượt thì cập nhật kết quả. Dù bạn hiểu những chi tiết này, code cũng dễ ra bug, tìm bug còn không biết tìm thế nào, thật khá khiến người bực mình.

**Nên hôm nay tôi sẽ viết một bộ khung code thuật toán cửa sổ trượt, tôi viết sẵn cho bạn cả chỗ nào cần in debug, sau này gặp vấn đề liên quan, bạn sẽ viết ra từ trí nhớ khung như sau rồi sửa ba chỗ là được, đảm bảo không ra bug**.

Vì ví dụ của bài này đa số là đề liên quan chuỗi con, chuỗi thực tế chính là mảng, nên tôi rồi đem nhập đặt thành chuỗi: khi bạn làm đề căn cứ đề cụ thể tự tùy biến là được:

```java
// Khung mã giả thuật toán cửa sổ trượt
void slidingWindow(String s) {
    // Dùng cấu trúc dữ liệu phù hợp ghi dữ liệu trong cửa sổ, căn cứ cảnh cụ thể tùy biến
    // Ví như, tôi muốn ghi số lần phần tử trong cửa sổ xuất hiện, sẽ dùng map
    // Nếu tôi muốn ghi tổng phần tử trong cửa sổ, là có thể chỉ dùng một int
    Object window = ...;

    int left = 0, right = 0;
    while (right < s.length()) {
        // c là ký tự sẽ dời vào cửa sổ
        char c = s[right];
        window.add(c)
        // Tăng cửa sổ
        right++;
        // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
        ...

        // *** Vị trí output debug ***
        // Chú ý trong code cách giải cuối cùng đừng print
        // Vì thao tác IO rất tốn thời gian, có thể dẫn đến timeout
        printf("window: [%d, %d)\n", left, right);
        // ***********************

        // kiểm tra cửa sổ trái có cần co không
        while (left < right && window needs shrink) {
            // d là ký tự sẽ dời khỏi cửa sổ
            char d = s[left];
            window.remove(d)
            // Thu nhỏ cửa sổ
            left++;
            // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
            ...
        }
    }
}
```

**Hai chỗ `...` trong khung biểu thị chỗ cập nhật dữ liệu cửa sổ, trong đề cụ thể, việc bạn cần làm chính là tới chỗ này điền logic code**. Hơn nữa, thao tác ở hai chỗ `...` này lần lượt là thao tác cập nhật mở rộng và thu nhỏ cửa sổ, lát nữa bạn sẽ phát hiện thao tác của chúng hoàn toàn đối xứng.

Nói ngoài lề, có độc giả bình luận khung này của tôi, nói bảng băm tốc độ chậm, không bằng dùng mảng thay bảng băm; còn có người thích đem code viết đặc biệt ngắn gọn, nói tôi code như vậy quá nhiều thừa thãi, tốc độ không đủ nhanh. Ý kiến của tôi là, thuật toán chủ yếu xem độ phức tạp thời gian, bạn có thể đảm bảo độ phức tạp thời gian của mình tối ưu là được. Còn tốc độ chạy LeetCode, cái đó hơi huyền học, chỉ cần không phải chậm quá đáng sẽ không vấn đề gì, căn bản không đáng để bạn tối ưu từ phạm vi biên dịch, đừng bỏ gốc theo ngọn...

Lại nói, giáo trình thuật toán của tôi trọng điểm nằm ở tư tưởng thuật toán, bạn trước hết cần đem tư duy khung vận dụng thành thạo, rồi tùy bạn tùy biến code, đảm bảo bạn viết sao cũng viết đúng.

Trở lại vấn đề chính, dưới đây sẽ trực tiếp lấy bốn đạo đề gốc Lực khấu tới áp dụng khung này, trong đó đạo đầu sẽ giải thích chi tiết nguyên lý, bốn đạo sau sẽ trực tiếp nhắm mắtxử gọn.

## Một, chuỗi cửa sổ nhỏ nhất

Xem trước LeetCode 76 "Chuỗi cửa sổ nhỏ nhất" độ khó Hard:

<Problem slug="minimum-window-substring" />

Chính là nói cần `S`(source) tìm chuỗi con chứa toàn bộ chữ cái trong `T`(target), mà chuỗi con này nhất định là ngắn nhất trong mọi chuỗi con có thể.

Nếu chúng ta dùng giải bạo lực, code đại khái như sau:

```java
for (int i = 0; i < s.length(); i++)
    for (int j = i + 1; j < s.length(); j++)
        if s[i:j] chứa mọi chữ cái của t:
            cập nhật đáp án
```

ý tưởng rất trực tiếp, nhưng hiển nhiên, độ phức tạp của thuật toán này chắc chắn lớn hơn O(N^2), không tốt.

**ý tưởng thuật toán cửa sổ trượt là như sau**:

1, Chúng ta trong chuỗi `S` dùngkỹ thuật con trỏ trái-phải trong hai con trỏ, khởi tạo `left = right = 0`, đem đoạn chỉ số**đóng-trái mở-phải** `[left, right)` gọi là một "cửa sổ".

::: tip Tại sao cần đoạn "đóng-trái mở-phải"

**Về lý bạn có thể thiết kế đoạn hai đầu đều mở hoặc hai đầu đều đóng, nhưng thiết kế thành đoạn đóng-trái mở-phải là tiện xử lý nhất**.

Vì như vậy khi khởi tạo `left = right = 0` khi đoạn `[0, 0)` trong không có phần tử, nhưng chỉ cần để `right` dời sang phải (mở rộng) một bit, đoạn `[0, 1)` sẽ chứa một phần tử `0`.

Nếu bạn đặt thành đoạn hai đầu đều mở, vậy để `right` dời sang phải một bit xong đoạn mở `(0, 1)` vẫn không có phần tử; nếu bạn đặt thành đoạn hai đầu đều đóng, vậy đoạn ban đầu `[0, 0]` sẽ chứa một phần tử. Hai tình huống này đều sẽ cho xử lý biên dẫn đến phiền phức không cần thiết.

:::

2, Chúng ta trước không ngừng tăng con trỏ `right` mở rộng cửa sổ `[left, right)`, cho đến khi chuỗi trong cửa sổ phù hợp yêu cầu (chứa mọi ký tự trong `T`).

3, Lúc này, chúng ta dừng tăng `right`, chuyển sang không ngừng tăng con trỏ `left` thu nhỏ cửa sổ `[left, right)`, cho đến khi chuỗi trong cửa sổ không còn phù hợp yêu cầu (không chứa mọi ký tự trong `T` nữa). Đồng thời, mỗi lần tăng `left`, chúng ta đều cần cập nhật một vòng kết quả.

4, Lặp bước 2 và 3, cho đến khi `right` đến điểm cuối của chuỗi `S`.

ý tưởng này thực ra cũng không khó, **bước 2tương đương với đang tìm một "nghiệm khả thi", rồi bước 3 đang tối ưu "nghiệm khả thi" này, cuối cùng tìm giải tối ưu**, cũng chính là chuỗi phủ ngắn nhất. Con trỏ trái-phải luân phiên tiến, kích thước cửa sổ tăng tăng giảm giảm, sẽ giống một con sâu lông, một co một duỗi, không ngừng trượt sang phải, đây chính là lai lịch tên "cửa sổ trượt".

Dưới đây vẽ hình hiểu một chút, `needs` và `window` tương đương với bộ đếm, lần lượt ghi số lần ký tự trong `T` xuất hiện và số lần xuất hiện của ký tự tương ứng trong "cửa sổ".

Trạng thái ban đầu:

![](https://labuladong.online/algo/images/slidingwindow/1.png)

Tăng `right`, cho đến khi cửa sổ `[left, right)` chứa mọi ký tự trong `T`:

![](https://labuladong.online/algo/images/slidingwindow/2.png)

Bây giờ bắt đầu tăng `left`, thu nhỏ cửa sổ `[left, right)`:

![](https://labuladong.online/algo/images/slidingwindow/3.png)

Cho đến khi chuỗi trong cửa sổ không còn phù hợp yêu cầu, `left` không tiếp tục di nữa:

![](https://labuladong.online/algo/images/slidingwindow/4.png)

Về sau lặp quá trình trên, trước dời `right`, rồi dời `left`... Cho đến khi con trỏ `right` đến điểm cuối của chuỗi `S`, thuật toán kết thúc.

Nếu bạn có thể hiểu quá trình trên, chúc mừng, bạn đã hoàn toàn nắm tư tưởng thuật toán cửa sổ trượt.**Bây giờ chúng ta xem khung code cửa sổ trượt này dùng thế nào**:

Trước hết, khởi tạo hai bảng băm `window` và `need`, ghi ký tự trong cửa sổ và ký tự cần gom đủ:

```java
// Ghi số lần ký tự trong window xuất hiện
HashMap<Character, Integer> window = new HashMap<>();
// Ghi số lần ký tự cần xuất hiện
HashMap<Character, Integer> need = new HashMap<>();
for (int i = 0; i < t.length(); i++) {
    char c = t.charAt(i);
    need.put(c, need.getOrDefault(c, 0) + 1);
}
```

Rồi, dùng biến `left` và `right` khởi tạo hai đầu cửa sổ, đừng quên, đoạn `[left, right)` đóng-trái mở-phải, nên trường hợp ban đầu cửa sổ không chứa bất kỳ phần tử nào:

```java
int left = 0, right = 0;
int valid = 0;
while (right < s.length()) {
    // c là ký tự sẽ dời vào cửa sổ
    char c = s.charAt(right);
    // Dời phải cửa sổ
    right++;
    // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
    ...
}
```

**Trong đó biến `valid` biểu thị số lượng ký tự trong cửa sổ thỏa điều kiện `need`**, nếu `valid` và kích thước của `need.size` giống nhau, thì giải thích cửa sổ đã thỏa điều kiện, đã hoàn toàn phủ chuỗi `T`.

**Bây giờ bắt đầu áp dụng mẫu, chỉ cần suy nghĩ mấy câu hỏi sau**:

1, Lúc nào hẳn dời `right` mở rộng cửa sổ? Khi cửa sổ thêm vào ký tự, hẳn cập nhật dữ liệu nào?

2, Lúc nào cửa sổ hẳn dừng mở rộng, bắt đầu dời `left` thu nhỏ cửa sổ? Khi dời ký tự khỏi cửa sổ, hẳn cập nhật dữ liệu nào?

3, Kết quả chúng ta muốn hẳn khi mở rộng cửa sổ hay khi thu nhỏ cửa sổ tiến hành cập nhật?

Nếu một ký tự vào cửa sổ, hẳn tăng bộ đếm `window`; nếu một ký tự khi sẽ dời khỏi cửa sổ, hẳn giảm bộ đếm `window`; khi `valid` thỏa `need` khi hẳn co cửa sổ; hẳn khi co cửa sổ thì cập nhật kết quả cuối cùng.

Dưới đây là code đầy đủ:

```java
class Solution {
    public String minWindow(String s, String t) {
        Map<Character, Integer> need = new HashMap<>();
        Map<Character, Integer> window = new HashMap<>();
        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        int left = 0, right = 0;
        int valid = 0;
        // Ghi chỉ số bắt đầu và độ dài của chuỗi phủ nhỏ nhất
        int start = 0, len = Integer.MAX_VALUE;
        while (right < s.length()) {
            // c là ký tự sẽ dời vào cửa sổ
            char c = s.charAt(right);
            // Mở rộng cửa sổ
            right++;
            // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (window.get(c).equals(need.get(c)))
                    valid++;
            }

            // kiểm tra cửa sổ trái có cần co không
            while (valid == need.size()) {
                // Ở đây cập nhật chuỗi phủ nhỏ nhất
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }
                // d là ký tự sẽ dời khỏi cửa sổ
                char d = s.charAt(left);
                // Thu nhỏ cửa sổ
                left++;
                // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
                if (need.containsKey(d)) {
                    if (window.get(d).equals(need.get(d)))
                        valid--;
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        // Trả về chuỗi phủ nhỏ nhất
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
    }
}
```

<visual slug='minimum-window-substring' >

Bạn có thể bấm mở panel trực quan hóa bên dưới, bấm nhiều lần <code type="click">while (right < s.length)</code> dòng code này, là có thể thấy quá trình trượt của cửa sổ trượt `[left, right)`:

</visual>

::: warning Độc giả dùng Java chú ý

Khi so sánh lớp bọc Java cần đặc biệt cẩn thận, kiểu `Integer`, `String` hẳn dùng phương thức `equals`kiểm tra bằng nhau, mà không thể trực tiếp dùng dấu bằng `==`, nếu không sẽ lỗi. Nên khi thu nhỏ cửa sổ cập nhật dữ liệu, không thể viết trực tiếp thành `window.get(d) == need.get(d)`, mà cần dùng `window.get(d).equals(need.get(d))`, code đề về sau cùng lý.

:::

Trong code trên, khi chúng ta phát hiện một ký tự nào đó số lượng trong `window` thỏa nhu cầu của `need`, sẽ cần cập nhật `valid`, biểu thị có một ký tự đã thỏa yêu cầu. Hơn nữa, bạn có thể phát hiện, hai lần thao tác cập nhật dữ liệu trong cửa sổ hoàn toàn đối xứng.

Khi `valid == need.size()` khi, giải thích mọi ký tự trong `T` đã được phủ, đã nhận được một chuỗi phủ khả thi, bây giờ hẳn bắt đầu co cửa sổ, để nhận được "chuỗi phủ nhỏ nhất".

Khi dời `left` co cửa sổ khi, ký tự trong cửa sổ đều là nghiệm khả thi, nên hẳn ở giai đoạn co cửa sổ tiến hành cập nhật chuỗi phủ nhỏ nhất, để từ nghiệm khả thi tìm kết quả cuối cùng độ dài ngắn nhất.

Đến đây, hẳn có thể hoàn toàn hiểu bộ khung này, thuật toán cửa sổ trượt lại không khó, chính là vấn đề chi tiết khiến người ta phiền chết được.**Sau này gặp thuật toán cửa sổ trượt, bạn cứ theo khung này viết code, bảo đảm không có bug, còn đỡ việc**.

Dưới đây sẽ trực tiếp lợi dụng bộ khung nàyxử gọn vài bài đề, bạn cơ bản một mắt là có thể nhận ra ý tưởng.

## Hai, hoán vị chuỗi

Đây là LeetCode 567 "Hoán vị của chuỗi", độ khó trung bình:

<Problem slug="permutation-in-string" />

Chú ý, `s1` nhập có thể chứa ký tự trùng, nên độ khó bài này không nhỏ.

Loại đề này, là thuật toán cửa sổ trượt rõ ràng, ** tương đương cho bạn một `S` và một `T`, hỏi bạn trong `S` có tồn tại một chuỗi con, chứa mọi ký tự trong `T` mà không chứa ký tự khác không**?

Trước hết, trước copy paste code khung thuật toán trước đó, rồi làm rõ mấy câu hỏi vừa nêu, là có thể viết ra đáp án bài này:

```java
class Solution {
    // kiểm tra trong s có tồn tại hoán vị của t không
    public boolean checkInclusion(String t, String s) {
        Map<Character, Integer> need = new HashMap<>();
        Map<Character, Integer> window = new HashMap<>();
        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        int left = 0, right = 0;
        int valid = 0;
        while (right < s.length()) {
            char c = s.charAt(right);
            right++;
            // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (window.get(c).intValue() == need.get(c).intValue())
                    valid++;
            }

            // kiểm tra cửa sổ trái có cần co không
            while (right - left >= t.length()) {
                // Ở đâykiểm tra có tìm được chuỗi con hợp lệ không
                if (valid == need.size())
                    return true;
                char d = s.charAt(left);
                left++;
                // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
                if (need.containsKey(d)) {
                    if (window.get(d).intValue() == need.get(d).intValue())
                        valid--;
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        // Chưa tìm được chuỗi con phù hợp điều kiện
        return false;
    }
}
```

<visual slug='permutation-in-string' >

Bạn có thể bấm mở panel trực quan hóa bên dưới, bấm nhiều lần <code type="click">while (right < s.length)</code> dòng code này, là có thể thấy quá trình trượt của cửa sổ độ dài cố định:

</visual>

Với code cách giải của bài này, cơ bản và chuỗi phủ nhỏ nhất giống hệt nhau, chỉ cần đổi vài chỗ:

1, Bài này thời cơ dời `left` thu nhỏ cửa sổ là khi kích thước cửa sổ lớn hơn `t.length()` khi, vì hoán vị, hiển nhiên độ dài hẳn giống nhau.

2, Khi phát hiện `valid == need.size()` khi, thì cho thấy trong cửa sổ chính là một hoán vị hợp lệ, nên ngay lập tức trả về `true`.

Còn xử lý mở rộng và thu nhỏ cửa sổ thế nào, và chuỗi phủ nhỏ nhất hoàn toàn giống nhau.

> [!NOTE]
> Do trong bài này `[left, right)` thực raduy trì là một cửa sổ** độ dài cố định**, độ dài cửa sổ là `t.length()`. Vì cửa sổ độ dài cố định mỗi lần trượt về trước chỉ dời ra một ký tự, nên hoàn toàn có thể đem while trong sửa thành if, hiệu quả giống nhau.

## Ba, tìm mọi từ dị vị

Đây là LeetCode 438 "Tìm mọi từ dị vị trong chuỗi", độ khó trung bình:

<Problem slug="find-all-anagrams-in-a-string" />

haha, cái gọi là từ dị vị này, chẳng phải chính là hoán vị sao, bày ra một cách nói cao siêu là có thể đánh lừa người sao?** Nói trắng ra, nhập một chuỗi `S`, một chuỗi `T`, tìm trong `S` mọi hoán vị của `T`, trả về chỉ số bắt đầu của chúng**.

Trực tiếp viết ra từ trí nhớ một chút khung, làm rõ 4 câu hỏi vừagiảng, là có thểxử gọn bài này:

```java
class Solution {
    public List<Integer> findAnagrams(String s, String t) {
        Map<Character, Integer> need = new HashMap<>();
        Map<Character, Integer> window = new HashMap<>();
        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        int left = 0, right = 0;
        int valid = 0;
        // Ghi kết quả
        List<Integer> res = new ArrayList<>();
        while (right < s.length()) {
            char c = s.charAt(right);
            right++;
            // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
            if (need.containsKey(c)) {
                window.put(c, window.getOrDefault(c, 0) + 1);
                if (window.get(c).equals(need.get(c))) {
                    valid++;
                }
            }
            // kiểm tra cửa sổ trái có cần co không
            while (right - left >= t.length()) {
                // Khi cửa sổ phù hợp điều kiện khi, đem chỉ số bắt đầu thêm vào res
                if (valid == need.size())
                    res.add(left);
                char d = s.charAt(left);
                left++;
                // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
                if (need.containsKey(d)) {
                    if (window.get(d).equals(need.get(d))) {
                        valid--;
                    }
                    window.put(d, window.get(d) - 1);
                }
            }
        }
        return res;
    }
}
```

Cùng tìm hoán vị của chuỗi giống nhau, chỉ là sau khi tìm được một từ dị vị (hoán vị) hợp lệ đem chỉ số bắt đầu thêm vào `res` là được.

<visual slug='find-all-anagrams-in-a-string' >

Bạn có thể bấm mở panel trực quan hóa bên dưới, bấm nhiều lần <code type="click">while (right < s.length)</code> dòng code này, là có thể thấy quá trình trượt của cửa sổ độ dài cố định:

</visual>

## Bốn, chuỗi con không trùng dài nhất

Đây là LeetCode 3 "Chuỗi con không ký tự trùng dài nhất", độ khó trung bình:

<Problem slug="longest-substring-without-repeating-characters" />

Bài này cuối cùng có chút điểm mới, không phải một bộ khung sẽ ra đáp án, có điều ngược lại đơn giản hơn, sửa chút khung là được:

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> window = new HashMap<>();
        int left = 0, right = 0;
        // Ghi kết quả
        int res = 0;
        while (right < s.length()) {
            char c = s.charAt(right);
            right++;
            // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
            window.put(c, window.getOrDefault(c, 0) + 1);
            // kiểm tra cửa sổ trái có cần co không
            while (window.get(c) > 1) {
                char d = s.charAt(left);
                left++;
                // Tiến hành một loạt cập nhật dữ liệu trong cửa sổ
                window.put(d, window.get(d) - 1);
            }
            // Ở đây cập nhật đáp án
            res = Math.max(res, right - left);
        }
        return res;
    }
}
```

<visual slug='longest-substring-without-repeating-characters' >

Bạn có thể bấm mở panel trực quan hóa bên dưới, bấm nhiều lần <code type="click">while (right < s.length)</code> dòng code này, là có thể thấy quá trình trượt cửa sổ cập nhật đáp án:

</visual>

Đây chính là biến đơn giản, liền `need` và `valid` cũng không cần, mà cập nhật dữ liệu trong cửa sổ cũng chỉ cần đơn giản cập nhật bộ đếm `window` là được.

Khi giá trị `window[c]` lớn hơn 1 khi, giải thích trong cửa sổ tồn tại ký tự trùng, không phù hợp điều kiện, sẽ nên dời `left` thu nhỏ cửa sổ.

Duy nhất cần chú ý là, ở đâu cập nhật kết quả `res`? Chúng ta muốn là chuỗi con không trùng dài nhất, giai đoạn nào có thể đảm bảo chuỗi trong cửa sổ là không trùng?

Chỗ này và trước đó không giống, cần co cửa sổ hoàn thành xong cập nhật `res`, vì điều kiện while co cửa sổ là tồn tại phần tử trùng, nói cách khác sau khi co xong nhất định đảm bảo trong cửa sổ không trùng.

Rồi, mẫu thuật toán cửa sổ trượt sẽ giảng đến đây, hy vọng mọi người có thể hiểu tư tưởng trong đó, nhớ mẫu thuật toán và thấu hiểu và vận dụng linh hoạt. Nhắc lại một chút, gặp vấn đề mảng con/chuỗi con, bạn chỉ cần có thể trả lời ra mấy câu hỏi sau, là có thể vận dụng thuật toán cửa sổ trượt:

1, Lúc nào hẳn mở rộng cửa sổ?

2, Lúc nào hẳn thu nhỏ cửa sổ?

3, Lúc nào hẳn cập nhật đáp án?

Tôi trong [Bài tập kinh điển cửa sổ trượt](https://labuladong.online/algo/problem-set/sliding-window/) dùng bộ mô thức tư duy này liệt kê thêm nhiều bài tập kinh điển, nhằm tăng cường hiểu và nhớ của bạn với thuật toán, sau này sẽ lại cũng không sợ vấn đề chuỗi con, mảng con.

<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập】Bài tập kinh điểnkỹ thuật tổng tiền tố](https://labuladong.online/algo/problem-set/perfix-sum/)
 - [【Luyện tập】Cài đặt tổng quát hàng đợi đơn điệu và bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-queue/)
 - [【Luyện tập】Bài tập kinh điển thuật toán cửa sổ trượt](https://labuladong.online/algo/problem-set/sliding-window/)
 - [Thiết kế quy hoạch động: Mảng con lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/)
 - [Cấu trúc hàng đợi đơn điệu giải vấn đề cửa sổ trượt](https://labuladong.online/algo/data-structure/monotonic-queue/)
 - [Kỹ thuật hai con trỏxử gọn bảy đạo đề mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/)
 - [Học tư duy khung (framework) của cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Mở rộng: Giải thích chi tiết sắp xếp trộn và ứng dụng](https://labuladong.online/algo/practice-in-action/merge-sort/)
 - [Cửa sổ trượt mở rộng: Thuật toánkhớp ký tự Rabin Karp](https://labuladong.online/algo/practice-in-action/rabinkarp/)
 - [kỹ thuật mảng vòng](https://labuladong.online/algo/data-structure-basic/cycle-array/)
 - [Trọng tâm và bẫy thường gặp khi luyện đề thuật toán](https://labuladong.online/algo/intro/how-to-learn-algorithms/)
 - [Hướng dẫn thực dụng phân tích độ phức tạp thời-không thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/)

</details><hr>

<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của tôi](https://labuladong.online/algo/intro/chrome/) bấm vào các đề sau có thể xem trực tiếpý tưởng giải:</strong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/?show=1) | [1004. Số lượng 1 liên tục lớn nhất III](https://leetcode.cn/problems/max-consecutive-ones-iii/?show=1) | 🟠 |
| [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/?show=1) | [1438. Mảng con liên tục dài nhất có hiệu tuyệt đối không vượt quá giới hạn](https://leetcode.cn/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/?show=1) | 🟠 |
| [1658. Minimum Operations to Reduce X to Zero](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/?show=1) | [1658. Số thao tác ít nhất để giảm x về 0](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/?show=1) | 🟠 |
| [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/?show=1) | [209. Mảng con độ dài nhỏ nhất](https://leetcode.cn/problems/minimum-size-subarray-sum/?show=1) | 🟠 |
| [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/?show=1) | [219. Tồn tại phần tử trùng II](https://leetcode.cn/problems/contains-duplicate-ii/?show=1) | 🟢 |
| [220. Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/?show=1) | [220. Tồn tại phần tử trùng III](https://leetcode.cn/problems/contains-duplicate-iii/?show=1) | 🔴 |
| [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/?show=1)🔒 | [340. Chuỗi con dài nhất chứa nhiều nhất K ký tự khác nhau](https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/?show=1)🔒 | 🟠 |
| [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/?show=1) | [395. Chuỗi con dài nhất có ít nhất K ký tự trùng](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/?show=1) | 🟠 |
| [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/?show=1) | [424. Ký tự trùng dài nhất sau thay thế](https://leetcode.cn/problems/longest-repeating-character-replacement/?show=1) | 🟠 |
| [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/?show=1) | [560. Mảng con có tổng bằng K](https://leetcode.cn/problems/subarray-sum-equals-k/?show=1) | 🟠 |
| [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/?show=1) | [713. Mảng con có tích nhỏ hơn K](https://leetcode.cn/problems/subarray-product-less-than-k/?show=1) | 🟠 |
| [862. Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/?show=1) | [862. Mảng con ngắn nhất có tổng ít nhất K](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/?show=1) | 🔴 |
| - | [Kiếm chỉ Offer 48. Chuỗi con dài nhất không chứa ký tự trùng](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer 57 - II. Dãy số nguyên dương liên tục có tổng bằng s](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/?show=1) | 🟢 |
| - | [Kiếm chỉ Offer II 008. Mảng con ngắn nhất có tổng lớn hơn bằng target](https://leetcode.cn/problems/2VG8Kg/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 009. Mảng con có tích nhỏ hơn K](https://leetcode.cn/problems/ZVAVXX/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 010. Mảng con có tổng bằng k](https://leetcode.cn/problems/QTMn0o/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 014. Từ dị vị trong chuỗi](https://leetcode.cn/problems/MPnaiL/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 015. Mọi từ dị vị trong chuỗi](https://leetcode.cn/problems/VabMRr/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 016. Chuỗi con dài nhất không ký tự trùng](https://leetcode.cn/problems/wtcaE1/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 017. Chuỗi ngắn nhất chứa mọi ký tự](https://leetcode.cn/problems/M1oyTv/?show=1) | 🔴 |
| - | [Kiếm chỉ Offer II 057. Hiệu giá trị và chỉ số đều trong phạm vi cho](https://leetcode.cn/problems/7WqeDu/?show=1) | 🟠 |

</details>
<hr>

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

![](https://labuladong.online/algo/images/souyisou2.png)
