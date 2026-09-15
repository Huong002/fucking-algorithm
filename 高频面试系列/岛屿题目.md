# Càn quét mọi bài toán đảo trong một bài



![](https://labuladong.online/algo/images/souyisou1.png)

** thông báo: để đáp ứng nhu cầu của đông đảo độc giả, website đã mở [ lộ trình học cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người ~ ngoài ra, bạn nên học bài viết trên [ website ](https://labuladong.online/algo/), để có trải nghiệm tốt hơn.**



 đọc xong bài này, bạn không chỉ học sẽ thuật toán khuôn mẫu, còn có thể xuôi thì giải quyết như sau đề bài:

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [1020. Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/) | [1020. số lượng vùng đất tách rời ](https://leetcode.cn/problems/number-of-enclaves/) | 🟠 |
| [1254. Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/) | [1254. đếm số đảo khép kín ](https://leetcode.cn/problems/number-of-closed-islands/) | 🟠 |
| [1905. Count Sub Islands](https://leetcode.com/problems/count-sub-islands/) | [1905. đếm đảo con ](https://leetcode.cn/problems/count-sub-islands/) | 🟠 |
| [200. Number of Islands](https://leetcode.com/problems/number-of-islands/) | [200. số lượng đảo ](https://leetcode.cn/problems/number-of-islands/) | 🟠 |
| [694. Number of Distinct Islands](https://leetcode.com/problems/number-of-distinct-islands/)🔒 | [694. số lượng đảo khác nhau ](https://leetcode.cn/problems/number-of-distinct-islands/)🔒 | 🟠 |
| [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/) | [695. diện tích lớn nhất của đảo ](https://leetcode.cn/problems/max-area-of-island/) | 🟠 |

**-----------**



> [!NOTE]
> trước khi đọc bài này, bạn cần học trước:
>
> - [ thuật toán series cây nhị phân (cương lĩnh nhận bài) ](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
> - [ quay lui thuật toán cốt lõi khung ](https://labuladong.online/algo/essential-technique/backtrack-framework/)
> - [ một số thắc mắc về thuật toán quay lui/DFS ](https://labuladong.online/algo/essential-technique/backtrack-vs-dfs/)

 đảo series thuật toán vấn đề là bài phỏng vấn tần suất cao kinh điển, tuy bài cơ bản không khó, nhưng dạng bài này có một số mở rộng thú vị, ví dụ như tìm số lượng đảo con, tìm số lượng đảo có hình dạng khác nhau, v.v., bài này sẽ quét sạch các bài đó.

** đảo series đề bài điểm kiểm tra cốt lõi chính là dùng thuật toán DFS/BFS để duyệt mảng hai chiều **.

 bài này chủ yếu giảng cách dùng thuật toán DFS để xử gọn các bài toán series đảo, tuy nhiên mạch suy nghĩ cốt lõi khi dùng thuật toán BFS, chẳng qua là viết lại DFS thành BFS mà thôi.

 vậy thì dùng DFS tìm kiếm trong ma trận hai chiều thế nào? nếu bạn coi mỗi vị trí trong ma trận hai chiều là một node, bốn vị trí trên dưới trái phải của node này chính là các node kề nhau, thì toàn bộ ma trận có thể trừu tượng hóa thành một cấu trúc " đồ thị " cấu trúc.

 theo [ tư duy khung khi học cấu trúc dữ liệu và thuật toán ](https://labuladong.online/algo/essential-technique/algorithm-summary/), hoàn toàn có thể viết lại khung code DFS cho ma trận hai chiều dựa trên khung duyệt cây nhị phân:

```java
//  cây nhị phân  duyệt  khung 
void traverse(TreeNode root) {
    traverse(root.left);
    traverse(root.right);
}

//  hai chiều  ma trận  duyệt  khung 
void dfs(int[][] grid, int i, int j, boolean[][] visited) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n) {
        //  vượt  ra  tìm  dẫn  biên 
        return;
    }
    if (visited[i][j]) {
        //  đã  duyệt    (i, j)
        return;
    }

    //  vào  vào  khi  trước  node  (i, j)
    visited[i][j] = true;

    // tiến vào các node kề nhau (cây 4 nhánh) 
    //  trên 
    dfs(grid, i - 1, j, visited);
    //  dưới 
    dfs(grid, i + 1, j, visited);
    //  trái 
    dfs(grid, i, j - 1, visited);
    //  phải 
    dfs(grid, i, j + 1, visited);
}
```

 vì ma trận hai chiều về bản chất là một " đồ thị ", nên trong quá trình duyệt cần một mảng boolean `visited` để tránh đi đường cũ, nếu bạn hiểu được đoạn code trên, thì xử lý mọi bài toán series đảo đều rất đơn giản.

 ở đây nói thêm một mẹo nhỏ thường dùng để xử lý mảng hai chiều, đôi khi bạn sẽ thấy dùng " mảng hướng " để xử lý việc duyệt trên dưới trái phải, và phần trước [ giải chi tiết thuật toán union-find ](https://labuladong.online/algo/data-structure/union-find/) code rất tương tự:

```java
//  mảng hướng ,  chia  rời  đại diện  trên ,  dưới ,  trái ,  phải 
int[][] dirs = new int[][]{{-1,0}, {1,0}, {0,-1}, {0,1}};

void dfs(int[][] grid, int i, int j, boolean[][] visited) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n) {
        //  vượt  ra  tìm  dẫn  biên 
        return;
    }
    if (visited[i][j]) {
        //  đã  duyệt    (i, j)
        return;
    }

    //  vào  vào  node  (i, j)
    visited[i][j] = true;
    //  đệ quy  duyệt  trên dưới  khoảng    node 
    for (int[] d : dirs) {
        int next_i = i + d[0];
        int next_j = j + d[1];
        dfs(grid, next_i, next_j, visited);
    }
    //  rời khỏi  node  (i, j)
}
```

 cách viết này chẳng qua là dùng vòng lặp for để xử lý việc duyệt trên dưới trái phải mà thôi, bạn có thể chọn cách viết theo sở thích cá nhân. dưới đây giải bài theo khung trên kết hợp với bảng trực quan hóa.







## số lượng đảo

Đây là bài 200 " số lượng đảo ", bài đơn giản nhất và cũng kinh điển nhất, đề bài sẽ cho đầu vào một mảng hai chiều `grid`, trong đó chỉ chứa `0` hoặc `1`, `0` biểu diễn nước biển, `1` biểu diễn đất liền, và giả sử xung quanh ma trận này đều bị nước biển bao vây.

 chúng ta nói đất liền nối thành mảng tạo thành đảo, vậy thì hãy viết một thuật toán, tính số lượng đảo trong ma trận `grid` này, chữ ký hàm như sau:

```java
int numIslands(char[][] grid);
```

 ví dụ như đề bài cho bạn đầu vào `grid` có bốn hòn đảo, thuật toán nên trả về 4:

![](https://labuladong.online/algo/images/island/1.jpg)

 ý tưởng rất đơn giản, mấu chốt nằm ở cách tìm và đánh dấu " đảo ", lúc này cần thuật toán DFS phát huy tác dụng, chúng ta xem trực tiếp code lời giải:

```java
class Solution {
    // hàm chính, tính số lượng đảo 
    int numIslands(char[][] grid) {
        int res = 0;
        int m = grid.length, n = grid[0].length;
        //  duyệt  grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    // mỗi khi phát hiện một đảo, số lượng đảo cộng một 
                    res++;
                    //  sau đó  sử dụng  DFS  sẽ  đảo  nhấn chìm 
                    dfs(grid, i, j);
                }
            }
        }
        return res;
    }

    //  bắt đầu từ (i, j), biến mọi vùng đất liền kề nhau thành nước biển
    void dfs(char[][] grid, int i, int j) {
        int m = grid.length, n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n) {
            //  vượt  ra  tìm  dẫn  biên 
            return;
        }
        if (grid[i][j] == '0') {
            // đã là nước biển  
            return;
        }
        // biến (i, j) thành nước biển
        grid[i][j] = '0';
        // nhấn chìm vùng đất liền trên dưới trái phải  
        dfs(grid, i + 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/number-of-islands/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌟 animation trực quan hóa code 🌟</strong>
</summary>
</details>
</a>
<hr/>



** tại sao mỗi lần gặp đảo, đều dùng thuật toán DFS để " nhấn chìm "? chủ yếu để đỡ việc, tránh phải duy trì mảng `visited` mảng **.

 vì `dfs` hàm duyệt tới vị trí có giá trị `0` sẽ trả về trực tiếp, nên chỉ cần đặt mọi vị trí đã đi qua thành `0`, là có tác dụng không đi đường cũ.

> [!TIP]
> loại thuật toán DFS này còn có biệt danh là thuật toán FloodFill thuật toán, giờ có thấy cái tên FloodFill khá hợp không~ ~

 bài toán thuật toán cơ bản nhất này nói tới đây thôi, chúng ta xem các bài sau có trò gì.

## đảo khép kín số lượng

 bài trước nói xung quanh ma trận hai chiều có thể coi cũng bị nước biển bao vây, nên đất liền sát biên cũng được tính là đảo.

 bài 1254 bài " đếm số đảo khép kín " có hai điểm khác với bài trước:

1, dùng `0` biểu diễn đất liền, dùng `1` biểu diễn nước biển.

2, bắt bạn tính " đảo khép kín ". cái gọi là " đảo khép kín " chính là vùng `1` bị `0`, cũng chính là ** " đảo khép kín "**.

 chữ ký hàm như sau:

```java
int closedIsland(int[][] grid)
```

 ví dụ như đề bài cho bạn đầu vào ma trận hai chiều như sau:

![](https://labuladong.online/algo/images/island/2.png)

 thuật toán trả về 2, chỉ có vùng `0` màu xám trong hình là " đảo khép kín ".

** vậy thì kiểm tra " đảo khép kín "? thực ra rất đơn giản, loại bỏ những đảo sát biên trong bài trước, phần còn lại chẳng phải chính là " đảo khép kín " hay sao **?

 có mạch suy nghĩ này, là có thể xem trực tiếp code rồi, chú ý bài này quy định `0` biểu diễn đất liền, dùng `1` biểu diễn nước biển:

```java
class Solution {
    // hàm chính: tính số lượng đảo khép kín 
    public int closedIsland(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int j = 0; j < n; j++) {
            // nhấn chìm các đảo dựa vào cạnh trên
            dfs(grid, 0, j);
            // nhấn chìm các đảo dựa vào cạnh dưới
            dfs(grid, m - 1, j);
        }
        for (int i = 0; i < m; i++) {
            // nhấn chìm các đảo dựa vào cạnh trái
            dfs(grid, i, 0);
            // nhấn chìm các đảo dựa vào cạnh phải
            dfs(grid, i, n - 1);
        }
        //  duyệt  grid,  còn lại    đảo  đều  là  đảo khép kín 
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    res++;
                    dfs(grid, i, j);
                }
            }
        }
        return res;
    }

    //  bắt đầu từ (i, j), biến mọi vùng đất liền kề nhau thành nước biển
    void dfs(int[][] grid, int i, int j) {
        int m = grid.length, n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n) {
            return;
        }
        if (grid[i][j] == 1) {
            // đã là nước biển  
            return;
        }
        // biến (i, j) thành nước biển
        grid[i][j] = 1;
        // nhấn chìm vùng đất liền trên dưới trái phải  
        dfs(grid, i + 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/number-of-closed-islands/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🥳 animation trực quan hóa code 🥳</strong>
</summary>
</details>
</a>
<hr/>



 chỉ cần nhấn chìm trước mọi vùng đất liền sát biên, sau đó tính ra chính là đảo khép kín.

> [!TIP]
> xử lý dạng bài đảo này ngoài thuật toán DFS/BFS, Union Find thuật toán Disjoint Set cũng là một phương pháp có thể chọn, phần trước [ vận dụng thuật toán Union Find ](https://labuladong.online/algo/data-structure/union-find/) đã dùng thuật toán Union Find để giải một bài tương tự.

 lời giải của bài đảo này sửa một chút là giải được bài 1020 " số lượng vùng đất tách rời ", bài này không bắt bạn tìm số lượng đảo khép kín, mà bắt bạn tìm tổng diện tích đảo khép kín.

 thực ra mạch suy nghĩ đều giống nhau, nhấn chìm đất liền sát biên trước, sau đó đếm số đất liền còn lại là được, rất đơn giản. tuy nhiên chú ý trong bài 1020 `1` biểu diễn đất liền, `0` biểu diễn nước biển.

 vì giới hạn độ dài, code cụ thể mình không viết nữa, chúng ta tiếp tục xem các bài đảo khác.

## diện tích lớn nhất của đảo

Đây là bài 695 " diện tích lớn nhất của đảo ", `0` biểu diễn nước biển, `1` biểu diễn đất liền, giờ không bắt bạn tính số lượng đảo nữa, mà bắt bạn tính diện tích của đảo lớn nhất, chữ ký hàm như sau:

```java
int maxAreaOfIsland(int[][] grid)
```

 ví dụ như đề bài cho bạn đầu vào một ma trận hai chiều như sau:

![](https://labuladong.online/algo/images/island/3.jpg)

 trong đó đảo có diện tích lớn nhất là đảo màu cam đỏ, thuật toán trả về diện tích 6 của nó.

** mạch suy nghĩ tổng thể của bài này hoàn toàn giống trước đó, chỉ có điều `dfs` hàm nhấn chìm đảo đồng thời, còn nên tìm cách ghi lại diện tích của đảo này **.

 chúng ta có thể đặt giá trị trả về cho hàm `dfs`, ghi lại số đất liền nhấn chìm mỗi lần, xem trực tiếp lời giải nhé:

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        // ghi lại diện tích lớn nhất của đảo 
        int res = 0;
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    // nhấn chìm đảo, đồng thời cập nhật diện tích đảo lớn nhất
                    res = Math.max(res, dfs(grid, i, j));
                }
            }
        }
        return res;
    }

    // nhấn chìm vùng đất liền kề với (i, j), trả về diện tích đất liền đã nhấn chìm
    int dfs(int[][] grid, int i, int j) {
        int m = grid.length, n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n) {
            //  vượt  ra  tìm  dẫn  biên 
            return 0;
        }
        if (grid[i][j] == 0) {
            // đã là nước biển  
            return 0;
        }
        // biến (i, j) thành nước biển
        grid[i][j] = 0;

        return dfs(grid, i + 1, j)
            + dfs(grid, i, j + 1)
            + dfs(grid, i - 1, j)
            + dfs(grid, i, j - 1) + 1;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/max-area-of-island/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 animation trực quan hóa code 🌈</strong>
</summary>
</details>
</a>
<hr/>



 lời giải so với trước đó gần như nhau, mình không nói thêm nữa, hai bài đảo tiếp theo khá đòi hỏi kỹ xảo, chúng ta xem trọng điểm một chút.

## số lượng đảo con

 nếu nói các bài trước đều là bài mẫu, thì bài 1905 " đếm đảo con " có lẽ phải động não rồi:

<Problem slug="count-sub-islands" />

** mấu chốt của bài này nằm ở việc, kiểm tra nhanh đảo con thế nào **? chắc chắn có thể nhờ [thuật toán Union Find](https://labuladong.online/algo/data-structure/union-find/) để kiểm tra, tuy nhiên trọng tâm của bài này là thuật toán DFS, không mở rộng thuật toán Disjoint Set nữa.

 trong trường hợp nào thì một đảo `grid2` trong `B` là `grid1` trong `A` là đảo con của một đảo?

 khi trong đảo `B` mọi vùng đất liền đều cũng là đất liền trong đảo `A`, đảo `B` thì đảo `A` là đảo con của một đảo.

** nói ngược lại, nếu trong đảo `B` tồn tại một mảnh đất liền, mà tại vị trí tương ứng trong đảo `A` là nước biển, thì đảo `B` không phải là đảo con của đảo `A` là đảo con của một đảo **.

 như vậy, chúng ta chỉ cần duyệt mọi đảo trong `grid2`, loại bỏ những đảo không thể là đảo con, phần còn lại chính là đảo con.

 căn cứ mạch suy nghĩ này, có thể viết trực tiếp code dưới đây:

```java
class Solution {
    public int countSubIslands(int[][] grid1, int[][] grid2) {
        int m = grid1.length, n = grid1[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid1[i][j] == 0 && grid2[i][j] == 1) {
                    // đảo này chắc chắn không phải đảo con, nhấn chìm nó
                    dfs(grid2, i, j);
                }
            }
        }
        // lúc này các đảo còn lại trong grid2 đều là đảo con, tính số lượng đảo 
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid2[i][j] == 1) {
                    res++;
                    dfs(grid2, i, j);
                }
            }
        }
        return res;
    }

    //  bắt đầu từ (i, j), biến mọi vùng đất liền kề nhau thành nước biển
    void dfs(int[][] grid, int i, int j) {
        int m = grid.length, n = grid[0].length;
        if (i < 0 || j < 0 || i >= m || j >= n) {
            return;
        }
        if (grid[i][j] == 0) {
            return;
        }

        grid[i][j] = 0;
        dfs(grid, i + 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/count-sub-islands/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



 mạch suy nghĩ của bài này hơi giống với mạch suy nghĩ tính số lượng " đảo khép kín ", chỉ có điều cái sau loại bỏ những đảo sát biên, cái trước loại bỏ những đảo không thể là đảo con.

## số lượng đảo khác nhau

 đây là bài đảo cuối cùng của bài này, là bài chốt hạ, đương nhiên thú vị nhất.

 bài 694 bài " số lượng đảo khác nhau ", đề bài vẫn cho đầu vào một ma trận hai chiều, `0` biểu diễn nước biển, `1` biểu diễn đất liền, lần này bắt bạn tính số lượng đảo ** khác nhau (distinct)** số lượng đảo, chữ ký hàm như sau:

```java
int numDistinctIslands(int[][] grid)
```

 ví dụ như đề bài cho đầu vào ma trận hai chiều dưới đây:

![](https://labuladong.online/algo/images/island/5.jpg)

 trong đó có bốn đảo, nhưng đảo góc dưới trái và đảo góc trên phải có hình dạng giống nhau, nên tổng cộng có ba đảo khác nhau, thuật toán trả về 3.

 rõ ràng chúng ta phải tìm cách chuyển " đảo " trong ma trận hai chiều, thành kiểu như chuỗi chẳng hạn, sau đó dùng cấu trúc dữ liệu như HashSet để khử trùng lặp, cuối cùng thu được số lượng đảo khác nhau.

 nếu muốn chuyển đảo thành chuỗi, nói trắng ra chính là tuần tự hóa, tuần tự hóa nói trắng ra chính là duyệt mà, phần trước [ tuần tự hóa và phản tuần tự hóa cây nhị phân ](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/) đã giảng chuyển đổi qua lại giữa cây nhị phân và chuỗi, ở đây cũng tương tự.

** đầu tiên, đối với các đảo có hình dạng giống nhau, nếu xuất phát từ cùng một điểm khởi đầu, `dfs` thứ tự duyệt của hàm **.

 vì thứ tự duyệt được viết chết trong hàm đệ quy của bạn, không thay đổi động:

```java
void dfs(int[][] grid, int i, int j) {
    //  đệ quy  thứ tự : 
    //  trên 
    dfs(grid, i - 1, j);
    //  dưới 
    dfs(grid, i + 1, j);
    //  trái 
    dfs(grid, i, j - 1);
    //  phải 
    dfs(grid, i, j + 1);
}
```

 vì vậy, thứ tự duyệt theo nghĩa nào đó có thể dùng để mô tả hình dạng đảo, ví dụ như hai đảo trong hình dưới đây:

![](https://labuladong.online/algo/images/island/6.png)

 giả sử thứ tự duyệt của chúng là:

```
 dưới ,  phải ,  trên ,  quay lui  trên ,  quay lui  phải ,  quay lui  dưới 
```

 nếu mình lần lượt dùng `1, 2, 3, 4` để biểu diễn trên dưới trái phải, dùng `-1, -2, -3, -4` để biểu diễn thao tác quay lui trên dưới trái phải, thì có thể biểu diễn thứ tự duyệt của chúng như thế này:

```
2, 4, 1, -1, -4, -2
```

** bạn xem, đây tương đương với kết quả tuần tự hóa đảo, chỉ cần mỗi lần dùng `dfs` duyệt đảo thì sinh chuỗi số này để so sánh, là tính được rốt cuộc có bao nhiêu đảo khác nhau **.

::: info nhất định phải ghi lại thao tác " quay lui " hay không??

 độc giả tinh ý hỏi rằng, tại sao ghi lại thao tác " quay lui " mới biểu diễn duy nhất được thứ tự duyệt? không ghi thao tác quay lui dường như cũng được mà? không đúng, trên thực tế bắt buộc phải ghi thao tác quay lui.

 ví dụ như " dưới, phải, quay lui phải, quay lui dưới " và " dưới, quay lui dưới, phải, quay lui phải " hiển nhiên là hai thứ tự duyệt khác nhau, nhưng nếu không ghi thao tác quay lui, thì cả hai đều là " dưới, phải ", trở thành cùng một thứ tự duyệt, hiển nhiên là sai.

:::

 nên chúng ta cần cải tiến một chút hàm `dfs` hàm, thêm một số tham số hàm để ghi lại thứ tự duyệt:

```java
void dfs(int[][] grid, int i, int j, StringBuilder sb, int dir) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n 
        || grid[i][j] == 0) {
        return;
    }
    //  tiền thứ tự  duyệt  vị trí :  vào  vào  (i, j)
    grid[i][j] = 0;
    sb.append(dir).append(',');
    
    //  trên 
    dfs(grid, i - 1, j, sb, 1);
    //  dưới 
    dfs(grid, i + 1, j, sb, 2);
    //  trái 
    dfs(grid, i, j - 1, sb, 3);
    //  phải 
    dfs(grid, i, j + 1, sb, 4);
    
    //  hậu thứ tự  duyệt  vị trí :  rời khỏi  (i, j)
    sb.append(-dir).append(',');
}
```

> [!NOTE]
> xem kỹ code này, lựa chọn trước khi đệ quy, quay lui sau khi đệ quy, nó có giống giống,, [ quay lui thuật toán khung ](https://labuladong.online/algo/essential-technique/backtrack-framework/)? trên thực tế nó chính là thuật toán quay lui, vì nó quan tâm tới " cành cây " (), chứ không phải " node " ().
>
> bạn hoàn toàn có thể viết lại hàm này thành dạng chuẩn của thuật toán quay lui.

`dir` ghi lại hướng, `dfs` sau khi hàm, `sb` ghi lại toàn bộ thứ tự duyệt. có hàm `dfs` này là dễ rồi, chúng ta có thể viết trực tiếp code lời giải cuối cùng:

```java
class Solution {
    public int numDistinctIslands(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        // ghi lại kết quả tuần tự hóa của mọi đảo 
        HashSet<String> islands = new HashSet<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    // nhấn chìm đảo, đồng thời lưu kết quả tuần tự hóa của đảo 
                    StringBuilder sb = new StringBuilder();
                    // hướng ban đầu có thể viết tùy ý, không ảnh hưởng tính đúng đắn 
                    dfs(grid, i, j, sb, 666);
                    islands.add(sb.toString());
                }
            }
        }
        //  không  giống nhau    số lượng đảo 
        return islands.size();
    }

    private void dfs(int[][] grid, int i, int j, StringBuilder sb, int dir) {
        //  thấy  phần trên 
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/number-of-distinct-islands/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🥳 animation trực quan hóa code 🥳</strong>
</summary>
</details>
</a>
<hr/>



 như vậy, bài này được giải quyết, còn về việc tại sao khi gọi ban đầu hàm `dfs` `dir` tham số, có thể viết tùy ý, vì hàm `dfs` này trên thực tế là thuật toán quay lui, nó quan tâm tới " cành cây " chứ không phải " node ", phần trước [ kiến thức cơ bản về thuật toán đồ thị ](https://labuladong.online/algo/data-structure-basic/graph-basic/) có viết khác biệt cụ thể, ở đây không nhắc lại nữa.

 trên đây chính là mạch suy nghĩ để giải mọi bài toán series đảo, có lẽ các bài trước phần lớn mọi người đều làm được, nhưng hai bài cuối vẫn khá khéo léo, hy vọng bài này có ích cho bạn.







<hr>
<details class="hint-container details">
<summary><strong> Bài viết trích dẫn bài này </strong></summary>

 - [【 luyện tập tăng cường 】 bài tập kinh điển về BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui I](https://labuladong.online/algo/problem-set/backtrack-i/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về Disjoint Set ](https://labuladong.online/algo/problem-set/union-find/)
 - [ cương lĩnh cốt lõi của thuật toán series cây nhị phân ](https://labuladong.online/algo/essential-technique/binary-tree-summary/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong> Bài tập trích dẫn bài này </strong></summary>

<strong> cài đặt [ plugin luyện đề Chrome của tôi ](https://labuladong.online/algo/intro/chrome/) nhấp vào các bài dưới đây để xem trực tiếp ý tưởng giải bài: </strong>

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [1219. Path with Maximum Gold](https://leetcode.com/problems/path-with-maximum-gold/?show=1) | [1219. thợ mỏ vàng ](https://leetcode.cn/problems/path-with-maximum-gold/?show=1) | 🟠 |
| [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/?show=1) | [547. số lượng tỉnh ](https://leetcode.cn/problems/number-of-provinces/?show=1) | 🟠 |
| [79. Word Search](https://leetcode.com/problems/word-search/?show=1) | [79. tìm kiếm từ ](https://leetcode.cn/problems/word-search/?show=1) | 🟠 |
| [924. Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread/?show=1) | [924. giảm thiểu sự lây lan của phần mềm độc hại ](https://leetcode.cn/problems/minimize-malware-spread/?show=1) | 🔴 |
| [ câu hỏi phỏng vấn 13. phạm vi di chuyển của robot LCOF](https://leetcode.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/?show=1) | [ câu hỏi phỏng vấn 13. phạm vi di chuyển của robot ](https://leetcode.cn/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 105. diện tích lớn nhất của đảo ](https://leetcode.cn/problems/ZL6zAn/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)