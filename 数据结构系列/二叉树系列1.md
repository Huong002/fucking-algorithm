# Tâm pháp cây nhị phân (Phần ý tưởng)




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/) | [114. Trải cây nhị phân thành linked list](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/) | 🟠 |
| [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) | [116. Điền con trỏ Node phía phải tiếp theo của mỗi Node](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/) | 🟠 |
| [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) | [226. Lật cây nhị phân](https://leetcode.cn/problems/invert-binary-tree/) | 🟢 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về cấu trúc cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
> - [Tâm pháp cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)

> tip: Bài này có bản video: [Tư duy khung cây nhị phân/đệ quy (Phần cương lĩnh)](https://www.bilibili.com/video/BV1nG411x77H/). Khuyên follow tài khoản B đứng của tôi, tôi sẽ dẫn mọi người học kỹ thuật thuật toán hơi khó bằng cách đọc kèm video.



Bài này nối tiếp [Tâm pháp cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/), nhắc lại tổng cương lĩnh giải cây nhị phân ở bài trước:

> [!NOTE]
> Mô thức tư duy cây nhị phân chia hai loại :
>
> **1, Có thể qua duyệt một lần cây nhị phân ra đáp án không**? Nếu được, dùng một hàm `traverse` phối hợp biến ngoài để cài đặt, đây gọi mô thức tư duy「duyệt」.
>
> **2, Có thể định nghĩa một hàm đệ quy, qua đáp án của bài con (cây con) suy ra ra đáp án của bài gốc không**? Nếu được, viết định nghĩa của hàm đệ quy này, và tận dụng giá trị trả về của hàm này, đây gọi mô thức tư duy「phân rã bài toán」.
>
> Dù dùng mô thức tư duy nào, bạn đều cần nghĩ:
>
> **Nếu tách riêng một Node cây nhị phân, nó cần làm việc gì? Cần làm lúc nào (vị trí tiền/trung/hậu thứ tự)**? Node khác không cần bạn lo, hàm đệ quy sẽ giúp bạn chạy thao tác giống nhau trên mọi Node.

Bài này lấy mấy bài tương đối đơn giản ví dụ, dẫn bạn thực hành dùng mấy tổng cương lĩnh này, hiểu tư duy「duyệt」và tư duy「phân rã bài toán」khác và liên hệ thế nào.






## Bài một, lật cây nhị phân

Chúng ta bắt đầu từ bài đơn giản, xem bài 226 trên LeetCode「Lật cây nhị phân」, nhập Node gốc `root` của một cây nhị phân, bắt bạn cả cây đối xứng lật, ví dụ cây nhị phân nhập như sau:

```
     4
   / \
  2 7
 / \ / \
1 3 6 9
```

Thuật toán lật cây nhị phân tại chỗ, khiến cây gốc `root` thành:

```
     4
   / \
  7 2
 / \ / \
9 6 3 1
```

Không khó phát hiện, chỉ cần Node trái/phải của mỗi Node trên cây nhị phân hoán đổi, kết quả cuối chính là cây nhị phân lật hoàn toàn.

Vậy giờ bắt đầu nhẩm đọc tổng cương lĩnh giải cây nhị phân:

**1, Bài này có thể dùng mô thức tư duy「duyệt」giải không**?

Được, tôi viết một hàm `traverse` duyệt mỗi Node, để mỗi Node đảo ngược Node trái/phải là được.

 tách riêng một Node, cần nó làm gì? Để nó hoán đổi Node trái/phải của mình.

Cần làm lúc nào? dường như vị trí tiền/trung/hậu thứ tự đều được.

Tổng hợp, có thể viết code lời giải như sau:

```java
class Solution {
    // Hàm chính
    public TreeNode invertTree(TreeNode root) {
        // Duyệt cây nhị phân, hoán đổi Node con mỗi Node
        traverse(root);
        return root;
    }

    // Hàm duyệt cây nhị phân
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }

        // *** Vị trí tiền thứ tự ***
        // Việc mỗi Node cần làm chính là hoán đổi Node trái/phải của nó
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;

        // Khung duyệt, đi duyệt Node của cây con trái/phải
        traverse(root.left);
        traverse(root.right);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-invert-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>



Bạn chuyển code vị trí tiền thứ tự tới vị trí hậu thứ tự cũng được, nhưng chuyển thẳng tới vị trí trung thứ tự thì không được, cần sửa chút, cái này phải rất dễ xem ra, tôi thì không nói.

Theo lý, bài này đã giải xong, nhưng để so sánh, chúng ta tiếp tục nghĩ xuống.

**2, Bài này có thể dùng mô thức tư duy「phân rã bài toán」giải không**?

Chúng ta thử gán định nghĩa cho hàm `invertTree`:

```java
// Định nghĩa: lật cây nhị phân gốc root này, trả về Node gốc của cây nhị phân lật sau 
TreeNode invertTree(TreeNode root);
```

Rồi nghĩ, với một Node cây nhị phân `x` nào đó chạy `invertTree(x)`, bạn có thể lợi dụng định nghĩa của hàm đệ quy này làm gì?

Tôi có thể dùng `invertTree(x.left)` lật cây con trái của `x` trước, rồi dùng `invertTree(x.right)` lật cây con phải của `x`, cuối cùng hoán đổi cây con trái/phải của `x`, vừa đúng hoàn thành lật cả cây nhị phân gốc `x`, tức hoàn thành định nghĩa của `invertTree(x)`.

Viết thẳng code lời giải :

```java
class Solution {
    // Định nghĩa: lật cây nhị phân gốc root này, trả về Node gốc của cây nhị phân lật sau 
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        // Lợi dụng định nghĩa hàm, lật cây con trái/phải trước
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);

        // Rồi hoán đổi Node con trái/phải
        root.left = right;
        root.right = left;

        // Tự nhất quán với logic định nghĩa: cây nhị phân gốc root đã được lật, trả về root
        return root;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-invert-tree2/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>



 ý tưởng 「phân rã bài toán」này, cốt lõi là bạn cần cho hàm đệ quy một định nghĩa hợp, rồi dùng định nghĩa của hàm để giải thích code của bạn; nếu logic của bạn thành công tự nhất quán, vậy cho thấy thuật toán này đúng.

Tốt rồi, bài này phân tích tới đây, ý tưởng 「duyệt」và「phân rã bài toán」đều giải được, xem bài tiếp.

## Bài hai, điền con trỏ phía phải của Node

Đây là bài 116 trên LeetCode「Điền con trỏ phía phải của mỗi Node cây nhị phân」, xem đề:

<Problem slug="populating-next-right-pointers-in-each-node" />

```java
// Chữ ký hàm
Node connect(Node root);
```

Ý đề chính là mỗi tầng Node của cây nhị phân đều dùng con trỏ `next` nối lại:

![](https://labuladong.online/algo/images/binary-tree-i/1.png)

Mà đề nói, nhập là một「cây nhị phân hoàn hảo」, hình tượng cả cây nhị phân là một tam giác dương, trừ Node nhất phải con trỏ `next` sẽ trỏ tới `null`, phía phải của Node khác chắc chắn có Node kề.

Bài này làm sao? nhẩm đọc tổng cương lĩnh giải cây nhị phân:

**1, Bài này có thể dùng mô thức tư duy「duyệt」giải không**?

Rất hiển nhiên, chắc chắn được.

Việc mỗi Node cần làm cũng rất đơn giản, con trỏ `next` của mình trỏ tới Node phía phải là được.

Có thể bạn sẽ bắt chước bài trước, viết thẳng code sau:

```java
// Hàm duyệt cây nhị phân
void traverse(Node root) {
    if (root == null || root.left == null) {
        return;
    }
    // trỏ tới con trỏ next của Node con trái sang Node con phải
    root.left.next = root.right;

    traverse(root.left);
    traverse(root.right);
}
```

Nhưng, đoạn code này thực có vấn đề lớn, vì nó chỉ nối được hai Node cùng Node cha, xem lại hình này:

![](https://labuladong.online/algo/images/binary-tree-i/1.png)

Node 5 và Node 6 không thuộc cùng Node cha, vậy theo logic của đoạn code này, nó hai thì không cách nào được nối lại, đây không phù hợp ý đề, nhưng vấn đề ở đâu?

**Hàm `traverse` truyền thống là duyệt mọi Node của cây nhị phân, nhưng giờ chúng ta muốn duyệt thực ra là「khe hở」giữa hai Node kề**.

Nên chúng ta có thể trừu tượng trên cơ sở cây nhị phân, bạn xem mỗi ô vuông trong hình thành một Node:

![](https://labuladong.online/algo/images/binary-tree-i/3.png)

**Như vậy, một cây nhị phân được trừu tượng thành một cây tam chạc, mỗi Node trên cây tam chạc chính là hai Node kề của cây nhị phân gốc**.

Giờ, chúng ta chỉ cần cài đặt một hàm `traverse` để duyệt cây tam chạc này, việc mỗi「Node cây tam chạc」cần làm chính là nối hai Node cây nhị phân bên trong mình lại:

```java
class Solution {
    // Hàm chính
    public Node connect(Node root) {
        if (root == null) return null;
        // Duyệt「cây tam chạc」, nối Node kề
        traverse(root.left, root.right);
        return root;
    }

    // Khung duyệt cây tam chạc
    void traverse(Node node1, Node node2) {
        if (node1 == null || node2 == null) {
            return;
        }
        // *** Vị trí tiền thứ tự ***
        // nối hai Node truyền vào lại
        node1.next = node2;
        
        // Nối hai Node con của cùng Node cha
        traverse(node1.left, node1.right);
        traverse(node2.left, node2.right);
        // Nối hai Node con vượt Node cha
        traverse(node1.right, node2.left);
    }
}
```

Như vậy, hàm `traverse` duyệt cả「cây tam chạc」, mọi Node cây nhị phân kề nhau đều nối lại, cũng tránh vấn đề chúng ta xuất hiện trước đó, bài này giải hoàn hảo.

**2, Bài này có thể dùng mô thức tư duy「phân rã bài toán」giải không**?

Ừm, dường như không có ý tưởng đặc biệt tốt, nên bài này không cách nào dùng tư duy「phân rã bài toán」để giải.

## Bài ba, trải cây nhị phân thành linked list

Đây là bài 114 trên LeetCode「Trải cây nhị phân thành linked list」, xem đề:

<Problem slug="flatten-binary-tree-to-linked-list" />

```java
// Chữ ký hàm như sau
void flatten(TreeNode root);
```

**1, Bài này có thể dùng mô thức tư duy「duyệt」giải không**?

Nhìn sơ cảm giác được: duyệt tiền thứ tự cả cây, vừa duyệt vừa dựng một「linked list」là được:

```java
// Node đầu ảo, dummy.right chính là kết quả
TreeNode dummy = new TreeNode(-1);
// Con trỏ dùng dựng linked list
TreeNode p = dummy;

void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    // Vị trí tiền thứ tự 
    p.right = new TreeNode(root.val);
    p = p.right;

    traverse(root.left);
    traverse(root.right);
}
```

Nhưng chú ý chữ ký của hàm `flatten`, kiểu trả về là `void`, tức đề hy vọng chúng ta làm phẳng cây nhị phân tại chỗ thành linked list.

Như vậy, không cách nào qua duyệt cây nhị phân đơn giản để giải bài này.

**2, Bài này có thể dùng mô thức tư duy「phân rã bài toán」giải không**?

Chúng ta thử cho ra định nghĩa của hàm `flatten`:

```java
// Định nghĩa: nhập Node root, rồi cây nhị phân gốc root sẽ được làm phẳng thành một linked list
void flatten(TreeNode root);
```

Có định nghĩa hàm này, theo yêu cầu đề một cây làm phẳng thành một linked list thế nào?

Với một Node `x`, có thể chạy quy trình sau:

1, Dùng `flatten(x.left)` và `flatten(x.right)` làm phẳng cây con trái/phải của `x` trước.

2, Nối cây con phải của `x` xuống dưới cây con trái, rồi cả cây con trái làm cây con phải.

![](https://labuladong.online/algo/images/binary-tree-i/2.jpeg)

Như vậy, cả cây nhị phân gốc `x` thì được làm phẳng, vừa đúng hoàn thành định nghĩa của `flatten(x)`.

Xem thẳng cài đặt code:

```java
class Solution {
    // Định nghĩa: làm phẳng cây gốc root thành linked list
    public void flatten(TreeNode root) {
        // base case
        if (root == null) return;
        
        // Lợi dụng định nghĩa, cây con trái/phải làm phẳng 
        flatten(root.left);
        flatten(root.right);

        // *** Vị trí duyệt hậu thứ tự ***
        // 1, Cây con trái/phải đã được làm phẳng thành một linked list
        TreeNode left = root.left;
        TreeNode right = root.right;
        
        // 2, Lấy cây con trái làm cây con phải
        root.left = null;
        root.right = left;

        // 3, Nối cây con phải gốc vào cuối của cây con phải hiện tại
        TreeNode p = root;
        while (p.right != null) {
            p = p.right;
        }
        p.right = right;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/flatten-binary-tree-to-linked-list/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>



Bạn xem, đây chính là sức hấp dẫn của đệ quy, bạn nói hàm `flatten` làm phẳng cây con trái/phải thế nào?

Không dễ nói rõ, nhưng chỉ cần biết định nghĩa của `flatten` như vậy và lợi dụng định nghĩa này, để mỗi Node làm việc nó phải làm, rồi hàm `flatten` sẽ theo định nghĩa làm việc.

Đến đây, bài này cũng giải xong, ý tưởng đệ quy của bài trước [Lật linked list theo nhóm k](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/) và bài này cũng có điểm tương tự .

Cuối cùng, kết nối lại với đầu cuối, ghi lại lại tổng cương lĩnh giải cây nhị phân.

Mô thức tư duy cây nhị phân chia hai loại :

**1, Có thể qua duyệt một lần cây nhị phân ra đáp án không**? Nếu được, dùng một hàm `traverse` phối hợp biến ngoài để cài đặt, đây gọi mô thức tư duy「duyệt」.

**2, Có thể định nghĩa một hàm đệ quy, qua đáp án của bài con (cây con) suy ra ra đáp án của bài gốc không**? Nếu được, viết định nghĩa của hàm đệ quy này, và tận dụng giá trị trả về của hàm này, đây gọi mô thức tư duy「phân rã bài toán」.

Dù dùng mô thức tư duy nào, bạn đều cần nghĩ:

**Nếu tách riêng một Node cây nhị phân, nó cần làm việc gì? Cần làm lúc nào (vị trí tiền/trung/hậu thứ tự)**? Node khác không cần bạn lo, hàm đệ quy sẽ giúp bạn chạy thao tác giống nhau trên mọi Node.

Hy vọng bạn có thể cảm nhận sâu sắc kỹ, và dùng cho mọi bài cây nhị phân.

Bài này tới đây thôi, thêm nhiều bài tập cây nhị phân kinh điển và rèn luyện tư duy đệ quy, xem [Luyện chuyên đề đệ quy](https://labuladong.online/algo/intro/binary-tree-practice/) trong chương cây nhị phân.






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Dùng đồng thời hai tư duy giải bài ](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài I](https://labuladong.online/algo/problem-set/binary-tree-traverse-i/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài II](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài III](https://labuladong.online/algo/problem-set/binary-tree-traverse-iii/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần dựng cây)](https://labuladong.online/algo/data-structure/bst-part3/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần đặc tính)](https://labuladong.online/algo/data-structure/bst-part1/)
 - [Tâm pháp cây nhị phân (Phần dựng cây)](https://labuladong.online/algo/data-structure/binary-tree-part2/)
 - [Thuật toán chia để trị chi tiết: độ ưu tiên tính](https://labuladong.online/algo/fname.html?fname=分治算法)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm Chỉ Offer 26. Cấu trúc con của cây](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 27. đối xứng Cây nhị phân](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/?show=1) | 🟢 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
