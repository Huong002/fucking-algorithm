# Thuật toán như xếp Lego: tự tay làm LRU



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [146. LRU Cache](https://leetcode.com/problems/lru-cache/) | [146. Cache LRU](https://leetcode.cn/problems/lru-cache/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
> - [Cơ bản linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
> - [Cơ bản hash table](https://labuladong.online/algo/data-structure-basic/hashmap-basic/)

Thuật toán LRU chính là một chiến lược loại bỏ cache, nguyên lý không khó, nhưng khi phỏng vấn viết ra thuật toán không bug thì tương đối cần kỹ thuật, cần trừu tượng và tách nhỏ cấu trúc dữ liệu từng lớp, bài này sẽ đưa bạn viết một code đẹp.

Cấu trúc dữ liệu mấu chốt thuật toán LRU dùng là hash-linked-list `LinkedHashMap`, chương cơ bản cấu trúc dữ liệu [tự tay cài đặt hash-linked-list](https://labuladong.online/algo/data-structure-basic/hashtable-with-linked-list/) giảng chuyên về nguyên lý và implement code hash-linked-list. Nếu bạn chưa xem cũng không sao, bài này sẽ giảng lại nguyên lý cốt lõi của hash-linked-list để implement thuật toán LRU.

Dung lượng bộ nhớ đệm của máy có hạn, nếu bộ nhớ đệm đầy sẽ phải xóa vài nội dung, nhường chỗ cho nội dung mới. Nhưng vấn đề là, xóa nội dung nào? Ta chắc chắn mong xóa những cache chẳng có mấy tác dụng, còn giữ dữ liệu hữu dụng ở lại cache, tiện sau này tiếp tục dùng. Vậy, dữ liệu thế nào thì ta xem là dữ liệu "hữu dụng"?

Thuật toán loại cache LRU chính là một chiến lược hay dùng. Tên đầy đủ LRU là Least Recently Used, nghĩa là ta cho rằng dữ liệu dùng gần đây hẳn là 「hữu dụng」, dữ liệu rất lâu không dùng hẳn vô dụng, bộ nhớ đầy thì ưu tiên xóa những dữ liệu rất lâu không dùng đó.

Lấy ví dụ đơn giản, điện thoại Android đều có thể để phần mềm chạy nền, ví dụ tôi lần lượt mở "cài đặt", "quản gia", "lịch", vậy giờ chúng ở nền xếp thứ tự thế này:

![](https://labuladong.online/algo/images/lru/1.jpg)

Nhưng lúc này nếu tôi truy cập giao diện "cài đặt" một chút, thì "cài đặt" sẽ được đưa lên đầu, thành thế này:

![](https://labuladong.online/algo/images/lru/2.jpg)

Giả sử điện thoại tôi chỉ cho đồng thời mở 3 app, giờ đã đầy. Vậy nếu tôi mở mới một app "đồng hồ", thì bắt buộc phải đóng một app để nhường chỗ cho "đồng hồ", đóng cái nào?

Theo chiến lược LRU, thì đóng "quản gia" dưới cùng, vì đó là lâu nhất không dùng, rồi để app mới mở lên trên cùng:

![](https://labuladong.online/algo/images/lru/3.jpg)

Giờ bạn hẳn hiểu chiến lược LRU (Least Recently Used) rồi. Đương nhiên còn chiến lược loại bỏ cache khác, ví dụ đừng theo thứ tự thời gian truy cập để loại, mà theo tần suất truy cập (chiến lược LFU) để loại v.v., mỗi loại có cảnh ứng dụng. Bài này giảng chiến lược thuật toán LRU, tôi sẽ ở [chi tiết thuật toán LFU](https://labuladong.online/algo/frequency-interview/lfu/) giảng thuật toán LFU.






## Một, mô tả thuật toán LRU

LeetCode 146 「cơ chế cache LRU」 chính là bắt bạn thiết kế cấu trúc dữ liệu:

Trước phải nhận một tham số `capacity` làm dung lượng lớn nhất cache, rồi implement hai API, một là method `put(key, val)` lưu cặp key-value, một là method `get(key)` lấy `val` tương ứng `key`, nếu `key` không tồn tại trả về -1.

Chú ý nhé, method `get` và `put` bắt buộc đều có độ phức tạp thời gian $O(1)$, ta lấy ví dụ cụ thể xem thuật toán LRU làm việc thế nào.

```java
// Dung lượng cache là 2
LRUCache cache = new LRUCache(2);
// Bạn có thể hiểu cache là một hàng đợi
// Giả sử trái là đầu hàng, phải là đuôi hàng
// Dùng gần đây xếp đầu hàng, lâu không dùng xếp đuôi hàng
// Ngoặc tròn biểu thị cặp key-value (key, val)

cache.put(1, 1);
// cache = [(1, 1)]

cache.put(2, 2);
// cache = [(2, 2), (1, 1)]

// Trả về 1
cache.get(1);
// cache = [(1, 1), (2, 2)]
// Giải thích: vì mới truy cập key 1, nên đưa lên đầu hàng
// Trả về giá trị 1 tương ứng key 1

cache.put(3, 3);
// cache = [(3, 3), (1, 1)]
// Giải thích: dung lượng cache đã đầy, cần xóa nội dung nhường chỗ
// Ưu tiên xóa dữ liệu lâu không dùng, tức dữ liệu đuôi hàng
// Rồi chèn dữ liệu mới vào đầu hàng

// Trả về -1 (không tìm thấy)
cache.get(2);
// cache = [(3, 3), (1, 1)]
// Giải thích: trong cache không tồn tại dữ liệu key 2

cache.put(1, 4);    
// cache = [(1, 4), (3, 3)]
// Giải thích: key 1 đã tồn tại, ghi đè giá trị gốc 1 thành 4
// Đừng quên cũng phải đưa cặp key-value lên đầu hàng
```

## Hai, thiết kế thuật toán LRU

Phân tích quá trình thao tác trên, muốn method `put` và `get` độ phức tạp thời gian O(1), ta có thể tổng kết điều kiện cần của cấu trúc dữ liệu `cache` này:

1, Rõ ràng phần tử trong `cache` bắt buộc phải có thứ tự thời gian, để phân biệt dùng gần đây và lâu không dùng, khi đầy dung lượng phải xóa phần tử lâu nhất không dùng để nhường chỗ.

2, Ta phải ở trong `cache` nhanh chóng tìm xem `key` nào đó có tồn tại không và lấy được `val` tương ứng;

3, Mỗi lần truy cập `key` nào trong `cache`, cần biến phần tử này thành dùng gần đây, nghĩa là `cache` phải hỗ trợ chèn và xóa nhanh phần tử ở vị trí tùy ý.

Vậy, cấu trúc dữ liệu nào đồng thời phù hợp điều kiện trên? Hash table tìm nhanh, nhưng dữ liệu không có thứ tự cố định; linked list có phân thứ tự, chèn xóa nhanh, nhưng tìm chậm. Nên kết hợp một chút, thành một cấu trúc dữ liệu mới: hash-linked-list `LinkedHashMap`.






Cấu trúc dữ liệu cốt lõi của thuật toán cache LRU chính là hash-linked-list, là sự kết hợp của doubly-linked-list và hash table. Cấu trúc dữ liệu này trông thế này:

![](https://labuladong.online/algo/images/lru/4.jpg)

Dựa vào cấu trúc này, ta phân tích từng điều một 3 điều kiện trên:

1, Nếu ta mỗi lần mặc định thêm phần tử từ đuôi linked list, vậy rõ ràng phần tử càng gần đuôi chính là dùng gần đây, phần tử càng gần đầu chính là lâu nhất không dùng.

2, Với một `key` nào đó, ta có thể qua hash table định vị nhanh tới node trong linked list, từ đó lấy `val` tương ứng.

3, Linked list rõ ràng hỗ trợ chèn và xóa nhanh ở vị trí tùy ý, sửa con trỏ là được. Chỉ là linked list truyền thống không thể theo chỉ số truy cập nhanh phần tử ở vị trí nào đó, còn ở đây nhờ hash table, có thể qua `key` ánh xạ nhanh tới node linked list tùy ý, rồi chèn và xóa.

**Có lẽ bạn hỏi, vì sao phải là doubly-linked-list, singly-linked-list được không? Ngoài ra, đã lưu `key` trong hash table rồi, vì sao trong linked list còn phải lưu `key` và `val`, chỉ lưu `val` chẳng phải được sao**?

Lúc nghĩ đều là vấn đề, chỉ lúc làm mới có đáp án. Nguyên nhân thiết kế như vậy, bắt buộc phải tự tay implement xong thuật toán LRU mới hiểu, nên chúng ta bắt đầu xem code nhé.

## Ba, implement code

Nhiều ngôn ngữ có sẵn hàm thư viện hash-linked-list hay chức năng LRU tương tự, nhưng để giúp mọi người hiểu chi tiết thuật toán, ta trước hết tự làm lại từ đầu implement một lần thuật toán LRU, rồi dùng `LinkedHashMap` có sẵn của Java implement một lần nữa.

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

Rồi dựa vào kiểu `Node` của ta dựng một doubly-linked-list, implement vài API mà thuật toán LRU bắt buộc phải có:

```java
class DoubleList {  
    // Node ảo đầu đuôi
    private Node head, tail;  
    // Số phần tử linked list
    private int size;
    
    public DoubleList() {
        // Khởi tạo dữ liệu doubly-linked-list
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
        size = 0;
    }

    // Thêm node x ở đuôi linked list, thời gian O(1)
    public void addLast(Node x) {
        x.prev = tail.prev;
        x.next = tail;
        tail.prev.next = x;
        tail.prev = x;
        size++;
    }

    // Xóa node x trong linked list (x chắc tồn tại)
    // Vì là doubly-linked-list mà cho node Node mục tiêu, thời gian O(1)
    public void remove(Node x) {
        x.prev.next = x.next;
        x.next.prev = x.prev;
        size--;
    }
    
    // Xóa node đầu trong linked list, và trả về node đó, thời gian O(1)
    public Node removeFirst() {
        if (head.next == tail)
            return null;
        Node first = head.next;
        remove(first);
        return first;
    }

    // Trả về độ dài linked list, thời gian O(1)
    public int size() { return size; }

}
```

Nếu thao tác linked list không quen, xem bài trước [tự tay cài đặt doubly-linked-list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/).

Tới đây là trả lời được câu hỏi vừa rồi "vì sao bắt buộc phải dùng doubly-linked-list" rồi, vì ta cần thao tác xóa. Xóa một node không chỉ cần có con trỏ bản thân node đó, cũng cần thao tác con trỏ node tiền thân của nó, mà chỉ doubly-linked-list mới hỗ trợ tìm thẳng tiền thân, đảm bảo độ phức tạp thời gian O(1).

> [!IMPORTANT]
> Chú ý doubly-linked-list ta implement thì API chỉ chèn được từ đuôi, nghĩa là dữ liệu gần đuôi là dùng gần đây, dữ liệu gần đầu là lâu nhất không dùng.

Có implement của doubly-linked-list, ta chỉ cần ở thuật toán LRU kết hợp nó với hash table là được, trước hết dựng khung code:

```java
class LRUCache {
    // key -> Node(key, val)
    private HashMap<Integer, Node> map;
    // Node(k1, v1) <-> Node(k2, v2)...
    private DoubleList cache;
    // Dung lượng lớn nhất
    private int cap;
    
    public LRUCache(int capacity) {
        this.cap = capacity;
        map = new HashMap<>();
        cache = new DoubleList();
    }
}
```

Trước hết đừng hoảng khi implement method `get` và `put` của thuật toán LRU. Vì ta phải đồng thời duy trì một doubly-linked-list `cache` và một hash table `map`, rất dễ sót vài thao tác, ví dụ xóa `key` nào đó, ở trong `cache` thì xóa `Node` tương ứng, nhưng lại quên xóa `key` ở trong `map`.

**Cách hiệu quả giải vấn đề này là: cung cấp một lớp API trừu tượng trên hai cấu trúc dữ liệu này**.

Chính là cố để method chính `get` và `put` của LRU tránh trực tiếp thao tác chi tiết `map` và `cache`. Ta có thể implement trước mấy hàm sau:

```java
class LRUCache {
    // Để tiết kiệm độ dài, lược phần code trên cho...

    // Đưa key nào đó thành dùng gần đây
    private void makeRecently(int key) {
        Node x = map.get(key);
        // Trước xóa node này khỏi linked list
        cache.remove(x);
        // Chèn lại vào đuôi hàng
        cache.addLast(x);
    }

    // Thêm phần tử dùng gần đây
    private void addRecently(int key, int val) {
        Node x = new Node(key, val);
        // Đuôi linked list chính là phần tử dùng gần đây
        cache.addLast(x);
        // Đừng quên thêm ánh xạ key trong map
        map.put(key, x);
    }

    // Xóa một key nào đó
    private void deleteKey(int key) {
        Node x = map.get(key);
        // Xóa khỏi linked list
        cache.remove(x);
        // Xóa khỏi map
        map.remove(key);
    }

    // Xóa phần tử lâu nhất không dùng
    private void removeLeastRecently() {
        // Phần tử đầu ở đầu linked list chính là lâu nhất không dùng
        Node deletedNode = cache.removeFirst();
        // Đồng thời đừng quên xóa key nó khỏi map
        int deletedKey = deletedNode.key;
        map.remove(deletedKey);
    }
}
```

Ở đây là trả lời được câu hỏi trước "vì sao phải ở trong linked list đồng thời lưu key và val, chứ không chỉ lưu val", chú ý hàm `removeLeastRecently`, ta cần dùng `deletedNode` để lấy `deletedKey`.

Nghĩa là, khi dung lượng cache đã đầy, ta không chỉ phải xóa một `Node` cuối, còn phải đem `key` ánh xạ tới node đó trong `map` đồng thời xóa, mà `key` này chỉ có thể lấy từ `Node`. Nếu struct `Node` chỉ lưu `val`, vậy ta sẽ không biết `key` là gì, sẽ không thể xóa key trong `map`, gây lỗi.

Các method trên chính là đóng gói các thao tác đơn giản, gọi các hàm này có thể tránh trực tiếp thao tác linked list `cache` và hash table `map`, dưới đây tôi implement trước method `get` của thuật toán LRU:

```java
class LRUCache {
    // Để tiết kiệm độ dài, lược phần code trên cho...

    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }
        // Đưa dữ liệu đó thành dùng gần đây
        makeRecently(key);
        return map.get(key).val;
    }
}
```

Method `put` hơi phức tạp, ta trước hết vẽ hình làm rõ logic của nó:

![](https://labuladong.online/algo/images/lru/put.jpg)

Như vậy ta có thể nhẹ nhàng viết ra code của method `put`:

```java
class LRUCache {
    // Để tiết kiệm độ dài, lược phần code trên cho...
    
    public void put(int key, int val) {
        if (map.containsKey(key)) {
            // Xóa dữ liệu cũ
            deleteKey(key);
            // Dữ liệu mới chèn là dữ liệu dùng gần đây
            addRecently(key, val);
            return;
        }
        
        if (cap == cache.size()) {
            // Xóa phần tử lâu nhất không dùng
            removeLeastRecently();
        }
        // Thêm thành phần tử dùng gần đây
        addRecently(key, val);
    }
}
```

Tới đây, bạn hẳn đã hoàn toàn nắm nguyên lý và cách implement thuật toán LRU. Xem implement đầy đủ: (giữ nguyên code đầy đủ như bản gốc, chỉ dịch comment như trên)

Bạn cũng có thể dùng kiểu có sẵn `LinkedHashMap` của Java hay `MyLinkedHashMap` đã implement ở [tự tay cài đặt hash-linked-list](https://labuladong.online/algo/data-structure-basic/hashtable-with-linked-list/) để implement thuật toán LRU, logic và trước đây hoàn toàn nhất quán. (code giữ nguyên, comment đã dịch ở trên)

Tới đây, thuật toán LRU đã không còn gì huyền bí. Xem thêm các bài liên quan thiết kế cấu trúc dữ liệu ở [bài tập kinh điển về thiết kế cấu trúc dữ liệu](https://labuladong.online/algo/problem-set/ds-design/).



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Hiểu session và cookie trong một bài](https://labuladong.online/algo/fname.html?fname=session和cookie)
 - [Thuật toán như xếp Lego: tự tay làm LFU](https://labuladong.online/algo/frequency-interview/lfu/)
 - [Công thức "lấy điểm" thi viết thuật toán](https://labuladong.online/algo/other-skills/tips-in-exam/)

</details><hr>



<hr>
<details class="hint-container details">
<summary><strong>Bài tập trích dẫn bài này</strong></summary>

<strong>Cài [plugin luyện đề Chrome của mình](https://labuladong.online/algo/intro/chrome/) mở các bài sau để xem thẳng ý tưởng giải:</strong>

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| - | [Kiếm chỉ Offer II 031. Cache dùng ít gần đây nhất](https://leetcode.cn/problems/OrIXps/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
