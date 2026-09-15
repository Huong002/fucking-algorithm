# Process, thread, file descriptor trong Linux là gì



![](https://labuladong.online/algo/images/souyisou1.png)

**Thông báo: Theo nhu cầu của đông đảo độc giả, website đã mở [lộ trình học cấp tốc](https://labuladong.online/algo/intro/quick-learning-plan/), bạn nào cần có thể xem qua, cảm ơn sự ủng hộ của mọi người~ Ngoài ra, bạn nên học bài viết trên [website](https://labuladong.online/algo/) của mình để có trải nghiệm tốt hơn.**



**-----------**

Nói tới process, e là câu hỏi thường gặp nhất khi phỏng vấn chính là quan hệ thread và process, vậy nói trước đáp án: **Trong hệ Linux, process và thread gần như không khác nhau**.

Process trong Linux chính là một cấu trúc dữ liệu, hiểu rõ là hiểu được nguyên lý底层 của file descriptor, redirect, lệnh pipe, cuối cùng ta nhìn từ góc OS xem vì sao nói thread và process cơ bản không khác.

### Một, process là gì

Trước hết, một cách trừu tượng, máy tính của chúng ta chính là thứ này:

![](https://labuladong.online/algo/images/linuxProcess/1.jpg)

Hình chữ nhật lớn biểu thị **không gian bộ nhớ** của máy, hình chữ nhật nhỏ đại diện **process**, hình tròn góc trái dưới biểu thị **đĩa**, hình góc phải dưới biểu thị vài **thiết bị input output**, ví dụ chuột bàn phím màn hình v.v. Ngoài ra, chú ý không gian bộ nhớ bị chia hai khối, nửa trên biểu thị **user space**, nửa dưới biểu thị **kernel space**.

User space chứa tài nguyên process user cần dùng, ví dụ bạn mở một mảng trong code, mảng này肯定 nằm ở user space; kernel space存放 tài nguyên hệ thống process kernel cần load, những tài nguyên này一般 không cho user truy cập. Nhưng chú ý có process user sẽ share vài tài nguyên kernel space, ví dụ vài thư viện liên kết động v.v.

Ta viết một chương trình hello bằng C, biên dịch được file thực thi, chạy ở dòng lệnh là in ra một câu hello world, rồi chương trình thoát. Ở层面 OS, chính là tạo mới một process, process này đọc file thực thi ta biên dịch vào không gian bộ nhớ, rồi thực thi, cuối cùng thoát.

**File thực thi bạn biên dịch xong chỉ là một file**, không phải process, file thực thi必须要載入 bộ nhớ,包装 thành một process mới thực sự chạy được. Process phải靠 OS tạo, mỗi process đều có thuộc tính固有, ví dụ số process (PID), trạng thái process, file đã mở v.v., process tạo xong, đọc vào chương trình của bạn, chương trình của bạn mới được hệ thống thực thi.

Vậy, OS tạo process thế nào? **Với OS, process chính là một cấu trúc dữ liệu**, ta xem thẳng source Linux:

```cpp
struct task_struct {
	// 进程状态 -> Trạng thái process
	long			  state;
	// 虚拟内存结构体 -> Struct bộ nhớ ảo
	struct mm_struct  *mm;
	// 进程号 -> Số process
	pid_t			  pid;
	// 指向父进程的指针 -> Con trỏ tới process cha
	struct task_struct __rcu  *parent;
	// 子进程列表 -> Danh sách process con
	struct list_head		children;
	// 存放文件系统信息的指针 -> Con trỏ存放 thông tin filesystem
	struct fs_struct		*fs;
	// 一个数组，包含该进程打开的文件指针 -> Một mảng chứa con trỏ file process này đã mở
	struct files_struct		*files;
};
```

 `task_struct` chính là mô tả của kernel Linux về một process, cũng có thể gọi là 「mô tả process」. Source khá phức tạp, tôi ở đây chỉ截 một phần nhỏ hay gặp.

Trong đó thú vị là con trỏ `mm` và con trỏ `files`. `mm` trỏ tới bộ nhớ ảo của process, chính là nơi載入 tài nguyên và file thực thi; con trỏ `files` trỏ tới một mảng, mảng này装 mọi con trỏ file process đó đã mở.

### Hai, file descriptor là gì

Nói trước `files`, nó là một mảng con trỏ file. Thông thường, một process sẽ đọc input từ `files[0]`, ghi output vào `files[1]`, ghi thông tin lỗi vào `files[2]`.

Lấy ví dụ, theo góc của ta hàm `printf` của C là in ký tự ra dòng lệnh, nhưng theo góc process, chính là ghi dữ liệu vào `files[1]`; tương tự, hàm `scanf` chính là process cố đọc dữ liệu từ file `files[0]` này.

**Mỗi process khi được tạo, ba vị trí đầu của `files` được điền giá trị mặc định, lần lượt trỏ tới luồng input chuẩn, luồng output chuẩn, luồng lỗi chuẩn. 「File descriptor」 ta hay nói chính là chỉ số của mảng con trỏ file này**, nên file descriptor của chương trình mặc định 0 là input, 1 là output, 2 là lỗi.
 
Ta có thể vẽ lại một图:

![](https://labuladong.online/algo/images/linuxProcess/2.jpg)

Với máy tính一般, luồng input là bàn phím, luồng output là màn hình, luồng lỗi cũng là màn hình, nên giờ process này nối ba dây với kernel. Vì phần cứng đều do kernel quản, process của ta cần qua 「system call」 để process kernel truy cập tài nguyên phần cứng.

> [!NOTE]
> Đừng quên, trong Linux mọi thứ đều được trừu tượng thành file, thiết bị cũng là file, có thể đọc và ghi.

Nếu chương trình ta viết cần tài nguyên khác, ví dụ mở một file để đọc ghi, cũng rất đơn giản, system call để kernel mở file, file này sẽ được đặt ở vị trí thứ 4 của `files`:

![](https://labuladong.online/algo/images/linuxProcess/3.jpg)

Hiểu nguyên lý này, **redirect input** rất dễ hiểu, chương trình muốn đọc dữ liệu sẽ tới `files[0]` đọc, nên ta chỉ cần trỏ `files[0]` tới một file, thì chương trình sẽ đọc dữ liệu từ file này, chứ không phải từ bàn phím:

```shell
$ command < file.txt
```

![](https://labuladong.online/algo/images/linuxProcess/5.jpg)

Tương tự, **redirect output** chính là trỏ `files[1]` tới một file, thì output của chương trình sẽ không ghi vào màn hình, mà ghi vào file này:

```shell
$ command > file.txt
```

![](https://labuladong.online/algo/images/linuxProcess/4.jpg)

Redirect lỗi cũng vậy, không赘述 nữa.

**Ký hiệu pipe** thực ra cũng异曲同工, nối output của một process với input của process khác thành một 「pipe」, dữ liệu truyền trong đó, phải nói tư tưởng thiết kế này thực sự rất优美:

```shell
$ cmd1 | cmd2 | cmd3
```

![](https://labuladong.online/algo/images/linuxProcess/6.jpg)

Tới đây, bạn có thể cũng thấy chỗ cao minh của思路 thiết kế 「trong Linux mọi thứ đều là file」, dù là thiết bị, process khác, socket hay file thực sự, toàn bộ đều có thể đọc ghi,统一装 vào một mảng `files` đơn giản, process qua file descriptor đơn giản truy cập tài nguyên tương ứng, chi tiết cụ thể giao cho OS,解耦 hiệu quả,优美高效.

### Ba, thread là gì

Trước hết phải rõ, đa process và đa thread đều là đồng thời, đều có thể nâng hiệu suất dùng processor, nên giờ mấu chốt là đa thread và đa process khác gì.

Vì sao nói trong Linux thread và process cơ bản không khác? Vì nhìn từ góc kernel Linux, không phân biệt thread và process.

Ta biết system call `fork()` có thể tạo process con mới, hàm `pthread()` có thể tạo thread mới. **Nhưng dù thread hay process, đều dùng struct `task_struct` để biểu thị, khác duy nhất chính là vùng dữ liệu share khác nhau**.

Nói cách khác, thread nhìn không khác process, chỉ là vài vùng dữ liệu của thread share với process cha, còn process con là copy bản sao, chứ không share. Ví dụ như struct `mm` và struct `files` trong thread đều share, tôi vẽ hai图 bạn sẽ hiểu:

![](https://labuladong.online/algo/images/linuxProcess/7.jpg)

![](https://labuladong.online/algo/images/linuxProcess/8.jpg)

Nên chương trình đa thread của ta phải dùng cơ chế lock, tránh nhiều thread đồng thời ghi vào cùng vùng, nếu không có thể gây loạn dữ liệu.

Vậy bạn có thể hỏi, **đã process và thread na ná, mà đa process dữ liệu không share, tức không tồn tại vấn đề loạn dữ liệu, vì sao dùng đa thread普遍 hơn đa process nhiều**?

Vì thực tế đồng thời share dữ liệu普遍 hơn mà, ví dụ mười người đồng thời rút mười đồng từ một tài khoản, ta mong số dư tài khoản share này giảm đúng một trăm đồng, chứ không mong mỗi người được một bản copy tài khoản, mỗi bản copy giảm mười đồng.

Đương nhiên, phải nói rõ, chỉ hệ Linux coi thread là process share dữ liệu, không nhìn đặc biệt, nhiều OS khác phân biệt thread và process, thread có cấu trúc dữ liệu đặc hữu, tôi thấy không简洁 bằng thiết kế này của Linux, tăng độ phức tạp hệ thống.

Trong Linux tạo thread và process mới hiệu suất đều rất cao, với vấn đề copy vùng nhớ khi tạo process mới, Linux dùng chiến lược copy-on-write để tối ưu, tức không thực sự复制 không gian bộ nhớ process cha, mà đợi tới khi cần thao tác ghi mới đi复制. **Nên trong Linux tạo process mới và tạo thread mới đều rất nhanh**.



<hr>
<details class="hint-container details">
<summary><strong>Bài viết trích dẫn bài này</strong></summary>

 - [Cái bẫy của pipe Linux](https://labuladong.online/algo/fname.html?fname=linux技巧3)
 - [Về Linux shell: những技巧 bạn phải biết](https://labuladong.online/algo/fname.html?fname=linuxshell)

</details><hr>





**＿＿＿＿＿＿＿＿＿＿＿＿＿**



![](https://labuladong.online/algo/images/souyisou2.png)
