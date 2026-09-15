# Về Linux shell: những mẹo bạn phải biết


![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

Tôi rất thích dùng hệ Linux, dù nói giao diện đồ họa của Windows đúng là làm tốt hơn Linux, nhưng hỗ trợ script thì quá kém. Lúc đầu hơi không quen thao tác dòng lệnh, nhưng quen rồi ngược lại thấy rê chuột click click mới là thủ phạm lãng phí thời gian...

**Vậy với dòng lệnh Linux, bài này không giới thiệu cách dùng cụ thể của lệnh nào, mà kết hợp tình huống dùng để nói những chi tiết dễ gây lú và mẹo tăng hiệu suất**.

1. Khác biệt giữa standard input và tham số lệnh.

2. Chạy lệnh nền mà thoát terminal là thoát hết.

3. Khác biệt chuỗi bọc nháy đơn và nháy kép.

4. Có lệnh đi với `sudo` là command not found.

5. Cách tránh nhập tên file, path, lệnh trùng lặp, cùng vài mẹo nhỏ khác.

### Một, khác biệt standard input và tham số

Vấn đề này chắc chắn dễ gây lú nhất, cụ thể là không rõ khi nào dùng pipe `|` và redirect `>`, `<`, khi nào dùng biến `$`.

Ví dụ, giờ tôi có script tự nối băng thông `connect.sh`, nằm ở home:

```shell
$ where connect.sh
/home/fdl/bin/connect.sh
```

Nếu muốn xóa script này mà muốn gõ phím ít đi, làm sao? Tôi từng thử thế này:

```shell
$ where connect.sh | rm
```

Thực tế thao tác vậy là sai, cách đúng phải thế này:

```shell
$ rm $(where connect.sh)
```

Cái trước cố nối kết quả `where` vào standard input của `rm`, cái sau cố truyền kết quả làm tham số dòng lệnh.

**Standard input chính là lệnh kiểu `scanf` hay `readline` trong ngôn ngữ lập trình; còn tham số là mảng `args` truyền vào hàm `main` của chương trình**.

Bài trước [File descriptor Linux](https://labuladong.online/algo/fname.html?fname=linux进程) từng nói, pipe và redirect là đưa dữ liệu làm standard input của chương trình, còn `$(cmd)` là đọc output của lệnh `cmd` làm tham số.

Nói bằng ví dụ vừa rồi, source `rm` chắc chắn không nhận standard input, mà nhận tham số dòng lệnh để xóa file tương ứng. Ngược lại, lệnh `cat` vừa nhận standard input vừa nhận tham số:

```shell
$ cat filename
...file text...

$ cat < filename
...file text...

$ echo 'hello world' | cat
hello world
```

**Nếu lệnh có thể làm terminal block, nghĩa là lệnh đó nhận standard input, ngược lại là không nhận**, ví dụ bạn chỉ chạy lệnh `cat` không thêm tham số, terminal sẽ block, chờ bạn nhập chuỗi rồi echo lại chuỗi đó.

### Hai, chạy chương trình nền

Ví dụ bạn remote login vào server, chạy một web Django:

```shell
$ python manager.py runserver 0.0.0.0
Listening on 0.0.0.0:8080...
```

Giờ bạn có thể test dịch vụ Django qua IP server, nhưng terminal lúc này block, nhập gì cũng không phản hồi, trừ khi nhập Ctrl-C hay Ctrl-/ để kill process python.

Có thể thêm ký hiệu `&` sau lệnh, như vậy dòng lệnh không block, có thể phản hồi lệnh tiếp theo của bạn, nhưng nếu bạn logout khỏi server thì không truy cập web đó nữa.

Nếu muốn sau khi thoát server vẫn truy cập được web, nên viết lệnh thế này `(cmd &)`:

```shell
$ (python manager.py runserver 0.0.0.0 &)
Listening on 0.0.0.0:8080...

$ logout
```

**Nguyên lý tầng đáy là thế này**:

Mỗi terminal dòng lệnh là một process shell, chương trình bạn chạy trong terminal thực chất đều là process con fork từ shell này. Bình thường shell sẽ block, chờ process con thoát mới nhận lệnh mới của bạn. Thêm `&` chỉ là để shell không block nữa, có thể phản hồi lệnh mới. Nhưng dù sao, nếu bạn đóng port dòng lệnh shell này, mọi process con bám vào nó đều thoát.

Còn chạy lệnh kiểu `(cmd &)` là treo lệnh `cmd` dưới một daemon `systemd`, nhận `systemd` làm cha, như vậy khi bạn thoát terminal hiện tại thì với lệnh `cmd` vừa rồi hoàn toàn không ảnh hưởng.

Tương tự, còn một cách chạy nền hay dùng là:

```shell
$ nohup some_cmd &
```

Lệnh `nohub` cũng nguyên lý tương tự, nhưng qua test của tôi thì dạng `(cmd &)` ổn định hơn.

### Ba, khác biệt nháy đơn và nháy kép

Shell khác nhau hành vi có khác chút ít, nhưng có một điểm chắc chắn, **với mấy ký hiệu `$`, `(`, `)`, chuỗi bọc nháy đơn không escape gì, chuỗi bọc nháy kép sẽ escape**.

Hành vi shell có thể test, dùng lệnh `set -x` sẽ bật echo lệnh của shell, bạn có thể qua echo quan sát shell rốt cuộc đang chạy lệnh gì:

![](https://labuladong.online/algo/images/linuxshell/1.png)

Thấy rõ `echo $(cmd)` và `echo "$(cmd)"`, kết quả gần giống, nhưng vẫn khác. Chú ý quan sát, kết quả escape của nháy kép sẽ tự thêm nháy đơn, còn cái trước không.

**Nói cách khác, nếu chuỗi tham số `$` đọc ra chứa dấu cách, nên dùng nháy kép bọc lại, nếu không sẽ lỗi**.

### Bốn, sudo không tìm thấy lệnh

Đôi khi lệnh user thường dùng được, thêm `sudo` lấy quyền lại báo command not found:

```shell
$ connect.sh
network-manager: Permission denied

$ sudo connect.sh
sudo: command not found
```

Nguyên nhân là script `connect.sh` này chỉ tồn tại trong biến môi trường của user đó:

```shell
$ where connect.sh 
/home/fdl/bin/connect.sh
```

**Khi dùng `sudo`, hệ thống sẽ dùng quyền và biến môi trường của user đó quy định trong file `/etc/sudoers`**, mà script này trong thư mục biến môi trường `/etc/sudoers` đương nhiên tìm không thấy.

Cách giải là dùng path của file script, chứ không chỉ dùng tên script:

```shell
$ sudo /home/fdl/bin/connect.sh
```

### Năm, nhập tên file giống nhau phiền quá

Chuỗi bọc ngoặc nhọn nối bằng dấu phẩy có thể tự expand, rất hữu dụng, xem thẳng ví dụ:

```shell
$ echo {one,two,three}file
onefile twofile threefile

$ echo {one,two,three}{1,2,3}
one1 one2 one3 two1 two2 two3 three1 three2 three3
```

Thấy không, mỗi ký tự trong ngoặc nhọn đều có thể kết hợp nối với chuỗi sau (hay trước), **chú ý ngoặc nhọn và dấu phẩy trong đó không được tách bằng dấu cách, nếu không sẽ bị coi là chuỗi thường**.

Mẹo này có ích thực tế gì? Đơn giản hữu dụng nhất là expand tham số cho lệnh `cp`, `mv`, `rm`:

```shell
$ cp /very/long/path/file{,.bak}
# Copy cho file một bản tên file.bak

$ rm file{1,3,5}.txt
# Xóa file1.txt file3.txt file5.txt

$ mv *.{c,cpp} src/
# Chuyển mọi file hậu tố .c và .cpp vào thư mục src
```

### Sáu, nhập tên path phiền quá

**Dùng `cd -` để về thư mục vừa ở**, xem thẳng ví dụ:

```shell
$ pwd
/very/long/path
# Về home ngó cái
$ cd
$ pwd
/home/labuladong
# Lại về thư mục vừa rồi
$ cd -
$ pwd
/very/long/path
```

**Lệnh đặc biệt `!$` sẽ thay thành path cuối của lệnh trước**, xem thẳng ví dụ:

```shell
# Chưa thêm quyền thực thi
$ /usr/bin/script.sh
zsh: permission denied: /usr/bin/script.sh

$ chmod +x !$
chmod +x /usr/bin/script.sh
```

**Lệnh đặc biệt `!*` sẽ thay thành mọi path file đã nhập của lệnh trước**, xem thẳng ví dụ:

```shell
# Đã tạo ba file script
$ file script1.sh script2.sh script3.sh

# Thêm quyền thực thi cho tất cả
$ chmod +x !*
chmod +x script1.sh script2.sh script3.sh
```

**Có thể thêm thư mục làm việc hay dùng vào biến môi trường `CDPATH`**, khi lệnh `cd` không tìm thấy file/thư mục bạn chỉ định trong thư mục hiện tại, sẽ tự tới thư mục trong `CDPATH` tìm.

Ví dụ tôi hay tới thư mục `/var/log` tìm log, có thể chạy lệnh sau:

```shell
$ export CDPATH='~:/var/log'
# Lệnh cd sẽ tìm mở rộng trong thư mục ~ và /var/log

$ pwd
/home/labuladong/musics
$ cd mysql
cd /var/log/mysql
$ pwd
/var/log/mysql
$ cd my_pictures
cd /home/labuladong/my_pictures
```

Mẹo này rất dễ dùng, như vậy khỏi viết path đầy đủ hoài, tiết kiệm không ít thời gian.

Cần chú ý, thao tác trên là bash hỗ trợ, các trình thông dịch shell phổ biến khác đương nhiên đều hỗ trợ mở rộng thư mục tìm kiếm của lệnh `cd`, nhưng có thể không phải sửa biến `CDPATH` này, cách thiết lập cụ thể có thể tự search.

### Bảy, nhập lệnh trùng phiền quá

**Dùng lệnh đặc biệt `!!`, có thể tự thay thành lệnh đã dùng lần trước**:

```shell
$ apt install net-tools
E: Could not open lock file - open (13: Permission denied)

$ sudo !!
sudo apt install net-tools
[sudo] password for fdl:
```

Có lệnh rất dài, nhất thời không nhớ tham số cụ thể thì sao?

**Với terminal bash, có thể dùng phím tắt `Ctrl+R` tìm ngược lệnh lịch sử**, sở dĩ nói tìm ngược là tìm lệnh đã nhập gần nhất.

Ví dụ nhấn `Ctrl+R` xong nhập `sudo`, bash sẽ tìm ra lệnh gần nhất chứa `sudo`, bạn enter là chạy được lệnh đó:

```shell
(reverse-i-search)`sudo': sudo apt install git
```

Nhưng cách này có nhược: thứ nhất chức năng này hình như chỉ bash hỗ trợ, tôi dùng zsh làm terminal thì không dùng được; thứ hai chỉ tìm ra một (gần nhất) lệnh, nếu tôi muốn tìm lệnh cũ nào đó thì chịu.

Với trường hợp này, **cách thường dùng nhất của chúng ta là dùng lệnh `history` kết hợp pipe và lệnh `grep` để tìm lệnh lịch sử nào đó**:

```shell
# Lọc mọi lệnh lịch sử chứa config
$ history | grep 'config'
 7352  ./configure
 7434  git config --global --unset https.proxy
 9609  ifconfig
 9985  clip -o | sed -z 's/\n/,\n/g' | clip
10433  cd ~/.config
```
Mọi lệnh shell bạn dùng đều được ghi lại, số phía trước nghĩa là lệnh thứ mấy, tìm được lệnh muốn dùng lại thì cũng không cần copy paste lệnh đó, **chỉ cần dùng `!` + số hiệu lệnh muốn dùng lại là chạy được lệnh đó**.

Lấy ví dụ trên, tôi muốn chạy lại lệnh `git config` đó, có thể thế này:

```shell
$ !7434
git config --global --unset https.proxy
# Chạy xong
```

Tôi thấy `history` cộng pipe cộng `grep` gõ chữ vẫn nhiều, có thể trong file cấu hình shell của bạn (`.bashrc`, `.zshrc` v.v.) viết một hàm thế này:

```shell
his()
{
    history | grep "$@"
}
```

Như vậy không cần viết nhiều, chỉ cần `his 'some_keyword'` là tìm được lệnh lịch sử.

Tôi thường không dùng bash làm terminal, tôi giới thiệu mọi người một terminal shell rất dễ dùng tên là zsh, đây cũng là shell tôi tự dùng. Terminal này còn có thể mở rộng đủ loại plugin, rất dễ dùng, cách cấu hình cụ thể có thể tự search.

### Mẹo nhỏ khác

**1, lệnh `yes` tự nhập ký tự `y` để xác nhận**:

Khi cài phần mềm nào đó, có thể có câu hỏi tương tác:

```shell
$ sudo apt install XXX
...
XXX will use 996 MB disk space, continue? [y/n]
```

Tình huống thường gặp là chúng ta đều muốn yes suốt, nhưng nếu muốn tự động hóa cài phần mềm thì rất phiền, gặp câu hỏi tương tác này là kẹt, còn phải xử lý tay.

Lệnh `yes` có thể giúp chúng ta:

```shell
$ yes | your_cmd
```

Như vậy sẽ tự `y` suốt, không dừng bắt ta nhập.

Nếu bạn đọc bài trước [File descriptor Linux](https://labuladong.online/algo/fname.html?fname=linux进程), thì biết nguyên lý rất đơn giản:

Bạn chạy riêng lệnh `yes`, phát hiện nó chính là in ra một đống ký tự y, qua pipe nối output với standard input của `your_cmd`, nếu `your_cmd` lại hỏi chán thì sẽ đọc dữ liệu từ standard input, cũng sẽ đọc được một y và dấu xuống dòng, hiệu quả giống bạn nhập tay y xác nhận.

**2, biến đặc biệt `$?` ghi giá trị trả về của lệnh trước**.

Trong Linux shell, theo thói quen của C, giá trị trả về 0 nghĩa là chương trình thoát bình thường, khác 0 là thoát bất thường. Đọc giá trị trả về của lệnh trước khi dùng dòng lệnh bình thường thấy không có ích gì, nhưng nếu bạn muốn viết script shell, biết giá trị trả về rất hữu dụng.

**Lấy ví dụ thực tế**, ví dụ repo Github fucking-algorithm của tôi, tôi cần thêm ba link footer上一篇,下一篇,目录 dưới cùng mọi file markdown, có bài đã có footer, phần lớn chưa.

Để tránh thêm trùng, tôi phải biết đuôi file md đã thêm chưa, lúc này có thể dùng biến `$?` kết hợp lệnh `grep` làm được:

```shell
#!/bin/bash
filename=$1
# Xem đuôi file có chứa từ khóa không
tail | grep '下一篇' $filename
# grep tìm thấy khớp trả về 0, không thấy trả về khác 0
[ $? -ne 0 ] && { 添加页脚; }
```

<!-- REVIEW_REQUIRED: giữ nguyên từ khóa `下一篇` trong `grep` và khối `{ 添加页脚; }` vì là key/logic minh họa khớp với tên link footer, đổi sẽ làm sai ví dụ. -->

**3, biến đặc biệt `$$` ghi PID của process hiện tại**.

Chức năng này khi dùng bình thường có thể cũng ít dùng, nhưng khi viết script shell cũng rất hữu dụng, ví dụ bạn muốn tạo file tạm trong `/tmp`, đặt tên file thì rất đau đầu, lúc này có thể dùng biến `$$` expand ra PID của process hiện tại làm tên file tạm, PID trong máy đều duy nhất, nên tuyệt không trùng, cũng không cần bạn nhớ tên file tạm.

Rồi, hôm nay chia sẻ mấy mẹo này thôi, nếu mọi người hứng thú với Linux, có thể nhấn xem chia sẻ, dữ liệu tốt lần sau viết tiếp.



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Cái bẫy của pipe Linux](https://labuladong.online/algo/fname.html?fname=linux技巧3)

</details><hr>





**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
