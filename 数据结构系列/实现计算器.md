# Mở rộng: Cài đặt một máy tính thế nào




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [224. Basic Calculator](https://leetcode.com/problems/basic-calculator/) | [224. Máy tính cơ bản](https://leetcode.cn/problems/basic-calculator/) | 🔴 |
| [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | [227. Máy tính cơ bản II](https://leetcode.cn/problems/basic-calculator-ii/) | 🟠 |
| [772. Basic Calculator III](https://leetcode.com/problems/basic-calculator-iii/)🔒 | [772. Máy tính cơ bản III](https://leetcode.cn/problems/basic-calculator-iii/)🔒 | 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Nguyên lý hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/queue-stack-basic/)

Chức năng máy tính chúng ta cuối cùng cần cài đặt như sau:

1, Nhập một chuỗi, có thể chứa `+ - * /`, số, ngoặc và khoảng trắng, thuật toán của bạn trả về kết quả tính.

2, Phải phù hợp quy tắc tính, ngoặc ưu tiên cao nhất, nhân chia trước cộng trừ sau.

3, Phép chia là chia nguyên, dù dương âm đều lấy chỉnh về 0 (5/2=2, -5/2=-2).

4, Có thể giả định biểu thức nhập chắc chắn hợp lệ, và quá trình tính không xuất hiện tràn số nguyên, không xuất hiện tình huống ngoài ý muốn chia cho 0.

Ví dụ nhập chuỗi sau, thuật toán sẽ trả về 9:

```java
  3 * (2 - 6 / (3 - 7))
= 3 * (2 - 6 / (-4))
= 3 * (2 - (-1))
= 9
```

Thấy đấy, đây đã rất gần máy tính dùng trong đời thực của chúng ta, dù trước đây chắc chắn đều dùng máy tính, nhưng nếu nghĩ đơn giản về cài đặt thuật toán của nó, sẽ lớn giật mình thất sắc :

1, Theo lẽ thường xử lý ngoặc, phải tính ngoặc trong cùng trước, rồi chậm chậm hóa giản ra ngoài. Quá trình này tay tính còn dễ sai, huống gì viết thành thuật toán!

2, Phải làm được nhân chia trước, cộng trừ sau, dạy trẻ con điểm này không tính khó, nhưng dạy máy tính e rằng hơi khó.

3, Phải xử lý khoảng trắng. Vì đẹp, chúng ta quen đánh khoảng trắng giữa số và toán tử, nhưng trong tính toán phải nghĩ cách bỏ qua khoảng trắng này.

Tôi nhớ nhiều giáo trình cấu trúc dữ liệu đại học, khi giảng stack, chắc đều lấy máy tính ví dụ, nhưng có một nói một, giảng thực sự rác, không biết bao nhiêu nhà khoa học máy tính tương lai thì được cấu trúc dữ liệu đơn giản này khuyên rút lui .

Vậy bài này trò chuyện làm sao cài đặt chức năng máy tính đầy đủ trên, **mấu chốt là từng lớp tháo gỡ vấn đề, chia nhỏ, đánh tan từng phần**, vài quy tắc thuật toán đơn giản là có thể xử lý tính toán cực phức tạp, tin rằng cách tư duy này giúp mọi người giải các vấn đề phức tạp.

Dưới đây tháo gỡ, bắt đầu từ một vấn đề đơn giản nhất.

## Một, chuỗi chuyển số nguyên

Đúng, chính là một vấn đề đơn giản vậy, trước hết nói tôi, chuyển một số nguyên **dương** dạng chuỗi thành kiểu int thế nào?

```java
String s = "458";

int n = 0;
for (int i = 0; i < s.length(); i++) {
    char c = s.charAt(i);
    n = 10 * n + (c - '0');
}
// n giờ thì bằng 458
```

Vẫn rất đơn giản đúng không, khuôn mẫu cũ. Nhưng dù đơn giản vậy, vẫn có bẫy : **ngoặc của `(c - '0')` không được lược bớt, nếu không có thể gây tràn số nguyên**.

Vì biến `c` là mã ASCII, nếu không cộng ngoặc sẽ cộng trước trừ sau, tưởng tượng `s` nếu gần INT_MAX, sẽ tràn. Nên dùng ngoặc đảm bảo trừ trước cộng sau mới được.

## Hai, xử lý cộng trừ

Giờ tiến thêm, **nếu biểu thức nhập chỉ chứa cộng trừ, mà không tồn tại khoảng trắng**, bạn tính kết quả thế nào? Lấy biểu thức chuỗi `1-12+3` ví dụ, nói một ý tưởng rất đơn giản:

1, Thêm cho số đầu một ký hiệu mặc định `+`, thành `+1-12+3`.

2, Ghép một toán tử và số thành một cặp, tức ba cặp `+1`, `-12`, `+3`, chuyển chúng thành số, rồi đặt vào một stack.

3, Cộng mọi số trong stack, chính là kết quả biểu thức gốc.

Chúng ta xem thẳng code, kết hợp một hình là hiểu:

```java
int calculate(String s) {
    Stack<Integer> stk = new Stack<>();
    // Ghi lại số trong biểu thức 
    int num = 0;
    // Ghi lại ký hiệu trước num, khởi tạo là +
    char sign = '+';
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        // Nếu là số, đọc liên tục vào num
        if (Character.isDigit(c)) {
            num = 10 * num + (c - '0');
        }
        // Nếu không phải số, chính là gặp ký hiệu tiếp theo, hoặc là cuối của biểu thức 
        // Vậy số và ký hiệu trước đó thì cần lưu vào stack
        if (c == '+' || c == '-' || i == s.length() - 1) {
            switch (sign) {
                case '+':
                    stk.push(num); break;
                case '-':
                    stk.push(-num); break;
            }
            // Cập nhật ký hiệu thành ký hiệu hiện tại, số đặt về 0 
            sign = c;
            num = 0;
        }
    }
    // Cộng mọi kết quả trong stack chính là đáp án
    int res = 0;
    while (!stk.isEmpty()) {
        res += stk.pop();
    }
    return res;
}
```

Tôi đoán chính là phần mang câu `switch` ở giữa hơi khó hiểu, `i` chính là quét từ trái sang phải, `sign` và `num` theo sau nó. Khi `s[i]` gặp một toán tử, tình huống như sau:

![](https://labuladong.online/algo/images/calculator/1.jpg)

Cho nên, lúc này cần dựa vào case khác nhau của `sign` chọn dương âm của `nums`, lưu vào stack, rồi cập nhật `sign` và đặt về 0 `nums` ghi cặp phù hợp và số tiếp theo.

Ngoài ra chú ý, không chỉ gặp ký hiệu mới kích hoạt vào stack, khi `i` đi tới cuối của biểu thức (`i == s.size() - 1`), cũng phải đẩy số phía trước vào stack, tiện tính kết quả cuối sau đó.

![](https://labuladong.online/algo/images/calculator/2.jpg)

Đến đây, thuật toán chỉ xử lý chuỗi cộng trừ gọn hoàn thành, hãy đảm bảo hiểu nội dung trên, nội dung sau thì dựa trên khung này sửa sửa là xong.

## Ba, xử lý nhân chia

Thực ra ý tưởng không khác gì chỉ xử lý cộng trừ, lấy chuỗi `2-3*4+5` ví dụ, ý tưởng cốt lõi vẫn là tách chuỗi thành tổ hợp ký hiệu và số.

Ví dụ trên có thể tách thành mấy cặp `+2`, `-3`, `*4`, `+5`, vừa rồi không phải chưa xử lý nhân chia sao, rất đơn giản, **phần khác đều không cần đổi**, thêm case tương ứng ở phần `switch` là được:

```java
int calculate(String s) {
    Stack<Integer> stk = new Stack<>();
    // Ghi lại số trong biểu thức 
    int num = 0;
    // Ghi lại ký hiệu trước num, khởi tạo là +
    char sign = '+';
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (Character.isDigit(c)) {
            num = 10 * num + (c - '0');
        }

        if (c == '+' || c == '-' || c == '/' || c == '*' || i == s.length() - 1) {
            int pre;
            switch (sign) {
                case '+':
                    stk.push(num); break;
                case '-':
                    stk.push(-num); break;
                // Chỉ cần lấy số trước đó làm tính tương ứng là được
                case '*':
                    pre = stk.pop();
                    stk.push(pre * num);
                    break;
                case '/':
                    pre = stk.pop();
                    stk.push(pre / num);
                    break;
            }
            // Cập nhật ký hiệu thành ký hiệu hiện tại, số đặt về 0 
            sign = c;
            num = 0;
        }
    }
    // Cộng mọi kết quả trong stack chính là đáp án
    int res = 0;
    while (!stk.isEmpty()) {
        res += stk.pop();
    }
    return res;
}
```

![](https://labuladong.online/algo/images/calculator/3.jpg)



**Nhân chia ưu tiên hơn cộng trừ thể hiện ở, nhân chia có thể kết hợp với số đỉnh stack, mà cộng trừ chỉ đặt vào mình vào stack**.

Giờ chúng ta nghĩ xem xử lý ký tự khoảng trắng có thể xuất hiện trong chuỗi thế nào. Thực ra theo code hiện tại, chúng ta căn bản không cần xử lý đặc biệt ký tự khoảng trắng, bạn chú ý điều kiện if, khi ký tự `c` là khoảng trắng, không làm xử lý gì với nó, bỏ qua thẳng.

Tốt rồi, thuật toán giờ đã có thể tính cộng trừ nhân chia theo đúng quy tắc, và tự động bỏ ký tự khoảng trắng, còn lại là làm sao để thuật toán nhận đúng ngoặc.

## Bốn, xử lý ngoặc

Xử lý ngoặc trong biểu thức nhìn phải khó nhất, nhưng thực không khó như nhìn. Chúng ta sửa chút code trên trước:

```java
int calculate(String s) {
    return _calculate(s, 0, s.length() - 1);
}

// Định nghĩa: trả về kết quả tính của biểu thức trong s[start..end]
int _calculate(String s, int start, int end) {
    // Cần chuyển chuỗi thành hàng đợi hai đầu tiện thao tác
    Stack<Integer> stk = new Stack<>();
    // Ghi lại số trong biểu thức 
    int num = 0;
    // Ghi lại ký hiệu trước num, khởi tạo là +
    char sign = '+';
    for (int i = start; i <= end; i++) {
        char c = s.charAt(i);
        if (Character.isDigit(c)) {
            num = 10 * num + (c - '0');
        }

        if (c == '+' || c == '-' || c == '/' || c == '*' || i == s.length() - 1) {
            int pre;
            switch (sign) {
                case '+':
                    stk.push(num);
                    break;
                case '-':
                    stk.push(-num);
                    break;
                // Chỉ cần lấy số trước đó làm tính tương ứng là được
                case '*':
                    pre = stk.pop();
                    stk.push(pre * num);
                    break;
                case '/':
                    pre = stk.pop();
                    stk.push(pre / num);
                    break;
            }
            // Cập nhật ký hiệu thành ký hiệu hiện tại, số đặt về 0 
            sign = c;
            num = 0;
        }
    }
    // Cộng mọi kết quả trong stack chính là đáp án
    int res = 0;
    while (!stk.isEmpty()) {
        res += stk.pop();
    }
    return res;
}
```

Ở đây chúng ta định nghĩa một hàm mới `_calculate`, nó nhận ba tham số, lần lượt là chuỗi `s`, và biên trái/phải `start` và `end` của chuỗi. Như vậy chúng ta có thể tính giá trị của con biểu thức bất kỳ trong `s`.

Vậy, tại sao nói xử lý ngoặc không khó như nhìn? **Vì ngoặc có tính đệ quy**. Lấy chuỗi `3*(4-5/2)-6` ví dụ:

```java
calculate(3 * (4 - 5/2) - 6)
= 3 * calculate(4 - 5/2) - 6
= 3 * 2 - 6
= 0
```

Có thể tự hình dung, dù bao nhiêu tầng ngoặc lồng nhau, qua hàm `_calculate` đệ quy gọi mình, đều có thể tính biểu thức trong ngoặc ra kết quả. **Nói cách khác, biểu thức ngoặc chứa, chúng ta xem thẳng thành một số là được**.

Vậy giờ vấn đề là, nếu tôi gặp một ngoặc trái `(`, làm sao biết ngoặc phải `)` tương ứng ở đâu? Lại cần dùng stack, chúng ta có thể dự tính với `s`, tìm trước vị trí ngoặc phải tương ứng mỗi ngoặc trái.

Xem cụ thể code, dựa trên hàm `_calculate` trên, thêm ít logic:

```java
class Solution {
    public int calculate(String s) {
        // key là index ngoặc trái, value là index ngoặc phải tương ứng
        Map<Integer, Integer> rightIndex = new HashMap<>();
        // Lợi dụng cấu trúc stack để tìm ngoặc tương ứng
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else if (s.charAt(i) == ')') {
                rightIndex.put(stack.pop(), i);
            }
        }
        return _calculate(s, 0, s.length() - 1, rightIndex);
    }

    // Định nghĩa: trả về kết quả tính của biểu thức trong s[start..end]
    private int _calculate(String s, int start, int end, Map<Integer, Integer> rightIndex) {
        // Cần chuyển chuỗi thành hàng đợi hai đầu tiện thao tác
        Stack<Integer> stk = new Stack<>();
        // Ghi lại số trong biểu thức 
        int num = 0;
        // Ghi lại ký hiệu trước num, khởi tạo là +
        char sign = '+';
        for (int i = start; i <= end; i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                num = 10 * num + (c - '0');
            }
            if (c == '(') {
                // Đệ quy tính biểu thức trong ngoặc
                num = _calculate(s, i + 1, rightIndex.get(i) - 1, rightIndex);
                i = rightIndex.get(i);
            }
            if (c == '+' || c == '-' || c == '*' || c == '/' || i == end) {
                int pre;
                switch (sign) {
                    case '+':
                        stk.push(num);
                        break;
                    case '-':
                        stk.push(-num);
                        break;
                    // Chỉ cần lấy số trước đó làm tính tương ứng là được
                    case '*':
                        pre = stk.pop();
                        stk.push(pre * num);
                        break;
                    case '/':
                        pre = stk.pop();
                        stk.push(pre / num);
                        break;
                }
                // Cập nhật ký hiệu thành ký hiệu hiện tại, số đặt về 0 
                sign = c;
                num = 0;
            }
        }
        // Cộng mọi kết quả trong stack chính là đáp án
        int res = 0;
        while (!stk.isEmpty()) {
            res += stk.pop();
        }
        return res;
    }
}
```

![](https://labuladong.online/algo/images/calculator/4.jpg)

![](https://labuladong.online/algo/images/calculator/5.jpg)

![](https://labuladong.online/algo/images/calculator/6.jpg)



Bạn xem, thêm hai ba dòng code, là có thể xử lý ngoặc, đây chính là sức hấp dẫn của đệ quy. Đến đây, toàn bộ chức năng máy tính cài đặt xong, qua từng lớp tháo gỡ vấn đề chia nhỏ, quay đầu xem, vấn đề này dường như cũng không phức tạp .

## Năm, tổng kết cuối

Bài này mượn vấn đề cài đặt máy tính, chủ yếu muốn diễn đạt một ý tưởng xử lý vấn đề phức tạp.

Chúng ta trước hết từ vấn đề đơn giản chuyển số từ chuỗi bắt đầu, rồi xử lý biểu thức chỉ chứa cộng trừ, rồi xử lý biểu thức chứa bốn phép cộng trừ nhân chia, rồi xử lý ký tự khoảng trắng, rồi xử lý biểu thức chứa ngoặc.

Thấy được, với vài vấn đề tương đối khó, lời giải của nó không phải một bước mà thì, mà là từng bước suy ra vào ốc tăng dần. Nếu ban đầu cho bạn đề gốc, bạn không biết làm, thậm chí không hiểu đáp án, đều rất bình thường, mấu chốt là chúng ta tự đơn giản hóa vấn đề thế nào, lấy lùi để tiến thế nào.

Hiểu rõ nguyên lý thuật toán máy tính sau, **code máy tính toàn năng cài đặt cuối cùng này có thể lưu lại**, vài bài thuật toán khác có thể yêu cầu bạn tính giá trị biểu thức, đến lúc đó có thể áp dụng lớp này ra dùng thẳng, không cần tự viết từ đầu.




<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [ khuôn mẫu 「lừa điểm」 bài kiểm tra thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
