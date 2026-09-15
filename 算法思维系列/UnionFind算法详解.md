# Thuật toán Union-Find (Disjoint Set)

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học đượccông thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) | [130. Vùng bị bao quanh](https://leetcode.cn/problems/surrounded-regions/) | 🟠 |
| [323. Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)🔒 | [323. Số lượng thành phần liên thông trong đồ thị vô hướng](https://leetcode.cn/problems/number-of-connected-components-in-an-undirected-graph/)🔒 | 🟠 |
| [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/) | [684. Kết nối dư thừa](https://leetcode.cn/problems/redundant-connection/) | 🟠 |
| [990. Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations/) | [990. Tính thỏa mãn của phương trình đẳng thức](https://leetcode.cn/problems/satisfiability-of-equality-equations/) | 🟠 |

**-----------**

> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản và duyệt cây đa phân](https://labuladong.online/algo/data-structure-basic/n-ary-tree-traverse-basic/)
> - [Cơ bản và cài đặt chung cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/)

Thuật toán hợp nhất-tìm kiếm Union-Find (Disjoint Set) là một thuật toán chuyên cho "tính liên thông động", tôi trước đây từng viết hai lần, vì tần suất khảo sát của thuật toán này cao, mà nó cũng là kiến thức tiền đề của thuật toán cây khung nhỏ nhất, nên tôi nguyên hợp bài này, cố gắng một bàigiảng hiểu thuật toán này.

Trước hết, bắt đầugiảng từ tính liên thông động của đồ thị là gì.

## Một, tính liên thông động

Nói đơn giản, tính liên thông động thực ra có thể trừu tượng thành nối dây cho một đồ thị. Ví như đồ thị dưới đây, tổng cộng có 10 nút, chúng không nối với nhau, lần lượt dùng 0~9 đánh dấu:

![](https://labuladong.online/algo/images/unionfind/1.jpg)

Bây giờ thuật toán Union-Find của chúng ta chủ yếu cần cài đặt hai API này:

```java
class UF {
    // Nối p và q
    public void union(int p, int q);
    // kiểm tra p và q có liên thông không
    public boolean connected(int p, int q);
    // Trả về trong đồ thị có bao nhiêu thành phần liên thông
    public int count();
}
```

"Liên thông" nói ở đây là một quan hệ tương đương, tức là nói có ba tính chất như sau:

1, Tính phản xạ: nút `p` và `p` liên thông.

2, Tính đối xứng: nếu nút `p` và `q` liên thông, vậy `q` và `p` cũng liên thông.

3, Tính bắc cầu: nếu nút `p` và `q` liên thông, `q` và `r` liên thông, vậy `p` và `r` cũng liên thông.

Ví như đồ thị trước đó, 0~9 hai điểm **khác nhau** bất kỳ đều không liên thông, gọi `connected` đều trả về false, thành phần liên thông là 10 cái.

Nếu bây giờ gọi `union(0, 1)`, vậy 0 và 1 được liên thông, thành phần liên thông giảm còn 9 cái.

Lại gọi `union(1, 2)`, lúc này 0,1,2 đều được liên thông, gọi `connected(0, 2)` cũng sẽ trả về true, thành phần liên thông biến thành 8 cái.

![](https://labuladong.online/algo/images/unionfind/2.jpg)

kiểm tra loại "quan hệ tương đương" này rất thực dụng, ví như trình biên dịch kiểm tra các tham chiếu khác nhau của cùng một biến, ví như tính vòng bạn bè trong mạng xã hội, v.v.

Như vậy, bạn hẳn đại khái hiểu là gì tính liên thông động, then chốt của thuật toán Union-Find nằm ở hiệu suất của hàm `union` và `connected`. Vậy dùng mô hình gì để biểu thị trạng thái liên thông của đồ thị này? Dùng cấu trúc dữ liệu gì để cài đặt code?

## Hai,ý tưởng cơ bản

Chú ý vừa rồi tôi đem "mô hình" và "cấu trúc dữ liệu" cụ thể tách ra nói, làm vậy là có nguyên nhân. Vì chúng ta dùng rừng (một số cây) để biểu thị tính liên thông động của đồ thị, dùng mảng để cài đặt cụ thể rừng này.

Dùng rừng biểu thị tính liên thông thế nào? Chúng ta giả sử mỗi nút của cây có một con trỏ chỉ nút cha của nó, nếu là nút gốc, con trỏ này chỉ chính mình. Ví như đồ thị 10 nút vừa rồi, lúc đầu không liên thông nhau, chính là như sau:

![](https://labuladong.online/algo/images/unionfind/3.jpg)

```java
class UF {
    // Ghi lại thành phần liên thông
    private int count;
    // Nút cha của nút x là parent[x]
    private int[] parent;

    // Hàm tạo, n là tổng số nút của đồ thị
    public UF(int n) {
        // Ban đầu không liên thông nhau
        this.count = n;
        // Con trỏ nút cha ban đầu chỉ chính mình
        parent = new int[n];
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }

    // Hàm khác
}
```

**Nếu hai nút nào đó được liên thông, thì để nút gốc của một nút (tùy ý) trong đó gắn vào nút gốc của nút còn lại**:

![](https://labuladong.online/algo/images/unionfind/4.jpg)

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ)
            return;
        // Hợp hai cây thành một cây
        parent[rootP] = rootQ;
        // parent[rootQ] = rootP cũng giống

        // Hai thành phần hợp hai làm một
        count--;
    }

    // Trả về nút gốc của một nút x nào đó
    private int find(int x) {
        // parent[x] == x của nút gốc
        while (parent[x] != x)
            x = parent[x];
        return x;
    }

    // Trả về số lượng thành phần liên thông hiện tại
    public int count() {
        return count;
    }
}
```

**Như vậy, nếu nút `p` và `q` liên thông, chúng nhất định có cùng nút gốc**:

![](https://labuladong.online/algo/images/unionfind/5.jpg)

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }
}
```

Đến đây, thuật toán Union-Find sẽ cơ bản hoàn thành. Có phải rất thần kỳ? không ngờ có thể dùng mảng như vậy để mô phỏng ra một rừng, khéo léo giải quyết vấn đề khá phức tạp này!

Vậy độ phức tạp của thuật toán này bao nhiêu? Chúng ta phát hiện, độ phức tạp trong API chính `connected` và `union` đều do hàm `find` gây ra, nên nói độ phức tạp của chúng và `find` giống nhau.

Chức năng chính của `find` chính là từ một nút nào đó duyệt lên đến gốc cây, độ phức tạp thời gian của nó chính là chiều cao của cây. Chúng ta có thể theo thói quen cho rằng chiều cao cây chính là `logN`, nhưng chuyện này không nhất định. Chiều cao `logN` chỉ tồn tại ở cây nhị phân cân bằng, với cây thường có thể xuất hiện tình huống cực đoan không cân bằng, khiến "cây" gần như suy biến thành "danh sách liên kết", chiều cao cây trường hợp xấu nhất có thể biến thành `N`.

![](https://labuladong.online/algo/images/unionfind/6.jpg)

Nên nói cách giải trên, độ phức tạp thời gian của `find`, `union`, `connected` đều là O(N). Độ phức tạp này rất không lý tưởng, bạn nghĩ xem lý thuyết đồ thị giải đều là vấn đề quy mô dữ liệu khổng lồ như mạng xã hội, với lệnh gọi `union` và `connected` rất thường xuyên, mỗi lần gọi cần thời gian tuyến tính hoàn toàn không thể chịu được.

**Then chốt của vấn đề nằm ở việc làm sao nghĩ cách tránh cây không cân bằng**? Chỉ cần dùng chút mẹo là được.

## Ba, tối ưu cân bằng

Chúng ta cần biết trường hợp nào có thể xuất hiện hiện tượng không cân bằng, then chốt nằm ở quá trình `union`:

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ)
            return;
        // Hợp hai cây thành một cây
        parent[rootP] = rootQ;
        // parent[rootQ] = rootP cũng được
        count--;
    }
}
```

Ban đầu chúng ta chính là đơn giản thô bạo đem cây chỗ `p` gắn vào dưới nút gốc của cây chỗ `q`, vậy chỗ này sẽ có thể xuất hiện tình trạng không cân bằng "đầu nặng chân nhẹ", ví như cục diện dưới đây:

![](https://labuladong.online/algo/images/unionfind/7.jpg)

Lâu ngày, cây có thể sinh trưởng rất không cân bằng.**Chúng ta thực ra hy vọng, cây nhỏ hơn gắn vào dưới cây lớn hơn, như vậy là có thể tránh đầu nặng chân nhẹ, cân bằng hơn**. Cách giải là dùng thêm một mảng `size`, ghi lại số nút mỗi cây chứa, chúng ta không ngại gọi là "trọng lượng":

```java
class UF {
    private int count;
    private int[] parent;
    // Thêm một mảng ghi "trọng lượng" của cây
    private int[] size;

    public UF(int n) {
        this.count = n;
        parent = new int[n];
        // Ban đầu mỗi cây chỉ có một nút
        // Trọng lượng hẳn khởi tạo 1
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }
    // Hàm khác
}
```

Ví như `size[3] = 5` biểu thị, cây lấy nút `3` làm gốc, tổng cộng có `5` nút. Như vậy chúng ta có thể sửa chút phương thức `union`:

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ)
            return;

        // Cây nhỏ gắn vào dưới cây lớn, khá cân bằng
        if (size[rootP] > size[rootQ]) {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        } else {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
        count--;
    }
}
```

Như vậy, thông qua so sánh trọng lượng cây, là có thể đảm bảo sinh trưởng của cây tương đối cân bằng, chiều cao cây đại khái ở số lượng khối `logN`, cực lớn nâng cao hiệu suất thực thi.

Lúc này, độ phức tạp thời gian của `find`, `union`, `connected` đều giảm còn O(logN), dù quy mô dữ liệu hàng trăm triệu, thời gian cần cũng rất ít.

## Bốn, nén đường đi (path compression)

Bước tối ưu này tuy code rất đơn giản, nhưng nguyên lý rất khéo léo.

**Thực ra chúng ta không quan tâm cấu trúc mỗi cây trông thế nào, chỉ quan tâm nút gốc**.

Vì dù cây trông thế nào, nút gốc của mỗi nút trên cây đều giống nhau, nên có thể nén sâu hơn chiều cao mỗi cây, khiến chiều cao cây thủy chung giữ ở hằng số không?

![](https://labuladong.online/algo/images/unionfind/8.jpg)

Như vậy nút cha của mỗi nút chính là nút gốc của cả cây, `find` là có thể lấy thời gian O(1) tìm được nút gốc của một nút nào đó, tương ứng, độ phức tạp `connected` và `union` đều giảm còn O(1).

Muốn làm được điểm này chủ yếu là sửa logic hàm `find`, rất đơn giản, nhưng bạn có thể thấy hai cách viết khác nhau.

Loại thứ nhất là trong `find` thêm một dòng code:

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    private int find(int x) {
        while (parent[x] != x) {
            // Dòng code này tiến hành nén đường đi
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }
}
```

Thao tác này hơi khó tin, xem GIF thì hiểu tác dụng của nó (để rõ ràng, cây này khá cực đoan):

![](https://labuladong.online/algo/images/unionfind/9.gif)

Dùng ngôn ngữ mô tả chính là, mỗi lần lặp while đều sẽ để một phần nút con di chuyển lên, như vậy mỗi lần gọi hàm `find` duyệt về gốc cây đồng thời, tiện tay rồi đem chiều cao cây rút ngắn.

Cách viết thứ hai của nén đường đi là như sau:

```java
class UF {
    // Để tiết kiệm dung lượng bài, lược phần code cho trên...

    // Phương thức find nén đường đi loại thứ hai
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
}
```

Tôi một thời từng cho rằng cách viết đệ quy này và cách viết lặp loại thứ nhất làm chuyện giống nhau, nhưng thực tế là tôi đã sơ ý rồi, có độc giả chỉ ra cách viết này tiến hành nén đường đi hiệu suất cao hơn cách giải trên.

Quá trình đệ quy này hơi khó hiểu, bạn có thể tự vẽ tay quá trình đệ quy. Tôi đem việc hàm này làm dịch thành dạng lặp, tiện cho bạn hiểu nguyên lý nó tiến hành nén đường đi:

```java
// Đoạn code lặp này tiện bạn hiểu chuyện code đệ quy làm
public int find(int x) {
    // Trước tìm nút gốc
    int root = x;
    while (parent[root] != root) {
        root = parent[root];
    }
    // Rồi đem tất cả nút từ x đến nút gốc trực tiếp gắn vào dưới nút gốc
    int old_parent = parent[x];
    while (x != root) {
        parent[x] = root;
        x = old_parent;
        old_parent = parent[old_parent];
    }
    return root;
}
```

Hiệu quả nén đường đi này như sau:

![](https://labuladong.online/algo/images/unionfind/10.jpeg)

So với loại nén đường đi thứ nhất, hiển nhiên phương pháp này nén triệt để hơn, trực tiếp đem cả một cành cây ép phẳng, một chút bất ngờ cũng không có. Dù một số trường hợp cực đoan sinh ra một cây khá cao, chỉ cần một lần nén đường đi là có thể đáng kể hạ thấp chiều cao cây, từ góc độ [phân tích khấu hao](https://labuladong.online/algo/essential-technique/complexity-analysis/) mà xem, độ phức tạp thời gian trung bình mọi thao tác vẫn là O(1), nên từ góc độ hiệu suất, khuyên bạn dùng thuật toán nén đường đi này.

**Ngoài ra, nếu dùng kỹ thuật nén đường đi, vậy tối ưu cân bằng của mảng `size` sẽ không cần thiết**. Nên thuật toán Union Find mà bạn thường thấy hẳn là cài đặt như sau:

```java
class UF {
    // Số lượng thành phần liên thông
    private int count;
    // Lưu nút cha của mỗi nút
    private int[] parent;

    // n là số lượng nút trong đồ thị
    public UF(int n) {
        this.count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

    // Liên thông nút p và nút q
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);

        if (rootP == rootQ)
            return;

        parent[rootQ] = rootP;
        // Hai thành phần liên thông hợp thành một thành phần liên thông
        count--;
    }

    // kiểm tra nút p và nút q có liên thông không
    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    // Trả về số lượng thành phần liên thông trong đồ thị
    public int count() {
        return count;
    }
}
```

Độ phức tạp của thuật toán Union-Find có thể phân tích như sau: hàm tạo khởi tạo cấu trúc dữ liệu cần độ phức tạp thời gian và không gian O(N); liên thông hai nút `union`, kiểm tra tính liên thông hai nút `connected`, tính thành phần liên thông `count` độ phức tạp thời gian đều làm O(1).

Đến đây, tin rằng bạn đã nắm logic cốt lõi của thuật toán Union-Find, tổng kết lại quá trình chúng ta tối ưu thuật toán:

1, Dùng mảng `parent` ghi nút cha của mỗi nút,tương đương với con trỏ chỉ nút cha, nên trong mảng `parent` thực tế lưu một rừng (một số cây đa phân).

2, Dùng mảng `size` ghi trọng lượng của mỗi cây, mục đích là để sau `union` cây vẫn có tính cân bằng, đảm bảo độ phức tạp thời gian các API là O(logN), mà không suy biến thành danh sách liên kết ảnh hưởng hiệu suất thao tác.

3, Trong hàm `find` tiến hành nén đường đi, đảm bảo chiều cao cây bất kỳ giữ ở hằng số, khiến độ phức tạp thời gian các API là O(1). Sau khi dùng nén đường đi, có thể không dùng tối ưu cân bằng của mảng `size`.

> [!TIP]
> Đa số thi viết đều cho phép bạn dùng IDE của mình code, nên bạn có thể trước hết đem lớp `UF` này dùng ngôn ngữ lập trình bạn quen viết sẵn, lúc thi viết cần thì trực tiếp lấy dùng. Lượng code của nó hơi nhiều, không cần viết từ đầu tại chỗ thi.

<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Thuật toán cây khung nhỏ nhất Kruskal](https://labuladong.online/algo/data-structure/kruskal/)
 - [Thuật toán cây khung nhỏ nhất Prim](https://labuladong.online/algo/data-structure/prim/)
 - [Nguyên lý Union Find](https://labuladong.online/algo/data-structure-basic/union-find-basic/)
 - [【Luyện tập】Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)
 - [【Luyện tập】Bài tập kinh điển hợp nhất-tìm kiếm](https://labuladong.online/algo/problem-set/union-find/)
 - [【Luyện tập】Vận dụng duyệt tầng giải đề II](https://labuladong.online/algo/problem-set/binary-tree-level-ii/)
 - [Một bàixử gọn mọi đề đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/)
 - [Cơ bản và loại thường gặp của cây nhị phân](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
 - [Học tư duy khung (framework) của cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Dùng thuật toán đánh bại thuật toán](https://labuladong.online/algo/fname.html?fname=PDF中的算法)

</details><hr>

<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của tôi](https://labuladong.online/algo/intro/chrome/) bấm vào các đề sau có thể xem trực tiếpý tưởng giải:</strong>

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [1361. Validate Binary Tree Nodes](https://leetcode.com/problems/validate-binary-tree-nodes/?show=1) | [1361. Kiểm chứng cây nhị phân](https://leetcode.cn/problems/validate-binary-tree-nodes/?show=1) | 🟠 |
| [200. Number of Islands](https://leetcode.com/problems/number-of-islands/?show=1) | [200. Số lượng đảo](https://leetcode.cn/problems/number-of-islands/?show=1) | 🟠 |
| [261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/?show=1)🔒 | [261.kiểm tra đồ thị có phải cây không](https://leetcode.cn/problems/graph-valid-tree/?show=1)🔒 | 🟠 |
| [310. Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/?show=1) | [310. Cây chiều cao nhỏ nhất](https://leetcode.cn/problems/minimum-height-trees/?show=1) | 🟠 |
| [368. Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset/?show=1) | [368. Tập con chia hết lớn nhất](https://leetcode.cn/problems/largest-divisible-subset/?show=1) | 🟠 |
| [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/?show=1) | [547. Số lượng tỉnh](https://leetcode.cn/problems/number-of-provinces/?show=1) | 🟠 |
| [582. Kill Process](https://leetcode.com/problems/kill-process/?show=1)🔒 | [582. Giết tiến trình](https://leetcode.cn/problems/kill-process/?show=1)🔒 | 🟠 |
| [737. Sentence Similarity II](https://leetcode.com/problems/sentence-similarity-ii/?show=1)🔒 | [737. Tương tự câu II](https://leetcode.cn/problems/sentence-similarity-ii/?show=1)🔒 | 🟠 |
| [765. Couples Holding Hands](https://leetcode.com/problems/couples-holding-hands/?show=1) | [765. Cặp đôi nắm tay](https://leetcode.cn/problems/couples-holding-hands/?show=1) | 🔴 |
| [924. Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread/?show=1) | [924. Giảm thiểu lan truyền phần mềm độc hại](https://leetcode.cn/problems/minimize-malware-spread/?show=1) | 🔴 |
| [947. Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/?show=1) | [947. Loại nhiều nhất đá cùng hàng hoặc cùng cột](https://leetcode.cn/problems/most-stones-removed-with-same-row-or-column/?show=1) | 🟠 |

</details>
<hr>

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

![](https://labuladong.online/algo/images/souyisou2.png)
