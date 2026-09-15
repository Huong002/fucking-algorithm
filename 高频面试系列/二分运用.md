# Khung tư duy khi vận dụng tìm kiếm nhị phân trong thực tế



![](https://labuladong.online/algo/images/souyisou1.png)

** thông báo: để đáp ứng nhu cầu của đông đảo độc giả, website đã mở [ lộ trình học cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người ~ ngoài ra, bạn nên học bài viết trên [ website ](https://labuladong.online/algo/), để có trải nghiệm tốt hơn.**



 đọc xong bài này, bạn không chỉ học sẽ thuật toán khuôn mẫu, còn có thể xuôi thì giải quyết như sau đề bài:

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | [1011. khả năng giao bưu kiện trong D ngày ](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/) | 🟠 |
| [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | [410. giá trị lớn nhất sau khi phân tách mảng ](https://leetcode.cn/problems/split-array-largest-sum/) | 🔴 |
| [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | [875. Keke ăn chuối ](https://leetcode.cn/problems/koko-eating-bananas/) | 🟠 |

**-----------**



> [!NOTE]
> trước khi đọc bài này, bạn cần học trước:
>
> - [ giải chi tiết khung tìm kiếm nhị phân ](https://labuladong.online/algo/essential-technique/binary-search-framework/)

 ở [ giải chi tiết khung tìm kiếm nhị phân ](https://labuladong.online/algo/essential-technique/binary-search-framework/) trong tôi đã nghiên cứu chi tiết vấn đề tìm kiếm nhị phân, tìm hiểu " tìm kiếm một phần tử ", " tìm biên trái ", "tìm biên phải" này, dạy bạn cách viết ra thuật toán tìm kiếm nhị phân đúng, không bug.

** nhưng khung code tìm kiếm nhị phân tổng kết ở phần trước chỉ giới hạn ở tình huống cơ bản "tìm kiếm phần tử chỉ định trong mảng có thứ tự" này, bài toán thuật toán cụ thể không trực tiếp như vậy, có thể bạn khó mà nhận ra bài này dùng được tìm kiếm nhị phân **.

 nên bài này tổng kết một bộ khung công thức vận dụng thuật toán tìm kiếm nhị phân, giúp bạn khi gặp bài toán thực tế liên quan tới thuật toán tìm kiếm nhị phân, có thể suy nghĩ phân tích một cách có hệ thống, từng bước vững chắc, viết ra đáp án.







## Code tìm kiếm nhị phân gốc

 nguyên mẫu của tìm kiếm nhị phân chính là tìm kiếm một phần tử "** mảng có thứ tự **" `target`, trả về chỉ số tương ứng của phần tử đó.

 nếu phần tử đó không tồn tại, thì có thể trả về một giá trị đặc biệt nào đó, kiểu chi tiết này chỉ cần tinh chỉnh cài đặt thuật toán là làm được.

 còn một vấn đề quan trọng nữa, nếu "** mảng có thứ tự **" tồn tại nhiều phần tử `target`, thì những phần tử này chắc chắn đứng cạnh nhau, ở đây liên quan tới việc thuật toán nên trả về chỉ số của phần tử `target` bên trái nhất hay bên phải nhất `target`, cũng chính là cái gọi là " tìm biên trái " và " tìm biên phải ", việc này cũng làm được bằng cách tinh chỉnh code thuật toán.

** ở phần trước [ khung cốt lõi của tìm kiếm nhị phân ](https://labuladong.online/algo/essential-technique/binary-search-framework/) đã tìm hiểu chi tiết vấn đề trình bày ở trên, độc giả nào chưa rõ phần này nên ôn lại phần trước **, độc giả nào đã hiểu rõ thuật toán tìm kiếm nhị phân cơ bản có thể đọc tiếp.

** trong các bài toán thuật toán cụ thể, thường dùng tới hai tình huống " tìm biên trái " và " tìm biên phải " **, hiếm khi bắt bạn đơn lẻ " tìm kiếm một phần tử ".

 vì đề thuật toán thường bắt bạn tìm giá trị tối ưu, ví dụ như bắt bạn tìm " tốc độ nhỏ nhất ", bắt bạn tìm " tải trọng thấp nhất ", quá trình tìm giá trị tối ưu, ắt là quá trình tìm kiếm một biên, nên dưới đây chúng ta phân tích chi tiết code thuật toán nhị phân tìm hai loại biên này.

> [!NOTE]
> chú ý, trong bài này mình đều viết theo kiểu tìm kiếm nhị phân đóng trái mở phải, nếu bạn quen kiểu đóng cả hai đầu, có thể tự sửa lại code.

" tìm biên trái " tìm kiếm nhị phân thuật toán code cài đặt cụ thể như sau:

```java
//  tìm biên trái 
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            //  khi  tìm   target  giờ ,  co lại  phải  phía  biên 
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left;
}
```

 giả sử mảng đầu vào `nums = [1,2,3,3,3,5,7]`, phần tử muốn tìm `target = 3`, thì thuật toán sẽ trả về chỉ số 2.

 nếu vẽ một hình, là như thế này:

![](https://labuladong.online/algo/images/binary-search-in-action/1.jpeg)

" tìm biên phải " tìm kiếm nhị phân thuật toán code cài đặt cụ thể như sau:

```java
//  tìm biên phải 
int right_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;

    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            //  khi  tìm   target  giờ ,  co lại  trái  phía  biên 
            left = mid + 1;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1;
}
```

 đầu vào như trên, thì thuật toán sẽ trả về chỉ số 4, nếu vẽ một hình, là như thế này:

![](https://labuladong.online/algo/images/binary-search-in-action/2.jpeg)

 tốt, nội dung trên đều thuộc phần ôn tập, mình nghĩ độc giả đọc tới đây chắc đều hiểu. hãy nhớ hình ảnh trên, mọi bài toán nào trừu tượng hóa được thành hình ảnh trên, đều có thể dùng tìm kiếm nhị phân để giải.







## Tổng quát hóa bài toán tìm kiếm nhị phân

 bài toán nào có thể vận dụng kỹ thuật thuật toán tìm kiếm nhị phân?

** đầu tiên, bạn phải trừu tượng hóa từ đề bài một biến độc lập `x`, một hàm `x` theo `f(x)`, và một giá trị mục tiêu `target`**.

 đồng thời, `x, f(x), target` còn phải thỏa mãn các điều kiện sau:

**1, `f(x)` bắt buộc phải là hàm đơn điệu trên `x` (đơn điệu tăng hay đơn điệu giảm đều được) **.

**2, đề bài bắt bạn tính `f(x) == target` thỏa mãn điều kiện ràng buộc `x` **.

 quy tắc trên nghe hơi trừu tượng, lấy một ví dụ cụ thể:

 cho bạn một mảng có thứ tự xếp tăng dần `nums` và một phần tử mục tiêu `target`, hãy tính ra `target` chỉ số của, nếu có nhiều phần tử mục tiêu, trả về chỉ số nhỏ nhất.

 đây chính là " tìm biên trái " dạng bài cơ bản này, code lời giải đã viết ở trước, nhưng trong đó `x, f(x), target` lần lượt là gì?

 chúng ta có thể coi chỉ số của phần tử trong mảng là biến độc lập `x`, quan hệ hàm `f(x)` có thể đặt như thế này:

```java
//  hàm  f(x)  là  về    biến  x    đơn điệu  tăng dần  hàm 
//  vào  ngẫm  nums  là  sẽ không  thay đổi   ,  vì vậy  có thể  bỏ qua ,  không  tính    biến 
int f(int x, int[] nums) {
    return nums[x];
}
```

 thực ra hàm `f` chính là truy cập mảng `nums`, vì mảng `nums` mà đề bài cho chúng ta xếp tăng dần, nên hàm `f(x)` chính là hàm đơn điệu tăng trên `x`.

 cuối cùng, đề bài bắt chúng ta tìm gì nhỉ? có phải bắt chúng ta tính chỉ số bên trái nhất của phần tử `target`?

 có phải tương đương với việc hỏi chúng ta " thỏa mãn `f(x) == target` `x` nhỏ nhất bằng bao nhiêu "?

 vẽ một hình, như sau:

![](https://labuladong.online/algo/images/binary-search-in-action/3.jpeg)

** nếu gặp một bài toán thuật toán, có thể trừu tượng hóa nó thành hình này, là có thể vận dụng thuật toán tìm kiếm nhị phân cho nó **.

 code thuật toán như sau:

```java
//  hàm  f  là  về    biến  x    đơn điệu  tăng dần  hàm 
int f(int x, int[] nums) {
    return nums[x];
}

int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (f(mid, nums) == target) {
            //  khi  tìm   target  giờ ,  co lại  phải  phía  biên 
            right = mid;
        } else if (f(mid, nums) < target) {
            left = mid + 1;
        } else if (f(mid, nums) > target) {
            right = mid;
        }
    }
    return left;
}
```

 đoạn code này thực ra là thừa thãi, chỉ là tinh chỉnh một chút code tìm kiếm nhị phân thông thường, bọc việc truy cập trực tiếp `nums[mid]` trong một lớp hàm `f`. nhưng, làm vậy trừu tượng hóa được khung tư tưởng tìm kiếm nhị phân trong bài toán thuật toán cụ thể.

## Khung công thức vận dụng tìm kiếm nhị phân

 muốn vận dụng tìm kiếm nhị phân để giải bài toán thuật toán cụ thể, có thể suy nghĩ bắt đầu từ khung code sau:

```java
//  hàm  f  là  về    biến  x    đơn điệu  hàm 
int f(int x) {
    // ...
}

// hàm chính, tìm giá trị tối ưu của x khi f(x) == target 
int solution(int[] nums, int target) {
    if (nums.length == 0) return -1;
    //  hỏi  chính mình :    biến  x  nhỏ nhất bằng bao nhiêu ?
    int left = ...;
    //  hỏi  chính mình :    biến  x    giá trị lớn nhất  là  nhiều  ít ?
    int right = ... + 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (f(mid) == target) {
            //  hỏi  chính mình :  đề bài  là  tìm  trái  biên  vẫn là  phải  biên ?
            // ...
        } else if (f(mid) < target) {
            //  hỏi  chính mình :  như thế nào  để  f(x)  lớn  một chút ?
            // ...
        } else if (f(mid) > target) {
            //  hỏi  chính mình :  như thế nào  để  f(x)  nhỏ  một chút ?
            // ...
        }
    }
    return left;
}
```

 cụ thể mà nói, muốn dùng thuật toán tìm kiếm nhị phân để giải bài toán, chia thành các bước sau:

**1, xác định `x, f(x), target` chia rời là gì, và viết code hàm `f` **.

**2, tìm `x` phạm vi giá trị làm khoảng tìm kiếm của tìm kiếm nhị phân, khởi tạo `left` và `right` biến **.

**3, căn cứ yêu cầu của đề bài, xác định nên dùng thuật toán tìm kiếm nhị phân tìm biên trái hay biên phải, viết code lời giải **.

 dưới đây dùng vài ví dụ để giảng quy trình này.

## Ví dụ một, Keke ăn chuối

 đây là bài 875 " Keke ăn chuối ":

<Problem slug="koko-eating-bananas" />

Keke mỗi giờ ăn nhiều nhất một đống chuối, nếu ăn không hết thì để sang giờ tiếp theo ăn tiếp; nếu ăn hết đống này mà vẫn còn thèm, cũng chỉ đợi tới giờ tiếp theo mới ăn đống tiếp theo.

 anh ấy muốn ăn hết mọi đống chuối trước khi bảo vệ quay lại, hãy xác định ** tốc độ nhỏ nhất `K`**. chữ ký hàm như sau:

```java
int minEatingSpeed(int[] piles, int H);
```

 như vậy, đối với bài này, làm sao vận dụng vừa mới tổng kết khuôn mẫu, viết ra tìm kiếm nhị phân lời giải code?

 suy nghĩ theo từng bước là được:

**1, xác định `x, f(x), target` chia rời là gì, và viết code hàm `f` **.

   biến `x` là gì? nhớ lại đồ thị hàm số trước đó, bản chất của tìm kiếm nhị phân chính là tìm kiếm biến độc lập.

 vì vậy, đề bài bắt tìm gì, thì đặt cái đó làm biến độc lập, Keke ăn chuối tốc độ ăn chuối chính là biến độc lập `x`.

 như vậy, ở `x` quan hệ hàm đơn điệu `f(x)` là gì?

 rõ ràng, tốc độ ăn chuối càng nhanh, thời gian cần để ăn hết mọi đống chuối càng ít, tốc độ và thời gian chính là một quan hệ hàm đơn điệu.

 vì vậy, `f(x)` hàm:

 nếu tốc độ ăn chuối là `x` quả/giờ, thì cần `f(x)` giờ để ăn hết mọi đống chuối.

 code cài đặt như sau:

```java
// định nghĩa: khi tốc độ là x, cần f(x) giờ để ăn hết mọi đống chuối 
// f(x) đơn điệu giảm dần theo x 
long f(int[] piles, int x) {
    long hours = 0;
    for (int i = 0; i < piles.length; i++) {
        hours += piles[i] / x;
        if (piles[i] % x > 0) {
            hours++;
        }
    }
    return hours;
}
```

> [!NOTE]
> tại sao `f(x)` giá trị trả về của `long` kiểu? vì bạn chú ý phạm vi dữ liệu đề bài cho và logic của hàm `f`.`piles` phần tử lớn nhất trong mảng 10^9, có nhiều nhất 10^4; vậy thì khi `x` lấy giá trị 1, `hours` biến sẽ bị cộng dồn tới cỡ 10^13, vượt quá giá trị lớn nhất của kiểu `int` (khoảng 2x10^9), nên ở đây dùng kiểu `long` để tránh tràn số nguyên có thể xảy ra.

`target` thì rất rõ ràng rồi, giới hạn thời gian ăn chuối `H` đương nhiên chính là `target`, là ràng buộc lớn nhất đối với giá trị trả về của `f(x)`.

**2, tìm `x` phạm vi giá trị làm khoảng tìm kiếm của tìm kiếm nhị phân, khởi tạo `left` và `right` biến **.

 Keke ăn chuối tốc độ ăn chuối nhỏ nhất bằng bao nhiêu? lớn nhất bằng bao nhiêu?

 rõ ràng, tốc độ nhỏ nhất phải là 1, tốc độ lớn nhất là giá trị lớn nhất trong mảng `piles`, vì mỗi giờ ăn nhiều nhất một đống chuối, có thèm mấy cũng vô ích mà thôi.

 ở đây có hai lựa chọn, hoặc bạn dùng một vòng lặp for để duyệt mảng `piles` mảng, tính giá trị lớn nhất, hoặc bạn xem ràng buộc đề bài cho, `piles` phạm vi giá trị của các phần tử trong, là bao nhiêu, sau đó gán cho `right` một giá trị ngoài phạm vi.

 mình chọn cách thứ hai, đề bài nói `1 <= piles[i] <= 10^9`, vậy thì mình có thể xác định biên khoảng tìm kiếm nhị phân:

```java
public int minEatingSpeed(int[] piles, int H) {
    int left = 1;
    //  chú ý ,  mình chọn kiểu viết tìm kiếm nhị phân đóng trái mở phải , right  là khoảng mở ,  nên cộng thêm một 
    int right = 1000000000 + 1;

    // ...
}
```

 vì tìm kiếm nhị phân của chúng ta có độ phức tạp cỡ logarit, vì vậy `right` dù, là một giá trị rất lớn thì hiệu suất thuật toán vẫn rất cao.

**3, căn cứ yêu cầu của đề bài, xác định nên dùng thuật toán tìm kiếm nhị phân tìm biên trái hay biên phải, viết code lời giải **.

 giờ chúng ta đã xác định biến độc lập `x` là tốc độ ăn chuối, `f(x)` là hàm đơn điệu giảm, `target` chính là giới hạn thời gian ăn chuối `H`, đề bài bắt chúng ta tính tốc độ nhỏ nhất, cũng chính là `x` phải càng nhỏ càng tốt:

![](https://labuladong.online/algo/images/binary-search-in-action/4.jpeg)

 đây chính là tìm kiếm nhị phân tìm biên trái mà, tuy nhiên chú ý `f(x)` là đơn điệu giảm, đừng nhắm mắt áp khung, cần kết hợp hình trên để suy nghĩ, viết code:

```java
class Solution {
    public int minEatingSpeed(int[] piles, int H) {
        int left = 1;
        int right = 1000000000 + 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (f(piles, mid) == H) {
                //  tìm biên trái ,  thì cần   co lại  phải  phía  biên 
                right = mid;
            } else if (f(piles, mid) < H) {
                //  cần  để  f(x)    giá trị trả về  lớn  một số 
                right = mid;
            } else if (f(piles, mid) > H) {
                //  cần  để  f(x)    giá trị trả về  nhỏ  một số 
                left = mid + 1;
            }
        }
        return left;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/koko-eating-bananas/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🍭 animation trực quan hóa code 🍭</strong>
</summary>
</details>
</a>
<hr/>



> [!TIP]
> ở đây mình dùng kiểu viết tìm kiếm nhị phân đóng trái mở phải, nếu muốn dùng kiểu đóng cả hai đầu, chỉ cần sửa giá trị khởi tạo của `right` `right` và logic cập nhật:
>
>
>
>
>
> ```java
> // cách viết tìm kiếm nhị phân đóng cả hai đầu
> int minEatingSpeed(int[] piles, int H) {
> int left = 1;
> // right là khoảng đóng, nên ở đây đổi thành giá trị lớn nhất
> int right = 1000000000;
>
> // right là khoảng đóng, nên ở đây đổi thành <=
> while (left <= right) {
> int mid = left + (right - left) / 2;
> if (f(piles, mid) <= H) {
> // right là khoảng đóng, nên ở đây dùng kiểu mid - 1
> right = mid - 1;
> } else {
> left = mid + 1;
> }
> }
> return left;
> }
> ```
>
>
>
> về các chi tiết trong thuật toán này, phần trước [ giải chi tiết thuật toán tìm kiếm nhị phân ](https://labuladong.online/algo/essential-technique/binary-search-framework/) đã phân tích chi tiết, ở đây không mở rộng nữa.

 tới đây, bài này được giải quyết. các nhánh if thừa trong khung code của chúng ta chủ yếu để giúp bạn dễ hiểu, sau khi viết ra lời giải đúng, bạn nên gộp các nhánh thừa, có thể nâng cao hiệu suất chạy của thuật toán:

```java
class Solution {
    public int minEatingSpeed(int[] piles, int H) {
        int left = 1;
        int right = 1000000000 + 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (f(piles, mid) <= H) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    // f(x) đơn điệu giảm dần theo x 
    long f(int[] piles, int x) {
        long hours = 0;
        for (int i = 0; i < piles.length; i++) {
            hours += piles[i] / x;
            if (piles[i] % x > 0) {
                hours++;
            }
        }
        return hours;
    }
}
```

## Ví dụ hai, vận chuyển hàng hóa

 xem tiếp bài 1011 " khả năng giao bưu kiện trong D ngày ":

<Problem slug="capacity-to-ship-packages-within-d-days" />

 phải `D` vận chuyển xong mọi kiện hàng theo thứ tự trong, hàng hóa không thể chia cắt, làm sao xác định tải trọng nhỏ nhất khi vận chuyển?

 chữ ký hàm như sau:

```java
int shipWithinDays(int[] weights, int days);
```

 giống với bài trước, chúng ta cứ làm theo quy trình là được:

**1, xác định `x, f(x), target` chia rời là gì, và viết code hàm `f` **.

 đề bài hỏi gì, thì cái đó chính là biến độc lập, cũng chính là nói tải trọng của thuyền chính là biến độc lập `x`.

 số ngày vận chuyển tỉ lệ nghịch với tải trọng, nên có thể để `f(x)` tính toán `x` số ngày vận chuyển cần thiết ứng với tải trọng, như vậy `f(x)` là đơn điệu giảm.

 hàm `f(x)` cài đặt như sau:

```java
// định nghĩa: khi tải trọng là x, cần f(x) ngày để vận chuyển hết mọi kiện hàng
// f(x) đơn điệu giảm dần theo x 
int f(int[] weights, int x) {
    int days = 0;
    for (int i = 0; i < weights.length; ) {
        // cố gắng xếp càng nhiều hàng càng tốt
        int cap = x;
        while (i < weights.length) {
            if (cap < weights[i]) break;
            else cap -= weights[i];
            i++;
        }
        days++;
    }
    return days;
}
```

 đối với bài này, `target` hiển nhiên chính là số ngày vận chuyển `D`, chúng ta phải tính tải trọng nhỏ nhất của thuyền dưới ràng buộc `f(x) == D`,.

**2, tìm `x` phạm vi giá trị làm khoảng tìm kiếm của tìm kiếm nhị phân, khởi tạo `left` và `right` biến **.

 tải trọng nhỏ nhất của thuyền bằng bao nhiêu? tải trọng lớn nhất bằng bao nhiêu?

 rõ ràng, tải trọng nhỏ nhất của thuyền phải là giá trị lớn nhất trong mảng `weights`, vì mỗi lần ít nhất phải chở một kiện hàng đi, không thể nói là chở không nổi.

 tải trọng lớn nhất hiển nhiên chính là tổng mọi phần tử của mảng `weights`, cũng chính là một lần chở hết mọi kiện hàng đi.

 như vậy đã xác định được khoảng tìm kiếm `[left, right)`:

```java
int shipWithinDays(int[] weights, int days) {
    int left = 0;
    // chú ý, right là khoảng mở, vì vậy cộng thêm một 
    int right = 1;
    for (int w : weights) {
        left = Math.max(left, w);
        right += w;
    }
    
    // ...
}
```

**3, cần căn cứ yêu cầu của đề bài, xác định nên dùng thuật toán tìm kiếm nhị phân tìm biên trái hay biên phải, viết code lời giải **.

 giờ chúng ta đã xác định biến độc lập `x` là khả năng tải của thuyền, `f(x)` là hàm đơn điệu giảm, `target` chính là giới hạn tổng số ngày vận chuyển `D`, đề bài yêu cầu tính tải trọng nhỏ nhất của thuyền, cũng chính là `x` phải càng nhỏ càng tốt:

![](https://labuladong.online/algo/images/binary-search-in-action/5.jpeg)

 đây chính là tìm kiếm nhị phân tìm biên trái mà, kết hợp hình trên là viết được code tìm kiếm nhị phân:

```java
public int shipWithinDays(int[] weights, int days) {
    int left = 0;
    // chú ý, right là khoảng mở, vì vậy cộng thêm một 
    int right = 1;
    for (int w : weights) {
        left = Math.max(left, w);
        right += w;
    }
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (f(weights, mid) == days) {
            //  tìm biên trái ,  thì cần   co lại  phải  phía  biên 
            right = mid;
        } else if (f(weights, mid) < days) {
            //  cần  để  f(x)    giá trị trả về  lớn  một số 
            right = mid;
        } else if (f(weights, mid) > days) {
            //  cần  để  f(x)    giá trị trả về  nhỏ  một số 
            left = mid + 1;
        }
    }
    
    return left;
}
```

 tới đây, lời giải của bài này cũng được viết ra, chúng ta gộp các nhánh if thừa, nâng cao tốc độ chạy của code, code cuối cùng như sau:

```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int left = 0;
        int right = 1;
        for (int w : weights) {
            left = Math.max(left, w);
            right += w;
        }

        while (left < right) {
            int mid = left + (right - left) / 2;
            if (f(weights, mid) <= days) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }

    int f(int[] weights, int x) {
        int days = 0;
        for (int i = 0; i < weights.length; ) {
            int cap = x;
            while (i < weights.length) {
                if (cap < weights[i]) break;
                else cap -= weights[i];
                i++;
            }
            days++;
        }
        return days;
    }
}
```


<hr/>
<a href="https://labuladong.online/algo-visualize/leetcode/capacity-to-ship-packages-within-d-days/" target="_blank">
<details style="max-width:90%;max-height:400px">
<summary>
<strong>🎃 animation trực quan hóa code 🎃</strong>
</summary>
</details>
</a>
<hr/>



## Ví dụ ba, phân tách mảng

 chúng ta thực hành bài 410 " giá trị lớn nhất sau khi phân tách mảng ", độ khó là khó:

<Problem slug="split-array-largest-sum" />

 chữ ký hàm như sau:

```java
int splitArray(int[] nums, int m);
```

 bài này hơi giống một bài quy hoạch động kinh điển ở phần trước [ ném trứng từ tòa nhà cao tầng ](https://labuladong.online/algo/dynamic-programming/egg-drop/), đề bài khá vòng vo, vừa giá trị lớn nhất vừa giá trị nhỏ nhất.

 nói đơn giản, cho bạn đầu vào một mảng `nums` và con số `m`, bạn phải phân tách `nums` thành `m`.

 chắc chắn có hơn một cách phân tách, mỗi cách phân tách đều chia `nums` thành `m` mảng con, trong `m` mảng con này chắc chắn có một mảng con có tổng lớn nhất, đúng không.

 chúng ta muốn tìm một cách phân tách, mà tổng mảng con lớn nhất nó phân ra là nhỏ nhất trong tổng mảng con lớn nhất của mọi cách.

 hãy để thuật toán của bạn trả về tổng mảng con lớn nhất tương ứng với cách phân tách này.

 trời ơi, nhìn đề bài này đã thấy khó không chịu nổi, hoàn toàn không có ý tưởng, bài này vận dụng công thức chúng ta nói trước đó thế nào, để chuyển hóa thành tìm kiếm nhị phân?

** thực ra, bài này giống hệt bài toán vận chuyển giảng ở trên, không tin thì mình viết lại đề bài cho bạn xem **:

 bạn chỉ có một tàu chở hàng, hiện có một số kiện hàng, trọng lượng mỗi kiện hàng là `nums[i]`, giờ bạn cần `m` vận chuyển hết số hàng này trong, hỏi tải trọng nhỏ nhất của tàu chở hàng của bạn bằng bao nhiêu?

Đây không chính là bài 1011 "khả năng giao bưu kiện trong D ngày" tôi vừa giải quyết sao?

 hàng hóa tàu chở hàng vận chuyển mỗi ngày chính là một mảng con của `nums`; ở `m` vận chuyển hết chính là phân tách `nums` thành `m`; để tải trọng của tàu chở hàng càng nhỏ càng tốt, chính là để tổng các phần tử của mảng con lớn nhất trong mọi mảng con càng nhỏ càng tốt.

 nên lời giải của bài này cứ sao chép trực tiếp code lời giải của bài toán vận chuyển là được:

```java
class Solution {
     public int splitArray(int[] nums, int m) {
        return shipWithinDays(nums, m);
    }

    int shipWithinDays(int[] weights, int days) {
        //  thấy  phần trên 
    }

    int f(int[] weights, int x) {
        //  thấy  phần trên 
    }
}
```

 bài viết tới đây là hết, tóm lại mà nói, nếu phát hiện trong đề bài tồn tại quan hệ đơn điệu, là có thể thử dùng mạch suy nghĩ tìm kiếm nhị phân để giải. làm rõ tính đơn điệu và loại tìm kiếm nhị phân, thông qua phân tích và vẽ hình, là có thể viết ra code cuối cùng.







<hr>
<details class="hint-container details">
<summary><strong> Bài viết trích dẫn bài này </strong></summary>

 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán tìm kiếm nhị phân ](https://labuladong.online/algo/problem-set/binary-search/)
 - [【 luyện tập tăng cường 】 bài tập kinh điển về thuật toán quay lui II](https://labuladong.online/algo/problem-set/backtrack-ii/)
 - [ xử gọn mọi bài toán series số xấu xí trong một bài ](https://labuladong.online/algo/frequency-interview/ugly-number-summary/)
 - [ mẫu code cốt lõi của thuật toán tìm kiếm nhị phân ](https://labuladong.online/algo/essential-technique/binary-search-framework/)
 - [ tư duy khung khi học cấu trúc dữ liệu và thuật toán ](https://labuladong.online/algo/essential-technique/algorithm-summary/)
 - [ dùng thuật toán đánh bại thuật toán ](https://labuladong.online/algo/fname.html?fname=PDF中的算法)
 - [ quy hoạch động kinh điển: ném trứng từ tòa nhà cao tầng ](https://labuladong.online/algo/dynamic-programming/egg-drop/)
 - [ giảng hai bài toán giai thừa thường gặp ](https://labuladong.online/algo/frequency-interview/factorial-problems/)

</details><hr>




<hr>
<details class="hint-container details">
<summary><strong> Bài tập trích dẫn bài này </strong></summary>

<strong> cài đặt [ plugin luyện đề Chrome của tôi ](https://labuladong.online/algo/intro/chrome/) nhấp vào các bài dưới đây để xem trực tiếp ý tưởng giải bài: </strong>

| LeetCode | LeetCode CN | độ khó |
|:----: |:----: |:----: |
| [1201. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/?show=1) | [1201. số xấu xí III](https://leetcode.cn/problems/ugly-number-iii/?show=1) | 🟠 |
| [1723. Find Minimum Time to Finish All Jobs](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/?show=1) | [1723. thời gian ngắn nhất để hoàn thành mọi công việc ](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/?show=1) | 🔴 |
| - | [Kiếm chỉ Offer II 073. khỉ đầu chó ăn chuối ](https://leetcode.cn/problems/nZZqjQ/?show=1) | 🟠 |

</details>
<hr>



**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)