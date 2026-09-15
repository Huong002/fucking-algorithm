# Kiếp trước kiếp này của thuật toán mật mã



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

Nói tới mật mã, đầu tiên ta nghĩ tới mật khẩu đăng nhập tài khoản, nhưng theo góc nhìn mật mã học, loại này hoàn toàn không phải mật mã đúng chuẩn.

Vì sao? Vì mật khẩu tài khoản của ta, nhờ giữ bí mật mà đạt tác dụng mã hóa: mật khẩu giấu trong lòng tôi, bạn không biết, nên bạn không đăng nhập được tài khoản tôi.

Nhưng kỹ thuật mật mã cho rằng, thông tin 「giữ kín」sớm muộn cũng bị lộ, nên thuật toán mã hóa không nên dựa vào 「giữ kín」 để đảm bảo tính bí mật, mà phải làm được: cho dù biết thuật toán mã hóa, vẫn bó tay. Nói huyền ảo một chút chính là, nói cho bạn mật khẩu của tôi, bạn vẫn không biết mật khẩu của tôi.
 
Huyền diệu nhất chính là thuật toán trao đổi key Diffie-Hellman, hồi đó tôi đã thấy rất ngạc nhiên, hai người trao đổi vài con số ngay trước mặt bạn, họ liền có được một bí mật chung, mà bạn lại hoàn toàn không thể tính ra bí mật này. Dưới đây sẽ giới thiệu trọng điểm thuật toán này.

Kỹ thuật mật mã trong bài này chủ yếu giải quyết vấn đề mã hóa và giải mã khi truyền tin. Phải giả sử quá trình truyền dữ liệu không an toàn, mọi thông tin đều bị nghe lén, nên đầu gửi phải mã hóa tin, đầu nhận sau khi nhận được tin, chắc chắn phải biết giải mã thế nào. Thú vị là, nếu bạn có thể để đầu nhận biết giải mã thế nào, thì kẻ nghe lén chẳng phải cũng biết giải mã thế nào sao?

Dưới đây, **ta sẽ giới thiệu thuật toán mã hóa đối xứng, thuật toán trao đổi key, thuật toán mã hóa bất đối xứng, chữ ký số, chứng chỉ public key**, xem quá trình gian nan giải quyết vấn đề truyền tin an toàn.

### Một, mã hóa đối xứng

Mật mã đối xứng, còn gọi là mật mã dùng chung key, đúng như tên gọi, cách mã hóa này dùng key giống nhau để mã hóa và giải mã.

Ví dụ tôi nói một cách mã hóa đối xứng đơn giản nhất. Trước hết ta biết tin đều có thể biểu thị thành chuỗi bit 0/1, cũng biết hai chuỗi bit giống nhau XOR với nhau kết quả bằng 0.

Vậy ta có thể sinh một chuỗi bit ngẫu nhiên dài bằng tin gốc làm key, rồi dùng nó XOR với tin gốc, sẽ sinh ra bản mã. Ngược lại, dùng key đó XOR bản mã một lần nữa, là khôi phục được tin gốc.

Đây là ví dụ đơn giản, nhưng hơi quá đơn giản, có nhiều vấn đề. Ví dụ độ dài key và tin gốc hoàn toàn trùng nhau, nếu tin gốc rất lớn, key cũng lớn y vậy, mà chi phí tính toán để sinh lượng lớn chuỗi bit ngẫu nhiên thật sự cũng khá lớn.

Đương nhiên, có nhiều thuật toán mã hóa đối xứng phức tạp ưu tú hơn giải mấy vấn đề này, ví dụ thuật toán Rijndael, thuật toán DES ba lớp v.v. **Chúng về mặt thuật toán là không thể chê được, tức sở hữu không gian key khổng lồ, cơ bản không thể brute-force bẻ khóa, mà quá trình mã hóa tương đối nhanh**.

**Nhưng, điểm yếu của mọi thuật toán mã hóa đối xứng nằm ở việc phân phối key**. Mã hóa và giải mã dùng cùng một key, đầu gửi bắt buộc phải tìm cách gửi key cho đầu nhận. Nếu kẻ nghe lén có khả năng trộm bản mã, chắc chắn cũng có thể trộm key, vậy thuật toán có không thể chê được tới đâu vẫn tự sụp đổ.

Nên, dưới đây giới thiệu hai thuật toán hay gặp nhất giải quyết vấn đề phân phối key, lần lượt là thuật toán trao đổi key Diffie-Hellman và thuật toán mã hóa bất đối xứng.

