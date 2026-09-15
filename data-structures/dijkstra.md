# Template thuật toán Dijkstra & ứng dụng




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản cấu trúc đồ thị & cài đặt tổng quát](https://labuladong.online/algo/data-structure-basic/graph-basic/)
> - [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
> - [Duyệt DFS/BFS cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)

> [!IMPORTANT]
> Thuật toán Dijkstra là thuật toán tính đường ngắn nhất đơn nguồn trong đồ thị, bản chất là một thuật toán BFS cải tiến đặc biệt, có hai điểm cải tiến :
>
> 1、Dùng [hàng đợi ưu tiên](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/), chứ không phải hàng đợi thường để chạy thuật toán BFS.
>
> 2, Thêm một memo, ghi tổng trọng số đường ngắn nhất từ đỉnh xuất phát tới mỗi Node tới được.

Trước khi học thuật toán đường ngắn nhất Dijkstra, bạn cần hiểu [Cơ bản cấu trúc đồ thị & cài đặt code tổng quát](https://labuladong.online/algo/data-structure-basic/graph-basic/) trước, trong giải thích dưới đây, tôi sẽ dùng API tổng quát của cấu trúc đồ thị `Graph`.

Ngoài ra, bạn bắt buộc phải hiểu [Duyệt DFS/BFS cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/) và [Duyệt DFS/BFS cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/) về nguyên lý cơ bản duyệt BFS, vì thuật toán Dijkstra bản chất chính là một thuật toán BFS cải tiến đặc biệt.

Khi giải thích thuật toán duyệt BFS của cây nhị phân và cấu trúc đồ thị, tôi đồng thời cho ra ba cách viết thuật toán BFS, nếu quên có thể về ôn lại.

Trong đó cách viết BFS thứ ba tương đối phức tạp, nhưng linh hoạt nhất, vì nó tạo mới một lớp `State`, cho phép mỗi Node duy trì độc lập ít thông tin thêm.

Code cụ thể như sau:

```java
// Duyệt thứ tự tầng cây đa chạc
// Mỗi Node tự duy trì lớp State, ghi độ sâu v.v. thông tin
class State {
    Node node;
    int depth;

    public State(Node node, int depth) {
        this.node = node;
        this.depth = depth;
    }
}

void levelOrderTraverse(Node root) {
    if (root == null) {
        return;
    }
    Queue<State> q = new LinkedList<>();
    // Ghi tầng hiện duyệt tới (Node gốc xem là tầng 1)
    q.offer(new State(root, 1));

    while (!q.isEmpty()) {
        State state = q.poll();
        Node cur = state.node;
        int depth = state.depth;
        // Truy cập Node cur, đồng thời biết tầng nó ở
        System.out.println("depth = " + depth + ", val = " + cur.val);

        for (Node child : cur.children) {
            q.offer(new State(child, depth + 1));
        }
    }
}


// Duyệt BFS cấu trúc đồ thị, bắt đầu từ Node s, và ghi tổng trọng số đường đi
// Mỗi Node tự duy trì lớp State, ghi tổng trọng số đi từ s tới
class State {
    // ID Node hiện tại
    int node;
    // Tổng trọng số từ đỉnh xuất phát s tới Node hiện tại
    int weight;

    public State(int node, int weight) {
        this.node = node;
        this.weight = weight;
    }
}


void bfs(Graph graph, int s) {
    boolean[] visited = new boolean[graph.size()];
    Queue<State> q = new LinkedList<>();

    q.offer(new State(s, 0));
    visited[s] = true;

    while (!q.isEmpty()) {
        State state = q.poll();
        int cur = state.node;
        int weight = state.weight;
        System.out.println("visit " + cur + " with path weight " + weight);
        for (Edge e : graph.neighbors(cur)) {
            if (!visited[e.to]) {
                q.offer(new State(e.to, weight + e.weight));
                visited[e.to] = true;
            }
        }
    }
}
```

Cách viết này với cấu trúc cây có hơi thừa, nhưng với đồ thị có trọng số, thì rất hữu dụng.

<visual slug="graph-node-bfs-traverse3" >

Trong panel trực quan này, tôi tạo một đồ thị có trọng số. Bạn có thể click nhiều lần dòng code <code type="click">console.log</code>, chú ý output dòng lệnh, cách viết này có thể đồng thời biết tổng đường đi từ đỉnh xuất phát tới Node hiện tại khi duyệt Node:

</visual>

Thuật toán Dijkstra chúng ta sắp cài đặt chính là cải tiến dựa trên thuật toán này, mỗi Node đều cần ghi tổng trọng số đường ngắn nhất từ đỉnh xuất phát tới mình, kết hợp cấu trúc dữ liệu sắp xếp động [hàng đợi ưu tiên](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/), là có thể tính hiệu quả đường ngắn nhất.

Dưới đây giới thiệu cụ thể cài đặt code tổng quát thuật toán Dijkstra.






## Chữ ký hàm Dijkstra

Trước hết, chúng ta có thể viết một chữ ký hàm tổng quát của thuật toán Dijkstra:

```java
// Nhập một đồ thị và một đỉnh xuất phát start, tính khoảng cách ngắn nhất từ start tới Node khác
int[] dijkstra(int start, Graph graph);
```

Nhập là một đồ thị `graph` và một đỉnh xuất phát `start`, trả về là một mảng ghi trọng số đường ngắn nhất, ví dụ dưới đây:

```java
int[] distTo = dijkstra(3, graph);
```


Mảng `distTo` lưu tổng đường ngắn nhất lấy Node `3` làm đỉnh xuất phát tới Node khác, ví dụ tổng trọng số đường ngắn nhất từ đỉnh xuất phát `3` tới Node `6` chính là `distTo[6]`.

Vì bản chất chính là BFS, nên thuật toán Dijkstra chuẩn sẽ duyệt bắt đầu từ đỉnh xuất phát `start`, tính hết đường ngắn nhất tới mọi Node khác tới được.

Dĩ nhiên, nếu nhu cầu của bạn chỉ tính đường ngắn nhất từ đỉnh xuất phát `start` tới một đỉnh đích `end` nào đó, vậy sửa chút trên thuật toán Dijkstra chuẩn là có thể hoàn thành nhu cầu này hiệu quả hơn, cái này chúng ta nói sau.

## Lớp `State`

Chúng ta cũng cần một lớp `State` phụ trợ chạy thuật toán BFS, rõ ràng, chúng ta dùng biến `id` ghi ID Node hiện tại, dùng biến `distFromStart` ghi khoảng cách từ đỉnh xuất phát tới Node hiện tại.

```java
class State {
    // id của Node đồ thị
    int id;
    // Khoảng cách từ Node start tới Node hiện tại
    int distFromStart;

    State(int id, int distFromStart) {
        this.id = id;
        this.distFromStart = distFromStart;
    }
}
```

## `distTo` ghi đường ngắn nhất

Thuật toán Dijkstra trong đồ thị có trọng số khác thuật toán BFS thường trong đồ thị không trọng số, trong thuật toán Dijkstra, trọng số đường đi lần đầu bạn đi qua một Node, không hẳn chính là nhỏ nhất, nên với cùng một Node, chúng ta có thể đi qua nhiều lần, mà mỗi lần `distFromStart` có thể đều khác, ví dụ hình dưới:

![](https://labuladong.online/algo/images/dijkstra/3.jpeg)

Tôi sẽ đi qua Node `5` ba lần, mỗi lần giá trị `distFromStart` đều khác, vậy tôi lấy lần nhỏ nhất của `distFromStart`, không phải chính là trọng số đường ngắn nhất từ đỉnh xuất phát `start` tới Node `5` sao?

Nên chúng ta cần một mảng `distTo` để ghi tổng trọng số đường ngắn nhất từ đỉnh xuất phát `start` tới mỗi Node, đóng vai trò memo.

Khi lặp lại duyệt tới cùng một Node, chúng ta có thể so sánh `distFromStart` hiện tại và giá trị trong `distTo`, nếu hiện tại nhỏ hơn, thì cập nhật `distTo`, ngược lại, thì không cần tiếp tục duyệt về sau nữa.

## Cài đặt code

Logic mã giả Dijkstra như sau:

```java
// Nhập một đồ thị và một đỉnh xuất phát start, tính khoảng cách ngắn nhất từ start tới Node khác
int[] dijkstra(int start, Graph graph) {
    // Số Node trong đồ thị
    int V = graph.size();
    // Ghi trọng số đường ngắn nhất, bạn có thể hiểu là dp table
    // Định nghĩa: giá trị distTo[i] chính là trọng số đường ngắn nhất từ Node start tới Node i
    int[] distTo = new int[V];
    // tìm giá trị nhỏ nhất, nên dp table khởi tạo là vô cùng dương
    Arrays.fill(distTo, Integer.MAX_VALUE);
    // base case, khoảng cách ngắn nhất từ start tới start chính là 0
    distTo[start] = 0;

    // Hàng đợi ưu tiên, distFromStart nhỏ xếp trước
    Queue<State> pq = new PriorityQueue<>((a, b) -> {
        return a.distFromStart - b.distFromStart;
    });

    // Bắt đầu BFS từ đỉnh xuất phát start
    pq.offer(new State(start, 0));

    while (!pq.isEmpty()) {
        State curState = pq.poll();
        int curNodeID = curState.id;
        int curDistFromStart = curState.distFromStart;

        if (curDistFromStart > distTo[curNodeID]) {
            // Đã có một đường ngắn hơn tới Node curNode
            continue;
        }
        // Cho Node kề của curNode vào hàng đợi
        for (int nextNodeID : graph.neighbors(curNodeID)) {
            // Xem khoảng cách từ curNode tới nextNode có ngắn hơn không
            int distToNextNode = distTo[curNodeID] + graph.weight(curNodeID, nextNodeID);
            if (distTo[nextNodeID] > distToNextNode) {
                // Cập nhật dp table
                distTo[nextNodeID] = distToNextNode;
                // Cho Node này và khoảng cách vào hàng đợi
                pq.offer(new State(nextNodeID, distToNextNode));
            }
        }
    }
    return distTo;
}
```

So với thuật toán BFS thường, bạn có thể có thắc mắc sau:

**1, Không có tập hợp `visited` ghi Node đã thăm, nên một Node sẽ được thăm nhiều lần, được thêm vào hàng đợi nhiều lần, vậy có khiến hàng đợi mãi không rỗng, gây vòng lặp vô hạn không**?

**2, Tại sao dùng hàng đợi ưu tiên `PriorityQueue` chứ không phải hàng đợi thường cài đặt bằng `LinkedList`? Tại sao cần theo giá trị `distFromStart` để sắp xếp**?

**3, Nếu tôi chỉ muốn tính đường ngắn nhất từ đỉnh xuất phát `start` tới một đỉnh đích `end` nào đó, có thể sửa thuật toán, nâng ít hiệu suất không**?

Chúng ta trả lời câu đầu trước, tại sao thuật toán này không dùng tập hợp `visited` cũng không vòng lặp vô hạn.

Với loại vấn đề này, tôi dạy bạn một cách nghĩ:

Điều kiện kết thúc vòng lặp là hàng đợi rỗng, vậy bạn thì cần chú ý xem khi nào đặt phần tử vào hàng đợi (gọi phương thức `offer`), lại chú ý xem khi nào lấy phần tử ra khỏi hàng đợi (gọi phương thức `poll`).

Vòng `while` mỗi lần chạy, đều lấy ra một phần tử, nhưng muốn đặt phần tử vào hàng đợi, nhưng có rất nhiều giới hạn, bắt buộc thỏa mãn điều kiện sau:

```java
// Xem khoảng cách từ curNode tới nextNode có ngắn hơn không
if (distTo[nextNodeID] > distToNextNode) {
    // Cập nhật dp table
    distTo[nextNodeID] = distToNextNode;
    pq.offer(new State(nextNodeID, distToNextNode));
}
```

Đây cũng là lý do tôi nói mảng `distTo` có thể hiểu thành dp table quen thuộc, vì logic thuật toán này chính là liên tục nhỏ nhất hóa phần tử trong mảng `distTo`:

Nếu bạn có thể làm khoảng cách tới `nextNodeID` ngắn hơn, thì cập nhật giá trị `distTo[nextNodeID]`, cho bạn vào hàng đợi, nếu không thì xin lỗi, không cho vào hàng đợi .

**Vì khoảng cách ngắn nhất (trọng số đường) giữa hai Node chắc chắn là một giá trị xác định, không thể giảm vô hạn xuống, nên hàng đợi chắc chắn sẽ rỗng, sau khi hàng đợi rỗng, trong mảng `distTo` ghi chính là「khoảng cách ngắn nhất」từ `start` tới Node khác**.

Tiếp theo giải câu hai, tại sao cần dùng `PriorityQueue` chứ không phải hàng đợi thường cài đặt bằng `LinkedList`?

Nếu bạn nhất định dùng hàng đợi thường, thực ra cũng không vấn đề, bạn có thể đổi thẳng `PriorityQueue` thành `LinkedList`, cũng ra đáp án đúng, nhưng hiệu suất sẽ thấp hơn nhiều.

**Thuật toán Dijkstra dùng hàng đợi ưu tiên, chủ yếu để tối ưu hiệu suất, tương tự một ý tưởng thuật toán tham lam**.

Tại sao nói là ý tưởng tham lam? Ví dụ tình huống sau, bạn muốn tính tổng trọng số đường ngắn nhất từ đỉnh xuất phát `start` tới đỉnh đích `end`:

![](https://labuladong.online/algo/images/dijkstra/4.jpeg)


Giả sử hiện bạn chỉ duyệt mấy Node này trong đồ thị, vậy bước tiếp bạn chuẩn bị duyệt Node nào? Ba đường này đều có thể thành một phần của đường ngắn nhất, **nhưng bạn thấy đường nào có「tiềm năng」hơn thành một phần trong đường ngắn nhất**?

Xét từ tình huống hiện tại, hiển nhiên đường màu cam khả năng lớn hơn, nên chúng ta hy vọng Node `2` xếp dựa vào trước trong hàng đợi, được lấy ra ưu tiên duyệt về sau .

Nên chúng ta dùng `PriorityQueue` làm hàng đợi, để Node có giá trị `distFromStart` nhỏ xếp trước, đây thì tương tự ý tưởng tham lam giảng trước đây [thuật toán tham lam](https://labuladong.online/algo/essential-technique/greedy/) nói tới, có thể tối ưu hiệu suất thuật toán ở mức lớn.

Mọi người phải nghe thuật toán Bellman-Ford, thuật toán này là thuật toán đường ngắn nhất tổng quát hơn, vì nó có thể xử lý đồ thị mang cạnh trọng số âm, logic thuật toán Bellman-Ford rất giống thuật toán Dijkstra, dùng chính là hàng đợi thường, bài này xách một câu, sau có thời gian viết cụ thể.

Tiếp theo nói câu ba, nếu chỉ quan tâm đường ngắn nhất từ đỉnh xuất phát `start` tới một đỉnh đích `end` nào đó, có thể sửa code nâng hiệu suất thuật toán không.

 chắc chắn có thể, vì thuật toán Dijkstra chuẩn sẽ tính đường ngắn nhất từ `start` tới mọi Node khác, bạn chỉ muốn tính tới `end`, tương đương giảm lượng tính, dĩ nhiên có thể nâng hiệu suất.

Sửa cần làm trong code cũng rất ít, chỉ cần sửa chữ ký hàm, thêm kiểm tra if là được:

```java
// Nhập đỉnh xuất phát start và đỉnh đích end, tính khoảng cách ngắn nhất từ đỉnh xuất phát tới đỉnh đích 
int dijkstra(int start, int end, List<Integer>[] graph) {

    // ...

    while (!pq.isEmpty()) {
        State curState = pq.poll();
        int curNodeID = curState.id;
        int curDistFromStart = curState.distFromStart;

        // Thêm kiểm tra ở đây là được, code khác không cần sửa 
        if (curNodeID == end) {
            return curDistFromStart;
        }

        if (curDistFromStart > distTo[curNodeID]) {
            continue;
        }

        // ...
    }

    // Nếu chạy tới đây, cho thấy từ start không đi được tới end
    return Integer.MAX_VALUE;
}
```

Vì tính chất tự sắp xếp của hàng đợi ưu tiên, **mỗi lần** lấy ra từ hàng đợi đều là nhỏ nhất của giá trị `distFromStart`, nên khi bạn **lần đầu** lấy đỉnh đích `end` ra khỏi hàng đợi, giá trị `distFromStart` lúc này tương ứng chính là khoảng cách ngắn nhất từ `start` tới `end`.

Thuật toán này return sớm hơn cài đặt trước, nên hiệu suất nâng nhất định.

Đây là panel trực quan của thuật toán Dijkstra, bạn có thể click code trong đó, xem quá trình chạy thuật toán:


<hr/>
<a href="https://labuladong.online/algo-visualize/tutorial/dijkstra-example/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 Animation trực quan hóa code🌟</strong>
</summary>
</details>
</a>
<hr/>

## Phân tích độ phức tạp thời gian

Độ phức tạp thời gian của thuật toán Dijkstra là bao nhiêu? Bạn lên mạng tra, có thể nói với bạn là $O(ElogV)$, trong đó `E` đại diện số cạnh trong đồ thị, `V` đại diện số Node trong đồ thị.

Vì lý tưởng hàng đợi ưu tiên nhiều nhất chứa `V` Node, số lần thao tác hàng đợi ưu tiên tỉ lệ thuận với `E`, nên độ phức tạp thời gian tổng thể chính là $O(ElogV)$.

Nhưng đây là lý tưởng, cài đặt code thuật toán Dijkstra có rất nhiều phiên bản, ngôn ngữ lập trình khác nhau hoặc API cấu trúc dữ liệu khác nhau đều khiến độ phức tạp thời gian của thuật toán thay đổi ít.

Ví dụ thuật toán Dijkstra cài đặt ở bài này, dùng cấu trúc dữ liệu `PriorityQueue` của Java, lớp container này tầng dưới dùng binary heap cài đặt, nhưng không cung cấp API thao tác phần tử trong hàng đợi qua index, nên hàng đợi sẽ có Node trùng, nhiều nhất có thể `E` Node tồn tại trong hàng đợi.

Nên độ phức tạp thuật toán Dijkstra cài đặt ở bài này không phải $O(ElogV)$ lý tưởng, mà là $O(ElogE)$, có thể hơi lớn hơn, vì số cạnh trong đồ thị thường lớn hơn số Node.

Nhưng với hàm log mà nói, dù cơ số lớn hơn, kết quả hàm log cũng không lớn hơn bao nhiêu, nên hiệu suất chạy thực tế của cài đặt thuật toán này cũng rất cao, trên chỉ là phân tích độ phức tạp thời gian khía cạnh lý thuyết, cung mọi người tham khảo.

Ở mục tiếp [Bài tập thuật toán Dijkstra](https://labuladong.online/algo/problem-set/dijkstra/) , chúng ta sẽ dùng thuật toán Dijkstra giải vài bài thuật toán cụ thể.






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Thuật toán cây khung nhỏ nhất Kruskal](https://labuladong.online/algo/data-structure/kruskal/)
 - [Thuật toán cây khung nhỏ nhất Prim](https://labuladong.online/algo/data-structure/prim/)
 - [【Luyện tập tăng cường】Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
 - [【Luyện tập tăng cường】Bài tập kinh điển thuật toán Dijkstra](https://labuladong.online/algo/problem-set/dijkstra/)
 - [Thuật toán kiểm tra đồ thị hai phần](https://labuladong.online/algo/data-structure/bipartite-graph/)
 - [Duyệt đệ quy/duyệt thứ tự tầng cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
 - [Cương lĩnh cốt lõi thuật toán series cây nhị phân](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
 - [Cơ bản cấu trúc đồ thị & cài đặt code tổng quát](https://labuladong.online/algo/data-structure-basic/graph-basic/)
 - [Duyệt DFS/BFS cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)
 - [Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Đại pháp tiết kiệm tiền du lịch: đường ngắn nhất có trọng số](https://labuladong.online/algo/dynamic-programming/cheap-travel/)
 - [Phát hiện vòng & thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/?show=1) | [1514. Đường có xác suất lớn nhất](https://leetcode.cn/problems/path-with-maximum-probability/?show=1) | 🟠 |
| [1631. Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/?show=1) | [1631. Đường tốn ít thể lực nhất](https://leetcode.cn/problems/path-with-minimum-effort/?show=1) | 🟠 |
| [286. Walls and Gates](https://leetcode.com/problems/walls-and-gates/?show=1)🔒 | [286. Tường và cổng](https://leetcode.cn/problems/walls-and-gates/?show=1)🔒 | 🟠 |
| [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/?show=1) | [310. Cây chiều cao nhỏ nhất](https://leetcode.cn/problems/minimum-height-trees/?show=1) | 🟠 |
| [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/?show=1) | [329. Đường tăng dài nhất trong ma trận](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/?show=1) | 🔴 |
| [505. The Maze II](https://leetcode.com/problems/the-maze-ii/?show=1)🔒 | [505. Mê cung II](https://leetcode.cn/problems/the-maze-ii/?show=1)🔒 | 🟠 |
| [542. 01 Matrix](https://leetcode.com/problems/01-matrix/?show=1) | [542. Ma trận 01](https://leetcode.cn/problems/01-matrix/?show=1) | 🟠 |
| [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/?show=1) | [743. Thời gian trễ mạng](https://leetcode.cn/problems/network-delay-time/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
