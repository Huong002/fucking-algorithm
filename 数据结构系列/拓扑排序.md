# Thuật toán phát hiện vòng & sắp xếp topo




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [207. Course Schedule](https://leetcode.com/problems/course-schedule/) | [207. Thời khóa biểu](https://leetcode.cn/problems/course-schedule/) | 🟠 |
| [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | [210. Thời khóa biểu II](https://leetcode.cn/problems/course-schedule-ii/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản cấu trúc đồ thị & cài đặt tổng quát](https://labuladong.online/algo/data-structure-basic/graph-basic/)
> - [Duyệt DFS/BFS cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)

> [!IMPORTANT]
> Trước khi sắp xếp topo, phải đảm bảo đồ thị không có vòng.
>
> Thứ tự「duyệt hậu thứ tự ngược」của đồ thị, chính là kết quả sắp xếp topo.

Cấu trúc dữ liệu đồ thị có vài thuật toán tương đối đặc biệt, ví dụ kiểm tra đồ thị hai phần, kiểm tra đồ thị có vòng/không vòng, sắp xếp topo, và kinh điển nhất cây khung nhỏ nhất, vấn đề đường ngắn nhất đơn nguồn, khó hơn chính là vấn đề kiểu network flow.

Nhưng xét hiện tại, như vấn đề network flow, bạn lại không thi cạnh tranh thi , không có thời gian thì không cần học; như [cây khung nhỏ nhất](https://labuladong.online/algo/data-structure/prim/) và [vấn đề đường ngắn nhất](https://labuladong.online/algo/data-structure/dijkstra/), dù xét từ góc luyện bài gặp không nhiều, nhưng chúng thuộc thuật toán kinh điển, còn sức có thể nắm; như [ kiểm tra đồ thị hai phần](https://labuladong.online/algo/data-structure/bipartite-graph/), sắp xếp topo loại này, bản chất chính là duyệt đồ thị, thuộc thuật toán tương đối cơ bản, phải nắm thành thạo.

**Vậy bài này thì kết hợp bài thuật toán cụ thể, nói hai thuật toán lý thuyết đồ thị: phát hiện vòng của đồ thị có hướng, thuật toán sắp xếp topo**.

Hai thuật toán này vừa có thể dùng ý tưởng DFS giải, vừa có thể dùng ý tưởng BFS giải, tương đối mà nói lời giải BFS xét từ cài đặt code gọn hơn, nhưng lời giải DFS giúp bạn hiểu thêm bí quyết duyệt đệ quy cấu trúc dữ liệu, nên trong bài này tôi giảng ý tưởng duyệt DFS trước, rồi giảng ý tưởng duyệt BFS.

## Thuật toán phát hiện vòng (bản DFS)

Xem trước bài 207 trên LeetCode「Thời khóa biểu」:

<Problem slug="course-schedule" />

```java
// Chữ ký hàm như sau
boolean canFinish(int numCourses, int[][] prerequisites);
```

Đề phải không khó hiểu, khi nào không sửa xong mọi khóa? Khi tồn tại phụ thuộc vòng.

Thực ra tình huống này trong đời thực cũng rất thường gặp, ví dụ chúng ta viết code import package cũng là một ví dụ, bắt buộc thiết kế hợp lý cấu trúc thư mục code, nếu không sẽ xuất hiện phụ thuộc vòng, trình biên dịch sẽ báo lỗi, nên trình biên dịch thực tế cũng dùng thuật toán tương tự để kiểm tra code của bạn có biên dịch thành công không.

**Thấy vấn đề phụ thuộc, đầu tiên nghĩ tới chính là vấn đề chuyển thành cấu trúc dữ liệu「đồ thị có hướng」, chỉ cần đồ thị tồn tại vòng, thì cho thấy tồn tại phụ thuộc vòng**.

Cụ thể, chúng ta trước hết có thể xem khóa thành Node trong「đồ thị có hướng」, số hiệu Node lần lượt `0, 1, ..., numCourses-1`, xem quan hệ phụ thuộc giữa khóa thành cạnh có hướng giữa Node.

Ví dụ bắt buộc học xong khóa `1` mới đi học khóa `3`, vậy thì có một cạnh có hướng từ Node `1` trỏ tới `3`.

Nên chúng ta có thể dựa vào mảng `prerequisites` nhập của đề sinh một đồ thị tương tự thế này:

![](https://labuladong.online/algo/images/topological-sort/1.jpeg)

**Nếu phát hiện đồ thị có hướng này tồn tại vòng, thì cho thấy giữa khóa tồn tại phụ thuộc vòng, chắc chắn không cách nào học hết; ngược lại, nếu không có vòng, vậy chắc chắn học xong toàn bộ khóa**.

Tốt, vậy muốn giải vấn đề này, trước hết chúng ta cần nhập của đề chuyển thành một đồ thị có hướng, rồi kiểm tra đồ thị có tồn tại vòng không.

Chuyển thành đồ thị thế nào? Bài trước [Lưu trữ cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/) viết hai dạng lưu đồ thị, ma trận kề và danh sách kề.

Ở đây tôi dùng dạng danh sách kề lưu đồ thị, trước hết có thể viết một hàm dựng đồ thị:

```java
List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
    // Tổng cộng numCourses Node trong đồ thị
    List<Integer>[] graph = new LinkedList[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new LinkedList<>();
    }
    for (int[] edge : prerequisites) {
        int from = edge[1], to = edge[0];
        // Thêm một cạnh có hướng từ from trỏ tới to
        // Hướng cạnh là quan hệ「 được phụ thuộc」, tức học xong khóa from mới học khóa to
        graph[from].add(to);
    }
    return graph;
}
```

Đồ thị dựng ra rồi, kiểm tra đồ thị có vòng không thế nào?

Rất đơn giản, chẳng qua muốn thi bạn duyệt mọi đường đi trong đồ thị, nếu tôi có thể duyệt mọi đường đi, vậy đường đi có thành vòng không thì dễ tính ra sao?

[Duyệt DFS/BFS cơ bản của đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/) viết dùng thuật toán DFS duyệt mọi đường đi của đồ thị thế nào, quên thì đi ôn, dưới đây cần dùng .

Tôi ở đây áp dụng thẳng template code DFS duyệt mọi đường đi, dùng một biến `hasCycle` ghi có tồn tại vòng không, **khi lặp duyệt tới Node trong `onPath`, thì cho thấy gặp vòng, đặt `hasCycle = true`**.

Dựa vào ý tưởng này, xem code bản đầu (sẽ timeout):

```java
class Solution {
    // Ghi Node trong stack đệ quy
    boolean[] onPath;
    // Ghi đồ thị có vòng không
    boolean hasCycle = false;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        
        onPath = new boolean[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            // Duyệt mọi Node trong đồ thị
            traverse(graph, i);
        }
        // Chỉ cần không có phụ thuộc vòng là có thể hoàn thành mọi khóa
        return !hasCycle;
    }

    // Hàm duyệt đồ thị, duyệt mọi đường đi
    void traverse(List<Integer>[] graph, int s) {
        if (hasCycle) {
            // Nếu đã tìm được vòng, cũng không cần duyệt nữa
            return;
        }

        if (onPath[s]) {
            // s đã trên đường đệ quy, cho thấy thành vòng
            hasCycle = true;
            return;
        }
        
        // Vị trí code tiền thứ tự 
        onPath[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        // Vị trí code hậu thứ tự 
        onPath[s] = false;
    }

    List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        // Code xem bài trước
    }
}
```

Chú ý đồ thị không phải mọi Node đều kề nhau, nên cần dùng vòng for mọi Node đều làm đỉnh xuất phát gọi một lần thuật toán tìm kiếm DFS.

Thực ra lời giải này đã đúng rồi, vì duyệt mọi đường đi, chắc chắn có thể kiểm tra có thành vòng không. Nhưng lời giải này không qua được mọi test case, sẽ timeout. Vậy nguyên nhân chắc chắn cũng đoán được, **có tính dư thừa ** .

 dư thừa tính ở đâu? Tôi giơ ví dụ bạn thì hiểu.

Giả sử giờ bạn lấy Node `2` làm đỉnh xuất phát duyệt mọi đường đi tới được, cuối cùng phát hiện không có vòng.

Giả sử Node `5` khác có một cạnh trỏ tới `2`, khi bạn lấy `5` làm đỉnh xuất phát duyệt mọi đường đi tới được, chắc chắn còn đi tới `2`, vậy xin hỏi, lúc này bạn có cần tiếp tục duyệt mọi đường đi tới được của `2` nữa không?

Đáp án là không cần, vì lần đầu bạn không tìm được vòng, vậy lần này cũng không thể tìm được vòng. Hiểu dư thừa tính trong này chưa? Nếu bạn thấy có ngược ví dụ, có thể tự vẽ, thực tế là không có ngược ví dụ .

Vậy chữa đúng bệnh là được: nếu chúng ta phát hiện một Node trước đó được duyệt qua, có thể bỏ qua thẳng, không cần lặp duyệt nữa.

Code tối ưu như sau:

```java
class Solution {
    // Ghi Node trong một stack đệ quy
    boolean[] onPath;
    // Ghi Node có được duyệt qua không
    boolean[] visited;
    // Ghi đồ thị có vòng không
    boolean hasCycle = false;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        
        onPath = new boolean[numCourses];
        visited = new boolean[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            // Duyệt mọi Node trong đồ thị
            traverse(graph, i);
        }
        // Chỉ cần không có phụ thuộc vòng là có thể hoàn thành mọi khóa
        return !hasCycle;
    }

    // Hàm duyệt đồ thị, duyệt mọi đường đi
    void traverse(List<Integer>[] graph, int s) {
        if (hasCycle) {
            // Nếu đã tìm được vòng, cũng không cần duyệt nữa
            return;
        }

        if (onPath[s]) {
            // s đã trên đường đệ quy, cho thấy thành vòng
            hasCycle = true;
            return;
        }
        
        if (visited[s]) {
            // không cần lặp duyệt Node đã duyệt qua nữa
            return;
        }

        // Vị trí code tiền thứ tự 
        visited[s] = true;
        onPath[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        // Vị trí code hậu thứ tự 
        onPath[s] = false;
    }


    List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        // Code xem bài trước
    }
}
```

<visual slug='course-schedule'>

Node `visited` true màu xanh, Node `onPath` true màu cam.

Bạn có thể mở panel trực quan, click nhiều lần đoạn code <code type="click">if (onPath[s])</code>, là có thể xem quá trình DFS duyệt đồ thị.

</visual>



Bài này giải xong, cốt lõi chính là kiểm tra một đồ thị có hướng có tồn tại vòng không.

Nhưng nếu người ra đề hỏi tiếp, bắt bạn không chỉ kiểm tra có tồn tại vòng không, còn cần trả về vòng này cụ thể có những Node nào, làm sao?

Bạn có thể nói, index true trong `onPath`, không phải chính là số hiệu Node nhóm thành vòng sao?

Không phải, giả sử duyệt bắt đầu từ Node `0`, Node xanh trong hình dưới là đường đệ quy, chúng trong `onPath` đều true, nhưng hiển nhiên Node thành vòng chỉ là một phần trong đó:

![](https://labuladong.online/algo/images/topological-sort/4.jpeg)

Vấn đề này mọi người có thể nghĩ trước, cách chắc chắn có nhiều, tôi chỉ cho ra một lời giải thường dùng.

> [!NOTE]
> lời giải đơn giản trực tiếp nhất là, trên cơ sở mảng `boolean[] onPath`, chúng ta dùng thêm một stack `Stack<Integer> path`, thứ tự Node trải qua trong quá trình duyệt cũng lưu lại.
>
> Ví dụ theo thứ tự duyệt xanh ở hình trên, phần tử từ đáy tới đỉnh của `path` chính là `[0,4,5,9,8,7,6]`. Lúc này lại gặp Node `5` một lần, vậy là có thể biết phần `[5,9,8,7,6]` này là vòng.

Tiếp theo, chúng ta mà nói một thuật toán đồ thị kinh điển: sắp xếp topo.

## Thuật toán sắp xếp topo (bản DFS)

Xem bài 210 trên LeetCode「Thời khóa biểu II」:

<Problem slug="course-schedule-ii" />

Bài này chính là bản nâng của bài trên, không chỉ bắt bạn kiểm tra có thể hoàn thành mọi khóa không, mà tiến thêm bắt bạn trả về một thứ tự học hợp lý, đảm bảo khi bắt đầu học mỗi khóa, khóa trước đặt đều đã học xong.

Chữ ký hàm như sau:

```java
int[] findOrder(int numCourses, int[][] prerequisites);
```

Ở đây tôi nói trước danh từ sắp xếp topo (Topological Sorting), định nghĩa search trên mạng rất toán, ở đây dứt khoát dùng một hình của Baidu Baike để bạn cảm nhận trực quan:

![](https://labuladong.online/algo/images/topological-sort/top.jpg)

**Nói trực quan chính là, bắt bạn một đồ thị「kéo phẳng」, mà trong đồ thị「kéo phẳng」này, mọi hướng mũi tên đều nhất quán**, ví dụ mọi mũi tên ở hình trên đều hướng phải .

Rất hiển nhiên, nếu một đồ thị có hướng tồn tại vòng, không cách nào sắp xếp topo, vì chắc chắn không làm được mọi hướng mũi tên nhất quán; ngược lại, nếu một đồ thị là「đồ thị có hướng không chu trình」, vậy chắc chắn có thể sắp xếp topo.

Nhưng bài này và sắp xếp topo có quan hệ gì?

**Thực ra cũng không khó xem ra, nếu khóa trừu tượng thành Node, quan hệ phụ thuộc giữa khóa trừu tượng thành cạnh có hướng, vậy kết quả sắp xếp topo của đồ thị này chính là thứ tự học**.

Trước hết, chúng ta kiểm tra khóa phụ thuộc nhập của đề có thành vòng không, thành vòng thì không cách nào sắp xếp topo, nên chúng ta có thể tái sử dụng hàm chính của bài trước:

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    if (!canFinish(numCourses, prerequisites)) {
        // Không thể hoàn thành mọi khóa
        return new int[]{};
    }
    // ...
}
```

Vậy vấn đề mấu chốt tới rồi, sắp xếp topo thế nào? Có phải lại cần khoe kỹ thuật cao lớn trên gì?

**Thực ra đặc biệt đơn giản, kết quả duyệt hậu thứ tự của cấu trúc đồ thị đảo ngược, chính là kết quả sắp xếp topo**.

::: note Có cần đảo ngược không?

Có độc giả nhắc, anh ta thấy trên mạng thuật toán sắp xếp topo chính là kết quả duyệt hậu thứ tự, không cần đảo ngược kết quả duyệt hậu thứ tự, tại sao?

Bạn đúng có thể thấy lời giải như vậy, nguyên nhân là định nghĩa cạnh của anh ta khi dựng đồ thị khác tôi. Đồ thị tôi dựng hướng mũi tên là quan hệ「 được phụ thuộc」, ví dụ Node `1` trỏ tới `2`, ý nghĩa là Node `1` được Node `2` phụ thuộc, tức làm xong `1` mới đi làm `2`, vì vậy hơn phù hợp trực giác của chúng ta.

Nếu bạn ngược lại, cạnh có hướng định nghĩa là quan hệ「phụ thuộc」, vậy toàn bộ cạnh trong đồ thị đảo ngược, là có thể không đảo ngược kết quả duyệt hậu thứ tự . Cụ thể, chính là trong code lời giải của tôi `graph[from].add(to);` đổi thành `graph[to].add(from);` là có thể không đảo ngược .

:::

Xem thẳng code lời giải, trên cơ sở code phát hiện vòng của bài trước thêm logic ghi kết quả duyệt hậu thứ tự :

```java
class Solution {
    // Ghi kết quả duyệt hậu thứ tự 
    List<Integer> postorder = new ArrayList<>();
    // Ghi có tồn tại vòng không
    boolean hasCycle = false;
    boolean[] visited, onPath;

    // Hàm chính
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        visited = new boolean[numCourses];
        onPath = new boolean[numCourses];
        // Duyệt đồ thị
        for (int i = 0; i < numCourses; i++) {
            traverse(graph, i);
        }
        // Đồ thị có vòng không cách nào sắp xếp topo
        if (hasCycle) {
            return new int[]{};
        }
        // Kết quả duyệt hậu thứ tự ngược tức là là kết quả sắp xếp topo
        Collections.reverse(postorder);
        int[] res = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            res[i] = postorder.get(i);
        }
        return res;
    }

    // Hàm duyệt đồ thị
    void traverse(List<Integer>[] graph, int s) {
        if (onPath[s]) {
            // Phát hiện vòng
            hasCycle = true;
        }
        if (visited[s] || hasCycle) {
            return;
        }
        // Vị trí duyệt tiền thứ tự 
        onPath[s] = true;
        visited[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        // Vị trí duyệt hậu thứ tự 
        postorder.add(s);
        onPath[s] = false;
    }

    // Hàm dựng đồ thị
    List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        // Code xem bài trước
    }
}
```

<visual slug='course-schedule-ii'>

Node `visited` true màu xanh, Node `onPath` true màu cam.

Bạn có thể mở panel trực quan, click nhiều lần đoạn code <code type="click">if (onPath[s])</code>, là có thể xem quá trình DFS duyệt đồ thị.

</visual>



Code dù nhìn nhiều, nhưng logic phải rất rõ, chỉ cần đồ thị không vòng, vậy chúng ta thì gọi hàm `traverse` duyệt DFS đồ thị, ghi kết quả duyệt hậu thứ tự, cuối cùng kết quả duyệt hậu thứ tự đảo ngược, làm đáp án cuối.

**Vậy tại sao kết quả đảo ngược duyệt hậu thứ tự chính là sắp xếp topo**?

Tôi ở đây cũng tránh chứng minh toán, dùng một ví dụ trực quan để giải thích, chúng ta thì nói cây nhị phân, đây là khung duyệt cây nhị phân chúng ta nói nhiều lần:

```java
void traverse(TreeNode root) {
    // Vị trí code duyệt tiền thứ tự 
    traverse(root.left)
    // Vị trí code duyệt trung thứ tự 
    traverse(root.right)
    // Vị trí code duyệt hậu thứ tự 
}
```

Duyệt hậu thứ tự của cây nhị phân là khi nào? Duyệt xong cây con trái/phải sau mới chạy code vị trí hậu thứ tự . Nói cách khác, khi Node của cây con trái/phải đều được cho vào danh sách kết quả sau, Node gốc mới được cho vào.

**Đặc điểm này của duyệt hậu thứ tự rất quan trọng, sở dĩ cơ sở của sắp xếp topo là duyệt hậu thứ tự, vì một nhiệm vụ bắt buộc đợi mọi nhiệm vụ nó phụ thuộc đều hoàn thành sau mới bắt đầu thực hiện**.

Bạn cây nhị phân hiểu thành một đồ thị có hướng, hướng cạnh là do Node cha trỏ tới Node con, vậy chính là hình dưới:

![](https://labuladong.online/algo/images/topological-sort/2.jpeg)

Với kết quả duyệt hậu thứ tự chuẩn, Node gốc xuất hiện cuối cùng, chỉ cần kết quả duyệt ngược lại, chính là kết quả sắp xếp topo:

![](https://labuladong.online/algo/images/topological-sort/3.jpeg)

Tôi biết có độc giả sẽ hỏi, kết quả đảo ngược duyệt hậu thứ tự, và kết quả duyệt tiền thứ tự có quan hệ gì?

Với cây nhị phân bạn nhìn dường như có quan hệ, thực tế hai người không có quan hệ gì. Bạn nghìn vạn đừng cho rằng kết quả đảo ngược duyệt hậu thứ tự tương đương kết quả duyệt tiền thứ tự .

 khác biệt mấu chốt của nó hai ở [Tư tưởng cây nhị phân (Phần cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/) đã giảng , code vị trí hậu thứ tự là đợi cây con trái/phải đều duyệt xong mới chạy, chỉ nó mới thể hiện được quan hệ「phụ thuộc」, thứ tự duyệt khác đều không làm được.






## Thuật toán phát hiện vòng (bản BFS)

Vừa giảng dùng thuật toán DFS lợi dụng mảng `onPath` kiểm tra có tồn tại vòng không; cũng giảng dùng thuật toán DFS lợi dụng duyệt hậu thứ tự ngược sắp xếp topo.

Thực ra thuật toán BFS mượn mảng `indegree` ghi「bậc vào」mỗi Node, cũng có thể cài đặt hai thuật toán này. Độc giả không quen thuật toán BFS có thể đọc bài trước [Khung cốt lõi thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/).

Cái gọi là「bậc ra」và「bậc vào」là khái niệm trong「đồ thị có hướng」, rất trực quan: nếu một Node `x` có `a` cạnh trỏ tới Node khác, đồng thời được `b` cạnh chỉ, thì cân bậc ra của Node `x` là `a`, bậc vào là `b`.

Nói trước thuật toán phát hiện vòng, xem thẳng code lời giải BFS:

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // Dựng đồ thị, cạnh có hướng đại diện quan hệ「 được phụ thuộc」
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        // Dựng mảng bậc vào
        int[] indegree = new int[numCourses];
        for (int[] edge : prerequisites) {
            int from = edge[1], to = edge[0];
            // Bậc vào của Node to cộng một
            indegree[to]++;
        }

        // Dựa vào bậc vào khởi tạo Node trong hàng đợi
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                // Node i không có bậc vào, tức không có Node phụ thuộc
                // Có thể làm đỉnh xuất phát sắp xếp topo, thêm vào hàng đợi
                q.offer(i);
            }
        }

        // Ghi số Node duyệt
        int count = 0;
        // Bắt đầu chạy vòng BFS
        while (!q.isEmpty()) {
            // Pop Node cur, và giảm bậc vào của Node nó trỏ tới một
            int cur = q.poll();
            count++;
            for (int next : graph[cur]) {
                indegree[next]--;
                if (indegree[next] == 0) {
                    // Nếu bậc vào thành 0, cho thấy Node next phụ thuộc đều được duyệt
                    q.offer(next);
                }
            }
        }

        // Nếu mọi Node đều được duyệt qua, cho thấy không thành vòng
        return count == numCourses;
    }

    // Hàm dựng đồ thị
    List<Integer>[] buildGraph(int n, int[][] edges) {
        // Xem bài trước
    }
}
```

Tôi tóm tắt ý tưởng đoạn thuật toán BFS này trước:

1, Dựng danh sách kề, như trước, hướng cạnh biểu diễn quan hệ「 được phụ thuộc」.

2, Dựng một mảng `indegree` ghi bậc vào mỗi Node, tức `indegree[i]` ghi bậc vào của Node `i`.

3, Khởi tạo Node trong hàng đợi BFS, cho vào Node bậc vào 0 vào hàng đợi trước.

**4, Bắt đầu chạy vòng BFS, liên tục pop Node trong hàng đợi, giảm bậc vào của Node kề, và thêm vào Node bậc vào thành 0 vào hàng đợi**.

**5, Nếu cuối cùng mọi Node đều được duyệt qua (`count` bằng số Node), thì cho thấy không tồn tại vòng, ngược lại thì cho thấy tồn tại vòng**.

Tôi vẽ hình bạn thì dễ hiểu, ví dụ đồ thị dưới, số trong Node đại diện bậc vào của Node đó:

![](https://labuladong.online/algo/images/topological-sort/5.jpeg)

Sau khi hàng đợi khởi tạo, Node bậc vào 0 được thêm vào hàng đợi trước:

![](https://labuladong.online/algo/images/topological-sort/6.jpeg)

Bắt đầu chạy vòng BFS, pop một Node khỏi hàng đợi, giảm bậc vào của Node kề, đồng thời thêm vào Node bậc vào 0 mới sinh vào hàng đợi:

![](https://labuladong.online/algo/images/topological-sort/7.jpeg)

Tiếp tục pop Node khỏi hàng đợi, và giảm bậc vào của Node kề, lần này không có Node bậc vào 0 mới sinh:

![](https://labuladong.online/algo/images/topological-sort/8.jpeg)

Tiếp tục pop Node khỏi hàng đợi, và giảm bậc vào của Node kề, đồng thời thêm vào Node bậc vào 0 mới sinh vào hàng đợi:

![](https://labuladong.online/algo/images/topological-sort/9.jpeg)

Tiếp tục pop Node, đến khi hàng đợi rỗng:

![](https://labuladong.online/algo/images/topological-sort/10.jpeg)

Lúc này, mọi Node đều được duyệt một lần, cũng thì cho thấy đồ thị không tồn tại vòng.

Ngược lại, nếu theo logic trên chạy thuật toán BFS, tồn tại Node không được duyệt, thì cho thấy thành vòng.

Ví dụ tình huống dưới, ban đầu trong hàng đợi chỉ có một Node bậc vào 0:

![](https://labuladong.online/algo/images/topological-sort/11.jpeg)

Khi pop Node này và giảm bậc vào của Node kề sau hàng đợi rỗng, nhưng không sinh Node bậc vào 0 mới thêm vào hàng đợi, nên thuật toán BFS kết thúc :

![](https://labuladong.online/algo/images/topological-sort/12.jpeg)

Bạn thấy rồi, nếu tồn tại Node không được duyệt, vậy cho thấy đồ thị tồn tại vòng, giờ quay đầu xem code BFS, bạn phải thì rất dễ hiểu logic trong đó.






## Thuật toán sắp xếp topo (bản BFS)

**Nếu bạn hiểu thuật toán phát hiện vòng bản BFS, vậy thì rất dễ ra thuật toán sắp xếp topo bản BFS, vì thứ tự duyệt Node chính là kết quả sắp xếp topo**.

Ví dụ ví dụ đầu vừa giơ, giá trị trong mỗi Node ở hình dưới tức là thứ tự vào hàng đợi :

![](https://labuladong.online/algo/images/topological-sort/13.jpeg)

Hiển nhiên, thứ tự này chính là một kết quả sắp xếp topo khả thi .

Nên, chúng ta sửa chút thuật toán phát hiện vòng bản BFS, ghi thứ tự duyệt Node là ra kết quả sắp xếp topo:

```java
class Solution {

    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Dựng đồ thị, giống thuật toán phát hiện vòng
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        // Tính bậc vào, giống thuật toán phát hiện vòng
        int[] indegree = new int[numCourses];
        for (int[] edge : prerequisites) {
            int from = edge[1], to = edge[0];
            indegree[to]++;
        }

        // Dựa vào bậc vào khởi tạo Node trong hàng đợi, giống thuật toán phát hiện vòng
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }

        // Ghi kết quả sắp xếp topo
        int[] res = new int[numCourses];
        // Ghi thứ tự duyệt Node (index)
        int count = 0;
        // Bắt đầu chạy thuật toán BFS
        while (!q.isEmpty()) {
            int cur = q.poll();
            // Thứ tự pop Node tức là là kết quả sắp xếp topo
            res[count] = cur;
            count++;
            for (int next : graph[cur]) {
                indegree[next]--;
                if (indegree[next] == 0) {
                    q.offer(next);
                }
            }
        }

        if (count != numCourses) {
            // Tồn tại vòng, sắp xếp topo không tồn tại
            return new int[]{};
        }
        
        return res;
    }

    // Hàm dựng đồ thị
    List<Integer>[] buildGraph(int n, int[][] edges) {
        // Xem bài trước
    }
}
```

Theo lý, [duyệt đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/) đều cần mảng `visited` ngăn ngừa đi đường cũ , thuật toán BFS ở đây thực ra là qua mảng `indegree` cài đặt tác dụng của mảng `visited`, chỉ Node bậc vào 0 mới vào hàng đợi , nhờ đó đảm bảo không xuất hiện vòng lặp vô hạn.

Tốt rồi, tới đây thuật toán phát hiện vòng, cài đặt BFS của thuật toán sắp xếp topo cũng giảng xong, tiếp tục để một bài nghĩ:

Với thuật toán phát hiện vòng của BFS, nếu hỏi bạn Node hình thành vòng cụ thể là những ai, bạn phải cài đặt thế nào?






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Thuật toán cây khung nhỏ nhất Kruskal](https://labuladong.online/algo/data-structure/kruskal/)
 - [【Luyện tập tăng cường】Bài tập kinh điển BFS I](https://labuladong.online/algo/problem-set/bfs/)
 - [Cơ bản cấu trúc đồ thị & cài đặt code tổng quát](https://labuladong.online/algo/data-structure-basic/graph-basic/)
 - [Duyệt DFS/BFS cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)
 - [Tư duy khung học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Dùng thuật toán đánh bại thuật toán](https://labuladong.online/algo/fname.html?fname=PDF中的算法)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện bài Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài dưới đây có thể xem trực tiếp ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/?show=1) | [310. Cây chiều cao nhỏ nhất](https://leetcode.cn/problems/minimum-height-trees/?show=1) | 🟠 |
| - | [Kiếm Chỉ Offer II 113. Thứ tự khóa học](https://leetcode.cn/problems/QA2IGt/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
