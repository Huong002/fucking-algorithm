# Cơ bản thuật toán đồ thị

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ user cũ gia hạn~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/) | [797. Mọi đường đi có thể](https://leetcode.cn/problems/all-paths-from-source-to-target/) | 🟠
| - | [Kiếm Chỉ Offer II 110. Mọi đường đi](https://leetcode.cn/problems/bP4bmD/) | 🟠

**-----------**

> tip: Bài này có bản video: [Cơ bản đồ thị & thuật toán duyệt](https://www.bilibili.com/video/BV19G41187cL/). Khuyên follow tài khoản B đứng của tôi, tôi sẽ dẫn mọi người học kỹ thuật thuật toán hơi khó bằng cách đọc kèm video.



Thường có độc giả hỏi tôi cấu trúc dữ liệu「đồ thị (graph)」, thực ra tôi ở [Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/) đã nói, dù đồ thị có thể tạo ra nhiều thuật toán hơn, giải vấn đề phức tạp hơn, nhưng bản chất đồ thị có thể coi là mở rộng của cây đa chạc.

Phỏng vấn bài kiểm tra hiếm xuất hiện vấn đề liên quan đồ thị, dù có, đa số cũng là bài duyệt đơn giản, cơ bản có thể bê nguyên duyệt cây đa chạc.

Vậy, bài này vẫn giữ phong cách của kênh, chỉ giảng phần thực dụng nhất, gần chúng ta nhất của「đồ thị」, để bạn có nhận thức trực quan về đồ thị, cuối bài tôi cho ra thuật toán đồ thị kinh điển khác, hiểu bài này sau phải đều lấy dưới được.

### Cấu trúc logic và cài đặt cụ thể của đồ thị

Một đồ thị gồm **Node** và **cạnh** cấu thành, cấu trúc logic như sau:

![](https://labuladong.online/algo/images/图/0.jpg)

**Gì gọi 「cấu trúc logic」? Tức là để tiện nghiên cứu, chúng ta đồ thị trừu tượng thành bộ dạng này**.

Dựa vào cấu trúc logic này, chúng ta có thể cho rằng cài đặt mỗi Node như sau:

<!-- muliti_language -->
```java
/* Cấu trúc logic của Node đồ thị */
class Vertex {
    int id;
    Vertex[] neighbors;
}
```

Thấy cài đặt này, bạn có quen không? Nó gần như giống hệt Node cây đa chạc chúng ta nói trước đó:

<!-- muliti_language -->
```java
/* Node cây N chạc cơ bản */
class TreeNode {
    int val;
    TreeNode[] children;
}
```

Cho nên, đồ thị thực không có gì cao sâu, bản chất chính là cây đa chạc cao cấp hơn mà thôi, thuật toán duyệt DFS/BFS áp dụng cho cây, toàn bộ áp dụng cho đồ thị.

Nhưng, cài đặt trên là「về logic」, thực tế chúng ta hiếm dùng lớp `Vertex` này cài đặt đồ thị, mà dùng **danh sách kề và ma trận kề** thường nói để cài đặt.

Ví dụ vẫn đồ thị vừa rồi:

![](https://labuladong.online/algo/images/图/0.jpg)

Cách lưu danh sách kề và ma trận kề như sau:

![](https://labuladong.online/algo/images/图/2.jpeg)

Danh sách kề rất trực quan, tôi lưu lân cận mỗi Node `x` vào một danh sách, rồi liên kết `x` với danh sách này, như vậy là có thể qua một Node `x` tìm mọi Node kề của nó.

Ma trận kề thì là một mảng boolean hai chiều, chúng ta tạm gọi là `matrix`, nếu Node `x` và `y` kề nhau, vậy thì `matrix[x][y]` đặt thành `true` (ô xanh lá ở hình trên đại diện `true`). Nếu muốn tìm lân cận của Node `x`, đi quét một vòng `matrix[x][..]` là được.

Nếu dùng dạng code để thể hiện, danh sách kề và ma trận kề khoảng dài thế này:

<!-- muliti_language -->
```java
// Danh sách kề
// graph[x] lưu mọi Node lân cận của x
List<Integer>[] graph;

// Ma trận kề
// matrix[x][y] ghi x có cạnh trỏ tới y không
boolean[][] matrix;
```

**Vậy, tại sao có hai cách lưu đồ thị này? chắc chắn vì chúng các có ưu kém **.

Với danh sách kề, lợi là tốn ít không gian.

Bạn xem ma trận kề trống nhiều vị trí vậy, chắc chắn cần nhiều không gian lưu hơn.

Nhưng, danh sách kề không kiểm tra nhanh hai Node có kề nhau không.

Ví dụ tôi muốn kiểm tra Node `1` và Node `3` có kề nhau không, tôi cần đi trong danh sách lân cận `1` tương ứng ở danh sách kề tìm `3` có tồn tại không. Nhưng với ma trận kề thì đơn giản, chỉ cần xem `matrix[1][3]` là biết, hiệu suất cao.

Cho nên, dùng cách nào cài đặt đồ thị, cần xem tình huống cụ thể.

::: tip

Trong bài thuật toán thông thường, dùng danh sách kề sẽ thường xuyên hơn, chủ yếu vì thao tác dậy so cho đơn giản, nhưng không có nghĩa là ma trận kề phải được xem nhẹ. Ma trận là một công cụ toán mạnh, vài tính chất mơ hồ của đồ thị có thể mượn tính ma trận tinh diệu thể hiện ra. Nhưng bài này không chuẩn bị dẫn vào nội dung toán, nên độc giả hứng thú có thể tự tìm học.

:::

Cuối cùng, chúng ta rõ ràng thêm khái niệm **bậc** (degree) đặc có trong lý thuyết đồ thị, trong đồ thị vô hướng,「bậc」chính là số cạnh mỗi Node kề nhau .

Vì cạnh của đồ thị có hướng có hướng, nên trong đồ thị có hướng「bậc」mỗi Node được chia thành **bậc vào** (indegree) và **bậc ra** (outdegree), ví dụ hình dưới:

![](https://labuladong.online/algo/images/图/0.jpg)

Trong đó bậc vào của Node `3` là 3 (có ba cạnh trỏ tới nó), bậc ra là 1 (nó có 1 cạnh trỏ tới Node khác).

Tốt rồi, với cấu trúc dữ liệu「đồ thị」này, hiểu những cái trên thì quá đủ.

Vậy bạn có thể hỏi, mô hình đồ thị chúng ta nói trên chỉ là「đồ thị có hướng không trọng số」, không phải còn đồ thị có trọng số, đồ thị vô hướng, vân vân...

**Thực ra, những mô hình phức tạp hơn này đều dựa trên đồ thị đơn giản nhất này phái sinh ra**.

**Đồ thị có hướng có trọng số cài đặt sao**? Rất đơn giản :

Nếu là danh sách kề, chúng ta không chỉ lưu mọi Node lân cận của một Node `x` nào đó, còn lưu trọng số từ `x` tới mỗi lân cận, không phải cài đặt đồ thị có hướng có trọng số sao?

Nếu là ma trận kề, `matrix[x][y]` không còn là boolean, mà là một giá trị int, 0 biểu diễn không kề nhau, giá trị khác biểu diễn trọng số, không phải thành đồ thị có hướng có trọng số sao?

Nếu dùng dạng code để thể hiện, khoảng dài thế này:

<!-- muliti_language -->
```java
// Danh sách kề
// graph[x] lưu mọi Node lân cận của x và trọng số tương ứng
List<int[]>[] graph;

// Ma trận kề
// matrix[x][y] ghi trọng số cạnh x trỏ tới y, 0 biểu diễn không kề
int[][] matrix;
```

**Đồ thị vô hướng cài đặt sao**? Cũng rất đơn giản, cái gọi là「vô hướng」, có phải tương đương 「hai chiều」?

![](https://labuladong.online/algo/images/图/3.jpeg)

Nếu nối Node `x` và `y` trong đồ thị vô hướng, `matrix[x][y]` và `matrix[y][x]` đều thành `true` không phải được; danh sách kề cũng thao tác tương tự, thêm `y` trong danh sách lân cận của `x`, đồng thời thêm `x` trong danh sách lân cận của `y`.

Hợp kỹ thuật trên lại, thì thành đồ thị vô hướng có trọng số...

Tốt rồi, giới thiệu cơ bản về đồ thị tới đây, giờ dù đồ thị lộn xộn gì, trong lòng bạn phải đều có đáy .

Dưới đây xem vấn đề mọi cấu trúc dữ liệu đều không thoát được: duyệt.

### Duyệt đồ thị

**[Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/abstraction-of-algorithm/) nói, các cấu trúc dữ liệu được phát minh ra chẳng qua để duyệt và truy cập, nên「duyệt」là cơ bản của mọi cấu trúc dữ liệu**.

Đồ thị duyệt sao? Vẫn câu đó, tham khảo cây đa chạc, khung duyệt DFS cây đa chạc như sau:

<!-- muliti_language -->
```java
/* Khung duyệt cây đa chạc */
void traverse(TreeNode root) {
    if (root == null) return;
    // Vị trí tiền thứ tự 
    for (TreeNode child : root.children) {
        traverse(child);
    }
    // Vị trí hậu thứ tự 
}
```

Khác biệt lớn nhất của đồ thị và cây đa chạc là, đồ thị có thể chứa vòng, bạn duyệt bắt đầu từ một Node nào của đồ thị, có thể đi một vòng lại về Node này, mà cây không xuất hiện tình huống này, xuất phát từ một Node ắt nhiên đi tới Node lá, tuyệt không thể về chính nó.

Nên, nếu đồ thị chứa vòng, khung duyệt thì cần một mảng `visited` phụ trợ:

<!-- muliti_language -->
```java
// Ghi Node được duyệt qua
boolean[] visited;
// Ghi đường đi từ đỉnh xuất phát tới Node hiện tại
boolean[] onPath;

/* Khung duyệt đồ thị */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // Đi qua Node s, đánh dấu đã duyệt
    visited[s] = true;
    // Làm lựa chọn: đánh dấu Node s trên đường đi
    onPath[s] = true;
    for (int neighbor : graph.neighbors(s)) {
        traverse(graph, neighbor);
    }
    // Hủy lựa chọn: Node s rời đường đi
    onPath[s] = false;
}
```

Chú ý khác biệt của mảng `visited` và mảng `onPath`, vì cây nhị phân tính là đồ thị đặc biệt, nên dùng quá trình duyệt cây nhị phân để hiểu khác biệt hai mảng này:

![](https://labuladong.online/algo/images/迭代遍历二叉树/1.gif)

**GIF trên mô tả quá trình đệ quy duyệt cây nhị phân, Node được đánh dấu true trong `visited` dùng màu xám biểu diễn, Node được đánh dấu true trong `onPath` dùng màu xanh biểu diễn **, loại suy game rắn tham ăn, `visited` ghi ô rắn đi qua, mà `onPath` chỉ ghi thân rắn. Trong quá trình duyệt đồ thị, `onPath` dùng kiểm tra có thành vòng không, loại suy cảnh rắn tự cắn mình (thành vòng), giờ bạn hiểu khác biệt hai người rồi chứ.

Nếu bắt bạn xử lý vấn đề liên quan đường đi, biến `onPath` này chắc chắn sẽ được dùng, ví dụ [sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/) thì có dùng.

Ngoài ra, bạn phải chú ý, thao tác mảng `onPath` này rất giống [ khuôn mẫu cốt lõi thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/) làm「làm lựa chọn」và「hủy lựa chọn」, khác ở vị trí:「làm lựa chọn」và「hủy lựa chọn」của thuật toán quay lui trong vòng for, mà thao tác với mảng `onPath` ngoài vòng for.

Tại sao có khác biệt này? Đây chính là khác biệt của thuật toán quay lui và thuật toán DFS giảng trong [Đông ca dẫn bạn làm cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/): thuật toán quay lui quan tâm không phải Node, mà là cành cây. Nếu không nhớ, mạnh mãnh liệt khuyên đọc lại bài trước.

Với thuật toán quay lui, chúng ta cần làm lựa chọn và hủy lựa chọn trên「cành cây」:

![](https://labuladong.online/algo/images/backtracking/5.jpg)

Khác biệt của chúng có thể phản ánh vào code thế này:

<!-- muliti_language -->
```java
// Thuật toán DFS, điểm quan tâm ở Node
void traverse(TreeNode root) {
    if (root == null) return;
    printf("Vào Node %s", root);
    for (TreeNode child : root.children) {
        traverse(child);
    }
    printf("Rời Node %s", root);
}

// Thuật toán quay lui, điểm quan tâm ở cành cây
void backtrack(TreeNode root) {
    if (root == null) return;
    for (TreeNode child : root.children) {
        // Làm lựa chọn
        printf("Từ %s tới %s", root, child);
        backtrack(child);
        // Hủy lựa chọn
        printf("Từ %s tới %s", child, root);
    }
}
```

Nếu chạy đoạn code này, bạn sẽ phát hiện Node gốc bị bỏ sót :

<!-- muliti_language -->
```java
void traverse(TreeNode root) {
    if (root == null) return;
    for (TreeNode child : root.children) {
        printf("Vào Node %s", child);
        traverse(child);
        printf("Rời Node %s", child);
    }
}
```

Nên với duyệt「đồ thị」ở đây, chúng ta phải dùng thuật toán DFS, tức thao tác `onPath` đặt vào ngoài vòng for, nếu không sẽ bỏ sót ghi duyệt điểm khởi đầu.

Nói nhiều mảng `onPath` vậy, nói tiếp mảng `visited`, mục đích của nó rất rõ, vì đồ thị có thể chứa vòng, mảng `visited` chính là ngăn ngừa đệ quy lặp duyệt cùng một Node vào vào vòng lặp vô hạn.

Dĩ nhiên, nếu đề nói với bạn đồ thị không chứa vòng, có thể lược hết mảng `visited`, cơ bản chính là duyệt cây đa chạc.

### Thực hành bài tập

Dưới đây chúng ta xem bài 797 trên LeetCode「Mọi đường đi có thể」, chữ ký hàm như sau:

```java
List<List<Integer>> allPathsSourceTarget(int[][] graph);
```

Đề nhập một **đồ thị có hướng không chu trình**, đồ thị này chứa `n` Node, đánh số `0, 1, 2,..., n - 1`, hãy tính mọi đường đi từ Node `0` tới Node `n - 1`.

`graph` nhập này thực ra chính là đồ thị biểu diễn「danh sách kề」, `graph[i]` lưu mọi Node lân cận của Node `i`.

Ví dụ nhập `graph = [[1,2],[3],[3],[]]`, thì đại diện đồ thị dưới:

![](https://labuladong.online/algo/images/图/1.jpg)

Thuật toán phải trả về `[[0,1,3],[0,2,3]]`, tức mọi đường đi từ `0` tới `3`.

**lời giải rất đơn giản, lấy `0` làm đỉnh xuất phát duyệt đồ thị, đồng thời ghi đường đi duyệt qua, khi duyệt tới đỉnh đích thì ghi đường đi lại là được**.

Vì đồ thị nhập là không vòng, chúng ta thì không cần mảng `visited` phụ trợ, áp dụng thẳng khung duyệt đồ thị:

<!-- muliti_language -->
```java
class Solution {
    // Ghi mọi đường đi
    List<List<Integer>> res = new LinkedList<>();
        
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        // duy trì đường đi trải qua trong quá trình đệ quy
        LinkedList<Integer> path = new LinkedList<>();
        traverse(graph, 0, path);
        return res;
    }

    /* Khung duyệt đồ thị */
    void traverse(int[][] graph, int s, LinkedList<Integer> path) {
        // Thêm Node s vào đường đi
        path.addLast(s);

        int n = graph.length;
        if (s == n - 1) {
            // Tới đỉnh đích 
            res.add(new LinkedList<>(path));
            // Có thể return thẳng ở đây, nhưng cần removeLast duy trì đúng path
            // path.removeLast();
            // return;
            // Không return cũng được, vì đồ thị không chứa vòng, không xuất hiện đệ quy vô hạn
        }

        // Đệ quy mỗi Node kề
        for (int v : graph[s]) {
            traverse(graph, v, path);
        }
        
        // chuyển Node s khỏi đường đi
        path.removeLast();
    }
}
```

<visual slug='all-paths-from-source-to-target'/>

Bài này giải vậy xong, chú ý đặc tính ngôn ngữ Java, vì tham số hàm Java truyền là tham chiếu đối tượng, nên khi thêm `path` vào `res` cần copy một danh sách mới, nếu không cuối cùng danh sách trong `res` đều rỗng.

Cuối cùng tóm tắt, cách lưu đồ thị chủ yếu có danh sách kề và ma trận kề, dù đồ thị màu mè gì, đều có thể dùng hai cách này lưu.

Trong bài kiểm tra, thuật toán thi nhiều nhất là duyệt đồ thị, rất tương tự với khung duyệt cây đa chạc.

Dĩ nhiên, đồ thị còn nhiều thuật toán thú vị khác, ví dụ [ kiểm tra đồ thị hai phần](https://labuladong.online/algo/data-structure/bipartite-graph/), [phát hiện vòng và sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/) (kiểm tra tham chiếu vòng của trình biên dịch chính là thuật toán tương tự ), [cây khung nhỏ nhất](https://labuladong.online/algo/data-structure/kruskal/), [thuật toán đường ngắn nhất Dijkstra](https://labuladong.online/algo/data-structure/dijkstra/) vân vân, độc giả hứng thú có thể xem, bài này tới đây thôi.




<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Template & ứng dụng thuật toán Dijkstra](https://labuladong.online/algo/data-structure/dijkstra/)
 - [Thuật toán cây khung nhỏ nhất Prim](https://labuladong.online/algo/data-structure/prim/)
 - [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài II](https://labuladong.online/algo/problem-set/binary-tree-post-order-2/)
 - [【Luyện tập tăng cường】Dùng vị trí hậu thứ tự giải bài III](https://labuladong.online/algo/problem-set/binary-tree-post-order-3/)
 - [【Luyện tập tăng cường】Dùng duyệt thứ tự tầng giải bài II](https://labuladong.online/algo/problem-set/binary-tree-level-2/)
 - [Một bài diệt gọn mọi bài đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/)
 - [Đông ca dẫn bạn làm cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
 - [Thuật toán kiểm tra đồ thị hai phần](https://labuladong.online/algo/data-structure/bipartite-graph/)
 - [Cơ bản & kiểu thường gặp của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
 - [Tìm giữa ngàn người: vấn đề người nổi tiếng](https://labuladong.online/algo/frequency-interview/find-celebrity/)
 - [Template thuật toán Trie diệt gọn năm bài](https://labuladong.online/algo/data-structure/trie/)
 - [Cài đặt code mảng động](https://labuladong.online/algo/data-structure-basic/array-implement/)
 - [Khung khuôn mẫu giải bài thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/)
 - [Thuật toán Union-Find](https://labuladong.online/algo/data-structure/union-find/)
 - [Tâm đắc luyện bài của tôi: bản chất thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Phát hiện vòng & thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)
 - [Dùng thuật toán đánh bại thuật toán](https://labuladong.online/algo/other-skills/algorithm-in-pdf/)
 - [Học thuật toán và trải nghiệm flow](https://labuladong.online/algo/other-skills/hert-flow/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN |
| :----: | :----: |
| [1245. Tree Diameter](https://leetcode.com/problems/tree-diameter/?show=1)🔒 | [1245. Đường kính của cây](https://leetcode.cn/problems/tree-diameter/?show=1)🔒 |
| [133. Clone Graph](https://leetcode.com/problems/clone-graph/?show=1) | [133. Clone đồ thị](https://leetcode.cn/problems/clone-graph/?show=1) |
| [1443. Minimum Time to Collect All Apples in a Tree](https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree/?show=1) | [1443. Thời gian ít nhất thu mọi táo trên cây](https://leetcode.cn/problems/minimum-time-to-collect-all-apples-in-a-tree/?show=1) |
| [200. Number of Islands](https://leetcode.com/problems/number-of-islands/?show=1) | [200. Số đảo](https://leetcode.cn/problems/number-of-islands/?show=1) |
| [2049. Count Nodes With the Highest Score](https://leetcode.com/problems/count-nodes-with-the-highest-score/?show=1) | [2049. Đếm số Node điểm cao nhất](https://leetcode.cn/problems/count-nodes-with-the-highest-score/?show=1) |
| [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/?show=1) | [310. Cây chiều cao nhỏ nhất](https://leetcode.cn/problems/minimum-height-trees/?show=1) |
| [542. 01 Matrix](https://leetcode.com/problems/01-matrix/?show=1) | [542. Ma trận 01](https://leetcode.cn/problems/01-matrix/?show=1) |
| [582. Kill Process](https://leetcode.com/problems/kill-process/?show=1)🔒 | [582. Kill tiến trình](https://leetcode.cn/problems/kill-process/?show=1)🔒 |
| [79. Word Search](https://leetcode.com/problems/word-search/?show=1) | [79. Tìm từ](https://leetcode.cn/problems/word-search/?show=1) |
| - | [Kiếm Chỉ Offer II 110. Mọi đường đi](https://leetcode.cn/problems/bP4bmD/?show=1) |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Algorithm Notes của labuladong》 đã xuất bản, follow tài khoản WeChat chính thức xem chi tiết; reply「**全家桶**」 qua tin nhắn có thể tải PDF kèm luyện bài 全家桶**:

![](https://labuladong.online/algo/images/souyisou2.png)
