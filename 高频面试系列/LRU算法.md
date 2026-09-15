# Thuật toán như xếp Lego: tự tay làm LRU



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được套路 thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | 力扣 | Độ khó |
| :----: | :----: | :----: |
| [146. LRU Cache](https://leetcode.com/problems/lru-cache/) | [146. Cache LRU](https://leetcode.cn/problems/lru-cache/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
> - [Cơ bản linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
> - [Cơ bản hash table](https://labuladong.online/algo/data-structure-basic/hashmap-basic/)

Thuật toán LRU chính là một chiến lược loại cache, nguyên lý không khó, nhưng khi phỏng vấn viết ra thuật toán không bug比较 có技巧, cần trừu tượng và拆解 cấu trúc dữ liệu层层, bài này就帶 bạn viết một code đẹp.

Cấu trúc dữ liệu mấu chốt thuật toán LRU dùng là hash-linked-list `LinkedHashMap`, chương cơ bản cấu trúc dữ liệu [tự tay实现 hash-linked-list](https://labuladong.online/algo/data-structure-basic/hashtable-with-linked-list/)讲 chuyên nguyên lý và implement code hash-linked-list. Nếu bạn chưa xem cũng không sao, bài này sẽ讲 lại nguyên lý cốt lõi hash-linked-list, để implement thuật toán LRU.

Dung lượng cache của máy有限, nếu cache đầy就要 xóa vài nội dung, nhường chỗ cho nội dung mới. Nhưng vấn đề là, xóa nội dung nào? Ta肯定 mong xóa những cache没什么 dùng, còn giữ dữ liệu hữu dụng ở lại cache, tiện sau tiếp tục dùng. Vậy, dữ liệu thế nào, ta判定 là dữ liệu 「hữu dụng」?

Thuật toán loại cache LRU chính là một chiến lược hay dùng. Tên đầy đủ LRU là Least Recently Used, nghĩa là ta cho rằng dữ liệu dùng gần đây hẳn là 「hữu dụng」, dữ liệu rất lâu không dùng hẳn vô dụng, bộ nhớ đầy就 ưu tiên xóa những dữ liệu rất lâu không dùng đó.

Lấy ví dụ đơn giản, điện thoại Android đều có thể để phần mềm chạy后台, ví dụ tôi lần lượt mở 「cài đặt」「quản gia」「lịch」, vậy giờ chúng在后台 xếp thứ tự thế này:

![](https://labuladong.online/algo/images/lru/1.jpg)

Nhưng lúc này nếu tôi truy cập giao diện 「cài đặt」 một chút, thì 「cài đặt」 sẽ被提前 lên đầu, thành thế này:

![](https://labuladong.online/algo/images/lru/2.jpg)

Giả sử điện thoại tôi chỉ cho đồng thời mở 3 app, giờ đã đầy. Vậy nếu tôi mở mới một app 「đồng hồ」,就必须 đóng một app để nhường chỗ cho 「đồng hồ」, đóng cái nào?

Theo chiến lược LRU,就 đóng 「quản gia」 dưới cùng, vì đó là lâu nhất không dùng, rồi để app mới mở lên trên cùng:

![](https://labuladong.online/algo/images/lru/3.jpg)

Giờ bạn hẳn hiểu chiến lược LRU (Least Recently Used) rồi. Đương nhiên còn chiến lược loại cache khác, ví dụ đừng按时序 truy cập để loại, mà按 tần suất truy cập (chiến lược LFU) để loại v.v.,各 có cảnh ứng dụng. Bài này讲 chiến lược thuật toán LRU, tôi sẽ ở [chi tiết thuật toán LFU](https://labuladong.online/algo/frequency-interview/lfu/)讲 thuật toán LFU.






## Một, mô tả thuật toán LRU

LeetCode 146 「cơ chế cache LRU」 chính là bắt bạn thiết kế cấu trúc dữ liệu:

Trước phải nhận một tham số `capacity` làm dung lượng lớn nhất cache, rồi implement hai API, một là method `put(key, val)`存 cặp key-value, một là method `get(key)` lấy `val` tương ứng `key`, nếu `key` không tồn tại trả về -1.

Chú ý nhé, method `get` và `put`必须 đều độ phức tạp thời gian $O(1)$, ta lấy ví dụ cụ thể xem thuật toán LRU làm việc thế nào.

```java
// 缓存容量为 2 -> Dung lượng cache là 2
LRUCache cache = new LRUCache(2);
// 你可以把 cache 理解成一个队列 -> Bạn có thể hiểu cache là một hàng đợi
// 假设左边是队头，右边是队尾 -> Giả sử trái là đầu hàng, phải là đuôi hàng
// 最近使用的排在队头，久未使用的排在队尾 -> Dùng gần đây xếp đầu hàng, lâu không dùng xếp đuôi hàng
// 圆括号表示键值对 (key, val) -> Ngoặc tròn biểu thị cặp key-value (key, val)

cache.put(1, 1);
// cache = [(1, 1)]

cache.put(2, 2);
// cache = [(2, 2), (1, 1)]

// 返回 1 -> Trả về 1
cache.get(1);
// cache = [(1, 1), (2, 2)]
// 解释：因为最近访问了键 1，所以提前至队头 -> Giải thích: vì mới truy cập key 1, nên đưa lên đầu hàng
// 返回键 1 对应的值 1 -> Trả về giá trị 1 tương ứng key 1

cache.put(3, 3);
// cache = [(3, 3), (1, 1)]
// 解释：缓存容量已满，需要删除内容空出位置 -> Giải thích: dung lượng cache đã đầy, cần xóa nội dung nhường chỗ
// 优先删除久未使用的数据，也就是队尾的数据 -> Ưu tiên xóa dữ liệu lâu không dùng, tức dữ liệu đuôi hàng
// 然后把新的数据插入队头 -> Rồi chèn dữ liệu mới vào đầu hàng

// 返回 -1 (未找到) -> Trả về -1 (không tìm thấy)
cache.get(2);
// cache = [(3, 3), (1, 1)]
// 解释：cache 中不存在键为 2 的数据 -> Giải thích: trong cache không tồn tại dữ liệu key 2

cache.put(1, 4);    
// cache = [(1, 4), (3, 3)]
// 解释：键 1 已存在，把原始值 1 覆盖为 4 -> Giải thích: key 1 đã tồn tại, ghi đè giá trị gốc 1 thành 4
// 不要忘了也要将键值对提前到队头 -> Đừng quên cũng phải đưa cặp key-value lên đầu hàng
```

## Hai, thiết kế thuật toán LRU

Phân tích quá trình thao tác trên, muốn method `put` và `get` độ phức tạp thời gian O(1), ta có thể tổng kết điều kiện cần của cấu trúc dữ liệu `cache` này:

1,显然 phần tử trong `cache`必须 có时序, để phân biệt dùng gần đây và lâu không dùng, khi đầy容量要 xóa phần tử lâu nhất không dùng nhường chỗ.

2, Ta要在 `cache`中 nhanh tìm `key` nào đó có tồn tại không và được `val` tương ứng;

3, Mỗi lần truy cập `key` nào trong `cache`, cần biến phần tử này thành dùng gần đây, nghĩa là `cache`要 hỗ trợ chèn và xóa nhanh phần tử ở vị trí tùy ý.

Vậy, cấu trúc dữ liệu nào đồng thời符合 điều kiện trên? Hash table tìm nhanh, nhưng dữ liệu không có thứ tự cố định; linked list có phân thứ tự, chèn xóa nhanh, nhưng tìm chậm. Nên结合 một chút, thành một cấu trúc dữ liệu mới: hash-linked-list `LinkedHashMap`.






Cấu trúc dữ liệu cốt lõi của thuật toán cache LRU chính là hash-linked-list,結合体 của doubly-linked-list và hash table. Cấu trúc dữ liệu này长这样:

![](https://labuladong.online/algo/images/lru/4.jpg)

靠 cấu trúc này, ta逐一 phân tích 3 điều kiện trên:

1, Nếu ta mỗi lần mặc định thêm phần tử từ đuôi linked list, vậy显然 phần tử càng靠 đuôi chính là dùng gần đây, phần tử càng靠 đầu chính là lâu nhất không dùng.

2, Với một `key` nào đó, ta có thể qua hash table定位 nhanh tới node trong linked list, từ đó lấy `val` tương ứng.

3, Linked list显然 hỗ trợ chèn và xóa nhanh ở vị trí tùy ý, sửa con trỏ là được. Chỉ là linked list truyền thống không thể theo chỉ số truy cập nhanh phần tử vị trí nào, còn ở đây靠 hash table, có thể qua `key` ánh xạ nhanh tới node linked list tùy ý, rồi chèn và xóa.

**Có lẽ bạn hỏi, vì sao phải là doubly-linked-list, singly-linked-list được không? Ngoài ra, đã hash table中存 `key` rồi, vì sao linked list中 còn phải存 `key` và `val`, chỉ存 `val` chẳng phải được sao**?

Lúc nghĩ đều là vấn đề, chỉ lúc làm mới có đáp án. Nguyên nhân thiết kế vậy,必须要 ta亲自 implement thuật toán LRU xong mới hiểu, nên ta bắt đầu xem code吧～

## Ba, implement code

Nhiều ngôn ngữ có hàm thư viện内置 hash-linked-list hay chức năng LRU tương tự, nhưng để giúp mọi người hiểu chi tiết thuật toán, ta trước tự造轮子 implement một lần thuật toán LRU, rồi dùng `LinkedHashMap`内置 của Java implement một lần nữa.

Trước, ta viết ra class node của [doubly-linked-list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/), để đơn giản, `key` và `val` đều coi là int:

```java
class Node {
    public int key, val;
    public Node next, prev;
    public Node(int k, int v) {
        this.key = k;
        this.val = v;
    }
}
```

Rồi靠 kiểu `Node` của ta dựng một doubly-linked-list, implement vài API thuật toán LRU必须:

```java
class DoubleList {  
    // 头尾虚节点 -> Node ảo đầu đuôi
    private Node head, tail;  
    // 链表元素数 -> Số phần tử linked list
    private int size;
    
    public DoubleList() {
        // 初始化双向链表的数据 -> Khởi tạo dữ liệu doubly-linked-list
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
        size = 0;
    }

    // 在链表尾部添加节点 x，时间 O(1) -> Thêm node x ở đuôi linked list, thời gian O(1)
    public void addLast(Node x) {
        x.prev = tail.prev;
        x.next = tail;
        tail.prev.next = x;
        tail.prev = x;
        size++;
    }

    // 删除链表中的 x 节点（x 一定存在） -> Xóa node x trong linked list (x chắc tồn tại)
    // 由于是双链表且给的是目标 Node 节点，时间 O(1) -> Vì là doubly-linked-list mà cho node Node mục tiêu, thời gian O(1)
    public void remove(Node x) {
        x.prev.next = x.next;
        x.next.prev = x.prev;
        size--;
    }
    
    // 删除链表中第一个节点，并返回该节点，时间 O(1) -> Xóa node đầu trong linked list, và trả về node đó, thời gian O(1)
    public Node removeFirst() {
        if (head.next == tail)
            return null;
        Node first = head.next;
        remove(first);
        return first;
    }

    // 返回链表长度，时间 O(1) -> Trả về độ dài linked list, thời gian O(1)
    public int size() { return size; }

}
```

Nếu thao tác linked list không quen, xem bài trước [tự tay实现 doubly-linked-list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/).

Tới đây là trả lời được câu hỏi vừa rồi 「vì sao必须要 dùng doubly-linked-list」 rồi, vì ta cần thao tác xóa. Xóa một node không chỉ要 được con trỏ bản thân node đó, cũng cần thao tác con trỏ node tiền驱 nó, còn doubly-linked-list mới hỗ trợ tìm thẳng tiền驱, đảm bảo độ phức tạp thời gian O(1).

> [!IMPORTANT]
> Chú ý doubly-linked-list ta implement API chỉ能 chèn từ đuôi, nghĩa là dữ liệu靠 đuôi là dùng gần đây, dữ liệu靠 đầu là lâu nhất không dùng.

Có implement doubly-linked-list, ta chỉ cần ở thuật toán LRU結合 nó với hash table là được, trước dựng khung code:

```java
class LRUCache {
    // key -> Node(key, val)
    private HashMap<Integer, Node> map;
    // Node(k1, v1) <-> Node(k2, v2)...
    private DoubleList cache;
    // 最大容量 -> Dung lượng lớn nhất
    private int cap;
    
    public LRUCache(int capacity) {
        this.cap = capacity;
        map = new HashMap<>();
        cache = new DoubleList();
    }
}
```

Trước đừng慌 implement method `get` và `put` của thuật toán LRU. Vì ta要 đồng thời维护 một doubly-linked-list `cache` và một hash table `map`, rất dễ漏掉 vài thao tác, ví dụ xóa `key` nào, ở `cache`中 xóa `Node` tương ứng, nhưng却 quên ở `map`中 xóa `key`.

**Cách hiệu quả giải vấn đề này là: cung cấp một lớp API trừu tượng trên hai cấu trúc dữ liệu này**.

Chính là cố để method chính `get` và `put` của LRU tránh trực tiếp thao tác chi tiết `map` và `cache`. Ta có thể implement trước mấy hàm sau:

```java
class LRUCache {
    // 为了节约篇幅，省略上文给出的代码部分... -> Để tiết kiệm篇幅, lược phần code trên cho...

    // 将某个 key 提升为最近使用的 -> Đưa key nào đó thành dùng gần đây
    private void makeRecently(int key) {
        Node x = map.get(key);
        // 先从链表中删除这个节点 -> Trước xóa node này khỏi linked list
        cache.remove(x);
        // 重新插到队尾 -> Chèn lại vào đuôi hàng
        cache.addLast(x);
    }

    // 添加最近使用的元素 -> Thêm phần tử dùng gần đây
    private void addRecently(int key, int val) {
        Node x = new Node(key, val);
        // 链表尾部就是最近使用的元素 -> Đuôi linked list chính là phần tử dùng gần đây
        cache.addLast(x);
        // 别忘了在 map 中添加 key 的映射 -> Đừng quên thêm ánh xạ key trong map
        map.put(key, x);
    }

    // 删除某一个 key -> Xóa một key nào đó
    private void deleteKey(int key) {
        Node x = map.get(key);
        // 从链表中删除 -> Xóa khỏi linked list
        cache.remove(x);
        // 从 map 中删除 -> Xóa khỏi map
        map.remove(key);
    }

    // 删除最久未使用的元素 -> Xóa phần tử lâu nhất không dùng
    private void removeLeastRecently() {
        // 链表头部的第一个元素就是最久未使用的 -> Phần tử đầu ở đầu linked list chính là lâu nhất không dùng
        Node deletedNode = cache.removeFirst();
        // 同时别忘了从 map 中删除它的 key -> Đồng thời đừng quên xóa key nó khỏi map
        int deletedKey = deletedNode.key;
        map.remove(deletedKey);
    }
}
```

Ở đây là trả lời được câu hỏi trước 「vì sao要在 linked list đồng thời lưu key và val, chứ không chỉ lưu val」, chú ý hàm `removeLeastRecently`, ta cần dùng `deletedNode` được `deletedKey`.

Nghĩa là, khi容量 cache đã đầy, ta không chỉ要 xóa một `Node` cuối, còn phải把 `key` ánh xạ tới node đó trong `map` đồng thời xóa, mà `key` này chỉ能由 `Node` được. Nếu struct `Node` chỉ lưu `val`, vậy ta就 không biết `key` là gì,就 không thể xóa key trong `map`, gây lỗi.

Method trên chính là封装 thao tác đơn giản, gọi các hàm này có thể tránh trực tiếp thao tác linked list `cache` và hash table `map`, dưới đây tôi implement trước method `get` của thuật toán LRU:

```java
class LRUCache {
    // 为了节约篇幅，省略上文给出的代码部分... -> Để tiết kiệm篇幅, lược phần code trên cho...

    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }
        // 将该数据提升为最近使用的 -> Đưa dữ liệu đó thành dùng gần đây
        makeRecently(key);
        return map.get(key).val;
    }
}
```

Method `put` hơi phức tạp, ta trước vẽ图搞 rõ logic nó:

![](https://labuladong.online/algo/images/lru/put.jpg)

Như vậy ta có thể轻松 viết ra code method `put`:

```java
class LRUCache {
    // 为了节约篇幅，省略上文给出的代码部分... -> Để tiết kiệm篇幅, lược phần code trên cho...
    
    public void put(int key, int val) {
        if (map.containsKey(key)) {
            // 删除旧的数据 -> Xóa dữ liệu cũ
            deleteKey(key);
            // 新插入的数据为最近使用的数据 -> Dữ liệu mới chèn là dữ liệu dùng gần đây
            addRecently(key, val);
            return;
        }
        
        if (cap == cache.size()) {
            // 删除最久未使用的元素 -> Xóa phần tử lâu nhất không dùng
            removeLeastRecently();
        }
        // 添加为最近使用的元素 -> Thêm thành phần tử dùng gần đây
        addRecently(key, val);
    }
}
```

Tới đây, bạn hẳn đã完全 nắm nguyên lý và implement thuật toán LRU. Xem implement đầy đủ: (giữ nguyên code đầy đủ như bản gốc, chỉ dịch comment như trên)

Bạn cũng có thể dùng kiểu内置 `LinkedHashMap` của Java hay `MyLinkedHashMap` implement ở [tự tay实现 hash-linked-list](https://labuladong.online/algo/data-structure-basic/hashtable-with-linked-list/) để implement thuật toán LRU, logic và trước完全一致. (code giữ nguyên, comment đã dịch ở trên)

Tới đây, thuật toán LRU就 không còn gì神秘. Thêm bài liên quan thiết kế cấu trúc dữ liệu xem [bài tập kinh điển thiết kế cấu trúc dữ liệu](https://labuladong.online/algo/problem-set/ds-design/).



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Hiểu session và cookie trong một bài](https://labuladong.online/algo/fname.html?fname=session和cookie)
 - [Thuật toán như xếp Lego: tự tay làm LFU](https://labuladong.online/algo/frequency-interview/lfu/)
 - [套路「lấy điểm」笔试 thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)

</details><hr>



<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin刷题 Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng思路 giải:</strong>

| LeetCode | 力扣 | Độ khó |
| :----: | :----: | :----: |
| - | [剑指 Offer II 031. Cache dùng ít gần đây nhất](https://leetcode.cn/problems/OrIXps/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
