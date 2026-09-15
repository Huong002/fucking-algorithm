# Phép nhân chuỗi

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình nhanh thành](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu (Lực khấu) | Độ khó |
| :----: | :----: | :----: |
| [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/) | [43. Nhân chuỗi](https://leetcode.cn/problems/multiply-strings/) | 🟠 |

**-----------**

Với những con số tương đối nhỏ, làm phép tính có thể dùng trực tiếp toán tử mà ngôn ngữ lập trình cung cấp, nhưng nếu hai thừa số nhân nhau rất lớn, kiểu dữ liệu mà ngôn ngữ cung cấp có thể bị tràn số. Một phương án thay thế là: toán hạng được nhập dưới dạng chuỗi, sau đó mô phỏng quá trình tính nhẩm phép nhân như hồi tiểu học để tính ra kết quả, và kết quả cũng được biểu diễn bằng chuỗi.

Xem LeetCode 43 「Nhân chuỗi」:

<Problem slug="multiply-strings" />

Cần lưu ý, `num1` và `num2` có thể rất dài, nên không thể chuyển trực tiếp chúng thành số nguyên rồi tính toán, ý tưởng duy nhất là mô phỏng phép nhân tay của chúng ta.

Ví dụ ta tính tay `123 × 45`, sẽ tính như sau:

![](https://labuladong.online/algo/images/string-multiply/1.jpg)

Tính `123 × 5`, rồi tính `123 × 4`, cuối cùng cộng lệch một vị trí. Quy trình này e rằng học sinh tiểu học cũng có thể thành thạo, nhưng liệu bạn có thể **máy móc hóa (cơ giới hóa) thêm quá trình tính toán này**, viết thành một bộ chỉ dẫn thuật toán để máy tính không có chút trí tuệ nào cũng thực thi được không?

Nhìn quá trình đơn giản này, trong đó liên quan đến nhớ khi nhân, cộng lệch vị trí, còn liên quan đến nhớ khi cộng; hơn nữa còn có một số vấn đề khó nhận ra, ví dụ số có hai chữ số nhân với số có hai chữ số, kết quả có thể là bốn chữ số, cũng có thể là ba chữ số, bạn làm sao nghĩ ra một cách xử lý chuẩn hóa? Đó chính là sự quyến rũ của thuật toán, nếu không có tư duy máy tính, vấn đề đơn giản cũng có thể không có cách nào xử lý tự động được.

Trước hết, cách tính tay này của chúng ta vẫn còn quá 「cao cấp」, chúng ta phải 「sơ cấp」 hơn một chút, quá trình của `123 × 5` và `123 × 4` còn có thể phân rã thêm, cuối cùng cộng lại:

![](https://labuladong.online/algo/images/string-multiply/2.jpg)

Bây giờ `123` không lớn lắm, nếu là một con số rất lớn thì không thể tính trực tiếp tích được. Chúng ta có thể dùng một mảng ở phía dưới để nhận kết quả cộng dồn:

![](https://labuladong.online/algo/images/string-multiply/3.jpg)

Toàn bộ quá trình tính toán đại khái như sau, **có hai con trỏ `i, j` di chuyển trên `num1` và `num2`, tính tích, đồng thời cộng dồn tích vào vị trí đúng của `res`**, như hình GIF dưới đây:

![](https://labuladong.online/algo/images/string-multiply/4.gif)

Bây giờ còn một vấn đề then chốt, làm sao cộng dồn tích vào vị trí đúng của `res`, hay nói cách khác, làm sao tính chỉ số tương ứng của `res` thông qua `i, j`?

Thực ra, sau khi quan sát kỹ sẽ phát hiện, **tích của `num1[i]` và `num2[j]` tương ứng chính là hai vị trí `res[i+j]` và `res[i+j+1]` này**.

![](https://labuladong.online/algo/images/string-multiply/6.jpg)

Hiểu rõ điểm này, là có thể dùng code mô phỏng ra quá trình tính toán này:

```java
class Solution {
    public String multiply(String num1, String num2) {
        int m = num1.length(), n = num2.length();
        // Kết quả nhiều nhất có m + n chữ số
        int[] res = new int[m + n];
        // Nhân từng chữ số bắt đầu từ hàng đơn vị
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                // Vị trí chỉ số tương ứng của tích trong res
                int p1 = i + j, p2 = i + j + 1;
                // Cộng dồn vào res
                int sum = mul + res[p2];
                res[p2] = sum % 10;
                res[p1] += sum / 10;
            }
        }
        // Tiền tố kết quả có thể lưu số 0 (các bit chưa dùng)
        int i = 0;
        while (i < res.length && res[i] == 0)
            i++;
        // Chuyển kết quả tính toán thành chuỗi
        StringBuilder str = new StringBuilder();
        for (; i < res.length; i++)
            str.append(res[i]);

        return str.length() == 0 ? "0" : str.toString();
    }
}
```

Đến đây, thuật toán nhân chuỗi đã hoàn thành.

**Tổng kết lại**, một số cách tư duy mà chúng ta quen thuộc đến mức coi là đương nhiên, trong mắt máy tính lại rất khó làm được. Ví dụ quy trình số học mà chúng ta quen thuộc không phức tạp, nhưng nếu bắt bạn tiến thêm một bước, dịch thành logic code, không đơn giản. Thuật toán cần đơn giản hóa thêm quy trình tính toán, thông qua cách vừa tính vừa cộng dồn để ra kết quả.

Tục ngữ dạy chúng ta, đừng rơi vào lối mòn tư duy, đừng chương trình hóa, phải tư duy mở rộng, phải sáng tạo. Nhưng tôi thấy chương trình hóa cũng không phải chuyện xấu, có thể đáng kể nâng cao hiệu suất, giảm tỷ lệ sai sót. Thuật toán chẳng phải là một bộ tư duy được chương trình hóa sao, chỉ có chương trình hóa mới để máy tính giúp chúng ta giải quyết vấn đề phức tạp!

Có lẽ thuật toán chính là một loại **tư duy đi tìm lối mòn tư duy**, hy vọng bài viết này có ích với bạn.

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

![](https://labuladong.online/algo/images/souyisou2.png)
