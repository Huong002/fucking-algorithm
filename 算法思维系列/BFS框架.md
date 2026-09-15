# Khung套路 giải đề bằng thuật toán BFS

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình速 thành](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học được套路 thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [752. Open the Lock](https://leetcode.com/problems/open-the-lock/) | [752. Mở khóa bàn xoay](https://leetcode.cn/problems/open-the-lock/) | 🟠 |
| [773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/) | [773. Câu đố trượt](https://leetcode.cn/problems/sliding-puzzle/) | 🔴 |

**-----------**

> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Duyệt đệ quy/duyệt tầng của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
> - [Duyệt đệ quy/duyệt tầng của cây đa phân](https://labuladong.online/algo/data-structure-basic/n-ary-tree-traverse-basic/)
> - [Duyệt DFS/BFS của cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)

Tôi nhiều lần nhấn mạnh, thuật toán DFS/quay lui/BFS loại này, bản chất chính là把 vấn đề cụ thể trừu tượng thành cấu trúc cây, rồi duyệt cây này tiến hành bạo lực穷举, nên code của những thuật toán穷举 này bản chất chính là code duyệt cây.

Chải lại quan hệ nhân quả trong đó:

Bản chất của thuật toán DFS/quay lui chính là đệ quy duyệt một cây穷举 (cây đa phân), mà duyệt đệ quy cây đa phân lại衍生 từ duyệt đệ quy cây nhị phân. Nên tôi nói bản chất của thuật toán DFS/quay lui là duyệt đệ quy cây nhị phân.

Bản chất của thuật toán BFS chính là duyệt một đồ thị, dưới đây bạn sẽ thấy, khung thuật toán BFS chính là code thuật toán duyệt nút đồ thị trong [Duyệt DFS/BFS của cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/).

Mà thuật toán duyệt đồ thị thực ra chính là thuật toán duyệt cây đa phân thêm một mảng `visited`防 vòng lặp chết; thuật toán duyệt cây đa phân lại衍生 từ thuật toán duyệt cây nhị phân. Nên tôi nói bản chất của thuật toán BFS chính là duyệt tầng của cây nhị phân.

Tại sao thuật toán BFS thường dùng để求解 vấn đề đường đi ngắn nhất? Tôi trong [Duyệt đệ quy/duyệt tầng của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/) từng dùng ví dụ độ sâu nhỏ nhất của cây nhị phân说明 chi tiết.

Thực ra cái gọi là đường đi ngắn nhất, đều có thể类比 thành loại vấn đề độ sâu nhỏ nhất của cây nhị phân (tìm nút lá gần nút gốc nhất), duyệt đệ quy必须要 duyệt mọi nút của cả cây mới能 tìm được nút mục tiêu, mà duyệt tầng không cần duyệt mọi nút là搞定 được, nên duyệt tầng phù hợp giải loại vấn đề đường đi ngắn nhất này.

Chải như vậy hẳn够 rõ了吧?

Nên trước khi đọc bài này, cần đảm bảo bạn đã học [Duyệt đệ quy/duyệt tầng của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)、[Duyệt đệ quy/duyệt tầng của cây đa phân](https://labuladong.online/algo/data-structure-basic/n-ary-tree-traverse-basic/) và [Duyệt DFS/BFS của cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/) phía trước,先把 thuật toán duyệt của mấy cấu trúc dữ liệu cơ bản này chơi明白, thuật toán khác都会 rất dễ hiểu.

**Trọng điểm của bài này nằm ở, dạy bạn với vấn đề thuật toán cụ thể tiến hành trừu tượng và chuyển hóa thế nào, rồi套 khung thuật toán BFS tiến hành求解**.

Trong đề thi viết phỏng vấn thực tế, thường không phải trực tiếp bắt bạn duyệt cấu trúc dữ liệu chuẩn như cây/đồ thị, mà cho bạn một đề cảnh cụ thể, bạn cần把 cảnh cụ thể trừu tượng thành một cấu trúc đồ thị/cây chuẩn, rồi lợi dụng thuật toán BFS穷举得出 đáp án.

Ví như cho bạn một game mê cung, hãy tính số bước ít nhất đi đến lối ra? Nếu mê cung này còn chứa cổng dịch chuyển, có thể瞬間 dịch chuyển sang vị trí khác, vậy số bước ít nhất lại là bao nhiêu?

Lại ví như hai từ, yêu cầu bạn thông qua thay thế nào đó,把 một cái biến thành cái còn lại, mỗi lần có thể thay/xóa/chèn một ký tự, ít nhất要 thao tác mấy lần?

Lại ví như game nối hình, điều kiện hai ô消除 không chỉ图案 giống nhau, còn得 đảm bảo连线 ngắn nhất giữa hai ô không được nhiều hơn hai góc ngoặt. Bạn chơi nối hình, bấm hai tọa độ, game判断连线 ngắn nhất của chúng có mấy góc ngoặt thế nào?

Bạn xem mấy ví dụ trên, có phải cảm giác và cấu trúc cây/đồ thị chúng ta học trước đó hoàn toàn扯不上 quan hệ? Nhưng thực tế chỉ cần稍加 trừu tượng, chúng chính là duyệt cấu trúc cây/đồ thị, thực sự quá đơn giản枯燥.

Dưới đây dùng几道 ví dụ để讲解套路 khung BFS, sau này再也 đừng thấy loại vấn đề này khó giải.

## Một, khung thuật toán

Khung thuật toán BFS thực ra chính là code BFS duyệt cấu trúc đồ thị cho trong [Duyệt DFS/BFS của cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/), tổng cộng có ba cách viết.

Với vấn đề thuật toán BFS thực tế, cách viết thứ nhất đơn giản nhất, nhưng局限性太大, không常用; cách viết thứ hai常用 nhất, đề thuật toán BFS độ khó trung bình cơ bản đều có thể dùng cách viết này giải; cách viết thứ ba hơi phức tạp, nhưng linh hoạt nhất, có thể會 trong một số vấn đề BFS难度较大 dùng đến. Trong [Chương bài tập thuật toán BFS](https://labuladong.online/algo/problem-set/bfs/) tiếp theo, sẽ có một số đề难度 lớn hơn dùng cách viết thứ ba, đến lúc đó bạn có thể tự thử.

Ví dụ của bài này đều độ khó trung bình, nên解法 bài này给出 đều以 cách viết thứ hai làm chuẩn:

```java
// Từ s bắt đầu BFS duyệt mọi nút của đồ thị, mà ghi lại số bước duyệt
// Khi đi đến nút mục tiêu target时, trả về số bước
int bfs(int s, int target) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> q = new LinkedList<>();
    q.offer(s);
    visited[s] = true;
    // Ghi số bước từ s đi đến nút hiện tại
    int step = 0;
    while (!q.isEmpty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            int cur = q.poll();
            System.out.println("visit " + cur + " at step " + step);
            // 判断 có đến终点 không
            if (cur == target) {
                return step;
            }
            // 把 nút 이웃加入 hàng đợi,扩散 tìm kiếm ra xung quanh
            for (int to : neighborsOf(cur)) {
                if (!visited[to]) {
                    q.offer(to);
                    visited[to] = true;
                }
            }
        }
        step++;
    }
    // Nếu đi đến đây,说明 trong đồ thị không tìm được nút mục tiêu
    return -1;
}
```

Code khung trên gần như chính là từ [Duyệt DFS/BFS của cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/) copy qua, chỉ có điều thêm một tham số `target`, khi lần đầu đi đến `target`时, trực tiếp kết thúc thuật toán并 trả về số bước đã đi.

Dưới đây chúng ta dùng vài ví dụ cụ thể xem运用 khung này thế nào.

## Hai, 773. Câu đố trượt

LeetCode 773 「Câu đố trượt」 chính là một đề có thể vận dụng khung BFS giải quyết, yêu cầu của đề như sau:

Cho bạn một ghép hình trượt 2x3, dùng một mảng 2x3 `board` biểu thị. Trong ghép hình có sáu số 0~5, trong đó **số 0就 biểu thị ô trống đó**, bạn có thể di chuyển số trong đó, khi `board` biến thành `[[1,2,3],[4,5,0]]`时, thắng game.

Hãy viết một thuật toán, tính số lần di chuyển ít nhất cần để thắng game, nếu không thể thắng game, trả về -1.

Ví như mảng hai chiều nhập `board = [[4,1,2],[5,0,3]]`, thuật toán hẳn trả về 5:

![](https://labuladong.online/algo/images/sliding_puzzle/5.jpeg)

Nếu nhập là `board = [[1,2,3],[5,4,0]]`,则 thuật toán trả về -1, vì trong cục diện này dù thế nào cũng không thể thắng game.

Tôi cảm thấy bài này挺 thú vị, hồi nhỏ từng chơi game ghép hình tương tự, ví như Hoa Dung Đạo:

![](https://labuladong.online/algo/images/sliding_puzzle/2.jpeg)

Bạn cần di chuyển những ô này, nghĩ cách让 Tào Tháo từ vị trí ban đầu di chuyển đến vị trí lối ra dưới cùng nhất.

Hoa Dung Đạo hẳn比 bài này khó hơn, vì trong bài này của Lực khấu kích thước mỗi ô có thể coi là giống nhau, mà trong Hoa Dung Đạo kích thước mỗi ô còn không giống nhau.

Trở lại bài này, chúng ta把 bài này trừu tượng thành cấu trúc cây/đồ thị thế nào, từ đó dùng khung thuật toán BFS giải?

Thực ra trạng thái ban đầu của bàn cờ là có thể coi là điểm bắt đầu:

```
[[2,4,1],
 [5,0,3]]
```

Trạng thái mục tiêu cuối cùng của chúng ta là把 bàn cờ biến thành như sau:

```
[[1,2,3],
 [4,5,0]]
```

Vậy đây là có thể coi là终点.

Bây giờ vấn đề này chẳng phải trở thành một vấn đề đồ thị sao? Đề hỏi thực ra chính là đường đi ngắn nhất từ điểm bắt đầu đến终点 cần bao nhiêu嘛.

Nút 이웃 của điểm bắt đầu là ai? 把 số 0 và số trên-dưới-trái-phải tiến hành hoán đổi, thực ra chính là bốn nút 이웃 của điểm bắt đầu嘛 (do trong bài này kích thước bàn cờ là 2x3, nên nút 이웃 thực tế trong biên chỉ số sẽ nhỏ hơn bốn):

![](https://labuladong.online/algo/images/sliding_puzzle/3.jpeg)

Cứ thế, bốn nút 이웃 này còn có各自 bốn nút 이웃, vậy đây chẳng phải就是 một cấu trúc đồ thị sao?

Vậy tôi từ điểm bắt đầu dùng thuật toán BFS duyệt đồ thị này, lần đầu đến终点时, số bước đã đi chính là đáp án.

Mã giả như sau:

```java
int bfs(int[][] board, int[][] target) {
    Queue<int[][]> q = new LinkedList<>();
    HashSet visited = new HashSet<>();

    // 把 điểm bắt đầu加入 hàng đợi
    q.offer(board);
    visited.add(board);

    int step = 0;
    while (!q.isEmpty()) {
        int sz = q.size();
        for (int i = 0; i < sz; i++) {
            int[][] cur = q.poll();
            // 判断 có đến终点 không
            if (cur == target) {
                return step;
            }
            // 把 nút 이웃 của nút hiện tại加入 hàng đợi
            for (int[][] neighbor : getNeighbors(cur)) {
                if (!visited.contains(neighbor)) {
                    q.offer(neighbor);
                    visited.add(neighbor);
                }
            }
        }
        step++;
    }
    return -1;
}

List<int[][]> getNeighbors(int[][] board) {
    // 把 số 0 trong board và số trên-dưới-trái-phải tiến hành hoán đổi,得到 4 nút 이웃
}
```

Với bài này, cấu trúc đồ thị chúng ta trừu tượng ra cũng sẽ chứa chu trình, nên cần một mảng `visited` ghi nút đã đi qua, tránh thành vòng dẫn đến lặp vô hạn.

Ví như tôi từ nút `[[2,4,1],[5,0,3]]` bắt đầu, số 0 dời sang phải得到 nút mới `[[2,4,1],[5,3,0]]`, nhưng số 0 trong nút mới này cũng có thể dời sang trái, lại sẽ về `[[2,4,1],[5,0,3]]`, chuyện này thực ra chính là thành vòng. Chúng ta cũng cần một tập băm `visited` để ghi nút đã đi qua,防 thành vòng dẫn đến lặp vô hạn.

Còn một vấn đề, `board` trong bài này là một mảng hai chiều, chúng ta trong [Nguyên lý bảng băm/tập băm](https://labuladong.online/algo/data-structure-basic/hashmap-basic/) từng giới thiệu, mảng hai chiều loại cấu trúc dữ liệu可变 không thể trực tiếp加入 tập băm.

Nên chúng ta còn要用 chút技巧, nghĩ cách把 mảng hai chiều chuyển thành một kiểu bất biến mới能存 vào tập băm. Giải pháp thường gặp là把 mảng hai chiều序列化 thành một chuỗi, như vậy là có thể trực tiếp存 vào tập băm.

**Trong đó比较 có技巧 điểm nằm ở, mảng hai chiều có khái niệm 「trên-dưới-trái-phải」, nén thành chuỗi một chiều后, còn怎么把 số 0 và số trên-dưới-trái-phải tiến hành hoán đổi**?

Với bài này, đề nói kích thước mảng nhập đều là 2 x 3, nên chúng ta có thể trực tiếp viết tay ra ánh xạ này:

```java
// Ghi lại chỉ số kề nhau của chuỗi một chiều
int[][] neighbor = new int[][]{
    {1, 3},
    {0, 4, 2},
    {1, 5},
    {0, 4},
    {3, 1, 5},
    {4, 2}
};
```

**Hàm ý của ánh xạ này chính là, trong chuỗi một chiều, chỉ số kề trong mảng hai chiều của chỉ số `i` là `neighbor[i]`**:

![](https://labuladong.online/algo/images/sliding_puzzle/4.jpeg)

:::: details Nếu là mảng hai chiều `m x n`, làm sao?

Với một mảng hai chiều `m x n`, viết tay ánh xạ chỉ số một chiều của nó肯定 không thực tế, cần dùng code sinh ánh xạ chỉ số một chiều của nó.

Quan sát hình trên là có thể phát hiện, nếu một phần tử `e` nào đó trong mảng hai chiều có chỉ số trong mảng một chiều là `i`, vậy chỉ số trong mảng một chiều của phần tử kề trái-phải của `e` chính là `i - 1` và `i + 1`, mà chỉ số trong mảng một chiều của phần tử kề trên-dưới của `e` chính là `i - n` và `i + n`, trong đó `n` là số cột của mảng hai chiều.

Như vậy, với mảng hai chiều `m x n`, chúng ta có thể viết một hàm để sinh ánh xạ `neighbor` của nó:

```java
int[][] generateNeighborMapping(int m, int n) {
    int[][] neighbor = new int[m * n][];
    for (int i = 0; i < m * n; i++) {
        List<Integer> neighbors = new ArrayList<>();

        // Nếu không phải cột đầu, có 이웃 trái
        if (i % n != 0) neighbors.add(i - 1);

        // Nếu không phải cột cuối, có 이웃 phải
        if (i % n != n - 1) neighbors.add(i + 1);

        // Nếu không phải hàng đầu, có 이웃 trên
        if (i - n >= 0) neighbors.add(i - n);

        // Nếu không phải hàng cuối, có 이웃 dưới
        if (i + n < m * n) neighbors.add(i + n);

        // Đặc tính ngôn ngữ Java, chuyển kiểu List thành mảng int[]
        neighbor[i] = neighbors.stream().mapToInt(Integer::intValue).toArray();
    }
    return neighbor;
}
```

::::

Như vậy, dù số 0 ở đâu, đều có thể thông qua ánh xạ chỉ số này得到 chỉ số kề để hoán đổi. Dưới đây là cài đặt code đầy đủ:

```java
class Solution {
    public int slidingPuzzle(int[][] board) {
        String target = "123450";
        // Chuyển mảng 2x3 thành chuỗi làm điểm bắt đầu BFS
        String start = "";
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                start = start + board[i][j];
            }
        }

        // ****** Khung thuật toán BFS bắt đầu ******
        Queue<String> q = new LinkedList<>();
        HashSet<String> visited = new HashSet<>();
        // Từ điểm bắt đầu BFS tìm kiếm
        q.offer(start);
        visited.add(start);

        int step = 0;
        while (!q.isEmpty()) {
            int sz = q.size();
            for (int i = 0; i < sz; i++) {
                String cur = q.poll();
                // 判断 có đạt cục diện mục tiêu không
                if (target.equals(cur)) {
                    return step;
                }
                // Hoán đổi số 0 và số kề nhau
                for (String neighborBoard : getNeighbors(cur)) {
                    // 防 đi đường quay lại
                    if (!visited.contains(neighborBoard)) {
                        q.offer(neighborBoard);
                        visited.add(neighborBoard);
                    }
                }
            }
            step++;
        }
        // ****** Khung thuật toán BFS kết thúc ******
        return -1;
    }

    private List<String> getNeighbors(String board) {
        // Ghi lại chỉ số kề nhau của chuỗi một chiều
        int[][] mapping = new int[][]{
                {1, 3},
                {0, 4, 2},
                {1, 5},
                {0, 4},
                {3, 1, 5},
                {4, 2}
        };

        int idx = board.indexOf('0');
        List<String> neighbors = new ArrayList<>();
        for (int adj : mapping[idx]) {
            String new_board = swap(board.toCharArray(), adj, idx);
            neighbors.add(new_board);
        }
        return neighbors;
    }

    private String swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
        return new String(chars);
    }
}
```

<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/sliding-puzzle/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Hình động可视 hóa code 🌈</strong>
</summary>
</details>
</a>
<hr/>

Bài này就 giải xong. Bạn sẽ phát hiện bản thân thuật toán BFS cách viết đều là套路 cố định, điểm khó của bài này thực ra nằm ở把 đề chuyển thành mô hình BFS穷举, rồi dùng phương pháp hợp lý把 mảng đa chiều chuyển thành chuỗi, để tập băm ghi nút đã thăm.

Dưới đây xem thêm một đạo đề cảnh thực tế.

## Ba, số lần ít nhất để mở khóa mật mã

Xem LeetCode 752 「Mở khóa bàn xoay」,比较 thú vị:

<Problem slug="open-the-lock" />

Chữ ký hàm như sau:

```java
int openLock(String[] deadends, String target)
```

Trong đề mô tả chính là loại khóa mật mã thường gặp trong đời sống chúng ta, nếu không có bất kỳ ràng buộc nào, số lần拨 ít nhất rất dễ tính. Ví như muốn拨 đến `"1234"`, vậy từng số拨動 là được, số lần拨 ít nhất chính là `1 + 2 + 3 + 4 = 10` lần.

Nhưng điểm khó bây giờ就在于, trong quá trình拨 khóa mật mã không thể xuất hiện `deadends`, như vậy就有 chút độ khó. Nếu gặp `deadends`, bạn该 xử lý thế nào, mới khiến tổng số lần拨 ít nhất?

Ngàn vạn đừng rơi vào chi tiết, thử nghĩ đủ loại tình huống cụ thể.要 biết bản chất của thuật toán chính là穷举, chúng ta trực tiếp từ `"0000"` bắt đầu bạo lực穷举,把 mọi tình huống拨 có thể đều穷举 ra, chẳng lẽ còn sợ không tìm được số lần拨 ít nhất sao?

**Bước một, chúng ta不管 mọi điều kiện giới hạn,不管 giới hạn của `deadends` và `target`,就 suy nghĩ một vấn đề: nếu让 bạn thiết kế một thuật toán,穷举 mọi tổ hợp mật mã có thể, bạn làm sao**?

就 từ `"0000"` bắt đầu, nếu bạn chỉ xoay một cái, có mấy khả năng? Tổng cộng có 4 vị trí, mỗi vị trí có thể xoay lên, cũng có thể xoay xuống, cũng chính là có thể穷举 ra `"1000", "9000", "0100", "0900"...` tổng 8 loại mật mã.

Rồi, lại以 8 loại mật mã này làm cơ sở, trong đó mỗi mật mã lại có thể xoay một cái衍生 ra 8 loại mật mã, cứ thế...

Cây đệ quy trong lòng ra chưa? Hẳn là một cây tám phân, mỗi nút đều có 8 nút con,衍生 xuống dưới.

Đoạn mã giả dưới就 mô tả思路 trên, dùng duyệt tầng một cây tám phân:

```java
// 把 s[j]拨 lên một lần
String plusOne(String s, int j) {
    char[] ch = s.toCharArray();
    if (ch[j] == '9')
        ch[j] = '0';
    else
        ch[j] += 1;
    return new String(ch);
}
// 把 s[i]拨 xuống một lần
String minusOne(String s, int j) {
    char[] ch = s.toCharArray();
    if (ch[j] == '0')
        ch[j] = '9';
    else
        ch[j] -= 1;
    return new String(ch);
}

// Khung BFS, tìm số lần拨 ít nhất
void BFS(String target) {
    Queue<String> q = new LinkedList<>();
    q.offer("0000");

    int step = 0;

    while (!q.isEmpty()) {
        int sz = q.size();
        // 把 mọi nút trong hàng đợi hiện tại扩散 ra xung quanh
        for (int i = 0; i < sz; i++) {
            String cur = q.poll();
            // 判断 có đến终点 không
            if (cur.equals(target)) {
                return step;
            }

            // Một mật mã có thể衍生 ra 8 mật mã kề nhau
            for (String neighbor : getNeighbors(cur)) {
                q.offer(neighbor);
            }
        }
        // Ở đây tăng số bước
        step++;
    }
}
// 把 mỗi位 của s拨 lên một lần hoặc拨 xuống một lần, 8 mật mã kề nhau
List<String> getNeighbors(String s) {
    List<String> neighbors = new ArrayList<>();
    for (int i = 0; i < 4; i++) {
        neighbors.add(plusOne(s, i));
        neighbors.add(minusOne(s, i));
    }
    return neighbors;
}
```

Code này đã có thể穷举 mọi tổ hợp mật mã có thể, nhưng còn có vấn đề cần giải.

1、Sẽ đi đường quay lại, chúng ta có thể từ `"0000"`拨 đến `"1000"`, nhưng等 từ hàng đợi lấy ra `"1000"`时, còn会拨 ra một `"0000"`, như vậy会 sinh vòng lặp chết.

Vấn đề này rất dễ giải, thực ra chính là thành vòng嘛, chúng ta dùng một tập `visited` ghi mật mã đã穷举 qua, lần nữa gặp时, đừng加入 hàng đợi là được.

2、Chưa xử lý `deadends`,按道理 những 「mật mã chết」 này không thể xuất hiện.

Vấn đề này cũng dễ xử lý, dùng thêm một tập `deadends` ghi những mật mã chết này, phàm gặp những mật mã này, đừng加入 hàng đợi là được.

Hoặc còn có thể đơn giản hơn, trực tiếp把 mật mã chết trong `deadends` làm phần tử ban đầu của tập `visited`, như vậy cũng có thể đạt mục đích.

Dưới đây là cài đặt code đầy đủ:

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        // Ghi mật mã chết cần bỏ qua
        Set<String> deads = new HashSet<>();
        for (String s : deadends) deads.add(s);
        if (deads.contains("0000")) return -1;

        // Ghi mật mã đã穷举 qua,防 đi đường quay lại
        Set<String> visited = new HashSet<>();
        Queue<String> q = new LinkedList<>();
        // Từ điểm bắt đầu khởi động tìm kiếm theo chiều rộng
        int step = 0;
        q.offer("0000");
        visited.add("0000");

        while (!q.isEmpty()) {
            int sz = q.size();
            // 把 mọi nút trong hàng đợi hiện tại扩散 ra xung quanh
            for (int i = 0; i < sz; i++) {
                String cur = q.poll();

                // 判断 có đến终点 không
                if (cur.equals(target))
                    return step;

                // 把 nút 이웃 hợp lệ của một nút加入 hàng đợi
                for (String neighbor : getNeighbors(cur)) {
                    if (!visited.contains(neighbor) && !deads.contains(neighbor)) {
                        q.offer(neighbor);
                        visited.add(neighbor);
                    }
                }
            }
            // Ở đây tăng số bước
            step++;
        }
        // Nếu穷举 hết cũng không tìm được mật mã mục tiêu, chính là không tìm được
        return -1;
    }

    // 把 s[j]拨 lên một lần
    String plusOne(String s, int j) {
        char[] ch = s.toCharArray();
        if (ch[j] == '9')
            ch[j] = '0';
        else
            ch[j] += 1;
        return new String(ch);
    }

    // 把 s[i]拨 xuống một lần
    String minusOne(String s, int j) {
        char[] ch = s.toCharArray();
        if (ch[j] == '0')
            ch[j] = '9';
        else
            ch[j] -= 1;
        return new String(ch);
    }

    // 把 mỗi位 của s拨 lên một lần hoặc拨 xuống một lần, 8 mật mã kề nhau
    List<String> getNeighbors(String s) {
        List<String> neighbors = new ArrayList<>();
        for (int i = 0; i < 4; i++) {
            neighbors.add(plusOne(s, i));
            neighbors.add(minusOne(s, i));
        }
        return neighbors;
    }
}
```

## Bốn, tối ưu BFS hai chiều

Dưới đây giới thiệu thêm một思路 tối ưu của thuật toán BFS: **BFS hai chiều**, có thể nâng cao hiệu suất tìm kiếm BFS.

Bạn把技巧 này coi như đọc mở rộng là được, trong đề thi viết phỏng vấn thường, thuật toán BFS thường đã够 dùng, nếu gặp超时 không qua được, hoặc truy vấn của người phỏng vấn, có thể xét解法 có cần tối ưu BFS hai chiều không.

BFS hai chiều chính là衍生 từ thuật toán BFS chuẩn:

**Khung BFS truyền thống là từ điểm bắt đầu扩散 ra xung quanh, khi gặp终点时 dừng; mà BFS hai chiều则是 từ điểm bắt đầu và终点 đồng thời bắt đầu扩散, khi hai bên có giao nhau时 dừng**.

Tại sao như vậy能够 nâng cao hiệu suất?

Giống như có hai người A và B, BFS truyền thống就相当于 A xuất phát đi tìm B, mà B đứng yên tại chỗ không động; BFS hai chiều则是 A và B cùng xuất phát,双向奔赴. Vậy đương nhiên trường hợp thứ hai A và B có thể gặp nhau nhanh hơn.

![](https://labuladong.online/algo/images/bfs/1.jpeg)

![](https://labuladong.online/algo/images/bfs/2.jpeg)

Cấu trúc cây trong hình, nếu终点 ở dưới cùng nhất,按 chiến lược thuật toán BFS truyền thống, sẽ把 nút của cả cây tìm kiếm一遍, cuối cùng tìm được `target`; mà BFS hai chiều thực ra chỉ duyệt nửa cây就 xuất hiện giao nhau, cũng chính là tìm được khoảng cách ngắn nhất.

Đương nhiên từ ký hiệu Big O phân tích độ phức tạp thuật toán, hai loại BFS này trong trường hợp xấu nhất đều có thể duyệt hết mọi nút, nên độ phức tạp thời gian lý thuyết đều là $O(N)$, nhưng chạy thực tế BFS hai chiều确实会 nhanh hơn.

::: info Hạn chế của BFS hai chiều

**Bạn必须 biết终点 ở đâu, mới能 dùng BFS hai chiều tối ưu**.

Với thuật toán BFS, chúng ta肯定 biết điểm bắt đầu, nhưng终点 cụ thể là gì, lúc đầu chúng ta có thể并不知道.

Ví như vấn đề khóa mật mã và ghép hình trượt trên, đề đều cho明确终点, đều có thể dùng BFS hai chiều tối ưu.

Nhưng ví như vấn đề chiều cao nhỏ nhất của cây nhị phân chúng ta thảo luận trong [Duyệt DFS/BFS của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/), điểm bắt đầu là nút gốc,终点 là nút lá gần nút gốc nhất, lúc thuật toán bắt đầu bạn并不知道终点 cụ thể ở đâu, nên就没 cách dùng BFS hai chiều tối ưu.

:::

Dưới đây chúng ta lấy vấn đề khóa mật mã làm ví dụ, xem làm sao把 thuật toán BFS thường tối ưu thành thuật toán BFS hai chiều, xem trực tiếp code吧:

```java
class Solution {
    public int openLock(String[] deadends, String target) {
        Set<String> deads = new HashSet<>();
        for (String s : deadends) deads.add(s);
        // base case
        if (deads.contains("0000")) return -1;
        if (target.equals("0000")) return 0;

        // Dùng tập hợp không dùng hàng đợi, có thể nhanh判斷 phần tử tồn tại không
        Set<String> q1 = new HashSet<>();
        Set<String> q2 = new HashSet<>();
        Set<String> visited = new HashSet<>();

        int step = 0;
        q1.add("0000");
        visited.add("0000");
        q2.add(target);
        visited.add(target);

        while (!q1.isEmpty() && !q2.isEmpty()) {
            // Ở đây tăng số bước
            step++;

            // Tập băm trong quá trình duyệt không thể sửa, nên dùng newQ1 lưu nút 이웃
            Set<String> newQ1 = new HashSet<>();

            // Lấy nút 이웃 của mọi nút trong q1
            for (String cur : q1) {
                // 把 nút 이웃 chưa duyệt của một nút加入 tập hợp
                for (String neighbor : getNeighbors(cur)) {
                    // 判断 có đến终点 không
                    if (q2.contains(neighbor)) {
                        return step;
                    }
                    if (!visited.contains(neighbor) && !deads.contains(neighbor)) {
                        newQ1.add(neighbor);
                        visited.add(neighbor);
                    }
                }
            }
            // newQ1 lưu nút 이웃 của q1
            q1 = newQ1;
            // Vì mỗi lần BFS đều扩散 q1, nên把 tập hợp số lượng phần tử ít làm q1
            if (q1.size() > q2.size()) {
                Set<String> temp = q1;
                q1 = q2;
                q2 = temp;
            }
        }
        return -1;
    }

    // 把 s[j]拨 lên một lần
    String plusOne(String s, int j) {
        char[] ch = s.toCharArray();
        if (ch[j] == '9')
            ch[j] = '0';
        else
            ch[j] += 1;
        return new String(ch);
    }

    // 把 s[i]拨 xuống một lần
    String minusOne(String s, int j) {
        char[] ch = s.toCharArray();
        if (ch[j] == '0')
            ch[j] = '9';
        else
            ch[j] -= 1;
        return new String(ch);
    }

    List<String> getNeighbors(String s) {
        List<String> neighbors = new ArrayList<>();
        for (int i = 0; i < 4; i++) {
            neighbors.add(plusOne(s, i));
            neighbors.add(minusOne(s, i));
        }
        return neighbors;
    }
}
```

BFS hai chiều vẫn循 khung thuật toán BFS, nhưng có vài khác biệt chi tiết:

1、Không dùng hàng đợi lưu phần tử nữa, mà đổi dùng [tập băm](https://labuladong.online/algo/data-structure-basic/hash-set/), tiện nhanh判 hai tập hợp có giao nhau không.

2、Điều chỉnh vị trí return step. Vì trong BFS hai chiều không còn đơn giản判断 có đến终点 không, mà判断 hai tập hợp có giao nhau không, nên要在 tính ra nút 이웃时就 tiến hành判断.

3、Còn một điểm tối ưu, mỗi lần đều giữ `q1` là tập hợp số lượng phần tử ít hơn, như vậy có thể nhất định程度 giảm số lần tìm kiếm.

Vì按 logic BFS, phần tử trong hàng đợi (tập hợp) càng nhiều, sau khi扩散 nút 이웃 thì phần tử trong hàng đợi (tập hợp) mới càng nhiều; trong thuật toán BFS hai chiều, nếu mỗi lần chúng ta đều chọn một tập hợp ít hơn tiến hành扩散, vậy速度 tăng trưởng chiếm không gian就会 chậm hơn, hiệu suất就会 cao hơn.

Có điều nói lại, **dù BFS truyền thống hay BFS hai chiều, dù làm tối ưu hay không, từ tiêu chuẩn Big O đo, độ phức tạp thời gian đều giống nhau**, chỉ能 nói BFS hai chiều là một技巧 nâng cao,速度 chạy thuật toán sẽ相对 nhanh hơn, nắm hay không nắm thực ra đều无所谓.

Then chốt nhất vẫn要把 khung通用 BFS记下来,並做到熟练运用, phía sau có [Chương bài tập BFS](https://labuladong.online/algo/problem-set/bfs/), hãy bạn thử vận dụng技巧 của bài này, giải đề trong đó.

<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Thuật toán cây khung nhỏ nhất Prim](https://labuladong.online/algo/data-structure/prim/)
 - [【Luyện tập】Bài tập kinh điển BFS I](https://labuladong.online/algo/problem-set/bfs/)
 - [【Luyện tập】Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
 - [【Luyện tập】Bài tập kinh điển quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
 - [【Luyện tập】Bài tập kinh điển hợp nhất-tìm kiếm](https://labuladong.online/algo/problem-set/union-find/)
 - [【Luyện tập】Vận dụng duyệt tầng giải đề I](https://labuladong.online/algo/problem-set/binary-tree-level-i/)
 - [【Luyện tập】Vận dụng duyệt tầng giải đề II](https://labuladong.online/algo/problem-set/binary-tree-level-ii/)
 - [Thuật toán判定 đồ thị hai phía](https://labuladong.online/algo/data-structure/bipartite-graph/)
 - [Cơ bản và loại thường gặp của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
 - [Duyệt đệ quy/duyệt tầng của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
 - [Cương领 cốt lõi loạt thuật toán cây nhị phân](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
 - [Học tư duy khung của cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Mẹo tiết kiệm tiền du lịch: Đường đi ngắn nhất có trọng số](https://labuladong.online/algo/dynamic-programming/cheap-travel/)
 - [Phát hiện chu trình và thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)
 - [Dùng thuật toán đánh bại thuật toán](https://labuladong.online/algo/fname.html?fname=PDF中的算法)
 - [Học thuật toán và trải nghiệm dòng chảy](https://labuladong.online/algo/fname.html?fname=心流)

</details><hr>

<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin刷 đề Chrome của tôi](https://labuladong.online/algo/intro/chrome/) bấm vào các đề sau có thể xem trực tiếp思路 giải:</strong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1091. Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/?show=1) | [1091. Đường đi ngắn nhất trong ma trận nhị phân](https://leetcode.cn/problems/shortest-path-in-binary-matrix/?show=1) | 🟠 |
| [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/?show=1) | [111. Độ sâu nhỏ nhất của cây nhị phân](https://leetcode.cn/problems/minimum-depth-of-binary-tree/?show=1) | 🟢 |
| [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/?show=1) | [117. Điền con trỏ nút phải tiếp theo của mỗi nút II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/?show=1) | 🟠 |
| [127. Word Ladder](https://leetcode.com/problems/word-ladder/?show=1) | [127. Nối từ](https://leetcode.cn/problems/word-ladder/?show=1) | 🔴 |
| [1926. Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/?show=1) | [1926. Lối ra gần入口 nhất trong mê cung](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/?show=1) | 🟠 |
| [2850. Minimum Moves to Spread Stones Over Grid](https://leetcode.com/problems/minimum-moves-to-spread-stones-over-grid/?show=1) | [2850. Số lần di chuyển ít nhất để rải đá khắp lưới](https://leetcode.cn/problems/minimum-moves-to-spread-stones-over-grid/?show=1) | 🟠 |
| [286. Walls and Gates](https://leetcode.com/problems/walls-and-gates/?show=1)🔒 | [286. Tường và cổng](https://leetcode.cn/problems/walls-and-gates/?show=1)🔒 | 🟠 |
| [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/?show=1) | [310. Cây chiều cao nhỏ nhất](https://leetcode.cn/problems/minimum-height-trees/?show=1) | 🟠 |
| [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/?show=1) | [329. Đường tăng dài nhất trong ma trận](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/?show=1) | 🔴 |
| [365. Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/?show=1) | [365. Vấn đề ấm nước](https://leetcode.cn/problems/water-and-jug-problem/?show=1) | 🟠 |
| [431. Encode N-ary Tree to Binary Tree](https://leetcode.com/problems/encode-n-ary-tree-to-binary-tree/?show=1)🔒 | [431. Mã hóa cây N phân thành cây nhị phân](https://leetcode.cn/problems/encode-n-ary-tree-to-binary-tree/?show=1)🔒 | 🔴 |
| [433. Minimum Genetic Mutation](https://leetcode.com/problems/minimum-genetic-mutation/?show=1) | [433. Đột biến gen nhỏ nhất](https://leetcode.cn/problems/minimum-genetic-mutation/?show=1) | 🟠 |
| [490. The Maze](https://leetcode.com/problems/the-maze/?show=1)🔒 | [490. Mê cung](https://leetcode.cn/problems/the-maze/?show=1)🔒 | 🟠 |
| [505. The Maze II](https://leetcode.com/problems/the-maze-ii/?show=1)🔒 | [505. Mê cung II](https://leetcode.cn/problems/the-maze-ii/?show=1)🔒 | 🟠 |
| [542. 01 Matrix](https://leetcode.com/problems/01-matrix/?show=1) | [542. Ma trận 01](https://leetcode.cn/problems/01-matrix/?show=1) | 🟠 |
| [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/?show=1) | [547. Số lượng tỉnh](https://leetcode.cn/problems/number-of-provinces/?show=1) | 🟠 |
| [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/?show=1) | [863. Mọi nút cách K trong cây nhị phân](https://leetcode.cn/problems/all-nodes-distance-k-in-binary-tree/?show=1) | 🟠 |
| [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/?show=1) | [994. Cam thối](https://leetcode.cn/problems/rotting-oranges/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 109. Mở khóa mật mã](https://leetcode.cn/problems/zlDJc7/?show=1) | 🟠 |

</details>
<hr>

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

![](https://labuladong.online/algo/images/souyisou2.png)
