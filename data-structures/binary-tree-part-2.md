# Tâm pháp cây nhị phân (Phần dựng cây)




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | [105. Dựng cây nhị phân từ duyệt tiền thứ tự và trung thứ tự ](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) | 🟠 |
| [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) | [106. Dựng cây nhị phân từ duyệt trung thứ tự và hậu thứ tự ](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) | 🟠 |
| [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/) | [654. Cây nhị phân lớn nhất](https://leetcode.cn/problems/maximum-binary-tree/) | 🟠 |
| [889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) | [889. Dựng cây nhị phân theo duyệt tiền thứ tự và hậu thứ tự ](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-postorder-traversal/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về cấu trúc cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
> - [Tâm pháp cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)

Bài này nối tiếp [Tâm pháp cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/) là bài thứ hai, nhắc lại tổng cương lĩnh giải cây nhị phân ở bài trước:

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

Bài đầu [Tâm pháp cây nhị phân (Phần tư duy)](https://labuladong.online/algo/data-structure/binary-tree-part1/) giảng hai cách tư duy「duyệt」và「phân rã bài toán」, bài này giảng vấn đề loại dựng cây nhị phân.

**Vấn đề dựng cây nhị phân thường đều dùng ý tưởng 「phân rã bài toán」: dựng cả cây = Node gốc + dựng cây con trái + dựng cây con phải**.

Tiếp theo xem thẳng bài.

## Dựng cây nhị phân lớn nhất

Trước một bài đơn giản, đây là bài 654 trên LeetCode「Cây nhị phân lớn nhất」, đề như sau:

<Problem slug="maximum-binary-tree" />

```java
// Chữ ký hàm như sau
TreeNode constructMaximumBinaryTree(int[] nums);
```

Mỗi Node cây nhị phân đều có thể coi là Node gốc của một cây con, với Node gốc, đầu tiên cần làm dĩ nhiên là muốn cách mình dựng ra trước, rồi muốn cách dựng cây con trái/phải của mình.

Nên, chúng ta cần duyệt mảng tìm giá trị lớn nhất `maxVal`, nhờ đó Node gốc `root` làm ra, rồi với mảng bên trái `maxVal` và mảng bên phải đệ quy dựng, làm cây con trái/phải của `root`.

 theo ví dụ đề cho, mảng nhập là `[3,2,1,6,0,5]`, với Node gốc của cả cây mà nói, thực ra đang làm việc này:

```java
TreeNode constructMaximumBinaryTree([3,2,1,6,0,5]) {
    // Tìm giá trị lớn nhất trong mảng
    TreeNode root = new TreeNode(6);
    // Đệ quy gọi dựng cây con trái/phải
    root.left = constructMaximumBinaryTree([3,2,1]);
    root.right = constructMaximumBinaryTree([0,5]);
    return root;
}

// Giá trị lớn nhất trong nums hiện tại chính là Node gốc, rồi dựa vào index đệ quy gọi mảng trái/phải dựng cây con trái/phải là được
// Chi tiết hơn, chính là mã giả sau
TreeNode constructMaximumBinaryTree(int[] nums) {
    if (nums is empty) return null;
    // Tìm giá trị lớn nhất trong mảng
    int maxVal = Integer.MIN_VALUE;
    int index = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] > maxVal) {
            maxVal = nums[i];
            index = i;
        }
    }

    TreeNode root = new TreeNode(maxVal);
    // Đệ quy gọi dựng cây con trái/phải
    root.left = constructMaximumBinaryTree(nums[0..index-1]);
    root.right = constructMaximumBinaryTree(nums[index+1..nums.length-1]);
    return root;
}
```

**Giá trị lớn nhất trong `nums` hiện tại chính là Node gốc, rồi dựa vào index đệ quy gọi mảng trái/phải dựng cây con trái/phải là được**.

Rõ ý tưởng, chúng ta có thể viết lại một hàm phụ `build`, khống chế index của `nums`:

```java
class Solution {

    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }

    // Định nghĩa: nums[lo..hi] dựng thành cây phù hợp điều kiện, trả về Node gốc
    TreeNode build(int[] nums, int lo, int hi) {
        // base case
        if (lo > hi) {
            return null;
        }

        // Tìm giá trị lớn nhất trong mảng và index tương ứng
        int index = -1, maxVal = Integer.MIN_VALUE;
        for (int i = lo; i <= hi; i++) {
            if (maxVal < nums[i]) {
                index = i;
                maxVal = nums[i];
            }
        }

        // Dựng Node gốc ra trước
        TreeNode root = new TreeNode(maxVal);
        // Đệ quy gọi dựng cây con trái/phải
        root.left = build(nums, lo, index - 1);
        root.right = build(nums, index + 1, hi);
        
        return root;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/maximum-binary-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 Animation trực quan hóa code👾</strong>
</summary>
</details>
</a>
<hr/>



Đến đây, bài này làm xong, vẫn khá đơn giản đúng không, dưới đây xem hai bài khó hơn ít.

## Dựng cây nhị phân qua kết quả duyệt tiền thứ tự và trung thứ tự 

Bài 105 trên LeetCode「Dựng cây nhị phân từ duyệt tiền thứ tự và trung thứ tự 」chính là bài kinh điển này, phỏng vấn bài kiểm tra thường thi :

<Problem slug="construct-binary-tree-from-preorder-and-inorder-traversal" />

```java
// Chữ ký hàm như sau
TreeNode buildTree(int[] preorder, int[] inorder);
```

 không nói thêm lời thừa, nghĩ thẳng ý tưởng, trước hết nghĩ, Node gốc phải làm gì.

**tương tự bài trước, chúng ta chắc chắn cần muốn cách xác định giá trị Node gốc, Node gốc làm ra, rồi đệ quy dựng cây con trái/phải là được**.

Chúng ta ôn lại trước, kết quả duyệt tiền thứ tự và trung thứ tự có đặc điểm gì?

```java
void traverse(TreeNode root) {
    // Duyệt tiền thứ tự 
    preorder.add(root.val);
    traverse(root.left);
    traverse(root.right);
}

void traverse(TreeNode root) {
    traverse(root.left);
    // Duyệt trung thứ tự 
    inorder.add(root.val);
    traverse(root.right);
}
```

Bài trước [Cây nhị phân thì mấy khung đó](https://labuladong.online/algo/data-structure/flatten-nested-list-iterator/) viết, khác biệt thứ tự duyệt này, khiến phân bố phần tử trong mảng `preorder` và `inorder` có đặc điểm sau:

![](https://labuladong.online/algo/images/binary-tree-ii/1.jpeg)

Tìm Node gốc rất đơn giản, giá trị đầu của duyệt tiền thứ tự `preorder[0]` chính là giá trị Node gốc.

Mấu chốt là qua giá trị Node gốc, chia mảng `preorder` và `inorder` thành hai nửa, dựng cây con trái/phải của Node gốc thế nào?

Nói cách khác, với phần `?` trong code sau phải điền gì:

```java
TreeNode buildTree(int[] preorder, int[] inorder) {
    // Theo định nghĩa hàm, dùng preorder và inorder dựng cây nhị phân
    return build(preorder, 0, preorder.length - 1,
                inorder, 0, inorder.length - 1);
}

// Định nghĩa của hàm build:
// Nếu mảng duyệt tiền thứ tự là preorder[preStart..preEnd],
// Mảng duyệt trung thứ tự là inorder[inStart..inEnd],
// Dựng cây nhị phân, trả về Node gốc của cây này
TreeNode build(int[] preorder, int preStart, int preEnd, 
            int[] inorder, int inStart, int inEnd) {
    // Giá trị Node root tương ứng chính là phần tử đầu của mảng duyệt tiền thứ tự 
    int rootVal = preorder[preStart];
    // Index của rootVal trong mảng duyệt trung thứ tự 
    int index = 0;
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == rootVal) {
            index = i;
            break;
        }
    }

    TreeNode root = new TreeNode(rootVal);
    // Đệ quy dựng cây con trái/phải
    root.left = build(preorder, ?, ?,
                    inorder, ?, ?);

    root.right = build(preorder, ?, ?,
                    inorder, ?, ?);
    return root;
}
```

Với biến `rootVal` và `index` trong code, chính là tình huống hình dưới:

![](https://labuladong.online/algo/images/binary-tree-ii/2.jpeg)

Ngoài ra, cũng có độc giả chú ý, qua vòng for duyệt xác định `index` hiệu suất không tính cao, có thể tối ưu thêm.

Vì đề nói giá trị Node cây nhị phân không tồn tại trùng, nên có thể dùng một HashMap lưu map từ phần tử tới index, như vậy là có thể tra thẳng `index` tương ứng `rootVal` qua HashMap:

```java
// Lưu map từ giá trị tới index trong inorder
HashMap<Integer, Integer> valToIndex = new HashMap<>();

public TreeNode buildTree(int[] preorder, int[] inorder) {
    for (int i = 0; i < inorder.length; i++) {
        valToIndex.put(inorder[i], i);
    }
    return build(preorder, 0, preorder.length - 1,
                 inorder, 0, inorder.length - 1);
}

TreeNode build(int[] preorder, int preStart, int preEnd, 
               int[] inorder, int inStart, int inEnd) {
    int rootVal = preorder[preStart];
    // Tránh vòng for tìm rootVal
    int index = valToIndex.get(rootVal);
    // ...
}
```

Giờ chúng ta xem hình làm bài điền, mấy dấu hỏi dưới phải điền gì:

```java
root.left = build(preorder, ?, ?,
                  inorder, ?, ?);

root.right = build(preorder, ?, ?,
                   inorder, ?, ?);
```

Với index bắt đầu và kết thúc của mảng `inorder` tương ứng cây con trái/phải tương đối dễ xác định:

![](https://labuladong.online/algo/images/binary-tree-ii/3.jpeg)

```java
root.left = build(preorder, ?, ?,
                  inorder, inStart, index - 1);

root.right = build(preorder, ?, ?,
                   inorder, index + 1, inEnd);
```

Với mảng `preorder`? Xác định index bắt đầu và kết thúc tương ứng mảng trái/phải thế nào?

Cái này có thể qua số Node của cây con trái suy ra ra, giả sử số Node của cây con trái là `leftSize`, vậy tình huống index trên mảng `preorder` như sau:

![](https://labuladong.online/algo/images/binary-tree-ii/4.jpeg)

Nhìn hình này là có thể viết index tương ứng `preorder` vào:

```java
int leftSize = index - inStart;

root.left = build(preorder, preStart + 1, preStart + leftSize,
                  inorder, inStart, index - 1);

root.right = build(preorder, preStart + leftSize + 1, preEnd,
                   inorder, index + 1, inEnd);
```

Đến đây, ý tưởng thuật toán tổng thể hoàn thành, chúng ta vá thêm base case là có thể viết code lời giải :

```java
class Solution {
    // Lưu map từ giá trị tới index trong inorder
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,
                    inorder, 0, inorder.length - 1);
    }

    // Định nghĩa của hàm build:
    // Nếu mảng duyệt tiền thứ tự là preorder[preStart..preEnd],
    // Mảng duyệt trung thứ tự là inorder[inStart..inEnd],
    // Dựng cây nhị phân, trả về Node gốc của cây này
    TreeNode build(int[] preorder, int preStart, int preEnd, 
               int[] inorder, int inStart, int inEnd) {

        if (preStart > preEnd) {
            return null;
        }

        // Giá trị Node root tương ứng chính là phần tử đầu của mảng duyệt tiền thứ tự 
        int rootVal = preorder[preStart];
        // Index của rootVal trong mảng duyệt trung thứ tự 
        int index = valToIndex.get(rootVal);

        int leftSize = index - inStart;

        // Dựng Node gốc hiện tại ra trước
        TreeNode root = new TreeNode(rootVal);
        // Đệ quy dựng cây con trái/phải
        root.left = build(preorder, preStart + 1, preStart + leftSize,
                        inorder, inStart, index - 1);

        root.right = build(preorder, preStart + leftSize + 1, preEnd,
                        inorder, index + 1, inEnd);
        return root;
    }
}
```

Hàm chính của chúng ta chỉ cần gọi hàm `buildTree` là được, bạn nhìn hàm nhiều tham số vậy, lời giải nhiều code vậy, dường như so bài trên giảng khó nhiều, khiến người nhìn đã thấy ngại, thực tế, những tham số này chẳng qua khống chế vị trí bắt đầu/kết thúc mảng, vẽ hình là giải được.

## Dựng cây nhị phân qua kết quả duyệt hậu thứ tự và trung thứ tự 

 tương tự bài trước, lần này chúng ta lợi dụng mảng kết quả duyệt **hậu thứ tự ** và **trung thứ tự ** để khôi phục cây nhị phân, đây là bài 106 trên LeetCode「Dựng cây nhị phân từ duyệt hậu thứ tự và trung thứ tự 」:

<Problem slug="construct-binary-tree-from-inorder-and-postorder-traversal" />

```java
// Chữ ký hàm như sau
TreeNode buildTree(int[] inorder, int[] postorder);
```

 tương tự, xem đặc điểm của duyệt hậu thứ tự và trung thứ tự :

```java
void traverse(TreeNode root) {
    traverse(root.left);
    traverse(root.right);
    // Duyệt hậu thứ tự 
    postorder.add(root.val);
}