### Hai, thuật toán trao đổi key

Key ta nói thường chính là một con số rất lớn, thuật toán dùng số này mã hóa, giải mã. Vấn đề nằm ở chỗ, kênh truyền không an toàn, mọi dữ liệu phát ra đều bị trộm. Nói cách khác, có cách nào, để hai người đường đường chính chính trao đổi một bí mật ngay trước mắt mọi người, đưa key đối xứng an toàn tới tay đầu nhận?

Thuật toán trao đổi key Diffie-Hellman làm được điều này. **Nói chính xác, thuật toán này không phải gửi an toàn một bí mật cho đối phương, mà thông qua vài con số dùng chung, hai bên tự mình sinh ra một bí mật giống nhau trong lòng, mà bí mật này của hai bên, kẻ nghe lén thứ ba không thể sinh ra được**.

Có lẽ đây chính là thần giao cách cảm trong truyền thuyết.

Thuật toán này quy tắc không phức tạp, bạn thậm chí đều có thể tìm bạn bè thử dùng chung bí mật, lát nữa tôi sẽ vẽ đơn giản quy trình cơ bản của nó. Trước đó, cần rõ một vấn đề: **không phải mọi phép toán đều có phép ngược**.

Ví dụ đơn giản nhất chính là hàm băm một chiều ta đều biết, cho một số `a` và một hàm băm `f`, bạn rất nhanh tính ra `f(a)`, nhưng nếu cho bạn `f(a)` và `f`, suy ra `a` là việc cơ bản không làm được. Thuật toán trao đổi key trông huyền ảo vậy, chính là lợi dụng tính không đảo ngược được này.

Dưới đây, xem quy trình của thuật toán trao đổi key là gì, theo cách đặt tên quen thuộc, hai bên chuẩn bị chạy thuật toán trao đổi key gọi là Alice và Bob, kẻ xấu định trộm nội dung giao tiếp của họ trên mạng gọi là Hack nhé.

Trước hết, Alice và Bob bàn bạc ra hai số `N` và `G` làm phần tử sinh, đương nhiên quá trình bàn bạc có thể bị kẻ nghe lén Hack trộm mất, nên tôi vẽ hai số này ra giữa, đại diện cho việc ba bên đều biết:

