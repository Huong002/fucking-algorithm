# Thuật toán BFSxử gọn đủ loại game trí tuệ

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho user cũ~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học đượccông thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | Lực khấu | Độ khó |
| :----: | :----: | :----: |
| [773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/) | [773. Câu đố trượt](https://leetcode.cn/problems/sliding-puzzle/) | 🔴

**-----------**

Game ghép hình trượt mọi người hẳn đều chơi qua, dưới đây là một ghép hình trượt 4x4:

![](https://labuladong.online/algo/images/sliding_puzzle/1.jpeg)

Trong ghép hình có một ô trống, có thể lợi dụng ô trống này để di chuyển số khác. Bạn cần thông qua di chuyển những số này, nhận được một thứ tự sắp xếp cụ thể nào đó, như vậy tính là thắng.

Hồi nhỏ tôi còn chơi một game trí tuệ gọi là "Hoa Dung Đạo", cũng khá tương tự ghép hình trượt:

![](https://labuladong.online/algo/images/sliding_puzzle/2.jpeg)

Thực tế, game ghép hình trượt cũng gọi là Hoa Dung Đạo số, bạn xem hai thứ khá tương tự.

Vậy game này chơi thế nào? Tôi nhớ là có một sốcông thức, tương tự công thức khôi phục rubik. Nhưng hôm nay chúng ta không nghiên cứu kỹ thuật khiến người hói đầu, **những game trí tuệ này tất cả đều có thể dùng thuật toán tìm kiếm vét cạn để giải quyết, nên hôm nay chúng ta sẽ học đi đôi với hành, dùng khung thuật toán BFS để xử gọn những game này**.

### Một, phân tích đề bài

LeetCode 773 "Câu đố trượt" chính là vấn đề này, yêu cầu của đề như sau:

Cho bạn một ghép hình trượt 2x3, dùng một mảng 2x3 `board` biểu thị. Trong ghép hình có sáu số 0~5, trong đó**số 0 sẽ biểu thị ô trống đó**, bạn có thể di chuyển số trong đó, khi `board` biến thành `[[1, 2, 3], [4, 5, 0]]`, thắng game.

Hãy viết một thuật toán, tính số lần di chuyển ít nhất cần để thắng game, nếu không thể thắng game, trả về -1.

Ví như mảng hai chiều nhập `board = [[4,1,2],[5,0,3]]`, thuật toán hẳn trả về 5:

![](https://labuladong.online/algo/images/sliding_puzzle/5.jpeg)

Nếu nhập là `board = [[1,2,3],[5,4,0]]`, thuật toán trả về -1, vì trong cục diện này dù thế nào cũng không thể thắng game.

### Hai, phân tíchý tưởng

Với loại vấn đề tính số bước ít nhất này, chúng ta sẽ cần nhạy bén nghĩ đến thuật toán BFS.

Đề này chuyển thành vấn đề BFS có một số kỹ thuật, chúng ta đối mặt với các vấn đề như sau:

1, Thuật toán BFS thường, là từ một điểm bắt đầu `start` xuất phát, tìm đường đến điểm cuối `target`, nhưng vấn đề ghép hình không phải đang tìm đường, mà đang không ngừng hoán đổi số, chuyện này hẳn chuyển thành bài toán BFS thế nào?

2, Dù vấn đề này có thể chuyển thành vấn đề BFS, xử lý điểm bắt đầu `start` và điểm cuối `target` thế nào? Chúng đều là mảng, đem mảng đặt vào hàng đợi, áp dụng khung BFS, nghĩ thôi đã thấy khá phiền phức và kém hiệu quả.

Trước trả lời câu hỏi thứ nhất, **thuật toán BFS không chỉ là một thuật toán tìm đường, mà là một thuật toán tìm kiếm bạo lực**, chỉ cần liên quan vấn đềvét cạn bạo lực, BFS sẽ có thể dùng, mà có thể nhanh nhất tìm được đáp án.

Bạn nghĩ xem máy tính giải vấn đề thế nào? Đâu cókỹ thuật đặc biệt gì, bản chất chính là đem tất cả nghiệm khả thi bạo lựcvét cạn ra, rồi từ đó tìm một giải tối ưu mà thôi.

Hiểu rõ đạo lý này, vấn đề của chúng ta sẽ chuyển thành:**làm saovét cạn ra tất cả cục diện mà `board` cục diện hiện tại có thể phát sinh ra**? Chuyện này sẽ đơn giản, xem vị trí số 0, hoán đổi với số trên-dưới-trái-phải là được:

![](https://labuladong.online/algo/images/sliding_puzzle/3.jpeg)

Như vậy thực ra chính là một vấn đề BFS, mỗi lần trước hết tìm số 0, rồi hoán đổi với số xung quanh, tạo thành cục diện mới thêm vào hàng đợi... Khi lần đầu đến `target`, thì nhận được số bước ít nhất để thắng game.

Với câu hỏi thứ hai, `board` chỗ này vẻn vẹn là mảng hai chiều 2x3, nên có thể nén thành một chuỗi một chiều.**Trong đó khá cókỹ thuật điểm nằm ở, mảng hai chiều có khái niệm "trên-dưới-trái-phải", nén thành một chiều xong, làm sao nhận được chỉ số trên-dưới-trái-phải của một chỉ số nào đó**?

Với bài này, đề nói kích thước mảng nhập đều là 2 x 3, nên chúng ta có thể trực tiếp viết tay ra ánh xạ này:

<!-- muliti_language -->
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

**Hàm ý này chính là, trong chuỗi một chiều, chỉ số kề trong mảng hai chiều của chỉ số `i` là `neighbor[i]`**:

![](https://labuladong.online/algo/images/sliding_puzzle/4.jpeg)

Vậy với một mảng hai chiều `m x n`, viết tay ánh xạ chỉ số một chiều của nó chắc chắn không thực tế, làm sao dùng code sinh ánh xạ chỉ số một chiều của nó?

Quan sát hình trên là có thể phát hiện, nếu một phần tử `e` nào đó trong mảng hai chiều có chỉ số trong mảng một chiều là `i`, vậy chỉ số trong mảng một chiều của phần tử kề trái-phải của `e` chính là `i - 1` và `i + 1`, mà chỉ số trong mảng một chiều của phần tử kề trên-dưới của `e` chính là `i - n` và `i + n`, trong đó `n` là số cột của mảng hai chiều.

Như vậy, với mảng hai chiều `m x n`, chúng ta có thể viết một hàm để sinh ánh xạ chỉ số `neighbor` của nó:

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

Đến đây, chúng ta rồi đem vấn đề này hoàn toàn chuyển thành vấn đề BFS chuẩn, nhờ khung code của bài trước [Khung thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/), trực tiếp là có thể áp dụng ra code cách giải:

<!-- muliti_language -->
```java
class Solution {
    public int slidingPuzzle(int[][] board) {
        int m = 2, n = 3;
        StringBuilder sb = new StringBuilder();
        String target = "123450";
        // Chuyển mảng 2x3 thành chuỗi làm điểm bắt đầu BFS
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                sb.append(board[i][j]);
            }
        }
        String start = sb.toString();

        // Ghi lại chỉ số kề nhau của chuỗi một chiều
        int[][] neighbor = new int[][]{
                {1, 3},
                {0, 4, 2},
                {1, 5},
                {0, 4},
                {3, 1, 5},
                {4, 2}
        };

        /******* Khung thuật toán BFS bắt đầu *******/
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
                // kiểm tra có đến cục diện mục tiêu không
                if (target.equals(cur)) {
                    return step;
                }
                // Tìm chỉ số của số 0
                int idx = 0;
                for (; cur.charAt(idx) != '0'; idx++) ;
                // Hoán đổi số 0 và số kề nhau
                for (int adj : neighbor[idx]) {
                    String new_board = swap(cur.toCharArray(), adj, idx);
                    // tránh đi đường quay lại
                    if (!visited.contains(new_board)) {
                        q.offer(new_board);
                        visited.add(new_board);
                    }
                }
            }
            step++;
        }
        /******* Khung thuật toán BFS kết thúc *******/
        return -1;
    }

    private String swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
        return new String(chars);
    }
}
```

<visual slug='sliding-puzzle'/>

Đến đây, đề này sẽ giải xong, thực ra khung hoàn toàn không đổi, công thức đều giống nhau, chúng ta chỉ tốn khá nhiều thời gian đem game ghép hình trượt chuyển thành thuật toán BFS.

Nhiều game trí tuệ đều như vậy, tuy trông đặc biệt khéo léo, nhưng đều không chịu nổi bạo lựcvét cạn, thuật toán thường dùng chính là thuật toán quay lui hoặc thuật toán BFS.

<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Khungcông thức giải đề bằng thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/)

</details><hr>

<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của tôi](https://labuladong.online/algo/intro/chrome/) bấm vào các đề sau có thể xem trực tiếpý tưởng giải:</strong>

| LeetCode | Lực khấu |
| :----: | :----: |
| [365. Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/?show=1) | [365. Vấn đề ấm nước](https://leetcode.cn/problems/water-and-jug-problem/?show=1) |

</details>
<hr>

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

** “ Ghi chép thuật toán của labuladong ” đã xuất bản, theo dõikênh WeChat chính thức xem chi tiết; nhắn tin tới hộp thư "** toàn **" có thể tải PDFđi kèm vàbộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)
