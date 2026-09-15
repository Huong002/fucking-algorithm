# Tìm nhau trăm nghìn độ: bài toán người nổi tiếng



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [277. Find the Celebrity](https://leetcode.com/problems/find-the-celebrity/)🔒 | [277. Tìm người nổi tiếng](https://leetcode.cn/problems/find-the-celebrity/)🔒 | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
[Cơ bản cấu trúc đồ thị và implement chung](https://labuladong.online/algo/data-structure-basic/graph-basic/)

Hôm nay bàn bài kinh điển "vấn đề người nổi tiếng":

Cho bạn quan hệ xã hội của `n` người (bạn biết hai người tùy ý có quen nhau không), rồi hãy bạn tìm "người nổi tiếng" trong những người này.

Cái gọi là "người nổi tiếng" có hai điều kiện:

1, Mọi người khác đều quen "người nổi tiếng".

2, "người nổi tiếng" không quen bất kỳ ai khác.

Đây là một bài toán thuật toán liên quan đến đồ thị, quan hệ xã hội mà, về bản chất là trừu tượng thành một đồ thị.

Nếu coi mỗi người là một node trong đồ thị, quan hệ "quen" coi là cạnh có hướng giữa các node, vậy người nổi tiếng chính là một node đặc biệt trong đồ thị này:

![](https://labuladong.online/algo/images/celebrity/1.jpeg)

**Node này không có một cạnh có hướng nào trỏ tới node khác; mà mọi node khác đều có một cạnh trỏ tới node này**.

Hay nói chuyên nghiệp một chút, bậc ra của node người nổi tiếng bằng 0, bậc vào bằng `n - 1`.

Vậy, quan hệ xã hội của `n` người này biểu thị thế nào?

Bài trước [cơ bản thuật toán lý thuyết đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/) nói, đồ thị có hai dạng lưu, một là danh sách kề, một là ma trận kề, ưu thế chủ yếu của danh sách kề là tiết kiệm không gian lưu; ưu thế chủ yếu của ma trận kề là có thể nhanh chóng kiểm tra hai node có kề nhau không.

Với vấn đề người nổi tiếng, rõ ràng sẽ hay cần kiểm tra hai người có quen nhau không, chính là hai node có kề nhau không, nên ta có thể dùng ma trận kề để biểu thị quan hệ xã hội người với người.

Vậy, đem vấn đề người nổi tiếng mô tả thành dạng thuật toán chính là thế này:

Cho bạn input một mảng hai chiều cỡ `n x n` (ma trận kề) `graph` biểu thị một đồ thị có `n` node, mỗi người là một node trong đồ thị, đánh số `0` tới `n - 1`.

Nếu `graph[i][j] == 1` đại diện người thứ `i` quen người thứ `j`, nếu `graph[i][j] == 0` đại diện người thứ `i` không quen người thứ `j`.

Có đồ thị này biểu thị quan hệ người với người, hãy bạn tính xem, trong `n` người này, có tồn tại "người nổi tiếng" không?

Nếu tồn tại, thuật toán trả về số hiệu của người nổi tiếng này, nếu không tồn tại, thuật toán trả về -1.

Chữ ký hàm như sau:

```java
int findCelebrity(int[][] graph);
```

Ví dụ ma trận kề input trông thế này:

![](https://labuladong.online/algo/images/celebrity/2.jpeg)

Vậy thuật toán nên trả về 2.

LeetCode 277 "tìm người nổi tiếng" chính là vấn đề kinh điển này, nhưng không phải đem ma trận kề truyền thẳng cho bạn, mà chỉ cho bạn biết tổng số người `n`, đồng thời cung cấp một API `knows` để tra quan hệ xã hội người với người:

```java
// Có thể gọi thẳng, trả về i có quen j không
boolean knows(int i, int j);

// Hãy bạn implement: trả về số hiệu của "người nổi tiếng"
int findCelebrity(int n) {
    // todo
}
```

Rõ ràng, API `knows` về bản chất vẫn đang truy cập ma trận kề. Để đơn giản, về sau ta cứ theo dạng đề LeetCode để thảo luận vấn đề kinh điển này.

## Cách giải vét cạn

Ta nghĩ nhanh là viết ra một thuật toán đơn giản thô bạo:

```java
class Solution extends Relation {
    public int findCelebrity(int n) {
        for (int cand = 0; cand < n; cand++) {
            int other;
            for (other = 0; other < n; other++) {
                if (cand == other) continue;
                // Đảm bảo người khác đều quen cand, mà cand không quen ai khác
                // Nếu không candsẽ không thể là người nổi tiếng
                if (knows(cand, other) || !knows(other, cand)) {
                    break;
                }
            }
            if (other == n) {
                // Tìm thấy người nổi tiếng
                return cand;
            }
        }
        // Không một ai phù hợp đặc tính người nổi tiếng
        return -1;
    }
}
```

`cand` là viết tắt của ứng viên (candidate), thuật toán vét cạn của ta chính là liệt kê vét cạn từ đầu, coi mỗi người là ứng viên, kiểm tra có phù hợp điều kiện "người nổi tiếng" không.

Vừa rồi cũng nói, hàm `knows` ở tầng đáy chính là đang truy cập một ma trận kề hai chiều, một lần gọi độ phức tạp thời gian O(1), nên cách giải vét cạn có tổng độ phức tạp thời gian xấu nhất O(N^2).

Vậy, có cách cao minh khác tối ưu độ phức tạp thời gian không? Thực ra có không gian tối ưu, bạn nghĩ xem, chỗ tốn thời gian nhất của ta giờ ở đâu?

Với mỗi ứng viên `cand`, ta đều phải dùng một vòng for trong để kiểm tra `cand` này rốt cuộc có phù hợp điều kiện "người nổi tiếng" không.

Vòng for trong này trông thật ngốc, dù kiểm tra một người "là người nổi tiếng" bắt buộc phải dùng một vòng for, nhưng kiểm tra một người "không phải người nổi tiếng" thì không cần phiền vậy.

**Vì định nghĩa "người nổi tiếng" đảm bảo tính duy nhất của "người nổi tiếng", nên ta có thể lợi dụng phép loại trừ, trước hết loại những người rõ ràng không phải "người nổi tiếng", từ đó tránh lồng vòng for, giảm độ phức tạp thời gian**.






## Cách giải tối ưu

Tôi lặp lại một lần định nghĩa cái gọi là "người nổi tiếng":

1, Mọi người khác đều quen người nổi tiếng.

2, Người nổi tiếng không quen bất kỳ ai khác.

Định nghĩa này rất thú vị, nó đảm bảo trong đám đông có nhiều nhất một người nổi tiếng.

Rất dễ hiểu, nếu có hai người đồng thời là người nổi tiếng, vậy hai định nghĩa nàythì tự mâu thuẫn.

**Nói cách khác, chỉ cần quan sát quan hệ hai ứng viên tùy ý, tôi nhất định có thể xác định một người trong đó không phải người nổi tiếng, rồi loại nó**.

Còn ứng viên kia có phải người nổi tiếng không, chỉ xem quan hệ hai người chắc chắn không xác định được, nhưng điều này không quan trọng, quan trọng là loại được một ứng viên ắt không phải người nổi tiếng, thu hẹp vòng vây.

Đây là cốt lõi của tối ưu, cũng tương đối khó hiểu, nên ta trước hết nói vì sao quan sát quan hệ hai ứng viên tùy ý thì loại được một.

Bạn nghĩ xem, quan hệ giữa hai người có thể thế nào?

Chẳng qua bốn loại: bạn quen tôi mà tôi không quen bạn, tôi quen bạn mà bạn không quen tôi, hai ta quen nhau, hai ta không quen nhau.

Nếu ví người làm node, cạnh có hướng đỏ biểu thị không quen, cạnh có hướng xanh biểu thị quen, vậy quan hệ hai người chẳng qua bốn trường hợp sau:

![](https://labuladong.online/algo/images/celebrity/3.jpeg)

Không ngại coi số hiệu hai người này lần lượt là `cand` và `other`, rồi ta phân tích từng trường hợp một, xem loại một người thế nào.

Với trường hợp một, `cand` quen `other`, nên `cand` chắc chắn không phải người nổi tiếng, loại. Vì người nổi tiếng không thể quen người khác.

Với trường hợp hai, `other` quen `cand`, nên `other` chắc chắn không phải người nổi tiếng, loại.

Với trường hợp ba, hai người quen nhau, chắc chắn đều không phải người nổi tiếng, có thể tùy ý loại một.

Với trường hợp bốn, hai người không quen nhau, chắc chắn đều không phải người nổi tiếng, có thể tùy ý loại một. Vì người nổi tiếng hẳn được mọi người khác quen.

Tóm lại, chỉ cần quan sát quan hệ tùy ý của hai người, thì ít nhất có thể xác định một người không phải người nổi tiếng, kiểm tra tình huống trên có thể dùng code sau biểu thị:

```java
if (knows(cand, other) || !knows(other, cand)) {
    // cand không thể là người nổi tiếng
} else {
    // other không thể là người nổi tiếng
}
```

Nếu hiểu được đặc điểm này, vậy viết ra cách giải tối ưu thì đơn giản.

**Ta có thể không ngừng từ các ứng viên chọn hai ra, rồi loại một, tới cuối chỉ còn một ứng viên, lúc này lại dùng một vòng for để kiểm tra ứng viên này có phải "người nổi tiếng" hàng thật không**.

Ý tưởng này có code đầy đủ như sau:

```java
class Solution extends Relation {
    public int findCelebrity(int n) {
        if (n == 1) return 0;
        // Bỏ mọi ứng viên vào hàng đợi
        LinkedList<Integer> q = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            q.addLast(i);
        }
        // Loại mãi, tới khi chỉ còn một ứng viên thì dừng vòng
        while (q.size() >= 2) {
            // Mỗi lần lấy hai ứng viên, loại một
            int cand = q.removeFirst();
            int other = q.removeFirst();
            if (knows(cand, other) || !knows(other, cand)) {
                // cand không thể là người nổi tiếng, loại, để other về đội
                q.addFirst(other);
            } else {
                // other không thể là người nổi tiếng, loại, để cand về đội
                q.addFirst(cand);
            }
        }

        // Giờ loại chỉ còn một ứng viên, kiểm tra hắn có thật là người nổi tiếng không
        int cand = q.removeFirst();
        for (int other = 0; other < n; other++) {
            if (other == cand) {
                continue;
            }
            // Đảm bảo người khác đều quen cand, mà cand không quen ai khác
            if (!knows(other, cand) || knows(cand, other)) {
                return -1;
            }
        }
        // cand là người nổi tiếng
        return cand;
    }
}
```

Thuật toán này tránh lồng vòng for, độ phức tạp thời gian xuống O(N), nhưng phải đưa vào một hàng đợi để lưu tập ứng viên, dùng độ phức tạp không gian O(N).

> [!NOTE]
> Tác dụng của `LinkedList` chỉ là đóng vai container đựng ứng viên, mỗi lần tìm hai ra so và loại, còn về việc cụ thể tìm hai nào thì đều không quan trọng, nghĩa là thứ tự ứng viên về đội không quan trọng, ta dùng `addFirst` chỉ để tiện tối ưu sau, bạn hoàn toàn có thể dùng `addLast`, kết quả đều giống.

Có thể tối ưu thêm, đem độ phức tạp không gian cũng tối ưu bỏ không?

## Cách giải cuối

Nếu bạn hiểu cách giải tối ưu trên, thực ra có thể không cần không gian phụ mà giải vấn đề này, code như sau:

```java
class Solution extends Relation {
    public int findCelebrity(int n) {
        int cand = 0;
        for (int other = 1; other < n; other++) {
            if (!knows(other, cand) || knows(cand, other)) {
                // cand không thể là người nổi tiếng, loại bỏ
                // Giả sử other là người nổi tiếng
                cand = other;
            } else {
                // other không thể là người nổi tiếng, loại bỏ
                // Không cần làm gì, tiếp tục giả sử cand là người nổi tiếng
            }
        }

        // cand giờ là kết quả cuối cùng sau loại trừ, nhưng không đảm bảo nhất định là người nổi tiếng
        for (int other = 0; other < n; other++) {
            if (cand == other) continue;
            // Cần đảm bảo người khác đều quen cand, mà cand không quen ai khác
            if (!knows(other, cand) || knows(cand, other)) {
                return -1;
            }
        }

        return cand;
    }
}
```

Cách giải trước của ta dùng `LinkedList` đóng vai hàng đợi để lưu tập ứng viên, còn cách giải tối ưu này lợi dụng luân phiên hai biến `other` và `cand`, mô phỏng quá trình thao tác hàng đợi trước đó của ta, tránh dùng không gian lưu phụ.

Giờ đây, cách giải vấn đề người nổi tiếng có độ phức tạp thời gian O(N), không gian O(1), đã là cách giải tối ưu.







**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
