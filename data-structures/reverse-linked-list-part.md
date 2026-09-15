# Tổng hợp cách lật danh sách liên kết đơn hoa công thức 




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | [206. Lật linked list](https://leetcode.cn/problems/reverse-linked-list/) | 🟢 |
| [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | [25. Lật linked list theo nhóm K](https://leetcode.cn/problems/reverse-nodes-in-k-group/) | 🔴 |
| [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) | [92. Lật linked list II](https://leetcode.cn/problems/reverse-linked-list-ii/) | 🟠 |

**-----------**



Lật danh sách liên kết đơn bằng lặp không phải việc khó, nhưng cài đặt đệ quy thì hơi khó. Nếu thêm chút khó, bắt bạn chỉ lật một phần trong danh sách liên kết đơn, bạn có thể đồng thời dùng lặp và đệ quy cài đặt không? Tiến thêm, nếu bắt bạn lật linked list theo nhóm k, các hạ lại ứng đối thế nào?

Bài này thì từ nông tới sâu, một lần giải hết vấn đề thao tác linked list này. Tôi sẽ đồng thời dùng cách đệ quy và lặp, và kết hợp panel trực quan giúp bạn hiểu, để tăng cường tư duy đệ quy và khả năng thao tác con trỏ linked list của bạn.

## Lật toàn bộ danh sách liên kết đơn 

Trong LeetCode/ LeetCode CN, cấu trúc tổng quát của danh sách liên kết đơn như sau:

```java
// Cấu trúc Node danh sách liên kết đơn 
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

Lật danh sách liên kết đơn là một bài thuật toán tương đối cơ bản, bài 206 trên LeetCode「Lật linked list」chính là vấn đề này:

<Pronlem slug="reverse-linked-list" />

Dưới đây chúng ta thử dùng nhiều cách giải vấn đề này.

### lời giải lặp

Cách thường của bài này chính là lời giải lặp, qua thao tác mấy con trỏ, hướng con trỏ của mỗi Node trong linked list lật, không có khó gì, chủ yếu là chi tiết thao tác con trỏ.

Ở đây cho thẳng code, kết hợp chú thích và panel trực quan phải không khó hiểu:

```java
class Solution {
    // Lật danh sách liên kết đơn đỉnh xuất phát là head
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        // Vì cấu trúc danh sách liên kết đơn, ít nhất cần dùng ba con trỏ mới hoàn thành lật lặp
        // cur là Node duyệt hiện tại, pre là Node node tiền nhiệm của cur, nxt là Node node kế nhiệm của cur
        ListNode pre, cur, nxt;
        pre = null; cur = head; nxt = head.next;
        while (cur != null) {
            // đuổi Node lật
            cur.next = pre;
            // Cập nhật vị trí con trỏ
            pre = cur;
            cur = nxt;
            if (nxt != null) {
                nxt = nxt.next;
            }
        }
        // Trả về Node đầu sau lật
        return pre;
    }
}
```

<visual slug="reverse-linked-list-iter" >

Bạn có thể mở panel trực quan dưới, click nhiều lần dòng code <code type="click">cur.next = pre</code>, là có thể trực quan thấy quá trình lật danh sách liên kết đơn :

</visual>

> [!TIP]
> Logic code thao tác danh sách liên kết đơn trên không phức tạp, mà cũng không chỉ một cách viết đúng của tôi. Nhưng khi thao tác con trỏ, có vài tiểu xảo rất cơ bản, rất đơn giản, có thể khiến ý tưởng viết code của bạn rõ hơn:
>
> 1, Một khi xuất hiện thao tác kiểu `nxt.next`, thì cần phản xạ điều kiện nghĩ, kiểm tra trước `nxt` có null không, nếu không dễ xuất hiện null pointer exception.
>
> 2, Chú ý điều kiện kết thúc vòng lặp. Bạn cần biết khi vòng lặp kết thúc, vị trí các con trỏ, như vậy mới đảm bảo trả về đáp án đúng. Nếu bạn thấy hơi phức tạp nghĩ không rõ, vậy bắt tay làm vẽ một tình huống đơn giản nhất chạy thuật toán, ví dụ bài này có thể vẽ một danh sách liên kết đơn chỉ hai Node `1->2`, rồi là có thể xác định sau khi vòng lặp kết thúc vị trí các con trỏ.

### lời giải đệ quy

 lời giải lặp trên thao tác con trỏ dù hơi rườm rà, nhưng ý tưởng vẫn tương đối rõ. Nếu giờ bắt bạn dùng đệ quy để lật danh sách liên kết đơn, bạn có ý tưởng gì?

Với người mới có thể rất khó nghĩ, đây rất bình thường. Nếu bạn học tư duy thuật toán series cây nhị phân ở phần sau, quay đầu xem lại bài này, mới có thể tự nghĩ ra thuật toán này.

Vì cấu trúc cây nhị phân bản thân chính là mở rộng của danh sách liên kết đơn, tương đương là danh sách liên kết nhị chạc, nên tư duy đệ quy trên cây nhị phân, áp dụng lên danh sách liên kết đơn là giống nhau.

**Mấu chốt đệ quy lật danh sách liên kết đơn là, vấn đề bản thân tồn tại cấu trúc bài con**.

Ví dụ, giờ cho bạn nhập một danh sách liên kết đơn đầu là `1` là `1->2->3->4`, vậy nếu tôi bỏ qua đầu `1` này, chỉ lấy ra danh sách liên kết con `2->3->4` này, nó cũng là danh sách liên kết đơn đúng không?

Vậy hàm `reverseList` này của bạn, chỉ cần nhập một danh sách liên kết đơn, là có thể lật cho tôi đúng không? Vậy bạn có thể dùng hàm này lật danh sách liên kết con `2->3->4` này trước không, rồi nghĩ cách `1` nối vào cuối của `4->3->2` sau lật, có phải thì hoàn thành lật cả linked list?

```java
reverseList(1->2->3->4) = reverseList(2->3->4) -> 1
```


**Đây chính là ý tưởng 「phân rã bài toán」, qua định nghĩa của hàm đệ quy, bài gốc phân thành nếu làm bài con quy mô nhỏ hơn, cấu trúc giống nhau, cuối cùng qua đáp án của bài con ghép lại của bài gốc**.

Trong tutorial sau sẽ có chương chuyên giải thích và luyện tư duy này, ở đây không triển khai .

Xem trước cài đặt code đệ quy lật danh sách liên kết đơn :

```java
class Solution {
    // Định nghĩa: nhập một đầu danh sách liên kết đơn, linked list này lật, trả về đầu mới
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode last = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return last;
    }
}
```

Thuật toán này thường lấy thể hiện khéo hay và tinh tế của đệ quy, dưới đây chúng ta giải thích chi tiết đoạn code này, cuối cùng cho ra panel trực quan, bạn có thể tự bắt tay làm tìm hiểu quá trình đệ quy.

Với thuật toán đệ quy ý tưởng 「phân rã bài toán」, quan trọng nhất chính là rõ ràng định nghĩa của hàm đệ quy. Cụ thể, định nghĩa hàm `reverseList` của chúng ta như sau:

**Nhập một Node `head`, linked list「 đỉnh xuất phát là `head`」lật, và trả về đầu sau lật**.

Hiểu định nghĩa của hàm sau, xem lại vấn đề này. Ví dụ chúng ta muốn lật linked list này:

![](https://labuladong.online/algo/images/reverse-linked-list/1.jpg)

Vậy sau khi nhập `reverseList(head)`, sẽ đệ quy ở đây:

```java
ListNode last = reverseList(head.next);
```


Đừng nhảy vào đệ quy (đầu bạn có thể đè mấy stack?), mà cần dựa vào định nghĩa hàm vừa rồi, làm rõ đoạn code này sẽ sinh kết quả gì:

![](https://labuladong.online/algo/images/reverse-linked-list/2.jpg)

Sau khi `reverseList(head.next)` này chạy xong, cả linked list thì thành thế này:

![](https://labuladong.online/algo/images/reverse-linked-list/3.jpg)

Và dựa vào định nghĩa hàm, hàm `reverseList` sẽ trả về đầu sau lật, chúng ta dùng biến `last` nhận.

Giờ xem lại code dưới:

```java
head.next.next = head;
```

![](https://labuladong.online/algo/images/reverse-linked-list/4.jpg)

Tiếp theo:

```java
head.next = null;
return last;
```

![](https://labuladong.online/algo/images/reverse-linked-list/5.jpg)





Thần không thần kỳ, như vậy cả linked list thì lật qua! Code đệ quy chính là gọn thanh lịch vậy, nhưng trong đó có hai chỗ cần chú ý:

1, Hàm đệ quy cần có base case, chính là câu này:

```java
if (head == null || head.next == null) {
    return head;
}
```

Ý là nếu linked list rỗng hoặc chỉ một Node, kết quả lật chính là nó, trả về thẳng là được.

2, Khi linked list đệ quy lật sau, đầu mới là `last`, mà `head` trước đó thành Node cuối cùng, đừng quên cuối của linked list cần trỏ tới null:

```java
head.next = null;
```

Như vậy, cả danh sách liên kết đơn thì hoàn thành lật, thần không thần kỳ? Dưới đây là quá trình trực quan lật linked list đệ quy:


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/reverse-linked-list/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>

> [!NOTE]
> Dù panel trực quan có thể trình diễn mọi chi tiết của cả quá trình đệ quy, nhưng tôi không khuyên người mới quá chấp chi tiết. Khuyên trước theo cách tư duy giải thích hình trên hiểu đệ quy, rồi qua panel trực quan cộng sâu hiểu.

> [!NOTE]
> Đáng nói, đệ quy thao tác linked list không hiệu quả.
>
> lời giải đệ quy và lời giải lặp so, độ phức tạp thời gian đều O(N), nhưng độ phức tạp không gian của lời giải lặp là O(1), mà lời giải đệ quy cần stack, độ phức tạp không gian là O(N).
>
> Nên đệ quy thao tác linked list có thể dùng luyện tư duy đệ quy, nhưng xét hiệu suất vẫn dùng thuật toán lặp tốt hơn.

## Lật N Node đầu của linked list

Lần này chúng ta cài đặt một hàm thế này:

```java
// Lật n Node đầu của linked list (n <= độ dài linked list)
ListNode reverseN(ListNode head, int n)
```

Ví dụ với linked list ở hình dưới, chạy `reverseN(head, 3)`:

![](https://labuladong.online/algo/images/reverse-linked-list/6.jpg)






### lời giải lặp

 lời giải lặp phải tương đối tốt viết, trên cơ sở `reverseList` cài đặt trước đó sửa chút là được:

```java
ListNode reverseN(ListNode head, int n) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode pre, cur, nxt;
    pre = null; cur = head; nxt = head.next;
    while (n > 0) {
        cur.next = pre;
        pre = cur;
        cur = nxt;
        if (nxt != null) {
            nxt = nxt.next;
        }
        n--;
    }
    // Lúc này cur là Node thứ n + 1, head là cuối Node sau lật
    head.next = cur;
    // Lúc này pre là đầu Node sau lật
    return pre;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/reverse-n-iter/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>



### lời giải đệ quy

 ý tưởng đệ quy và đệ quy lật cả linked list gần giống, chỉ cần sửa chút là được:

```java
// Node kế nhiệm 
ListNode successor = null;

// Lật n Node đỉnh xuất phát là head, trả về đầu mới
ListNode reverseN(ListNode head, int n) {
    if (n == 1) {
        // Ghi Node thứ n + 1
        successor = head.next;
        return head;
    }
    // Lấy head.next làm đỉnh xuất phát, cần lật n - 1 Node đầu
    ListNode last = reverseN(head.next, n - 1);

    head.next.next = head;
    // Để Node head sau lật nối với Node phía sau
    head.next = successor;
    return last;
}
```

 khác biệt cụ thể:

1, base case thành `n == 1`, lật một phần tử, chính là nó, **đồng thời cần ghi Node kế nhiệm **, tức cần ghi Node thứ `n + 1`.

2, Vừa rồi chúng ta `head.next` đặt thẳng thành null, vì cả linked list lật sau `head` gốc thành Node cuối cùng của cả linked list. Nhưng giờ Node `head` sau khi đệ quy lật không nhất định là Node cuối cùng, nên cần ghi kế nhiệm `successor` (Node thứ `n + 1`), sau lật `head` nối lên.




![](https://labuladong.online/algo/images/reverse-linked-list/7.jpg)


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/list-reverse-n/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>



## Lật một phần của linked list

Chúng ta có thể tiến thêm, cho bạn một khoảng index, bắt bạn phần tử trong khoảng này của danh sách liên kết đơn lật, phần khác không đổi.

Bài 92 trên LeetCode「Lật linked list II」chính là vấn đề này:

<Problem slug="reverse-linked-list-ii" />



Đề nhập khoảng index `[m, n]` (index bắt đầu từ 1), chỉ lật phần tử linked list trong khoảng, chữ ký hàm như sau:

```java
ListNode reverseBetween(ListNode head, int m, int n)
```

### lời giải lặp

 ý tưởng thuần lặp tương đối trực tiếp, có thể tìm Node thứ `m - 1` trước, rồi tái sử dụng hàm `reverseN` cài đặt trước đó là được:

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if (m == 1) {
            return reverseN(head, n);
        }
        // Tìm node tiền nhiệm của Node thứ m
        ListNode pre = head;
        for (int i = 1; i < m - 1; i++) {
            pre = pre.next;
        }
        // Bắt đầu lật từ Node thứ m
        pre.next = reverseN(pre.next, n - m + 1);
        return head;
    }

    ListNode reverseN(ListNode head, int n) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre, cur, nxt;
        pre = null; cur = head; nxt = head.next;
        while (n > 0) {
            cur.next = pre;
            pre = cur;
            cur = nxt;
            if (nxt != null) {
                nxt = nxt.next;
            }
            n--;
        }
        // Lúc này cur là Node thứ n + 1, head là cuối Node sau lật
        head.next = cur;
        // Lúc này pre là đầu Node sau lật
        return pre;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/reverse-linked-list-ii-iter/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 Animation trực quan hóa code👾</strong>
</summary>
</details>
</a>
<hr/>



### lời giải đệ quy

 lời giải thuần đệ quy, vẫn là tìm Node thứ `m - 1`, rồi tái sử dụng hàm `reverseN` cài đặt trước đó là được.

Mấu chốt là, qua cách đệ quy tìm Node thứ `m - 1` thế nào?

Nếu chúng ta xem index của `head` là 1, vậy chúng ta muốn lật bắt đầu từ phần tử thứ `m` đúng không; nếu xem index của `head.next` là 1? Vậy tương đối với `head.next`, khoảng lật phải bắt đầu từ phần tử thứ `m - 1`; vậy với `head.next.next`?...

Đây thực ra chính là dùng cách đệ quy để lặp. Chúng ta có thể viết code thế này:

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        // base case
        if (m == 1) {
            return reverseN(head, n);
        }
        // Tiến tới đỉnh xuất phát lật kích hoạt base case
        head.next = reverseBetween(head.next, m - 1, n - 1);
        return head;
    }

    // Node kế nhiệm 
    ListNode successor = null;

    // Lật n Node đỉnh xuất phát là head, trả về đầu mới
    ListNode reverseN(ListNode head, int n) {
        if (n == 1) {
            // Ghi Node thứ n + 1
            successor = head.next;
            return head;
        }
        ListNode last = reverseN(head.next, n - 1);

        head.next.next = head;
        head.next = successor;
        return last;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/reverse-linked-list-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>



## Lật linked list theo nhóm K

Vấn đề này thường thấy trong kinh nghiệm phỏng vấn, mà trên LeetCode độ khó là Hard, xem đề:

<Problem slug="reverse-nodes-in-k-group" />

Có lót từng lớp trước đó, nó thực khó vậy sao? Thực ra chỉ cần bạn dùng tư duy「phân rã bài toán」, rồi tái sử dụng thẳng hàm `reverseN` phía trước là được.






### Phân tích ý tưởng 

Nghĩ kỹ có thể phát hiện **vấn đề này có tính đệ quy**.

Ví dụ chúng ta gọi `reverseKGroup(head, 2)` với linked list này, tức lật linked list theo nhóm 2 Node:

![](https://labuladong.online/algo/images/kgroup/1.jpg)

Nếu tôi tìm cách lật 2 Node đầu, vậy những Node phía sau xử lý sao? Những Node phía sau này cũng là một linked list, mà quy mô (độ dài) nhỏ hơn linked list gốc này, đây thì gọi bài con quy mô nhỏ hơn, cấu trúc giống nhau.

Chúng ta có thể di con trỏ `head` gốc tới đầu của đoạn linked list phía sau này, rồi tiếp tục đệ quy gọi `reverseKGroup(head, 2)`:

![](https://labuladong.online/algo/images/kgroup/2.jpg)

Phát hiện tính đệ quy, là có thể ra quy trình thuật toán đại khái :

**1, Lật `k` phần tử đầu là `head` trước**. Ở đây có thể tái sử dụng hàm `reverseN` cài đặt trước đó.

![](https://labuladong.online/algo/images/kgroup/3.jpg)

**2, Lấy phần tử thứ `k + 1` làm `head` đệ quy gọi hàm `reverseKGroup`**.

![](https://labuladong.online/algo/images/kgroup/4.jpg)

**3, Nối kết quả của hai quá trình trên lại**.

![](https://labuladong.online/algo/images/kgroup/5.jpg)

### Cài đặt code

Kết hợp giải thích từng bước trên, code là có thể viết thẳng ra. Tôi ở đây dùng hàm `reverseN` dạng lặp, bạn muốn dùng dạng đệ quy cũng được:

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null) return null;
        // khoảng [a, b) chứa k phần tử chờ lật
        ListNode a, b;
        a = b = head;
        for (int i = 0; i < k; i++) {
            // Không đủ k, không cần lật nữa
            if (b == null) return head;
            b = b.next;
        }
        // Lật k phần tử đầu
        ListNode newHead = reverseN(a, k);
        // Lúc này b trỏ tới đầu chờ lật của nhóm tiếp theo
        // Đệ quy lật linked list sau đó và nối lại
        a.next = reverseKGroup(b, k);
        return newHead;
    }

    // Hàm lật N Node đầu cài đặt ở trên
    ListNode reverseN(ListNode head, int n) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre, cur, nxt;
        pre = null; cur = head; nxt = head.next;
        while (n > 0) {
            cur.next = pre;
            pre = cur;
            cur = nxt;
            if (nxt != null) {
                nxt = nxt.next;
            }
            n--;
        }
        head.next = cur;
        return pre;
    }
}
```

Rất nhanh, bài này giải xong.


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/reverse-nodes-in-k-group/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>

## Tổng kết cuối

Tư tưởng đệ quy tương đối tư tưởng lặp, hơi khó hiểu một chút, kỹ thuật xử lý là: đừng nhảy vào đệ quy, mà lợi dụng định nghĩa rõ ràng để cài đặt logic thuật toán.

Xử lý vấn đề nhìn tương đối khó, có thể thử chia nhỏ, vài lời giải đơn giản sửa, giải vấn đề khó.






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Bài tập kinh điển hai con trỏ linked list](https://labuladong.online/algo/problem-set/linkedlist-two-pointers/)
 - [Tâm pháp cây nhị phân (Phần ý tưởng )](https://labuladong.online/algo/data-structure/binary-tree-part1/)
 - [ kiểm tra linked list palindrome thế nào](https://labuladong.online/algo/data-structure/palindrome-linked-list/)
 - [Thuật toán sắp xếp bánh](https://labuladong.online/algo/frequency-interview/pancake-sorting/)
 - [ khuôn mẫu 「lừa điểm」 bài kiểm tra thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/?show=1) | [2. Cộng hai số](https://leetcode.cn/problems/add-two-numbers/?show=1) | 🟠 |
| [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/?show=1) | [24. Hoán đổi Node trong linked list theo cặp](https://leetcode.cn/problems/swap-nodes-in-pairs/?show=1) | 🟠 |
| [445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/?show=1) | [445. Cộng hai số II](https://leetcode.cn/problems/add-two-numbers-ii/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 24. Lật linked list](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer II 024. Lật linked list](https://leetcode.cn/problems/UHnkqh/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer II 025. Cộng hai số trong linked list](https://leetcode.cn/problems/lMSNwu/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