void traverse(TreeNode root) {
    traverse(root.left);
    // Duyệt trung thứ tự 
    inorder.add(root.val);
    traverse(root.right);
}
```

Khác biệt thứ tự duyệt này, khiến phân bố phần tử trong mảng `postorder` và `inorder` có đặc điểm sau:

![](https://labuladong.online/algo/images/binary-tree-ii/5.jpeg)

 khác biệt mấu chốt của bài này và bài trước là, duyệt hậu thứ tự và duyệt tiền thứ tự ngược nhau, giá trị Node gốc tương ứng là phần tử cuối của `postorder`.

Khung thuật toán tổng thể rất tương tự bài trước, chúng ta vẫn viết một hàm phụ `build`:

```java
class Solution {
    // Lưu map từ giá trị tới index trong inorder
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(inorder, 0, inorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    // Định nghĩa của hàm build:
    // Mảng duyệt hậu thứ tự là postorder[postStart..postEnd],
    // Mảng duyệt trung thứ tự là inorder[inStart..inEnd],
    // Dựng cây nhị phân, trả về Node gốc của cây này
    TreeNode build(int[] inorder, int inStart, int inEnd,
                int[] postorder, int postStart, int postEnd) {
        // Giá trị Node root tương ứng chính là phần tử cuối của mảng duyệt hậu thứ tự 
        int rootVal = postorder[postEnd];
        // Index của rootVal trong mảng duyệt trung thứ tự 
        int index = valToIndex.get(rootVal);

        TreeNode root = new TreeNode(rootVal);
        // Đệ quy dựng cây con trái/phải
        root.left = build(inorder, ?, ?,
                        postorder, ?, ?);

        root.right = build(inorder, ?, ?,
                        postorder, ?, ?);
        return root;
    }
}
```

Giờ trạng thái tương ứng của `postoder` và `inorder` như sau:

![](https://labuladong.online/algo/images/binary-tree-ii/6.jpeg)

Chúng ta có thể theo hình trên điền đúng index chỗ dấu hỏi:

```java
int leftSize = index - inStart;

root.left = build(inorder, inStart, index - 1,
                  postorder, postStart, postStart + leftSize - 1);

root.right = build(inorder, index + 1, inEnd,
                   postorder, postStart + leftSize, postEnd - 1);
```

Tổng hợp, có thể viết code lời giải đầy đủ:

```java
class Solution {
    // Lưu map từ giá trị tới index trong inorder
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(inorder, 0, inorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    // Định nghĩa của hàm build:
    // Mảng duyệt hậu thứ tự là postorder[postStart..postEnd],
    // Mảng duyệt trung thứ tự là inorder[inStart..inEnd],
    // Dựng cây nhị phân, trả về Node gốc của cây này
    TreeNode build(int[] inorder, int inStart, int inEnd,
                int[] postorder, int postStart, int postEnd) {

        if (inStart > inEnd) {
            return null;
        }
        // Giá trị Node root tương ứng chính là phần tử cuối của mảng duyệt hậu thứ tự 
        int rootVal = postorder[postEnd];
        // Index của rootVal trong mảng duyệt trung thứ tự 
        int index = valToIndex.get(rootVal);
        // Số Node của cây con trái
        int leftSize = index - inStart;
        TreeNode root = new TreeNode(rootVal);
        // Đệ quy dựng cây con trái/phải
        root.left = build(inorder, inStart, index - 1,
                            postorder, postStart, postStart + leftSize - 1);
        
        root.right = build(inorder, index + 1, inEnd,
                            postorder, postStart + leftSize, postEnd - 1);
        return root;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/construct-binary-tree-from-inorder-and-postorder-traversal/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>



Có lót của bài trước, bài này giải rất nhanh, chẳng qua `rootVal` thành phần tử cuối, sửa tham số của hàm đệ quy mà thôi, chỉ cần hiểu đặc tính của cây nhị phân, cũng không khó viết ra.

## Dựng cây nhị phân qua kết quả duyệt hậu thứ tự và tiền thứ tự 

Đây là bài 889 trên LeetCode「Dựng cây nhị phân theo duyệt tiền thứ tự và hậu thứ tự 」, cho bạn nhập kết quả duyệt tiền thứ tự và hậu thứ tự của cây nhị phân, bắt bạn khôi phục cấu trúc cây nhị phân.

Chữ ký hàm như sau:

```java
TreeNode constructFromPrePost(int[] preorder, int[] postorder);
```

Bài này và hai bài trước có khác biệt bản chất:

**Qua kết quả duyệt tiền thứ tự trung thứ tự, hoặc hậu thứ tự trung thứ tự có thể xác định duy nhất một cây nhị phân gốc, nhưng qua kết quả duyệt tiền thứ tự hậu thứ tự không cách nào xác định cây nhị phân gốc duy nhất**.

Đề cũng nói, nếu có nhiều kết quả khôi phục khả thi, bạn có thể trả về bất kỳ loại nào.

Tại sao? Chúng ta nói, khuôn mẫu dựng cây nhị phân rất đơn giản, tìm Node gốc trước, rồi tìm và đệ quy dựng cây con trái/phải là được.

Hai bài trước, có thể qua kết quả duyệt tiền thứ tự hoặc hậu thứ tự tìm Node gốc, rồi dựa vào kết quả duyệt trung thứ tự xác định cây con trái/phải (đề nói cây không có Node trùng `val`).

Bài này, bạn có thể xác định Node gốc, nhưng không cách nào biết chính xác cây con trái/phải có những Node nào.

Lấy ví dụ, ví dụ cho bạn nhập này:

```
preorder = [1,2,3], postorder = [3,2,1]
```

Hai cây dưới đều phù hợp điều kiện, nhưng hiển nhiên cấu trúc chúng khác:

![](https://labuladong.online/algo/images/binary-tree-ii/7.png)

Nhưng nói lại, dùng kết quả duyệt hậu thứ tự và tiền thứ tự khôi phục cây nhị phân, logic lời giải và hai bài trước khác không nhiều, cũng là qua khống chế index của cây con trái/phải để dựng:

**1, Trước hết phần tử đầu của kết quả duyệt tiền thứ tự hoặc phần tử cuối của kết quả duyệt hậu thứ tự xác định là giá trị Node gốc**.

**2, Rồi phần tử thứ hai của kết quả duyệt tiền thứ tự làm giá trị của Node gốc cây con trái**.

**3, Tìm giá trị của Node gốc cây con trái trong kết quả duyệt hậu thứ tự, nhờ đó xác định biên index của cây con trái, rồi xác định biên index của cây con phải, đệ quy dựng cây con trái/phải là được**.

![](https://labuladong.online/algo/images/binary-tree-ii/8.jpeg)

Chi tiết xem code.

```java
class Solution {
    // Lưu map từ giá trị tới index trong postorder
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        for (int i = 0; i < postorder.length; i++) {
            valToIndex.put(postorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    // Định nghĩa: theo preorder[preStart..preEnd] và postorder[postStart..postEnd]
    // Dựng cây nhị phân, và trả về Node gốc.
    TreeNode build(int[] preorder, int preStart, int preEnd,
                   int[] postorder, int postStart, int postEnd) {
        if (preStart > preEnd) {
            return null;
        }
        if (preStart == preEnd) {
            return new TreeNode(preorder[preStart]);
        }

        // Giá trị Node root tương ứng chính là phần tử đầu của mảng duyệt tiền thứ tự 
        int rootVal = preorder[preStart];
        // Giá trị của root.left là phần tử thứ hai của duyệt tiền thứ tự 
        // Mấu chốt dựng cây nhị phân qua duyệt tiền thứ tự và hậu thứ tự là qua Node gốc của cây con trái
        // Xác định khoảng phần tử của cây con trái/phải trong preorder và postorder
        int leftRootVal = preorder[preStart + 1];
        // Index của leftRootVal trong mảng duyệt hậu thứ tự 
        int index = valToIndex.get(leftRootVal);
        // Số phần tử của cây con trái
        int leftSize = index - postStart + 1;

        // Dựng Node gốc hiện tại ra trước
        TreeNode root = new TreeNode(rootVal);
        // Đệ quy dựng cây con trái/phải
        // Dựa vào index của Node gốc cây con trái và số phần tử suy ra biên index của cây con trái/phải
        root.left = build(preorder, preStart + 1, preStart + leftSize,
                postorder, postStart, index);
        root.right = build(preorder, preStart + leftSize + 1, preEnd,
                postorder, index + 1, postEnd - 1);

        return root;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/construct-binary-tree-from-preorder-and-postorder-traversal/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 Animation trực quan hóa code👾</strong>
</summary>
</details>
</a>
<hr/>



Code và hai bài trước rất tương tự, chúng ta có thể nhìn code nghĩ, tại sao cây nhị phân khôi phục qua kết quả duyệt tiền thứ tự và hậu thứ tự có thể không duy nhất?

Mấu chốt ở câu này:

```java
int leftRootVal = preorder[preStart + 1];
```

Chúng ta giả sử phần tử thứ hai của duyệt tiền thứ tự là Node gốc của cây con trái, nhưng thực tế cây con trái có thể là con trỏ rỗng, vậy phần tử này phải là Node gốc của cây con phải. Vì ở đây không cách nào kiểm tra chính xác, nên dẫn đến đáp án cuối không duy nhất.

Đến đây, vấn đề khôi phục cây nhị phân qua kết quả duyệt tiền thứ tự và hậu thứ tự cũng giải xong.

Cuối cùng kết nối lại với bài trước, **vấn đề dựng cây nhị phân thường đều dùng ý tưởng 「phân rã bài toán」: dựng cả cây = Node gốc + dựng cây con trái + dựng cây con phải**. Tìm Node gốc ra trước, rồi dựa vào giá trị Node gốc tìm phần tử của cây con trái/phải, rồi đệ quy dựng cây con trái/phải.

Giờ bạn có hiểu huyền diệu trong đó không?




<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Bài tập kinh điển về cây tìm kiếm nhị phân I](https://labuladong.online/algo/problem-set/bst1/)
 - [【Luyện tập tăng cường】Dùng tư duy「phân rã bài toán」 giải bài I](https://labuladong.online/algo/problem-set/binary-tree-divide-i/)
 - [【Luyện tập tăng cường】Dùng tư duy「phân rã bài toán」 giải bài II](https://labuladong.online/algo/problem-set/binary-tree-divide-ii/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần đặc tính)](https://labuladong.online/algo/data-structure/bst-part1/)
 - [Tâm pháp cây nhị phân (Phần serialize)](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/?show=1) | [1008. Dựng cây tìm kiếm nhị phân từ duyệt tiền thứ tự ](https://leetcode.cn/problems/construct-binary-search-tree-from-preorder-traversal/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 07. Dựng lại cây nhị phân](https://leetcode.cn/problems/zhong-jian-er-cha-shu-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 33. Chuỗi duyệt hậu thứ tự của cây tìm kiếm nhị phân](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
