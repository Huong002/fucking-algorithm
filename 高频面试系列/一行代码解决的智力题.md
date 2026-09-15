# Bài thuật toán giải bằng một dòng code

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
| [292. Nim Game](https://leetcode.com/problems/nim-game/) | [292. Game Nim](https://leetcode.cn/problems/nim-game/) | 🟢
| [319. Bulb Switcher](https://leetcode.com/problems/bulb-switcher/) | [319. Công tắc bóng đèn](https://leetcode.cn/problems/bulb-switcher/) | 🟠
| [877. Stone Game](https://leetcode.com/problems/stone-game/) | [877. Game đá](https://leetcode.cn/problems/stone-game/) | 🟠

**-----------**

Dưới đây là ba bài "đố mẹo" thú vị tôi tổng kết trong quá trình luyện đề, có thể dùng lập trình thuật toán giải, nhưng chỉ cần nghĩ một chút, là tìm ra quy luật, nghĩ thẳng đáp án.

### Một, game Nim

LeetCode 292 「game Nim」 cho quy tắc game thế này:

Bạn và bạn bạn trước mặt có một đống đá, các bạn lần lượt lấy, một lần lấy ít nhất một viên, nhiều nhất ba viên, ai lấy viên đá cuối thắng.

Giả sử các bạn đều rất thông minh, bạn bắt đầu lấy trước, hãy viết một thuật toán, input một số nguyên dương `n`, trả về bạn có thắng không (true hay false).

Ví dụ giờ có 4 viên đá, thuật toán nên trả về false. Vì dù bạn lấy 1 viên 2 viên hay 3 viên, đối phương đều một lần lấy hết, lấy viên đá cuối, nên bạn nhất định thua.

Trước hết, bài này chắc chắn có thể dùng quy hoạch động, vì rõ ràng bài gốc tồn tại bài toán con, mà bài toán con tồn tại trùng lặp. Nhưng vì các bạn đều rất thông minh, liên quan đếnđối đầu giữa bạn và đối thủ, quy hoạch động sẽ tương đối phức tạp.

**Ý tưởng giải loại vấn đề này thông thường đều nghĩ ngược**:

Nếu tôi thắng được, vậy lúc tới lượt tôi lấy đá bắt buộc phải còn 1~3 viên đá, như vậy tôi mới một lần lấy hết.

Làm sao tạo ra cục diện vậy? Rõ ràng, nếu đối thủ lấy lúc chỉ còn 4 viên đá, thì dù hắn lấy thế nào, luôn còn 1~3 viên đá, tôi sẽ thắng.

Làm sao ép đối thủ đối mặt 4 viên đá? Phải nghĩ cách, để lúc tôi chọn còn 5~7 viên đá, vậy tôi sẽ nắm chắc để đối phươngbắt buộc phải đối mặt 4 viên đá.

Làm sao tạo ra cục diện 5~7 viên đá? Để đối thủ đối mặt 8 viên đá, dù hắn lấy thế nào, đều cho tôi còn 5~7 viên, tôi sẽ thắng.

Cứ tuần hoàn vậy, ta phát hiện chỉ cầndẫm phải bội 4,thì rơi vào bẫy,mãi mãi thoát không khỏi bội 4, mà nhất định thua. Nên cách giải bài này vô cùng đơn giản:

<!-- muliti_language -->
```java
boolean canWinNim(int n) {
    // Nếu lên đã dẫm phải bội 4, thì chịu thua đi
    // Nếu không, có thể khống chế đối phương ở bội 4,ắt thắng
    return n % 4 != 0;
}
```

### Hai, game đá

LeetCode 877 「game đá」 quy tắc thế này:

Bạn và bạn bạn trước mặt có một hàng đống đá, dùng mảng `piles` biểu thị, `piles[i]` biểu thị đống thứ `i` có bao nhiêu đá. Các bạn lần lượt lấy đá, một lần lấy một đống, nhưng chỉ có thể lấy đống đá trái nhất hay phải nhất. Mọi đá bị lấy hết xong, ai có đá nhiều, người đó thắng.

**Giả sử các bạn đều rất thông minh**, bạn bắt đầu lấy trước, hãy viết một thuật toán, input một mảng `piles`, trả về bạn có thắng không (true hay false).

Chú ý, số đống đá là chẵn, nên số đống hai bạn lấy nhất định giống nhau. Tổng số đá là lẻ, nghĩa là cuối các bạn không thể có đá nhiều bằng nhau, nhất định có thắng thua.

Lấy ví dụ, `piles=[2, 1, 9, 5]`, bạn lấy trước, có thể lấy 2 hay 5, bạn chọn 2.

`piles=[1, 9, 5]`, tới đối thủ, có thể lấy 1 hay 5, hắn chọn 5.

`piles=[1, 9]` tới bạn lấy, bạn lấy 9.

Cuối cùng, đối thủ của bạn chỉ có thể lấy 1.

Như vậy xuống, bạn tổng có `2 + 9 = 11` viên đá, đối thủ có `5 + 1 = 6` viên đá, bạn thắng được, nên thuật toán nên trả về true.

Bạn thấy,không phải đơn giản chọn số lớn, vì sao lần đầu chọn 2 chứ không phải 5? Vì sau 5 là 9, bạn mà tham lợi nhất thời,thì đem đống đá 9 này lộ cho đối thủ, vậy bạn sẽ thua.

Đây cũng là nhấn mạnh hai bên đều rất thông minh, thuật toán cũng cầu quá trình quyết sách tối ưu xem bạn có thắng không.

Bài này lại liên quan đối đầu hai người, cũng có thể dùng thuật toán quy hoạch động vét cạn thử, tương đối phiền. Nhưng ta chỉ cần suy nghĩ sâu quy tắc, sẽ kinh ngạc: chỉ cần bạn đủ thông minh, bạn ắt thắng không nghi ngờ, vì bạn đi trước.

<!-- muliti_language -->
```java
boolean stoneGame(int[] piles) {
    return true;
}
```

Vì sao? Vì đề có hai điều kiện rất quan trọng: một là tổng cộng đá có chẵn đống, tổng số đá là lẻ. Hai điều kiện tưởng chừng tăng tính công bằng cho game này, ngược lại khiến game này thành game lùa gà. Ta lấy `piles=[2, 1, 9, 5]` giảng, giả sử bốn đống đá này từ trái sang phải chỉ số lần lượt 1,2,3,4.

Nếu ta đem bốn đống đá nàytheo tính chẵn lẻ của chỉ số chia hai nhóm, tức đống thứ 1,3 và đống thứ 2,4, vậy số đá hai nhóm này nhất định khác nhau, nghĩa là một nhóm nhiều một nhóm ít. Vì tổng số đá là lẻ, không thể chia đều.

Còn làm người đầu lấy đá, bạn có thể khống chế mình lấy mọi đống chẵn, hay mọi đống lẻ.

Bạn đầu có thể chọn đống thứ 1 hay đống thứ 4. Nếu bạn muốn đống chẵn, bạn thì lấy đống thứ 4, như vậy để lại cho đối thủ chọn chỉ có đống thứ 1,3, hắn dù lấy thế nào, đống thứ 2 lại lộ ra, bạn sẽ lấy được. Tương tự, nếu bạn muốn lấy đống lẻ, bạn thì lấy đống thứ 1, để lại cho đối thủ chỉ có đống thứ 2,4, hắn dù lấy thế nào, đống thứ 3 lại lộ cho bạn.

Nghĩa là, bạn có thể ở bước đầu đã quan sát tốt, tổng đá đống lẻ nhiều, hay tổng đá đống chẵn nhiều, rồi từng bước chắc chắn, mọi thứ đều nằm trong kiểm soát. Biết lỗ hổng này, có thể đem ra đố bạn bè không biết.

### Ba, vấn đề công tắc đèn

LeetCode 319 「công tắc bóng đèn」 quy tắc thế này:

Có `n` bóng đèn, lúc đầu đều tắt. Giờ phải tiến hành `n` vòng thao tác:

Vòng 1 là nhấn công tắc mỗi bóng một cái (mở toàn bộ).

Vòng 2 là nhấn công tắc mỗi hai bóng một cái (chính là nhấn công tắc bóng thứ 2,4,6... chúng bị tắt).

Vòng 3 là nhấn công tắc mỗi ba bóng một cái (chính là nhấn công tắc bóng thứ 3,6,9... có cái bị tắt, ví dụ 3, có cái được mở, ví dụ 6)...

Cứ vậy, tới vòng thứ `n`, tức chỉ nhấn công tắc bóng thứ `n` một cái.

Giờ cho bạn input một số nguyên dương `n` đại diện số bóng, hỏi bạn qua `n` vòng thao tác, mấy bóng sáng?

Ta đương nhiên có thể dùng một mảng boolean biểu thị tình hình công tắc các bóng này, rồi mô phỏng quá trình thao tác này, cuối cùng đếm là ra kết quả. Nhưng vậycó vẻ không có linh tính, cách giải tốt nhất thế này:

<!-- muliti_language -->
```java
int bulbSwitch(int n) {
    return (int)Math.sqrt(n);
}
```

Gì? Vấn đề này với căn bậc hai có quan hệ gì? Thực ra cách giải nàykhá tinh diệu, nếu không ai nói cách giải,thật không dễ nghĩ rõ.

Trước hết, vì bóng ngay từ đầu đều tắt, nên bóng nào cuối cùng nếu sáng,ắt phải bị nhấn số lần lẻ công tắc.

Ta giả sử chỉ có 6 bóng, mà ta chỉ nhìn bóng thứ 6. Cần tiến hành 6 vòng thao tác đúng không, hỏi với bóng thứ 6, sẽ bị nhấn mấy lần công tắc? Không khó nhận ra, vòng 1 bị nhấn, vòng 2, vòng 3, vòng 6 đều bị nhấn.

Vì sao vòng 1,2,3,6 bị nhấn? Vì `6=1*6=2*3`. Tình huống thông thường, ước đều theo cặp xuất hiện, nghĩa là số lần công tắc bị nhấn thông thường là chẵn. Nhưng có trường hợp đặc biệt, ví dụ tổng có 16 bóng, vậy bóng thứ 16 bị nhấn mấy lần?

`16 = 1*16 = 2*8 = 4*4`

Trong đó ước 4 lặp lại, nên bóng thứ 16 bị nhấn 5 lần, lẻ. Giờ bạn hẳn hiểu vấn đề này vì sao liên quan với căn bậc hairồi chứ??

Nhưng, ta chẳng phải muốn tính cuối cùng mấy bóng sáng sao, trực tiếp căn bậc hai một cái là ý gì? Nghĩ một chút là hiểu được.

Cứ giả sử giờ tổng có 16 bóng, ta tính căn 16, bằng 4, điều này cho thấy cuối cùng sẽ có 4 bóng sáng, chúng lần lượt là bóng thứ `1*1=1`, `2*2=4`, `3*3=9` và `4*4=16`.

Cho dù với `n` mà kết quả căn là số thập phân, ép thành int, cũng tương đương một cận trên nguyên lớn nhất, mọi số nguyên nhỏ hơn cận trên này, bình phương xong các chỉ số đều là chỉ số bóng cuối cùng sáng. Nên nói ta trực tiếp đem căn bậc hai ép thành số nguyên, chính là đáp án của vấn đề này.



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Chớp nhoáng mọi bài toán dãy xấu](https://labuladong.online/algo/frequency-interview/ugly-number-summary/)
 - [Tâm đắc luyện đề của tôi: bản chất thuật toán](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [Quy hoạch động kinh điển: vấn đề đối đầu](https://labuladong.online/algo/dynamic-programming/game-theory/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Ghi chép thuật toán của labuladong》 đã xuất bản, theo dõi kênh WeChat chính thức để xem chi tiết; nhắn tin tới hộp thư "**bộ toàn tập**" có thể tải PDF đi kèm và bộ luyện đề toàn tập**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code ngôn ngữ khác======

[292.Game Nim](https://leetcode-cn.com/problems/nim-game)

[877.Game đá](https://leetcode-cn.com/problems/stone-game)

[319.Công tắc bóng đèn](https://leetcode-cn.com/problems/bulb-switcher)



### python

Do [JodyZ0203](https://github.com/JodyZ0203) cung cấp cách giải Python3 292. Game Nim:

```Python
class Solution:
    def canWinNim(self, n: int) -> bool:
        # Nếu chia dư 0, nghĩa là bội của 4, nên ắt thua
        # Nếu chia dư khác 0, nghĩa là không phải bội của 4, nghĩa là ắt thắng
        return n % 4 != 0  
```

Do [JodyZ0203](https://github.com/JodyZ0203) cung cấp cách giải Python3 877. Game đá:

```Python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        # Ở tiền đề hai bên đều thông minh, đi trước ắt thắng không nghi ngờ
        # Đi trước có thể quan sát trước tổng đá đống chẵn hay đống lẻ nhiều hơn
        return True
```


Do [JodyZ0203](https://github.com/JodyZ0203) cung cấp cách giải Python3 319. Công tắc bóng đèn:

```Python
class Solution:
    def bulbSwitch(self, n: int) -> int:
        # Căn số bóng rồi làm tròn xuống là được
        return floor(sqrt (n))       
```



### c++

Do [JodyZ0203](https://github.com/JodyZ0203) cung cấp cách giải C++ 877. Game đá:

```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
       // Ở tiền đề hai bên đều thông minh, đi trước ắt thắng không nghi ngờ
       return true;
    }
};
```



Do [JodyZ0203](https://github.com/JodyZ0203) cung cấp cách giải C++ 319. Công tắc bóng đèn:

```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        // Căn số bóng rồi làm tròn xuống là được
        return floor(sqrt (n));
    }
};
```



### javascript

[292.Game Nim](https://leetcode-cn.com/problems/nim-game)

```js
/**
 * @param {number} n
 * @return {boolean}
 */
var canWinNim = function(n) {
    // Nếu lên đã dẫm phải bội 4, thì chịu thua đi
    // Nếu không, có thể khống chế đối phương ở bội 4, ắt thắng
    return n % 4 !== 0;
};
```

[877.Game đá](https://leetcode-cn.com/problems/stone-game)

```js
var stoneGame = function(piles) {
    return true;
};
```

[319.Công tắc bóng đèn](https://leetcode-cn.com/problems/bulb-switcher)

```js
/**
 * @param {number} n
 * @return {number}
 */
var bulbSwitch = function(n) {
    return Math.floor(Math.sqrt(n));
};
```
