# quay lui thuật toán xử gọn mọi bài toán hoán vị/tổ hợp/tập con



![](https://labuladong.online/algo/images/souyisou1.png)

** thông báo: để đáp ứng nhu cầu của đông đảo độc giả, website đã mở [ lộ trình học cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người ~ ngoài ra, bạn nên học bài viết trên [ website ](https://labuladong.online/algo/), để có trải nghiệm tốt hơn.**



 đọc xong bài này, bạn không chỉ học sẽ thuật toán khuôn mẫu, còn có thể xuôi thì giải quyết như sau đề bài:

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/) | [216. tổng tổ hợp III](https://leetcode.cn/problems/combination-sum-iii/) | 🟠 |
| [39. Combination Sum](https://leetcode.com/problems/combination-sum/) | [39. tổng tổ hợp ](https://leetcode.cn/problems/combination-sum/) | 🟠 |
| [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) | [40. tổng tổ hợp II](https://leetcode.cn/problems/combination-sum-ii/) | 🟠 |
| [46. Permutations](https://leetcode.com/problems/permutations/) | [46. hoán vị đầy đủ ](https://leetcode.cn/problems/permutations/) | 🟠 |
| [47. Permutations II](https://leetcode.com/problems/permutations-ii/) | [47. hoán vị đầy đủ II](https://leetcode.cn/problems/permutations-ii/) | 🟠 |
| [77. Combinations](https://leetcode.com/problems/combinations/) | [77. tổ hợp ](https://leetcode.cn/problems/combinations/) | 🟠 |
| [78. Subsets](https://leetcode.com/problems/subsets/) | [78. tập con ](https://leetcode.cn/problems/subsets/) | 🟠 |
| [90. Subsets II](https://leetcode.com/problems/subsets-ii/) | [90. tập con II](https://leetcode.cn/problems/subsets-ii/) | 🟠 |
| - | [Kiếm chỉ Offer II 082. tổ hợp của tập hợp có phần tử trùng lặp ](https://leetcode.cn/problems/4sjJUc/) | 🟠 |
| - | [Kiếm chỉ Offer II 084. hoán vị đầy đủ của tập hợp có phần tử trùng lặp ](https://leetcode.cn/problems/7p8L0Z/) | 🟠 |

**-----------**



> [!NOTE]
> trước khi đọc bài này, bạn cần học trước:
>
> - [ thuật toán series cây nhị phân (cương lĩnh nhận bài) ](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
> - [ quay lui thuật toán cốt lõi khung ](https://labuladong.online/algo/essential-technique/backtrack-framework/)

> tip: bài này có bản video: [ quay lui thuật toán xử gọn mọi bài toán hoán vị/tổ hợp/tập con ](https://www.bilibili.com/video/BV1Yt4y1t7dK/). bạn nên theo dõi tài khoản Bilibili của mình, mình sẽ dùng video dẫn đọc để cùng mọi người học những kỹ thuật thuật toán hơi khó.



 mặc dù các bài toán hoán vị, tổ hợp, tập con đã học từ cấp ba, nhưng nếu muốn viết code thuật toán giải quyết nó, vẫn rất thử thách tư duy máy tính. Bài viết này sẽ giảng cách viết code giải quyết ý tưởng cốt lõi của mấy vấn đề này, sau này có biến thể gì, bạn cũng có thể dễ dàng xử lý, lấy bất biến ứng vạn biến.

Dù là bài toán hoán vị, tổ hợp hay tập con, nói đơn giản chẳng qua là để bạn lấy một số phần tử từ dãy `nums` theo quy tắc cho trước, chủ yếu có mấy loại biến thể dưới đây:

** Hình thức một: phần tử không trùng lặp và không được chọn lặp, tức là mỗi phần tử trong `nums` nhiều nhất chỉ được sử dụng một lần, đây cũng là hình thức cơ bản nhất **.

Lấy tổ hợp làm ví dụ, nếu truyền vào `nums = [2,3,6,7]`, và tổng bằng 7 thì chỉ có `[7]`.

** Hình thức hai: phần tử có thể trùng lặp nhưng không được chọn lặp, tức là `nums` có thể lưu trữ phần tử trùng lặp, mỗi phần tử nhiều nhất chỉ được sử dụng một lần **.

Lấy tổ hợp làm ví dụ, nếu truyền vào `nums = [2,5,2,1,2]`, và tổng bằng 7 thì có hai loại `[2,2,2,1]` và `[5,2]`.

** hình thức ba, phần tử không trùng lặp nhưng được chọn lặp, tức là `nums` các phần tử trong, mỗi có thể được sử dụng một số lần **.

Lấy tổ hợp làm ví dụ, nếu truyền vào `nums = [2,3,6,7]`, và tổng bằng 7 thì có hai loại `[2,2,3]` và `[7]`.

 đương nhiên, cũng có thể nói có thứ bốn loại hình thức, tức là phần tử vừa trùng lặp vừa được chọn lặp. nhưng đã có thể chọn lặp, thì việc lưu trữ trùng lặp còn cần thiết gì? sau khi khử trùng lặp thì tương đương với hình thức ba, vì vậy trường hợp này không cần xét.

 trên mặt ví dụ lấy từ bài toán tổ hợp, nhưng các bài toán hoán vị, tổ hợp, tập con đều có thể có ba loại hình thức cơ bản này, vì vậy tổng cộng có 9 biến thể.







 ngoài ra, đề bài cũng có thể thêm các điều kiện giới hạn khác, ví dụ bắt bạn tính tổng bằng `target` và số lượng bằng `k`, thì như vậy lại có thể phái sinh ra một loạt biến thể, chẳng trách phỏng vấn bài kiểm tra trong thường xuyên thi tới hoán vị tổ hợp loại này dạng bài cơ bản.

** nhưng dù hình thức thay đổi thế nào, bản chất chính là vét cạn mọi nghiệm, mà những nghiệm này biểu hiện thành cấu trúc cây, vì vậy vận dụng hợp lý khung thuật toán quay lui, chỉ cần sửa nhẹ khung code là quét sạch các bài này **.

 cụ thể mà nói, bạn cần đọc và hiểu phần trước [ quay lui thuật toán cốt lõi khuôn mẫu ](https://labuladong.online/algo/essential-technique/backtrack-framework/), sau đó hãy nhớ cây quay lui của bài toán tập con và bài toán hoán vị dưới đây, là có thể giải mọi bài toán liên quan đến hoán vị/tổ hợp/tập con:

![](https://labuladong.online/algo/images/permutation/1.jpeg)

![](https://labuladong.online/algo/images/permutation/2.jpeg)

 tại sao chỉ cần nhớ hai cấu trúc cây này là giải được mọi bài liên quan?

** đầu tiên, tổ hợp vấn đề và tập con vấn đề thực ra là tương đương nhau, phần sau sẽ giảng; còn ba loại biến thể nói trước đây, chẳng qua là cắt bớt hoặc thêm một số cành trên hai cây này mà thôi **.

 như vậy, tiếp theo chúng ta bắt đầu vét cạn, duyệt qua cả 9 dạng của bài toán hoán vị/tổ hợp/tập con, học học làm sao dùng quay lui thuật toán nó xử gọn một lượt.

> [!NOTE]
> ngoài ra, một số độc giả code lời giải hoán vị/tập con/tổ hợp bạn từng xem có thể khác với code mình giới thiệu trong bài này. vì thuật toán quay lui có hai góc nhìn vét cạn, tôi sẽ ở phần sau [ mô hình bóng-hộp: quay lui thuật toán hai góc nhìn của vét cạn ](https://labuladong.online/algo/practice-in-action/two-views-of-backtrack/) giảng rõ cho bạn từng bước. hiện tại chưa thích hợp để giảng ngay những lời giải đó cho bạn, bạn cứ học theo mạch suy nghĩ của mình là được.







## tập con (phần tử không trùng lặp và không được chọn lặp)

 bài 78 bài " tập con " chính là bài này:

 đề bài cho bạn đầu vào một không có phần tử trùng lặp mảng `nums`, trong đó mỗi phần tử được dùng nhiều nhất một lần, hãy trả về `nums`.

 chữ ký hàm như sau:

```java
List<List<Integer>> subsets(int[] nums)
```

 ví dụ đầu vào `nums = [1,2,3]`, thuật toán nên trả về các tập con như sau:

```java
[ [],[1],[2],[3],[1,2],[1,3],[2,3],[1,2,3] ]
```

 tốt, tôi tạm thời chưa xét cách cài đặt bằng code, hãy nhớ lại kiến thức cấp ba của chúng ta, cách suy ra mọi tập con bằng tay?

 đầu tiên, sinh ra các tập con có số phần tử là 0, tức là tập rỗng `[]`, để tiện biểu diễn, mình gọi nó là `S_0`.

 sau đó, trên cơ sở `S_0` sinh ra các tập con có số phần tử là 1, tôi ký hiệu là `S_1`:

![](https://labuladong.online/algo/images/permutation/3.jpeg)

 tiếp theo, tôi có thể trên cơ sở `S_1` suy luận ra `S_2`, tức là các tập con có 2 phần tử:

![](https://labuladong.online/algo/images/permutation/4.jpeg)

 tại sao tập hợp `[2]` chỉ cần thêm `3`, mà không thêm các phần tử phía trước `1`?

 vì các phần tử trong tập hợp không cần xét thứ tự, `[1,2,3]` trong `2` sau mặt chỉ có `3`, nếu bạn thêm `1`, như vậy `[2,1]` sẽ trùng với tập con đã sinh ra trước đó `[1,2]` trùng lặp.

** nói cách khác, chúng ta giữ nguyên thứ tự tương đối giữa các phần tử để tránh xuất hiện tập con trùng lặp **.

 tiếp theo, tôi có thể thông qua `S_2` đưa ra `S_3`, trên thực tế `S_3` chỉ có một tập hợp `[1,2,3]`, nó được suy ra thông qua `[1,2]` mà ra.

 toàn bộ quá trình suy luận chính là một cây như vậy:

![](https://labuladong.online/algo/images/permutation/5.jpeg)

 chú ý đặc tính của cây này:

** nếu coi node gốc là tầng 0, lấy các phần tử trên cành nối mỗi node với node gốc làm giá trị của node đó, thì mọi node ở tầng `n` chính là mọi tập con có kích thước `n` **.

 ví dụ như các tập con có kích thước 2 chính là giá trị của các node ở tầng này:

![](https://labuladong.online/algo/images/permutation/6.jpeg)

> [!NOTE]
> ** chú ý, từ đây về sau trong bài này, cụm từ " giá trị của node " đều chỉ các phần tử trên cành nối node với node gốc, và coi node gốc là tầng 0 **.

 tiến thêm một bước nữa, nếu muốn tính mọi tập con, thì chỉ cần duyệt cây đa phân này, thu thập giá trị của mọi node là xong?

 xem trực tiếp code:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    // ghi lại đường đi đệ quy của thuật toán quay lui 
    LinkedList<Integer> track = new LinkedList<>();

    // hàm chính 
    public List<List<Integer>> subsets(int[] nums) {
        backtrack(nums, 0);
        return res;
    }

    // hàm cốt lõi của thuật toán quay lui, duyệt cây quay lui của bài toán tập con 
    void backtrack(int[] nums, int start) {

        // vị trí tiền thứ tự, giá trị của mỗi node đều là một tập con 
        res.add(new LinkedList<>(track));
        
        // khung chuẩn của thuật toán quay lui 
        for (int i = start; i < nums.length; i++) {
            // đưa ra lựa chọn 
            track.addLast(nums[i]);
            // dùng tham số start điều khiển việc duyệt cành cây, tránh sinh ra trùng lặp   
            backtrack(nums, i + 1);
            // rút lại lựa chọn 
            track.removeLast();
        }
    }
}
```

 độc giả đã đọc phần trước [ quay lui thuật toán cốt lõi khung ](https://labuladong.online/algo/essential-technique/backtrack-framework/) độc giả chắc sẽ dễ dàng hiểu đoạn code này, chúng ta dùng `start` tham số để kiểm soát sự phát triển của cành cây nhằm tránh sinh tập con trùng lặp, dùng `track` ghi lại giá trị đường đi từ node gốc tới mỗi node, đồng thời thu thập giá trị đường đi của mỗi node tại vị trí tiền thứ tự, duyệt xong cây quay lui là thu thập được mọi tập con:

![](https://labuladong.online/algo/images/permutation/5.jpeg)

 cuối cùng, `backtrack` phần đầu hàm dường như không có base case, liệu có rơi vào đệ quy vô hạn không?

 thực ra không đâu, khi `start == nums.length` giờ, giá trị của node lá sẽ được đưa vào `res`, nhưng vòng lặp for sẽ không chạy, vậy là kết thúc đệ quy.






<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/subsets/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 animation trực quan hóa code 🍭</strong>
</summary>
</details>
</a>
<hr/>



## tổ hợp (phần tử không trùng lặp và không được chọn lặp)

 nếu bạn đã sinh thành công mọi tập con không trùng lặp, thì chỉ cần sửa nhẹ code là sinh được mọi tổ hợp không trùng lặp.

 ví dụ như, bắt bạn trong `nums = [1,2,3]` lấy ra 2 phần tử để tạo thành mọi tổ hợp, bạn làm thế nào?

 nghĩ một chút sẽ thấy, kích thước cho 2 mà tổng các số bằng số mục tiêu, chẳng phải chính là mọi tập con có kích thước 2 hay sao.

** vì vậy mình nói tổ hợp và tập con là một: kích thước cho `k` chính là tập con có kích thước `k` **.

 ví dụ bài 77 bài " tổ hợp ":

 cho hai số nguyên `n` và `k`, trả về mọi tổ hợp `[1, n]` có thể có trong phạm vi `k`.

 chữ ký hàm như sau:

```java
List<List<Integer>> combine(int n, int k)
```

 ví dụ `combine(3, 2)` giá trị trả về nên là:

```java
[ [1,2],[1,3],[2,3] ]
```

 đây là bài toán tổ hợp chuẩn, nhưng mình diễn đạt lại một chút là thành bài toán tập con:

** cho bạn đầu vào một mảng `nums = [1,2..,n]` và một số nguyên dương `k`, hãy sinh ra mọi tập con có kích thước `k` **.

 vẫn lấy `nums = [1,2,3]` làm ví dụ, lúc nãy bắt bạn tìm mọi tập con, chính là thu thập giá trị của mọi node; ** giờ bạn chỉ cần thu thập các node ở tầng 2 (coi node gốc là tầng 0) là xong, chính là mọi tổ hợp có kích thước 2 **:

![](https://labuladong.online/algo/images/permutation/6.jpeg)

 phản ánh vào code, chỉ cần sửa nhẹ base case, để thuật toán chỉ thu thập các node ở tầng `k` là được:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    // ghi lại đường đi đệ quy của thuật toán quay lui 
    LinkedList<Integer> track = new LinkedList<>();

    // hàm chính 
    public List<List<Integer>> combine(int n, int k) {
        backtrack(1, n, k);
        return res;
    }

    void backtrack(int start, int n, int k) {
        // base case
        if (k == track.size()) {
            // duyệt tới tầng thứ k, thu thập giá trị của node hiện tại 
            res.add(new LinkedList<>(track));
            return;
        }
        
        // khung chuẩn của thuật toán quay lui 
        for (int i = start; i <= n; i++) {
            // chọn 
            track.addLast(i);
            // dùng tham số start điều khiển việc duyệt cành cây, tránh sinh ra trùng lặp   
            backtrack(i + 1, n, k);
            // rút lại lựa chọn 
            track.removeLast();
        }
    }
}
```

 như vậy, bài toán tổ hợp chuẩn cũng được giải quyết.


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/combinations/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 animation trực quan hóa code 🌈</strong>
</summary>
</details>
</a>
<hr/>

## hoán vị (phần tử không trùng lặp và không được chọn lặp)

 bài toán hoán vị đã được giảng ở phần trước [ quay lui thuật toán cốt lõi khung ](https://labuladong.online/algo/essential-technique/backtrack-framework/) rồi, ở đây chỉ lướt nhanh qua.

 bài 46 bài " hoán vị đầy đủ " chính là bài toán hoán vị chuẩn:

 cho một mảng `nums` **không chứa phần tử trùng lặp**, trả về mọi **hoán vị đầy đủ** của nó.

 chữ ký hàm như sau:

```java
List<List<Integer>> permute(int[] nums)
```

 ví dụ đầu vào `nums = [1,2,3]`, giá trị trả về của hàm nên là:

```java
[
    [1,2,3],[1,3,2],
    [2,1,3],[2,3,1],
    [3,1,2],[3,2,1]
]
```



 bài toán tổ hợp/tập con vừa giảng dùng `start` biến để đảm bảo sau phần tử `nums[start]` chỉ xuất hiện các phần tử trong `nums[start+1..]`, bằng cách cố định vị trí tương đối của các phần tử để đảm bảo không xuất hiện tập con trùng lặp.

** nhưng bản thân bài toán hoán vị bắt bạn vét cạn vị trí của các phần tử, `nums[i]` về sau cũng có thể xuất hiện phần tử phía bên trái của `nums[i]`, nên bộ cách làm trước đó không còn dùng được, cần dùng thêm `used` mảng để đánh dấu những phần tử nào còn có thể được chọn **.

 hoán vị đầy đủ chuẩn có thể trừu tượng hóa thành cây đa phân như sau:

![](https://labuladong.online/algo/images/permutation/7.jpeg)

 chúng ta dùng `used` mảng đánh dấu các phần tử đã nằm trên đường đi để tránh chọn lặp, sau đó thu thập giá trị trên mọi node lá, chính là kết quả của mọi hoán vị đầy đủ:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    // ghi lại đường đi đệ quy của thuật toán quay lui 
    LinkedList<Integer> track = new LinkedList<>();
    // các phần tử trong track sẽ được đánh dấu true trong used
    boolean[] used;

    // hàm chính, truyền vào một nhóm số không trùng lặp, trả về mọi hoán vị đầy đủ của nó 
    public List<List<Integer>> permute(int[] nums) {
        used = new boolean[nums.length];
        backtrack(nums);
        return res;
    }

    // hàm cốt lõi của thuật toán quay lui 
    void backtrack(int[] nums) {
        // base case, đã tới node lá 
        if (track.size() == nums.length) {
            // thu thập giá trị trên node lá   
            res.add(new LinkedList(track));
            return;
        }

        // khung chuẩn của thuật toán quay lui 
        for (int i = 0; i < nums.length; i++) {
            // đã lưu trong track, không thể chọn trùng lặp 
            if (used[i]) {
                continue;
            }
            // đưa ra lựa chọn 
            used[i] = true;
            track.addLast(nums[i]);
            // tiến vào tầng tiếp theo của cây quay lui 
            backtrack(nums);
            // hủy bỏ lựa chọn 
            track.removeLast();
            used[i] = false;
        }
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/permutations/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



 như vậy, hoán vị đầy đủ vấn đề thì giải quyết.

 nhưng nếu đề bài không bắt bạn tính hoán vị đầy đủ, mà bắt bạn tính hoán vị có số phần tử là `k`, tính thế nào?

 cũng rất đơn giản, sửa lại `backtrack` base case của hàm, chỉ thu thập các node ở tầng `k` là được:

```java
// hàm cốt lõi của thuật toán quay lui 
void backtrack(int[] nums, int k) {
    // base case, đã tới tầng thứ k, thu thập giá trị của node 
    if (track.size() == k) {
        // giá trị của node ở tầng thứ k chính là tập con có kích thước k   
        res.add(new LinkedList(track));
        return;
    }

    // khung chuẩn của thuật toán quay lui 
    for (int i = 0; i < nums.length; i++) {
        // ...
        backtrack(nums, k);
        // ...
    }
}
```

## tập con/tổ hợp (phần tử có thể trùng lặp nhưng không được chọn lặp)

 đầu vào của bài toán tập con chuẩn vừa giảng là `nums` không có phần tử trùng lặp, nhưng nếu tồn tại phần tử trùng lặp, thì xử lý thế nào?

 bài 90 bài " tập con II" chính là một bài như vậy:

 cho bạn một mảng số nguyên `nums`, trong đó có thể chứa phần tử trùng lặp, hãy trả về mọi tập con có thể có của mảng đó.

 chữ ký hàm như sau:

```java
List<List<Integer>> subsetsWithDup(int[] nums)
```

 ví dụ đầu vào `nums = [1,2,2]`, bạn nên in ra:

```java
[ [],[1],[2],[1,2],[2,2],[1,2,2] ]
```

 đương nhiên, theo lý mà nói " tập hợp " không nên chứa phần tử trùng lặp, nhưng đã đề bài hỏi như vậy, chúng ta cứ bỏ qua chi tiết này, suy nghĩ kỹ xem bài này làm thế nào mới là việc chính.

 lấy `nums = [1,2,2]` làm ví dụ, để phân biệt hai phần tử `2` là các phần tử khác nhau, về sau chúng ta viết thành `nums = [1,2,2']`.

 vẽ cấu trúc cây của tập con theo mạch suy nghĩ trước đó, rõ ràng, hai cành kề nhau có cùng giá trị sẽ sinh ra trùng lặp:

![](https://labuladong.online/algo/images/permutation/8.jpeg)

```text
[ 
    [],
    [1],[2],[2'],
    [1,2],[1,2'],[2,2'],
    [1,2,2']
]
```

 bạn có thể thấy, `[2]` và `[1,2]` hai kết quả này xuất hiện trùng lặp, vì vậy chúng ta cần cắt tỉa, nếu một node có nhiều cành kề nhau mang giá trị giống nhau, thì chỉ duyệt cành đầu tiên, cắt bỏ các cành còn lại, đừng duyệt chúng:

![](https://labuladong.online/algo/images/permutation/9.jpeg)

** thể hiện vào code, cần sắp xếp trước, để các phần tử giống nhau đứng cạnh nhau, nếu phát hiện `nums[i] == nums[i-1]`, thì bỏ qua **:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> track = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        // sắp xếp trước để các phần tử giống nhau đứng cạnh nhau 
        Arrays.sort(nums);
        backtrack(nums, 0);
        return res;
    }

    void backtrack(int[] nums, int start) {
        // vị trí tiền thứ tự, giá trị của mỗi node đều là một tập con 
        res.add(new LinkedList<>(track));
        
        for (int i = start; i < nums.length; i++) {
            // logic cắt tỉa: các cành kề nhau có giá trị giống nhau thì chỉ duyệt cành đầu tiên 
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }
            track.addLast(nums[i]);
            backtrack(nums, i + 1);
            track.removeLast();
        }
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/subsets-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🌈 animation trực quan hóa code 🌈</strong>
</summary>
</details>
</a>
<hr/>



 đoạn code này gần như giống hệt code của bài toán tập con chuẩn trước đó, chỉ là thêm logic sắp xếp và cắt tỉa.

 còn về việc tại sao phải cắt tỉa như vậy, kết hợp với hình phía trước chắc cũng dễ hiểu, như vậy bài toán tập con có phần tử trùng lặp cũng được giải quyết.

** chúng ta đã nói bài toán tổ hợp và bài toán tập con là tương đương nhau **, nên chúng ta xem trực tiếp một bài toán tổ hợp nhé, đây là bài 40 " tổng tổ hợp II":

 cho bạn đầu vào `candidates` và một tổng mục tiêu `target`, từ `candidates` tìm ra mọi tổ hợp có tổng bằng `target`.

`candidates` có thể tồn tại phần tử trùng lặp, và mỗi số trong đó được dùng nhiều nhất một lần.

 nói đây là một bài toán tổ hợp, thực ra hỏi theo cách khác là thành bài toán tập con: hãy tính ra `candidates` mọi tập con trong `target`.

 vậy bài này làm thế nào?

 so với lời giải của bài toán tập con, chỉ cần dùng thêm một biến `trackSum` để ghi lại tổng các phần tử trên đường quay lui, sau đó sửa base case một chút là giải được bài này:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    // ghi lại đường đi quay lui 
    LinkedList<Integer> track = new LinkedList<>();
    // ghi lại tổng các phần tử trên đường đi trong track 
    int trackSum = 0;

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        if (candidates.length == 0) {
            return res;
        }
        // sắp xếp trước để các phần tử giống nhau đứng cạnh nhau 
        Arrays.sort(candidates);
        backtrack(candidates, 0, target);
        return res;
    }

    // hàm chính của thuật toán quay lui 
    void backtrack(int[] nums, int start, int target) {
        // base case, đạt tới tổng mục tiêu, tìm được nghiệm phù hợp điều kiện   
        if (trackSum == target) {
            res.add(new LinkedList<>(track));
            return;
        }
        // base case, vượt quá tổng mục tiêu, kết thúc trực tiếp 
        if (trackSum > target) {
            return;
        }

        // khung chuẩn của thuật toán quay lui 
        for (int i = start; i < nums.length; i++) {
            // logic cắt tỉa: các cành có giá trị giống nhau thì chỉ duyệt cành đầu tiên 
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }
            // đưa ra lựa chọn 
            track.add(nums[i]);
            trackSum += nums[i];
            // đệ quy duyệt tầng tiếp theo của cây quay lui 
            backtrack(nums, i + 1, target);
            // rút lại lựa chọn 
            track.removeLast();
            trackSum -= nums[i];
        }
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/combination-sum-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



## hoán vị (phần tử có thể trùng lặp nhưng không được chọn lặp)

 nếu đầu vào của bài toán hoán vị tồn tại trùng lặp, thì hơi phức tạp hơn bài toán tập con/tổ hợp một chút, chúng ta xem bài 47 " hoán vị đầy đủ II":

 cho bạn đầu vào một dãy số có thể chứa chữ số trùng lặp `nums`, hãy viết một thuật toán, trả về mọi hoán vị đầy đủ có thể có, chữ ký hàm như sau:

```java
List<List<Integer>> permuteUnique(int[] nums)
```

 ví dụ đầu vào `nums = [1,2,2]`, hàm trả về:

```java
[ [1,2,2],[2,1,2],[2,2,1] ]
```

 xem code lời giải trước:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> track = new LinkedList<>();
    boolean[] used;

    public List<List<Integer>> permuteUnique(int[] nums) {
        // sắp xếp trước để các phần tử giống nhau đứng cạnh nhau 
        Arrays.sort(nums);
        used = new boolean[nums.length];
        backtrack(nums);
        return res;
    }

    void backtrack(int[] nums) {
        if (track.size() == nums.length) {
            res.add(new LinkedList(track));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }
            // logic cắt tỉa mới thêm, cố định vị trí tương đối của các phần tử giống nhau trong hoán vị 
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }
            track.add(nums[i]);
            used[i] = true;
            backtrack(nums);
            track.removeLast();
            used[i] = false;
        }
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/permutations-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



 bạn so sánh với code lời giải hoán vị đầy đủ chuẩn trước đó, đoạn code lời giải này chỉ có hai điểm khác:

1, đúng `nums` đã được sắp xếp.

2, đã thêm một dòng logic cắt tỉa bổ sung.

 loại suy từ bài toán tập con/tổ hợp có đầu vào chứa phần tử trùng lặp, bạn chắc cũng hiểu làm vậy là để tránh xuất hiện kết quả trùng lặp.

 nhưng chú ý logic cắt tỉa của bài toán hoán vị, hơi khác với logic cắt tỉa của bài toán tập con/tổ hợp: đã thêm mới logic kiểm tra `!used[i - 1]`.

 chỗ này muốn hiểu cần một chút kỹ xảo, hãy nghe mình giảng từ từ. để tiện nghiên cứu, vẫn dùng dấu phẩy trên để phân biệt các phần tử giống nhau `'`.

 giả sử đầu vào là `nums = [1,2,2']`, thuật toán hoán vị đầy đủ chuẩn sẽ cho đáp án như sau:





```
[
    [1,2,2'],[1,2',2],
    [2,1,2'],[2,2',1],
    [2',1,2],[2',2,1]
]
```



 rõ ràng, kết quả này tồn tại trùng lặp, ví dụ `[1,2,2']` và `[1,2',2]` lẽ ra chỉ được tính là một hoán vị, nhưng lại bị tính thành hai hoán vị khác nhau.

 nên mấu chốt hiện tại nằm ở việc, thiết kế logic cắt tỉa thế nào, để loại bỏ kiểu trùng lặp này?

** đáp án là, giữ nguyên vị trí tương đối của các phần tử giống nhau trong hoán vị **.

 ví dụ như `nums = [1,2,2']` ví dụ này, mình giữ cho trong hoán vị phần tử `2` luôn đứng trước `2'`.

 như vậy thì, từ 6 hoán vị ở trên bạn chỉ chọn ra được 3 hoán vị thỏa mãn điều kiện này:

```
[ [1,2,2'],[2,1,2'],[2,2',1] ]
```

 đây chính là đáp án đúng.

 tiến thêm nữa, nếu `nums = [1,2,2',2'']`, mình chỉ cần giữ cho các phần tử trùng lặp `2` có vị trí tương đối cố định, ví dụ như `2 -> 2' -> 2''`, cũng thu được kết quả hoán vị đầy đủ không trùng lặp.

 suy nghĩ kỹ, chắc sẽ dễ dàng hiểu rõ nguyên lý trong đó:

** thuật toán hoán vị đầy đủ chuẩn sở dĩ xuất hiện trùng lặp, là vì coi các dãy hoán vị tạo bởi các phần tử giống nhau thành các dãy khác nhau, nhưng trên thực tế chúng phải giống nhau; còn nếu cố định thứ tự dãy tạo bởi các phần tử giống nhau, đương nhiên sẽ tránh được trùng lặp **.

 vậy thì phản ánh vào code, bạn chú ý xem logic cắt tỉa này:





```java
// logic cắt tỉa mới thêm, cố định vị trí tương đối của các phần tử giống nhau trong hoán vị 
if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
    // nếu phần tử kề nhau bằng nhau chưa được dùng, thì bỏ qua 
    continue;
}
// chọn nums[i]
```



** khi xuất hiện phần tử trùng lặp, ví dụ đầu vào `nums = [1,2,2',2'']`, `2'` chỉ khi `2` đã được sử dụng thì mới được chọn, tương tự, `2''` chỉ khi `2'` đã được sử dụng thì mới được chọn, như vậy đảm bảo vị trí tương đối của các phần tử giống nhau trong hoán vị được giữ cố định **.

 mở rộng thêm ở đây một chút, nếu bạn đổi `!used[i - 1]` trong logic cắt tỉa trên thành `used[i - 1]`, thực ra cũng qua được mọi test case, nhưng hiệu suất sẽ giảm đi, tại sao vậy?

 sở dĩ sửa như vậy mà không phát sinh lỗi, là vì cách viết này tương đương với việc duy trì thứ tự tương đối `2'' -> 2' -> 2`, cuối cùng cũng đạt được hiệu quả khử trùng lặp.

 nhưng tại sao viết vậy hiệu suất lại giảm? vì cách viết này cắt bỏ quá ít cành.

 ví dụ đầu vào `nums = [2,2',2'']`, cây quay lui sinh ra như sau:

![](https://labuladong.online/algo/images/permutation/12.jpeg)

 nếu dùng cành màu xanh lá để biểu diễn đường đi mà hàm `backtrack` đã duyệt qua, cành màu đỏ biểu diễn điểm kích hoạt logic cắt tỉa, như vậy `!used[i - 1]` cây quay lui thu được từ logic cắt tỉa này trông như thế này:

![](https://labuladong.online/algo/images/permutation/13.jpeg)

 mà `used[i - 1]` cây quay lui thu được từ logic cắt tỉa này như sau:

![](https://labuladong.online/algo/images/permutation/14.jpeg)

 có thể thấy, `!used[i - 1]` logic cắt tỉa này cắt rất gọn gàng dứt khoát, mà `used[i - 1]` logic cắt tỉa này tuy cũng cho kết quả không trùng lặp, nhưng nó cắt bỏ ít cành hơn, tồn tại nhiều phép tính vô ích hơn, nên hiệu suất sẽ kém hơn một chút.

 bạn có thể dùng nút " Chỉnh sửa " của bảng trực quan hóa để tự sửa code kiểm chứng, xem cây quay lui sinh ra từ hai cách viết khác nhau thế nào:


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/permutations-ii/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>👾 animation trực quan hóa code 👾</strong>
</summary>
</details>
</a>
<hr/>

 đương nhiên, về việc khử trùng lặp hoán vị, cũng có độc giả đề xuất ý tưởng cắt tỉa khác:

```java
void backtrack(int[] nums, LinkedList<Integer> track) {
    if (track.size() == nums.length) {
        res.add(new LinkedList(track));
        return;
    }

    // ghi lại giá trị trên cành cây trước đó     
    // đề bài nói -10 <= nums[i] <= 10, vì vậy khởi tạo một giá trị đặc biệt 
    int prevNum = -666;
    for (int i = 0; i < nums.length; i++) {
        // loại trừ lựa chọn không hợp lệ 
        if (used[i]) {
            continue;
        }
        if (nums[i] == prevNum) {
            continue;
        }

        track.add(nums[i]);
        used[i] = true;
        // ghi lại giá trị trên cành cây này   
        prevNum = nums[i];

        backtrack(nums, track);

        track.removeLast();
        used[i] = false;
    }
}
```

 ý tưởng này cũng đúng, hãy tưởng tượng một node xuất hiện các cành giống nhau:

![](https://labuladong.online/algo/images/permutation/11.jpeg)

 nếu không xử lý, các cây con dưới những cành giống nhau này cũng sẽ mọc giống hệt nhau, nên sẽ xuất hiện hoán vị trùng lặp.

 vì sau khi sắp xếp mọi phần tử bằng nhau đều đứng cạnh nhau, nên chỉ cần dùng `prevNum` để ghi lại giá trị của cành trước đó, là có thể tránh duyệt các cành có cùng giá trị, từ đó tránh sinh ra các cây con giống nhau, cuối cùng tránh xuất hiện hoán vị trùng lặp.

 rồi, như vậy bài toán hoán vị có đầu vào trùng lặp cũng được giải quyết.

## tập con/tổ hợp (phần tử không trùng lặp nhưng được chọn lặp)

 cuối cùng đã tới loại cuối cùng: mảng đầu vào không có phần tử trùng lặp, nhưng mỗi phần tử có thể được dùng vô hạn lần.

 xem trực tiếp bài 39 " tổng tổ hợp ":

 cho bạn một mảng số nguyên không có phần tử trùng lặp `candidates` và một tổng mục tiêu `target`, tìm ra `candidates` các tổ hợp trong `target` mà tổng các số bằng số mục tiêu.`candidates` mỗi số trong.

 chữ ký hàm như sau:

```java
List<List<Integer>> combinationSum(int[] candidates, int target)
```

 ví dụ đầu vào `candidates = [1,2,3], target = 3`, thuật toán nên trả về:

```
[ [1,1,1],[1,2],[3] ]
```

 bài này nói là bài toán tổ hợp, thực ra cũng là bài toán tập con: `candidates` những tập con nào của `target`?

 muốn giải loại bài này, cũng phải quay lại cây quay lui, ** chúng ta thử suy nghĩ trước, bài toán tập con/tổ hợp chuẩn đảm bảo không dùng lặp phần tử bằng cách nào **?

 đáp án nằm ở tham số `backtrack` `start`:

```java
// khung thuật toán quay lui cho tổ hợp không trùng lặp 
void backtrack(int[] nums, int start) {
    for (int i = start; i < nums.length; i++) {
        // ...
        // đệ quy duyệt tầng tiếp theo của cây quay lui, chú ý tham số 
        backtrack(nums, i + 1);
        // ...
    }
}
```

   `i` từ `start` bắt đầu, thì cây quay lui tầng tiếp theo bắt đầu từ `start + 1` bắt đầu, từ đó đảm bảo phần tử `nums[start]` này không bị dùng lặp:

![](https://labuladong.online/algo/images/permutation/1.jpeg)

 vậy thì ngược lại, nếu mình muốn mỗi phần tử được dùng lặp, mình chỉ cần đổi `i + 1` trong logic cắt tỉa trên thành `i` là được:

```java
// khung thuật toán quay lui cho tổ hợp được chọn lặp 
void backtrack(int[] nums, int start) {
    for (int i = start; i < nums.length; i++) {
        // ...
        // đệ quy duyệt tầng tiếp theo của cây quay lui, chú ý tham số 
        backtrack(nums, i);
        // ...
    }
}
```

 việc này tương đương với việc thêm một cành cho cây quay lui trước đó, trong quá trình duyệt cây này, một phần tử có thể được dùng vô hạn lần:

![](https://labuladong.online/algo/images/permutation/10.jpeg)

 đương nhiên, như vậy cây quay lui này sẽ mọc mãi không dừng, nên hàm đệ quy của chúng ta cần đặt base case thích hợp để kết thúc thuật toán, tức là khi tổng đường đi lớn hơn `target` thì không cần duyệt tiếp xuống nữa.

 code lời giải của bài này như sau:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    // ghi lại đường đi quay lui 
    LinkedList<Integer> track = new LinkedList<>();
    // ghi lại tổng các phần tử trên đường đi trong track
    int trackSum = 0;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if (candidates.length == 0) {
            return res;
        }
        backtrack(candidates, 0, target);
        return res;
    }

    // hàm chính của thuật toán quay lui 
    void backtrack(int[] nums, int start, int target) {
        // base case, tìm được tổng mục tiêu, ghi lại kết quả 
        if (trackSum == target) {
            res.add(new LinkedList<>(track));
            return;
        }
        // base case, vượt quá tổng mục tiêu, dừng duyệt xuống dưới 
        if (trackSum > target) {
            return;
        }
        // khung chuẩn của thuật toán quay lui 
        for (int i = start; i < nums.length; i++) {
            // chọn nums[i]
            trackSum += nums[i];
            track.add(nums[i]);
            // đệ quy duyệt tầng tiếp theo của cây quay lui 
            backtrack(nums, i, target);
            // cùng một phần tử có thể được dùng lặp, chú ý tham số 
            // rút lại lựa chọn nums[i]
            trackSum -= nums[i];
            track.removeLast();
        }
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/combination-sum/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 animation trực quan hóa code 🍭</strong>
</summary>
</details>
</a>
<hr/>



## hoán vị (phần tử không trùng lặp nhưng được chọn lặp)

 trên LeetCode không có bài nào kiểm tra trực tiếp tình huống này, chúng ta thử nghĩ trước, `nums` trong trường hợp các phần tử trong mảng, sẽ có những hoán vị nào?

 ví dụ đầu vào `nums = [1,2,3]`, vậy thì hoán vị đầy đủ trong điều kiện này tổng cộng có 3^3 = 27 loại:

```java
[
  [1,1,1],[1,1,2],[1,1,3],[1,2,1],[1,2,2],[1,2,3],[1,3,1],[1,3,2],[1,3,3],
  [2,1,1],[2,1,2],[2,1,3],[2,2,1],[2,2,2],[2,2,3],[2,3,1],[2,3,2],[2,3,3],
  [3,1,1],[3,1,2],[3,1,3],[3,2,1],[3,2,2],[3,2,3],[3,3,1],[3,3,2],[3,3,3]
]
```

** thuật toán hoán vị đầy đủ chuẩn dùng `used` mảng để cắt tỉa, tránh dùng lặp cùng một phần tử. nếu cho phép dùng lặp phần tử, thì cứ thoải mái, loại bỏ mọi logic cắt tỉa của `used` mảng là được **.

 vậy thì bài này đơn giản rồi, code như sau:

```java
class Solution {

    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> track = new LinkedList<>();

    public List<List<Integer>> permuteRepeat(int[] nums) {
        backtrack(nums);
        return res;
    }

    // hàm cốt lõi của thuật toán quay lui 
    void backtrack(int[] nums) {
        // base case, đã tới node lá 
        if (track.size() == nums.length) {
            // thu thập giá trị trên node lá   
            res.add(new LinkedList(track));
            return;
        }

        // khung chuẩn của thuật toán quay lui 
        for (int i = 0; i < nums.length; i++) {
            // đưa ra lựa chọn 
            track.add(nums[i]);
            // tiến vào tầng tiếp theo của cây quay lui 
            backtrack(nums);
            // hủy bỏ lựa chọn 
            track.removeLast();
        }
    }
}
```

 tới đây, 9 biến thể của bài toán hoán vị/tổ hợp/tập con đã được giảng xong.

## tổng kết cuối cùng

 cùng ôn lại khác biệt về code của ba dạng bài toán hoán vị/tổ hợp/tập con.

 vì bài toán tập con và bài toán tổ hợp về bản chất là một, chẳng qua base case có một chút khác biệt, nên đặt hai bài này cạnh nhau mà xem.

** Hình thức một: phần tử không trùng lặp và không được chọn lặp, tức là mỗi phần tử trong `nums` nhiều nhất chỉ được sử dụng một lần **, code cốt lõi của `backtrack` như sau:

```java
// khung thuật toán quay lui cho bài toán tổ hợp/tập con 
void backtrack(int[] nums, int start) {
    // khung chuẩn của thuật toán quay lui 
    for (int i = start; i < nums.length; i++) {
        // đưa ra lựa chọn 
        track.addLast(nums[i]);
        // chú ý tham số 
        backtrack(nums, i + 1);
        // rút lại lựa chọn 
        track.removeLast();
    }
}

// khung thuật toán quay lui cho bài toán hoán vị 
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // logic cắt tỉa 
        if (used[i]) {
            continue;
        }
        // đưa ra lựa chọn 
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // rút lại lựa chọn 
        track.removeLast();
        used[i] = false;
    }
}
```

** Hình thức hai: phần tử có thể trùng lặp nhưng không được chọn lặp, tức là `nums` có thể lưu trữ phần tử trùng lặp, mỗi phần tử nhiều nhất chỉ được sử dụng một lần **, mấu chốt nằm ở sắp xếp và cắt tỉa, code cốt lõi của `backtrack` như sau:

```java
Arrays.sort(nums);
// khung thuật toán quay lui cho bài toán tổ hợp/tập con 
void backtrack(int[] nums, int start) {
    // khung chuẩn của thuật toán quay lui 
    for (int i = start; i < nums.length; i++) {
        // logic cắt tỉa, bỏ qua các cành kề nhau có giá trị giống nhau 
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        // đưa ra lựa chọn 
        track.addLast(nums[i]);
        // chú ý tham số 
        backtrack(nums, i + 1);
        // rút lại lựa chọn 
        track.removeLast();
    }
}


Arrays.sort(nums);
// khung thuật toán quay lui cho bài toán hoán vị 
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // logic cắt tỉa 
        if (used[i]) {
            continue;
        }
        // logic cắt tỉa, cố định vị trí tương đối của các phần tử giống nhau trong hoán vị 
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        // đưa ra lựa chọn 
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // rút lại lựa chọn 
        track.removeLast();
        used[i] = false;
    }
}
```

** hình thức ba, phần tử không trùng lặp nhưng được chọn lặp, tức là `nums` các phần tử trong, mỗi có thể được sử dụng một số lần **, chỉ cần xóa logic khử trùng lặp là được, `backtrack` cốt lõi code như sau:

```java
// khung thuật toán quay lui cho bài toán tổ hợp/tập con 
void backtrack(int[] nums, int start) {
    // khung chuẩn của thuật toán quay lui 
    for (int i = start; i < nums.length; i++) {
        // đưa ra lựa chọn 
        track.addLast(nums[i]);
        // chú ý tham số 
        backtrack(nums, i);
        // rút lại lựa chọn 
        track.removeLast();
    }
}

// khung thuật toán quay lui cho bài toán hoán vị 
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // đưa ra lựa chọn 
        track.addLast(nums[i]);
        backtrack(nums);
        // rút lại lựa chọn 
        track.removeLast();
    }
}
```

 chỉ cần suy nghĩ từ góc độ cây, những bài này trông có vẻ phức tạp đa biến, thực ra chỉ cần sửa base case là giải được, đây cũng là lý do mình nhấn mạnh tầm quan trọng của dạng bài cây trong [ tư duy khung khi học thuật toán và cấu trúc dữ liệu ](https://labuladong.online/algo/essential-technique/algorithm-summary/) và [ cầm tay luyện cây nhị phân (cương lĩnh nhận bài) ](https://labuladong.online/algo/essential-technique/binary-tree-summary/).

 nếu bạn đọc được tới đây, thật sự phải vỗ tay khen bạn, tin rằng sau này gặp các bài thuật toán lộn xộn kiểu gì, bạn cũng nhìn một cái là thấu bản chất của chúng, lấy bất biến ứng vạn biến. ngoài ra, xét tới độ dài bài viết, bài này chưa phân tích độ phức tạp của các thuật toán này, bạn có thể dùng phương pháp phân tích độ phức tạp mình đã giảng trong [ hướng dẫn thực hành phân tích độ phức tạp thời gian-không gian của thuật toán ](https://labuladong.online/algo/essential-technique/complexity-analysis/) để thử tự phân tích độ phức tạp của chúng.







<hr>
<details class="hint-container details">
<summary><strong> Bài viết trích dẫn bài này </strong></summary>

 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui I](https://labuladong.online/algo/problem-set/backtrack-i/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui III](https://labuladong.online/algo/problem-set/backtrack-iii/)
 - [ cương lĩnh cốt lõi của thuật toán series cây nhị phân ](https://labuladong.online/algo/essential-technique/binary-tree-summary/)
 - [ chuyển đổi tư duy giữa quy hoạch động và thuật toán quay lui ](https://labuladong.online/algo/dynamic-programming/word-break/)
 - [ khung công thức giải bài bằng thuật toán quay lui ](https://labuladong.online/algo/essential-technique/backtrack-framework/)
 - [ tư duy khung khi học cấu trúc dữ liệu và thuật toán ](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [ mô hình bóng-hộp: quay lui thuật toán hai góc nhìn của vét cạn ](https://labuladong.online/algo/practice-in-action/two-views-of-backtrack/)
 - [ hướng dẫn thực hành phân tích độ phức tạp thời gian-không gian của thuật toán ](https://labuladong.online/algo/essential-technique/complexity-analysis/)
 - [ giải đáp một số thắc mắc về thuật toán quay lui/thuật toán DFS ](https://labuladong.online/algo/essential-technique/backtrack-vs-dfs/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong> Bài tập trích dẫn bài này </strong></summary>

<strong> cài đặt [ plugin luyện đề Chrome của tôi ](https://labuladong.online/algo/intro/chrome/) nhấp vào các bài dưới đây để xem trực tiếp ý tưởng giải bài: </strong>

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [1079. Letter Tile Possibilities](https://leetcode.com/problems/letter-tile-possibilities/?show=1) | [1079. in chữ rời ](https://leetcode.cn/problems/letter-tile-possibilities/?show=1) | 🟠 |
| [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/?show=1) | [131. phân tách chuỗi palindrome ](https://leetcode.cn/problems/palindrome-partitioning/?show=1) | 🟠 |
| [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/?show=1) | [17. tổ hợp chữ cái của số điện thoại ](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/?show=1) | 🟠 |
| [254. Factor Combinations](https://leetcode.com/problems/factor-combinations/?show=1)🔒 | [254. tổ hợp các thừa số ](https://leetcode.cn/problems/factor-combinations/?show=1)🔒 | 🟠 |
| [267. Palindrome Permutation II](https://leetcode.com/problems/palindrome-permutation-ii/?show=1)🔒 | [267. hoán vị palindrome II](https://leetcode.cn/problems/palindrome-permutation-ii/?show=1)🔒 | 🟠 |
| [368. Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset/?show=1) | [368. tập con chia hết lớn nhất ](https://leetcode.cn/problems/largest-divisible-subset/?show=1) | 🟠 |
| [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/?show=1) | [491. dãy con tăng dần ](https://leetcode.cn/problems/non-decreasing-subsequences/?show=1) | 🟠 |
| [638. Shopping Offers](https://leetcode.com/problems/shopping-offers/?show=1) | [638. gói quà lớn ](https://leetcode.cn/problems/shopping-offers/?show=1) | 🟠 |
| [967. Numbers With Same Consecutive Differences](https://leetcode.com/problems/numbers-with-same-consecutive-differences/?show=1) | [967. số có hiệu liên tiếp bằng nhau ](https://leetcode.cn/problems/numbers-with-same-consecutive-differences/?show=1) | 🟠 |
| [996. Number of Squareful Arrays](https://leetcode.com/problems/number-of-squareful-arrays/?show=1) | [996. số lượng mảng vuông ](https://leetcode.cn/problems/number-of-squareful-arrays/?show=1) | 🔴 |
| - | [Kiếm chỉ Offer 38. hoán vị của chuỗi ](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 079. tất cả tập con ](https://leetcode.cn/problems/TVdhkn/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 080. tổ hợp chứa k phần tử ](https://leetcode.cn/problems/uUsW3B/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 081. tổ hợp cho phép chọn lặp phần tử ](https://leetcode.cn/problems/Ygoe9J/?show=1) | 🟠 |
| - | [Kiếm chỉ Offer II 083. hoán vị đầy đủ của tập hợp không có phần tử trùng lặp ](https://leetcode.cn/problems/VvJkup/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)