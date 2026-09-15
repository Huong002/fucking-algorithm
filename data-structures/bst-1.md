# Tâm pháp Cây tìm kiếm nhị phân BST (Phần đặc tính)




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [1038. Binary Search Tree to Greater Sum Tree](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/) | [1038. Từ cây tìm kiếm nhị phân sang cây tổng lớn hơn](https://leetcode.cn/problems/binary-search-tree-to-greater-sum-tree/) | 🟠 |
| [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) | [230. Phần tử nhỏ thứ K trong cây tìm kiếm nhị phân](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/) | 🟠 |
| [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/) | [538. Biến cây tìm kiếm nhị phân thành cây tích lũy ](https://leetcode.cn/problems/convert-bst-to-greater-tree/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về cấu trúc cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)

Bài trước dẫn bạn làm cây nhị phân từng bước đã viết [Phần tư duy](https://labuladong.online/algo/data-structure/binary-tree-part1/), [Phần dựng cây](https://labuladong.online/algo/data-structure/binary-tree-part2/), [Phần hậu thứ tự ](https://labuladong.online/algo/data-structure/binary-tree-part3/) và [Phần serialize](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/).

Hôm nay mở series cây tìm kiếm nhị phân (Binary Search Tree, sau đây viết tắt là BST), dẫn bạn làm BST từng bước.

Trước hết, đặc tính của BST chắc mọi người đều quen rồi (xem chi tiết ở chương kiến thức cơ bản [Cơ bản về cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)):

1, Với mỗi Node `node` của BST, giá trị các Node trong cây con trái đều nhỏ hơn giá trị của `node`, giá trị các Node trong cây con phải đều lớn hơn giá trị của `node`.

2, Với mỗi Node `node` của BST, cây con trái và cây con phải của nó đều là BST.

Cây tìm kiếm nhị phân không tính phức tạp, nhưng tôi thấy nó có thể tính là nửa giang sơn của lĩnh vực cấu trúc dữ liệu, các cấu trúc dữ liệu trực tiếp dựa trên BST có cây AVL, cây đỏ-đen, v.v., sở hữu tính chất tự cân bằng, có thể cho hiệu suất thêm/xóa/tìm/sửa cấp logN; còn có cây B+, cây đoạn (segment tree) v.v. cấu trúc đều được thiết kế dựa trên tư tưởng của BST.

**Xét từ góc độ làm bài thuật toán với BST, ngoài định nghĩa của nó, còn một tính chất quan trọng: kết quả duyệt trung thứ tự (inorder) của BST là có thứ tự (tăng dần)**.

Tức là, nếu cho một BST, đoạn code sau có thể in giá trị mỗi Node trong BST theo thứ tự tăng dần:

```java
void traverse(TreeNode root) {
    if (root == null) return;
    traverse(root.left);
    // Vị trí code duyệt trung thứ tự 
    print(root.val);
    traverse(root.right);
}
```

Vậy dựa vào tính chất này, chúng ta làm hai bài thuật toán.

## Tìm phần tử nhỏ thứ K

Đây là bài 230 trên LeetCode「Phần tử nhỏ thứ K trong cây tìm kiếm nhị phân」, xem đề:

<Problem slug="kth-smallest-element-in-a-bst" />

Nhu cầu này rất thường gặp đúng không, một ý tưởng trực tiếp là sắp xếp tăng dần, rồi tìm phần tử thứ `k`. Duyệt trung thứ tự BST thực ra chính là kết quả sắp xếp tăng dần, tìm phần tử thứ `k` chắc chắn không phải việc khó.

Theo ý tưởng này, có thể viết thẳng code:

```java
class Solution {
    int kthSmallest(TreeNode root, int k) {
        // Lợi dụng đặc tính duyệt trung thứ tự của BST
        traverse(root, k);
        return res;
    }

    // Ghi lại kết quả
    int res = 0;
    // Ghi lại thứ hạng của phần tử hiện tại
    int rank = 0;
    void traverse(TreeNode root, int k) {
        if (root == null) {
            return;
        }
        traverse(root.left, k);

        // Vị trí code trung thứ tự 
        rank++;
        if (k == rank) {
            // Tìm được phần tử nhỏ thứ k
            res = root.val;
            return;
        }

        traverse(root.right, k);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/kth-smallest-element-in-a-bst/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>



Bài này làm xong rồi, nhưng vẫn phải nói thêm vài câu, vì lời giải này không phải lời giải hiệu quả nhất, mà chỉ áp dụng cho bài này.

Bài trước [Tính trung vị của dòng dữ liệu hiệu quả](https://labuladong.online/algo/practice-in-action/find-median-from-data-stream/) đã từng nhắc vấn đề hôm nay:

> [!NOTE]
> Nếu bắt bạn cài đặt một phương thức tính phần tử tương ứng theo thứ hạng trong cây tìm kiếm nhị phân `select(int k)`, bạn sẽ thiết kế thế nào?

Nếu làm theo cách vừa nói, lợi dụng tính chất「duyệt trung thứ tự BST chính là kết quả sắp xếp tăng dần」, mỗi lần tìm phần tử nhỏ thứ `k` đều phải duyệt trung thứ tự một lần, độ phức tạp thời gian worst-case là $O(N)$, `N` là số Node của BST.

Phải biết tính chất BST rất đỉnh, như cây đỏ-đen cải tiến tự cân bằng BST, thêm/xóa/tìm/sửa đều có độ phức tạp $O(logN)$, bắt bạn tính một phần tử nhỏ thứ `k`, độ phức tạp thời gian lại cần $O(N)$, hơi kém hiệu quả.

Cho nên, tính phần tử nhỏ thứ `k`, thuật toán tốt nhất chắc chắn cũng là độ phức tạp cấp log, nhưng điều này phụ thuộc vào lượng thông tin mà Node BST ghi lại.

Chúng ta nghĩ xem tại sao thao tác của BST hiệu quả vậy? Lấy tìm kiếm một phần tử mà nói, nguyên nhân căn bản khiến BST tìm được phần tử đó trong thời gian log vẫn nằm trong định nghĩa của BST, trái nhỏ phải lớn, nên mỗi Node đều có thể so sánh giá trị của mình để quyết định đi tìm giá trị mục tiêu ở cây con trái hay cây con phải, nhờ đó tránh duyệt toàn cây, đạt độ phức tạp cấp log.

Vậy quay lại vấn đề này, muốn tìm phần tử nhỏ thứ `k`, hay nói là tìm phần tử có thứ hạng `k`, nếu muốn đạt độ phức tạp cấp log, mấu chốt cũng nằm ở mỗi Node phải biết mình xếp thứ mấy.

Ví dụ bạn bắt tôi tìm phần tử hạng `k`, Node hiện tại biết mình hạng `m`, vậy tôi có thể so sánh `m` và `k`:

1, Nếu `m == k`, hiển nhiên là tìm được phần tử thứ `k` rồi, trả về Node hiện tại là được.

2, Nếu `k < m`, cho thấy phần tử hạng `k` ở cây con trái, nên có thể đi cây con trái tìm phần tử thứ `k`.

3, Nếu `k > m`, cho thấy phần tử hạng `k` ở cây con phải, nên có thể đi cây con phải tìm phần tử thứ `k - m - 1`.

Như vậy là có thể giảm độ phức tạp thời gian xuống $O(logN)$.

Vậy, làm sao để mỗi Node biết thứ hạng của mình?

Đây chính là điều chúng ta nói trước đó, cần duy trì thông tin thêm trong Node cây nhị phân. **Mỗi Node cần ghi lại, cây nhị phân lấy mình làm gốc có bao nhiêu Node**.

Tức là, trường trong `TreeNode` của chúng ta phải như sau:

```java
class TreeNode {
    int val;
    // Tổng số Node của cây lấy Node này làm gốc
    int size;
    TreeNode left;
    TreeNode right;
}
```

Có trường `size`, cộng thêm tính chất trái nhỏ phải lớn của Node BST, với mỗi Node `node` là có thể suy ra thứ hạng của `node` qua `node.left`, nhờ đó làm được thuật toán cấp log vừa nói.

Dĩ nhiên, trường `size` cần được duy trì đúng khi thêm/xóa phần tử, `TreeNode` mà LeetCode cho không có trường `size` này, nên bài này chúng ta chỉ đành lợi dụng đặc tính duyệt trung thứ tự của BST để cài đặt, nhưng ý tưởng tối ưu nói ở trên là thao tác thường gặp của BST, vẫn cần hiểu.

## BST chuyển thành cây tích lũy 

Bài 538 và 1038 trên LeetCode đều là bài này, hoàn toàn giống nhau, bạn có thể làm cả hai luôn. Xem đề:

<Problem slug="convert-bst-to-greater-tree" />

Đề chắc không khó hiểu, ví dụ Node 5 trong hình, nếu chuyển thành cây tích lũy, Node lớn hơn 5 có 6,7,8, cộng thêm bản thân 5, nên giá trị của Node này trên cây tích lũy phải là 5+6+7+8=26.

Chúng ta cần biến BST thành cây tích lũy, chữ ký hàm như sau:

```java
TreeNode convertBST(TreeNode root)
```

Theo ý tưởng chung của cây nhị phân, cần nghĩ mỗi Node nên làm gì, nhưng với bài này rất khó nghĩ ra ý tưởng gì.

Mỗi Node của BST trái nhỏ phải lớn, đây dường như là thông tin hữu ích, vì tổng tích lũy là tính tổng mọi phần tử lớn hơn hoặc bằng giá trị hiện tại, vậy mỗi Node đều đi tính tổng cây con phải, không phải được sao?

Không được. Với một Node mà nói, đúng là cây con phải đều là phần tử lớn hơn nó, nhưng vấn đề là Node cha của nó cũng có thể là phần tử lớn hơn nó? Cái này không xác định được, chúng ta lại không có con trỏ chạm tới Node cha, nên ý tưởng chung của cây nhị phân ở đây dùng không được.

**Đường này không thông, chúng ta cứ đổi ý tưởng, vẫn lợi dụng đặc tính duyệt trung thứ tự của BST**.

Vừa rồi chúng ta nói code duyệt trung thứ tự của BST có thể in giá trị Node theo tăng dần, vậy nếu tôi muốn in giá trị Node theo giảm dần thì sao?

Rất đơn giản, chỉ cần đổi thứ tự đệ quy, duyệt cây con phải trước, cây con trái sau là được:

```java
void traverse(TreeNode root) {
    if (root == null) return;
    // Đệ quy duyệt cây con phải trước
    traverse(root.right);
    // Vị trí code duyệt trung thứ tự 
    print(root.val);
    // Đệ quy duyệt cây con trái sau
    traverse(root.left);
}
```

**Đoạn code này có thể in giá trị Node BST theo giảm dần, nếu duy trì một biến tích lũy ngoài `sum`, rồi gán `sum` cho mỗi Node trong BST, không phải đã biến BST thành cây tích lũy sao**?

Xem code là hiểu ngay:

```java
class Solution {
    TreeNode convertBST(TreeNode root) {
        traverse(root);
        return root;
    }

    // Ghi lại tổng tích lũy 
    int sum = 0;
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        traverse(root.right);
        // duy trì tổng tích lũy 
        sum += root.val;
        // Biến BST thành cây tích lũy 
        root.val = sum;
        traverse(root.left);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/convert-bst-to-greater-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🥳 Animation trực quan hóa code🥳</strong>
</summary>
</details>
</a>
<hr/>



Bài này giải xong rồi, cốt lõi vẫn là đặc tính duyệt trung thứ tự của BST, chỉ là chúng ta sửa thứ tự đệ quy, duyệt giảm dần giá trị phần tử BST, nhờ đó khớp với yêu cầu cây tích lũy của đề.

Tóm tắt đơn giản, vấn đề liên quan BST, hoặc lợi dụng đặc tính trái nhỏ phải lớn của BST để nâng hiệu suất thuật toán, hoặc lợi dụng đặc tính duyệt trung thứ tự để thỏa mãn yêu cầu đề bài, cũng chỉ mấy chuyện đó.

Bài này đến đây thôi, thêm nhiều bài tập cây nhị phân kinh điển và rèn luyện tư duy đệ quy, xem phần [bài tập](https://labuladong.online/algo/problem-set/bst1/) trong chương cây nhị phân.






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Bài tập kinh điển về cây tìm kiếm nhị phân I](https://labuladong.online/algo/problem-set/bst1/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần thao tác cơ bản)](https://labuladong.online/algo/data-structure/bst-part2/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần dựng cây)](https://labuladong.online/algo/data-structure/bst-part3/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm Chỉ Offer II 054. Tổng mọi giá trị lớn hơn hoặc bằng Node](https://leetcode.cn/problems/w6cpku/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
