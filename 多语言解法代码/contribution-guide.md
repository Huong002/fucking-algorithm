# Sửa lỗi trong plugin luyện đề của labuladong

## Bối cảnh

Để giúp mọi người học thuật toán tốt hơn, trước đây tôi đã viết nhiều bài hướng dẫn thuật toán và phát triển một loạt plugin luyện đề, gọi chung là "combo luyện đề của labuladong" (labuladong 的刷题全家桶), chi tiết xem [tại đây](https://labuladong.github.io/article/fname.html?fname=全家桶简介).

Lời giải trong hướng dẫn và plugin của tôi chủ yếu dùng ngôn ngữ Java, vì Java là ngôn ngữ khuôn phép, ngay cả khi chưa từng tiếp xúc cũng tương đối dễ hiểu logic. Nhưng giờ chatGPT đã xuất hiện, tôi liền nhờ chatGPT viết lại lời giải của mình sang nhiều ngôn ngữ, hy vọng thân thiện hơn với các bạn có nền tảng công nghệ khác nhau.

chatGPT viết lại khá tốt, nhưng khó tránh khỏi vẫn còn một số lỗi, vì vậy tôi mong cùng mọi người sửa các lỗi này.

## Cách báo lỗi

Nếu bạn phát hiện code lời giải nào đó không vượt qua được toàn bộ test case của LeetCode (thường là code do chatGPT viết lại mới gặp tình huống này, còn code lời giải của tôi đều đã qua kiểm thử mới đăng), bạn có thể [bấm vào đây](https://github.com/labuladong/fucking-algorithm/issues/new?assignees=&labels=code+bug&template=bug_report.yml&title=%5Bbug%5D%5B%7B%E8%BF%99%E9%87%8C%E6%9B%BF%E6%8D%A2%E4%B8%BA%E5%87%BA%E9%94%99%E7%9A%84%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80%7D%5D+%7B%E8%BF%99%E9%87%8C%E6%9B%BF%E6%8D%A2%E4%B8%BA%E5%87%BA%E9%94%99%E7%9A%84%E5%8A%9B%E6%89%A3%E9%A2%98%E7%9B%AE%E6%A0%87%E8%AF%86%E7%AC%A6%7D+) gửi issue theo mẫu, tôi và các bạn khác sẽ gửi PR để sửa các lỗi này.

## Cách sửa lỗi

Trước hết, cảm ơn bạn đã sẵn lòng sửa lỗi cho code lời giải trong plugin của tôi. Sau khi bạn gửi PR sửa lỗi vào repo này, bạn sẽ trở thành contributor của repo, xuất hiện trong danh sách người đóng góp ở trang chủ của repo. Repo này đã đạt 115k star, đóng góp của bạn sẽ được rất nhiều người nhìn thấy.

Sửa code rất đơn giản, mọi code lời giải đa ngôn ngữ đều được lưu trong [多语言解法代码/solution_code.md](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%A4%9A%E8%AF%AD%E8%A8%80%E8%A7%A3%E6%B3%95%E4%BB%A3%E7%A0%81/solution_code.md), bạn chỉ cần sửa file này là được. Nội dung trong đó được tổ chức như sau:

    https://leetcode.cn/problems/xxx 的多语言解法👇

    ```cpp
    class Solution {
    public:
        int xxx() {
            // ...
        }
    };
    ```

    ```java
    class Solution {
        public int xxx() {
            // ...
        }
    }
    ```

    ```python
    class Solution:
        def xxx(self):
            # ...
    ```

    ```javascript
    var xxx = function() {
        // ...
    }
    ```

    ```go
    func xxx() {
        // ...
    }
    ```

    https://leetcode.cn/problems/xxx 的多语言解法👆


Ví dụ bạn muốn sửa lời giải JavaScript của [https://leetcode-cn.com/problems/longest-palindromic-substring/](https://leetcode-cn.com/problems/longest-palindromic-substring/), bạn có thể tìm từ khóa `longest-palindromic-substring` trong [多语言解法代码/solution_code.md](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%A4%9A%E8%AF%AD%E8%A8%80%E8%A7%A3%E6%B3%95%E4%BB%A3%E7%A0%81/solution_code.md) là sẽ thấy lời giải đa ngôn ngữ của bài này, rồi sửa code lời giải JavaScript tương ứng và gửi PR là xong.

Plugin của tôi sẽ tự động lấy nội dung mới nhất của file này, nên sau khi PR của bạn được merge vào nhánh master, phần nội dung sửa trong plugin cũng sẽ có hiệu lực.

## Yêu cầu khi gửi PR

1、PR của bạn phải là sửa phần code trong file [多语言解法代码/solution_code.md](https://github.com/labuladong/fucking-algorithm/blob/master/%E5%A4%9A%E8%AF%AD%E8%A8%80%E8%A7%A3%E6%B3%95%E4%BB%A3%E7%A0%81/solution_code.md), không sửa file khác và nội dung khác.

2、Mục đích dịch lời giải của tôi sang đa ngôn ngữ là để giúp các bạn có nền tảng khác nhau hiểu được tư duy thuật toán, vì vậy code bạn sửa có thể không phải là cách tối ưu hiệu năng nhất, nhưng nên cố gắng giữ nhất quán với hướng tiếp cận trong lời giải của tôi, và giữ đầy đủ chú thích (comment) như trong lời giải của tôi.

3、Mô tả PR của bạn cần kèm ảnh chụp màn hình chứng minh code đã qua toàn bộ test case. Tiêu đề PR có định dạng `[fix][{lang}] {slug}`, trong đó `{lang}` cần thay bằng ngôn ngữ của lời giải bạn sửa, ví dụ `[fix][cpp]`, `{slug}` cần thay bằng định danh của đề bài bạn sửa (phần cuối của URL đề bài), ví dụ đề [https://leetcode.cn/problems/search-a-2d-matrix/](https://leetcode.cn/problems/search-a-2d-matrix/) có định danh là `search-a-2d-matrix`.

**Bạn có thể xem PR này như một ví dụ**: https://github.com/labuladong/fucking-algorithm/pull/1112
