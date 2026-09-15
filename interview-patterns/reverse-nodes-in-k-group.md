# Đảo linked list theo nhóm k

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên web bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn user cũ~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | [25. Đảo linked list theo nhóm K](https://leetcode.cn/problems/reverse-nodes-in-k-group/) | 🔴 |

**-----------**

Bài trước [đệ quy đảo một phần linked list](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/) giảng cách đệ quy đảo một phần linked list, có bạn hỏi đảo linked list bằng lặp thế nào, vậy phần một bài này sẽ giảng cách đảo linked list đơn bằng lặp.

Có hàm đảo này xong, ta vẫn sẽ dùng cách đệ quy giải LeetCode 25 「đảo linked list theo nhóm K」, nên lúc kiểm tra tư duy đệ quy của bạn tới, chuẩn bị xong chưa?

Xem trước đề, không khó hiểu:

<Problem slug="reverse-nodes-in-k-group" />

Vấn đề này hay thấy trong kinh nghiệm phỏng vấn, mà độ khó trên LeetCode là Hard, nó thật khó vậy sao?

Với bài thuật toán cấu trúc dữ liệu cơ bản thực ra đều không khó, chỉ cần kết hợp đặc điểm để tách nhỏ phân tích từng chút, thông thường đều không có điểm khó. Dưới đây ta tách nhỏ vấn đề này.

### Một, phân tích vấn đề

Trước hết, bài trước [tư duy framework học cấu trúc dữ liệu](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/)đề cập tới, linked list là cấu trúc dữ liệu vừa có tính đệ quy vừa có tính lặp, nghĩ kỹ sẽ phát hiện **vấn đề này có tính đệ quy**.

Cái gì gọi là tính đệ quy? Lên hình hiểu trực tiếp, ví dụ ta gọi `reverseKGroup(head, 2)` với linked list này, tức đảo linked list theo nhóm 2 node:

![](https://labuladong.online/algo/images/kgroup/1.jpg)

Nếu tôi tìm cách đảo 2 node đầu, thì những node sau xử lý sao? Những node sau này cũng là một linked list, mà quy mô (độ dài) nhỏ hơn linked list gốc này, đây chính là **bài toán con**.

![](https://labuladong.online/algo/images/kgroup/2.jpg)

Ta có thể di chuyển con trỏ `head` gốc tới đầu đoạn linked list sau này, rồi tiếp tục gọi đệ quy `reverseKGroup(head, 2)`, vì bài toán con (phần linked list sau) và bài gốc (cả linked list) cấu trúc hoàn toàn giống nhau, đây chính là tính đệ quy.

Phát hiện tính đệ quy, là được quy trình thuật toán đại khái:

**1, Trước đảo `k` phần tử đầu `head`**.

![](https://labuladong.online/algo/images/kgroup/3.jpg)

**2, Lấy phần tử thứ `k + 1` làm `head` gọi đệ quy hàm `reverseKGroup`**.

![](https://labuladong.online/algo/images/kgroup/4.jpg)

**3, Nối kết quả hai quá trình trên lại**.

![](https://labuladong.online/algo/images/kgroup/5.jpg)

Ý tưởng chung là vậy, cuối cùng đáng chú ý là, hàm đệ quy đều có base case, với vấn đề này là gì?

Đề nói, nếu phần tử cuối không đủ `k`, thì giữ nguyên. Đây chính là base case, lát nữa sẽ thể hiện trong code.

### Hai, implement code

Trước hết, ta cần implement một hàm `reverse` đảo phần tử trong một khoảng. Trước đó ta đơn giản hóa một chút, cho node đầu linked list, đảo cả linked list thế nào?

<!-- muliti_language -->
```java
// Đảo linked list lấy a làm node đầu
ListNode reverse(ListNode a) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    while (cur != null) {
        nxt = cur.next;
        // Đảo từng node
        cur.next = pre;
        // Cập nhật vị trí con trỏ
        pre = cur;
        cur = nxt;
    }
    // Trả về node đầu sau đảo
    return pre;
}
```

Quá trình chạy thuật toán như GIF sau::

![](https://labuladong.online/algo/images/kgroup/8.gif)

Lần này dùng ý tưởng lặp để implement, dựa vào animation để hiểu hẳn rất dễ.

"Đảo linked list lấy `a` làm node đầu" thực chất chính là "đảo node giữa `a` tới null", vậy nếu bắt bạn "đảo node giữa `a` tới `b`", bạn biết không?

Chỉ cần sửa chữ ký hàm, và đem `null` trong code trên thành `b` là được:

<!-- muliti_language -->
```java
/** Đảo phần tử khoảng [a, b), chú ý đóng trái mở phải */
ListNode reverse(ListNode a, ListNode b) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    // Sửa điều kiện dừng while là được
    while (cur != b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // Trả về node đầu sau đảo
    return pre;
}
```

Giờ ta đã implement bằng lặp chức năng đảo một phần linked list, tiếp theo cứ theo logic trước viết hàm `reverseKGroup` là được:

<!-- muliti_language -->
```java
ListNode reverseKGroup(ListNode head, int k) {
    if (head == null) return null;
    // Khoảng [a, b) chứa k phần tử chờ đảo
    ListNode a, b;
    a = b = head;
    for (int i = 0; i < k; i++) {
        // Không đủ k, không cần đảo, base case
        if (b == null) return head;
        b = b.next;
    }
    // Đảo k phần tử đầu
    ListNode newHead = reverse(a, b);
    // Đệ quy đảo linked list sau và nối lại
    a.next = reverseKGroup(b, k);
    return newHead;
}
```

Giải thích mấy dòng code sau vòng `for`, chú ý hàm `reverse` đảo khoảng `[a, b)`, nên tình hình thế này:

![](https://labuladong.online/algo/images/kgroup/6.jpg)

Phần đệ quy sẽ không triển khai nữa, cả hàm đệ quy chạy xong chính là kết quả này, hoàn toàn phù hợp đề:

![](https://labuladong.online/algo/images/kgroup/7.jpg)

<visual slug='reverse-nodes-in-k-group'/>

### Ba, nói hai câu cuối

Xét lượng đọc, bài thuật toán liên quan cấu trúc dữ liệu cơ bản người xem đều không nhiều, tôi muốn nói như vậy là sẽ chịu thiệt.

Mọi người thích xem vấn đề quy hoạch động, có thể vì phỏng vấn rất hay gặp, nhưng theo hiểu cá nhân tôi, nhiều tư tưởng thuật toán đều bắt nguồn từ cấu trúc dữ liệu. Một trong những tác phẩm thành danh của kênh chúng ta, [tư duy framework học cấu trúc dữ liệu](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/) từng đề cập, cái gì động quy, quay lui, chia trị, thực ra đều là duyệt cây, cấu trúc cây này nó chẳng phải là một linked list đa chạc sao? Bạn xử lý được vấn đề cấu trúc dữ liệu cơ bản, giải bài thuật toán thông thường hẳn cũng không quá tốn sức.

Vậy phân giải vấn đề, phát hiện tính đệ quy thế nào? Cái này chỉ có thể luyện nhiều, tôi ở khóa tinh phẩm cấu trúc dữ liệu có giảng [implement đệ quy linked list đơn](https://aep.h5.xeknow.com/s/1RQzXc), hẳn giúp bạn hiểu sâu thêm về đệ quy.



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Đông ca đưa bạn làm cây nhị phân (phần ý tưởng)](https://labuladong.online/algo/data-structure/binary-tree-part1/)
 - [Công thức "lấy điểm" thi viết thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)
 - [Ma thuật đệ quy: đảo linked list đơn](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng ý tưởng giải:</strong>

| LeetCode | LeetCode CN |
| :----: | :----: |
| [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/?show=1) | [24. Đổi hai hai node trong linked list](https://leetcode.cn/problems/swap-nodes-in-pairs/?show=1) |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》 đã xuất bản, theo dõi kênh WeChat chính thức để xem chi tiết; nhắn tin tới hộp thư "**bộ toàn tập**" có thể tải PDF đi kèm và bộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======

[25.Đảo linked list theo nhóm K](https://leetcode-cn.com/problems/reverse-nodes-in-k-group)

### javascript

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */


// Ví dụ một: đảo linked list lấy a làm node đầu
let reverse = function (a) {
    let pre, cur, nxt;
    pre = null;
    cur = a;
    nxt = a;
    while (cur != null) {
        nxt = cur.next;
        // Đảo từng node
        cur.next = pre;
        // Cập nhật vị trí con trỏ
        pre = cur;
        cur = nxt;
    }
    // Trả về node đầu sau đảo
    return pre;
}

/** Đảo phần tử khoảng [a, b), chú ý đóng trái mở phải */
let reverse = (a, b) => {
    let pre, cur, nxt;
    pre = null;
    cur = a;
    nxt = a;
    // Sửa điều kiện dừng while là được
    while (cur !== b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // Trả về node đầu sau đảo
    return pre;
}


/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
let reverseKGroup = (head, k) => {
    if (head == null) return null;
    // Khoảng [a, b) chứa k phần tử chờ đảo
    let a, b;
    a = b = head;
    for (let i = 0; i < k; i++) {
        // Không đủ k, không cần đảo, base case
        if(b==null) return head;
        b = b.next;
    }

    // Đảo k phần tử đầu
    let newHead = reverse(a,b);

    // Đệ quy đảo linked list sau và nối lại
    a.next = reverseKGroup(b,k);
    return newHead;
}
```
