# Chiêu 「lấy điểm」 khi thi viết thuật toán



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**



Trước hết trả lời một câu: luyện đề LeetCode là luyện thẳng trên web tốt hơn hay trên IDE local tốt hơn?

Nếu là dạng chấm thi kiểu Niuke tự xử input/output thì nhất định phải viết trên IDE, cái này không có gì để nói, nhưng **với dạng chấm như LeetCode, cá nhân tôi thích luyện thẳng trên web**, vì hai lý do:

**1, Tiện**

Vì có cấu trúc dữ liệu LeetCode tự định, ví dụ `TreeNode`, `ListNode`, ở local bạn còn phải copy class đó qua.

Mà trên IDE không test được, viết code xong còn phải paste lên web chạy test, vậy thà viết thẳng trên web.

Thuật toán lại không phải code dự án, lượng code đều khá nhỏ, lợi ích autocomplete của IDE cơ bản có thể bỏ qua.

**2, Thực dụng**

Tới lúc phỏng vấn, đề thuật toán interviewer cho đa số mong bạn hoàn thành thẳng trên web, tốt nhất vừa viết vừa giảng hướng suy nghĩ.

Nếu lúc luyện bình thường đã quen không có autocomplete, quen viết tay code não biên dịch, thì lúc phỏng vấn viết code sẽ nhanh và thong dong hơn.

