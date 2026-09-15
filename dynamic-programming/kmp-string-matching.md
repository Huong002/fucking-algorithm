# Quy hoạch động và thuật toán khớp ký tự KMP

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho người dùng cũ~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được mô-típ thuật toán mà còn tiện thể giải được các đề sau:

| LeetCode | Lực khấu (LeetCode Trung Quốc) | Độ khó |
| :----: | :----: | :----: |
| [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | [28. Tìm chỉ số của vị trí khớp đầu tiên trong chuỗi](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/) | 🟠 |

**-----------**

::: tip

Trước khi đọc bài này, gợi ý bạn học trước một thuật toán khớp chuỗi khác: [Thuật toán khớp ký tự Rabin Karp](https://labuladong.online/algo/practice-in-action/rabinkarp/).

:::

Thuật toán KMP (thuật toán Knuth-Morris-Pratt) là một thuật toán khớp chuỗi nổi tiếng, hiệu quả rất cao nhưng quả thật hơi phức tạp.

Nhiều bạn đọc than thuật toán KMP không hiểu nổi, điều này rất bình thường, nghĩ tới cách giảng thuật toán KMP trong giáo trình đại học, không biết đã có bao nhiêu Knuth, Morris, Pratt tương lai bị dọa bỏ cuộc từ sớm. Có một số bạn giỏi tự tay suy diễn từng bước quá trình KMP để hỗ trợ hiểu thuật toán, đây cũng là một cách, nhưng bài này sẽ giúp bạn đọc hiểu nguyên lý thuật toán về mặt logic. Chỉ trong mười dòng code, KMP tan thành mây khói.

**Quy ước trước ở đầu bài, bài này dùng `pat` biểu thị chuỗi mẫu, độ dài là `M`, `txt` biểu thị chuỗi văn bản, độ dài là `N`. Thuật toán KMP là tìm chuỗi con `pat` trong `txt`, nếu tồn tại thì trả về chỉ số bắt đầu của chuỗi con này, nếu không thì trả về -1**.

Vì sao mình cho rằng thuật toán KMP chính là một bài toán quy hoạch động, lát nữa sẽ giải thích. Với quy hoạch động, trước đây đã nhấn mạnh nhiều lần phải xác định rõ ý nghĩa của mảng `dp`, mà cùng một bài toán có thể có không chỉ một cách định nghĩa ý nghĩa mảng `dp`, định nghĩa khác nhau sẽ có cách giải khác nhau.

Thuật toán KMP mà bạn đọc từng thấy hẳn là: một loạt thao tác kỳ quái xử lý `pat` rồi tạo thành một mảng một chiều `next`, rồi dựa vào mảng này lại qua một loạt thao tác phức tạp để khớp `txt`. Độ phức tạp thời gian O(N), độ phức tạp không gian O(M). Thật ra mảng `next` này tương đương mảng `dp`, trong đó ý nghĩa các phần tử liên quan tới tiền tố và hậu tố của `pat`, quy tắc phán đoán phức tạp, khó hiểu. **Còn bài này dùng một mảng `dp` hai chiều (nhưng độ phức tạp không gian vẫn là O(M)), định nghĩa lại ý nghĩa các phần tử trong đó, khiến độ dài code giảm mạnh, tính dễ hiểu tăng mạnh**.

::: note

Code bài này tham khảo《Thuật toán 4》, code gốc trong sách dùng tên mảng là `dfa` (máy trạng thái hữu hạn xác định), vì tài khoản công chúng của bọn mình trước đây có cả loạt bài quy hoạch động nên không nói từ ngữ đao to búa lớn này nữa, mình đã sửa code trong sách một chút và dùng tiếp tên mảng `dp`.

:::

### Một, Tổng quan thuật toán KMP

Trước hết vẫn giới thiệu đơn giản thuật toán KMP và thuật toán khớp vét cạn khác nhau ở đâu, điểm khó ở đâu, và liên quan gì tới quy hoạch động.

Bài 28「Hiện thực strStr」trên LeetCode chính là bài toán khớp chuỗi, thuật toán khớp chuỗi vét cạn rất dễ viết, xem logic chạy của nó:

<!-- muliti_language -->
```java
// Khớp vét cạn (mã giả)
int search(String pat, String txt) {
    int M = pat.length;
    int N = txt.length;
    for (int i = 0; i <= N - M; i++) {
        int j;
        for (j = 0; j < M; j++) {
            if (pat[j] != txt[i+j])
                break;
        }
        // pat khớp hết rồi
        if (j == M) return i;
    }
    // trong txt không tồn tại chuỗi con pat
    return -1;
}
```

Với thuật toán vét cạn, nếu xuất hiện ký tự không khớp thì đồng thời lùi con trỏ `txt` và `pat`, vòng for lồng nhau, độ phức tạp thời gian `O(MN)`, độ phức tạp không gian`O(1)`. Vấn đề lớn nhất là, nếu trong chuỗi có nhiều ký tự lặp lại thì thuật toán này trông rất ngốc.

Ví dụ `txt = "aaacaaab", pat = "aaab"`:

![](https://labuladong.online/algo/images/kmp/1.gif)

Rất rõ ràng, trong `pat` vốn không có ký tự c, hoàn toàn không cần lùi con trỏ `i`, cách giải vét cạn rõ ràng đã làm nhiều thao tác không cần thiết.

Điểm khác của thuật toán KMP là, nó sẽ tốn không gian để ghi lại một số thông tin, trong tình huống trên sẽ trông rất khôn:

![](https://labuladong.online/algo/images/kmp/2.gif)

Lấy thêm ví dụ tương tự `txt = "aaaaaaab", pat = "aaab"`, cách giải vét cạn vẫn sẽ ngốc nghếch lùi con trỏ `i` như ví dụ trên, mà thuật toán KMP lại biết khôn lỏi:

![](https://labuladong.online/algo/images/kmp/3.gif)

Vì thuật toán KMP biết các ký tự a trước ký tự b đều đã khớp, nên mỗi lần chỉ cần so ký tự b có được khớp không là được.

**Thuật toán KMP không bao giờ lùi con trỏ `i` của `txt`, không đi đường vòng (không quét lặp `txt`), mà nhờ thông tin lưu trong mảng `dp` để đưa `pat` tới vị trí đúng đắn rồi khớp tiếp**, độ phức tạp thời gian chỉ cần O(N), lấy không gian đổi thời gian, nên mình cho rằng nó là một thuật toán quy hoạch động.

Điểm khó của thuật toán KMP là, tính thông tin trong mảng `dp` thế nào? Dựa vào các thông tin này di chuyển đúng đắn con trỏ `pat` thế nào? Việc này cần **máy trạng thái hữu hạn xác định** hỗ trợ, đừng sợ thuật ngữ văn chương đao to búa lớn này, thật ra nó giống hệt mảng `dp` của quy hoạch động, đợi bạn học xong cũng có thể lấy từ này đi dọa người khác.

Còn một điểm cần xác định rõ: **tính mảng `dp` này chỉ liên quan tới chuỗi `pat`**. Ý là, chỉ cần cho mình một `pat`, mình tính được mảng `dp` qua chuỗi mẫu này, rồi bạn cho mình `txt` khác nhau thế nào mình cũng không sợ, dùng mảng `dp` này mình đều hoàn thành khớp chuỗi trong thời gian O(N).

Cụ thể, ví dụ hai ví dụ nêu trên:

```python
txt1 = "aaacaaab" 
pat = "aaab"
txt2 = "aaaaaaab" 
pat = "aaab"
```

`txt` của ta khác nhau, nhưng `pat` giống nhau, nên mảng `dp` mà thuật toán KMP dùng là cùng một mảng.

Chỉ là với tình huống sắp xuất hiện không khớp dưới đây của `txt1`:

![](https://labuladong.online/algo/images/kmp/txt1.jpg)

Mảng `dp` chỉ thị `pat` di chuyển thế này:

![](https://labuladong.online/algo/images/kmp/txt2.jpg)

::: note

Đừng hiểu `j` này là chỉ số, ý nghĩa của nó nói chính xác hơn hẳn là **trạng thái** (state), nên nó mới xuất hiện ở vị trí kỳ lạ này, phần sau sẽ nói kỹ.

:::

Còn với tình huống sắp xuất hiện không khớp dưới đây của `txt2`:

![](https://labuladong.online/algo/images/kmp/txt3.jpg)

Mảng `dp` chỉ thị `pat` di chuyển thế này:

![](https://labuladong.online/algo/images/kmp/txt4.jpg)

Hiểu rõ mảng `dp` chỉ liên quan tới `pat` rồi, vậy ta thiết kế thuật toán KMP thế này sẽ khá đẹp:

<!-- muliti_language -->
```java
public class KMP {
    private int[][] dp;
    private String pat;

    public KMP(String pat) {
        this.pat = pat;
        // Xây dựng mảng dp qua pat
        // Cần thời gian O(M)
    }

    public int search(String txt) {
        // Khớp txt nhờ mảng dp
        // Cần thời gian O(N)
    }
}
```

Như vậy, khi ta cần dùng cùng một `pat` để khớp các `txt` khác nhau thì không cần tốn thời gian xây dựng mảng `dp` nữa:

```java
KMP kmp = new KMP("aaab");
int pos1 = kmp.search("aaacaaab"); //4
int pos2 = kmp.search("aaaaaaab"); //4
```

### Hai, Tổng quan máy trạng thái

Vì sao nói thuật toán KMP liên quan tới máy trạng thái? Là thế này, ta có thể coi việc khớp `pat` chính là chuyển trạng thái. Ví dụ khi pat = "ABABC":

![](https://labuladong.online/algo/images/kmp/state.jpg)

Như hình trên, số trong vòng tròn chính là trạng thái, trạng thái 0 là trạng thái bắt đầu, trạng thái 5 (`pat.length`) là trạng thái kết thúc. Khi bắt đầu khớp thì `pat` ở trạng thái bắt đầu, một khi chuyển tới trạng thái kết thúc thì tức là đã tìm thấy `pat` trong `txt`. Ví dụ nói hiện tại ở trạng thái 2, tức là chuỗi "AB" đã được khớp:

![](https://labuladong.online/algo/images/kmp/state2.jpg)

Ngoài ra, ở trạng thái khác nhau thì hành vi chuyển trạng thái của `pat` cũng khác. Ví dụ giả sử giờ đã khớp tới trạng thái 4, nếu gặp ký tự A thì nên chuyển sang trạng thái 3, gặp ký tự C thì nên chuyển sang trạng thái 5, gặp ký tự B thì nên chuyển sang trạng thái 0:

![](https://labuladong.online/algo/images/kmp/state4.jpg)

Cụ thể là ý gì, ta xem từng ví dụ. Dùng biến `j` biểu thị con trỏ đang trỏ trạng thái hiện tại, hiện tại `pat` đã khớp tới trạng thái 4:

![](https://labuladong.online/algo/images/kmp/exp1.jpg)

Nếu gặp ký tự "A", theo mũi tên chỉ, chuyển sang trạng thái 3 là khôn nhất:

![](https://labuladong.online/algo/images/kmp/exp3.jpg)

Nếu gặp ký tự "B", theo mũi tên chỉ, chỉ có thể chuyển sang trạng thái 0 (một đêm quay về vạch xuất phát):

![](https://labuladong.online/algo/images/kmp/exp5.jpg)

Nếu gặp ký tự "C", theo mũi tên chỉ, nên chuyển sang trạng thái kết thúc 5, điều này cũng có nghĩa là khớp xong:

![](https://labuladong.online/algo/images/kmp/exp7.jpg)

Đương nhiên, còn có thể gặp ký tự khác, ví dụ Z, nhưng hiển nhiên nên chuyển sang trạng thái bắt đầu 0, vì trong `pat` vốn không có ký tự Z:

![](https://labuladong.online/algo/images/kmp/z.jpg)

Ở đây để cho rõ ràng, khi vẽ đồ thị trạng thái ta lược bỏ mũi tên chuyển các ký tự khác về trạng thái 0, chỉ vẽ chuyển trạng thái của các ký tự xuất hiện trong `pat`:

![](https://labuladong.online/algo/images/kmp/allstate.jpg)

Bước then chốt nhất của thuật toán KMP chính là xây dựng đồ thị chuyển trạng thái này. **Muốn xác định hành vi chuyển trạng thái, phải làm rõ hai biến, một là trạng thái khớp hiện tại, hai là ký tự gặp phải**; xác định hai biến này rồi là biết trong tình huống này nên chuyển sang trạng thái nào.

Dưới đây xem quá trình thuật toán KMP khớp chuỗi `txt` theo đồ thị chuyển trạng thái này:

![](https://labuladong.online/algo/images/kmp/kmp.gif)

**Hãy nhớ quá trình khớp trong GIF này, đây chính là logic cốt lõi của thuật toán KMP**!

Để mô tả đồ thị chuyển trạng thái, ta định nghĩa một mảng dp hai chiều, ý nghĩa của nó như sau:

```python
dp[j][c] = next
0 <= j < M, biểu thị trạng thái hiện tại
0 <= c < 256, biểu thị ký tự gặp phải (mã ASCII)
0 <= next <= M, biểu thị trạng thái tiếp theo

dp[4]['A'] = 3 nghĩa là:
hiện tại là trạng thái 4, nếu gặp ký tự A,
pat nên chuyển sang trạng thái 3

dp[1]['B'] = 2 nghĩa là:
hiện tại là trạng thái 1, nếu gặp ký tự B,
pat nên chuyển sang trạng thái 2
```

Dựa vào định nghĩa mảng dp này và quá trình chuyển trạng thái vừa rồi, ta có thể viết trước code hàm search của thuật toán KMP:

<!-- muliti_language -->
```java
public int search(String txt) {
    int M = pat.length();
    int N = txt.length();
    // Trạng thái khởi đầu của pat là 0
    int j = 0;
    for (int i = 0; i < N; i++) {
        // Hiện tại là trạng thái j, gặp ký tự txt[i],
        // pat nên chuyển sang trạng thái nào?
        j = dp[j][txt.charAt(i)];
        // Nếu tới trạng thái kết thúc, trả về chỉ số bắt đầu của vị trí khớp
        if (j == M) return i - M + 1;
    }
    // Chưa tới trạng thái kết thúc, khớp thất bại
    return -1;
}
```

Tới đây hẳn vẫn dễ hiểu, mảng `dp` chính là đồ thị chuyển trạng thái ta vừa vẽ, nếu chưa rõ thì quay lại xem quá trình diễn tiến thuật toán trong GIF. Dưới đây sẽ giảng: làm sao xây dựng mảng `dp` này qua `pat`?

### Ba, Xây dựng đồ thị chuyển trạng thái

Nhớ lại vừa nói: **muốn xác định hành vi chuyển trạng thái, phải làm rõ hai biến, một là trạng thái khớp hiện tại, hai là ký tự gặp phải**, mà ta đã xác định ý nghĩa mảng `dp` theo logic này, vậy khung xây dựng mảng `dp` là thế này:

```python
for 0 <= j < M: # trạng thái
    for 0 <= c < 256: # ký tự
        dp[j][c] = next
```

Trạng thái next này tính thế nào? Hiển nhiên, **nếu ký tự `c` gặp phải khớp với `pat[j]`**, trạng thái nên tiến lên một, tức là `next = j + 1`, ta gọi tình huống này là **tiến trạng thái**:

![](https://labuladong.online/algo/images/kmp/forward.jpg)

**Nếu ký tự `c` và `pat[j]` không khớp**, trạng thái sẽ lùi (hoặc đứng yên), ta gọi tình huống này là **khởi động lại trạng thái**:

![](https://labuladong.online/algo/images/kmp/back.jpg)

Vậy làm sao biết nên khởi động lại ở trạng thái nào? Trước khi trả lời câu này, ta định nghĩa thêm một cái tên: **trạng thái bóng** (tên do mình đặt), dùng biến `X` biểu thị. **Cái gọi là trạng thái bóng, chính là có cùng tiền tố với trạng thái hiện tại**. Ví dụ tình huống dưới đây:

![](https://labuladong.online/algo/images/kmp/shadow.jpg)

Trạng thái hiện tại `j = 4`, trạng thái bóng của nó là `X = 2`, chúng đều có cùng tiền tố "AB". Vì trạng thái `X` và trạng thái `j` có tiền tố giống nhau, nên khi trạng thái `j` chuẩn bị khởi động lại trạng thái (ký tự `c` gặp phải không khớp với `pat[j]`), có thể lấy **vị trí khởi động lại gần nhất** qua đồ thị chuyển trạng thái của `X`.

Ví dụ tình huống vừa rồi, nếu trạng thái `j` gặp một ký tự "A" thì nên chuyển đi đâu? Trước hết chỉ khi gặp "C" mới tiến trạng thái được, gặp "A" hiển nhiên chỉ có thể khởi động lại trạng thái. **Trạng thái `j` sẽ ủy thác ký tự này cho trạng thái `X` xử lý, tức là `dp[j]['A'] = dp[X]['A']`**:

![](https://labuladong.online/algo/images/kmp/shadow1.jpg)

Vì sao làm vậy được? Vì: đã xác định phía `j` ký tự "A" không tiến trạng thái được, **chỉ có thể lùi**, mà KMP là phải **lùi ít nhất có thể** để tránh tính toán thừa. Vậy `j` có thể đi hỏi `X` có cùng tiền tố với mình, nếu `X` gặp "A" mà tiến trạng thái được thì chuyển qua đó, vì như vậy lùi ít nhất.

![](https://labuladong.online/algo/images/kmp/A.gif)

Đương nhiên, nếu ký tự gặp phải là "B", trạng thái `X` cũng không tiến trạng thái được, chỉ có thể lùi, `j` cứ lùi theo hướng `X` chỉ là được:

![](https://labuladong.online/algo/images/kmp/shadow2.jpg)

Bạn có thể hỏi, sao `X` này biết gặp ký tự "B" thì lùi về trạng thái 0? Vì `X` mãi đi sau lưng `j`, trạng thái `X` chuyển thế nào trước đó đã tính xong. Thuật toán quy hoạch động chẳng phải là dùng kết quả quá khứ giải quyết vấn đề hiện tại sao?

Như vậy, ta viết chi tiết hơn khung code vừa rồi:

```python
int X # trạng thái bóng
for 0 <= j < M:
    for 0 <= c < 256:
        if c == pat[j]:
            # tiến trạng thái
            dp[j][c] = j + 1
        else: 
            # khởi động lại trạng thái
            # ủy thác X tính vị trí khởi động lại
            dp[j][c] = dp[X][c] 
```

### Bốn, Hiện thực code

Nếu nội dung trước đó bạn đều hiểu, chúc mừng, giờ chỉ còn một vấn đề: trạng thái bóng `X` lấy thế nào? Dưới đây xem thẳng code đầy đủ.

<!-- muliti_language -->
```java
public class KMP {
    private int[][] dp;
    private String pat;

    public KMP(String pat) {
        this.pat = pat;
        int M = pat.length();
        // dp[trạng thái][ký tự] = trạng thái tiếp theo
        dp = new int[M][256];
        // base case
        dp[0][pat.charAt(0)] = 1;
        // Trạng thái bóng X khởi đầu là 0
        int X = 0;
        // Trạng thái hiện tại j bắt đầu từ 1
        for (int j = 1; j < M; j++) {
            for (int c = 0; c < 256; c++) {
                if (pat.charAt(j) == c) 
                    dp[j][c] = j + 1;
                else 
                    dp[j][c] = dp[X][c];
            }
            // Cập nhật trạng thái bóng
            X = dp[X][pat.charAt(j)];
        }
    }

    public int search(String txt) {...}
}
```

Giải thích trước dòng code này:

```java
// base case
dp[0][pat.charAt(0)] = 1;
```

Dòng code này là base case, chỉ khi gặp ký tự pat[0] này mới khiến trạng thái chuyển từ 0 sang 1, gặp ký tự khác thì vẫn đứng yên ở trạng thái 0 (Java mặc định khởi tạo mảng toàn là 0).

Trạng thái bóng `X` khởi đầu là 0 trước, rồi theo bước tiến của `j` mà không ngừng cập nhật. Dưới đây xem rốt cuộc nên **cập nhật trạng thái bóng `X` thế nào**:

```java
int X = 0;
for (int j = 1; j < M; j++) {
    ...
    // Cập nhật trạng thái bóng
    // Hiện tại là trạng thái X, gặp ký tự pat[j],
    // pat nên chuyển sang trạng thái nào?
    X = dp[X][pat.charAt(j)];
}
```

Cập nhật `X` thật ra rất giống quá trình cập nhật trạng thái `j` trong hàm `search`:

```java
int j = 0;
for (int i = 0; i < N; i++) {
    // Hiện tại là trạng thái j, gặp ký tự txt[i],
    // pat nên chuyển sang trạng thái nào?
    j = dp[j][txt.charAt(i)];
    ...
}
```

**Nguyên lý trong đó rất tinh tế**, chú ý giá trị khởi đầu của biến vòng for trong code, có thể hiểu thế này: cái sau là khớp `pat` trong `txt`, cái trước là khớp `pat[1..end]` trong `pat`, trạng thái `X` mãi đi sau trạng thái `j` một trạng thái và có tiền tố chung dài nhất với `j`. Nên mình ví `X` là trạng thái bóng, dường như cũng hơi xác đáng.

Ngoài ra, xây dựng mảng dp là suy diễn về sau theo base case `dp[0][..]`. Đây chính là nguyên nhân mình cho rằng thuật toán KMP là một thuật toán quy hoạch động.

Dưới đây xem quá trình xây dựng đầy đủ của đồ thị chuyển trạng thái, bạn sẽ hiểu chỗ tinh diệu trong tác dụng của trạng thái `X`:

![](https://labuladong.online/algo/images/kmp/dfa.gif)

Tới đây, cốt lõi của thuật toán KMP cuối cùng cũng viết xong! Xem code đầy đủ của thuật toán KMP nào:

<!-- muliti_language -->
```java
public class KMP {
    private int[][] dp;
    private String pat;

    public KMP(String pat) {
        this.pat = pat;
        int M = pat.length();
        // dp[trạng thái][ký tự] = trạng thái tiếp theo
        dp = new int[M][256];
        // base case
        dp[0][pat.charAt(0)] = 1;
        // Trạng thái bóng X khởi đầu là 0
        int X = 0;
        // Xây dựng đồ thị chuyển trạng thái (sửa gọn hơn một chút)
        for (int j = 1; j < M; j++) {
            for (int c = 0; c < 256; c++)
                dp[j][c] = dp[X][c];
            dp[j][pat.charAt(j)] = j + 1;
            // Cập nhật trạng thái bóng
            X = dp[X][pat.charAt(j)];
        }
    }

    public int search(String txt) {
        int M = pat.length();
        int N = txt.length();
        // Trạng thái khởi đầu của pat là 0
        int j = 0;
        for (int i = 0; i < N; i++) {
            // Tính trạng thái tiếp theo của pat
            j = dp[j][txt.charAt(i)];
            // Tới trạng thái kết thúc, trả về kết quả
            if (j == M) return i - M + 1;
        }
        // Chưa tới trạng thái kết thúc, khớp thất bại
        return -1;
    }
}
```

Qua phần giảng giải ví dụ chi tiết trước đó, bạn hẳn hiểu được ý nghĩa đoạn code này, đương nhiên bạn cũng có thể viết thuật toán KMP thành một hàm. Code cốt lõi cũng chính là phần vòng for trong hai hàm, đếm xem có quá mười dòng không?

### Năm, Tổng kết cuối cùng

Thuật toán KMP truyền thống dùng một mảng một chiều `next` ghi thông tin tiền tố, còn bài này dùng một mảng hai chiều `dp` để giải quyết bài toán khớp ký tự dưới góc độ chuyển trạng thái, nhưng độ phức tạp không gian vẫn là O(256M) = O(M).

Trong quá trình `pat` khớp `txt`, chỉ cần làm rõ hai vấn đề「đang ở trạng thái nào」và「ký tự gặp phải là gì」là xác định được nên chuyển sang trạng thái nào (tiến hay lùi).

Với một chuỗi mẫu `pat`, tổng cộng có M trạng thái, với ký tự ASCII thì tổng cộng không quá 256 loại. Nên ta xây dựng một mảng `dp[M][256]` để bao mọi tình huống, và xác định rõ ý nghĩa mảng `dp`:

`dp[j][c] = next` biểu thị, hiện tại là trạng thái `j`, gặp ký tự `c` thì nên chuyển sang trạng thái `next`.

Xác định rõ ý nghĩa của nó là viết được code hàm search dễ dàng.

Còn cách xây dựng mảng `dp` này thì cần một trạng thái phụ `X`, nó mãi đi sau trạng thái hiện tại `j` một trạng thái và có tiền tố chung dài nhất với `j`, ta đặt cho nó cái tên「trạng thái bóng」.

Khi xây dựng hướng chuyển của trạng thái hiện tại `j`, chỉ có ký tự `pat[j]` mới khiến trạng thái tiến (`dp[j][pat[j]] = j+1`); còn với ký tự khác chỉ có thể lùi trạng thái, nên đi hỏi trạng thái bóng `X` xem nên lùi về đâu (`dp[j][other] = dp[X][other]`, trong đó `other` là mọi ký tự ngoài `pat[j]`).

Với trạng thái bóng `X`, ta khởi tạo nó là 0, và cập nhật theo bước tiến của `j`, cách cập nhật rất giống quá trình cập nhật `j` trong quá trình search (`X = dp[X][pat[j]]`).

Thuật toán KMP cũng chỉ là mấy chuyện của quy hoạch động, mục lục bài viết trên tài khoản công chúng của bọn mình có cả loạt bài chuyên về quy hoạch động, mà đều làm theo một khung, chẳng qua là mô tả logic bài toán, xác định rõ ý nghĩa mảng `dp`, định nghĩa base case mấy chuyện cỏn con đó. Hy vọng bài này giúp mọi người hiểu sâu hơn về quy hoạch động.



<hr>
<details class="hint-container details">
<summary><strong>Các bài viết trích dẫn bài này</strong></summary>

 - [Tâm đắc cày bài của mình: Bản chất của thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Mở rộng thuật toán cửa sổ trượt: Thuật toán khớp ký tự Rabin Karp](https://labuladong.online/algo/practice-in-action/rabinkarp/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》đã xuất bản, theo dõi tài khoản công chúng để xem chi tiết; trả lời「**full bộ**」ở hậu trường để tải PDF kèm theo và full bộ cày bài**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code các ngôn ngữ khác======

[28. Hiện thực strStr()](https://leetcode-cn.com/problems/implement-strstr)

### python

[MoguCloud](https://github.com/MoguCloud) cung cấp code Python đầy đủ hiện thực strStr():

```python
class Solution:
  def strStr(self, haystack: str, needle: str) -> int:
    # Kiểm tra điều kiện biên
    if not needle:
      return 0
    pat = needle
    txt = haystack

    M = len(pat)
    # dp[trạng thái][ký tự] = trạng thái tiếp theo
    dp = [[0 for _ in range(256)] for _ in pat]
    # base case
    dp[0][ord(pat[0])] = 1
    # Trạng thái bóng X khởi tạo là 0
    X = 0
    for j in range(1, M):
      for c in range(256):
        dp[j][c] = dp[X][c]
        dp[j][ord(pat[j])] = j + 1
        # Cập nhật trạng thái bóng
        X = dp[X][ord(pat[j])]

        N = len(txt)
        # Trạng thái khởi đầu của pat là 0 
        j = 0
        for i in range(N):
          # Tính trạng thái tiếp theo của pat
          j = dp[j][ord(txt[i])]
          # Tới trạng thái kết thúc, trả về kết quả
          if j == M:
            return i - M + 1
          # Chưa tới trạng thái kết thúc, khớp thất bại
          return -1
```



### javascript

```js
class KMP {
  constructor(pat) {
    this.pat = pat;
    let m = pat.length;

    // dp[trạng thái][ký tự] = trạng thái tiếp theo  Khởi tạo một ma trận số nguyên m*256
    this.dp = new Array(m);
    for (let i = 0; i < m; i++) {
      this.dp[i] = new Array(256);
      this.dp[i].fill(0, 0, 256);
    }

    // base case
    this.dp[0][this.pat[0].charCodeAt()] = 1;

    // Trạng thái bóng X khởi đầu là 0
    let x = 0;

    // Xây dựng đồ thị chuyển trạng thái
    for (let j = 1; j < m; j++) {
      for (let c = 0; c < 256; c++) {
        this.dp[j][c] = this.dp[x][c];
      }

      // dp[][mã ASCII tương ứng]
      this.dp[j][this.pat[j].charCodeAt()] = j + 1;

      // Cập nhật trạng thái bóng
      x = this.dp[x][this.pat[j].charCodeAt()]
    }
  }

  search(txt) {

    let m = this.pat.length;
    let n = txt.length;

    // Trạng thái khởi đầu của pat là 0
    let j = 0;
    for (let i = 0; i < n; i++) {
      // Tính trạng thái tiếp theo của pat
      j = this.dp[j][txt[i].charCodeAt()];

      // Tới trạng thái kết thúc, trả về kết quả
      if (j === m) return i - m + 1;
    }

    // Chưa tới trạng thái kết thúc, khớp thất bại
    return -1;
  }

}

/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) { 
  if(haystack === ""){
    if(needle !== ""){
      return -1;
    }
    return 0;
  }

  if(needle === ""){
    return 0;
  }
  let kmp = new KMP(needle);
  return kmp.search(haystack)
};
```


