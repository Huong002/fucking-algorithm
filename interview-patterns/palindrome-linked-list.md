# Cách kiểm tra linked list palindrome



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) | [234. Linked list palindrome](https://leetcode.cn/problems/palindrome-linked-list/) | 🟢 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
> - [Cơ bản linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
 > - [Kỹ thuật con trỏ đôi linked list](https://labuladong.online/algo/essential-technique/linked-list-skills-summary/)
[Tổng hợp kỹ thuật con trỏ đôi mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/)

Bài trước [tổng hợp kỹ thuật con trỏ đôi mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/) giảng chuỗi palindrome và vấn đề dãy palindrome, trước hết ôn đơn giản.

**Tìm** chuỗi palindrome, ý tưởng cốt lõi là mở rộng từ trung tâm ra hai đầu:

```java
// Tìm trong s chuỗi palindrome dài nhất lấy s[left] và s[right] làm trung tâm
String palindrome(String s, int left, int right) {
    // Tránh vượt biên chỉ số
    while (left >= 0 && right < s.length()
            && s.charAt(left) == s.charAt(right)) {
        // Hai con trỏ, mở ra hai bên
        left--;
        right++;
    }
    // Trả về chuỗi palindrome dài nhất lấy s[left] và s[right] làm trung tâm
    return s.substring(left + 1, right);
}
```

Vì độ dài chuỗi palindrome có thể lẻ cũng có thể chẵn, dài lẻ chỉ tồn tại một điểm trung tâm, còn dài chẵn tồn tại hai điểm trung tâm, nên hàm trên cần truyền `l` và `r`.

Còn **kiểm tra** một chuỗi có phải chuỗi palindrome không thì đơn giản hơn nhiều, không cần xét chẵn lẻ, chỉ cần [kỹ thuật hai con trỏ](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/), xích lại từ hai đầu vào giữa là được:

```java
boolean isPalindrome(String s) {
    // Hai con trỏ trái phải đi ngược nhau
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

Code trên rất dễ hiểu phải không, **vì chuỗi palindrome đối xứng, nên đọc xuôi và đọc ngược hẳn giống nhau, đặc điểm này là mấu chốt để giải vấn đề chuỗi palindrome**.

Dưới đây mở rộng trường hợp đơn giản nhất này, để giải: làm sao kiểm tra một "linked list đơn" có phải palindrome.

## Một, kiểm tra linked list đơn có phải palindrome

Xem LeetCode 234 "linked list palindrome":

<Problem slug="palindrome-linked-list" />

Chữ ký hàm như sau:

```java
boolean isPalindrome(ListNode head);
```

Mấu chốt của đề này nằm ở việc, linked list đơn không thể duyệt ngược, không thể dùng kỹ thuật hai con trỏ.

Vậy cách đơn giản nhất chính là, đảo linked list gốc rồi lưu vào một linked list mới, rồi so hai linked list này có giống nhau không. Về cách đảo linked list, xem bài trước [đệ quy đảo một phần linked list](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/).

Tôi ở [tư duy framework học cấu trúc dữ liệu](https://labuladong.online/algo/essential-technique/algorithm-summary/) từng nói, linked list có cấu trúc đệ quy, cấu trúc cây chẳng qua là dẫn xuất của linked list. Vậy, **linked list thực ra cũng có thể có duyệt tiền thứ và hậu thứ, dựa vào ý tưởng duyệt hậu thứ của cây nhị phân, không cần một cách tường minh đảo linked list gốc cũng có thể duyệt ngược linked list**:

```java
// Framework duyệt cây nhị phân
void traverse(TreeNode root) {
    // Code duyệt tiền thứ
    traverse(root.left);
    // Code duyệt trung thứ
    traverse(root.right);
    // Code duyệt hậu thứ
}

// Duyệt đệ quy linked list đơn
void traverse(ListNode head) {
    // Code duyệt tiền thứ
    traverse(head.next);
    // Code duyệt hậu thứ
}
```

Framework này có ý nghĩa chỉ đạo gì? Nếu tôi muốn in xuôi giá trị `val` trong linked list, có thể viết code ở vị trí duyệt tiền thứ; ngược lại, nếu muốn duyệt ngược linked list, là có thể thao tác ở vị trí duyệt hậu thứ:

```java
// In ngược giá trị phần tử trong linked list đơn
void traverse(ListNode head) {
    if (head == null) return;
    traverse(head.next);
    // Code duyệt hậu thứ
    print(head.val);
}
```

Nói tới đây, thực ra có thể sửa một chút, mô phỏng hai con trỏ để implement chức năng kiểm tra palindrome:

```java
class Solution {
    // Con trỏ di từ trái sang phải
    ListNode left;
    // Con trỏ di từ phải sang trái
    ListNode right;

    // Ghi lại linked list có phải palindrome không
    boolean res = true;

    boolean isPalindrome(ListNode head) {
        left = head;
        traverse(head);
        return res;
    }

    void traverse(ListNode right) {
        if (right == null) {
            return;
        }

        // Lợi dụng đệ quy, đi tới đuôi linked list
        traverse(right.next);

        // Vị trí duyệt hậu thứ, con trỏ right lúc này trỏ đuôi phải linked list
        // Nên có thể so với con trỏ left để kiểm tra có phải linked list palindrome không
        if (left.val != right.val) {
            res = false;
        }
        left = left.next;
    }
}
```

Logic cốt lõi làm vậy là gì? **Thực tế chính là bỏ node linked list vào một stack, rồi lấy ra, lúc này thứ tự phần tử chính là ngược**, chỉ là ta lợi dụng ngăn xếp hàm đệ quy mà thôi.

<visual slug='is-palindrome' >

Bạn có thể mở panel trực quan dưới, click nhiều lần dòng code <code type="click">if (right === null)</code>, là thấy con trỏ `right` lợi dụng ngăn xếp đệ quy đi tới đuôi linked list, rồi click nhiều lần dòng code <code type="click">left = left.next;</code>, là thấy `left` tiến, con trỏ `right` lùi, đi ngược nhau, cuối cùng hoàn thành kiểm tra palindrome:

</visual>

Đương nhiên, dù tạo một linked list đảo hay lợi dụng duyệt hậu thứ, thời gian và không gian thuật toán đều là O(N). Dưới đây ta nghĩ xem, có thể không dùng không gian phụ mà giải vấn đề này không?

## Hai, tối ưu độ phức tạp không gian

Ý tưởng tốt hơn là thế này:

**1, Trước hết qua con trỏ nhanh chậm trong [kỹ thuật con trỏ đôi linked list](https://labuladong.online/algo/essential-technique/linked-list-skills-summary/) tìm điểm giữa linked list**:

```java
ListNode slow, fast;
slow = fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// Con trỏ slow giờ trỏ điểm giữa linked list
```

![](https://labuladong.online/algo/images/palindrome-list/1.jpg)



**2, Nếu con trỏ `fast` không trỏ `null`, nghĩa là độ dài linked list lẻ, `slow` còn phải tiến thêm một bước**:

```java
if (fast != null)
    slow = slow.next;
```

![](https://labuladong.online/algo/images/palindrome-list/2.jpg)

**3, Đảo linked list sau bắt đầu từ `slow`, giờ là có thể bắt đầu so chuỗi palindrome**:

```java
ListNode left = head;
ListNode right = reverse(slow);

while (right != null) {
    if (left.val != right.val)
        return false;
    left = left.next;
    right = right.next;
}
return true;
```

![](https://labuladong.online/algo/images/palindrome-list/3.jpg)



Tới đây, gộp 3 đoạn code trên lại là hiệu quả giải được vấn đề này, trong đó hàm `reverse` xem [đảo linked list đơn](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/):

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow, fast;
        slow = fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        if (fast != null)
            slow = slow.next;
        
        ListNode left = head;
        ListNode right = reverse(slow);
        while (right != null) {
            if (left.val != right.val)
                return false;
            left = left.next;
            right = right.next;
        }
        
        return true;
    }

    ListNode reverse(ListNode head) {
        ListNode pre = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```

Quá trình thuật toán như GIF sau:

![](https://labuladong.online/algo/images/kgroup/8.gif)

<visual slug='palindrome-linked-list'>

Bạn có thể mở panel trực quan dưới, click nhiều lần dòng code <code type="click">while (right != null)</code>, là thấy con trỏ `left` và `right` đi ngược nhau, cuối cùng hoàn thành kiểm tra palindrome:

</visual>

Thời gian tổng thuật toán O(N), không gian O(1), đã tối ưu nhất.

Tôi biết chắc chắn có bạn hỏi: cách giải này dù hiệu quả, nhưng phá cấu trúc gốc của linked list input, có thể tránh khiếm khuyết này không?

Thực ra vấn đề này rất dễ giải, mấu chốt nằm ở việc lấy được vị trí hai con trỏ `p, q` này:

![](https://labuladong.online/algo/images/palindrome-list/4.jpg)

Như vậy, chỉ cần trước khi hàm return thêm một đoạn code là khôi phục thứ tự linked list ban đầu:

```java
p.next = reverse(q);
```

Do giới hạn độ dài, tôi sẽ không viết nữa, bạn có thể tự thử.

## Ba, tổng kết

Trước hết, tìm chuỗi palindrome là mở từ giữa ra hai đầu, kiểm tra chuỗi palindrome là co từ hai đầu vào giữa. Với linked list đơn, không thể trực tiếp duyệt ngược, có thể tạo một linked list đảo mới, có thể lợi dụng duyệt hậu thứ của linked list, cũng có thể dùng stack để xử lý ngược thứ tự linked list đơn.

Cụ thể tới vấn đề kiểm tra linked list palindrome, vì tính đặc thù của palindrome, có thể không đảo hoàn toàn linked list, mà chỉ đảo một phần linked list, đem độ phức tạp không gian xuống O(1).




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm chỉ Offer II 027. Linked list palindrome](https://leetcode.cn/problems/aMhZSa/?show=1) | 🟢 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
