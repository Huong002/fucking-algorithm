# Học tư duy khung (framework) của cấu trúc dữ liệu và thuật toán

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho user cũ~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

**-----------**

> tip: Bài này có bản video: [Học tư duy khung của cấu trúc dữ liệu và thuật toán](https://www.bilibili.com/video/BV1EN4y1M79p/). Khuyên theo dõi tài khoản Bilibili của tôi, tôi sẽ dùng cách dẫn đọc bằng video để đưa mọi người học những kỹ thuật thuật toán hơi khó.

Đây là bản sửa của một bài [Học tư duy khung của cấu trúc dữ liệu và thuật toán](https://mp.weixin.qq.com/s/gE-5KMi4bBvJovdsQXIKgw) từ rất lâu trước. Bài trước đó nhận được đánh giá tốt rộng rãi, chưa xem cũng không sao, bài này sẽ bao hàm tất cả nội dung trước đó, đồng thời sẽ lấy rất nhiều ví dụ code, dạy bạn dùng tư duy khung thế nào.

Trước hết, chỗ này giảng đều là cấu trúc dữ liệu thông thường, chúng ta không phải thi thuật toán thi đấu, mục đích của chúng ta là nhanh chóng nâng cao năng lực thuật toán, bồi dưỡng tư duy thuật toán, thật không cần ra đề quá lệch quá quái. Ngoài ra, dưới đây là tổng kết kinh nghiệm cá nhân của tôi, không có sách thuật toán nào viết những thứ này, nên mong độc giả thử hiểu góc độ của tôi, đừng băn khoăn vào vấn đề chi tiết, vì bài này chính là hy vọng giúp bạn xây dựng một nhận thức mang tính khung với cấu trúc dữ liệu và thuật toán.

Tư duy khung từ tổng thể đến chi tiết, từ trên xuống dưới, từ trừu tượng đến cụ thể là tổng quát, không chỉ học cấu trúc dữ liệu và thuật toán, học bất kỳ kiến thức nào khác đều hiệu quả.

### Một, cách lưu trữ của cấu trúc dữ liệu

**Cách lưu trữ của cấu trúc dữ liệu chỉ có hai loại: mảng (lưu trữ tuần tự) và danh sách liên kết (lưu trữ móc xích)**.

Câu này hiểu thế nào, chẳng phải còn bảng băm, ngăn xếp, hàng đợi, heap, cây, đồ thị, v.v. đủ loại cấu trúc dữ liệu sao?

Chúng ta phân tích vấn đề, nhất định phải có tư duy đệ quy, từ trên xuống dưới, từ trừu tượng đến cụ thể. Bạn vừa lên đã liệt kê nhiều như vậy, những cái đó đều thuộc "kiến trúc thượng tầng", mà mảng và danh sách liên kết mới là "cơ sở kết cấu". Vì những cấu trúc dữ liệu đa dạng đó, xét đến cùng, đều là thao tác đặc biệt trên danh sách liên kết hoặc mảng, chỉ khác API mà thôi.

Ví dụ "hàng đợi", "ngăn xếp" hai cấu trúc dữ liệu này vừa có thể dùng danh sách liên kết cũng có thể dùng mảng cài đặt. Dùng mảng cài đặt, sẽ cần xử lý vấn đề mở rộng/thu hẹp; dùng danh sách liên kết cài đặt, không có vấn đề này, nhưng cần nhiều không gian bộ nhớ hơn để lưu con trỏ nút.

Hai cách biểu diễn của "đồ thị", danh sách kề chính là danh sách liên kết, ma trận kề chính là mảng hai chiều. Ma trận kề kiểm tra tính liên thông nhanh chóng, và có thể tiến hành phép toán ma trận để giải một số vấn đề, nhưng nếu đồ thị khá thưa thì rất tốn không gian. Danh sách kề khá tiết kiệm không gian, nhưng nhiều thao tác về hiệu suất chắc chắn kém hơn ma trận kề.

"Bảng băm" chính là thông qua hàm băm ánh xạ khóa vào một mảng lớn. Mà với phương pháp giải xung đột băm, phương pháp móc xích cần đặc tính danh sách liên kết, thao tác đơn giản, nhưng cần thêm không gian để lưu con trỏ; phương pháp dò tuyến tính sẽ cần đặc tính mảng để định vị liên tục, không cần không gian lưu con trỏ, nhưng thao tác hơi phức tạp.

"Cây", dùng mảng cài đặt chính là "heap", vì "heap" là một cây nhị phân đầy đủ, dùng mảng lưu không cần con trỏ nút, thao tác cũng khá đơn giản; dùng danh sách liên kết cài đặt chính là loại "cây" rất thường gặp, vì không nhất định là cây nhị phân đầy đủ, nên không phù hợp dùng mảng lưu. Vì thế, trên cấu trúc "cây" danh sách liên kết này, lại phát sinh ra đủ loại thiết kế khéo léo, như cây tìm kiếm nhị phân, cây AVL, cây đỏ-đen, cây đoạn, cây B, v.v., để ứng đối vấn đề khác nhau.

Bạn quen thuộc cơ sở dữ liệu Redis có thể cũng biết, Redis cung cấp vài cấu trúc dữ liệu thường dùng như danh sách, chuỗi, tập hợp, nhưng với mỗi cấu trúc dữ liệu, cách lưu tầng đáy đều có ít nhất hai loại, để căn cứ tình hình thực tế dữ liệu lưu mà dùng cách lưu phù hợp.

Tổng hợp, loại cấu trúc dữ liệu rất nhiều, thậm chí bạn cũng có thể phát minh cấu trúc dữ liệu của mình, nhưng lưu tầng đáy chẳng qua cũng chỉ là mảng hoặc danh sách liên kết, **ưu khuyết điểm của hai loại như sau**:

**Mảng** do lưu liền kề liên tục, có thể truy cập ngẫu nhiên, thông qua chỉ số tìm nhanh phần tử tương ứng, mà tương đối tiết kiệm không gian lưu. Nhưng chính vì lưu liên tục, không gian bộ nhớ bắt buộc phải cấp một lần cho đủ, nên nói mảng nếu muốn mở rộng, cần cấp lại một vùng không gian lớn hơn, rồi đem toàn bộ dữ liệu copy qua, độ phức tạp thời gian O(N); hơn nữa nếu bạn muốn ở giữa mảng tiến hành chèn và xóa, mỗi lần bắt buộc di chuyển tất cả dữ liệu phía sau để giữ liên tục, độ phức tạp thời gian O(N).

**Danh sách liên kết** vì phần tử không liên tục, mà dựa vào con trỏ chỉ vị trí phần tử tiếp theo, nên không tồn tại vấn đề mở rộng của mảng; nếu biết nút trước và nút sau của một phần tử nào đó, thao tác con trỏ là có thể xóa phần tử đó hoặc chèn phần tử mới, độ phức tạp thời gian O(1). Nhưng chính vì không gian lưu không liên tục, bạn không thể căn cứ một chỉ số tính ra địa chỉ phần tử tương ứng, nên không thể truy cập ngẫu nhiên; hơn nữa do mỗi phần tử bắt buộc lưu con trỏ chỉ vị trí phần tử trước sau, sẽ tốn tương đối nhiều không gian lưu hơn.

### Hai, thao tác cơ bản của cấu trúc dữ liệu

Với bất kỳ cấu trúc dữ liệu nào, thao tác cơ bản của nó chẳng qua cũng chỉ là duyệt + truy cập, cụ thể hơn chính là: thêm xóa tìm sửa.

**Loại cấu trúc dữ liệu rất nhiều, nhưng mục đích chúng tồn tại đều là trong cảnh ứng dụng khác nhau, thêm xóa tìm sửa hiệu quả nhất có thể**. Nói chuyện này chẳng phải chính là sứ mệnh của cấu trúc dữ liệu sao?

Duyệt + truy cập thế nào? Chúng ta vẫn nhìn từ tầng cao nhất, duyệt + truy cập của đủ loại cấu trúc dữ liệu chẳng qua cũng chỉ là hai dạng: tuyến tính và phi tuyến.

Tuyến tính chính là đại diện lặp for/while, phi tuyến chính là đại diện đệ quy. Cụ thể thêm một bước, chẳng qua cũng chỉ là mấy khung sau:

Khung duyệt mảng, cấu trúc lặp tuyến tính điển hình:

<!-- muliti_language -->
```java
void traverse(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        // Lặp truy cập arr[i]
    }
}
```

Khung duyệt danh sách liên kết, vừa có cấu trúc lặp và đệ quy:

<!-- muliti_language -->
```java
/* Nút danh sách đơn cơ bản */
class ListNode {
    int val;
    ListNode next;
}

void traverse(ListNode head) {
    for (ListNode p = head; p != null; p = p.next) {
        // Lặp truy cập p.val
    }
}

void traverse(ListNode head) {
    // Đệ quy truy cập head.val
    traverse(head.next);
}
```

Khung duyệt cây nhị phân, cấu trúc duyệt đệ quy phi tuyến điển hình:

<!-- muliti_language -->
```java
/* Nút cây nhị phân cơ bản */
class TreeNode {
    int val;
    TreeNode left, right;
}

void traverse(TreeNode root) {
    traverse(root.left);
    traverse(root.right);
}
```

Bạn xem cách duyệt đệ quy của cây nhị phân và cách duyệt đệ quy của danh sách liên kết, có giống không? Nhìn lại cấu trúc cây nhị phân và cấu trúc danh sách đơn, có giống không? Nếu thêm vài nhánh, cây N phân bạn có sẽ duyệt không?

Khung cây nhị phân có thể mở rộng thành khung duyệt cây N phân:

<!-- muliti_language -->
```java
/* Nút cây N phân cơ bản */
class TreeNode {
    int val;
    TreeNode[] children;
}

void traverse(TreeNode root) {
    for (TreeNode child : root.children)
        traverse(child);
}
```

Duyệt cây `N` phân lại có thể mở rộng thành duyệt đồ thị, vì đồ thị chính là vài cây `N` phân kết hợp lại. Bạn nói đồ thị có thể xuất hiện chu trình? Chuyện này rất dễ, dùng một mảng bool `visited` làm đánh dấu là được, chỗ này sẽ không viết code.

**Cái gọi là khung, chính là công thức. Bất kể thêm xóa tìm sửa, những code này đều mãi mãi không thể tách rời kết cấu**, bạn có thể lấy kết cấu này làm cương lĩnh lớn, căn cứ vấn đề cụ thể thêm code trên khung là được, dưới đây sẽ lấy ví dụ cụ thể.

### Ba, hướng dẫnluyện đề thuật toán

Trước hết cần làm rõ, cấu trúc dữ liệu là công cụ, thuật toán là thông qua công cụ phù hợp giải phương pháp của vấn đề cụ thể. Tức là nói, trước khi học thuật toán, chí ít phải hiểu rõ những cấu trúc dữ liệu thường dùng, hiểu rõ đặc tính và khuyết điểm của chúng.

Nên thứ tựluyện đề tôi khuyên là:

**1, Học trước thuật toán thường dùng của cấu trúc dữ liệu cơ bản như mảng, danh sách liên kết**, ví dụ lật danh sách đơn, mảng tổng tiền tố, tìm kiếm nhị phân, v.v.

Vì những thuật toán này thuộc loại biết rồi thì không thấy khó, chưa biết thì thấy khó, độ khó không lớn, học chúng sẽ không tốn nhiều thời gian. Mà những thuật toán nhỏ mà đẹp này thường khiến bạn trầm trồ khen tinh diệu, có thể bồi dưỡng hiệu quả hứng thú của bạn với thuật toán.

**2, Học xong thuật toán cơ bản, đừng vội vừa lên đã luyện đề thường thi như thuật toán quay lui, quy hoạch động, mà hãy làm cây nhị phân trước, làm cây nhị phân trước, làm cây nhị phân trước**, chuyện quan trọng nói ba lần.

::: tip Gợi ý

Trên Lực khấu có phân loại đề cây nhị phân chuyên:

[https://leetcode.cn/tag/binary-tree/](https://leetcode.cn/tag/binary-tree/)

:::

Đây là thể hội đích thân nhiều năm luyện đề của tôi, hình dưới là ảnh chụp lần nộp bài lúc tôi mới bắt đầu học thuật toán:

![](https://labuladong.online/algo/images/others/leetcode.jpeg)

Dữ liệu đọc bàikênh WeChat chính thức cho thấy, đa số người với bài thuật toán liên quan cấu trúc dữ liệu không hứng thú, mà quan tâm hơn đến kỹ thuật như động quy quay lui chia trị. Tại sao cần làm cây nhị phân trước, **vì cây nhị phân dễ nhất bồi dưỡng tư duy khung, mà tất cả kỹ thuật thuật toán đệ quy, bản chất đều là vấn đề duyệt cây**.

Làm cây nhị phân thấy đề không có ý tưởng? Căn cứ vấn đề của nhiều độc giả, thực ra mọi người không phải không có ý tưởng, chỉ là không hiểu "khung" mà chúng ta nói là gì.

**Đừng xem thường vài hàng code rách này, gần như tất cả đề cây nhị phân đều một bộ khung này là ra**:

<!-- muliti_language -->
```java
void traverse(TreeNode root) {
    // Vị trí tiền thứ
    traverse(root.left);
    // Vị trí trung thứ
    traverse(root.right);
    // Vị trí hậu thứ
}
```

Ví dụ tôi tùy ý lấy cách giải vài bài đề ra, không cần quản logic code cụ thể, chỉ cần xem khung trong đó phát huy tác dụng thế nào là được.

LeetCode 124, độ khó khó, bắt bạn tìm tổng đường đi lớn nhất trong cây nhị phân, code chính như sau:

<!-- muliti_language -->
```java
int res = Integer.MIN_VALUE;
int oneSideMax(TreeNode root) {
    if (root == null) return 0;
    int left = max(0, oneSideMax(root.left));
    int right = max(0, oneSideMax(root.right));
    // Vị trí hậu thứ
    res = Math.max(res, left + right + root.val);
    return Math.max(left, right) + root.val;
}
```

Chú ý vị trí hàm đệ quy, đây chính là một duyệt hậu thứ, chẳng qua cũng chỉ là chính là đem tên hàm `traverse` đổi thành `oneSideMax` mà thôi.

LeetCode 105, độ khó trung bình, bắt bạn căn cứ kết quả duyệt tiền thứ và trung thứ khôi phục một cây nhị phân, vấn đề rất kinh điển, code chính như sau:

<!-- muliti_language -->
```java
TreeNode build(int[] preorder, int preStart, int preEnd,
               int[] inorder, int inStart, int inEnd) {
    // Vị trí tiền thứ, tìm chỉ số cây con trái phải
    if (preStart > preEnd) {
        return null;
    }
    int rootVal = preorder[preStart];
    int index = 0;
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == rootVal) {
            index = i;
            break;
        }
    }
    int leftSize = index - inStart;
    TreeNode root = new TreeNode(rootVal);

    // Đệ quy dựng cây con trái phải
    root.left = build(preorder, preStart + 1, preStart + leftSize,
                      inorder, inStart, index - 1);
    root.right = build(preorder, preStart + leftSize + 1, preEnd,
                       inorder, index + 1, inEnd);
    return root;
}
```

Đừng xem tham số hàm này nhiều, chỉ là để khống chế chỉ số mảng mà thôi. Chú ý tìm vị trí hàm đệ quy `build`, bản chất thuật toán này cũng chính là một duyệt tiền thứ, vì nó ở vị trí duyệt tiền thứ thêm một đống logic code.

LeetCode 230, độ khó trung bình, tìm phần tử nhỏ thứ `k` trong cây tìm kiếm nhị phân, code chính như sau:

<!-- muliti_language -->
```java
int res = 0;
int rank = 0;
void traverse(TreeNode root, int k) {
    if (root == null) {
        return;
    }
    traverse(root.left, k);
    /* Vị trí code duyệt trung thứ */
    rank++;
    if (k == rank) {
        res = root.val;
        return;
    }
    /*****************/
    traverse(root.right, k);
}
```

Đây chẳng phải chính là một duyệt trung thứ, với một cây BST duyệt trung thứ nghĩa là gì, hẳn không cần giải thích rồi.

Bạn xem, đề cây nhị phân chẳng qua cũng chỉ có vậy, chỉ cần đem khung viết ra, rồi tới vị trí tương ứng thêm code là được, đây chẳng phải chính là ý tưởng sao.

Với một người hiểu cây nhị phân, làm một đề cây nhị phân tốn không bao lâu. Vậy nếu bạn với luyện đề không biết bắt đầu từ đâu hoặc có tâm lý sợ hãi, không ngại bắt đầu từ cây nhị phân, 10 đề đầu có lẽ hơi khó chịu; kết hợp khung làm thêm 20 đề, có lẽ bạn sẽ có chút hiểu của mình; làm xong toàn bộ chuyên đề, rồi đi làm chuyên đề quay lui động quy chia trị, **bạn sẽ phát hiện chỉ cần liên quan vấn đề đệ quy, đều là vấn đề cây**.

::: tip

[Chương luyện tập chuyên đề cây nhị phân](https://labuladong.online/algo/problem-set/binary-tree-traverse-1/) của gốc trạm theo công thức và mô thức tư duy cố địnhgiảng 150 đề cây nhị phân, có thể cầm tay đưa bạn làm hết đề phân loại cây nhị phân, nhanh chóng nắm tư duy đệ quy.

:::

Lấy ví dụ nữa, nói vài bài vấn đề bài viết trước của chúng ta từng viết.

[giải chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) từng nói bài toán gom tiền lẻ, cách giải vét cạn chính là duyệt một cây N phân:

![](https://labuladong.online/algo/images/动态规划详解进阶/5.jpg)

<!-- muliti_language -->
```java
int dp(int[] coins, int amount) {
    // base case
    if (amount == 0) return 0;
    if (amount < 0) return -1;

    int res = Integer.MAX_VALUE;
    for (int coin : coins) {
        int subProblem = dp(coins, amount - coin);
        // Bài toán con vô nghiệm thì bỏ qua
        if (subProblem == -1) continue;
        // Trong bài toán con chọn giải tối ưu, rồi cộng một
        res = Math.min(res, subProblem + 1);
    }
    return res == Integer.MAX_VALUE ? -1 : res;
}
```

Nhiều code như vậy xem không hiểu thì làm sao? Trực tiếp rút ra khung, là có thể nhận ra ý tưởng cốt lõi:

<!-- muliti_language -->
```python
# Chẳng qua là một bài toán duyệt cây N phân mà thôi
int dp(int amount) {
    for (int coin : coins) {
        dp(amount - coin);
    }
}
```

Thực ra nhiều bài toán quy hoạch động chính là đang duyệt một cây, nếu bạn với thao tác duyệt cây nằm lòng, chí ít biết sao đem ý tưởng chuyển thành code, cũng biết sao rútý tưởng cốt lõi của cách giải người khác.

Nhìn lại thuật toán quay lui, bài trước [giải chi tiết thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/) dứt khoát trực tiếp nói, thuật toán quay lui chính là một bài toán duyệt tiền-hậu thứ của cây N phân, không có ngoại lệ.

Ví như bài toán hoán vị đầy đủ, bản chất hoán vị đầy đủ chính là đang duyệt cây dưới đây, đường đi đến nút lá chính là một hoán vị đầy đủ:

![](https://labuladong.online/algo/images/backtracking/1.jpg)

Code chính của thuật toán hoán vị đầy đủ như sau:

<!-- muliti_language -->
```java
void backtrack(int[] nums, LinkedList<Integer> track) {
    if (track.size() == nums.length) {
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (track.contains(nums[i]))
            continue;
        track.add(nums[i]);
        // Vào cây quyết định tầng tiếp theo
        backtrack(nums, track);
        track.removeLast();
    }
}
```

Xem không hiểu? Không sao, rút phần đệ quy trong đó ra:

<!-- muliti_language -->
```java
/* Rút ra khung duyệt cây N phân */
void backtrack(int[] nums, LinkedList<Integer> track) {
    for (int i = 0; i < nums.length; i++) {
        backtrack(nums, track);
}
```

Khung duyệt cây N phân, tìm ra rồi? Bạn nói, cấu trúc cây có trọng không?

**Tổng hợp, với bạn sợ thuật toán, có thểluyện đề liên quan cây trước, thử nhìn vấn đề từ khung, mà đừng băn khoăn vào vấn đề chi tiết**.

băn khoăn vấn đề chi tiết, sẽ ví dụ băn khoăn `i` rốt cuộc hẳn cộng đến `n` hay cộng đến `n - 1`, kích thước mảng này rốt cuộc hẳn mở `n` hay `n + 1`?

Nhìn vấn đề từ khung, chính là giống chúng ta như vậy dựa trên khung tiến hành rút ra và mở rộng, vừa có thể khi xem cách giải người khác nhanh chóng hiểu logic cốt lõi, cũng có ích trong việc tìm hướng ý tưởng khi chúng ta tự viết cách giải.

Đương nhiên, nếu chi tiết sai, bạn không nhận được đáp án đúng, nhưng chỉ cần có khung, bạn sai nữa cũng sai không đến đâu, vì hướng của bạn đúng.

Nhưng, nếu trong lòng bạn không có khung, vậy bạn căn bản không thể giải đề, cho bạn đáp án, bạn cũng sẽ không phát hiện đây chính là một bài toán duyệt cây.

Tư duy này rất quan trọng, trong [giải chi tiết quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) tổng kết mấy bước quy trình tìm phương trình chuyển trạng thái, có lúc theo quy trình viết ra cách giải, có thể tự mình cũng không biết tại sao đúng, dù sao nó chính là đúng...

**Đây chính là sức mạnh của khung, có thể đảm bảo lúc bạn nhanh ngủ gật, vẫn có thể viết ra chương trình đúng; dù bạn gì cũng không sẽ, đều có thể hơn người khác cao một cấp**.

Cuối bài này, tổng kết lại:

Cách lưu cơ bản của cấu trúc dữ liệu chính là móc xích và tuần tự hai loại, thao tác cơ bản chính là thêm xóa tìm sửa, cách duyệt chẳng qua cũng chỉ là lặp và đệ quy.

Học xong thuật toán cơ bản, khuyên bắt đầu làm từ vấn đề loạt "cây nhị phân", kết hợp tư duy khung, đem cấu trúc cây hiểu cho thấu, rồi đi xem chuyên đề quay lui, động quy, chia trị, hiểu ý tưởng sẽ càng sâu sắc.

<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Mẫu thuật toán Dijkstra và ứng dụng](https://labuladong.online/algo/data-structure/dijkstra/)
 - [Một bàixử gọn tất cả đề đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/)
 - [Đông ca đưa bạn làm cây nhị phân (loạt tuần tự hóa)](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/)
 - [Đông ca đưa bạn làm cây nhị phân (loạt cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
 - [Thuật toánkiểm tra đồ thị hai phía](https://labuladong.online/algo/data-structure/bipartite-graph/)
 - [Nguyên lý cơ bản heap nhị phân](https://labuladong.online/algo/data-structure-basic/binary-heap-basic/)
 - [Mẫu thuật toán cây tiền tốxử gọn năm đạo đề](https://labuladong.online/algo/data-structure/trie/)
 - [Quay luixử gọn mọi vấn đề hoán vị/tổ hợp/tập con](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)
 - [Khungcông thức giải bằng thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/)
 - [Cơ bản lý thuyết đồ thị và thuật toán duyệt](https://labuladong.online/algo/data-structure/graph-traverse/)
 - [Làm sao đảo danh sách liên kết theo nhóm K](https://labuladong.online/algo/data-structure/reverse-nodes-in-k-group/)
 - [Làm saokiểm tra danh sách liên kếtpalindrome](https://labuladong.online/algo/data-structure/palindrome-linked-list/)
 - [Giải thích chi tiết sắp xếp trộn và ứng dụng](https://labuladong.online/algo/practice-in-action/merge-sort/)
 - [Tâm đắcluyện đề của tôi: Bản chất thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Nguyên lý cơ bản mảng (lưu tuần tự)](https://labuladong.online/algo/data-structure-basic/array-basic/)
 - [Bàn sơ về hệ thống lưu trữ: Nguyên lý thiết kế cây LSM](https://labuladong.online/algo/other-skills/lsm-tree/)
 - [Phát hiện chu trình và thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)
 - [Dùng ngăn xếp mô phỏng đệ quy lặp duyệt cây nhị phân](https://labuladong.online/algo/data-structure/iterative-traversal-binary-tree/)
 - [Dùng danh sách liên kết cài đặt hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/linked-queue-stack/)
 - [Vấn đề tổng mục tiêu: Biến thể ba lô](https://labuladong.online/algo/dynamic-programming/target-sum/)
 - [Học thuật toán và trải nghiệm dòng chảy](https://labuladong.online/algo/other-skills/hert-flow/)
 - [Hướng dẫn thực dụng phân tích độ phức tạp thời-không thuật toán](https://labuladong.online/algo/essential-technique/complexity-analysis/)
 - [Đề không cho tôi làm gì, tôi cứ làm nấy](https://labuladong.online/algo/data-structure/flatten-nested-list-iterator/)

</details><hr>

<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của tôi](https://labuladong.online/algo/intro/chrome/) bấm vào các đề sau có thể xem trực tiếpý tưởng giải:</strong>

| LeetCode | Lực khấu |
| :----: | :----: |
| [341. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/?show=1) | [341. Làm phẳng trình lặp danh sách lồng](https://leetcode.cn/problems/flatten-nested-list-iterator/?show=1) |
| [589. N-ary Tree Preorder Traversal](https://leetcode.com/problems/n-ary-tree-preorder-traversal/?show=1) | [589. Duyệt tiền thứ cây N phân](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/?show=1) |
| [590. N-ary Tree Postorder Traversal](https://leetcode.com/problems/n-ary-tree-postorder-traversal/?show=1) | [590. Duyệt hậu thứ cây N phân](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/?show=1) |

</details>
<hr>

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

** “ Ghi chép thuật toán của labuladong ” đã xuất bản, theo dõikênh WeChat chính thức xem chi tiết; nhắn tin tới hộp thư "** toàn **" có thể tải PDFđi kèm vàbộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======
