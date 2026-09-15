# Thuật toán xáo trộn Fisher-Yates (Shuffle)

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên website bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn cho user cũ~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

Đọc xong bài này, bạn không chỉ học đượccông thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [384. Shuffle an Array](https://leetcode.com/problems/shuffle-an-array/) | [384. Xáo trộn mảng](https://leetcode.cn/problems/shuffle-an-array/) | 🟠

**-----------**

Tôi biết mọi người sẽ đủ loại thuật toán sắp xếp hoa hòe, nhưng nếu bảo bạn xáo trộn một mảng, liệu bạn có làm tới mức nắm chắc trong lòng? Dù bạn nghĩ bừa nghĩ ra một thuật toán, làm sao chứng minh thuật toán của bạn chính là đúng? Thuật toán xáo trộn không giống thuật toán sắp xếp, kết quả duy nhất có thể rất dễ kiểm nghiệm, vì "loạn" có thể có rất nhiều loại, bạn làm sao chứng minh thuật toán của bạn là "thật sự loạn"?

Nên chúng ta đối mặt hai vấn đề:

1. Thế nào gọi là "thật sự loạn"?

2. Thiết kế thuật toán thế nào để xáo trộn mảng mới đạt tới "thật sự loạn"?

Loại thuật toán này gọi là "thuật toán đặt ngẫu nhiên" hay "thuật toán xáo trộn Fisher-Yates".

Bài này chia hai phần, phần một giải chi tiết thuật toán xáo trộn hay dùng nhất. Vì chi tiết của thuật toán này dễ sai, mà tồn tại vài loại biến thể, tuy có khác biệt tinh tế nhưng đều đúng, nên bài này cần giới thiệu một tư tưởng tổng quát đơn giản đảm bảo bạn viết ra thuật toán xáo trộn đúng. Phần hai giảng dùng "phương pháp Monte Carlo" để kiểm nghiệm kết quả xáo trộn của chúng ta có phải thật sự loạn không. Tư tưởng phương pháp Monte Carlo không khó, nhưng cách cài đặt cũng mỗi có đặc điểm.

### Một, thuật toán xáo trộn

Loại thuật toán này đều dựa vào chọn ngẫu nhiên phần tử rồi hoán đổi để lấy tính ngẫu nhiên, xem trực tiếp code (mã giả), thuật toán này có 4 dạng, đều đúng:

<!-- muliti_language -->
```java
// Thu được một số nguyên ngẫu nhiên trong đoạn đóng [min, max]
int randInt(int min, int max);

// Cách viết thứ nhất
void shuffle(int[] arr) {
    int n = arr.length();
    /******** Khác biệt chỉ hai dòng này ********/
    for (int i = 0 ; i < n; i++) {
        // Chọn ngẫu nhiên một phần tử từ i đến cuối
        int rand = randInt(i, n - 1);
        /*************************/
        swap(arr[i], arr[rand]);
    }
}

// Cách viết thứ hai
    for (int i = 0 ; i < n - 1; i++)
        int rand = randInt(i, n - 1);

// Cách viết thứ ba
    for (int i = n - 1 ; i >= 0; i--)
        int rand = randInt(0, i);

// Cách viết thứ tư
    for (int i = n - 1 ; i > 0; i--)
        int rand = randInt(0, i);

```

**Tiêu chuẩn phân tích tính đúng đắn của thuật toán xáo trộn: kết quả sinh ra bắt buộc có n! khả năng, nếu không chính là sai**. Chuyện này giải thích rất dễ, vì một mảng độ dài n thì hoán vị đầy đủ có n! loại, tức là nói kết quả xáo trộn tổng cộng có n! loại. Thuật toán bắt buộc có thể phản ánh sự thật này, mới là đúng.

Chúng ta trước hết dùng tiêu chuẩn này phân tích tính đúng đắn của **cách viết thứ nhất**:

<!-- muliti_language -->
```java
// Giả sử truyền vào một arr như vậy
int[] arr = {1,3,5,7,9};

void shuffle(int[] arr) {
    int n = arr.length(); // 5
    for (int i = 0 ; i < n; i++) {
        int rand = randInt(i, n - 1);
        swap(arr[i], arr[rand]);
    }
}
```

Vòng for lượt lặp đầu tiên, `i = 0`, phạm vi giá trị của `rand` là `[0, 4]`, có 5 giá trị có thể.

![](https://labuladong.online/algo/images/洗牌算法/1.png)

Vòng for lượt lặp thứ hai, `i = 1`, phạm vi giá trị của `rand` là `[1, 4]`, có 4 giá trị có thể.

![](https://labuladong.online/algo/images/洗牌算法/2.png)

Về sau cứ thế, đến lần lặp cuối cùng, `i = 4`, phạm vi giá trị của `rand` là `[4, 4]`, chỉ có 1 giá trị có thể.

![](https://labuladong.online/algo/images/洗牌算法/3.png)

Có thể thấy, tất cả kết quả có thể mà toàn bộ quá trình sinh ra có `n! = 5! = 5*4*3*2*1` loại, nên thuật toán này đúng.

Phân tích**cách viết thứ hai**, các lần lặp phía trước đều giống nhau, chỉ ít một lần lặp mà thôi. Nên lần lặp cuối cùng, `i = 3`, phạm vi giá trị của `rand` là `[3, 4]`, có 2 giá trị có thể.

```java
// Cách viết thứ hai
// arr = {1,3,5,7,9}, n = 5
    for (int i = 0 ; i < n - 1; i++)
        int rand = randInt(i, n - 1);
```

Nên tất cả kết quả có thể mà toàn bộ quá trình sinh ra vẫn có `5*4*3*2 = 5! = n!` loại, vì nhân với 1 có hay không cũng được. Nên cách viết này cũng đúng.

Nếu nội dung trên bạn đều hiểu, vậy bạn thì có thể phát hiện**cách viết thứ ba** chính là cách viết thứ nhất, chỉ là đem mảng lặp từ sau ra trước mà thôi;**cách viết thứ tư** là cách viết thứ hai từ sau ra trước. Nên chúng đều đúng.

Nếu độc giả từng suy nghĩ về thuật toán xáo trộn, có thể nghĩ ra thuật toán như sau, nhưng **cách viết này sai**:

<!-- muliti_language -->
```java
void shuffle(int[] arr) {
    int n = arr.length();
    for (int i = 0 ; i < n; i++) {
        // Mỗi lần đều từ đoạn đóng [0, n-1]
        // ở giữa chọn ngẫu nhiên phần tử để hoán đổi
        int rand = randInt(0, n - 1);
        swap(arr[i], arr[rand]);
    }
}
```

Bây giờ bạn hẳn đã hiểu tại sao cách viết này sai. Vì tất cả kết quả có thể mà cách viết này thu được có `n^n` loại, chứ không phải `n!` loại, mà `n^n` không thể là bội nguyên của `n!`.

Ví dụ `arr = {1,2,3}`, kết quả đúng hẳn có `3!= 6` khả năng, mà cách viết này tổng cộng có `3^3 = 27` kết quả có thể. Vì 27 không thể bị 6 chia hết, nên nhất định có một số trường hợp bị "thiên vị", tức là nói một số trường hợp xuất hiện với xác suất lớn hơn, nên kết quả xáo trộn này không tính là "thật sự loạn".

Trên đây chúng ta từ trực giác giải thích đơn giản tiêu chuẩn đúng của thuật toán xáo trộn, không chứng minh toán học, tôi nghĩ mọi người cũng lười chứng minh. Với vấn đề xác suất chúng ta có thể dùng "phương pháp Monte Carlo" để kiểm chứng đơn giản.

### Hai, phương pháp Monte Carlo kiểm chứng tính đúng

Thuật toán xáo trộn, hay nói là tiêu chuẩn đo lường tính đúng đắn của thuật toán đặt ngẫu nhiên là:**với mỗi kết quả có thể thì xác suất xuất hiện bắt buộc bằng nhau, tức là nói cần đủ ngẫu nhiên**.

Nếu không dùng chứng minh toán học chặt chẽ xác suất bằng nhau, có thể dùng phương pháp Monte Carlo ước lượng gần đúng xác suất có bằng nhau không, kết quả có đủ ngẫu nhiên không.

Nhớ hồi cấp 3 có bài toán: ném điểm ngẫu nhiên vào một hình vuông, hình vuông này nằm sát một hình tròn, cho bạn biết tổng số điểm ném và số điểm rơi trong hình tròn, bắt bạn tính số pi.

![](https://labuladong.online/algo/images/洗牌算法/4.png)

Chuyện này thực ra chính là lợi dụng phương pháp Monte Carlo: khi điểm ném đủ nhiều, số lượng điểm là có thể gần đúng đại diện diện tích hình. Thông qua công thức diện tích, từ tỷ số diện tích hình vuông và hình tròn có thể rất dễ suy ra số pi. Đương nhiên điểm ném càng nhiều, số pi tính ra càng chính xác, thể hiện đầy đủ chân lý sức mạnh tạo nên kỳ tích.

Tương tự, chúng ta có thể với cùng một mảng tiến hành một triệu lần xáo trộn, thống kê số lần mỗi loại kết quả xuất hiện, lấy tần suất làm xác suất, có thể rất dễ nhận ra thuật toán xáo trộn có đúng không. Tư tưởng tổng thể rất đơn giản, có điều cài đặt cũng có chút kỹ thuật, dưới đây phân tích đơn giản vài ý tưởng cài đặt.

**ý tưởng thứ nhất**, chúng ta liệt kê tất cả hoán vị của mảng arr ra, làm thành một biểu đồ cột (giả sử arr = {1,2,3}):

![](https://labuladong.online/algo/images/洗牌算法/5.jpg)

Mỗi lần tiến hành thuật toán xáo trộn xong, rồi đem tần suất tương ứng của kết quả xáo trộn thu được cộng một, lặp lại 1 triệu lần, nếu tổng số lần mỗi loại kết quả xuất hiện gần như, vậy thì cho thấy xác suất mỗi loại kết quả xuất hiện hẳn bằng nhau. Viết mã giả củaý tưởng này:

<!-- muliti_language -->
```java
void shuffle(int[] arr);

// Monte Carlo
int N = 1000000;
HashMap count; // Làm biểu đồ cột
for (i = 0; i < N; i++) {
    int[] arr = {1,2,3};
    shuffle(arr);
    // Lúc này arr đã bị xáo trộn
    count[arr] += 1; 
}
for (int feq : count.values())
    print(feq / N + " "); // Tần suất
```

Phương án kiểm nghiệm này khả thi, có điều có độc giả sẽ hỏi, toàn bộ hoán vị của arr có n! loại (n là độ dài arr), nếu n khá lớn, vậy chẳng phải độ phức tạp không gian nổ sao?

Đúng, có điều làm phương pháp kiểm chứng, chúng ta không cần n quá lớn, thường dùng arr độ dài 5 hoặc 6 thử một chút sẽ gần như, vì chúng ta chỉ muốn so sánh xác suất kiểm chứng tính đúng mà thôi.

**ý tưởng thứ hai**, có thể nghĩ như vầy, mảng arr toàn là 0, chỉ có một số 1. Chúng ta với arr tiến hành 1 triệu lần xáo trộn, ghi lại số lần số 1 xuất hiện ở mỗi vị trí chỉ số, nếu số lần mỗi chỉ số xuất hiện gần như, cũng có thể giải thích xác suất mỗi loại kết quả xáo trộn bằng nhau.

```java
void shuffle(int[] arr);

// Phương pháp Monte Carlo
int N = 1000000;
int[] arr = {1,0,0,0,0};
int[] count = new int[arr.length];
for (int i = 0; i < N; i++) {
    shuffle(arr); // Xáo trộn arr
    for (int j = 0; j < arr.length; j++)
        if (arr[j] == 1) {
            count[j]++;
            break;
        }
}
for (int feq : count)
    print(feq / N + " "); // Tần suất
```

![](https://labuladong.online/algo/images/洗牌算法/6.png)

ý tưởng này cũng khả thi, mà tránh được độ phức tạp không gian giai thừa, nhưng nhiều rồi vòng for lồng nhau, độ phức tạp thời gian cao hơn chút. Có điều do lượng dữ liệu test của chúng ta sẽ không lớn, những vấn đề này đều có thể bỏ qua.

Ngoài ra, độc giả tinh ý có thể phát hiện một vấn đề, haiý tưởng trên khai báo vị trí arr khác nhau, một trong vòng for, một ngoài vòng for. Thực ra hiệu quả đều giống nhau, vì thuật toán của chúng ta luôn cần xáo trộn arr, nên thứ tự của arr không quan trọng, chỉ cần phần tử không đổi là được.

### Ba, tổng kết cuối

Phần một của bài này giới thiệu thuật toán xáo trộn (thuật toán đặt ngẫu nhiên), thông qua mộtkỹ thuật phân tích đơn giản chứng minh bốn dạng đúng của thuật toán này, đồng thời phân tích một cách viết sai thường gặp, tin rằng bạn nhất định có thể viết ra thuật toán xáo trộn đúng.

Phần hai viết tiêu chuẩn đo lường tính đúng của thuật toán xáo trộn, tức xác suất mỗi loại kết quả ngẫu nhiên xuất hiện bắt buộc bằng nhau. Nếu chúng ta không dùng chứng minh toán học chặt chẽ, có thể thông qua phương pháp Monte Carlo sức mạnh tạo nên kỳ tích, kiểm chứng sơ tính đúng của thuật toán. Phương pháp Monte Carlo cũng có ý tưởng khác nhau, có điều yêu cầu không cần quá nghiêm ngặt, vì chúng ta chỉ tìm một kiểm chứng đơn giản.

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

** “ Ghi chép thuật toán của labuladong ” đã xuất bản, theo dõikênh WeChat chính thức xem chi tiết; nhắn tin tới hộp thư "** toàn **" có thể tải PDFđi kèm vàbộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======

[384.Xáo trộn mảng](https://leetcode-cn.com/problems/shuffle-an-array)

### javascript

```js
// Thu được một số nguyên ngẫu nhiên trong đoạn đóng [min, max]
const randInt = function (minNum, maxNum) {
    return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10);
};


// Cách viết thứ nhất
let shuffle = function (arr) {
    const swap = (i, j) => {
        let t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

    let n = arr.length;

    /******** Khác biệt chỉ hai dòng này ********/
    for (let i = 0; i < n; i++) {
        // Chọn ngẫu nhiên một phần tử từ i đến cuối
        let rand = randInt(i, n - 1);
        /*************************/
        // Hoán đổi phần tử trên i rand
        swap(i, rand);
    }
}

// Cách viết thứ hai
let shuffle = function (arr) {
    const swap = (i, j) => {
        let t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

    let n = arr.length;


    /******** Khác biệt chỉ hai dòng này ********/
    for (let i = 0; i < n - 1; i++) {
        let rand = randInt(i, n - 1);
        /*************************/
        // Hoán đổi phần tử trên i rand
        swap(i, rand);
    }
}

// Cách viết thứ ba
let shuffle = function (arr) {
    const swap = (i, j) => {
        let t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

    let n = arr.length;


    /******** Khác biệt chỉ hai dòng này ********/
    for (let i = n - 1; i >= 0; i--) {
        let rand = randInt(0, i);

        /*************************/
        // Hoán đổi phần tử trên i rand
        swap(i, rand);
    }
}

// Cách viết thứ tư
let shuffle = function (arr) {
    const swap = (i, j) => {
        let t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

    let n = arr.length;


    /******** Khác biệt chỉ hai dòng này ********/
    for (let i = n - 1; i > 0; i--) {
        let rand = randInt(0, i);
        /*************************/
        // Hoán đổi phần tử trên i rand
        swap(i, rand);
    }
}
```