Trước tôi phỏng vấn Kuaishou, có interviewer bắt tôi [implement thuật toán LRU](https://labuladong.online/algo/data-structure/lru-cache/), tôi viết thẳng implement doubly-linked-list, hash-linked-list trên web hết, mà một lần chạy không bug, thấy được vẻ ngạc nhiên của interviewer 😂

Tôi đợt tuyển mùa thu làm được máy gặt offer, phần lớn chính vì ải viết tay thuật toán vượt kỳ vọng interviewer, thật ra đều nhờ trước đây luyện đề trên web mà ra.

Đương nhiên, thực sự không muốn luyện trên web, cũng có thể dùng plugin luyện đề vscode hay JetBrains của tôi, plugin và nội dung web của tôi đều kết hợp hoàn hảo.

Tiếp theo giới thiệu vài cách 「luồn lách」 rất thực dụng và mẹo debug, nâng toàn diện xác suất bạn qua kỳ thi viết.






## Tránh mạnh đánh yếu

Mọi người cũng biết, phần lớn đề thi viết cần bạn tự xử dữ liệu input, rồi để chương trình in output. Nguyên lý tầng đáy của việc chấm là, dùng redirect `>` của Linux ghi output chương trình bạn vào file, rồi so output bạn với đáp án đúng có giống không.

Vậy điểm khó của những vấn đề này coi như không tồn tại, ta có thể làm tắt, lấy ví dụ đơn giản hóa, giả sử đề cho bạn input một chuỗi ký tự cách nhau bằng dấu cách, bảo bạn đây là một linked list đơn, hãy đảo linked list này, mà nhấn mạnh, nhất định phải biến số input thành linked list rồi mới đảo nhé!

Vậy bạn làm sao? Thật sự tự định nghĩa class node `ListNode`, rồi viết code biến input thành linked list, rồi dùng thao tác con trỏ làm người ta nhức đầu để ngoan ngoãn đảo linked list?

Hiểu rõ ta tới để AC đề, không phải tới học tư duy thuật toán, hệ chấm không phán đoán được logic thuật toán, chỉ phán đoán output bạn có đúng không. Nên cách lách là lưu thẳng input vào mảng, rồi dùng [mẹo hai con trỏ](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/) vài dòng code đảo nó, rồi in ra xong.

Tôi từng thấy không ít đề này, ví dụ đề nói input là một linked list đơn, bắt tôi đảo nhóm linked list, mà còn nhấn mạnh phải dùng đệ quy implement, chính là thuật toán [đảo linked list theo nhóm k](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/) ở bài trước. Ừ, nếu dùng mảng đảo, hai phút là viết xong, hehe.
 
Còn đề [làm phẳng nested list](https://labuladong.online/algo/data-structure/flatten-nested-list-iterator/) bài trước đã giảng, hướng suy nghĩ rất khéo, nhưng khi gặp ở kỳ thi viết, input là một chuỗi dạng `[1,[4,[6]]]`, vậy dùng thẳng regex để tách số ra, chính là một list làm phẳng rồi……






## Chọn ngôn ngữ lập trình

Chỉ xét góc làm bài thuật toán, cá nhân tôi khá khuyên dùng Java làm ngôn ngữ thi viết. Vì IntelliJ nhà JetBrain thật sự quá ngon, so với editor của ngôn ngữ khác, không chỉ có lệnh tắt `psvm` và `sout` (bạn mà tới cái này còn không biết thì mau úp mặt đi), mà còn giúp bạn check nhiều lỗi gõ nhầm, ví dụ quên tăng biến trong vòng `while`, hay câu `return` viết nhầm vào vòng lặp do sơ suất.

C++ cũng tạm được, nhưng tôi thấy không dễ dùng bằng Java. Tôi nhớ C++ tới hàm `split` tách chuỗi cũng không có, chỉ riêng điểm này tôi đã không muốn dùng C++……

Còn một điểm, code C++ giới hạn thời gian thấp hơn, ngôn ngữ khác giới hạn 4000ms, C++ giới hạn 2000ms, tôi thấy khá thiệt. Chả trách xem người ta dùng C++ viết thuật toán, để tăng tốc, đều không dùng container `vector` chuẩn, cứ phải dùng mảng `int[]` gốc, tôi nhìn còn nhức đầu.

Python thì tôi luyện đề dùng khá ít, vì tôi không thích dùng ngôn ngữ động, khó debug. Nhưng ngôn ngữ này đúng là cung cấp nhiều chức năng thực dụng, nếu bạn nắm rõ mánh Python, có thể luồn lách lúc nào đó. Ví dụ [thuật toán tính giá trị biểu thức](https://labuladong.online/algo/data-structure/implement-calculator/) bài trước viết là thuật toán level khó, nhưng nếu dùng hàm `exec` có sẵn của Python, trực tiếp là tính ra đáp án.

Cái này trong kỳ thi viết chắc chắn rất có lợi, vì trước đã nói rồi, ta cần kết quả, không ai quan tâm bạn lấy được kết quả bằng cách nào.






## Phân tầng code lời giải

Phân tầng code có thể coi là thói quen khá tốt, có thể tăng tốc viết code và giảm khó debug.

Đơn giản nói là, đừng viết mọi code trong hàm `main`, công thức tôi vẫn hay dùng là, hàm `main` phụ trách nhận dữ liệu, thêm một hàm `solution` phụ trách thống nhất xử lý dữ liệu và output đáp án, rồi dùng một hàm như `backtrack` xử lý logic thuật toán cụ thể.

Lấy ví dụ, ví dụ một đề, tôi quyết dùng quy hoạch động có memo giải, cấu trúc code đại khái thế này:




```java
public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        // Chủ yếu nhận dữ liệu
        int N = scanner.nextInt();
        int[][] orders = new int[N][2];
        for (int i = 0; i < N; i++) {
            orders[i][0] = scanner.nextInt();
            orders[i][1] = scanner.nextInt();
        }
        // Ủy cho solution giải
        solution(orders);
    }

    static void solution(int[][] orders) {
        // Loại vài case biên cơ bản
        if (orders.length == 0) {
            System.out.println("None");
            return;
        }
        // Ủy hàm dp chạy logic thuật toán cụ thể
        int res = dp(orders, 0);
        // Xuất kết quả
        System.out.println(res);
    }

    // Memo
    static HashMap<String, Integer> memo = new HashMap<>();
    static int dp(int[][] orders, int start) {
        // Logic thuật toán cụ thể
    }
}
```


Bạn xem phân tầng vậy có rõ không, mỗi hàm đều có nhiệm vụ chính phụ trách, chỗ nào có vấn đề, bạn cũng dễ debug.

Chứ không phải nói phải viết code chuẩn chỉnh thế nào, các ràng buộc như `private` miễn cũng không sao, biến đặt tên pinyin cũng OK, mấu chốt là đừng viết thẳng mọi code vào hàm `main`, thật loạn, không lỗi thì thôi, một khi lỗi, e là tốn công debug, không tìm ra vấn đề lại loạn trận, đó là điều nên cố tránh.

## Debug thuật toán thế nào

Lỗi code không tránh được, đôi khi cả hướng suy nghĩ còn sai, đôi khi là vấn đề chi tiết, ví dụ `i` và `j` viết ngược, vấn đề này kiểm tra thế nào?

Tôi nghĩ bài thuật toán thường chắc chắn không khó kiểm tra, mắt thường check hẳn không vấn đề gì, cùng lắm `print` vài giá trị biến mấu chốt, kiểu gì cũng phát hiện vấn đề.

**Điều khá làm người ta nhức đầu hẳn là kiểm tra vấn đề của thuật toán đệ quy**.

Nếu không có kinh nghiệm nhất định, quá trình hàm đệ quy rất khó được hiểu đúng, nên ở đây sẽ giảng kỹ cách debug thuật toán đệ quy hiệu quả.

Có bạn có thể nói, copy thuật toán vào IDE, rồi đặt breakpoint đi từng bước chẳng phải được sao?

Cách này chắc chắn được, nhưng bài trước nhiều lần nói, hàm đệ quy tốt nhất hiểu từ góc toàn cục, chứ đừng nhảy vào chi tiết cụ thể.

Nếu bạn với đệ quy chưa đủ quen, không có góc nhìn toàn cục, cách đặt breakpoint từng bước này cũng dễ làm người ta rối.

**Khuyên của tôi là in thẳng giá trị mấu chốt trong hàm đệ quy, kết hợp thụt lề, trực quan quan sát tình hình chạy của hàm đệ quy**.

Điều nâng hiệu suất debug của ta nhất chính là thụt lề, ngoài hàm lời giải, ta định nghĩa mới một hàm `printIndent` và một biến toàn cục `count`:




```java
// Biến toàn cục, ghi số tầng đệ quy của hàm đệ quy
int count = 0;

// Nhập n, in n thụt tab
void printIndent(int n) {
    for (int i = 0; i < n; i++) {
        printf("   ");
    }
}
```


Tiếp theo, công thức tới:

**Ở đầu hàm đệ quy, gọi `printIndent(count++)` và in biến mấu chốt; rồi trước mọi câu `return` gọi `printIndent(--count)` và in giá trị trả về**.

Lấy ví dụ cụ thể, ví dụ bài trước [quy hoạch động trong game Fallout](https://labuladong.online/algo/dynamic-programming/freedom-trail/) implement một hàm `dp` đệ quy, cấu trúc đại khái như sau:




```java
int dp(String ring, int i, String key, int j) {
    // base case
    if (j == key.length()) {
        return 0;
    }
    
    // Chuyển trạng thái
    for (int k : charToIndex.get(key.charAt(j))) {
        int subProblem = dp(ring, k, key, j + 1);
    }
    
    return res;
}
```


Hàm `dp` đệ quy này sau khi tôi debug, thành thế này:




```java
int count = 0;
void printIndent(int n) {
    for (int i = 0; i < n; i++) {
        System.out.print("   ");
    }
}

int dp(String ring, int i, String key, int j) {
    // printIndent(count++);
    // printf("i = %d, j = %d\n", i, j);
    
    if (j == key.length()) {
        // printIndent(--count);
        // printf("return 0\n");
        return 0;
    }
    

    for (int k : charToIndex.get(key.charAt(j))) {
        int subProblem = dp(ring, k, key, j + 1);
    }
    
    // printIndent(--count);
    // printf("return %d\n", res);
    return res;
}
```


**Chính là thêm vài code in ở đầu hàm và chỗ tương ứng mọi câu `return`**.

Nếu bỏ comment, chạy một test case, output như sau:

![](https://labuladong.online/algo/images/algo-debug-tech/1.jpg)

Như vậy, qua so sánh mức thụt tương ứng là biết mỗi lần đệ quy giá trị tham số mấu chốt `i, j`, cùng mỗi lần gọi đệ quy trả về kết quả bao nhiêu.

**Quan trọng nhất là, có thể trực quan thấy quá trình đệ quy, bạn có phát hiện đây chính là một cây đệ quy**?

![](https://labuladong.online/algo/images/algo-debug-tech/2.jpg)

Bài trước [chi tiết công thức quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) nói, hiểu hàm đệ quy quan trọng nhất chính là vẽ cây đệ quy, in vậy, tới cây đệ quy cũng không cần tự vẽ, mà còn thấy rõ giá trị trả về của mỗi lần đệ quy.

**Có thể nói, đây là mẹo nhỏ nâng 「cảm giác hạnh phúc」 khi luyện đề nhất, hiệu quả hơn đặt breakpoint trong IDE**.

Tôi ở panel trực quan hỗ trợ thẳng chức năng này, giá trị in trong hàm đệ quy sẽ tự thêm thụt lề, cụ thể xem [hướng dẫn dùng panel trực quan](https://labuladong.online/algo/intro/visualize/).






## Chiến lược ôn trước thi

Trước kỳ thi đừng cố chấp với một bài thuật toán nào, không đáng.

Nên xem càng nhiều đề đủ loại càng tốt, nghĩ năm phút, nghĩ không ra lời giải thì xem thẳng đáp án người khác. Hiểu hướng suy nghĩ là được, thậm chí tự viết một lần cũng không cần, vì khá tốn thời gian.

Lúc thi viết sợ nhất là không có hướng suy nghĩ, nên lướt qua đủ loại đề một chút, ít nhất trong lòng không hoảng, chỉ cần có hướng suy nghĩ, trung bình một đề hai ba mươi phút giải xong vẫn không khó.

Như trước đã nói, không có vấn đề gì mà liệt kê brute-force không giải được, cứ dùng thẳng [công thức backtrack](https://labuladong.online/algo/essential-technique/backtrack-framework/) mà làm tới, cùng lắm thêm memo, chẳng phải thành [công thức quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) sao, cùng lắm đề này tôi bỏ, brute-force qua 60% case cũng khá OK.

Không nói nhiều, công thức thứ này, nói ra thì đơn giản, chỉ một cái là hiểu ngay, nhưng vấn đề là không chỉ thì không hiểu. Bài này tôi giới thiệu đơn giản vài mẹo cho kỳ thi viết thuật toán, các bạn ngẫm kỹ nhé~






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Thuật toán chọn ngẫu nhiên theo trọng số](https://labuladong.online/algo/frequency-interview/random-pick-with-weight/)

</details><hr>





**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
