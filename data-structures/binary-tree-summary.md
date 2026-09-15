# Cương lĩnh cốt lõi thuật toán series cây nhị phân



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | [104. Độ sâu lớn nhất của cây nhị phân](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) | 🟢 |
| [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) | [144. Duyệt tiền thứ tự cây nhị phân](https://leetcode.cn/problems/binary-tree-preorder-traversal/) | 🟢 |
| [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/) | [543. Đường kính của cây nhị phân](https://leetcode.cn/problems/diameter-of-binary-tree/) | 🟢 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về cấu trúc cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)

> [!IMPORTANT]
> Bài này sẽ trừu tượng và quy nạp nhiều thuật toán, nên sẽ chứa lượng lớn link bài khác.
>
> **Độc giả lần đầu đọc bài này đừng học DFS bài này, gặp thuật toán chưa học hoặc chỗ không hiểu thì bỏ qua, chỉ cần có ấn tượng về lý thuyết tổng kết ở bài này là được**. Khi học kỹ thuật thuật toán phía sau của đứng, bạn tự nhiên dần hiểu tinh túy của bài này, sau này quay lại đọc bài này, sẽ cảm nhận sâu sắc sâu hơn.

Mạch của mọi bài viết trên đứng đều xây theo khung nêu ở [Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/), trong đó nhấn mạnh tầm quan trọng của bài cây nhị phân, nên đặt bài này ở series ắt đọc của chương một.

Tôi luyện bài nhiều năm vậy, cô đọng ra một tổng cương lĩnh thuật toán cây nhị phân đặt ở đây, có thể dùng từ không đặc biệt chuyên nghiệp, cũng không có giáo trình nào thu nhận tổng kết kinh nghiệm này của tôi, nhưng hiện bài kho của các nền tảng luyện bài, không có một bài cây nhị phân nào nằm ngoài khung này. Nếu bạn có thể phát hiện một bài và khung bài này cho không tương thích, mời lời nhắn cho tôi.

Tóm tắt ở đầu trước, mô thức tư duy cây nhị phân chia hai loại :

**1, Có thể qua duyệt một lần cây nhị phân ra đáp án không**? Nếu được, dùng một hàm `traverse` phối hợp biến ngoài để cài đặt, đây gọi mô thức tư duy「duyệt」.

**2, Có thể định nghĩa một hàm đệ quy, qua đáp án của bài con (cây con) suy ra ra đáp án của bài gốc không**? Nếu được, viết định nghĩa của hàm đệ quy này, và tận dụng giá trị trả về của hàm này, đây gọi mô thức tư duy「phân rã bài toán」.

Dù dùng mô thức tư duy nào, bạn đều cần nghĩ:

**Nếu tách riêng một Node cây nhị phân, nó cần làm việc gì? Cần làm lúc nào (vị trí tiền/trung/hậu thứ tự)**? Node khác không cần bạn lo, hàm đệ quy sẽ giúp bạn chạy thao tác giống nhau trên mọi Node.

Trong bài này sẽ dùng bài ví dụ , nhưng đều là bài đơn giản nhất, nên không cần lo mình không hiểu , tôi có thể giúp bạn từ vấn đề đơn giản nhất đúc kết ra chung tính của mọi bài cây nhị phân, và tư duy hàm chứa trong cây nhị phân được nâng tầm , ngược tay dùng vào [quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/), [thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/), [thuật toán chia để trị](https://labuladong.online/algo/essential-technique/divide-and-conquer/), [thuật toán đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/) đi , đây cũng là lý do tôi một thẳng nhấn mạnh tư duy khung. Hy vọng bạn sau khi học thuật toán cao cấp trên, cũng có thể quay đầu xem lại bài này, sẽ có nhận thức sâu hơn về chúng.

Trước hết, tôi vẫn cần không chán phiềnnhấn mạnh tầm quan trọng của cấu trúc dữ liệu cây nhị phân và thuật toán liên quan.







## Tầm quan trọng của cây nhị phân

Lấy ví dụ, như hai thuật toán sắp xếp kinh điển [sắp xếp nhanh](https://labuladong.online/algo/practice-in-action/quick-sort/) và [sắp xếp trộn ](https://labuladong.online/algo/practice-in-action/merge-sort/), với nó hai , bạn có hiểu gì?

**Nếu bạn nói tôi, sắp xếp nhanh chính là duyệt tiền thứ tự của cây nhị phân, sắp xếp trộn chính là duyệt hậu thứ tự của cây nhị phân, vậy tôi thì biết bạn là cao thủ thuật toán**.

Tại sao sắp xếp nhanh và sắp xếp trộn có thể có liên quan với cây nhị phân? Chúng ta phân tích đơn giản tư tưởng thuật toán và khung code của chúng:

Logic của sắp xếp nhanh là, nếu cần với `nums[lo..hi]` sắp xếp, chúng ta tìm một điểm phân giới `p` trước, qua hoán đổi phần tử khiến `nums[lo..p-1]` đều nhỏ hơn hoặc bằng `nums[p]`, và `nums[p+1..hi]` đều lớn hơn `nums[p]`, rồi đệ quy đi `nums[lo..p-1]` và `nums[p+1..hi]` tìm điểm phân giới mới, cuối cùng cả mảng thì được sắp xếp.

Khung code của sắp xếp nhanh như sau:

```java
void sort(int[] nums, int lo, int hi) {
    // ****** Vị trí duyệt tiền thứ tự ******
    // Qua hoán đổi phần tử dựng điểm phân giới p
    int p = partition(nums, lo, hi);
    // ************************

    sort(nums, lo, p - 1);
    sort(nums, p + 1, hi);
}
```

Dựng điểm phân giới trước, rồi đi mảng con trái/phải dựng điểm phân giới, bạn xem đây không phải duyệt tiền thứ tự của một cây nhị phân sao?

Nói tiếp logic của sắp xếp trộn, nếu cần với `nums[lo..hi]` sắp xếp, chúng ta sắp xếp `nums[lo..mid]` trước, rồi sắp xếp `nums[mid+1..hi]`, cuối cùng hai mảng con có thứ tự này trộn, cả mảng thì xếp tốt.

Khung code của sắp xếp trộn như sau:

```java
// Định nghĩa: sắp xếp nums[lo..hi]
void sort(int[] nums, int lo, int hi) {
    int mid = (lo + hi) / 2;
    // Sắp xếp nums[lo..mid]
    sort(nums, lo, mid);
    // Sắp xếp nums[mid+1..hi]
    sort(nums, mid + 1, hi);

    // ****** Vị trí hậu thứ tự ******
    // trộn nums[lo..mid] và nums[mid+1..hi]
    merge(nums, lo, mid, hi);
    // *********************
}
```

Sắp xếp mảng con trái/phải trước, rồi trộn (logic tương tự trộn linked list có thứ tự), bạn xem đây có phải khung duyệt hậu thứ tự của cây nhị phân? Ngoài ra, đây không phải thuật toán chia để trị truyền thuyết, tuy nhiên như này .

Nếu bạn liếc thì nhìn thấu chi tiết bên trong của những thuật toán sắp xếp này, còn cần cõng những thuật toán kinh điển này không? Không cần. Bạn có thể bắt tay làm một cách dễ dàng, từ khung duyệt cây nhị phân là có thể mở rộng ra thuật toán.

Nói nhiều vậy, nhằm cho thấy, tư tưởng thuật toán của cây nhị phân dùng rộng, thậm chí có thể nói, chỉ cần liên quan đệ quy, đều có thể trừu tượng thành vấn đề cây nhị phân.

Tiếp theo chúng ta giảng dậy từ tiền/trung/hậu thứ tự của cây nhị phân, để bạn hiểu sâu sức hấp dẫn của cấu trúc dữ liệu này.

## Hiểu sâu tiền/trung/hậu thứ tự 

Tôi trước némcho bạn mấy câu hỏi, mời nhẩm nhẩm nghĩ 30 giây:

1, Duyệt tiền/trung/hậu thứ tự cây nhị phân bạn hiểu là gì, chỉ là ba List thứ tự khác nhau sao?

2, Mời phân tích, duyệt hậu thứ tự có đặc thù gì?

3, Mời phân tích, tại sao cây đa chạc không có duyệt trung thứ tự ?

Trả lời không được, cho thấy bạn hiểu tiền/trung/hậu thứ tự chỉ giới hạn ở sách giáo khoa, nhưng không sao, tôi dùng cách loại suy giải thích duyệt tiền/trung/hậu thứ tự trong mắt tôi.

Trước hết, ôn lại khung duyệt đệ quy cây nhị phân nói ở [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/):

```java
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    // Vị trí tiền thứ tự 
    traverse(root.left);
    // Vị trí trung thứ tự 
    traverse(root.right);
    // Vị trí hậu thứ tự 
}
```

Tạm kệ cái gọi là tiền/trung/hậu thứ tự, chỉ xem hàm `traverse`, bạn nói nó đang làm gì?

Thực ra nó chính là một hàm có thể duyệt mọi Node của cây nhị phân, và bạn duyệt mảng hoặc linked list bản chất không khác:

```java
// Duyệt lặp mảng
void traverse(int[] arr) {
    for (int i = 0; i < arr.length; i++) {

    }
}

// Duyệt đệ quy mảng
void traverse(int[] arr, int i) {
    if (i == arr.length) {
        return;
    }
    // Vị trí tiền thứ tự 
    traverse(arr, i + 1);
    // Vị trí hậu thứ tự 
}

// Duyệt lặp danh sách liên kết đơn 
void traverse(ListNode head) {
    for (ListNode p = head; p != null; p = p.next) {

    }
}

// Duyệt đệ quy danh sách liên kết đơn 
void traverse(ListNode head) {
    if (head == null) {
        return;
    }
    // Vị trí tiền thứ tự 
    traverse(head.next);
    // Vị trí hậu thứ tự 
}
```

Duyệt danh sách liên kết đơn và mảng có thể lặp, cũng có thể đệ quy, **cấu trúc cây nhị phân chẳng qua là danh sách liên kết nhị chạc**, nó không cách nào sửa đơn giản thành dạng lặp của vòng for, nên chúng ta duyệt cây nhị phân thường đều dùng dạng đệ quy.

Bạn cũng chú ý, chỉ cần là duyệt dạng đệ quy, đều có thể có vị trí tiền thứ tự và hậu thứ tự, lần lượt trước và sau đệ quy.

**Cái gọi là vị trí tiền thứ tự, chính là lúc vừa vào một Node (phần tử), vị trí hậu thứ tự chính là lúc sắp rời một Node (phần tử)**, vậy tiến thêm, bạn viết code ở vị trí khác nhau, thời cơ chạy code cũng khác:

![](https://labuladong.online/algo/images/binary-tree-summary/1.jpeg)

Ví dụ, nếu bắt bạn **in ngược** giá trị mọi Node trên một danh sách liên kết đơn, bạn làm sao?

Cách cài đặt dĩ nhiên nhiều, nhưng nếu bạn hiểu đệ quy đủ thấu, có thể lợi dụng vị trí hậu thứ tự để thao tác:

```java
// Duyệt đệ quy danh sách liên kết đơn, in ngược phần tử linked list
void traverse(ListNode head) {
    if (head == null) {
        return;
    }
    traverse(head.next);
    // Vị trí hậu thứ tự 
    print(head.val);
}
```

Kết hợp hình trên, bạn phải biết tại sao đoạn code này có thể in ngược danh sách liên kết đơn, bản chất là lợi dụng stack của đệ quy giúp bạn cài đặt hiệu quả duyệt ngược.

Vậy nói về cây nhị phân cũng giống, chỉ là thêm một vị trí trung thứ tự mà thôi.

Trong sách giáo khoa chỉ hỏi bạn kết quả duyệt tiền/trung/hậu thứ tự lần lượt là gì, nên với một người chỉ học khóa cấu trúc dữ liệu đại học, anh ta khoảng cho rằng tiền/trung/hậu thứ tự của cây nhị phân chẳng qua tương ứng ba danh sách `List<Integer>` thứ tự khác nhau.

Nhưng tôi muốn nói, **tiền/trung/hậu thứ tự là ba thời điểm đặc biệt xử lý mỗi Node trong quá trình duyệt cây nhị phân**, tuyệt không chỉ là ba List thứ tự khác nhau:

Code vị trí tiền thứ tự chạy lúc vừa vào một Node cây nhị phân;

Code vị trí hậu thứ tự chạy lúc sắp rời một Node cây nhị phân;

Code vị trí trung thứ tự chạy lúc cây con trái của một Node cây nhị phân đều duyệt xong, sắp bắt đầu duyệt cây con phải.

Bạn chú ý dùng từ của bài này, tôi một thẳng nói「vị trí」tiền/trung/hậu thứ tự, chính là muốn khác với「duyệt」tiền/trung/hậu thứ tự mọi người thường nói: bạn có thể ở vị trí tiền thứ tự viết code về phía một List nhét phần tử, vậy cuối ra chính là kết quả duyệt tiền thứ tự ; nhưng không phải nói bạn thì không có thể viết code phức tạp hơn làm việc phức tạp hơn.

Vẽ thành hình, ba vị trí tiền/trung/hậu thứ tự trên cây nhị phân như sau:

![](https://labuladong.online/algo/images/binary-tree-summary/2.jpeg)

**Bạn có thể phát hiện mỗi Node đều có「duy nhất」vị trí tiền/trung/hậu thứ tự thuộc mình**, nên tôi nói duyệt tiền/trung/hậu thứ tự là ba thời điểm đặc biệt xử lý mỗi Node trong quá trình duyệt cây nhị phân.

Ở đây bạn cũng hiểu tại sao cây đa chạc không có vị trí trung thứ tự, vì mỗi Node của cây nhị phân chỉ cắt đổi một lần duy nhất cây con trái sang phải, mà Node cây đa chạc có thể nhiều Node con, sẽ nhiều lần cắt đổi cây con để duyệt, nên Node cây đa chạc không có vị trí duyệt trung thứ tự 「duy nhất」.

Nói nhiều cơ bản vậy, chính là muốn giúp bạn xây dựng nhận thức đúng về cây nhị phân, rồi bạn sẽ phát hiện:

**Mọi vấn đề của cây nhị phân, chính là bắt bạn đưa vào logic code khéo ở vị trí tiền/trung/hậu thứ tự, để đạt mục đích của mình, bạn chỉ cần nghĩ riêng mỗi Node phải làm gì, còn lại không cần bạn quản, ném cho khung duyệt cây nhị phân, đệ quy sẽ làm thao tác giống nhau trên mọi Node**.

Bạn cũng thấy, [Cơ bản thuật toán đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/) khung duyệt cây nhị phân mở rộng tới đồ thị, và lấy duyệt làm cơ sở cài đặt các thuật toán kinh điển của lý thuyết đồ thị, nhưng đây là chuyện sau, bài này thì không nói nhiều.






## Hai ý tưởng giải bài 

Bài trước [Tâm đắc học thuật toán của tôi](https://labuladong.online/algo/essential-technique/algorithm-summary/) nói:

**đệ quy của bài cây nhị phân có thể chia hai loại ý tưởng, loại một là duyệt một lần cây nhị phân ra đáp án, loại hai là qua phân rã bài toán tính ra đáp án, hai loại ý tưởng này lần lượt tương ứng [Khung cốt lõi thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/) và [Khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)**.

> [!TIP]
> Ở đây nói thói quen đặt tên hàm của tôi: khi dùng ý tưởng duyệt trong cây nhị phân chữ ký hàm thường là `void traverse(...)`, không có giá trị trả về, dựa vào cập nhật biến ngoài để tính kết quả, mà khi dùng ý tưởng phân rã bài toán tên hàm dựa vào chức năng cụ thể của hàm đó, mà thường sẽ có giá trị trả về, giá trị trả về là kết quả tính của bài con.
>
> Tương ứng, bạn sẽ phát hiện chữ ký hàm tôi cho trong [Khung cốt lõi thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/) thường cũng là `void backtrack(...)` không có giá trị trả về, mà trong [Khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) chữ ký hàm cho là hàm `dp` mang giá trị trả về. Đây cũng cho thấy mối liên hệ chằng chịt giữa hai thứ này và cây nhị phân.
>
> Dù đặt tên hàm không có yêu cầu cứng, nhưng tôi vẫn khuyên bạn cũng theo phong cách này của tôi, như vậy hơn nổi bật tác dụng của hàm và mô thức tư duy giải bài, tiện bạn tự hiểu và dùng.

Lúc đó tôi dùng vấn đề độ sâu lớn nhất của cây nhị phân để ví dụ, trọng điểm là hai ý tưởng này so với quy hoạch động và thuật toán quay lui, mà trọng điểm của bài này là phân tích hai ý tưởng này giải bài cây nhị phân thế nào.

Bài 104 trên LeetCode「Độ sâu lớn nhất của cây nhị phân」chính là bài độ sâu lớn nhất, cái gọi là độ sâu lớn nhất chính là số Node trên đường dài nhất từ Node gốc tới Node lá「xa nhất」, ví dụ nhập cây nhị phân này, thuật toán phải trả về 3:

![](https://labuladong.online/algo/images/binary-tree-summary/tree.jpg)

 ý tưởng làm bài này của bạn là gì? Hiển nhiên duyệt một lần cây nhị phân, dùng một biến ngoài ghi độ sâu mỗi Node tới, lấy giá trị lớn nhất là ra độ sâu lớn nhất, **đây chính là ý tưởng duyệt cây nhị phân tính đáp án**.

Code lời giải như sau:

```java
class Solution {
    // Ghi độ sâu lớn nhất
    int res = 0;

    // Ghi độ sâu của Node duyệt tới
    int depth = 0;

    public int maxDepth(TreeNode root) {
        traverse(root);
        return res;
    }

    // Khung duyệt cây nhị phân
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        // Vị trí tiền thứ tự 
        depth++;
        if (root.left == null && root.right == null) {
            // Tới Node lá, cập nhật độ sâu lớn nhất
            res = Math.max(res, depth);
        }
        traverse(root.left);
        traverse(root.right);
        // Vị trí hậu thứ tự 
        depth--;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-maxdepth1/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 Animation trực quan hóa code🍭</strong>
</summary>
</details>
</a>
<hr/>

 lời giải này phải rất dễ hiểu, nhưng tại sao cần ở vị trí tiền thứ tự tăng `depth`, ở vị trí hậu thứ tự giảm `depth`?

Vì phía trước nói, vị trí tiền thứ tự là lúc vào một Node, vị trí hậu thứ tự là lúc rời một Node, `depth` ghi độ sâu Node đệ quy tới hiện tại, bạn `traverse` hiểu thành một con trỏ bơi đi trên cây nhị phân, nên dĩ nhiên cần duy trì vậy.

Còn cập nhật `res`, bạn đặt vào vị trí tiền/trung/hậu thứ tự đều được, chỉ cần đảm bảo sau khi vào Node, trước khi rời Node (tức sau khi `depth` tự tăng, trước khi tự giảm) là được.

Dĩ nhiên, bạn cũng rất dễ phát hiện độ sâu lớn nhất của một cây nhị phân có thể qua độ sâu lớn nhất của cây con suy ra ra, **đây chính là ý tưởng phân rã bài toán tính đáp án**.

Code lời giải như sau:

```java
class Solution {
    // Định nghĩa: nhập Node gốc, trả về độ sâu lớn nhất của cây nhị phân này
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        // Lợi dụng định nghĩa, tính độ sâu lớn nhất của cây con trái/phải
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        // Độ sâu lớn nhất của cả cây bằng độ sâu lớn nhất của cây con trái/phải lấy max,
        // Rồi cộng thêm Node gốc chính mình 
        int res = Math.max(leftMax, rightMax) + 1;

        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-maxdepth2/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code🌈</strong>
</summary>
</details>
</a>
<hr/>

Chỉ cần rõ ràng định nghĩa của hàm đệ quy, lời giải này cũng không khó hiểu, nhưng tại sao logic code chính tập trung ở vị trí hậu thứ tự ?

Vì cốt lõi đúng của ý tưởng này là, bạn đúng có thể qua độ sâu lớn nhất của cây con suy ra ra độ sâu của cây gốc, nên dĩ nhiên cần trước lợi dụng định nghĩa của hàm đệ quy tính độ sâu lớn nhất của cây con trái/phải, rồi đưa ra độ sâu lớn nhất của cây gốc, logic chính tự nhiên đặt vào vị trí hậu thứ tự .

Nếu bạn hiểu hai ý tưởng của vấn đề độ sâu lớn nhất này, **vậy chúng ta quay đầu xem lại duyệt tiền/trung/hậu thứ tự cơ bản nhất của cây nhị phân**, ví dụ bài 144 trên LeetCode「Duyệt tiền thứ tự cây nhị phân」, bắt bạn tính kết quả duyệt tiền thứ tự .

 lời giải quen thuộc của chúng ta chính là dùng ý tưởng 「duyệt」, tôi nghĩ phải không có gì cần nói:

```java
class Solution {
    // lưu trữ kết quả duyệt tiền thứ tự 
    List<Integer> res = new LinkedList<>();

    // Trả về kết quả duyệt tiền thứ tự 
    public List<Integer> preorderTraversal(TreeNode root) {
        traverse(root);
        return res;
    }

    // Hàm duyệt cây nhị phân
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        // Vị trí tiền thứ tự 
        res.add(root.val);
        traverse(root.left);
        traverse(root.right);
    }
}
```

Nhưng bạn có thể dùng ý tưởng 「phân rã bài toán」, tính kết quả duyệt tiền thứ tự không?

Nói cách khác, đừng dùng hàm phụ như `traverse` và bất kỳ biến ngoài nào, đơn thuần dùng hàm `preorderTraverse` đề cho đệ quy giải bài, bạn biết không?

Chúng ta biết đặc điểm của duyệt tiền thứ tự là, giá trị Node gốc xếp đầu, tiếp là kết quả duyệt tiền thứ tự của cây con trái, cuối là kết quả duyệt tiền thứ tự của cây con phải:

![](https://labuladong.online/algo/images/binary-tree-summary/3.jpeg)

Vậy đây không phải có thể phân rã bài toán sao, **kết quả duyệt tiền thứ tự của một cây nhị phân = Node gốc + kết quả duyệt tiền thứ tự của cây con trái + kết quả duyệt tiền thứ tự của cây con phải**.

Nên, bạn có thể cài đặt thuật toán duyệt tiền thứ tự thế này:

```java
class Solution {
    // Định nghĩa: nhập Node gốc của một cây nhị phân, trả về kết quả duyệt tiền thứ tự của cây này
    List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if (root == null) {
            return res;
        }
        // Kết quả của duyệt tiền thứ tự, root.val ở đầu
        res.add(root.val);
        // Lợi dụng định nghĩa hàm, phía sau tiếp theo kết quả duyệt tiền thứ tự của cây con trái
        res.addAll(preorderTraversal(root.left));
        // Lợi dụng định nghĩa hàm, cuối tiếp theo kết quả duyệt tiền thứ tự của cây con phải
        res.addAll(preorderTraversal(root.right));
        return res;
    }
}
```

Duyệt trung thứ tự và hậu thứ tự cũng tương tự, chỉ cần `add(root.val)` đặt vào vị trí trung thứ tự và hậu thứ tự tương ứng là được.

 lời giải này ngắn gọn, nhưng tại sao không thường gặp?

Một nguyên nhân là **độ phức tạp của thuật toán này không dễ nắm vững **, tương đối phụ thuộc đặc tính ngôn ngữ.

Java dù ArrayList hay LinkedList, độ phức tạp của phương thức `addAll` đều O(N), nên tổng worst-case độ phức tạp thời gian sẽ đạt O(N^2), trừ phi bạn tự cài đặt một phương thức `addAll` độ phức tạp O(1), tầng dưới dùng linked list là có thể làm được, vì nhiều linked list chỉ cần thao tác con trỏ đơn giản là có thể nối lại.

Dĩ nhiên, nguyên nhân chủ yếu vẫn vì sách giáo khoa chưa bao giờ dạy vậy...

Trên giơ hai ví dụ đơn giản, nhưng còn không ít bài cây nhị phân có thể đồng thời dùng hai ý tưởng để nghĩ và giải, đây thì cần dựa vào bạn tự nhiều luyện và nghĩ, đừng chỉ thỏa mãn với một ý tưởng lời giải quen thuộc.

Tổng hợp, quá trình nghĩ tổng quát khi gặp một bài cây nhị phân là:

**1, Có thể qua duyệt một lần cây nhị phân ra đáp án không**? Nếu được, dùng một hàm `traverse` phối hợp biến ngoài để cài đặt.

**2, Có thể định nghĩa một hàm đệ quy, qua đáp án của bài con (cây con) suy ra ra đáp án của bài gốc không**? Nếu được, viết định nghĩa của hàm đệ quy này, và tận dụng giá trị trả về của hàm này.

**3, Dù dùng mô thức tư duy nào, bạn đều cần hiểu mỗi Node của cây nhị phân cần làm gì, cần làm lúc nào (tiền/trung/hậu thứ tự)**.

Ở [Luyện chuyên đề đệ quy cây nhị phân](https://labuladong.online/algo/intro/binary-tree-practice/) của đứng cột hơn 100 bài cây nhị phân, hoàn toàn dùng hai mô thức tư duy trên dẫn bạn luyện từng bước, giúp bạn nắm hoàn toàn tư duy đệ quy, dễ hiểu thuật toán cao cấp hơn.






## Đặc thù của vị trí hậu thứ tự 

Trước khi nói vị trí hậu thứ tự, nói đơn giản tiền thứ tự và trung thứ tự trước.

Vị trí tiền thứ tự bản thân thực ra không có tính chất đặc biệt gì, sở dĩ bạn phát hiện dường như nhiều bài đều ở vị trí tiền thứ tự viết code, thực tế vì chúng ta quen những code không nhạy với vị trí tiền/trung/hậu thứ tự viết ở vị trí tiền thứ tự mà thôi.

Vị trí trung thứ tự chủ yếu dùng trong tình huống BST, bạn hoàn toàn có thể duyệt trung thứ tự của BST cho là duyệt mảng có thứ tự.

> [!IMPORTANT]
> **Quan sát kỹ, code vị trí tiền/trung/hậu thứ tự, năng lực dựa lần tăng mạnh **.
>
> Code vị trí tiền thứ tự chỉ có thể từ tham số hàm lấy dữ liệu Node cha truyền tới.
>
> Code vị trí trung thứ tự không chỉ có thể lấy dữ liệu tham số, còn có thể lấy dữ liệu cây con trái qua giá trị trả về của hàm truyền về.
>
> Code vị trí hậu thứ tự mạnh nhất, không chỉ có thể lấy dữ liệu tham số, còn có thể đồng thời lấy dữ liệu cây con trái/phải qua giá trị trả về của hàm truyền về.
>
> Nên, vài trường hợp code chuyển tới vị trí hậu thứ tự hiệu suất cao nhất; vài việc, chỉ code vị trí hậu thứ tự làm được.

Lấy vài ví dụ cụ thể để cảm nhận khác biệt năng lực của chúng. Giờ cho bạn một cây nhị phân, tôi hỏi bạn hai vấn đề đơn giản:

1, Nếu xem Node gốc là tầng 1, in tầng mỗi Node thế nào?

2, In cây con trái/phải của mỗi Node các có bao nhiêu Node thế nào?

Vấn đề một có thể viết code thế này:

```java
// Hàm duyệt cây nhị phân
void traverse(TreeNode root, int level) {
    if (root == null) {
        return;
    }
    // Vị trí tiền thứ tự 
    printf("Node %s at level %d", root.val, level);
    traverse(root.left, level + 1);
    traverse(root.right, level + 1);
}

// Gọi thế này
traverse(root, 1);
```

Vấn đề hai có thể viết code thế này:

```java
// Định nghĩa: nhập một cây nhị phân, trả về tổng số Node của cây này
int count(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int leftCount = count(root.left);
    int rightCount = count(root.right);
    // Vị trí hậu thứ tự 
    printf("Cây con trái của Node %s có %d Node, cây con phải có %d Node",
            root, leftCount, rightCount);

    return leftCount + rightCount + 1;
}
```

> [!NOTE]
> Một Node ở tầng mấy, bạn từ Node gốc duyệt qua quá trình là có thể tiện thể ghi, dùng tham số của hàm đệ quy là có thể truyền xuống; mà lấy một Node làm gốc cả cây con có bao nhiêu Node, bạn bắt buộc duyệt xong cây con sau mới đếm rõ, rồi qua giá trị trả về của hàm đệ quy lấy đáp án.
>
> Kết hợp hai vấn đề đơn giản này, bạn nếm vị đặc điểm của vị trí hậu thứ tự, chỉ vị trí hậu thứ tự mới qua giá trị trả về lấy thông tin của cây con.
>
> **Vậy nói cách khác, một khi bạn phát hiện đề liên quan cây con, nhiều khả năng cần cho hàm đặt định nghĩa và giá trị trả về hợp, ở vị trí hậu thứ tự viết code**.

Tiếp theo xem vị trí hậu thứ tự phát huy tác dụng trong bài thực tế thế nào, trò chuyện đơn giản bài 543 trên LeetCode「Đường kính của cây nhị phân」, bắt bạn tính độ dài đường kính dài nhất của một cây nhị phân.

Cái gọi là「đường kính」của cây nhị phân, chính là độ dài đường đi giữa hai node bất kỳ.「Đường kính」dài nhất không nhất định cần xuyên Node gốc, ví dụ cây nhị phân dưới:

![](https://labuladong.online/algo/images/binary-tree-summary/tree1.png)

Đường kính dài nhất của nó là 3, tức mấy「đường kính」 `[4,2,1,3]`, `[4,2,1,9]` hoặc `[5,2,1,3]` này.

Mấu chốt giải bài này là, **độ dài「đường kính」mỗi cây nhị phân, chính là tổng độ sâu lớn nhất của cây con trái/phải của một Node**.

Giờ bắt tôi tìm đường kính dài nhất trong cả cây, vậy ý tưởng trực tiếp chính là duyệt mỗi Node trong cả cây, rồi qua độ sâu lớn nhất của cây con trái/phải của mỗi Node tính「đường kính」mỗi Node, cuối cùng mọi「đường kính」 tìm max là được.

Thuật toán độ sâu lớn nhất chúng ta vừa cài đặt xong, ý tưởng trên là có thể viết code sau:

```java
class Solution {
    // Ghi độ dài đường kính lớn nhất
    int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        // Với mỗi Node tính đường kính, tìm đường kính lớn nhất
        traverse(root);
        return maxDiameter;
    }

    // Duyệt cây nhị phân
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        // Với mỗi Node tính đường kính
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        int myDiameter = leftMax + rightMax;
        // Cập nhật đường kính lớn nhất toàn cục
        maxDiameter = Math.max(maxDiameter, myDiameter);
        
        traverse(root.left);
        traverse(root.right);
    }

    // Tính độ sâu lớn nhất của cây nhị phân
    int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        return 1 + Math.max(leftMax, rightMax);
    }
}
```

 lời giải này đúng, nhưng thời gian chạy rất dài, nguyên nhân cũng rất rõ, `traverse` duyệt mỗi Node còn gọi hàm đệ quy `maxDepth`, mà `maxDepth` là cần duyệt mọi Node của cây con, nên worst-case độ phức tạp thời gian là O(N^2).

Đây thì xuất hiện tình huống vừa thảo luận, **vị trí tiền thứ tự không cách nào lấy thông tin cây con, nên chỉ có thể để mỗi Node gọi hàm `maxDepth` đi tính độ sâu của cây con**.

Vậy tối ưu thế nào? Chúng ta phải logic tính「đường kính」 đặt vào vị trí hậu thứ tự, chính xác phải đặt vào vị trí hậu thứ tự của `maxDepth`, vì vị trí hậu thứ tự của `maxDepth` là biết độ sâu lớn nhất của cây con trái/phải.

Nên, sửa chút logic code là ra lời giải tốt hơn:

```java
class Solution {
    // Ghi độ dài đường kính lớn nhất
    int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return maxDiameter;
    }

    int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int leftMax = maxDepth(root.left);
        int rightMax = maxDepth(root.right);
        // Vị trí hậu thứ tự, tiện tính đường kính lớn nhất
        int myDiameter = leftMax + rightMax;
        maxDiameter = Math.max(maxDiameter, myDiameter);

        return 1 + Math.max(leftMax, rightMax);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/mydata-diameter-of-binary-tree/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code🎃</strong>
</summary>
</details>
</a>
<hr/>



Giờ độ phức tạp thời gian chỉ O(N) của hàm `maxDepth`.

 giảng tới đây, đối chiếu bài trước: gặp vấn đề cây con, đầu tiên nghĩ tới là cho hàm đặt giá trị trả về, rồi ở vị trí hậu thứ tự làm bài.

> [!NOTE]
> Câu hỏi nghĩ: mời bạn nghĩ, bài dùng duyệt hậu thứ tự dùng ý tưởng 「duyệt」hay ý tưởng 「phân rã bài toán」?


> [!NOTE]
> Bài lợi dụng vị trí hậu thứ tự, thường đều dùng ý tưởng 「phân rã bài toán」. Vì Node hiện tại nhận và lợi dụng thông tin cây con trả về, đây thì có nghĩa là bạn bài gốc phân thành bài con của Node hiện tại + cây con trái/phải.
>
> Ngược lại, nếu bạn viết lời giải đệ quy lồng đệ quy tương tự lúc đầu, nhiều khả năng cũng cần suy ngẫm lại có thể qua duyệt hậu thứ tự tối ưu không.

Thêm nhiều bài lợi dụng vị trí hậu thứ tự xem [Dẫn bạn làm cây nhị phân từng bước (Phần hậu thứ tự )](https://labuladong.online/algo/data-structure/binary-tree-part3/)、[Dẫn bạn làm cây tìm kiếm nhị phân từng bước (Phần hậu thứ tự )](https://labuladong.online/algo/data-structure/bst-part4/) và [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài ](https://labuladong.online/algo/problem-set/binary-tree-post-order-i/).






## Nhìn thuật toán QHD/quay lui/DFS từ góc cây khác biệt và liên hệ

Bài trước tôi nói quy hoạch động/thuật toán quay lui chính là hai ý tưởng khác nhau của thuật toán cây nhị phân thể hiện, tin độc giả có thể thấy tới đây phải cũng công nhận quan điểm này của tôi. Nhưng có độc giả tinh ý thường hỏi: cách nghĩ của bạn khiến tôi bừng tỉnh ngộ, nhưng bạn dường như một thẳng chưa giảng thuật toán DFS?

Thực ra tôi ở [Một bài diệt gọn mọi bài đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/) chính là dùng thuật toán DFS, nhưng tôi đúng chưa dùng riêng một bài giảng thuật toán DFS, **vì thuật toán DFS và thuật toán quay lui rất tương tự , chỉ khác chi tiết**.

 Khác chi tiết này là gì? Thực ra chính là「làm lựa chọn」và「hủy lựa chọn」rốt cuộc ở ngoài hay trong vòng for khác biệt, thuật toán DFS ở ngoài, thuật toán quay lui ở trong.

Tại sao có khác biệt này? Vẫn cần kết hợp cây nhị phân hiểu. Phần này tôi thì ba tư tưởng thuật toán kinh điển thuật toán quay lui, thuật toán DFS, quy hoạch động, và liên hệ khác biệt của chúng với thuật toán cây nhị phân, dùng một câu để cho thấy :

> [!IMPORTANT]
> Thuật toán QHD/DFS/quay lui đều có thể xem mở rộng của vấn đề cây nhị phân, chỉ là điểm quan tâm của chúng khác:
>
> - Thuật toán quy hoạch động thuộc ý tưởng phân rã bài toán (chia để trị), điểm quan tâm của nó ở cả「cây con」.
> - Thuật toán quay lui thuộc ý tưởng duyệt, điểm quan tâm của nó ở「cành cây」giữa Node.
> - Thuật toán DFS thuộc ý tưởng duyệt, điểm quan tâm của nó ở đơn lẻ 「Node」.

Hiểu thế nào? Tôi lần lượt giơ ba ví dụ bạn thì hiểu.

### Ví dụ một: thể hiện tư tưởng phân rã bài toán

**Ví dụ một**, cho bạn một cây nhị phân, mời bạn dùng ý tưởng phân rã bài toán viết một hàm `count`, tính cây nhị phân này tổng cộng bao nhiêu Node. Code rất đơn giản, trên đều viết:

```java
// Định nghĩa: nhập một cây nhị phân, trả về tổng số Node của cây này
int count(TreeNode root) {
    if (root == null) {
        return 0;
    }
    // Node hiện tại quan tâm là tổng số Node của hai cây con lần lượt bao nhiêu
    // Vì dùng kết quả của bài con có thể suy ra ra kết quả của bài gốc
    int leftCount = count(root.left);
    int rightCount = count(root.right);
    // Vị trí hậu thứ tự, số Node của cây con trái/phải cộng chính mình chính là số Node của cả cây
    return leftCount + rightCount + 1;
}
```

**Bạn xem, đây chính là ý tưởng phân rã bài toán của quy hoạch động, điểm nhìn của nó mãi là bài con cấu trúc giống nhau toàn bộ, loại suy lên cây nhị phân chính là「cây con」**.

Bạn xem lại vấn đề quy hoạch động cụ thể, ví dụ lấy Fibonacci làm ví dụ trong [khuôn mẫu Khung quy hoạch động giải thích chi tiết](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/), điểm quan tâm của chúng ta ở giá trị trả về của từng cây con:

```java
int fib(int N) {
    if (N == 1 || N == 2) return 1;
    return fib(N - 1) + fib(N - 2);
}
```

![](https://labuladong.online/algo/images/dynamic-programming/2.jpg)



### Ví dụ hai: thể hiện tư tưởng thuật toán quay lui

**Ví dụ hai**, cho bạn một cây nhị phân, mời bạn dùng ý tưởng duyệt viết một hàm `traverse`, in quá trình duyệt cây nhị phân này, bạn xem code:

```java
void traverse(TreeNode root) {
    if (root == null) return;
    printf("Từ Node %s vào Node %s", root, root.left);
    traverse(root.left);
    printf("Từ Node %s về Node %s", root.left, root);

    printf("Từ Node %s vào Node %s", root, root.right);
    traverse(root.right);
    printf("Từ Node %s về Node %s", root.right, root);
}
```

Không khó hiểu đúng không, tốt, giờ chúng ta từ cây nhị phân nâng cao thành cây đa chạc, code cũng tương tự :

```java
// Node cây đa chạc
class Node {
    int val;
    Node[] children;
}

void traverse(Node root) {
    if (root == null) return;
    for (Node child : root.children) {
        printf("Từ Node %s vào Node %s", root, child);
        traverse(child);
        printf("Từ Node %s về Node %s", child, root);
    }
}
```

Khung duyệt cây đa chạc này là có thể mở rộng ra khung thuật toán quay lui trong [Khung khuôn mẫu thuật toán quay lui giải thích chi tiết ](https://labuladong.online/algo/essential-technique/backtrack-framework/):

```java
// Khung thuật toán quay lui
void backtrack(...) {
    // base case
    if (...) return;

    for (int i = 0; i < ...; i++) {
        // Làm lựa chọn
        ...

        // Vào tầng cây quyết định tiếp theo
        backtrack(...);

        // Hủy lựa chọn vừa làm
        ...
    }
}
```



**Bạn xem, đây chính là ý tưởng duyệt của thuật toán quay lui, điểm nhìn của nó mãi là quá trình di chuyển giữa Node, loại suy lên cây nhị phân chính là「cành cây」**.

Bạn xem lại vấn đề thuật toán quay lui cụ thể, ví dụ hoán vị toàn phần giảng trong [Thuật toán quay lui diệt gọn chín loại hoán vị tổ hợp tập con](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/), điểm quan tâm của chúng ta ở từng cành cây:

```java
// Phần code cốt lõi thuật toán quay lui
void backtrack(int[] nums) {
    // Khung thuật toán quay lui
    for (int i = 0; i < nums.length; i++) {
        // Làm lựa chọn
        used[i] = true;
        track.addLast(nums[i]);

        // Vào cây quay lui tầng tiếp theo
        backtrack(nums);

        // Hủy lựa chọn
        track.removeLast();
        used[i] = false;
    }
}
```

![](https://labuladong.online/algo/images/permutation/2.jpeg)



### Ví dụ ba: thể hiện tư tưởng DFS

**Ví dụ ba**, tôi cho bạn một cây nhị phân, mời bạn viết một hàm `traverse`, giá trị mỗi Node trên cây nhị phân này đều cộng một. Rất đơn giản đúng không, code như sau:

```java
void traverse(TreeNode root) {
    if (root == null) return;
    // Giá trị mỗi Node duyệt qua cộng một
    root.val++;
    traverse(root.left);
    traverse(root.right);
}
```

**Bạn xem, đây chính là ý tưởng duyệt của thuật toán DFS, điểm nhìn của nó mãi ở đơn lẻ Node, loại suy lên cây nhị phân chính là xử lý mỗi「Node」**.

Bạn xem lại vấn đề thuật toán DFS cụ thể, ví dụ mấy bài đầu giảng trong [Một bài diệt gọn mọi bài đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/), điểm quan tâm của chúng ta là mỗi ô (Node) của mảng `grid`, chúng ta cần với ô duyệt qua làm ít xử lý, nên tôi nói dùng thuật toán DFS giải mấy bài này:

```java
// Logic cốt lõi thuật toán DFS
void dfs(int[][] grid, int i, int j) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n) {
        return;
    }
    if (grid[i][j] == 0) {
        return;
    }
    // Mỗi ô duyệt qua đánh dấu thành 0
    grid[i][j] = 0;
    dfs(grid, i + 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i - 1, j);
    dfs(grid, i, j - 1);
}
```

![](https://labuladong.online/algo/images/island/5.jpg)



Tốt, mời bạn nếm kỹ ba ví dụ đơn giản trên, có phải như tôi nói: quy hoạch động quan tâm cả「cây con」, thuật toán quay lui quan tâm「cành cây」giữa Node, thuật toán DFS quan tâm đơn lẻ 「Node」.

Có lót này, bạn thì rất dễ hiểu tại sao logic「làm lựa chọn」và「hủy lựa chọn」trong code thuật toán quay lui và thuật toán DFS vị trí khác nhau, xem hai đoạn code dưới:

```java
// Thuật toán DFS logic「làm lựa chọn」「hủy lựa chọn」 đặt vào ngoài vòng for
void dfs(Node root) {
    if (root == null) return;
    // Làm lựa chọn
    print("enter node %s", root);
    for (Node child : root.children) {
        dfs(child);
    }
    // Hủy lựa chọn
    print("leave node %s", root);
}

// Thuật toán quay lui logic「làm lựa chọn」「hủy lựa chọn」 đặt vào trong vòng for
void backtrack(Node root) {
    if (root == null) return;
    for (Node child : root.children) {
        // Làm lựa chọn
        print("I'm on the branch from %s to %s", root, child);
        backtrack(child);
        // Hủy lựa chọn
        print("I'll leave the branch from %s to %s", child, root);
    }
}
```

Thấy chứ, bạn thuật toán quay lui bắt buộc logic「làm lựa chọn」và「hủy lựa chọn」 đặt vào trong vòng for, nếu không sao lấy hai đầu điểm của「cành cây」?

## Duyệt thứ tự tầng 

Loại bài cây nhị phân chủ yếu dùng bồi dưỡng tư duy đệ quy, mà duyệt thứ tự tầng thuộc duyệt lặp, cũng tương đối đơn giản, ở đây thì lướt khung code:

```java
// Nhập Node gốc của một cây nhị phân, duyệt thứ tự tầng cây này
void levelTraverse(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    // Duyệt mỗi tầng của cây nhị phân từ trên xuống dưới
    while (!q.isEmpty()) {
        int sz = q.size();
        // Duyệt mỗi Node của mỗi tầng từ trái sang phải
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            // Cho Node tầng tiếp theo vào hàng đợi
            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
    }
}
```

Trong này vòng while và vòng for chia nhau phụ trách duyệt từ trên xuống dưới và từ trái sang phải:

![](https://labuladong.online/algo/images/dijkstra/1.jpeg)

Bài trước [Khung thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/) chính là từ duyệt thứ tự tầng của cây nhị phân mở rộng ra, thường dùng tìm vấn đề **đường ngắn nhất** của đồ thị không trọng số.

Dĩ nhiên khung này còn có thể sửa linh hoạt, bài không cần ghi tầng (bước) có thể bỏ vòng for trong khung trên, ví dụ bài trước [Thuật toán Dijkstra](https://labuladong.online/algo/data-structure/dijkstra/) tính vấn đề đường ngắn nhất của đồ thị có trọng số, thảo luận chi tiết mở rộng của thuật toán BFS.

Đáng nói, vài bài cây nhị phân rất rõ cần dùng kỹ thuật duyệt thứ tự tầng, cũng có thể dùng cách duyệt đệ quy để giải, mà kỹ thuật tính sẽ mạnh hơn, rất khảo bạn nắm vững tiền/trung/hậu thứ tự .

Tốt rồi, bài này đã đủ dài, xoay quanh vị trí tiền/trung/hậu thứ tự tính là khuôn mẫu trong bài cây nhị phân giảng rõ, thực có thể dùng ra bao nhiêu, thì cần bạn tự luyện bài thực hành và nghĩ.

Hy vọng mọi người có thể khám phá càng nhiều lời giải càng tốt, chỉ cần ngẫm thấu nguyên lý của cấu trúc dữ liệu cơ bản cây nhị phân này, vậy thì rất dễ trên đường học thuật toán cao cấp khác tìm nắm tay, khai thông mạch, hình thành vòng khép kín (đùa chút thôi).

Cuối cùng, [Luyện chuyên đề đệ quy cây nhị phân](https://labuladong.online/algo/intro/binary-tree-practice/) sẽ dẫn bạn từng bước dùng kỹ thuật bài này giảng .

## Trả lời câu hỏi khu bình luận

Về duyệt thứ tự tầng (và [Khung thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/) mở rộng từ nó), tôi ở cuối nói thêm mấy câu.

Nếu bạn đủ quen cây nhị phân, có thể nghĩ nhiều cách qua hàm đệ quy ra kết quả duyệt thứ tự tầng, ví dụ cách viết dưới:

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();

    public List<List<Integer>> levelTraverse(TreeNode root) {
        if (root == null) {
            return res;
        }
        // root xem là tầng 0
        traverse(root, 0);
        return res;
    }

    void traverse(TreeNode root, int depth) {
        if (root == null) {
            return;
        }
        // Vị trí tiền thứ tự, xem đã lưu Node tầng depth chưa
        if (res.size() <= depth) {
            // Lần đầu vào tầng depth
            res.add(new LinkedList<>());
        }
        // Vị trí tiền thứ tự, ở tầng depth thêm giá trị của Node root
        res.get(depth).add(root.val);
        traverse(root.left, depth + 1);
        traverse(root.right, depth + 1);
    }
}
```

 ý tưởng này xét từ kết quả quả thực có thể ra kết quả duyệt thứ tự tầng, nhưng bản chất của nó vẫn là duyệt tiền thứ tự của cây nhị phân, hay nói là ý tưởng của DFS, chứ không phải duyệt thứ tự tầng, hay nói ý tưởng của BFS. Vì lời giải này là dựa vào đặc điểm thứ tự tự trên xuống dưới, tự trái sang phải của duyệt tiền thứ tự ra kết quả đúng.

**Trừu tượng nói, lời giải này hơn giống 「duyệt cột」từ trái sang phải, chứ không phải「duyệt tầng」tự trên xuống dưới**. Nên với tình huống tính khoảng cách nhỏ nhất, lời giải này hoàn toàn tương đương thuật toán DFS, không có ưu thế hiệu năng của thuật toán BFS.

Còn có độc giả ưu tú bình luận ý tưởng đệ quy duyệt thứ tự tầng thế này:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();

    public List<List<Integer>> levelTraverse(TreeNode root) {
        if (root == null) {
            return res;
        }
        List<TreeNode> nodes = new LinkedList<>();
        nodes.add(root);
        traverse(nodes);
        return res;
    }

    void traverse(List<TreeNode> curLevelNodes) {
        // base case
        if (curLevelNodes.isEmpty()) {
            return;
        }
        // Vị trí tiền thứ tự, tính giá trị tầng hiện tại và danh sách Node tầng tiếp theo
        List<Integer> nodeValues = new LinkedList<>();
        List<TreeNode> nextLevelNodes = new LinkedList<>();
        for (TreeNode node : curLevelNodes) {
            nodeValues.add(node.val);
            if (node.left != null) {
                nextLevelNodes.add(node.left);
            }
            if (node.right != null) {
                nextLevelNodes.add(node.right);
            }
        }
        // Vị trí tiền thứ tự thêm kết quả, có thể ra duyệt thứ tự tầng tự trên xuống dưới
        res.add(nodeValues);
        traverse(nextLevelNodes);
        // Vị trí hậu thứ tự thêm kết quả, có thể ra kết quả duyệt thứ tự tầng tự dưới lên trên
        // res.add(nodeValues);
    }
}
```

Hàm `traverse` này rất giống hàm đệ quy duyệt danh sách liên kết đơn, thực ra chính là mỗi tầng của cây nhị phân trừu tượng hiểu thành một Node của danh sách liên kết đơn để duyệt.

So lời giải đệ quy trước, lời giải đệ quy này là「duyệt tầng」tự trên xuống dưới, hơn gần bí quyết của BFS, có thể làm cài đặt đệ quy của thuật toán BFS mở rộng tư duy .



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Cài đặt code Trie/Cây cây Trie /Cây tiền tố](https://labuladong.online/algo/data-structure/trie-implement/)
 - [【Luyện tập tăng cường】Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
 - [【Luyện tập tăng cường】Bài tập kinh điển về cây tìm kiếm nhị phân I](https://labuladong.online/algo/problem-set/bst1/)
 - [【Luyện tập tăng cường】Bài tập kinh điển về cây tìm kiếm nhị phân II](https://labuladong.online/algo/problem-set/bst2/)
 - [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài I](https://labuladong.online/algo/problem-set/binary-tree-post-order-i/)
 - [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài II](https://labuladong.online/algo/problem-set/binary-tree-post-order-ii/)
 - [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài III](https://labuladong.online/algo/problem-set/binary-tree-post-order-iii/)
 - [【Luyện tập tăng cường】Dùng đồng thời hai tư duy giải bài ](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view/)
 - [【Luyện tập tăng cường】Thêm bài tập về hash table](https://labuladong.online/algo/problem-set/hash-table/)
 - [【Luyện tập tăng cường】Bài tập kinh điển thuật toán quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
 - [【Luyện tập tăng cường】Bài tập kinh điển thuật toán quay lui III](https://labuladong.online/algo/problem-set/backtrack-iii/)
 - [【Luyện tập tăng cường】Dùng tư duy「phân rã bài toán」 giải bài I](https://labuladong.online/algo/problem-set/binary-tree-divide-i/)
 - [【Luyện tập tăng cường】Dùng tư duy「phân rã bài toán」 giải bài II](https://labuladong.online/algo/problem-set/binary-tree-divide-ii/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài I](https://labuladong.online/algo/problem-set/binary-tree-traverse-i/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài II](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii/)
 - [【Luyện tập tăng cường】Dùng tư duy「duyệt」 giải bài III](https://labuladong.online/algo/problem-set/binary-tree-traverse-iii/)
 - [【Luyện tập tăng cường】Dùng duyệt thứ tự tầng giải bài I](https://labuladong.online/algo/problem-set/binary-tree-level-i/)
 - [【Luyện tập tăng cường】Dùng duyệt thứ tự tầng giải bài II](https://labuladong.online/algo/problem-set/binary-tree-level-ii/)
 - [Một phương pháp diệt gọn bài House Robber trên LeetCode](https://labuladong.online/algo/dynamic-programming/house-robber/)
 - [Một bài diệt gọn mọi bài đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/)
 - [Tâm pháp cây tìm kiếm nhị phân (Phần hậu thứ tự )](https://labuladong.online/algo/data-structure/bst-part4/)
 - [Tâm pháp cây nhị phân (Phần hậu thứ tự )](https://labuladong.online/algo/data-structure/binary-tree-part3/)
 - [Tâm pháp cây nhị phân (Phần serialize)](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/)
 - [Tâm pháp cây nhị phân (Phần ý tưởng )](https://labuladong.online/algo/data-structure/binary-tree-part1/)
 - [Tâm pháp cây nhị phân (Phần dựng cây)](https://labuladong.online/algo/data-structure/binary-tree-part2/)
 - [Duyệt đệ quy/duyệt thứ tự tầng cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
 - [Khung khuôn mẫu giải bài thuật toán chia để trị](https://labuladong.online/algo/essential-technique/divide-and-conquer/)
 - [Thuật toán chia để trị chi tiết: độ ưu tiên tính](https://labuladong.online/algo/fname.html?fname=分治算法)
 - [Chuyển tư duy quy hoạch động và thuật toán quay lui](https://labuladong.online/algo/dynamic-programming/word-break/)
 - [Hai góc nhìn liệt kê của quy hoạch động](https://labuladong.online/algo/dynamic-programming/two-views-of-dp/)
 - [Thực hành thuật toán quay lui: chia tập hợp ](https://labuladong.online/algo/practice-in-action/partition-to-k-equal-sum-subsets/)
 - [Thuật toán quay lui diệt gọn mọi bài hoán vị / tổ hợp /tập con](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)
 - [Khung khuôn mẫu giải bài thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/)
 - [Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Mở rộng: trộn sắp xếp giải thích chi tiết & ứng dụng](https://labuladong.online/algo/practice-in-action/merge-sort/)
 - [Mở rộng: sắp xếp nhanh giải thích chi tiết & ứng dụng](https://labuladong.online/algo/practice-in-action/quick-sort/)
 - [Mở rộng: khung series tổ tiên chung gần nhất](https://labuladong.online/algo/practice-in-action/lowest-common-ancestor-summary/)
 - [Mở rộng: dùng stack mô phỏng đệ quy duyệt lặp cây nhị phân](https://labuladong.online/algo/data-structure/iterative-traversal-binary-tree/)
 - [Phát hiện vòng & thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)
 - [Mô hình hộp bi: hai góc nhìn liệt kê của thuật toán quay lui](https://labuladong.online/algo/practice-in-action/two-views-of-backtrack/)
 - [Học thuật toán và trải nghiệm flow](https://labuladong.online/algo/fname.html?fname=心流)
 - [QHD kinh điển: khoảng cách biên tập](https://labuladong.online/algo/dynamic-programming/edit-distance/)
 - [Giải đáp nếu làm thắc mắc thuật toán quay lui/DFS](https://labuladong.online/algo/essential-technique/backtrack-vs-dfs/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [100. Same Tree](https://leetcode.com/problems/same-tree/?show=1) | [100. Cây giống nhau](https://leetcode.cn/problems/same-tree/?show=1) | 🟢 |
| [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/?show=1) | [1008. Dựng cây tìm kiếm nhị phân từ duyệt tiền thứ tự ](https://leetcode.cn/problems/construct-binary-search-tree-from-preorder-traversal/?show=1) | 🟠 |
| [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/?show=1) | [101. Cây nhị phân đối xứng](https://leetcode.cn/problems/symmetric-tree/?show=1) | 🟢 |
| [1022. Sum of Root To Leaf Binary Numbers](https://leetcode.com/problems/sum-of-root-to-leaf-binary-numbers/?show=1) | [1022. Tổng số nhị phân từ gốc tới lá](https://leetcode.cn/problems/sum-of-root-to-leaf-binary-numbers/?show=1) | 🟢 |
| [1026. Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/?show=1) | [1026. Hiệu lớn nhất giữa Node và tổ tiên](https://leetcode.cn/problems/maximum-difference-between-node-and-ancestor/?show=1) | 🟠 |
| [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/?show=1) | [108. Biến mảng có thứ tự thành cây tìm kiếm nhị phân](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/?show=1) | 🟢 |
| [1080. Insufficient Nodes in Root to Leaf Paths](https://leetcode.com/problems/insufficient-nodes-in-root-to-leaf-paths/?show=1) | [1080. Node thiếu trên đường từ gốc tới lá](https://leetcode.cn/problems/insufficient-nodes-in-root-to-leaf-paths/?show=1) | 🟠 |
| [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/?show=1) | [110. Cây nhị phân cân bằng](https://leetcode.cn/problems/balanced-binary-tree/?show=1) | 🟢 |
| [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/?show=1) | [111. Độ sâu nhỏ nhất của cây nhị phân](https://leetcode.cn/problems/minimum-depth-of-binary-tree/?show=1) | 🟢 |
| [1110. Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/?show=1) | [1110. Xóa Node thành rừng](https://leetcode.cn/problems/delete-nodes-and-return-forest/?show=1) | 🟠 |
| [1120. Maximum Average Subtree](https://leetcode.com/problems/maximum-average-subtree/?show=1)🔒 | [1120. Trung bình lớn nhất của cây con](https://leetcode.cn/problems/maximum-average-subtree/?show=1)🔒 | 🟠 |
| [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/?show=1) | [113. Tổng đường đi II](https://leetcode.cn/problems/path-sum-ii/?show=1) | 🟠 |
| [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/?show=1) | [114. Trải cây nhị phân thành linked list](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/?show=1) | 🟠 |
| [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/?show=1) | [116. Điền con trỏ Node phía phải tiếp theo của mỗi Node](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/?show=1) | 🟠 |
| [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/?show=1) | [124. Tổng đường lớn nhất trong cây nhị phân](https://leetcode.cn/problems/binary-tree-maximum-path-sum/?show=1) | 🔴 |
| [1245. Tree Diameter](https://leetcode.com/problems/tree-diameter/?show=1)🔒 | [1245. Đường kính của cây](https://leetcode.cn/problems/tree-diameter/?show=1)🔒 | 🟠 |
| [1261. Find Elements in a Contaminated Binary Tree](https://leetcode.com/problems/find-elements-in-a-contaminated-binary-tree/?show=1) | [1261. Tìm phần tử trong cây nhị phân ô nhiễm ](https://leetcode.cn/problems/find-elements-in-a-contaminated-binary-tree/?show=1)🔒 | 🟠 |
| [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/?show=1) | [129. tìm Tổng số từ Node gốc tới Node lá](https://leetcode.cn/problems/sum-root-to-leaf-numbers/?show=1) | 🟠 |
| [1315. Sum of Nodes with Even-Valued Grandparent](https://leetcode.com/problems/sum-of-nodes-with-even-valued-grandparent/?show=1) | [1315. Tổng Node có ông nội chẵn](https://leetcode.cn/problems/sum-of-nodes-with-even-valued-grandparent/?show=1) | 🟠 |
| [1325. Delete Leaves With a Given Value](https://leetcode.com/problems/delete-leaves-with-a-given-value/?show=1) | [1325. Xóa Node lá giá trị cho trước](https://leetcode.cn/problems/delete-leaves-with-a-given-value/?show=1) | 🟠 |
| [1339. Maximum Product of Splitted Binary Tree](https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/?show=1) | [1339. Tích lớn nhất khi tách cây nhị phân](https://leetcode.cn/problems/maximum-product-of-splitted-binary-tree/?show=1)🔒 | 🟠 |
| [1367. Linked List in Binary Tree](https://leetcode.com/problems/linked-list-in-binary-tree/?show=1) | [1367. Linked list trong cây nhị phân](https://leetcode.cn/problems/linked-list-in-binary-tree/?show=1) | 🟠 |
| [1372. Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/?show=1) | [1372. Đường zigzag dài nhất trong cây nhị phân](https://leetcode.cn/problems/longest-zigzag-path-in-a-binary-tree/?show=1) | 🟠 |
| [1373. Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/?show=1) | [1373. Tổng khóa-giá trị lớn nhất của cây con BST](https://leetcode.cn/problems/maximum-sum-bst-in-binary-tree/?show=1) | 🔴 |
| [1376. Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/?show=1) | [1376. Thời gian cần để thông báo mọi nhân viên](https://leetcode.cn/problems/time-needed-to-inform-all-employees/?show=1) | 🟠 |
| [1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree](https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/?show=1) | [1379. Tìm Node tương ứng trong cây clone](https://leetcode.cn/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/?show=1) | 🟢 |
| [1430. Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree](https://leetcode.com/problems/check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree/?show=1)🔒 | [1430. kiểm tra Chuỗi cho có phải đường từ gốc tới lá không](https://leetcode.cn/problems/check-if-a-string-is-a-valid-sequence-from-root-to-leaves-path-in-a-binary-tree/?show=1)🔒 | 🟠 |
| [1443. Minimum Time to Collect All Apples in a Tree](https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree/?show=1) | [1443. Thời gian ít nhất thu mọi táo trên cây](https://leetcode.cn/problems/minimum-time-to-collect-all-apples-in-a-tree/?show=1) | 🟠 |
| [1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/?show=1) | [1448. Đếm số Node tốt trong cây nhị phân](https://leetcode.cn/problems/count-good-nodes-in-binary-tree/?show=1) | 🟠 |
| [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/?show=1) | [145. Duyệt hậu thứ tự cây nhị phân](https://leetcode.cn/problems/binary-tree-postorder-traversal/?show=1) | 🟢 |
| [1457. Pseudo-Palindromic Paths in a Binary Tree](https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/?show=1) | [1457. Đường giả palindrome trong cây nhị phân](https://leetcode.cn/problems/pseudo-palindromic-paths-in-a-binary-tree/?show=1) | 🟠 |
| [1469. Find All The Lonely Nodes](https://leetcode.com/problems/find-all-the-lonely-nodes/?show=1)🔒 | [1469. Tìm mọi Node con một ](https://leetcode.cn/problems/find-all-the-lonely-nodes/?show=1)🔒 | 🟢 |
| [1485. Clone Binary Tree With Random Pointer](https://leetcode.com/problems/clone-binary-tree-with-random-pointer/?show=1)🔒 | [1485. Clone cây nhị phân mang con trỏ random](https://leetcode.cn/problems/clone-binary-tree-with-random-pointer/?show=1)🔒 | 🟠 |
| [1490. Clone N-ary Tree](https://leetcode.com/problems/clone-n-ary-tree/?show=1)🔒 | [1490. Clone cây N chạc](https://leetcode.cn/problems/clone-n-ary-tree/?show=1)🔒 | 🟠 |
| [1593. Split a String Into the Max Number of Unique Substrings](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/?show=1) | [1593. Tách chuỗi để số chuỗi con duy nhất max](https://leetcode.cn/problems/split-a-string-into-the-max-number-of-unique-substrings/?show=1) | 🟠 |
| [1602. Find Nearest Right Node in Binary Tree](https://leetcode.com/problems/find-nearest-right-node-in-binary-tree/?show=1)🔒 | [1602. Tìm Node phía phải gần nhất trong cây nhị phân](https://leetcode.cn/problems/find-nearest-right-node-in-binary-tree/?show=1)🔒 | 🟠 |
| [1612. Check If Two Expression Trees are Equivalent](https://leetcode.com/problems/check-if-two-expression-trees-are-equivalent/?show=1)🔒 | [1612. Kiểm tra hai cây biểu thức có tương đương không](https://leetcode.cn/problems/check-if-two-expression-trees-are-equivalent/?show=1)🔒 | 🟠 |
| [1740. Find Distance in a Binary Tree](https://leetcode.com/problems/find-distance-in-a-binary-tree/?show=1)🔒 | [1740. Tìm khoảng cách trong cây nhị phân](https://leetcode.cn/problems/find-distance-in-a-binary-tree/?show=1)🔒 | 🟠 |
| [2049. Count Nodes With the Highest Score](https://leetcode.com/problems/count-nodes-with-the-highest-score/?show=1) | [2049. Đếm số Node điểm cao nhất](https://leetcode.cn/problems/count-nodes-with-the-highest-score/?show=1) | 🟠 |
| [2096. Step-By-Step Directions From a Binary Tree Node to Another](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/?show=1) | [2096. Hướng từng bước từ Node này sang Node khác](https://leetcode.cn/problems/step-by-step-directions-from-a-binary-tree-node-to-another/?show=1) | 🟠 |
| [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/?show=1) | [226. Lật cây nhị phân](https://leetcode.cn/problems/invert-binary-tree/?show=1) | 🟢 |
| [250. Count Univalue Subtrees](https://leetcode.com/problems/count-univalue-subtrees/?show=1)🔒 | [250. Đếm cây con cùng giá trị ](https://leetcode.cn/problems/count-univalue-subtrees/?show=1)🔒 | 🟠 |
| [254. Factor Combinations](https://leetcode.com/problems/factor-combinations/?show=1)🔒 | [254. tổ hợp Nhân tử](https://leetcode.cn/problems/factor-combinations/?show=1)🔒 | 🟠 |
| [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/?show=1) | [257. Mọi đường của cây nhị phân](https://leetcode.cn/problems/binary-tree-paths/?show=1) | 🟢 |
| [267. Palindrome Permutation II](https://leetcode.com/problems/palindrome-permutation-ii/?show=1)🔒 | [267. Hoán vị palindrome II](https://leetcode.cn/problems/palindrome-permutation-ii/?show=1)🔒 | 🟠 |
| [270. Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/?show=1)🔒 | [270. Giá trị cây BST gần nhất](https://leetcode.cn/problems/closest-binary-search-tree-value/?show=1)🔒 | 🟢 |
| [294. Flip Game II](https://leetcode.com/problems/flip-game-ii/?show=1)🔒 | [294. Game lật II](https://leetcode.cn/problems/flip-game-ii/?show=1)🔒 | 🟠 |
| [298. Binary Tree Longest Consecutive Sequence](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/?show=1)🔒 | [298. Chuỗi liên tục dài nhất của cây nhị phân](https://leetcode.cn/problems/binary-tree-longest-consecutive-sequence/?show=1)🔒 | 🟠 |
| [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/?show=1) | [332. Sắp lại hành trình](https://leetcode.cn/problems/reconstruct-itinerary/?show=1) | 🔴 |
| [333. Largest BST Subtree](https://leetcode.com/problems/largest-bst-subtree/?show=1)🔒 | [333. Cây con BST lớn nhất](https://leetcode.cn/problems/largest-bst-subtree/?show=1)🔒 | 🟠 |
| [339. Nested List Weight Sum](https://leetcode.com/problems/nested-list-weight-sum/?show=1)🔒 | [339. Tổng trọng số danh sách lồng](https://leetcode.cn/problems/nested-list-weight-sum/?show=1)🔒 | 🟠 |
| [366. Find Leaves of Binary Tree](https://leetcode.com/problems/find-leaves-of-binary-tree/?show=1)🔒 | [366. Tìm Node lá của cây nhị phân](https://leetcode.cn/problems/find-leaves-of-binary-tree/?show=1)🔒 | 🟠 |
| [386. Lexicographical Numbers](https://leetcode.com/problems/lexicographical-numbers/?show=1) | [386. Số thứ tự thứ tự từ điển ](https://leetcode.cn/problems/lexicographical-numbers/?show=1)🔒 | 🟠 |
| [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/?show=1) | [404. Tổng lá trái](https://leetcode.cn/problems/sum-of-left-leaves/?show=1) | 🟢 |
| [426. Convert Binary Search Tree to Sorted Doubly Linked List](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/?show=1)🔒 | [426. Biến BST thành linked list đôi có thứ tự](https://leetcode.cn/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/?show=1)🔒 | 🟠 |
| [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/?show=1) | [437. Tổng đường III](https://leetcode.cn/problems/path-sum-iii/?show=1) | 🟠 |
| [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/?show=1) | [501. đông đếm trong BST](https://leetcode.cn/problems/find-mode-in-binary-search-tree/?show=1) | 🟢 |
| [508. Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/?show=1) | [508. Tổng cây con xuất hiện nhiều nhất](https://leetcode.cn/problems/most-frequent-subtree-sum/?show=1) | 🟠 |
| [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/?show=1) | [513. Tìm giá trị trái dưới của cây](https://leetcode.cn/problems/find-bottom-left-tree-value/?show=1) | 🟠 |
| [515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/?show=1) | [515. Tìm max mỗi hàng cây](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/?show=1) | 🟠 |
| [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/?show=1) | [530. Hiệu tuyệt đối nhỏ nhất trong BST](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/?show=1) | 🟢 |
| [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/?show=1) | [538. Biến BST thành cây tích lũy ](https://leetcode.cn/problems/convert-bst-to-greater-tree/?show=1) | 🟠 |
| [549. Binary Tree Longest Consecutive Sequence II](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/?show=1)🔒 | [549. Chuỗi liên tục dài nhất trong cây nhị phân](https://leetcode.cn/problems/binary-tree-longest-consecutive-sequence-ii/?show=1)🔒 | 🟠 |
| [559. Maximum Depth of N-ary Tree](https://leetcode.com/problems/maximum-depth-of-n-ary-tree/?show=1) | [559. Độ sâu lớn nhất của cây N chạc](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/?show=1) | 🟢 |
| [563. Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/?show=1) | [563. Độ nghiêng của cây nhị phân](https://leetcode.cn/problems/binary-tree-tilt/?show=1) | 🟢 |
| [572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/?show=1) | [572. Cây con của cây khác](https://leetcode.cn/problems/subtree-of-another-tree/?show=1) | 🟢 |
| [582. Kill Process](https://leetcode.com/problems/kill-process/?show=1)🔒 | [582. Kill tiến trình](https://leetcode.cn/problems/kill-process/?show=1)🔒 | 🟠 |
| [606. Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/?show=1) | [606. Dựng chuỗi từ cây nhị phân](https://leetcode.cn/problems/construct-string-from-binary-tree/?show=1) | 🟢 |
| [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/?show=1) | [617. trộn Hai cây nhị phân](https://leetcode.cn/problems/merge-two-binary-trees/?show=1) | 🟢 |
| [623. Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/?show=1) | [623. Thêm một hàng vào cây](https://leetcode.cn/problems/add-one-row-to-tree/?show=1) | 🟠 |
| [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/?show=1) | [654. Cây nhị phân lớn nhất](https://leetcode.cn/problems/maximum-binary-tree/?show=1) | 🟠 |
| [663. Equal Tree Partition](https://leetcode.com/problems/equal-tree-partition/?show=1)🔒 | [663. Chia cây đều ](https://leetcode.cn/problems/equal-tree-partition/?show=1)🔒 | 🟠 |
| [666. Path Sum IV](https://leetcode.com/problems/path-sum-iv/?show=1)🔒 | [666. Tổng đường IV](https://leetcode.cn/problems/path-sum-iv/?show=1)🔒 | 🟠 |
| [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/?show=1) | [669. Cắt BST](https://leetcode.cn/problems/trim-a-binary-search-tree/?show=1) | 🟠 |
| [671. Second Minimum Node In a Binary Tree](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/?show=1) | [671. Node nhỏ thứ hai trong cây nhị phân](https://leetcode.cn/problems/second-minimum-node-in-a-binary-tree/?show=1) | 🟢 |
| [687. Longest Univalue Path](https://leetcode.com/problems/longest-univalue-path/?show=1) | [687. Đường cùng giá trị dài nhất](https://leetcode.cn/problems/longest-univalue-path/?show=1) | 🟠 |
| [776. Split BST](https://leetcode.com/problems/split-bst/?show=1)🔒 | [776. Tách BST](https://leetcode.cn/problems/split-bst/?show=1)🔒 | 🟠 |
| [865. Smallest Subtree with all the Deepest Nodes](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/?show=1) | [865. Cây con nhỏ nhất chứa mọi Node sâu nhất](https://leetcode.cn/problems/smallest-subtree-with-all-the-deepest-nodes/?show=1) | 🟠 |
| [894. All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees/?show=1) | [894. Mọi cây nhị phân thật khả thi](https://leetcode.cn/problems/all-possible-full-binary-trees/?show=1) | 🟠 |
| [897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/?show=1) | [897. Cây tìm kiếm thứ tự tăng](https://leetcode.cn/problems/increasing-order-search-tree/?show=1) | 🟢 |
| [938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/?show=1) | [938. Tổng khoảng của BST](https://leetcode.cn/problems/range-sum-of-bst/?show=1) | 🟢 |
| [951. Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/?show=1) | [951. Cây nhị phân tương đương lật](https://leetcode.cn/problems/flip-equivalent-binary-trees/?show=1) | 🟠 |
| [965. Univalued Binary Tree](https://leetcode.com/problems/univalued-binary-tree/?show=1) | [965. Cây nhị phân đơn giá trị ](https://leetcode.cn/problems/univalued-binary-tree/?show=1) | 🟢 |
| [968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/?show=1) | [968. Camera cây nhị phân](https://leetcode.cn/problems/binary-tree-cameras/?show=1) | 🔴 |
| [971. Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/?show=1) | [971. Lật cây nhị phân để khớp duyệt tiền thứ tự ](https://leetcode.cn/problems/flip-binary-tree-to-match-preorder-traversal/?show=1) | 🟠 |
| [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/?show=1) | [979. Chia xu trong cây nhị phân](https://leetcode.cn/problems/distribute-coins-in-binary-tree/?show=1) | 🟠 |
| [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/?show=1) | [987. Duyệt vuông thứ tự cây nhị phân](https://leetcode.cn/problems/vertical-order-traversal-of-a-binary-tree/?show=1) | 🔴 |
| [988. Smallest String Starting From Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf/?show=1) | [988. Chuỗi nhỏ nhất từ Node lá](https://leetcode.cn/problems/smallest-string-starting-from-leaf/?show=1) | 🟠 |
| [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/?show=1) | [99. Phục hồi BST](https://leetcode.cn/problems/recover-binary-search-tree/?show=1) | 🟠 |
| [993. Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/?show=1) | [993. Node anh em họ trong cây nhị phân](https://leetcode.cn/problems/cousins-in-binary-tree/?show=1) | 🟢 |
| [998. Maximum Binary Tree II](https://leetcode.com/problems/maximum-binary-tree-ii/?show=1) | [998. Cây nhị phân lớn nhất II](https://leetcode.cn/problems/maximum-binary-tree-ii/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 06. In linked list từ cuối lên đầu](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 26. Cấu trúc con của cây](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 27. đối xứng Cây nhị phân](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 28. Cây nhị phân đối xứng](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 33. Chuỗi duyệt hậu thứ tự của BST](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 34. Đường tổng một giá trị trong cây nhị phân](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 36. BST và linked list đôi](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer 55 - I. Độ sâu của cây nhị phân](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer 55 - II. Cây nhị phân cân bằng](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer II 044. Max mỗi tầng cây nhị phân](https://leetcode.cn/problems/hPov7L/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 045. Giá trị trái dưới tầng đáy nhất](https://leetcode.cn/problems/LwUNpT/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 049. Tổng số đường từ gốc tới lá](https://leetcode.cn/problems/3Etpl5/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 050. Tổng Node đường về phía dưới ](https://leetcode.cn/problems/6eUYwP/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 051. Đường tổng Node lớn nhất](https://leetcode.cn/problems/jC7MId/?show=1) | 🔴 |
| - | [Kiếm Chỉ Offer II 052. Trải phẳng BST](https://leetcode.cn/problems/NYBBNL/?show=1) | 🟢 |
| - | [Kiếm Chỉ Offer II 054. Tổng mọi giá trị lớn hơn hoặc bằng Node](https://leetcode.cn/problems/w6cpku/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
