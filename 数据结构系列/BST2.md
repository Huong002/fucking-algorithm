# Tâm pháp Cây tìm kiếm nhị phân BST (Phần thao tác cơ bản)




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình速成](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được套路thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | 力扣 | Độ khó |
| :----: | :----: | :----: |
| [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/) | [450. Xóa Node trong cây tìm kiếm nhị phân](https://leetcode.cn/problems/delete-node-in-a-bst/) | 🟠 |
| [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/) | [700. Tìm kiếm trong cây tìm kiếm nhị phân](https://leetcode.cn/problems/search-in-a-binary-search-tree/) | 🟢 |
| [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/) | [701. Thao tác chèn trong cây tìm kiếm nhị phân](https://leetcode.cn/problems/insert-into-a-binary-search-tree/) | 🟠 |
| [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) | [98. Kiểm chứng cây tìm kiếm nhị phân](https://leetcode.cn/problems/validate-binary-search-tree/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về cấu trúc cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)

Bài trước [Tâm pháp cây tìm kiếm nhị phân (Phần đặc tính)](https://labuladong.online/algo/data-structure/bst-part1/) đã giới thiệu đặc tính cơ bản của BST, còn lợi dụng đặc tính「duyệt trung序 có thứ tự」của cây tìm kiếm nhị phân để giải mấy bài, bài này cài đặt các thao tác cơ bản của BST: kiểm tra tính hợp lệ, thêm, xóa, tìm. Trong đó「xóa」và「kiểm tra hợp lệ」hơi phức tạp.

Thao tác cơ bản của BST chủ yếu dựa vào đặc tính「trái nhỏ phải lớn」, có thể làm thao tác tìm kiếm nhị phân tương tự trong cây nhị phân, hiệu suất tìm một phần tử rất cao. Ví dụ dưới đây chính là một cây nhị phân hợp lệ:

![](https://labuladong.online/algo/images/bst/0.png)

Với vấn đề liên quan BST, bạn có thể thường thấy logic code kiểu sau:

```java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // Tìm được mục tiêu, làm gì đó
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```

Khung code này thực ra gần giống khung duyệt cây nhị phân, chẳng qua là lợi dụng đặc tính trái nhỏ phải lớn của BST mà thôi. Tiếp theo xem thao tác cơ bản của cấu trúc BST cài đặt thế nào.

## Một, kiểm tra tính hợp lệ của BST

Bài 98 trên LeetCode「Kiểm chứng cây tìm kiếm nhị phân」bắt bạn判断BST nhập vào có hợp lệ không:

<Problem slug="validate-binary-search-tree" />

Chú ý, ở đây có坑nhé. Theo đặc tính trái nhỏ phải lớn của BST, mỗi Node muốn判断mình có phải Node BST hợp lệ không, việc要làm không phải là so sánh mình với con trái/phải sao? Cảm giác nên viết code thế này:

```java
boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    // Bên trái của root phải nhỏ hơn
    if (root.left != null && root.left.val >= root.val)
        return false;
    // Bên phải của root phải lớn hơn
    if (root.right != null && root.right.val <= root.val)
        return false;

    return isValidBST(root.left)
        && isValidBST(root.right);
}
```

Nhưng thuật toán này sai rồi, mỗi Node của BST phải nhỏ hơn **toàn bộ** Node của cây con phải, cây nhị phân dưới đây rõ ràng không phải BST, vì trong cây con phải của Node 10 có một Node 6, nhưng thuật toán của chúng ta sẽ判定nó là BST hợp lệ:

![](https://labuladong.online/algo/images/bst/假BST.png)

**Nguyên nhân lỗi là, với mỗi Node `root`, code chỉ kiểm tra Node con trái/phải của nó có符合nguyên tắc trái nhỏ phải lớn không; nhưng theo định nghĩa BST, toàn bộ cây con trái của `root` đều phải nhỏ hơn `root.val`, toàn bộ cây con phải đều phải lớn hơn `root.val`**.

Vấn đề là, với một Node `root` nào đó, nó chỉ quản được Node con trái/phải của mình, làm sao truyền ràng buộc của `root` cho cây con trái/phải? Xem code đúng:

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return _isValidBST(root, null, null);
    }

    // Định nghĩa: hàm này trả về mọi Node của cây con gốc root có thỏa mãn max.val > root.val > min.val không
    public boolean _isValidBST(TreeNode root, TreeNode min, TreeNode max) {
        // base case
        if (root == null) return true;
        // Nếu root.val không符合giới hạn của max và min,说明không phải BST hợp lệ
        if (min != null && root.val <= min.val) return false;
        if (max != null && root.val >= max.val) return false;
        // Theo định nghĩa, giới hạn giá trị lớn nhất của cây con trái là root.val, giá trị nhỏ nhất của cây con phải là root.val
        return _isValidBST(root.left, min, root) 
            && _isValidBST(root.right, root, max);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/validate-binary-search-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>



Chúng ta dùng hàm phụ trợ, tăng danh sách tham số hàm, mang thông tin thêm trong tham số, truyền ràng buộc này cho mọi Node của cây con, đây cũng là một tiểu xảo của thuật toán cây nhị phân.

## Tìm kiếm phần tử trong BST

Bài 700 trên LeetCode「Tìm kiếm trong cây tìm kiếm nhị phân」bắt bạn tìm Node có giá trị `target` trong BST, chữ ký hàm như sau:

```java
TreeNode searchBST(TreeNode root, int target);
```

Nếu là tìm trong một cây nhị phân thường, có thể viết code thế này:

```java
TreeNode searchBST(TreeNode root, int target) {
    if (root == null) return null;
    if (root.val == target) return root;
    // Node hiện tại chưa tìm được thì đệ quy去cây con trái/phải tìm
    TreeNode left = searchBST(root.left, target);
    TreeNode right = searchBST(root.right, target);

    return left != null ? left : right;
}
```

Viết vậy hoàn toàn đúng, nhưng đoạn code này tương đương liệt kê mọi Node, áp dụng cho mọi cây nhị phân. Vậy làm sao phát huy充分đặc thù của BST, dùng đặc tính「trái nhỏ phải lớn」?

Rất đơn giản, thực ra không cần đệ quy tìm cả hai bên, tư tưởng giống tìm kiếm nhị phân, dựa vào so sánh `target` và `root.val` là có thể loại một bên. Sửa chút思路trên:

```java
TreeNode searchBST(TreeNode root, int target) {
    if (root == null) {
        return null;
    }
    // Đi tìm ở cây con trái
    if (root.val > target) {
        return searchBST(root.left, target);
    }
    // Đi tìm ở cây con phải
    if (root.val < target) {
        return searchBST(root.right, target);
    }
    // Node hiện tại chính là giá trị mục tiêu
    return root;
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/search-in-a-binary-search-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 Animation trực quan hóa code👾</strong>
</summary>
</details>
</a>
<hr/>



## Chèn một số vào BST

Thao tác với cấu trúc dữ liệu chẳng qua là duyệt + truy cập, duyệt chính là「tìm」, truy cập chính là「sửa」. Cụ thể với vấn đề này, chèn một số, là tìm vị trí chèn trước, rồi thực hiện thao tác chèn.

Vì BST thường không tồn tại Node trùng giá trị, nên chúng ta thường không chèn giá trị đã tồn tại vào BST. **Code dưới đây đều mặc định không chèn giá trị đã tồn tại vào BST**.

Bài trước, chúng ta tổng kết khung duyệt trong BST, chính là vấn đề「tìm」. Áp khung trực tiếp, cộng thêm thao tác「sửa」là được.

**Một khi liên quan「sửa」,就类似vấn đề dựng cây nhị phân, hàm phải trả về kiểu `TreeNode`, và phải nhận giá trị trả về của gọi đệ quy**.

Bài 701 trên LeetCode「Thao tác chèn trong cây tìm kiếm nhị phân」chính là vấn đề này:

<Problem slug="insert-into-a-binary-search-tree" />

Xem thẳng code解法, có thể kết hợp chú thích và panel trực quan để hiểu:

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            // Tìm được vị trí trống để chèn Node mới
            return new TreeNode(val);
        }

        // Đi tìm vị trí chèn ở cây con phải
        if (root.val < val) {
            root.right = insertIntoBST(root.right, val);
        }
        // Đi tìm vị trí chèn ở cây con trái
        if (root.val > val) {
            root.left = insertIntoBST(root.left, val);
        }
        // Trả về root, đệ quy tầng trên sẽ nhận giá trị trả về làm Node con
        return root;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/insert-into-a-binary-search-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>



## Ba, xóa một số trong BST

Bài 450 trên LeetCode「Xóa Node trong cây tìm kiếm nhị phân」bắt bạn xóa một Node có giá trị `key` trong BST:

<Problem slug="delete-node-in-a-bst" />

Vấn đề này hơi phức tạp, giống thao tác chèn, tìm「tìm」rồi「sửa」, viết khung ra trước rồi nói:

```java
TreeNode deleteNode(TreeNode root, int key) {
    if (root.val == key) {
        // Tìm được rồi, tiến hành xóa
    } else if (root.val > key) {
        // Đi tìm ở cây con trái
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        // Đi tìm ở cây con phải
        root.right = deleteNode(root.right, key);
    }
    return root;
}
```

Tìm được Node mục tiêu rồi, ví dụ là Node `A`, xóa Node này thế nào, đây là khó. Vì khi xóa Node đồng thời không được phá tính chất BST. Có ba trường hợp, dùng hình để说明.

**Trường hợp 1**: `A`恰好là Node末端, hai Node con đều rỗng, vậy nó có thể去世tại chỗ.

![](https://labuladong.online/algo/images/bst/bst_deletion_case_1.png)

```java
if (root.left == null && root.right == null)
    return null;
```


**Trường hợp 2**: `A` chỉ có một Node con không rỗng, vậy nó phải để đứa con này接替vị trí của mình.

![](https://labuladong.online/algo/images/bst/bst_deletion_case_2.png)

```java
// Sau khi loại trường hợp 1
if (root.left == null) return root.right;
if (root.right == null) return root.left;
```


**Trường hợp 3**: `A` có hai Node con, phiền rồi, để không phá tính chất BST, `A`必须tìm Node lớn nhất trong cây con trái, hoặc Node nhỏ nhất trong cây con phải来接替mình. Chúng ta giảng theo cách thứ hai.

![](https://labuladong.online/algo/images/bst/bst_deletion_case_3.png)

```java
if (root.left != null && root.right != null) {
    // Tìm Node nhỏ nhất của cây con phải
    TreeNode minNode = getMin(root.right);
    // Biến root thành minNode
    root.val = minNode.val;
    // Chuyển去xóa minNode
    root.right = deleteNode(root.right, minNode.val);
}
```


Phân tích xong ba trường hợp, điền vào khung, đơn giản hóa code:

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (root.val == key) {
            // Hai if này xử lý đúng cả trường hợp 1 và 2
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            // Xử lý trường hợp 3
            // Lấy Node nhỏ nhất của cây con phải
            TreeNode minNode = getMin(root.right);
            // Xóa Node nhỏ nhất của cây con phải
            root.right = deleteNode(root.right, minNode.val);
            // Dùng Node nhỏ nhất của cây con phải thay Node root
            minNode.left = root.left;
            minNode.right = root.right;
            root = minNode;
        } else if (root.val > key) {
            root.left = deleteNode(root.left, key);
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
        }
        return root;
    }

    TreeNode getMin(TreeNode node) {
        // Trái nhất của BST chính là nhỏ nhất
        while (node.left != null) node = node.left;
        return node;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/delete-node-in-a-bst/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🥳 Animation trực quan hóa code🥳</strong>
</summary>
</details>
</a>
<hr/>



Như vậy, thao tác xóa hoàn thành. Chú ý, code trên khi xử lý trường hợp 3 đã hoán đổi hai Node `root` và `minNode` qua một loạt thao tác linked list hơi phức tạp:

```java
// Xử lý trường hợp 3
// Lấy Node nhỏ nhất của cây con phải
TreeNode minNode = getMin(root.right);
// Xóa Node nhỏ nhất của cây con phải
root.right = deleteNode(root.right, minNode.val);
// Dùng Node nhỏ nhất của cây con phải thay Node root
minNode.left = root.left;
minNode.right = root.right;
root = minNode;
```


Có độc giả sẽ thắc mắc, thay Node `root` sao phiền vậy, sửa thẳng trường `val` không phải được sao? Nhìn còn gọn dễ hiểu hơn:

```java
// Xử lý trường hợp 3
// Lấy Node nhỏ nhất của cây con phải
TreeNode minNode = getMin(root.right);
// Xóa Node nhỏ nhất của cây con phải
root.right = deleteNode(root.right, minNode.val);
// Dùng Node nhỏ nhất của cây con phải thay Node root
root.val = minNode.val;
```


Chỉ riêng với bài thuật toán này thì được, nhưng thao tác vậy không hoàn hảo, chúng ta thường không hoán đổi Node bằng cách sửa giá trị bên trong Node. Vì trong ứng dụng thực tế, vùng dữ liệu bên trong Node BST do user tự định nghĩa, có thể rất phức tạp, mà BST làm cấu trúc dữ liệu (một工具人), thao tác của nó phải解耦với vùng dữ liệu lưu bên trong, nên chúng ta thiên về dùng thao tác con trỏ để hoán đổi Node, căn bản không cần quan tâm dữ liệu bên trong.

Cuối cùng tóm tắt đơn giản, qua bài này, chúng ta tổng kết mấy技巧sau:

1、Nếu Node hiện tại ảnh hưởng tổng thể tới Node con bên dưới, có thể tăng danh sách tham số qua hàm phụ trợ, mượn tham số truyền thông tin.

2、Nắm phương pháp thêm/xóa/tìm/sửa của BST.

3、Khi đệ quy sửa cấu trúc dữ liệu, cần nhận giá trị trả về của gọi đệ quy, và trả về Node đã sửa.

Bài này đến đây thôi, thêm nhiều bài tập cây nhị phân kinh điển và rèn luyện tư duy đệ quy, xem [Luyện chuyên đề đệ quy](https://labuladong.online/algo/problem-set/bst1/) trong chương cây nhị phân.






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Cài đặt code Trie/Cây字典树/Cây tiền tố](https://labuladong.online/algo/data-structure/trie-implement/)
 - [【Luyện tập tăng cường】Bài tập kinh điển về cây tìm kiếm nhị phân II](https://labuladong.online/algo/problem-set/bst2/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần hậu序)](https://labuladong.online/algo/data-structure/bst-part4/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần dựng cây)](https://labuladong.online/algo/data-structure/bst-part3/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin刷题 Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp思路giải:</strong>

| LeetCode | 力扣 | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm Chỉ Offer 33. Chuỗi duyệt hậu序 của cây tìm kiếm nhị phân](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
