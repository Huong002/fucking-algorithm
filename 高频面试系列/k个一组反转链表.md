# Đảo linked list theo nhóm k

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên web bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn user cũ~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được套路 thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | 力扣 | Độ khó |
| :----: | :----: | :----: |
| [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | [25. Đảo linked list theo nhóm K](https://leetcode.cn/problems/reverse-nodes-in-k-group/) | 🔴 |

**-----------**

Bài trước [đệ quy đảo một phần linked list](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/)讲 cách đệ quy đảo một phần linked list, có bạn hỏi đảo linked list bằng lặp thế nào, vậy phần một bài này sẽ讲 đảo linked list đơn bằng lặp.

Có hàm đảo này xong, ta vẫn sẽ dùng cách đệ quy giải LeetCode 25 「đảo linked list theo nhóm K」, nên lúc kiểm tra tư duy đệ quy của bạn tới, chuẩn bị xong chưa?

Xem trước đề, không khó hiểu:

<Problem slug="reverse-nodes-in-k-group" />

Vấn đề này hay thấy trong面经, mà độ khó trên LeetCode là Hard, nó thật难 vậy sao?

Với bài thuật toán cấu trúc dữ liệu cơ bản其实都不难, chỉ cần结合 đặc điểm拆解 phân tích từng chút,一般都没难点. Dưới đây ta拆解 vấn đề này.

### Một, phân tích vấn đề

Trước, bài trước [tư duy framework học cấu trúc dữ liệu](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/)提到, linked list là cấu trúc dữ liệu兼具 tính đệ quy và lặp, nghĩ kỹ phát hiện **vấn đề này có tính đệ quy**.

Cái gì叫 tính đệ quy? Lên图 hiểu thẳng, ví dụ ta gọi `reverseKGroup(head, 2)` với linked list này, tức đảo linked list theo nhóm 2 node:

![](https://labuladong.online/algo/images/kgroup/1.jpg)

Nếu tôi设法 đảo 2 node đầu, thì những node sau xử sao? Những node sau này cũng là một linked list, mà quy mô (độ dài) nhỏ hơn linked list gốc này, đây就叫 **bài toán con**.

![](https://labuladong.online/algo/images/kgroup/2.jpg)

Ta có thể di con trỏ `head` gốc tới đầu đoạn linked list sau này, rồi tiếp tục gọi đệ quy `reverseKGroup(head, 2)`, vì bài toán con (phần linked list sau) và bài gốc (cả linked list) cấu trúc完全 giống, đây chính là tính đệ quy.

Phát hiện tính đệ quy, là được流程 thuật toán đại khái:

**1, Trước đảo `k` phần tử đầu `head`**.

![](https://labuladong.online/algo/images/kgroup/3.jpg)

**2, Lấy phần tử thứ `k + 1` làm `head` gọi đệ quy hàm `reverseKGroup`**.

![](https://labuladong.online/algo/images/kgroup/4.jpg)

**3, Nối kết quả hai quá trình trên lại**.

![](https://labuladong.online/algo/images/kgroup/5.jpg)

思路 tổng就是 vậy, cuối đáng chú ý là, hàm đệ quy đều có base case, với vấn đề này là gì?

Đề nói, nếu phần tử cuối không đủ `k`,就 giữ nguyên. Đây chính là base case, lát sẽ thể hiện trong code.

### Hai, implement code

Trước, ta要 implement một hàm `reverse` đảo phần tử trong một khoảng. Trước đó ta简化 một chút, cho node đầu linked list, đảo cả linked list thế nào?

<!-- muliti_language -->
```java
// 反转以 a 为头结点的链表 -> Đảo linked list lấy a làm node đầu
ListNode reverse(ListNode a) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    while (cur != null) {
        nxt = cur.next;
        // 逐个结点反转 -> Đảo từng node
        cur.next = pre;
        // 更新指针位置 -> Cập nhật vị trí con trỏ
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点 -> Trả về node đầu sau đảo
    return pre;
}
```

Quá trình chạy thuật toán như GIF sau::

![](https://labuladong.online/algo/images/kgroup/8.gif)

Lần này dùng思路 lặp để implement,靠 animation hiểu hẳn rất dễ.

「Đảo linked list lấy `a` làm node đầu」其实 chính là 「đảo node giữa `a` tới null」, vậy nếu bắt bạn 「đảo node giữa `a` tới `b`」, bạn biết không?

Chỉ cần sửa chữ ký hàm, và把 `null` trong code trên thành `b` là được:

<!-- muliti_language -->
```java
/** 反转区间 [a, b) 的元素，注意是左闭右开 -> Đảo phần tử khoảng [a, b), chú ý左闭右开 */
ListNode reverse(ListNode a, ListNode b) {
    ListNode pre, cur, nxt;
    pre = null; cur = a; nxt = a;
    // while 终止的条件改一下就行了 -> Sửa điều kiện dừng while là được
    while (cur != b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点 -> Trả về node đầu sau đảo
    return pre;
}
```

Giờ ta lặp implement chức năng đảo một phần linked list, tiếp就按 logic trước viết hàm `reverseKGroup` là được:

<!-- muliti_language -->
```java
ListNode reverseKGroup(ListNode head, int k) {
    if (head == null) return null;
    // 区间 [a, b) 包含 k 个待反转元素 -> Khoảng [a, b) chứa k phần tử chờ đảo
    ListNode a, b;
    a = b = head;
    for (int i = 0; i < k; i++) {
        // 不足 k 个，不需要反转，base case -> Không đủ k, không cần đảo, base case
        if (b == null) return head;
        b = b.next;
    }
    // 反转前 k 个元素 -> Đảo k phần tử đầu
    ListNode newHead = reverse(a, b);
    // 递归反转后续链表并连接起来 -> Đệ quy đảo linked list sau và nối lại
    a.next = reverseKGroup(b, k);
    return newHead;
}
```

Giải thích mấy dòng code sau vòng `for`, chú ý hàm `reverse` đảo khoảng `[a, b)`, nên tình hình thế này:

![](https://labuladong.online/algo/images/kgroup/6.jpg)

Phần đệ quy就不展开, cả hàm đệ quy xong就是 kết quả này,完全符合 đề:

![](https://labuladong.online/algo/images/kgroup/7.jpg)

<visual slug='reverse-nodes-in-k-group'/>

### Ba, nói hai câu cuối

Xét lượng đọc, bài thuật toán liên quan cấu trúc dữ liệu cơ bản người xem都不多, tôi muốn nói đây là要吃亏.

Mọi người thích xem vấn đề quy hoạch động, có thể vì phỏng vấn rất hay gặp, nhưng theo hiểu cá nhân tôi, nhiều tư tưởng thuật toán đều源于 cấu trúc dữ liệu. Tác phẩm thành danh之一 của公众号 ta, [tư duy framework học cấu trúc dữ liệu](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/)就提过, cái gì动规,回溯,分治 thuật toán,其实 đều là duyệt cây, cấu trúc cây này nó chẳng phải là một linked list đa chạc sao? Bạn能 xử vấn đề cấu trúc dữ liệu cơ bản, giải bài thuật toán一般 hẳn cũng không quá费事.

Vậy phân解 vấn đề, phát hiện tính đệ quy thế nào? Cái này chỉ能多 luyện, tôi ở khóa精品 cấu trúc dữ liệu讲 [implement đệ quy linked list đơn](https://aep.h5.xeknow.com/s/1RQzXc), hẳn giúp bạn加深 hiểu đệ quy thêm.



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Đông ca带 bạn刷 cây nhị phân (思路篇)](https://labuladong.online/algo/data-structure/binary-tree-part1/)
 - [套路「lấy điểm」笔试 thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)
 - [Ma thuật đệ quy: đảo linked list đơn](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin刷题 Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng思路 giải:</strong>

| LeetCode | 力扣 |
| :----: | :----: |
| [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/?show=1) | [24. Đổi hai hai node trong linked list](https://leetcode.cn/problems/swap-nodes-in-pairs/?show=1) |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》 đã xuất bản, follow公众号 xem chi tiết; trả lời后台 「**全家桶**」 có thể tải PDF配套 và全家桶刷题**:

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


// 示例一：反转以a为头结点的链表 -> Ví dụ một: đảo linked list lấy a làm node đầu
let reverse = function (a) {
    let pre, cur, nxt;
    pre = null;
    cur = a;
    nxt = a;
    while (cur != null) {
        nxt = cur.next;
        // 逐个结点反转 -> Đảo từng node
        cur.next = pre;
        // 更新指针位置 -> Cập nhật vị trí con trỏ
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点 -> Trả về node đầu sau đảo
    return pre;
}

/** 反转区间 [a, b) 的元素，注意是左闭右开 -> Đảo phần tử khoảng [a, b), chú ý左闭右开 */
let reverse = (a, b) => {
    let pre, cur, nxt;
    pre = null;
    cur = a;
    nxt = a;
    // while 终止的条件改一下就行了 -> Sửa điều kiện dừng while là được
    while (cur !== b) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    // 返回反转后的头结点 -> Trả về node đầu sau đảo
    return pre;
}


/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
let reverseKGroup = (head, k) => {
    if (head == null) return null;
    // 区间 [a, b) 包含 k 个待反转元素 -> Khoảng [a, b) chứa k phần tử chờ đảo
    let a, b;
    a = b = head;
    for (let i = 0; i < k; i++) {
        // 不足k个，不需反转，base case -> Không đủ k, không cần đảo, base case
        if(b==null) return head;
        b = b.next;
    }

    // 反转前k个元素 -> Đảo k phần tử đầu
    let newHead = reverse(a,b);

    // 递归反转后续链表并连接起来 -> Đệ quy đảo linked list sau và nối lại
    a.next = reverseKGroup(b,k);
    return newHead;
}
```
