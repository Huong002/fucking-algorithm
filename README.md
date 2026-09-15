[![Star History Chart](https://api.star-history.com/svg?repos=labuladong/fucking-algorithm&type=Date)](https://star-history.com/#labuladong/fucking-algorithm&Date)


English version is on [labuladong.online](https://labuladong.online/algo/en/) too. Just enjoy：)

# Ghi chép thuật toán của labuladong

Kho này có tổng cộng hơn 60 bài viết gốc, đều dựa trên các đề LeetCode, bao phủ mọi dạng đề và kỹ thuật, và nhất định phải đạt được **suy một ra ba, dễ hiểu**, tuyệt đối không phải là nơi chất đống code đơn thuần. Mục lục ở phía sau.

Tôi xin "xả" vài câu trước. **Luyện đề, luyện đề, luyện là đề, rèn là tư duy, mục đích của kho này chính là truyền đạt tư duy thuật toán ấy**. Nếu tôi chỉ viết một kho chứa code đề LeetCode thì có tác dụng quái gì? Không có giải thích hướng suy nghĩ, không có khung tư duy, cùng lắm ghi thêm độ phức tạp thời gian, thứ mà nhìn một cái là thấy ngay.

Nếu chỉ muốn đáp án thì rất dễ, dưới phần bình luận mỗi đề có đủ loại đáp án, thỉnh thoảng còn khoe code Python một dòng là xong, được bao nhiêu người like. Vấn đề là, bạn đi làm đề thuật toán, là để học mẹo vặt của ngôn ngữ lập trình, hay để học tư duy thuật toán? Niềm vui của bạn, rốt cuộc đến từ việc copy một dòng code của người khác cho qua test, số đề hoàn thành +1, hay đến từ việc tự mình dùng suy luận logic và khung thuật toán viết ra lời giải mà không cần xem đáp án?

Trên mạng hay có cao thủ chê tôi, nói thứ tôi viết quá cơ bản, hoặc nói không thể dựa vào tư duy khung để học thuật toán. Tôi chỉ có thể nói mọi người luyện thuật toán là để kiếm việc nuôi thân, không phải để thi đấu, tôi cũng từng lăn lộn bò trườn mà lên, chúng ta cần rõ ràng minh bạch và thu được điều gì đó, chứ không phải làm ra vẻ huyền bí mà chẳng chỉ ra điều gì.

Không tìm cách viết cho dễ hiểu, chẳng lẽ vừa vào đã thổi phồng cuốn *Introduction to Algorithms* lên tận trời, rồi khiến người ta kính nể mà chùn bước rút lui?

**Việc gì làm nhiều rồi cũng phát hiện ra công thức, tôi tổng kết các công thức khung của đủ loại thuật toán, tin rằng có thể giúp người khác bớt đi đường vòng**. Tôi là một đứa hoàn toàn tự học, mất một năm luyện đề và tổng kết, tự viết một bản cheat-sheet thuật toán, mục lục ở phía sau, ở đây không lảm nhảm nữa.

## Trước khi bắt đầu học

**1. Hãy cho kho này một star trước, thỏa mãn chút hư vinh của tôi**, chất lượng bài viết tuyệt đối xứng đáng với một star của bạn. Tôi vẫn đang tiếp tục sáng tác, hãy cho tôi chút động lực để viết tiếp, cảm ơn.

**2. Nên lưu lại website trực tuyến của tôi, đầu mỗi bài viết đều có link đề LeetCode tương ứng, có thể vừa đọc bài vừa luyện đề, tổng cộng có thể cầm tay dẫn bạn luyện 500 đề**：

2024 Địa chỉ mới nhất：https://labuladong.online/algo/

~~Địa chỉ GitHub Pages: https://labuladong.online/algo/~~

~~Địa chỉ Gitee Pages: https://labuladong.gitee.io/algo/~~

## Giới thiệu combo luyện đề của labuladong

### 1. Bảng trực quan hóa thuật toán

Website thuật toán, mọi plugin đi kèm của tôi đều tích hợp một công cụ trực quan hóa thuật toán, có thể trực quan hóa cấu trúc dữ liệu và quá trình đệ quy, giảm mạnh độ khó khi hiểu thuật toán. Code lời giải của hầu như mọi đề đều có bảng trực quan hóa tương ứng, xem giới thiệu bên dưới để biết chi tiết.


### 2. Website học tập

Nội dung đương nhiên là phần cốt lõi nhất trong chuỗi hướng dẫn thuật toán của tôi, mọi hướng dẫn thuật toán của tôi đều đăng trên website [labuladong.online](https://labuladong.online/algo/), tin rằng bạn sẽ dành rất nhiều thời gian học tập ở đây trong tương lai, chứ không chỉ thêm vào bookmark~

![](https://labuladong.github.io/pictures/简介/web_intro1.jpg)

### 3. Plugin Chrome

**Chức năng chính**：Plugin Chrome có thể xem nhanh "lời giải" hay "hướng suy nghĩ" của tôi ngay trên LeetCode bản tiếng Trung hoặc LeetCode bản tiếng Anh, đồng thời thêm quan hệ tham chiếu giữa đề bài và kỹ thuật thuật toán, có thể liên động với website/tài khoản công chúng/khóa học của tôi, mang đến cho độc giả trải nghiệm luyện đề mượt mà nhất. Sách hướng dẫn cài đặt và sử dụng xem ở mục lục bên dưới.

![](https://labuladong.github.io/pictures/简介/chrome_intro.jpg)


### 4. Plugin vscode

**Chức năng chính**：Về cơ bản giống chức năng của plugin Chrome, độc giả quen luyện đề trên vscode có thể dùng plugin này. Sách hướng dẫn cài đặt và sử dụng xem ở mục lục bên dưới.

![](https://labuladong.github.io/pictures/简介/vs_intro.jpg)


### 5. Plugin Jetbrains

**Chức năng chính**：Về cơ bản giống chức năng của plugin Chrome, độc giả quen luyện đề trên IDE nhà Jetbrains (PyCharm/Intellij/Goland, v.v.) có thể dùng plugin này. Sách hướng dẫn cài đặt và sử dụng xem ở mục lục bên dưới.

![](https://labuladong.github.io/pictures/简介/jb_intro.jpg)


Cuối cùng chúc mọi người học vui, tự do bơi lội trong biển đề!


# Mục lục bài viết

<!-- table start -->


* [Giới thiệu website](https://labuladong.online/algo/home/)

* [Lộ trình học cho người mới và học cấp tốc](https://labuladong.online/algo/menu/plan/)
  * [Lộ trình học cấp tốc theo mục lục](https://labuladong.online/algo/intro/quick-learning-plan/)
  * [Lộ trình học đầy đủ theo mục lục](https://labuladong.online/algo/intro/beginner-learning-plan/)
  * [Trọng tâm và bẫy thường gặp khi luyện thuật toán](https://labuladong.online/algo/intro/how-to-learn-algorithms/)
  * [Cách luyện tập/ôn tập các chương bài tập](https://labuladong.online/algo/intro/how-to-practice/)

* [Hướng dẫn sử dụng công cụ học tập đi kèm](https://labuladong.online/algo/menu/tools/)
  * [Trợ giảng AI giải đáp mọi lúc](https://labuladong.online/algo/intro/ai-assistant/)
  * [Hướng dẫn dùng bảng trực quan hóa thuật toán](https://labuladong.online/algo/intro/visualize/)
  * [Cách chơi và tổng hợp game thuật toán](https://labuladong.online/algo/intro/game/)
  * [Plugin Chrome luyện đề đi kèm](https://labuladong.online/algo/intro/chrome/)
  * [Plugin vscode/cursor luyện đề đi kèm](https://labuladong.online/algo/intro/vscode/)
  * [Plugin JetBrains luyện đề đi kèm](https://labuladong.online/algo/intro/jetbrains/)
  * [Hội viên trả phí của website](https://labuladong.online/algo/intro/site-vip/)

* [Nhập môn: kiến thức cơ bản và bài luyện ngôn ngữ lập trình](https://labuladong.online/algo/menu/)
  * [Mở đầu chương](https://labuladong.online/algo/intro/programming-language-basic/)
  * [Nền tảng ngôn ngữ C++](https://labuladong.online/algo/programming-language-basic/cpp/)
  * [Nền tảng ngôn ngữ Java](https://labuladong.online/algo/programming-language-basic/java/)
  * [Nền tảng ngôn ngữ Golang](https://labuladong.online/algo/programming-language-basic/golang/)
  * [Nền tảng ngôn ngữ Python](https://labuladong.online/algo/programming-language-basic/python/)
  * [Nền tảng ngôn ngữ JavaScript](https://labuladong.online/algo/intro/js/)
  * [Những điều cần biết khi giải đề LeetCode](https://labuladong.online/algo/intro/leetcode/)
  * [Thực hành luyện đề theo ngôn ngữ lập trình](https://labuladong.online/algo/programming-language-basic/lc-practice/)
  * [Template code chế độ ACM](https://labuladong.online/algo/intro/acm-mode/)

* [Cơ bản: giảng kỹ cấu trúc dữ liệu và sắp xếp](https://labuladong.online/algo/menu/quick-start/)
  * [Mở đầu chương](https://labuladong.online/algo/intro/data-structure-basic/)
  * [Nhập môn độ phức tạp thời gian và không gian](https://labuladong.online/algo/intro/complexity-basic/)

  * [Cầm tay hướng dẫn cài đặt mảng động](https://labuladong.online/algo/menu/dynamic-array/)
    * [Nguyên lý cơ bản của mảng (lưu trữ tuần tự)](https://labuladong.online/algo/data-structure-basic/array-basic/)
    * [Cài đặt code mảng động](https://labuladong.online/algo/data-structure-basic/array-implement/)

  * [Cầm tay hướng dẫn cài đặt danh sách liên kết đơn/kép](https://labuladong.online/algo/menu/linked-list/)
    * [Nguyên lý cơ bản của danh sách liên kết (lưu trữ mắt xích)](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
    * [Cài đặt code danh sách liên kết](https://labuladong.online/algo/data-structure-basic/linkedlist-implement/)
    * [[Game] Cài đặt game rắn săn mồi](https://labuladong.online/algo/game/snake/)

  * [Các biến thể của mảng và danh sách liên kết](https://labuladong.online/algo/menu/arr-linked/)
    * [Kỹ thuật mảng vòng và cách cài đặt](https://labuladong.online/algo/data-structure-basic/cycle-array/)
    * [Nguyên lý cốt lõi của skip list](https://labuladong.online/algo/data-structure-basic/skip-list-basic/)
    * [Nguyên lý bitmap và cách cài đặt](https://labuladong.online/algo/data-structure-basic/bitmap/)

  * [Cầm tay hướng dẫn cài đặt hàng đợi/ngăn xếp](https://labuladong.online/algo/menu/queue-stack/)
    * [Nguyên lý cơ bản của hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/queue-stack-basic/)
    * [Dùng danh sách liên kết cài đặt hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/linked-queue-stack/)
    * [Dùng mảng cài đặt hàng đợi/ngăn xếp](https://labuladong.online/algo/data-structure-basic/array-queue-stack/)
    * [Nguyên lý deque (hàng đợi hai đầu) và cách cài đặt](https://labuladong.online/algo/data-structure-basic/deque-implement/)

  * [Nguyên lý bảng băm và cách cài đặt](https://labuladong.online/algo/menu/hash-table/)
    * [Nguyên lý cốt lõi của bảng băm](https://labuladong.online/algo/data-structure-basic/hashmap-basic/)
    * [Dùng phương pháp chaining cài đặt bảng băm](https://labuladong.online/algo/data-structure-basic/hashtable-chaining/)
    * [Hai điểm khó của phương pháp thăm dò tuyến tính](https://labuladong.online/algo/data-structure-basic/linear-probing-key-point/)
    * [Hai cách cài đặt code phương pháp thăm dò tuyến tính](https://labuladong.online/algo/data-structure-basic/linear-probing-code/)
    * [Nguyên lý hash set và cách cài đặt code](https://labuladong.online/algo/data-structure-basic/hash-set/)

  * [Các biến thể của cấu trúc bảng băm](https://labuladong.online/algo/menu/hash-table-variation/)
    * [Dùng danh sách liên kết tăng cường bảng băm (LinkedHashMap)](https://labuladong.online/algo/data-structure-basic/hashtable-with-linked-list/)
    * [Dùng mảng tăng cường bảng băm (ArrayHashMap)](https://labuladong.online/algo/data-structure-basic/hashtable-with-array/)
    * [Nguyên lý bộ lọc Bloom và cách cài đặt](https://labuladong.online/algo/data-structure-basic/bloom-filter/)

  * [Cấu trúc cây nhị phân và cách duyệt](https://labuladong.online/algo/menu/binary-tree/)
    * [Kiến thức cơ bản và các loại cây nhị phân thường gặp](https://labuladong.online/algo/data-structure-basic/binary-tree-basic/)
    * [Duyệt cây nhị phân bằng đệ quy/duyệt theo tầng](https://labuladong.online/algo/data-structure-basic/binary-tree-traverse-basic/)
    * [Trường hợp áp dụng của DFS và BFS](https://labuladong.online/algo/data-structure-basic/use-case-of-dfs-bfs/)
    * [Duyệt cây đa phân bằng đệ quy/duyệt theo tầng](https://labuladong.online/algo/data-structure-basic/n-ary-tree-traverse-basic/)

  * [Các biến thể của cấu trúc cây nhị phân](https://labuladong.online/algo/menu/binary-tree/)
    * [Ứng dụng cây tìm kiếm nhị phân và trực quan hóa](https://labuladong.online/algo/data-structure-basic/tree-map-basic/)
    * [Cân bằng hoàn hảo của cây đỏ-đen và trực quan hóa](https://labuladong.online/algo/data-structure-basic/rbtree-basic/)
    * [Nguyên lý Trie/cây từ điển/cây tiền tố và trực quan hóa](https://labuladong.online/algo/data-structure-basic/trie-map-basic/)
    * [Nguyên lý cốt lõi của binary heap và trực quan hóa](https://labuladong.online/algo/data-structure-basic/binary-heap-basic/)
    * [Cài đặt code binary heap/hàng đợi ưu tiên](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/)
    * [Nguyên lý cốt lõi của cây đoạn (segment tree) và trực quan hóa](https://labuladong.online/algo/data-structure-basic/segment-tree-basic/)
    * [Nén dữ liệu và cây Huffman](https://labuladong.online/algo/data-structure-basic/huffman-tree/)
    * [Đang cập nhật](https://labuladong.online/algo/intro/updating/)

  * [Kiến thức cơ bản về cấu trúc đồ thị và tổng quan thuật toán](https://labuladong.online/algo/menu/graph-theory/)
    * [Thuật ngữ cơ bản trong lý thuyết đồ thị](https://labuladong.online/algo/data-structure-basic/graph-terminology/)
    * [Cài đặt code tổng quát cho cấu trúc đồ thị](https://labuladong.online/algo/data-structure-basic/graph-basic/)
    * [Duyệt đồ thị bằng DFS/BFS](https://labuladong.online/algo/data-structure-basic/graph-traverse-basic/)
    * [Đồ thị Euler và game vẽ một nét](https://labuladong.online/algo/data-structure-basic/eulerian-graph/)
    * [Tổng quan thuật toán đường đi ngắn nhất trên đồ thị](https://labuladong.online/algo/data-structure-basic/graph-shortest-path/)
    * [Tổng quan thuật toán cây khung nhỏ nhất](https://labuladong.online/algo/data-structure-basic/graph-minimum-spanning-tree/)
    * [Nguyên lý Union-Find (DSU)](https://labuladong.online/algo/data-structure-basic/union-find-basic/)
    * [Đang cập nhật](https://labuladong.online/algo/intro/updating/)

  * [Nguyên lý 10 thuật toán sắp xếp và trực quan hóa](https://labuladong.online/algo/menu/sorting/)
    * [Mở đầu chương](https://labuladong.online/algo/intro/sorting/)
    * [Chỉ số quan trọng của thuật toán sắp xếp](https://labuladong.online/algo/data-structure-basic/sort-basic/)
    * [Vấn đề của sắp xếp chọn (selection sort)](https://labuladong.online/algo/data-structure-basic/select-sort/)
    * [Sắp xếp nổi bọt (bubble sort) có tính ổn định](https://labuladong.online/algo/data-structure-basic/bubble-sort/)
    * [Tư duy ngược: sắp xếp chèn (insertion sort)](https://labuladong.online/algo/data-structure-basic/insertion-sort/)
    * [Vượt qua O(N^2): sắp xếp Shell](https://labuladong.online/algo/data-structure-basic/shell-sort/)
    * [Dùng khéo vị trí tiền thứ tự của cây nhị phân: sắp xếp nhanh (quick sort)](https://labuladong.online/algo/data-structure-basic/quick-sort/)
    * [Dùng khéo vị trí hậu thứ tự của cây nhị phân: sắp xếp trộn (merge sort)](https://labuladong.online/algo/data-structure-basic/merge-sort/)
    * [Vận dụng cấu trúc binary heap: sắp xếp vun đống (heap sort)](https://labuladong.online/algo/data-structure-basic/heap-sort/)
    * [Nguyên lý sắp xếp mới: sắp xếp đếm (counting sort)](https://labuladong.online/algo/data-structure-basic/counting-sort/)
    * [Sắp xếp thùng (bucket sort)](https://labuladong.online/algo/data-structure-basic/bucket-sort/)
    * [Sắp xếp cơ số (Radix Sort)](https://labuladong.online/algo/data-structure-basic/radix-sort/)

  * [Đang cập nhật](https://labuladong.online/algo/intro/updating/)


* [Chương 0: tổng hợp khung tư duy luyện đề cốt lõi](https://labuladong.online/algo/menu/core/)
  * [Mở đầu chương](https://labuladong.online/algo/intro/core-intro/)
  * [Tư duy khung khi học cấu trúc dữ liệu và thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
  * [Kỹ thuật hai con trỏ xử gọn 7 bài danh sách liên kết](https://labuladong.online/algo/essential-technique/linked-list-skills-summary/)
  * [Kỹ thuật hai con trỏ xử gọn 7 bài mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/)
  * [Template code cốt lõi của thuật toán cửa sổ trượt (Sliding Window)](https://labuladong.online/algo/essential-technique/sliding-window-framework/)
  * [Cương lĩnh cốt lõi của chuỗi thuật toán cây nhị phân](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
  * [Một góc nhìn + hai lối tư duy xử gọn đệ quy](https://labuladong.online/algo/essential-technique/understand-recursion/)
  * [Khung công thức giải đề quy hoạch động (DP)](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)
  * [Khung công thức giải đề thuật toán quay lui (backtracking)](https://labuladong.online/algo/essential-technique/backtrack-framework/)
  * [Khung công thức giải đề thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/)
  * [Quay lui xử gọn mọi bài hoán vị/tổ hợp/tập con](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)
  * [Khung công thức giải đề thuật toán tham lam](https://labuladong.online/algo/essential-technique/greedy/)
  * [Khung công thức giải đề thuật toán chia để trị](https://labuladong.online/algo/essential-technique/divide-and-conquer/)
  * [Hướng dẫn thực dụng phân tích độ phức tạp thời gian và không gian](https://labuladong.online/algo/essential-technique/complexity-analysis/)


* [Chương 1: thuật toán cấu trúc dữ liệu kinh điển](https://labuladong.online/algo/menu/ds/)
  * [Cầm tay luyện thuật toán danh sách liên kết](https://labuladong.online/algo/menu/linked-list/)
    * [Kỹ thuật hai con trỏ xử gọn 7 bài danh sách liên kết](https://labuladong.online/algo/essential-technique/linked-list-skills-summary/)
    * [Bài tập kinh điển hai con trỏ trên danh sách liên kết](https://labuladong.online/algo/problem-set/linkedlist-two-pointers/)
    * [Tổng hợp các cách đảo danh sách liên kết đơn](https://labuladong.online/algo/data-structure/reverse-linked-list-recursion/)
    * [Cách kiểm tra danh sách liên kết đối xứng (palindrome)](https://labuladong.online/algo/data-structure/palindrome-linked-list/)

  * [Cầm tay luyện thuật toán mảng](https://labuladong.online/algo/menu/array/)
    * [Kỹ thuật hai con trỏ xử gọn 7 bài mảng](https://labuladong.online/algo/essential-technique/array-two-pointers-summary/)
    * [[Game] Game match-3](https://labuladong.online/algo/game/match-three/)
    * [Các kỹ thuật duyệt mảng 2 chiều](https://labuladong.online/algo/practice-in-action/2d-array-traversal-summary/)
    * [Bài tập kinh điển hai con trỏ trên mảng](https://labuladong.online/algo/problem-set/array-two-pointers/)
    * [[Game] Game of Life](https://labuladong.online/algo/game/life-game/)
    * [Một phương pháp quét sạch họ bài nSum](https://labuladong.online/algo/practice-in-action/nsum/)
    * [Kỹ thuật nhỏ mà hay: mảng tổng tiền tố (prefix sum)](https://labuladong.online/algo/data-structure/prefix-sum/)
    * [Bài tập kinh điển kỹ thuật tổng tiền tố](https://labuladong.online/algo/problem-set/perfix-sum/)
    * [Kỹ thuật nhỏ mà hay: mảng hiệu (difference array)](https://labuladong.online/algo/data-structure/diff-array/)
    * [Template code cốt lõi của thuật toán cửa sổ trượt (Sliding Window)](https://labuladong.online/algo/essential-technique/sliding-window-framework/)
    * [Bài tập kinh điển thuật toán cửa sổ trượt](https://labuladong.online/algo/problem-set/sliding-window/)
    * [Mở rộng cửa sổ trượt: thuật toán khớp chuỗi Rabin-Karp](https://labuladong.online/algo/practice-in-action/rabinkarp/)
    * [Template code cốt lõi thuật toán tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/)
    * [Cách viết tìm kiếm nhị phân khoảng đóng-trái mở-phải](https://labuladong.online/algo/essential-technique/binary-search-left-open/)
    * [Khung tư duy khi vận dụng tìm kiếm nhị phân thực tế](https://labuladong.online/algo/frequency-interview/binary-search-in-action/)
    * [Bài tập kinh điển thuật toán tìm kiếm nhị phân](https://labuladong.online/algo/problem-set/binary-search/)
    * [Thuật toán chọn ngẫu nhiên có trọng số](https://labuladong.online/algo/frequency-interview/random-pick-with-weight/)
    * [Quyết định thuật toán đằng sau điển tích Điền Kỵ đua ngựa](https://labuladong.online/algo/practice-in-action/advantage-shuffle/)


  * [Thuật toán hàng đợi/ngăn xếp kinh điển](https://labuladong.online/algo/menu/queue-stack/)
    * [Dùng hàng đợi cài đặt ngăn xếp và ngược lại](https://labuladong.online/algo/data-structure/stack-queue/)
    * [Bài tập kinh điển về ngăn xếp](https://labuladong.online/algo/problem-set/stack/)
    * [Tổng hợp bài toán dấu ngoặc](https://labuladong.online/algo/problem-set/parentheses/)
    * [Bài tập kinh điển về hàng đợi](https://labuladong.online/algo/problem-set/queue/)
    * [Template thuật toán ngăn xếp đơn điệu giải 3 bài mẫu](https://labuladong.online/algo/data-structure/monotonic-stack/)
    * [Các biến thể của ngăn xếp đơn điệu và bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-stack/)
    * [Dùng cấu trúc hàng đợi đơn điệu giải bài toán cửa sổ trượt](https://labuladong.online/algo/data-structure/monotonic-queue/)
    * [Cài đặt tổng quát hàng đợi đơn điệu và bài tập kinh điển](https://labuladong.online/algo/problem-set/monotonic-queue/)

  * [Cầm tay luyện thuật toán cây nhị phân](https://labuladong.online/algo/menu/binary-tree/)
    * [Cương lĩnh cốt lõi của chuỗi thuật toán cây nhị phân](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
    * [Bí kíp cây nhị phân (phần tư duy)](https://labuladong.online/algo/data-structure/binary-tree-part1/)
    * [Bí kíp cây nhị phân (phần dựng cây)](https://labuladong.online/algo/data-structure/binary-tree-part2/)
    * [Bí kíp cây nhị phân (phần hậu thứ tự)](https://labuladong.online/algo/data-structure/binary-tree-part3/)
    * [Bí kíp cây nhị phân (phần tuần tự hóa)](https://labuladong.online/algo/data-structure/serialize-and-deserialize-binary-tree/)
    * [Bí kíp cây tìm kiếm nhị phân (phần tính chất)](https://labuladong.online/algo/data-structure/bst-part1/)
    * [Bí kíp cây tìm kiếm nhị phân (phần thao tác cơ bản)](https://labuladong.online/algo/data-structure/bst-part2/)
    * [Bí kíp cây tìm kiếm nhị phân (phần dựng cây)](https://labuladong.online/algo/data-structure/bst-part3/)
    * [Bí kíp cây tìm kiếm nhị phân (phần hậu thứ tự)](https://labuladong.online/algo/data-structure/bst-part4/)

  * [Tổng hợp bài tập thuật toán cây nhị phân](https://labuladong.online/algo/menu/100-bt/)
    * [Mở đầu chương](https://labuladong.online/algo/intro/binary-tree-practice/)
    * [Dùng tư duy "duyệt cây" giải đề I](https://labuladong.online/algo/problem-set/binary-tree-traverse-i/)
    * [Dùng tư duy "duyệt cây" giải đề II](https://labuladong.online/algo/problem-set/binary-tree-traverse-ii/)
    * [Dùng tư duy "duyệt cây" giải đề III](https://labuladong.online/algo/problem-set/binary-tree-traverse-iii/)
    * [Dùng tư duy "phân rã bài toán" giải đề I](https://labuladong.online/algo/problem-set/binary-tree-divide-i/)
    * [Dùng tư duy "phân rã bài toán" giải đề II](https://labuladong.online/algo/problem-set/binary-tree-divide-ii/)
    * [Vận dụng đồng thời hai lối tư duy để giải đề](https://labuladong.online/algo/problem-set/binary-tree-combine-two-view/)
    * [Tận dụng vị trí hậu thứ tự để giải đề I](https://labuladong.online/algo/problem-set/binary-tree-post-order-i/)
    * [Tận dụng vị trí hậu thứ tự để giải đề II](https://labuladong.online/algo/problem-set/binary-tree-post-order-ii/)
    * [Tận dụng vị trí hậu thứ tự để giải đề III](https://labuladong.online/algo/problem-set/binary-tree-post-order-iii/)
    * [Vận dụng duyệt theo tầng để giải đề I](https://labuladong.online/algo/problem-set/binary-tree-level-i/)
    * [Vận dụng duyệt theo tầng để giải đề II](https://labuladong.online/algo/problem-set/binary-tree-level-ii/)
    * [Bài mẫu kinh điển cây tìm kiếm nhị phân I](https://labuladong.online/algo/problem-set/bst1/)
    * [Bài mẫu kinh điển cây tìm kiếm nhị phân II](https://labuladong.online/algo/problem-set/bst2/)

  * [Mở rộng về cây nhị phân](https://labuladong.online/algo/menu/more-bt/)
    * [Mở rộng: khung giải họ bài tổ tiên chung gần nhất](https://labuladong.online/algo/practice-in-action/lowest-common-ancestor-summary/)
    * [Mở rộng: cách đếm số nút của cây nhị phân hoàn chỉnh](https://labuladong.online/algo/data-structure/count-complete-tree-nodes/)
    * [Mở rộng: trải phẳng cây đa phân một cách lazy](https://labuladong.online/algo/data-structure/flatten-nested-list-iterator/)
    * [Mở rộng: giải chi tiết và ứng dụng sắp xếp trộn](https://labuladong.online/algo/practice-in-action/merge-sort/)
    * [Mở rộng: giải chi tiết và ứng dụng sắp xếp nhanh](https://labuladong.online/algo/practice-in-action/quick-sort/)
    * [Mở rộng: dùng ngăn xếp mô phỏng đệ quy để duyệt cây nhị phân bằng lặp](https://labuladong.online/algo/data-structure/iterative-traversal-binary-tree/)

  * [Thiết kế cấu trúc dữ liệu kinh điển](https://labuladong.online/algo/menu/design/)
    * [Thuật toán như xếp Lego: tự tay cài đặt thuật toán LRU](https://labuladong.online/algo/data-structure/lru-cache/)
    * [Thuật toán như xếp Lego: tự tay cài đặt thuật toán LFU](https://labuladong.online/algo/frequency-interview/lfu/)
    * [Xóa/tìm phần tử bất kỳ trong mảng với thời gian hằng số](https://labuladong.online/algo/data-structure/random-set/)
    * [Thêm bài tập về bảng băm](https://labuladong.online/algo/problem-set/hash-table/)
    * [Bài tập kinh điển hàng đợi ưu tiên](https://labuladong.online/algo/problem-set/binary-heap/)
    * [Cài đặt code TreeMap/TreeSet](https://labuladong.online/algo/data-structure-basic/tree-map-implement/)
    * [Cài đặt code cây đoạn cơ bản](https://labuladong.online/algo/data-structure/segment-tree-implement/)
    * [Tối ưu: cài đặt cây đoạn động](https://labuladong.online/algo/data-structure/segment-tree-dynamic/)
    * [Tối ưu: cài đặt cây đoạn cập nhật lazy](https://labuladong.online/algo/data-structure/segment-tree-lazy-update/)
    * [Bài tập kinh điển cây đoạn](https://labuladong.online/algo/problem-set/segment-tree/)
    * [Cài đặt code cây Trie](https://labuladong.online/algo/data-structure/trie-implement/)
    * [Bài tập thuật toán cây Trie](https://labuladong.online/algo/problem-set/trie/)
    * [Thiết kế thuật toán xếp chỗ ngồi phòng thi](https://labuladong.online/algo/frequency-interview/exam-room/)
    * [Thêm bài tập thiết kế kinh điển](https://labuladong.online/algo/problem-set/ds-design/)
    * [Cài đặt thuật toán nén mã Huffman](https://labuladong.online/algo/data-structure/huffman-tree-implementation/)
    * [Nguyên lý và cài đặt thuật toán băm nhất quán](https://labuladong.online/algo/data-structure/consistent-hashing/)
    * [Mở rộng: cách cài đặt một máy tính bỏ túi](https://labuladong.online/algo/data-structure/implement-calculator/)
    * [Mở rộng: thuật toán tìm trung vị bằng hai binary heap](https://labuladong.online/algo/practice-in-action/find-median-from-data-stream/)
    * [Mở rộng: bài khử trùng lặp trong mảng (bản khó)](https://labuladong.online/algo/frequency-interview/remove-duplicate-letters/)


  * [Thuật toán đồ thị kinh điển](https://labuladong.online/algo/menu/graph/)
    * [Thuật toán kiểm tra đồ thị hai phía](https://labuladong.online/algo/data-structure/bipartite-graph/)
    * [Thuật toán Hierholzer tìm đường Euler](https://labuladong.online/algo/data-structure/eulerian-graph-hierholzer/)
    * [Bài tập kinh điển đường Euler](https://labuladong.online/algo/problem-set/eulerian-path/)
    * [Thuật toán phát hiện chu trình](https://labuladong.online/algo/data-structure/cycle-detection/)
    * [Thuật toán sắp xếp topo](https://labuladong.online/algo/data-structure/topological-sort/)
    * [Thuật toán Union-Find (DSU)](https://labuladong.online/algo/data-structure/union-find/)
    * [Bài tập kinh điển DSU](https://labuladong.online/algo/problem-set/union-find/)
    * [Nguyên lý cốt lõi và cài đặt thuật toán Dijkstra](https://labuladong.online/algo/data-structure/dijkstra/)
    * [Mở rộng Dijkstra: bài toán đường ngắn nhất có giới hạn](https://labuladong.online/algo/data-structure/dijkstra-follow-up/)
    * [Bài tập kinh điển thuật toán Dijkstra](https://labuladong.online/algo/problem-set/dijkstra/)
    * [Nguyên lý cốt lõi và cài đặt thuật toán A*](https://labuladong.online/algo/data-structure/a-star/)
    * [Thuật toán cây khung nhỏ nhất Kruskal](https://labuladong.online/algo/data-structure/kruskal/)
    * [Thuật toán cây khung nhỏ nhất Prim](https://labuladong.online/algo/data-structure/prim/)

* [Chương 2: thuật toán tìm kiếm brute-force kinh điển](https://labuladong.online/algo/menu/braute-force-search/)
  * [Thuật toán DFS/quay lui](https://labuladong.online/algo/menu/dfs/)
    * [Khung công thức giải đề thuật toán quay lui (backtracking)](https://labuladong.online/algo/essential-technique/backtrack-framework/)
    * [Thực hành quay lui: Sudoku và bài N hậu](https://labuladong.online/algo/practice-in-action/sudoku-nqueue/)
    * [[Game] Cài đặt tool 'hack' Sudoku](https://labuladong.online/algo/game/sudoku/)
    * [Quay lui xử gọn mọi bài hoán vị/tổ hợp/tập con](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/)
    * [Giải đáp một số thắc mắc về thuật toán quay lui/DFS](https://labuladong.online/algo/essential-technique/backtrack-vs-dfs/)
    * [Một bài xử gọn mọi bài toán đảo](https://labuladong.online/algo/frequency-interview/island-dfs-summary/)
    * [[Game] Dò mìn II](https://labuladong.online/algo/game/minesweeper-ii/)
    * [Mô hình bóng-hộp: hai góc nhìn liệt kê của thuật toán quay lui](https://labuladong.online/algo/practice-in-action/two-views-of-backtrack/)
    * [Thực hành quay lui: sinh dấu ngoặc](https://labuladong.online/algo/practice-in-action/generate-parentheses/)
    * [Thực hành quay lui: phân hoạch tập hợp](https://labuladong.online/algo/practice-in-action/partition-to-k-equal-sum-subsets/)
    * [Bài tập kinh điển thuật toán quay lui I](https://labuladong.online/algo/problem-set/backtrack-i/)
    * [Bài tập kinh điển thuật toán quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
    * [Bài tập kinh điển thuật toán quay lui III](https://labuladong.online/algo/problem-set/backtrack-iii/)

  * [Thuật toán BFS](https://labuladong.online/algo/menu/bfs/)
    * [Khung công thức giải đề thuật toán BFS](https://labuladong.online/algo/essential-technique/bfs-framework/)
    * [[Game] Giải mê cung](https://labuladong.online/algo/game/maze/)
    * [[Game] Game Hoa Dung Đạo](https://labuladong.online/algo/game/huarong-road/)
    * [[Game] Game nối hình (Lianliankan)](https://labuladong.online/algo/game/connect-two/)
    * [Bài tập kinh điển BFS I](https://labuladong.online/algo/problem-set/bfs/)
    * [Bài tập kinh điển BFS II](https://labuladong.online/algo/problem-set/bfs-ii/)

* [Chương 3: thuật toán quy hoạch động kinh điển](https://labuladong.online/algo/menu/dp/)
  * [Kỹ thuật cơ bản của quy hoạch động](https://labuladong.online/algo/menu/dp-basic/)
    * [Khung công thức giải đề quy hoạch động (DP)](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)
    * [Thiết kế DP: dãy con tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/)
    * [Cách xác định base case và giá trị khởi tạo của memo?](https://labuladong.online/algo/dynamic-programming/memo-fundamental/)
    * [Hai góc nhìn liệt kê của quy hoạch động](https://labuladong.online/algo/dynamic-programming/two-views-of-dp/)
    * [Chuyển đổi tư duy giữa quy hoạch động và thuật toán quay lui](https://labuladong.online/algo/dynamic-programming/word-break/)
    * [Nén không gian cho quy hoạch động](https://labuladong.online/algo/dynamic-programming/space-optimization/)
    * [Nguyên lý cấu trúc con tối ưu và hướng duyệt mảng dp](https://labuladong.online/algo/dynamic-programming/faq-summary/)

  * [Họ bài dãy con (subsequence)](https://labuladong.online/algo/menu/subsequence/)
    * [DP kinh điển: khoảng cách chỉnh sửa (edit distance)](https://labuladong.online/algo/dynamic-programming/edit-distance/)
    * [Thiết kế DP: mảng con có tổng lớn nhất](https://labuladong.online/algo/dynamic-programming/maximum-subarray/)
    * [DP kinh điển: dãy con chung dài nhất](https://labuladong.online/algo/dynamic-programming/longest-common-subsequence/)
    * [Template giải họ bài dãy con bằng DP](https://labuladong.online/algo/dynamic-programming/subsequence-problem/)

  * [Họ bài ba lô (knapsack)](https://labuladong.online/algo/menu/knapsack/)
    * [DP kinh điển: bài toán ba lô 0-1](https://labuladong.online/algo/dynamic-programming/knapsack1/)
    * [DP kinh điển: bài toán ba lô tập con](https://labuladong.online/algo/dynamic-programming/knapsack2/)
    * [DP kinh điển: bài toán ba lô đầy đủ](https://labuladong.online/algo/dynamic-programming/knapsack3/)
    * [Biến thể ba lô: tổng mục tiêu (target sum)](https://labuladong.online/algo/dynamic-programming/target-sum/)

  * [Dùng quy hoạch động 'phá đảo' game](https://labuladong.online/algo/menu/dp-game/)
    * [DP: tổng đường đi nhỏ nhất](https://labuladong.online/algo/dynamic-programming/minimum-path-sum/)
    * [Quy hoạch động giúp tôi phá đảo 'Tháp ma thuật'](https://labuladong.online/algo/dynamic-programming/magic-tower/)
    * [Quy hoạch động giúp tôi phá đảo 'Fallout 4'](https://labuladong.online/algo/dynamic-programming/freedom-trail/)
    * [Bí kíp du lịch tiết kiệm: đường ngắn nhất có trọng số](https://labuladong.online/algo/dynamic-programming/cheap-travel/)
    * [Đường ngắn nhất đa nguồn: thuật toán Floyd](https://labuladong.online/algo/data-structure/floyd/)
    * [DP kinh điển: biểu thức chính quy](https://labuladong.online/algo/dynamic-programming/regular-expression-matching/)
    * [DP kinh điển: thả trứng từ tòa nhà cao](https://labuladong.online/algo/dynamic-programming/egg-drop/)
    * [DP kinh điển: chọc bóng bay](https://labuladong.online/algo/dynamic-programming/burst-balloons/)
    * [DP kinh điển: bài toán trò chơi đối kháng](https://labuladong.online/algo/dynamic-programming/game-theory/)
    * [Một phương pháp quét sạch họ bài trộm nhà trên LeetCode](https://labuladong.online/algo/dynamic-programming/house-robber/)
    * [Một phương pháp quét sạch họ bài mua bán cổ phiếu trên LeetCode](https://labuladong.online/algo/dynamic-programming/stock-problem-summary/)

  * [Tuyển tập bài tập quy hoạch động](https://labuladong.online/algo/menu/dp-basic/)
    * [Mẫu bài trộm nhà](https://labuladong.online/algo/problem-set/rob-house/)
    * [Bài tập kinh điển bài toán ba lô](https://labuladong.online/algo/problem-set/knapsack/)
    * [Bài tập kinh điển quy hoạch động I](https://labuladong.online/algo/problem-set/dynamic-programming-i/)
    * [Bài tập kinh điển quy hoạch động II](https://labuladong.online/algo/problem-set/dynamic-programming-ii/)

  * [Họ bài tham lam](https://labuladong.online/algo/menu/greedy/)
    * [Khung công thức giải đề thuật toán tham lam](https://labuladong.online/algo/essential-technique/greedy/)
    * [Thuật toán đổ xăng của 'tài già'](https://labuladong.online/algo/frequency-interview/gas-station-greedy/)
    * [Thuật toán tham lam: bài toán lập lịch khoảng](https://labuladong.online/algo/frequency-interview/interval-scheduling/)
    * [Kỹ thuật sweep line: xếp phòng họp](https://labuladong.online/algo/frequency-interview/scan-line-technique/)
    * [Cắt video mà ra một thuật toán tham lam](https://labuladong.online/algo/frequency-interview/cut-video/)


* [Chương 4: các kỹ thuật thuật toán thường gặp khác](https://labuladong.online/algo/menu/other/)
  * [Kỹ thuật tính toán số học](https://labuladong.online/algo/menu/math/)
    * [Bài thuật toán giải được chỉ bằng một dòng code](https://labuladong.online/algo/frequency-interview/one-line-solutions/)
    * [Các thao tác bit thường dùng](https://labuladong.online/algo/frequency-interview/bitwise-operation/)
    * [Kỹ thuật toán học nhất định phải biết](https://labuladong.online/algo/essential-technique/math-techniques-summary/)
    * [[Game] Tool sinh bản đồ game dò mìn](https://labuladong.online/algo/game/minesweeper/)
    * [Bàn về thuật toán ngẫu nhiên trong game](https://labuladong.online/algo/frequency-interview/random-algorithm/)
    * [Hai bài giai thừa thường gặp khi thi](https://labuladong.online/algo/frequency-interview/factorial-problems/)
    * [Cách tìm số nguyên tố hiệu quả](https://labuladong.online/algo/frequency-interview/print-prime-number/)
    * [Cách tìm đồng thời phần tử thiếu và phần tử trùng](https://labuladong.online/algo/frequency-interview/mismatch-set/)
    * [Vài bài toán xác suất 'ngược trực giác'](https://labuladong.online/algo/frequency-interview/probability-problem/)
    * [Bài tập liên quan kỹ thuật toán học](https://labuladong.online/algo/problem-set/math-tricks/)

  * [Câu hỏi phỏng vấn kinh điển](https://labuladong.online/algo/menu/interview/)
    * [Cách giải hiệu quả bài hứng nước mưa](https://labuladong.online/algo/frequency-interview/trapping-rain-water/)
    * [Một bài xử gọn họ bài số xấu (ugly number)](https://labuladong.online/algo/frequency-interview/ugly-number-summary/)
    * [Một phương pháp giải 3 bài toán khoảng](https://labuladong.online/algo/practice-in-action/interval-problem-summary/)
    * [Ai ngờ chơi Đấu địa chủ cũng ra thuật toán](https://labuladong.online/algo/practice-in-action/split-array-into-consecutive-subsequences/)
    * [Thuật toán sắp xếp bánh kếp (pancake sort)](https://labuladong.online/algo/frequency-interview/pancake-sorting/)
    * [Phép nhân chuỗi số](https://labuladong.online/algo/practice-in-action/multiply-strings/)
    * [Cách kiểm tra hình chữ nhật hoàn hảo](https://labuladong.online/algo/frequency-interview/perfect-rectangle/)

* [Nội dung khác](https://labuladong.online/algo/menu/appendix/)
  * [Kiến thức cơ bản về máy tính](https://labuladong.online/algo/menu/computer-basics/)
    * [Hướng dẫn nhập môn phát triển frontend trong thời đại AI](https://labuladong.online/algo/computer-science/frontend-introduction/)
    * [Nhập môn kỹ thuật mã hóa hiện đại](https://labuladong.online/algo/computer-science/encryption-intro/)
    * [Hiểu sâu session và cookie](https://labuladong.online/algo/other-skills/session-and-cookie/)
    * [Hiểu sâu JSON Web Token (JWT)](https://labuladong.online/algo/computer-science/how-jwt-works/)
    * [Khác biệt và liên hệ giữa xác thực và ủy quyền](https://labuladong.online/algo/computer-science/authentication-vs-authorization/)
    * [Hiểu sâu khung ủy quyền OAuth 2.0](https://labuladong.online/algo/computer-science/oauth2-explained/)
    * [Xác thực OAuth 2.0 và OIDC](https://labuladong.online/algo/computer-science/oidc/)
    * [OAuth 2.0 và PKCE](https://labuladong.online/algo/computer-science/pkce/)
    * [Hiểu sâu đăng nhập một lần (SSO)](https://labuladong.online/algo/computer-science/sso/)
    * [Hiểu sâu chứng chỉ số và CA](https://labuladong.online/algo/computer-science/certificate-and-ca/)
    * [Hiểu sâu đàm phán khóa TLS](https://labuladong.online/algo/computer-science/tls-key-exchange/)
    * [Hiểu sâu xác thực hai chiều mTLS](https://labuladong.online/algo/computer-science/mtls/)
    * [Làm quen hệ thống file Linux](https://labuladong.online/algo/other-skills/linux-file-system/)
    * [Tiến trình, luồng và file descriptor trong Linux là gì](https://labuladong.online/algo/other-skills/linux-process/)
    * [Cái bẫy của toán tử pipe trong Linux](https://labuladong.online/algo/other-skills/linux-pipeline/)
    * [Mẹo dùng Linux shell](https://labuladong.online/algo/other-skills/linux-shell/)
    * [Bàn về hệ lưu trữ: nguyên lý thiết kế cây LSM](https://labuladong.online/algo/other-skills/lsm-tree/)
    * [Đang cập nhật](https://labuladong.online/algo/intro/updating/)

  * [Mẫu thiết kế (design pattern)](https://labuladong.online/algo/menu/design-pattern/)
    * [Mẫu Singleton](https://labuladong.online/algo/design-pattern/singleton/)
    * [Mẫu Factory Method](https://labuladong.online/algo/design-pattern/factory-method/)
    * [Mẫu Abstract Factory](https://labuladong.online/algo/design-pattern/abstract-factory/)
    * [Mẫu Builder](https://labuladong.online/algo/design-pattern/builder/)
    * [Mẫu Prototype](https://labuladong.online/algo/design-pattern/prototype/)
    * [Mẫu Adapter](https://labuladong.online/algo/design-pattern/adapter/)
    * [Mẫu Composite](https://labuladong.online/algo/design-pattern/composite/)
    * [Mẫu Decorator](https://labuladong.online/algo/design-pattern/decorator/)
    * [Mẫu Bridge](https://labuladong.online/algo/design-pattern/bridge/)
    * [Mẫu Observer](https://labuladong.online/algo/design-pattern/observer/)
    * [Mẫu Strategy](https://labuladong.online/algo/design-pattern/strategy/)
    * [Đang cập nhật](https://labuladong.online/algo/intro/updating/)


<!-- table end -->

# Cảm ơn các cao thủ sau đã tham gia dịch thuật (bản gốc tiếng Anh)

Xếp theo thứ tự từ điển của nickname：

[ABCpril](https://github.com/ABCpril), 
[andavid](https://github.com/andavid), 
[bryceustc](https://github.com/bryceustc), 
[build2645](https://github.com/build2645), 
[CarrieOn](https://github.com/CarrieOn), 
[cooker](https://github.com/xiaochuhub), 
[Dong Wang](https://github.com/Coder2Programmer), 
[ExcaliburEX](https://github.com/ExcaliburEX), 
[floatLig](https://github.com/floatLig), 
[ForeverSolar](https://github.com/foreversolar), 
[Fulin Li](https://fulinli.github.io/), 
[Funnyyanne](https://github.com/Funnyyanne), 
[GYHHAHA](https://github.com/GYHHAHA), 
[Hi_archer](https://hiarcher.top/), 
[Iruze](https://github.com/Iruze), 
[Jieyixia](https://github.com/Jieyixia), 
[Justin](https://github.com/Justin-YGG), 
[Kevin](https://github.com/Kevin-free), 
[Lrc123](https://github.com/Lrc123), 
[lriy](https://github.com/lriy), 
[Lyjeeq](https://github.com/Lyjeeq), 
[MasonShu](https://greenwichmt.github.io/), 
[Master-cai](https://github.com/Master-cai), 
[miaoxiaozui2017](https://github.com/miaoxiaozui2017), 
[natsunoyoru97](https://github.com/natsunoyoru97), 
[nettee](https://github.com/nettee), 
[PaperJets](https://github.com/PaperJets), 
[qy-yang](https://github.com/qy-yang), 
[realism0331](https://github.com/realism0331), 
[SCUhzs](https://github.com/brucecat), 
[Seaworth](https://github.com/Seaworth), 
[shazi4399](https://github.com/shazi4399), 
[ShuozheLi](https://github.com/ShuoZheLi/), 
[sinjoywong](https://blog.csdn.net/SinjoyWong), 
[sunqiuming526](https://github.com/sunqiuming526), 
[Tianhao Zhou](https://github.com/tianhaoz95), 
[timmmGZ](https://github.com/timmmGZ), 
[tommytim0515](https://github.com/tommytim0515), 
[ucsk](https://github.com/ucsk), 
[wadegrc](https://github.com/wadegrc), 
[walsvid](https://github.com/walsvid), 
[warmingkkk](https://github.com/warmingkkk), 
[Wonderxie](https://github.com/Wonderxie), 
[wsyzxxxx](https://github.com/wsyzxxxx), 
[xiaodp](https://github.com/xiaodp), 
[youyun](https://github.com/youyun), 
[yx-tan](https://github.com/yx-tan), 
[Zero](https://github.com/Mr2er0), 
[Ziming](https://github.com/ML-ZimingMeng/LeetCode-Python3)

# Donate

Nếu kho này giúp ích cho bạn, có thể mời tác giả một ly cà phê hòa tan

<img src="pictures/pay.jpg" width = "200" align=center />