![](https://labuladong.online/algo/images/cryptography/1.jpg)

Giờ Alice và Bob **trong lòng** mỗi người tự nghĩ ra một số, lần lượt gọi là `A` và `B` nhé:

![](https://labuladong.online/algo/images/cryptography/2.jpg)

Giờ Alice lấy số `A` trong lòng mình và `G` qua phép toán nào đó ra một số `AG`, rồi gửi cho Bob; Bob lấy số `B` trong lòng mình và `G` qua phép toán giống nhau ra một số `BG`, rồi gửi cho Alice:

![](https://labuladong.online/algo/images/cryptography/3.jpg)

Tình huống giờ thành thế này:

![](https://labuladong.online/algo/images/cryptography/4.jpg)

Chú ý, giống ví dụ hàm băm vừa rồi, biết `AG` và `G`, không thể suy ngược `A` là bao nhiêu, `BG` tương tự.

Vậy, Alice có thể qua `BG` và `A` của mình, dùng phép toán nào đó được một số `ABG`, Bob cũng có thể qua `AG` và `B` của mình, dùng phép toán nào đó được `ABG`, số này chính là bí mật chung của Alice và Bob.

Còn với Hack, có thể trộm `G`, `AG`, `BG` trong quá trình truyền, nhưng vì tính không đảo ngược, có kết hợp thế nào cũng không ra được số `ABG` này.

![](https://labuladong.online/algo/images/cryptography/5.jpg)

Trên đây chính là quy trình cơ bản, còn việc lấy số cụ thể thì có nhiều điểm cần chú ý, cách tính toán tra Baidu rất dễ tìm, vì giới hạn độ dài nên tôi không viết cụ thể ở đây.

Thuật toán này có thể với tiền đề bị kẻ thứ ba nghe lén, tính ra một bí mật mà người khác không tính được để làm key của thuật toán mã hóa đối xứng, rồi bắt đầu giao tiếp mã hóa đối xứng.

Với thuật toán này, Hack lại nghĩ ra một cách bẻ khóa, không nghe lén dữ liệu giao tiếp của Alice và Bob, mà trực tiếp đồng thời mạo danh thân phận Alice và Bob, chính là 「**tấn công man-in-the-middle**」 ta hay nói:

![](https://labuladong.online/algo/images/cryptography/6.jpg)

Như vậy, hai bên hoàn toàn không phát hiện mình đang dùng chung bí mật với Hack, hậu quả chính là Hack có thể giải mã thậm chí sửa dữ liệu.

**Thấy được, thuật toán trao đổi key cũng chưa giải quyết triệt để vấn đề phân phối key, khuyết điểm nằm ở chỗ không xác minh được thân phận đối phương**. Nên trước khi dùng thuật toán trao đổi key thường phải xác minh thân phận đối phương, ví dụ dùng chữ ký số.

### Ba, mã hóa bất đối xứng

Ý tưởng của mã hóa bất đối xứng chính là, dứt khoát đừng lén lút truyền key nữa, tôi tách key mã hóa và key giải mã ra, public key dùng để mã hóa, private key dùng để giải mã. Chỉ truyền public key cho đối phương, rồi đối phương bắt đầu gửi tôi dữ liệu đã mã hóa, tôi dùng private key là giải mã được. Còn kẻ nghe lén, lấy được public key và dữ liệu mã hóa cũng vô dụng, vì chỉ private key trong tay tôi mới giải mã được.

Có thể hình dung vậy, **private key là chìa khóa, còn public key là ổ khóa, có thể công khai ổ khóa ra, để người khác khóa dữ liệu gửi cho tôi; còn chìa khóa nhất định phải giữ trong tay mình, dùng để mở**. Thuật toán RSA ta hay gặp chính là thuật toán mã hóa bất đối xứng điển hình, implement cụ thể khá phức tạp, tôi không viết ở đây, trên mạng nhiều tài liệu.

Trong ứng dụng thực tế, tốc độ tính toán của mã hóa bất đối xứng chậm hơn mã hóa đối xứng nhiều, nên khi truyền lượng dữ liệu lớn, thông thường không dùng public key mã hóa thẳng dữ liệu, mà mã hóa key của mã hóa đối xứng, truyền cho đối phương, rồi hai bên dùng thuật toán mã hóa đối xứng truyền dữ liệu.

Cần chú ý, giống thuật toán Diffie-Hellman, **thuật toán mã hóa bất đối xứng cũng không xác định được thân phận hai bên giao tiếp, vẫn sẽ bị tấn công man-in-the-middle**. Ví dụ Hack chặn public key Bob phát ra, rồi mạo danh thân phận Bob gửi cho Alice public key của mình, vậy Alice không biết sẽ dùng public key của Hack mã hóa dữ liệu riêng tư, Hack có thể qua private key giải mã trộm.

Vậy, thuật toán Diffie-Hellman và thuật toán mã hóa bất đối xứng RSA đều có thể giải quyết phần nào vấn đề phân phối key, cũng có khuyết điểm giống nhau, vậy tình huống ứng dụng của hai bên khác nhau ở đâu?

Đơn giản nói, theo nguyên lý cơ bản hai thuật toán là thấy được:

Nếu hai bên đã có một phương án mã hóa đối xứng, mong giao tiếp mã hóa mà không để người khác có được chìa khóa, vậy có thể dùng thuật toán Diffie-Hellman trao đổi key.

Nếu bạn mong ai cũng có thể mã hóa tin, mà chỉ mình bạn giải mã được, vậy thì dùng thuật toán mã hóa bất đối xứng RSA, công bố public key.

Dưới đây, ta thử giải vấn đề xác thực thân phận đầu gửi.

### Bốn, chữ ký số

Vừa nói mã hóa bất đối xứng, công khai public key cho người khác mã hóa dữ liệu rồi gửi bạn, chỉ private key tương ứng trong tay bạn mới giải được bản mã. Thực ra, **private key cũng có thể dùng để mã hóa dữ liệu, với thuật toán RSA, dữ liệu do private key mã hóa chỉ public key mới mở được**.

Chữ ký số cũng là lợi dụng đặc tính của key bất đối xứng, nhưng đảo ngược hoàn toàn với mã hóa bằng public key: **vẫn công bố public key, nhưng dùng private key của bạn mã hóa dữ liệu, rồi công bố dữ liệu đã mã hóa ra, đây chính là chữ ký số**.

Bạn có thể hỏi, có ích gì, public key có thể mở dữ liệu do private key mã hóa, tôi còn mã hóa gửi đi, không phải vẽ chuyện sao?

Đúng, nhưng **tác dụng của chữ ký số vốn không phải đảm bảo tính bí mật của dữ liệu, mà là chứng minh thân phận bạn**, chứng minh dữ liệu này đúng là do chính bạn phát ra.

Bạn nghĩ xem, dữ liệu do private key của bạn mã hóa, chỉ public key của bạn mới mở được, vậy nếu một bản dữ liệu mã hóa có thể được public key của bạn mở, chẳng phải chứng tỏ bản dữ liệu này do chính bạn (người giữ private key) phát ra sao?

Đương nhiên, dữ liệu đã mã hóa chỉ là một chữ ký, chữ ký nên được phát ra cùng dữ liệu, quy trình cụ thể như sau:

1, Bob sinh public key và private key, rồi công bố public key ra, private key tự giữ.

2, **Dùng private key mã hóa dữ liệu làm chữ ký, rồi gửi dữ liệu kèm chữ ký cùng nhau đi**.

3, Alice nhận được dữ liệu và chữ ký, cần check xem bản dữ liệu này có phải Bob phát không, bèn dùng public key Bob đã phát trước đó thử giải mã chữ ký, đem dữ liệu nhận được so sánh với kết quả giải mã chữ ký, nếu hoàn toàn giống nhau, chứng tỏ dữ liệu không bị sửa, mà đúng là do Bob phát.

Vì sao Alice chắc chắn vậy, dù sao dữ liệu và chữ ký là hai phần, đều có thể bị đánh tráo mà? Nguyên nhân như sau:

1, Nếu ai sửa dữ liệu, vậy Alice giải mã chữ ký xong, so sánh sẽ phát hiện hai bên không khớp, nhận ra bất thường.

2, Nếu ai thay chữ ký, vậy Alice dùng public key của Bob chỉ giải ra một chuỗi loạn mã, rõ ràng không khớp với dữ liệu.

3, Có lẽ có kẻ định sửa dữ liệu, rồi lấy dữ liệu đã sửa xong chế thành chữ ký, khiến phép so sánh của Alice không phát hiện ra chỗ không khớp; nhưng một khi đã mở chữ ký thì không thể tạo lại chữ ký của Bob, vì không có private key của Bob.

Tóm lại, **chữ ký số có thể xác thực nguồn dữ liệu ở mức nhất định**. Sở dĩ nói ở mức nhất định, vì cách này vẫn có thể bị tấn công man-in-the-middle. Một khi liên quan tới phát public key, đầu nhận có thể nhận phải public key giả của man-in-the-middle, xác thực sai, vấn đề này trước sau vẫn không tránh được.

Nghe thì buồn cười, chữ ký số chính là một cách xác minh thân phận đối phương, nhưng tiền đề là thân phận đối phương phải là thật... Điều này dường như rơi vào vòng luẩn quẩn quả trứng con gà, **muốn xác định thân phận đối phương, bắt buộc phải có một nguồn đáng tin, nếu không, quy trình nhiều tới đâu cũng chỉ đang dời vấn đề đi, chứ không thực sự giải quyết vấn đề**.

### Năm, chứng chỉ public key

**Chứng chỉ thực chất chính là public key + chữ ký, do bên chứng thực thứ ba cấp**. Đưa bên thứ ba đáng tin vào, là phương án khả thi để chấm dứt vòng luẩn quẩn tin cậy.

Quy trình chứng thực chứng chỉ đại khái như sau:

1, Bob tới bên chứng thực đáng tin để chứng minh thân phận thật của bản thân, và cung cấp public key của mình.

2, Alice muốn giao tiếp với Bob, trước hết gửi request tới bên chứng thực để lấy public key của Bob, bên chứng thực sẽ gửi cho Alice một chứng chỉ (public key của Bob cùng chữ ký của mình trên public key đó).

3, Alice check chữ ký, xác định public key này đúng là do nhà chứng thực này gửi, giữa đường chưa bị sửa.

4, Alice qua public key này mã hóa dữ liệu, bắt đầu giao tiếp với Bob.

![](https://labuladong.online/algo/images/cryptography/7.jpg)

> [!NOTE]
> Trên chỉ để minh họa, chứng chỉ chỉ cần cài một lần, không cần mỗi lần đều gửi request tới bên chứng thực; thường là server gửi thẳng chứng chỉ cho client, chứ không phải bên chứng thực.

Có lẽ có người hỏi, Alice muốn qua chữ ký số xác định hiệu lực chứng chỉ, tiền đề là phải có public key (chứng thực) của bên đó, đây không phải lại quay về vòng luẩn quẩn vừa rồi sao?

Trình duyệt chính quy ta cài đều có sẵn chứng chỉ (chứa public key của nó) của bên chứng thực chính quy, dùng để xác nhận thân phận đối bên, nên mới nói chứng thực chứng chỉ là đáng tin.

Bob gửi public key cho bên chứng thực, cần cung cấp nhiều thông tin cá nhân để xác minh thân phận, khá nghiêm ngặt, nên nói cũng coi như đáng tin.

Có được public key đáng tin của Bob, giao tiếp giữa Alice và Bob nhờ thuật toán mã hóa bảo vệ, hoàn toàn không thể chê được.

Web chính quy giờ, đa số dùng giao thức HTTPS, chính là giữa giao thức HTTP và TCP thêm một lớp an toàn SSL/TLS. Sau khi trình duyệt bạn và server web hoàn thành bắt tay TCP, lớp SSL cũng sẽ bắt tay SSL trao đổi tham số an toàn, trong đó có chứa chứng chỉ của web đó, để trình duyệt xác minh thân phận của website. Lớp an toàn SSL xác minh xong, nội dung giao thức HTTP ở tầng trên đều bị mã hóa, đảm bảo truyền dữ liệu an toàn.

Như vậy, tấn công man-in-the-middle truyền thống gần như không còn không gian sống, thủ đoạn tấn công chỉ có thể chuyển từ khiếm khuyết kỹ thuật sang lừa gạt. Thực tế, hiệu quả của thủ đoạn này ngược lại còn cao hơn, ví dụ tôi đã phát hiện **nhiều web download trên mạng phát hành trình duyệt, không chỉ chứa thanh điều hướng và địa chỉ web yêu thích lộn xộn, còn chứa vài chứng chỉ của bên chứng thực không chính quy. Ai cũng có thể xin chứng chỉ, chứng chỉ không chính quy này rất có thể gây ra nguy cơ mất an toàn**.

### Sáu, tổng kết

Thuật toán mã hóa đối xứng dùng cùng một key để mã hóa và giải mã, khó bẻ khóa, tốc độ mã hóa nhanh, nhưng tồn tại vấn đề phân phối key.

Thuật toán trao đổi key Diffie-Hellman có thể để hai bên 「thần giao cách cảm」, giải quyết phần nào vấn đề phân phối key, nhưng không xác minh được thân phận bên giao tiếp, nên có thể bị tấn công man-in-the-middle.

Thuật toán mã hóa bất đối xứng sinh một cặp key, tách việc mã hóa và giải mã.

Thuật toán RSA làm thuật toán mã hóa bất đối xứng kinh điển, có hai công dụng: nếu dùng để mã hóa, có thể công bố public key để mã hóa, chỉ private key mình mới giải mã được, đảm bảo tính bí mật dữ liệu; nếu dùng cho chữ ký số, công bố public key xong, dùng private key mã hóa dữ liệu làm chữ ký, để chứng minh dữ liệu đó do người giữ private key gửi. Nhưng dù dùng cách nào, hễ liên quan tới phát public key, đều không tránh được tấn công man-in-the-middle.

Chứng chỉ public key chính là public key + chữ ký, do bên chứng thực thứ ba đáng tin cấp. Vì trình duyệt chính quy đều cài sẵn public key của bên chứng thực đáng tin, nên có thể phòng tấn công man-in-the-middle hiệu quả.

Lớp an toàn SSL/TLS trong giao thức HTTPS sẽ kết hợp dùng mấy cách mã hóa trên, **nên nói đừng cài trình duyệt không chính quy, đừng cài bừa chứng chỉ không rõ nguồn**.

Kỹ thuật mật mã chỉ là một phần nhỏ của an toàn, cho dù là website HTTPS đã qua chứng thực chính quy, cũng không có nghĩa là đáng tin, chỉ chứng tỏ việc truyền dữ liệu của nó an toàn. Kỹ thuật không bao giờ có thể thực sự bảo vệ bạn, quan trọng nhất vẫn phải nâng cao ý thức phòng ngừa an toàn cá nhân, để ý nhiều hơn, thận trọng xử lý dữ liệu nhạy cảm.




**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
