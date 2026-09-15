# Cách sắp xếp chỗ ngồi cho thí sinh

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho user cũ~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [855. Exam Room](https://leetcode.com/problems/exam-room/) | [855. Sắp chỗ phòng thi](https://leetcode.cn/problems/exam-room/) | 🟠

**-----------**

Bài này nói về LeetCode bài 855 "Sắp chỗ phòng thi", thú vị và có tính kỹ thuật nhất định. Loại đề này không giống thuật toán kiểu quy hoạch động đua IQ, mà xem sự hiểu biết của bạn về cấu trúc dữ liệu thường dùng và trình độ viết code, cá nhân cho rằng đáng coi trọng và học tập.

Ngoài lề một câu, nhiều độc giả đều hỏi khung thuật toán tổng kết ra thế nào, thực ra khung lại được gọt giũa dần dần từ chi tiết mà ra. Hy vọng mọi người sau khi xem bài viết của chúng tôi, tốt nhất dành thời gian tự tay làm các vấn đề liên quan, cái có được trên giấy rốt cuộc vẫn nông cạn, muốn thấu triệt việc này phải đích thân thực hành mà.

Trước hết mô tả đề bài: giả sử có một phòng thi, phòng thi có một hàng tổng cộng `N` chỗ ngồi, chỉ số lần lượt là `[0..N-1]`, thí sinh sẽ **lần lượt** vào phòng thi, và có thể rời phòng thi vào **bất kỳ lúc nào**.

Bạn làm giám thị, phải sắp xếp chỗ ngồi cho thí sinh, thỏa mãn: **mỗi khi một học sinh vào, bạn cần tối đa hóa khoảng cách giữa cậu ấy và người gần nhất; nếu có nhiều chỗ như vậy, xếp cậu ấy vào chỗ có chỉ số nhỏ nhất**. Điều này rất phù hợp thực tế phải không,

Tức là mời bạn implement một lớp như sau:

<!-- muliti_language -->
```java
class ExamRoom {
    // Hàm khởi tạo, truyền vào tổng số chỗ N
    public ExamRoom(int N);
    // Có một thí sinh đến, trả về chỗ bạn phân cho cậu ấy
    public int seat();
    // Thí sinh ngồi ở vị trí p đã rời đi
    // Có thể cho rằng vị trí p nhất định có thí sinh ngồi
    public void leave(int p);
}
```

Ví như phòng thi có 5 chỗ ngồi, lần lượt là `[0..4]`:

Thí sinh đầu tiên vào (gọi `seat()`), ngồi đâu cũng được, nhưng phải xếp cho cậu ấy vị trí có chỉ số nhỏ nhất, tức trả về vị trí 0.

Học sinh thứ hai vào (lại gọi `seat()`), phải cách người bên cạnh xa nhất, tức trả về vị trí 4.

Học sinh thứ ba vào, phải cách người bên cạnh xa nhất, nên ngồi giữa, tức chỗ 2.

Nếu lại vào một học sinh nữa, cậu ấy có thể ngồi chỗ 1 hoặc 3, lấy chỉ số nhỏ hơn là 1.

Cứ thế suy ra.

Tình huống vừa nói, chưa gọi hàm `leave`, nhưng độc giả chắc chắn có thể phát hiện quy luật:

**Nếu coi mỗi hai thí sinh kề nhau làm hai đầu mút của đoạn thẳng, xếp thí sinh mới chính là tìm đoạn thẳng dài nhất, rồi để thí sinh đó ở giữa chia đoạn thẳng này làm "hai phần", trung điểm chính là chỗ phân cho cậu ấy. `leave(p)` thực chất chính là bỏ đầu mút `p`, khiến hai đoạn thẳng kề nhau gộp thành một**.

Ý tưởng cốt lõi rất đơn giản phải không, nên vấn đề này thực tế chính là kiểm tra sự hiểu biết của bạn về cấu trúc dữ liệu. Với logic trên, bạn dùng cấu trúc dữ liệu nào để implement?

### Một, phân tích ý tưởng

Căn cứ ý tưởng trên, trước hết cần trừu tượng hóa học sinh ngồi trong phòng thành đoạn thẳng, chúng ta có thể đơn giản dùng một mảng cỡ 2 để biểu thị.

Ngoài ra, ý tưởng cần chúng ta tìm đoạn thẳng "dài nhất", còn cần bỏ đoạn thẳng, thêm đoạn thẳng.

**Hễ gặp yêu cầu lấy giá trị lớn nhất trong quá trình động, chắc chắn phải dùng cấu trúc dữ liệu có thứ tự, cấu trúc dữ liệu chúng ta thường dùng chính là heap nhị phân và cây tìm kiếm nhị phân cân bằng**. Hàng đợi ưu tiên implement bằng heap nhị phân lấy giá trị lớn nhất có độ phức tạp thời gian O(logN), nhưng chỉ xóa được giá trị lớn nhất. Cây nhị phân cân bằng cũng có thể lấy giá trị lớn nhất, cũng có thể sửa, xóa giá trị tùy ý, mà độ phức tạp thời gian đều là O(logN).

Tổng hợp lại, heap nhị phân không đáp ứng được thao tác `leave`, nên dùng cây nhị phân cân bằng. Nên ở đây chúng ta sẽ dùng một cấu trúc dữ liệu của Java là `TreeSet`, đây là một cấu trúc dữ liệu có thứ tự, tầng đáy do cây đỏ-đen duy trì tính có thứ tự.

Ở đây tiện nhắc một chút, vừa nói tới tập hợp (Set) hay ánh xạ (Map), có độc giả có thể đương nhiên cho rằng là tập hợp băm (HashSet) hay bảng băm (HashMap), hiểu như vậy là có chút vấn đề.

Vì tập hợp/ánh xạ băm ở tầng đáy implement bằng hàm băm và mảng, đặc tính là duyệt không có thứ tự cố định, nhưng hiệu suất thao tác cao, độ phức tạp thời gian O(1).

Mà tập hợp/ánh xạ còn có thể dựa vào cấu trúc dữ liệu tầng đáy khác, thường gặp chính là cây đỏ-đen (một loại cây tìm kiếm nhị phân cân bằng), đặc tính là tự động duy trì thứ tự phần tử trong đó, hiệu suất thao tác là O(logN). Loại này thường gọi là "tập hợp/ánh xạ có thứ tự".

`TreeSet` chúng ta dùng chính là một tập hợp có thứ tự, mục đích chính là để giữ tính có thứ tự của độ dài đoạn thẳng, tìm nhanh đoạn thẳng lớn nhất, xóa và chèn nhanh.

### Hai, đơn giản hóa vấn đề

Trước hết, nếu có nhiều chỗ để chọn, cần chọn chỗ có chỉ số nhỏ nhất phải không? **Chúng ta trước hết đơn giản hóa vấn đề, tạm thời bỏ qua yêu cầu này**, implement ý tưởng trên.

Vấn đề này còn dùng một kỹ thuật lập trình thường dùng, chính là dùng một "đoạn thẳng ảo" để thuật toán khởi động đúng, điều này và việc thuật toán liên quan linked list cần "node đầu ảo" là một đạo lý.

<!-- muliti_language -->
```java
class ExamRoom {
    // Ánh xạ đầu mút p tới đoạn thẳng lấy p làm đầu trái
    private Map<Integer, int[]> startMap;
    // Ánh xạ đầu mút p tới đoạn thẳng lấy p làm đầu phải
    private Map<Integer, int[]> endMap;
    // Lưu mọi đoạn thẳng theo độ dài từ nhỏ tới lớn
    private TreeSet<int[]> pq;
    private int N;

    public ExamRoom(int N) {
        this.N = N;
        startMap = new HashMap<>();
        endMap = new HashMap<>();
        pq = new TreeSet<>((a, b) -> {
            // Tính độ dài hai đoạn thẳng
            int distA = distance(a);
            int distB = distance(b);
            // Dài hơn thì lớn hơn, xếp sau
            return distA - distB;
        });
        // Trong tập hợp có thứ tự đặt trước một đoạn thẳng ảo
        addInterval(new int[] {-1, N});
    }

    /* Bỏ một đoạn thẳng */
    private void removeInterval(int[] intv) {
        pq.remove(intv);
        startMap.remove(intv[0]);
        endMap.remove(intv[1]);
    }

    /* Thêm một đoạn thẳng */
    private void addInterval(int[] intv) {
        pq.add(intv);
        startMap.put(intv[0], intv);
        endMap.put(intv[1], intv);
    }

    /* Tính độ dài một đoạn thẳng */
    private int distance(int[] intv) {
        return intv[1] - intv[0] - 1;
    }

    // ...
}
```

"Đoạn thẳng ảo" thực chất chính là để biểu thị mọi chỗ ngồi thành một đoạn thẳng:

![](https://labuladong.online/algo/images/座位调度/1.jpg)

Có bước đệm trên, API chính `seat` và `leave` là có thể viết được:

```java
class ExamRoom {
    // ...

    public int seat() {
        // Lấy đoạn thẳng dài nhất từ tập hợp có thứ tự
        int[] longest = pq.last();
        int x = longest[0];
        int y = longest[1];
        int seat;
        if (x == -1) { // Trường hợp một
            seat = 0;
        } else if (y == N) { // Trường hợp hai
            seat = N - 1;
        } else { // Trường hợp ba
            seat = (y - x) / 2 + x;
        }
        // Chia đoạn thẳng dài nhất thành hai đoạn
        int[] left = new int[] {x, seat};
        int[] right = new int[] {seat, y};
        removeInterval(longest);
        addInterval(left);
        addInterval(right);
        return seat;
    }

    public void leave(int p) {
        // Tìm ra đoạn thẳng trái phải của p
        int[] right = startMap.get(p);
        int[] left = endMap.get(p);
        // Gộp hai đoạn thẳng thành một đoạn thẳng
        int[] merged = new int[] {left[0], right[1]};
        removeInterval(left);
        removeInterval(right);
        addInterval(merged);
    }
}
```

![](https://labuladong.online/algo/images/座位调度/2.jpg)

Đến đây, thuật toán coi như đã implement cơ bản, code tuy nhiều, nhưng ý tưởng rất đơn giản: tìm đoạn thẳng dài nhất, tách từ giữa thành hai đoạn, trung điểm chính là giá trị trả về của `seat()`; tìm đoạn thẳng trái phải của `p`, gộp thành một đoạn thẳng, đây chính là logic của `leave(p)`.

### Ba, vấn đề nâng cao

Nhưng đề yêu cầu khi nhiều lựa chọn thì chọn chỗ có chỉ số nhỏ nhất, chúng ta vừa rồi đã bỏ qua vấn đề này. Ví như tình huống dưới đây sẽ sai:

![](https://labuladong.online/algo/images/座位调度/3.jpg)

Giờ trong tập hợp có thứ tự có đoạn thẳng `[0,4]` và `[4,9]`, vậy đoạn thẳng dài nhất `longest` chính là đoạn sau, theo logic của `seat`, sẽ tách `[4,9]`, tức trả về chỗ 6. Nhưng đáp án đúng hẳn là chỗ 2, vì 2 và 6 đều thỏa mãn điều kiện tối đa hóa khoảng cách thí sinh kề nhau, hai bên nên lấy nhỏ hơn.

![](https://labuladong.online/algo/images/座位调度/4.jpg)

**Gặp yêu cầu kiểu này của đề, cách giải quyết chính là sửa cách sắp xếp của cấu trúc dữ liệu có thứ tự**. Cụ thể tới vấn đề này, chính là sửa logic hàm so sánh của `TreeMap`:

```java
pq = new TreeSet<>((a, b) -> {
    int distA = distance(a);
    int distB = distance(b);
    // Nếu độ dài bằng nhau, thì so chỉ số
    if (distA == distB)
        return b[0] - a[0];
    return distA - distB;
});
```

Ngoài ra, còn phải đổi hàm `distance`, **không thể đơn giản để nó tính độ dài giữa hai đầu mút của một đoạn thẳng, mà để nó tính độ dài giữa trung điểm và đầu mút của đoạn thẳng đó**.

<!-- muliti_language -->
```java
class ExamRoom {
    // ...

    private int distance(int[] intv) {
        int x = intv[0];
        int y = intv[1];
        if (x == -1) return y;
        if (y == N) return N - 1 - x;
        // Độ dài giữa trung điểm và đầu mút
        return (y - x) / 2;
    }
}
```

![](https://labuladong.online/algo/images/座位调度/5.jpg)

Như vậy, giá trị `distance` của `[0,4]` và `[4,9]` bằng nhau, thuật toán sẽ so chỉ số của hai bên, lấy đoạn thẳng nhỏ hơn để tách. Tới đây, đề thuật toán này coi như đã giải quyết hoàn toàn.

### Bốn, tổng kết cuối

Vấn đề mà bài này bàn thực ra không tính là khó, tuy trông code khá nhiều. Vấn đề cốt lõi chính là kiểm tra sự hiểu và sử dụng cấu trúc dữ liệu có thứ tự, cùng chải lại một chút.

Xử lý vấn đề động thông thường đều sẽ dùng cấu trúc dữ liệu có thứ tự, ví như cây tìm kiếm nhị phân cân bằng và heap nhị phân, độ phức tạp thời gian của hai bên gần như nhau, nhưng bên trước hỗ trợ nhiều thao tác hơn.

Cây tìm kiếm nhị phân cân bằng đã tốt dùng vậy, còn dùng heap nhị phân làm gì? Vì tầng đáy của heap nhị phân chính là mảng, implement đơn giản, xem chi tiết bài trước [Giải chi tiết heap nhị phân](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/). Bạn thử implement một cây đỏ-đen xem? Thao tác phức tạp, mà không gian tiêu tốn tương đối sẽ nhiều hơn một chút. Vấn đề cụ thể, vẫn phải chọn cấu trúc dữ liệu thích hợp để giải quyết.

Hy vọng bài này có ích với mọi người.





**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》 đã xuất bản, theo dõi kênh WeChat chính thức để xem chi tiết; nhắn tin tới hộp thư "**bộ toàn tập**" có thể tải PDF đi kèm và bộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======

[855. Sắp chỗ phòng thi](https://leetcode-cn.com/problems/exam-room)

### javascript

js không có sẵn implement liên quan treeset, mà implement cũng khá phiền.

mảng array ghi vị trí có người

Các tình huống của seat như sau:

- Nếu không có array, seatNo mặc định là 0
- Khi array có một phần tử, thì xem khoảng cách tới hai phía, chọn phía xa hơn
- Duyệt từng cặp lấy tổng của giá trị giữa và giá trị đầu

```js
class ExamRoom {
    /**
     * @param {number} N
     */
    constructor(N) {
        this.array = [];
        this.seatNo = 0;
        this.number = N - 1;
    }
    /**
     * @return {number}
     */
    seat() {
        this.seatNo = 0;
        if (this.array.length == 1) {
            if (this.array[0] == 0) {
                this.seatNo = this.number;
            } else if (this.array[0] == this.number) {
                this.seatNo = 0;
            } else {
                let distance1 = this.array[0];
                let distance2 = this.number - this.array[0];
                if (distance1 >= distance2) {
                    this.seatNo = 0 + distance1;
                } else {
                    this.seatNo = distance1 + distance2;
                }
            }
        } else if ((this.array.length > 1)) {
            let maxDistance = this.array[0], start;
            for (let i = 0; i < this.array.length - 1; i++) {
                let distance = Math.floor((this.array[i + 1] - this.array[i] >>> 1));
                if (maxDistance < distance) {
                    maxDistance = distance;
                    start = this.array[i]
                    this.seatNo = start + maxDistance;
                }
            }
            if (this.number - this.array[this.array.length - 1] > maxDistance) {
                this.seatNo = this.number;
            }
        }
        this.array.push(this.seatNo);
        this.array.sort((a, b) => { return a - b })
        return this.seatNo;
    }
    /** 
     * @param {number} p
     * @return {void}
     */
    leave(p) {
        let index = this.array.indexOf(p)
        this.array.splice(index, 1)
    };
}
```

