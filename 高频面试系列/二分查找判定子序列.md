# Tìm kiếm nhị phân kiểm tra hiệu quả dãy con

<p align='center'>
<a href="https://github.com/labuladong/fucking-algorithm" target="view_window"><img alt="GitHub" src="https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat-square&logo=GitHub"></a>
<a href="https://labuladong.online/algo/" target="_blank"><img class="my_header_icon" src="https://img.shields.io/static/v1?label=精品课程&message=查看&color=pink&style=flat"></a>
<a href="https://www.zhihu.com/people/labuladong"><img src="https://img.shields.io/badge/%E7%9F%A5%E4%B9%8E-@labuladong-000000.svg?style=flat-square&logo=Zhihu"></a>
<a href="https://space.bilibili.com/14089380"><img src="https://img.shields.io/badge/B站-@labuladong-000000.svg?style=flat-square&logo=Bilibili"></a>
</p>

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: [Hội viên web bản mới](https://labuladong.online/algo/intro/site-vip/) sắp tăng giá; đã hỗ trợ gia hạn user cũ~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được công thức thuật toán, mà còn tiện thể giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [392. Is Subsequence](https://leetcode.com/problems/is-subsequence/) | [392. Kiểm tra dãy con](https://leetcode.cn/problems/is-subsequence/) | 🟢
| [792. Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/) | [792. Số từ khớp dãy con](https://leetcode.cn/problems/number-of-matching-subsequences/) | 🟠

**-----------**

Tìm kiếm nhị phân bản thân không khó hiểu, khó ở chỗ khéo vận dụng kỹ thuật tìm kiếm nhị phân.

Với một vấn đề, bạn có thể đều rất khó nghĩ nó với tìm kiếm nhị phân liên quan, ví dụ bài trước [dãy tăng dài nhất](https://labuladong.online/algo/dynamic-programming/longest-increasing-subsequence/)đã dựa vào một game bài để dẫn xuất ra cách giải tìm kiếm nhị phân.

Hôm nay giảng tiếp một bài khéo dùng tìm kiếm nhị phân, LeetCode 392 "kiểm tra dãy con":

Hãy bạn kiểm tra xem chuỗi `s` có phải dãy con của chuỗi `t` không (có thể giả định `s` dài tương đối nhỏ, mà `t` dài vô cùng lớn).

Lấy hai ví dụ:

```
s = "abc", t = "**a**h**b**gd**c**", return true.

s = "axc", t = "ahbgdc", return false.
```

Đề rất dễ hiểu, mà trông rất đơn giản, nhưng rất khó nghĩ vấn đề này liên quan với tìm kiếm nhị phân phải không?

### Một, phân tích vấn đề

Trước hết, một cách giải rất đơn giản thế này:

<!-- muliti_language -->
```java
boolean isSubsequence(String s, String t) {
    int i = 0, j = 0;
    while (i < s.length() && j < t.length()) {
        if (s.charAt(i) == t.charAt(j)) {
            i++;
        }
        j++;
    }
    return i == s.length();
}
```

<visual slug='is-subsequence'/>

Ý tưởng của nó cũng vô cùng đơn giản, lợi dụng hai con trỏ `i, j` lần lượt trỏ `s, t`, vừa tiến vừa khớp dãy con:

![](https://labuladong.online/algo/images/子序列/1.gif)

Bạn có lẽ hỏi, đây chẳng phải cách giải tối ưu sao, độ phức tạp thời gian chỉ cần O(N), N là độ dài `t`.

Đúng, nếu chỉ là vấn đề này, cách giải này đã đủ tốt, **nhưng vấn đề này còn có follow up**:

Nếu cho bạn một loạt chuỗi `s1,s2,...` và chuỗi `t`, bạn cần kiểm tra mỗi chuỗi `s` có phải dãy con của `t` không (có thể giả định `s` khá ngắn, `t` rất dài).

<!-- muliti_language -->
```java
boolean[] isSubsequence(String[] sn, String t);
```

Bạn có lẽ hỏi, đây không phải rất đơn giản sao, vẫn logic vừa rồi, thêm vòng for chẳng phải được?

Được, nhưng cách giải này xử lý mỗi `s` độ phức tạp thời gian vẫn là O(N), còn nếu khéo vận dụng tìm kiếm nhị phân, có thể giảm độ phức tạp thời gian xuống khoảng O(MlogN). Vì N tương đối lớn hơn M rất nhiều, nên hiệu suất cách sau cao hơn.

### Hai, ý tưởng nhị phân

Ý tưởng nhị phân chủ yếu là tiền xử lý `t`, dùng một từ điển `index` đem vị trí chỉ số mỗi ký tự xuất hiện lưu lại theo thứ tự:

<!-- muliti_language -->
```java
int m = s.length(), n = t.length();
ArrayList<Integer>[] index = new ArrayList[256];
// Ghi trước vị trí mỗi ký tự trong t xuất hiện
for (int i = 0; i < n; i++) {
    char c = t.charAt(i);
    if (index[c] == null) 
        index[c] = new ArrayList<>();
    index[c].add(i);
}
```

![](https://labuladong.online/algo/images/子序列/2.jpg)

Ví dụ với tình huống này, khớp "ab" xong, nên khớp "c":

![](https://labuladong.online/algo/images/子序列/1.jpg)

Theo cách giải trước, ta cần `j` tuyến tính tiến quét ký tự "c", nhưng nhờ thông tin ghi trong `index`, **có thể tìm nhị phân chỉ số lớn hơn j trong `index[c]`**, ở ví dụ hình trên, chính là trong `[0,2,6]` tìm chỉ số lớn hơn 4 đó:

![](https://labuladong.online/algo/images/子序列/3.jpg)

Như vậy là trực tiếp được chỉ số "c" tiếp. Giờ vấn đề chính là, dùng tìm kiếm nhị phân tính chỉ số vừa khéo lớn hơn 4 đó thế nào? Đáp án là, tìm kiếm nhị phân tìm biên trái là làm được.

### Ba, bàn lại tìm kiếm nhị phân

Ở bài trước [chi tiết tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/)đã giảng chi tiết cách viết đúng ba thuật toán tìm kiếm nhị phân. Tìm kiếm nhị phân trả về chỉ số giá trị mục tiêu `val`, với tìm kiếm nhị phân tìm **biên trái**, có một tính đặc thù:

**Khi `val` không tồn tại, chỉ số được trả về vừa khéo là chỉ số của phần tử nhỏ nhất lớn hơn `val`**.

Ý gì, chính là nói nếu trong mảng `[0,1,3,4]` tìm phần tử 2, thuật toán sẽ trả về chỉ số 2, chính là vị trí phần tử 3, phần tử 3 là phần tử nhỏ nhất lớn hơn 2 trong mảng. Nên ta có thể lợi dụng tìm nhị phân tránh quét tuyến tính.

<!-- muliti_language -->
```java
// Tìm kiếm nhị phân biên trái
int left_bound(ArrayList<Integer> arr, int target) {
    int left = 0, right = arr.size();
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (target > arr.get(mid)) {
            left = mid + 1;
        } else {
            right = mid;
        } 
    }
    if (left == arr.size()) {
        return -1;
    }
    return left;
}
```

Trên chính là tìm kiếm nhị phân biên trái, lát nữa sẽ dùng, chi tiết trong đó xem bài trước [chi tiết tìm kiếm nhị phân](https://labuladong.online/algo/essential-technique/binary-search-framework/), ở đây không lặp lại nữa.

Ở đây lấy chuỗi đơn `s` làm ví dụ, với nhiều chuỗi `s`, có thể tách phần tiền xử lý ra.

<!-- muliti_language -->
```java
boolean isSubsequence(String s, String t) {
    int m = s.length(), n = t.length();
    // Tiền xử lý t
    ArrayList<Integer>[] index = new ArrayList[256];
    for (int i = 0; i < n; i++) {
        char c = t.charAt(i);
        if (index[c] == null) 
            index[c] = new ArrayList<>();
        index[c].add(i);
    }
    
    // Con trỏ trên chuỗi t
    int j = 0;
    // Nhờ index tìm s[i]
    for (int i = 0; i < m; i++) {
        char c = s.charAt(i);
        // Cả t hoàn toàn không có ký tự c
        if (index[c] == null) return false;
        int pos = left_bound(index[c], j);
        // Trong khoảng tìm nhị phân không tìm thấy ký tự c
        if (pos == -1) return false;
        // Di con trỏ j về trước
        j = index[c].get(pos) + 1;
    }
    return true;
}
```

Quá trình chạy thuật toán thế này:

![](https://labuladong.online/algo/images/子序列/2.gif)

Thấy được nhờ tìm kiếm nhị phân, hiệu suất thuật toán có thể tăng lên đáng kể.

Hiểu ý tưởng này, ta có thể trực tiếp giải gọn LeetCode 792 "số từ khớp dãy con": cho bạn input một list chuỗi `words` và một chuỗi `s`, hỏi bạn trong `words` có mấy chuỗi là dãy con của `s`.

Chữ ký hàm như sau:

<!-- muliti_language -->
```java
int numMatchingSubseq(String s, String[] words)
```

Ta đem code bài trước sửa một chút là hoàn thành bài này:

<!-- muliti_language -->
```java
int numMatchingSubseq(String s, String[] words) {
    // Tiền xử lý s
    // char -> list chỉ số của char đó
    ArrayList<Integer>[] index = new ArrayList[256];
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (index[c] == null) {
            index[c] = new ArrayList<>();
        }
        index[c].add(i);
    }
    
    int res = 0;
    for (String word : words) {
        // Con trỏ trên chuỗi word
        int i = 0;
        // Con trỏ trên chuỗi s
        int j = 0;
        // Nhờ index tìm chỉ số mỗi ký tự trong word
        for (; i < word.length(); i++) {
            char c = word.charAt(i);
            // Cả s hoàn toàn không có ký tự c
            if (index[c] == null) {
                break;
            }
            int pos = left_bound(index[c], j);
            // Trong khoảng tìm nhị phân không tìm thấy ký tự c
            if (pos == -1) {
                break;
            }
            // Di con trỏ j về trước
            j = index[c].get(pos) + 1;
        }
        // Nếu word khớp xong, thì là dãy con
        if (i == word.length()) {
            res++;
        }
    }
    
    return res;
}

// Tìm kiếm nhị phân biên trái
int left_bound(ArrayList<Integer> arr, int target) {
    // Xem trên
}
```




**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》 đã xuất bản, theo dõi kênh WeChat chính thức để xem chi tiết; nhắn tin tới hộp thư "**bộ toàn tập**" có thể tải PDF đi kèm và bộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác====== 

[392. Kiểm tra dãy con](https://leetcode-cn.com/problems/is-subsequence)

### c++

[dekunma](https://www.linkedin.com/in/dekun-ma-036a9b198/) cung cấp code C++ 
**Cách giải một: duyệt (cũng có thể dùng hai con trỏ):**  

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        // Duyệt s
        for(int i = 0; i < s.size(); i++) {
            // Tìm vị trí ký tự s[i] trong t
            size_t pos = t.find(s[i]);
            
            // Nếu ký tự s[i] không trong t, trả về false
            if(pos == std::string::npos) return false;
            // Nếu s[i] trong t, về sau chỉ xem chuỗi con sau pos, tránh tìm trùng
            else t = t.substr(pos + 1);
        }
        return true;
    }
};
```

**Cách giải hai: tìm kiếm nhị phân:**  
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int m = s.size(), n = t.size();
        // Tiền xử lý t
        vector<int> index[256];
        for (int i = 0; i < n; i++) {
            char c = t[i];
            index[c].push_back(i);
        }
        // Con trỏ trên chuỗi t
        int j = 0;
        // Nhờ index tìm s[i]
        for (int i = 0; i < m; i++) {
            char c = s[i];
            // Cả t hoàn toàn không có ký tự c
            if (index[c].empty()) return false;
            int pos = left_bound(index[c], j);
            // Trong khoảng tìm nhị phân không tìm thấy ký tự c
            if (pos == index[c].size()) return false;
            // Di con trỏ j về trước
            j = index[c][pos] + 1;
        }
        return true;
    }
    // Tìm kiếm nhị phân biên trái
    int left_bound(vector<int> arr, int tar) {
        int lo = 0, hi = arr.size();
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            if (tar > arr[mid]) {
                lo = mid + 1;
            } else {
                hi = mid;
            } 
        }
        return lo;
    }
};
```



### javascript

Cách hai con trỏ quét một lần

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function (s, t) {
    let i = 0, j = 0;
    while (i < s.length && j < t.length) {
        if (s[i] === t[j]) i++;
        j++;
    }
    return i === s.length;
};
```



**Nâng cấp: cách nhị phân, ứng phó được trường hợp nhiều s**

```js
var isSubsequence = function (s, t) {
    let m = s.length, n = t.length;
    let index = new Array(256);
    // Ghi trước vị trí mỗi ký tự trong t xuất hiện
    for (let i = 0; i < n; i++) {
        let c = t[i];
        if (index[c] == null) {
            index[c] = [];
        }
        index[c].push(i)
    }

    // Con trỏ trên chuỗi t
    let j = 0;
    // Nhờ index tìm s[i]
    for (let i = 0; i < m; i++) {
        let c = s[i];
        // Cả t hoàn toàn không có ký tự c
        if (index[c] == null) return false
        let pos = left_bound(index[c], j);
        // Trong khoảng tìm nhị phân không tìm thấy ký tự c
        if (pos == index[c].length) return false;

        // Di con trỏ j về trước
        j = index[c][pos] + 1;
    }
    return true;
};

var left_bound = function (arr, tar) {
    let lo = 0, hi = arr.length;
    while (lo < hi) {

        let mid = lo + Math.floor((hi - lo) / 2);
        if (tar > arr[mid]) {
            lo = mid + 1;
        } else {
            hi = mid;
        }
    }
    return lo;
}
```
