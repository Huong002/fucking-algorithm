# Mấy bài toán xác suất phản trực giác

![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã ra mắt [Lộ trình cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), nếu cần bạn có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, mình khuyên bạn học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**

**-----------**

Bài trước [Bàn về thuật toán ngẫu nhiên trong game](https://labuladong.online/algo/frequency-interview/random-algorithm/) đã nói về phương pháp Monte Carlo để kiểm chứng thuật toán xác suất, hôm nay trò chuyện chút nội dung nhẹ nhàng: mấy vấn đề thú vị liên quan đến xác suất.

Tính xác suất có hai nguyên tắc đơn giản nhất sau:

Nguyên tắc một, tính xác suất nhất định phải có một hệ quy chiếu, gọi là "không gian mẫu", tức tất cả kết quả có thể xuất hiện của sự kiện ngẫu nhiên. Xác suất sự kiện A xảy ra = số điểm mẫu mà A chứa / tổng số điểm mẫu của không gian mẫu.

Nguyên tắc hai, tính xác suất nhất định phải hiểu rõ, xác suất là một tổng thể liên tục, không thể cắt rời xác suất liên tục ra, chính là cái gọi là xác suất có điều kiện.

Hai nguyên tắc trên hồi cấp 3 đã học, nhưng chúng ta vẫn rất dễ phạm lỗi, mà quy trình phạm lỗi cũng có có điểm tương đồng kỳ lạ:

Đầu tiên là bỏ qua nguyên tắc hai, tính sai không gian mẫu, rồi thông qua nguyên tắc một tính ra đáp án sai.

Dưới đây giới thiệu mấy vấn đề đơn giản nhưng dễ gây bối rối, lần lượt là bài toán trai gái, nghịch lý sinh nhật, bài toán ba cánh cửa. Đương nhiên, bài toán ba cánh cửa có lẽ là thứ mọi người quen tai nhất, nên sẽ nói thêm một số suy nghĩ thú vị.

### Một, bài toán trai gái

Giả sử có một gia đình, có hai đứa con, bây giờ cho bạn biết trong đó có một bé trai, hỏi đứa còn lại cũng là bé trai với xác suất bao nhiêu?

Rất nhiều người, bao gồm cả tôi, không cần suy nghĩ trả lời: 1/2 chứ gì, vì đứa còn lại hoặc là trai, hoặc là gái, mà xác suất ngang nhau. Nhưng thực tế, đáp án là 1/3.

Tư tưởng trên tại sao sai? Vì không tính đúng không gian mẫu, dẫn đến nguyên tắc một tính sai. Có hai đứa con, vậy không gian mẫu là 4, tức bốn trường hợp anh-em gái, anh-em trai, chị-em gái, chị-em trai. Đã biết có một bé trai, vậy loại trừ trường hợp chị-em gái, nên không gian mẫu còn 3. Đứa còn lại cũng là bé trai chỉ có 1 trường hợp anh-em trai, nên xác suất là 1/3.

Tại sao tính không gian mẫu lại sai? Vì chúng ta đã bỏ qua xác suất có điều kiện, tức lẫn lộn hai câu hỏi sau:

Gia đình này chỉ có một đứa con, đứa này là bé trai với xác suất bao nhiêu?

Gia đình này có hai đứa con, trong đó một đứa là bé trai, đứa còn lại là bé trai với xác suất bao nhiêu?

Theo nguyên tắc hai, vấn đề xác suất là liên tục, không thể lẫn lộn hai câu hỏi trên. Câu hỏi thứ hai cần dùng xác suất có điều kiện, tức tìm xác suất đứa còn lại cũng là bé trai với điều kiện một đứa là bé trai. Vận dụng công thức xác suất có điều kiện cũng rất dễ tính, sẽ không nói nhiều.

Thông qua vấn đề này, độc giả hẳn đã hiểu mối quan hệ của hai nguyên tắc tính xác suất, thứ dễ gây bối rối nhất chính là việc bỏ qua xác suất có điều kiện. Để không bị bối rối, cách đơn giản nhất là liệt kê tất cả kết quả có thể ra.

Cuối cùng, với vấn đề này tôi từng thấy một nghi vấn rất kỳ quặc: nếu hai đứa này là sinh đôi, không tồn tại khác biệt về tuổi thì sao?

Tôi không ngờ lại thấy có chút có lý! Nhưng thực ra, chúng ta chỉ thông qua khác biệt tuổi để biểu thị tính độc lập của hai đứa, tức là nói dù hai đứa cùng giới, cũng có hai khả năng. Nên đừng dùng sinh đôi để cãi cùn nữa.

### Hai, nghịch lý sinh nhật

Nghịch lý sinh nhật được dẫn ra từ một câu hỏi như sau: một căn phòng cần có bao nhiêu người, mới khiến xác suất tồn tại ít nhất hai người có cùng ngày sinh đạt 50%?

Đáp án là 23 người, tức là nói trong phòng nếu có 23 người, vậy thì có 50% xác suất sẽ tồn tại hai người trùng sinh nhật. Kết luận này trông khó tin, nên được gọi là nghịch lý. Theo trực giác, muốn đạt xác suất 50%, chí ít phải có 183 người, vì một năm có 365 ngày? Thực ra không phải, cảm thấy kết luận này khó tin chủ yếu có hai hiểu lầm tư duy:

** hiểu lầm thứ nhất là hiểu sai hàm ý của từ "tồn tại"**.

Độc giả có thể cho rằng, nếu trong 23 người xác suất xuất hiện trùng sinh nhật đã đạt 50%, có phải nghĩa là:

Giả sử bây giờ trong phòng đang ngồi 22 người, rồi tôi bước vào, vậy tôi có 50% xác suất tìm được một người trùng sinh nhật với mình. Nhưng chuyện này sao có thể?

Không phải, cách nghĩ này của bạn là lấy mình làm trung tâm, mà xác suất của đề bài đang mô tả tổng thể. Tức là hàm ý của "tồn tại" là chỉ bất kỳ hai người nào trong 23 người, liên quan đến tổ hợp chỉnh hợp, chắc là chẳng liên quan gì đến bạn.

Nếu bạn nhất định muốn tính xác suất tồn tại người trùng sinh nhật với mình là bao nhiêu, có thể tính như sau:

1 - P(22 người đều khác sinh nhật với tôi) = 1 -(364/365)^22 = 0.06

Kết quả tính ra như vậy có phải trông hợp lý hơn nhiều? Đối tượng tính của nghịch lý sinh nhật không phải một người nào đó, mà là một tổng thể, trong đó bao gồm tổ hợp sắp xếp của tất cả mọi người, tổng xác suất của chúng đương nhiên sẽ lớn hơn nhiều.

** hiểu lầm thứ hai là cho rằng xác suất biến đổi tuyến tính**.

Độc giả có thể cho rằng, nếu trong 23 người xác suất xuất hiện trùng sinh nhật đã đạt 50%, có phải nghĩa là xác suất của 46 người sẽ đạt 100%?

Không phải, giống như game có tỷ lệ trúng 50%, bạn chơi hai lần thì tỷ lệ trúng là 100% sao? Hiển nhiên không phải, bạn chơi hai lần thì tỷ lệ trúng là 75%:

`P(trúng trong hai lần) = P(lần đầu đã trúng) + P(lần đầu không trúng nhưng lần hai trúng) = 1/2 + 1/2*1/2 = 75%`

Vậy đổi sang nghịch lý sinh nhật cũng cùng đạo lý, xác suất không phải cộng dồn đơn giản, mà phải xét một quá trình liên tục, nên kết luận này cũng không có gì không hợp lẽ thường.

Vậy tại sao chỉ cần 23 người mà xác suất xuất hiện trùng sinh nhật đã lớn hơn 50%? Chúng ta tính trước xác suất sinh nhật của 23 người đều duy nhất (không trùng). Chỉ có 1 người thì xác suất sinh nhật duy nhất là `365/365`, 2 người thì xác suất sinh nhật duy nhất là `365/365 × 364/365`, cứ thế suy ra xác suất sinh nhật của 23 người đều duy nhất:

![](https://labuladong.online/algo/images/probability/p.png)

Tính ra khoảng 0.493, nên xác suất tồn tại trùng sinh nhật là 0.507, gần như chính là 50%. Thực tế, theo thuật toán này, khi số người đạt 70, xác suất tồn tại hai người trùng sinh nhật đã tăng lên 99.9%, cơ bản có thể coi là 100%. Nên xét về xác suất, trong một nhóm nhỏ vài chục người tồn tại người trùng sinh nhật thật chẳng có gì lạ.

### Ba, bài toán ba cánh cửa

Trò chơi này rất kinh điển: người tham gia đối mặt ba cánh cửa, trong đó hai cánh cửa sau là dê, một cánh cửa sau là xe thể thao. Người tham gia chỉ cần tùy ý chọn một cánh cửa, đồ sau cửa sẽ thuộc về anh ta (giá trị xe thể thao đương nhiên lớn hơn). Nhưng người dẫn chương trình quyết định giúp người tham gia một chút: sau khi anh ta chọn, trước hết đừng vội mở cánh cửa này, mà người dẫn chương trình mở một trong hai cánh cửa còn lại, trình bày con dê trong đó (người dẫn chương trình biết sau mỗi cánh cửa là gì), rồi cho người tham gia một cơ hội đổi cửa, lúc này người tham gia nên đổi hay không đổi?

Để tránh người đọc lần đầu thấy vấn đề này bị bối rối, mô tả cụ thể thêm vấn đề này:

Bạn là người tham gia game, bây giờ có cửa 1,2,3, giả sử bạn ngẫu nhiên chọn cửa 1, rồi người dẫn chương trình mở cửa 3 báo cho bạn sau đó là dê. Bây giờ, bạn kiên trì lựa chọn ban đầu cửa 1, hay chọn đổi thành cửa 2?

![](https://labuladong.online/algo/images/probability/sanmen.png)

Đáp án là nên đổi cửa, sau khi đổi xác suất rút được xe là 2/3, không đổi là 1/3. Lại một lần phản trực giác, cảm giác đổi hay không thì xác suất trúng hẳn đều như nhau, vì cuối cùng chắc chắn chỉ còn hai cửa, một là dê, một là xe, đây là sự thật, nên dù chọn cái nào xác suất chẳng phải đều 1/2 sao?

Tương tự bài toán trai gái nói trước đó, phương pháp đơn giản chắc chắn nhất là liệt kê tất cả kết quả có thể ra:

![](https://labuladong.online/algo/images/probability/tree.png)

Rất dễ thấy xác suất chọn đổi cửa mà trúng là 2/3, không đổi là 1/3.

Về vấn đề này còn có cách đơn giản hơn: người dẫn chương trình mở cửa thực tế đang "cô đặc" xác suất. Ban đầu bạn chọn trúng xe xác suất đương nhiên là 1/3, hai cửa còn lại chứa xe xác suất đương nhiên là 2/3, chuyện này chẳng có gì để nói. Nhưng người dẫn chương trình giúp bạn loại trừ một cửa chứa dê, tương đương đem 2/3 xác suất đó cô đặc lên cánh cửa còn lại này. Vậy, bạn nói bạn ôm cánh cửa 1/3 ban đầu, hay đổi thành cánh cửa đã qua "cô đặc" với xác suất 2/3?

Trực quan hơn chút, giả sử bạn ba chọn một, còn 2 cánh cửa, cho bạn thêm 98 cánh cửa đựng dê, xáo trộn ngẫu nhiên 100 cánh cửa này, hỏi bạn có đổi không? Khẳng định không đổi đúng, chuyện này rõ ràng đem xác suất pha loãng, khẳng định ôm cánh cửa ban đầu là có khả năng trúng xe nhất. Lại giả sử, ban đầu có 100 cánh cửa, bạn chọn một cánh, rồi người dẫn chương trình trong 99 cánh còn lại giúp bạn loại 98 con dê, hỏi bạn có đổi sang một cánh khác không? Khẳng định đổi đúng, cánh cửa trên tay bạn là 1%, cánh còn lại là 99%, hoặc cũng có thể hiểu như vầy, không đổi chỉ là chọn 1 cánh cửa, đổi cửa tương đương chọn 99 cánh cửa, vậy kết quả rất rõ ràng rồi?

Tư tưởng trên, có lẽ có độc giả đã từng suy nghĩ, dưới đây chúng ta suy nghĩ một câu hỏi như sau: giả sử lúc bạn quyết định có đổi cửa hay không, Tiểu Minh phá cửa xông vào, yêu cầu giúp bạn ra lựa chọn. Cậu ta hoàn toàn không biết chuyện xảy ra trước đó, cậu ta chỉ biết trước mặt có hai cánh cửa, một là xe một là dê, vậy xác suất cậu ta rút trúng xe là bao nhiêu?

Đương nhiên là 1/2, đây cũng là nguyên nhân căn bản mà rất nhiều người làm sai bài toán ba cửa. Tương tự nghịch lý sinh nhật, mọi người luôn dễ lấy mình làm trung tâm, thông quagóc nhìn của Tiểu Minh này để tính có đổi cửa hay không, chuyện này hiển nhiên sẽ rơi vào hiểu lầm.

Giống như có hai cái rương, rương số một có 4 bóng đen 2 bóng đỏ, rương số hai có 2 bóng đen 4 bóng đỏ, tùy ý chọn một rương, tùy ý sờ một bóng, hỏi bạn xác suất sờ ra bóng đỏ.

Với Tiểu Minh không biết gì, cậu ta sẽ ngẫu nhiên chọn một rương, ngẫu nhiên sờ bóng, xác suất sờ được bóng đỏ là: 1/2 × 2/6 + 1/2 × 4/6 = 1/2

Với bạn là người biết chuyện, bạn biết sờ ở rương số hai xác suất lớn, nên chỉ sờ ở rương số hai, xác suất sờ được bóng đỏ là: 0 × 2/6 + 1 × 4/6 = 2/3

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

![](https://labuladong.online/algo/images/souyisou2.png)
