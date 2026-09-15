# Chuyển đổi tư duy giữa quy hoạch động và thuật toán quay lui



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Để đáp ứng nhu cầu của đông đảo bạn đọc, website đã ra mắt [Lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được mô-típ thuật toán mà còn tiện thể giải được các đề sau:

| LeetCode | Lực khấu (LeetCode Trung Quốc) | Độ khó |
| :----: | :----: | :----: |
| [139. Word Break](https://leetcode.com/problems/word-break/) | [139. Tách từ](https://leetcode.cn/problems/word-break/) | 🟠 |
| [140. Word Break II](https://leetcode.com/problems/word-break-ii/) | [140. Tách từ II](https://leetcode.cn/problems/word-break-ii/) | 🔴 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
> 
> - [Thuật toán dòng cây nhị phân (cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
> - [Khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/)

Trước đây [Cầm tay chỉ bạn cày cây nhị phân (cương lĩnh)](https://labuladong.online/algo/essential-technique/binary-tree-summary/) chia liệt kê đệ quy thành hai ý tưởng「duyệt」và「phân rã bài toán」, trong đó ý tưởng「duyệt」mở rộng thêm một chút chính là [thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/), ý tưởng「phân rã bài toán」có thể mở rộng thành [thuật toán quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/).

Kiểu chuyển đổi tư duy này không chỉ giới hạn trong thuật toán liên quan tới cây nhị phân, bài này sẽ thoát khỏi dạng bài cây nhị phân để xem trong đề thuật toán thực tế làm sao trừu tượng hóa bài toán thành cấu trúc cây, tùy cơ ứng biến tối ưu từng bước, từ đó thực hiện chuyển đổi tư duy「duyệt」và「phân rã bài toán」, chuyển mượt mà từ thuật toán quay lui sang thuật toán quy hoạch động.

Nói ngoài lề một câu, bài trước [Giải thích chi tiết khung cốt lõi quy hoạch động](https://labuladong.online/algo/essential-technique/dynamic-programming-framework/) nói, **bài toán quy hoạch động chuẩn nhất định là tìm giá trị tối ưu**, vì dạng bài quy hoạch động có một tính chất gọi là「cấu trúc con tối ưu」, tức suy ra nghiệm tối ưu của bài toán gốc từ nghiệm tối ưu của bài toán con.

Nhưng trong ngữ cảnh thường ngày của bọn mình, dù không phải đề tìm giá trị tối ưu, chỉ cần thấy dùng bảng ghi nhớ loại bỏ bài toán con chồng lặp là bọn mình thường gọi nó là thuật toán quy hoạch động. Nói nghiêm ngặt thì điều này không phù hợp định nghĩa bài toán quy hoạch động, bảo cách giải này gọi là「thuật toán DFS kèm bảng ghi nhớ」có lẽ chính xác hơn. Nhưng mình cũng không cần lăn tăn chi tiết tên gọi này, đã gọi quen miệng thì gọi nó là quy hoạch động cũng không sao.

Hai đề bài này giảng giải cũng không phải tìm giá trị tối ưu, nhưng vẫn sẽ gọi cách giải của chúng là cách giải quy hoạch động, ở đây nói trước với mọi người về chỗ linh động này để bạn đọc kỹ tính khỏi thắc mắc. Không nói nhiều nữa, xem đề luôn.




## Tách từ I



Trước hết xem bài 139「Tách từ」trên LeetCode:

<Problem slug="word-break" />

Chữ ký hàm như sau:

```java
boolean wordBreak(String s, List<String> wordDict);
```

Đây là một đề phỏng vấn tần suất rất cao, ta cùng suy nghĩ xem giải nó thế nào qua ý tưởng「duyệt」và「phân rã bài toán」.

### Ý tưởng duyệt (cách giải quay lui)

**Nói trước ý tưởng「duyệt」, tức là dùng thuật toán quay lui giải bài này**. Ứng dụng kinh điển nhất của thuật toán quay lui chính là các bài hoán vị tổ hợp, không khó phát hiện đổi cách nói thì đề này cũng có thể biến thành một bài hoán vị:

Giờ cho bạn một danh sách từ `wordDict` không chứa từ lặp lại và một chuỗi `s`, hãy phán đoán xem có thể chọn ra một hoán vị gồm một số từ trong `wordDict` (có thể chọn lặp lại) để tạo thành chuỗi `s` không.

Đây chính là biến thể cuối cùng đã giảng trong bài trước [Quay lui xử gọn chín biến thể bài hoán vị tổ hợp](https://labuladong.online/algo/essential-technique/permutation-combination-subset-all-in-one/): bài hoán vị các phần tử không trùng lặp được chọn lặp lại, bài trước mình đã viết một hàm `permuteRepeat`, code như sau:

```java
class Solution {
    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> track = new LinkedList<>();

    // Hoán vị đầy đủ các phần tử không trùng lặp được chọn lặp lại
    public List<List<Integer>> permuteRepeat(int[] nums) {
        backtrack(nums);
        return res;
    }

    // Hàm cốt lõi của thuật toán quay lui
    void backtrack(int[] nums) {
        // base case, tới node lá
        if (track.size() == nums.length) {
            // Thu thập giá trị trên đường đi từ gốc tới node lá
            res.add(new LinkedList(track));
            return;
        }

        // Khung chuẩn của thuật toán quay lui
        for (int i = 0; i < nums.length; i++) {
            // Đưa ra lựa chọn
            track.add(nums[i]);
            // Đi vào tầng tiếp theo của cây quay lui
            backtrack(nums);
            // Hoàn tác lựa chọn
            track.removeLast();
        }
    }
}
```

Cho hàm này đầu vào `nums = [1,2,3]`, đầu ra là 3^3 = 27 tổ hợp có thể:




```java
[
  [1,1,1],[1,1,2],[1,1,3],[1,2,1],[1,2,2],[1,2,3],[1,3,1],[1,3,2],[1,3,3],
  [2,1,1],[2,1,2],[2,1,3],[2,2,1],[2,2,2],[2,2,3],[2,3,1],[2,3,2],[2,3,3],
  [3,1,1],[3,1,2],[3,1,3],[3,2,1],[3,2,2],[3,2,3],[3,3,1],[3,3,2],[3,3,3]
]
```



Đoạn code này thực chất chính là duyệt một cây `N`-chạc đầy chiều cao `N + 1` (`N` là độ dài `nums`), trong đó phần tử trên mỗi đường đi từ gốc tới lá chính là một kết quả hoán vị:

![](https://labuladong.online/algo/images/word-break/1.jpeg)

So sánh một chút, đề bài này trình bày cũng có nét tương đồng kỳ diệu, giả sử `wordDict = ["a", "aa", "ab"], s = "aaab"`, muốn dùng từ trong `wordDict` ghép thành `s`, thật ra cũng đối mặt một cây `M`-chạc tương tự, `M` là số lượng từ trong `wordDict`, **việc bạn cần làm chính là đứng tại mỗi node trên cây quay lui, xem từ nào khớp được tiền tố của `s[i..]`, từ đó phán đoán nên đi theo nhánh cây nào**:

![](https://labuladong.online/algo/images/word-break/2.jpeg)

Rồi, theo bài trước [Giải thích chi tiết khung thuật toán quay lui](https://labuladong.online/algo/essential-technique/backtrack-framework/) đã nói, bạn hiểu hàm `backtrack` như một con trỏ đi lại trên cây quay lui, duy trì biến `i` trên mỗi node là duyệt được cả cây quay lui để tìm ra tổ hợp khớp `s`.

Code cách giải quay lui như sau:

```java
class Solution {
    List<String> wordDict;
    // Ghi lại đã tìm được một đáp án hợp lệ hay chưa
    boolean found = false;
    // Ghi lại đường đi của thuật toán quay lui
    LinkedList<String> track = new LinkedList<>();

    // Hàm chính
    public boolean wordBreak(String s, List<String> wordDict) {
        this.wordDict = wordDict;
        // Chạy thuật toán quay lui liệt kê mọi tổ hợp có thể
        backtrack(s, 0);
        return found;
    }

    // Khung thuật toán quay lui
    void backtrack(String s, int i) {
        // base case
        if (found) {
            // Nếu đã tìm được đáp án thì không đệ quy tìm nữa
            return;
        }
        if (i == s.length()) {
            // Cả chuỗi s đã khớp xong, tìm được một đáp án hợp lệ
            found = true;
            return;
        }

        // Khung thuật toán quay lui
        for (String word : wordDict) {
            // Xem từ nào khớp được tiền tố của s[i..]
            int len = word.length();
            if (i + len <= s.length()
                && s.substring(i, i + len).equals(word)) {
                // Tìm được một từ khớp s[i..i+len)
                // Đưa ra lựa chọn
                track.addLast(word);
                // Đi vào tầng tiếp theo của cây quay lui, tiếp tục khớp s[i+len..]
                backtrack(s, i + len);
                // Hoàn tác lựa chọn
                track.removeLast();
            }
        }
    }
}
```

Đoạn code này viết nghiêm ngặt theo khung quay lui, hẳn không khó hiểu, nhưng đoạn code này không qua được mọi test case, ta phân tích độ phức tạp thời gian của nó theo phương pháp đã nói trong [Hướng dẫn thực dụng phân tích độ phức tạp thời-không gian](https://labuladong.online/algo/essential-technique/complexity-analysis/).

Cách ước lượng thô độ phức tạp thời gian của hàm đệ quy là lấy số lần gọi hàm đệ quy (số node trên cây đệ quy) x độ phức tạp của bản thân hàm đệ quy. Với đề này, mỗi node trên cây đệ quy thật ra chính là một lần cắt chuỗi `s`, vậy trong trường hợp xấu nhất `s` có bao nhiêu cách cắt? Trong chuỗi `s` độ dài `N` có tổng cộng `N - 1` khe để cắt, mỗi khe chọn「cắt」hoặc「không cắt」, nên `s` nhiều nhất có $O(2^N)$ cách cắt, tức trên cây đệ quy nhiều nhất có $O(2^N)$ node.

Đương nhiên, tình hình thực tế chắc chắn sẽ tốt hơn một chút, dù sao cũng có logic cắt tỉa, nhưng xét từ góc độ độ phức tạp xấu nhất thì số node trên cây đệ quy quả thật ở cấp số mũ.

Vậy độ phức tạp thời gian của bản thân hàm `backtrack` là bao nhiêu? Thời gian tốn chủ yếu là duyệt `wordDict` tìm từ khớp tiền tố của `s[i..]`:

```java
// Duyệt mọi từ của wordDict
for (String word : wordDict) {
    // Xem từ nào khớp được tiền tố của s[i..]
    int len = word.length();
    if (i + len <= s.length()
        && s.substring(i, i + len).equals(word)) {
        // Tìm được một từ khớp s[i..i+len)
        // ...
    }
}
```

Đặt độ dài `wordDict` là `M`, độ dài chuỗi `s` là `N`, vậy độ phức tạp thời gian xấu nhất của đoạn code này là $O(MN)$ (vòng for $O(M)$, phương thức `substring` của Java $O(N)$), nên tổng độ phức tạp thời gian là $O(2^N * MN)$.

Tiện đây nói một tối ưu chi tiết, thật ra bạn cũng có thể làm ngược lại, liệt kê tiền tố của `s[i..]` để kiểm tra trong `wordDict` có từ tương ứng không:

```java
// Chú ý, chuyển thành tập băm để nâng hiệu quả của phương thức contains
HashSet<String> wordDict = new HashSet<>(wordDict);

// Duyệt mọi tiền tố của s[i..]
for (int len = 1; i + len <= s.length(); len++) {
    // Xem trong wordDict có từ nào khớp được tiền tố của s[i..] không
    String prefix = s.substring(i, i + len);
    if (wordDict.contains(prefix)) {
        // Tìm được một từ khớp s[i..i+len)
        // ...
    }
}
```

Đoạn code này cho kết quả như đoạn code vừa rồi, nhưng độ phức tạp thời gian của đoạn code này trở thành $O(N^2)$, khác với đoạn code vừa rồi.

Rốt cuộc cách nào tốt hơn? Điều này phụ thuộc phạm vi dữ liệu đề cho. Đề này cho biết `1 <= s.length <= 300, 1 <= wordDict.length <= 1000`, nên kết quả $O(N^2)$ nhỏ hơn, hiệu quả chạy thực tế của đoạn code này hẳn cao hơn một chút, đây là một tối ưu chi tiết, bạn có thể tự làm thử, mình không viết nữa.

Nhưng dù bạn tối ưu đoạn code này, tổng độ phức tạp thời gian vẫn ở cấp số mũ $O(2^N * N^2)$, không qua được mọi test case, vậy vấn đề nằm ở đâu?

Ví dụ đầu vào `wordDict = ["a", "aa", "b"], s = "aaab"`, bạn chú ý khi quay lui liệt kê sẽ có tình huống lặp:

![](https://labuladong.online/algo/images/word-break/3.jpeg)

Hai phần đánh dấu đỏ trong hình, tuy trải qua các cách cắt khác nhau nhưng kết quả cắt ra lại giống nhau là `"aab"`, nên cây con dưới hai node này cũng lặp lại, tức tồn tại tính toán dư thừa, đây cũng là nguyên nhân độ phức tạp của thuật toán này ở cấp số mũ.

### Tối ưu bằng vị trí hậu thứ

Dù là thuật toán nào, cách loại bỏ tính toán dư thừa chính là thêm bảng ghi nhớ. Quay lui cũng có thể thêm bảng ghi nhớ, ta có thể gọi là「cắt tỉa」, tức cắt bỏ cây con dư thừa.

Ví dụ đối mặt cục diện chuỗi con `"aab"` này, mình muốn bảng ghi nhớ cho biết `"aab"` này rốt cuộc có cắt thành công được không? Nếu trước đó đã thử mà không cắt được thì mình bỏ qua luôn, không cần duyệt cây con để liệt kê cách cắt nữa, từ đó tối ưu hiệu quả. Nếu trước đó đã thử mà cắt thành công thì cũng chẳng liên quan gì tới bảng ghi nhớ nữa, vì bản thân `found == true` đã là base case, toàn bộ đệ quy sẽ dừng.

Đúng như phân tích về vị trí tiền thứ/hậu thứ trong [Tâm pháp chung dòng thuật toán cây nhị phân/đệ quy](https://labuladong.online/algo/essential-technique/binary-tree-summary/), muốn bảng ghi nhớ làm được điều này cần cập nhật bảng ghi nhớ tại vị trí hậu thứ, vì `"aab"` này thật ra là một cây con, đúng không? **Bạn cần khi duyệt xong cây con thì ghi vào bảng ghi nhớ xem cây con đó có cắt thành công được không**.

Hàm quay lui `backtrack` sinh ra để duyệt nên bản thân không có giá trị trả về, tức không có thông tin truyền về từ cây con. Nhưng với đề này ta vẫn có cách, vì chẳng phải có biến ngoài `found` sao? Biến này cho ta biết cây con có cắt thành công được không:

**Nếu `found` là false, tức là chưa tìm được một cách cắt thành công, cũng gián tiếp cho thấy cây con hiện tại không cắt thành công được**. Lúc này ta có thể ghi một dòng vào bảng ghi nhớ để loại bỏ liệt kê dư thừa.

Cụ thể vào code, chỉ cần sửa nhẹ là hiện thực được chức năng bảng ghi nhớ, để tiết kiệm độ dài mình chỉ đưa ra phần sửa:

```java
class Solution {
    // Bảng ghi nhớ, lưu chuỗi con (cây con) không cắt được để tránh tính toán lặp
    HashSet<String> memo = new HashSet<>();

    // ...

    void backtrack(String s, int i) {
        if (found) {
            return;
        }
        if (i == s.length()) {
            found = true;
            return;
        }

        // Logic cắt tỉa mới thêm, tra xem chuỗi con (cây con) đã tính chưa
        String suffix = s.substring(i);
        if (memo.contains(suffix)) {
            // Chuỗi con (cây con) hiện tại không cắt được thì không cần đệ quy nữa
            return;
        }

        for (String word : wordDict) {
            // ...
        }

        // Vị trí hậu thứ, ghi chuỗi con (cây con) không cắt được vào bảng ghi nhớ
        if (!found) {
            memo.add(suffix);
        }
    }
}
```

### Ý tưởng phân rã bài toán (quy hoạch động)

Bài trên giải được bằng quay lui, nói cho cùng vẫn vì đề này khá đơn giản, ta có thể nhờ biến `found` để cập nhật bảng ghi nhớ tại vị trí hậu thứ, bạn sẽ thấy bài Tách từ II nói sau không thể làm vậy.

Muốn cập nhật bảng ghi nhớ lưu đáp án cây con tại vị trí hậu thứ, thông thường vẫn phải nhờ giá trị trả về của hàm đệ quy, nên vẫn phải dùng lối tư duy phân rã bài toán.

Vừa rồi ta suy nghĩ bài này dưới góc độ hoán vị tổ hợp, giờ ta đổi góc nhìn, suy nghĩ xem có thể phân rã bài gốc thành bài toán con quy mô nhỏ hơn mà cấu trúc giống nhau không, rồi qua kết quả bài toán con tính ra kết quả bài gốc.

Với chuỗi `s` đầu vào, nếu mình tìm được một từ trong danh sách từ `wordDict` khớp tiền tố `s[0..k]` của `s`, vậy chỉ cần mình ghép được `s[k+1..]` thì chắc chắn ghép được cả `s`. Nói cách khác, mình đã phân rã bài gốc quy mô lớn `wordBreak(s[0..])` thành bài toán con quy mô nhỏ `wordBreak(s[k+1..])`, rồi qua nghiệm bài toán con suy ngược ra nghiệm bài gốc.

Có ý tưởng này là định nghĩa được một hàm `dp` và đưa ra định nghĩa của hàm đó:

```java
// Định nghĩa: trả về s[i..] có ghép được không
int dp(String s, int i);

// Tính cả chuỗi s có ghép được không thì gọi dp(s, 0)
```

Có định nghĩa hàm này là chuyển được logic vừa rồi thành mã giả một cách đại khái:

```java
List<String> wordDict;

// Định nghĩa: trả về s[i..] có ghép được không
int dp(String s, int i) {
    // base case, s[i..] là chuỗi rỗng
    if (i == s.length()) {
        return true;
    }
    // Duyệt wordDict, xem những từ nào là tiền tố của s[i..]
    for (Strnig word : wordDict) {
        // word là tiền tố của s[i..]
        if (s.substring(i).startsWith(word)) {
            int len = word.length();
            // Chỉ cần s[i+len..] ghép được là s[i..] ghép được
            if (dp(s, i + len) == true) {
                return true;
            }
        }
    }
    // Mọi từ đều đã thử qua, không ghép được cả chuỗi s
    return false;
}
```

Tương tự thuật toán quay lui đã nói, vòng for trong hàm `dp` cũng có thể tối ưu một chút:

```java
// Chú ý, dùng tập băm để kiểm tra tồn tại nhanh
HashSet<String> wordDict;

// Định nghĩa: trả về s[i..] có ghép được không
int dp(String s, int i) {
    // base case, s[i..] là chuỗi rỗng
    if (i == s.length()) {
        return true;
    }

    // Duyệt mọi tiền tố của s[i..], xem những tiền tố nào tồn tại trong wordDict
    for (int len = 1; i + len <= s.length(); len++) {
        // Trong wordDict tồn tại s[i..len)
        if (wordDict.contains(s.substring(i, i + len))) {
            // Chỉ cần s[i+len..] ghép được là s[i..] ghép được
            if (dp(s, i + len) == true) {
                return true;
            }
        }
    }
    // Mọi từ đều đã thử qua, không ghép được cả chuỗi s
    return false;
}
```

Với hàm `dp` này, vị trí con trỏ `i` chính là「trạng thái」, nên ta có thể tối ưu hiệu quả bằng cách thêm bảng ghi nhớ để tránh tính toán dư thừa cho bài toán con giống nhau. Code lời giải cuối cùng như sau:

```java
class Solution {
    // Dùng tập băm để tiện kiểm tra tồn tại nhanh
    HashSet<String> wordDict;
    // Bảng ghi nhớ, -1 nghĩa là chưa tính, 0 nghĩa là không ghép được, 1 nghĩa là ghép được
    int[] memo;

    // Hàm chính
    public boolean wordBreak(String s, List<String> wordDict) {
        // Chuyển thành tập băm để kiểm tra phần tử tồn tại nhanh
        this.wordDict = new HashSet<>(wordDict);
        // Bảng ghi nhớ khởi tạo là -1
        this.memo = new int[s.length()];
        Arrays.fill(memo, -1);
        return dp(s, 0);
    }

    // Định nghĩa: s[i..] có ghép được không
    boolean dp(String s, int i) {
        // base case
        if (i == s.length()) {
            return true;
        }
        // Tránh tính toán dư thừa
        if (memo[i] != -1) {
            return memo[i] == 0 ? false : true;
        }

        // Duyệt mọi tiền tố của s[i..]
        for (int len = 1; i + len <= s.length(); len++) {
            // Xem những tiền tố nào tồn tại trong wordDict
            String prefix = s.substring(i, i + len);
            if (wordDict.contains(prefix)) {
                // Tìm được một từ khớp s[i..i+len)
                // Chỉ cần s[i+len..] ghép được là s[i..] ghép được
                boolean subProblem = dp(s, i + len);
                if (subProblem == true) {
                    memo[i] = 1;
                    return true;
                }
            }
        }
        // s[i..] không ghép được
        memo[i] = 0;
        return false;
    }
}
```

> [!TIP]
> Chú ý trong quá trình tính `prefix`, ta gọi thẳng hàm cắt chuỗi con mà ngôn ngữ lập trình cung cấp, độ phức tạp thời gian của hàm này là $O(N)$. Không khó phát hiện chỉ số bắt đầu khi cắt chuỗi con cố định là `i`, chỉ số kết thúc tăng dần là `j`, nên ta tự duy trì chuỗi con `prefix` này để tránh gọi hàm cắt chuỗi con, nâng hiệu quả thêm một bước. Tối ưu nhỏ này để bạn tự làm nhé.


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/word-break/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 Animation trực quan hóa code 🌈</strong>
</summary>
</details>
</a>
<hr/>

Cách giải này qua được mọi test case, ta tính độ phức tạp thời gian của nó theo [Hướng dẫn thực dụng phân tích độ phức tạp thời-không gian](https://labuladong.online/algo/essential-technique/complexity-analysis/):

Nhờ có bảng ghi nhớ hỗ trợ, loại bỏ node lặp trên cây đệ quy khiến số lần gọi hàm đệ quy giảm từ cấp số mũ xuống còn số lượng trạng thái $O(N)$, độ phức tạp bản thân hàm vẫn là $O(N^2)$, nên tổng độ phức tạp thời gian là $O(N^3)$, hiệu quả tăng mạnh so với thuật toán quay lui.

## Tách từ II

Có bài trước làm nền, bài 140「Tách từ II」trên LeetCode dễ hơn nhiều, xem đề trước:

<Problem slug="word-break-ii" />

So với bài trước, bài này không chỉ hỏi `s` có ghép được không mà còn hỏi bạn ghép như thế nào, thật ra chỉ cần sửa nhẹ cách giải trước là giải được bài này.

### Ý tưởng duyệt (thuật toán quay lui)

Thuật toán quay lui ở bài trước duy trì một biến `found`, chỉ cần tìm được một phương án ghép là kết thúc sớm việc duyệt cây quay lui, vậy ở bài này ta không kết thúc sớm việc duyệt mà thu thập mọi phương án ghép khả thi là ra đáp án:

```java
class Solution {
    // Ghi lại kết quả
    List<String> res = new LinkedList<>();
    // Ghi lại đường đi của thuật toán quay lui
    LinkedList<String> track = new LinkedList<>();
    List<String> wordDict;

    // Hàm chính
    public List<String> wordBreak(String s, List<String> wordDict) {
        this.wordDict = wordDict;
        // Chạy thuật toán quay lui liệt kê mọi tổ hợp có thể
        backtrack(s, 0);
        return res;
    }

    // Khung thuật toán quay lui
    void backtrack(String s, int i) {
        // base case
        if (i == s.length()) {
            // Tìm được một tổ hợp hợp lệ ghép thành cả s, chuyển thành chuỗi
            res.add(String.join(" ", track));
            return;
        }

        // Khung thuật toán quay lui
        for (String word : wordDict) {
            // Xem từ nào khớp được tiền tố của s[i..]
            int len = word.length();
            if (i + len <= s.length()
                && s.substring(i, i + len).equals(word)) {
                // Tìm được một từ khớp s[i..i+len)
                // Đưa ra lựa chọn
                track.addLast(word);
                // Đi vào tầng tiếp theo của cây quay lui, tiếp tục khớp s[i+len..]
                backtrack(s, i + len);
                // Hoàn tác lựa chọn
                track.removeLast();
            }
        }
    }
}
```

Độ phức tạp thời gian của cách giải này tương tự bài trước, vẫn là $O(2^N * MN)$, nhưng vì dữ liệu bài này cho quy mô nhỏ hơn nên qua được mọi test case.

### Có thể tối ưu bằng vị trí hậu thứ không?

Tương tự trước đó, cách giải này vẫn còn dư địa tối ưu, vẫn là tình huống này:

![](https://labuladong.online/algo/images/word-break/3.jpeg)

Với cây con lặp lại, vẫn gây duyệt lặp không cần thiết, ta vẫn có thể tối ưu bằng cách dùng bảng ghi nhớ, tức có thể lưu kết quả cắt của chuỗi con `"aab"` vào bảng ghi nhớ để tránh duyệt lặp cây con giống nhau.

Nhưng dùng quay lui thì khó thêm bảng ghi nhớ, vì biến `track` của quay lui chỉ duy trì đường đi từ node gốc tới node hiện tại chứ không ghi thông tin cây con.

Nên dạng đề này muốn loại bỏ bài toán con chồng lặp thì thường phải dùng lối tư duy phân rã bài toán, dùng giá trị trả về của hàm để cập nhật bảng ghi nhớ.

### Ý tưởng phân rã bài toán (quy hoạch động)

Bài này cũng giải được bằng tư duy phân rã bài toán, chỉ cần sửa nhẹ hàm `dp` của bài trước:

```java
class Solution {
    HashSet<String> wordDict;
    // Bảng ghi nhớ
    List<String>[] memo;

    public List<String> wordBreak(String s, List<String> wordDict) {
        this.wordDict = new HashSet<>(wordDict);
        memo = new List[s.length()];
        return dp(s, 0);
    }

    // Định nghĩa: trả về mọi khả năng dùng wordDict tạo thành s[i..]
    List<String> dp(String s, int i) {
        List<String> res = new LinkedList<>();
        if (i == s.length()) {
            res.add("");
            return res;
        }
        // Tránh tính toán dư thừa
        if (memo[i] != null) {
            return memo[i];
        }

        // Duyệt mọi tiền tố của s[i..]
        for (int len = 1; i + len <= s.length(); len++) {
            // Xem những tiền tố nào tồn tại trong wordDict
            String prefix = s.substring(i, i + len);
            if (wordDict.contains(prefix)) {
                // Tìm được một từ khớp s[i..i+len)
                List<String> subProblem = dp(s, i + len);
                // Mọi tổ hợp tạo thành s[i+len..] cộng thêm prefix
                // Chính là mọi tổ hợp tạo thành s[i..]
                for (String sub : subProblem) {
                    if (sub.isEmpty()) {
                        // Tránh dấu cách thừa
                        res.add(prefix);
                    } else {
                        res.add(prefix + " " + sub);
                    }
                }
            }
        }
        // Lưu vào bảng ghi nhớ
        memo[i] = res;

        return res;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/word-break-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 Animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>

Cách giải này vẫn dùng bảng ghi nhớ loại bỏ bài toán con chồng lặp nên số lần gọi đệ quy hàm `dp` giảm xuống còn $O(N)$, nhưng độ phức tạp thời gian bản thân hàm `dp` lại tăng, vì `subProblem` là một danh sách tập con, độ dài của nó ở cấp số mũ.

Cộng thêm hiệu quả nối chuỗi vốn không cao, mà còn tốn bảng ghi nhớ để lưu kết quả mọi bài toán con, nên phân tích từ góc độ Big O thì độ phức tạp thời gian của thuật toán này không thấp hơn quay lui, vẫn ở cấp số mũ; nhưng cách giải này quả thật đã loại bỏ bài toán con chồng lặp nên cao tay hơn quay lui một chút.

Tóm lại, khi xử lý bài hoán vị tổ hợp ta thường dùng quay lui để duyệt cây quay lui chứ không dùng lối tư duy phân rã bài toán, vì lưu kết quả bài toán con đã cần nhiều thời gian và không gian, trừ phi gặp trường hợp cực đoan có nhiều bài toán con chồng lặp, nếu không sẽ lợi bất cập hại.

Trên đây là toàn bộ nội dung bài này, hy vọng bạn hiểu sâu hơn về lối tư duy quay lui và phân rã bài toán.




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
