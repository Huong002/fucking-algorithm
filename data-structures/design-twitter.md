# Thiết kế chức năng timeline trang bạn bè




![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc ](https://labuladong.online/algo/intro/quick-learning-plan/), ai cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



Đọc xong bài này, bạn không chỉ học được khuôn mẫu thuật toán, mà còn tiện tay giải được các bài sau:

| LeetCode | LeetCode CN | Độ khó |
| :----: | :----: | :----: |
| [355. Design Twitter](https://leetcode.com/problems/design-twitter/) | [355. Thiết kế Twitter](https://leetcode.cn/problems/design-twitter/) | 🟠 |

**-----------**



> [!NOTE]
> Trước khi đọc bài này, bạn cần học trước:
>
> - [Cơ bản về linked list](https://labuladong.online/algo/data-structure-basic/linkedlist-basic/)
> - [Cơ bản về hash table](https://labuladong.online/algo/data-structure-basic/hashmap-basic/)
> - [Cơ bản về binary heap](https://labuladong.online/algo/data-structure-basic/binary-heap-basic/)

Bài 355 trên LeetCode「Thiết kế Twitter」không chỉ bản thân đề rất hay, mà còn kết hợp thuật toán merge nhiều linked list có thứ tự và thiết kế hướng đối tượng (OO design), rất có ý nghĩa thực tế, bài này dẫn mọi người xem bài này.

Còn chức năng gì của Twitter liên quan thuật toán, đợi chúng ta mô tả yêu cầu đề là biết.

## Một, giới thiệu đề & tình huống ứng dụng

Twitter và Weibo chức năng gần giống, chúng ta chủ yếu cần cài đặt mấy API sau:

```java
class Twitter {

    // user đăng một tweet
    public void postTweet(int userId, int tweetId) {}
    
    // Trả về id động gần đây nhất của người mà user này follow (kể cả chính mình)
    // Nhiều nhất 10 mục, mà các động này bắt buộc xếp theo timeline từ mới tới cũ
    public List<Integer> getNewsFeed(int userId) {}
    
    // follower follow followee, nếu Id không tồn tại thì tạo mới
    public void follow(int followerId, int followeeId) {}
    
    // follower unfollow followee, nếu Id không tồn tại thì không làm gì
    public void unfollow(int followerId, int followeeId) {}
}
```

Lấy một ví dụ cụ thể, tiện mọi người hiểu cách dùng cụ thể của API:

```java
Twitter twitter = new Twitter();

twitter.postTweet(1, 5);
// User 1 gửi một tweet mới 5

twitter.getNewsFeed(1);
// return [5], vì chính mình là follow chính mình

twitter.follow(1, 2);
// User 1 follow user 2

twitter.postTweet(2, 6);
// User 2 gửi một tweet mới (id = 6)

twitter.getNewsFeed(1);
// return [6, 5]
// Giải thích: user 1 follow chính mình và user 2, nên trả về tweet gần đây của họ
// Mà 6 bắt buộc ở trước 5, vì 6 gửi gần đây hơn

twitter.unfollow(1, 2);
// User 1 unfollow user 2

twitter.getNewsFeed(1);
// return [5]
```



 tình huống này trong đời thực của chúng ta rất thường gặp. Lấy vòng bạn bè ví dụ, như tôi vừa add WeChat của crush, rồi tôi đi refresh động vòng bạn bè của mình, vậy động của crush sẽ xuất hiện trong danh sách động của tôi, mà còn xếp theo thời gian với động khác. Chỉ là Twitter follow một chiều, bạn WeChat tương đương follow hai chiều. Trừ phi, bị chặn...

Trong mấy API này đa số đều dễ cài đặt, khó cốt lõi nhất phải là `getNewsFeed`, vì kết quả trả về bắt buộc có thứ tự về thời gian, nhưng vấn đề là follow của user biến động, làm sao?

**Ở đây thì liên quan thuật toán**: nếu chúng ta lưu tweet từng user trong linked list, mỗi Node linked list lưu `id` bài viết và một timestamp `time` (ghi thời gian đăng tiện so sánh), mà linked list này xếp theo `time` có thứ tự, vậy nếu một user follow `k` user, chúng ta có thể dùng thuật toán merge `k` linked list có thứ tự để merge ra danh sách tweet có thứ tự, đúng đắn `getNewsFeed`!

Thuật toán cụ thể v.v. sẽ giải thích . Nhưng, dù chúng ta nắm thuật toán, phải biểu diễn lập trình user `user` và tweet `tweet` thế nào mới thuật toán dùng mượt? **Đây thì liên quan thiết kế hướng đối tượng đơn giản**, dưới đây chúng ta từ nông tới sâu, từng bước thiết kế.

## Hai, thiết kế hướng đối tượng

Dựa vào phân tích vừa rồi, chúng ta cần một lớp `User`, lưu thông tin `user`, còn cần một lớp `Tweet`, lưu thông tin tweet, và cần làm Node của linked list. Nên chúng ta dựng khung tổng thể trước:

```java
class Twitter {
    private static int timestamp = 0;
    private static class Tweet {}
    private static class User {}

    // Còn mấy phương thức API đó
    public void postTweet(int userId, int tweetId) {}
    public List<Integer> getNewsFeed(int userId) {}
    public void follow(int followerId, int followeeId) {}
    public void unfollow(int followerId, int followeeId) {}
}
```



Sở dĩ lớp `Tweet` và `User` đặt vào trong lớp `Twitter`, vì lớp `Tweet` bắt buộc phải dùng một timestamp toàn cục `timestamp`, mà lớp `User` lại cần dùng lớp `Tweet` ghi tweet user gửi, nên chúng đều làm inner class. Nhưng để rõ và gọn, phần sau sẽ mỗi inner class và phương thức API lấy ra cài đặt riêng.

### Cài đặt lớp Tweet

Dựa vào phân tích trước, lớp Tweet rất dễ cài đặt: mỗi instance Tweet cần ghi tweetId của mình và thời gian đăng time, mà làm Node linked list, cần có con trỏ next trỏ tới Node tiếp theo.

```java
class Tweet {
    private int id;
    private int time;
    private Tweet next;

    // Cần truyền nội dung tweet (id) và thời gian đăng
    public Tweet(int id, int time) {
        this.id = id;
        this.time = time;
        this.next = null;
    }
}
```

![](https://labuladong.online/algo/images/design-twitter/tweet.jpg)



### Cài đặt lớp User

Chúng ta muốn theo tình huống thực tế, thông tin một user cần lưu có userId, danh sách follow, và danh sách tweet user này từng đăng. Trong đó danh sách follow phải dùng tập hợp (Hash Set) để lưu, vì không được trùng, mà cần tìm nhanh; danh sách tweet phải do linked list lưu, tiện thao tác merge có thứ tự. Vẽ hình hiểu:

![](https://labuladong.online/algo/images/design-twitter/user.jpg)

Ngoài ra, theo nguyên tắc thiết kế hướng đối tượng,「follow」「unfollow」và「đăng bài」phải là hành vi của User, huống chi danh sách follow và danh sách tweet cũng lưu trong lớp User, nên chúng ta cũng phải thêm cho User mấy phương thức follow, unfollow và post:

```java
// static int timestamp = 0
class User {
    private int id;
    public Set<Integer> followed;
    // Node đầu linked list tweet user đăng
    public Tweet head;

    public User(int userId) {
        followed = new HashSet<>();
        this.id = userId;
        this.head = null;
        // Follow chính mình một chút
        follow(id);
    }

    public void follow(int userId) {
        followed.add(userId);
    }

    public void unfollow(int userId) {
        // Không thể unfollow chính mình
        if (userId != this.id)
            followed.remove(userId);
    }

    public void post(int tweetId) {
        Tweet twt = new Tweet(tweetId, timestamp);
        timestamp++;
        // Chèn tweet mới tạo vào đầu linked list
        // Tweet càng dựa vào trước giá trị time càng lớn
        twt.next = head;
        head = twt;
    }
}
```

### Cài đặt mấy phương thức API

```java
class Twitter {
    private static int timestamp = 0;
    private static class Tweet {...}
    private static class User {...}

    // Chúng ta cần một map userId và đối tượng User tương ứng dậy 
    private HashMap<Integer, User> userMap = new HashMap<>();

    // user đăng một tweet
    public void postTweet(int userId, int tweetId) {
        // Nếu userId không tồn tại, thì tạo mới
        if (!userMap.containsKey(userId))
            userMap.put(userId, new User(userId));
        User u = userMap.get(userId);
        u.post(tweetId);
    }
    
    // follower follow followee
    public void follow(int followerId, int followeeId) {
        // Nếu follower không tồn tại, thì tạo mới
		if(!userMap.containsKey(followerId)){
			User u = new User(followerId);
			userMap.put(followerId, u);
		}
        // Nếu followee không tồn tại, thì tạo mới
		if(!userMap.containsKey(followeeId)){
			User u = new User(followeeId);
			userMap.put(followeeId, u);
		}
		userMap.get(followerId).follow(followeeId);
    }
    
    // follower unfollow followee, nếu Id không tồn tại thì không làm gì
    public void unfollow(int followerId, int followeeId) {
        if (userMap.containsKey(followerId)) {
            User flwer = userMap.get(followerId);
            flwer.unfollow(followeeId);
        }
    }

    // Trả về id động gần đây nhất của người mà user này follow (kể cả chính mình)
    // Nhiều nhất 10 mục, mà các động này bắt buộc xếp theo timeline từ mới tới cũ
    public List<Integer> getNewsFeed(int userId) {
        // Cần hiểu thuật toán, xem phần sau 
    }
}
```



## Ba, thiết kế thuật toán

Cài đặt thuật toán merge k linked list có thứ tự cần dùng priority queue, cấu trúc dữ liệu này là ứng dụng quan trọng nhất của binary heap. Bạn có thể hiểu là nó có thể tự sắp xếp phần tử chèn vào, phần tử lộn xộn chèn vào thì được đặt vào vị trí đúng, có thể lấy ra phần tử có thứ tự theo từ nhỏ tới lớn (hoặc từ lớn tới nhỏ). Cụ thể xem [Triển khai priority queue bằng binary heap](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/).


```python
PriorityQueue pq
# Chèn lộn xộn
for i in {2,4,1,9,6}:
    pq.add(i)
while pq not empty:
    # Mỗi lần lấy phần tử đầu (nhỏ nhất)
    print(pq.pop())

# Xuất có thứ tự: 1,2,4,6,9
```


Mượn cấu trúc dữ liệu đỉnh này hỗ trợ, chúng ta rất dễ cài đặt chức năng cốt lõi này. Chú ý chúng ta đặt priority queue theo thuộc tính `time` xếp **giảm dần từ lớn tới nhỏ**, vì `time` càng lớn có nghĩa là thời gian càng gần, phải xếp phía trước:

```java
class Twitter {
    // Để tiết kiệm độ dài bài viết, lược bớt phần code đã cho ở trên...

    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res = new ArrayList<>();
        if (!userMap.containsKey(userId)) return res;
        // Id user của danh sách follow
        Set<Integer> users = userMap.get(userId).followed;
        // Tự động theo thuộc tính time xếp từ lớn tới nhỏ, dung lượng là kích thước users
        PriorityQueue<Tweet> pq = 
            new PriorityQueue<>(users.size(), (a, b)->(b.time - a.time));

        // Chèn trước mọi Node đầu linked list vào priority queue
        for (int id : users) {
            Tweet twt = userMap.get(id).head;
            if (twt == null) continue;
            pq.add(twt);
        }

        while (!pq.isEmpty()) {
            // Nhiều nhất trả về 10 mục là đủ
            if (res.size() == 10) break;
            // Pop giá trị time lớn nhất (đăng gần nhất)
            Tweet twt = pq.poll();
            res.add(twt.id);
            // Chèn Tweet tiếp theo vào để sắp xếp
            if (twt.next != null) 
                pq.add(twt.next);
        }
        return res;
    }
}
```



Quá trình này như sau, dưới đây là GIF tôi làm mô tả quá trình merge linked list. Giả sử có ba linked list Tweet theo thuộc tính time xếp giảm dần, chúng ta merge giảm dần thêm vào res. Chú ý số trong Node linked list ở hình là thuộc tính time, không phải thuộc tính id:

![](https://labuladong.online/algo/images/design-twitter/merge.gif)

Đến đây, chức năng timeline Twitter cực kỳ đơn giản hóa này thiết kế xong, thêm nhiều bài liên quan thiết kế cấu trúc dữ liệu xem [Bài tập kinh điển về thiết kế cấu trúc dữ liệu](https://labuladong.online/algo/problem-set/ds-design/).






<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [【Luyện tập tăng cường】Bài tập kinh điển về priority queue](https://labuladong.online/algo/problem-set/binary-heap/)

</details><hr>




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
