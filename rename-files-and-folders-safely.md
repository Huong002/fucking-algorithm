# Plan đổi tên file và thư mục sang tiếng Anh an toàn

## Mục tiêu
Đổi tên các file Markdown và thư mục nội dung tiếng Trung sang tên tiếng Anh dễ tìm kiếm, nhưng không làm gãy:

- Link ảnh trong `pictures/`.
- Link Markdown và URL ngoài.
- Tham chiếu GitHub hard-code trong tài liệu đóng góp.
- Liên kết/đường dẫn được dùng bởi tooling hoặc plugin.

## Hiện trạng đã xác nhận (scan 2026-09-15, đã verify bằng rg)

Repo hiện có các thư mục nội dung chính:

- `动态规划系列/` (18 file .md)
- `算法思维系列/` (17 file .md)
- `数据结构系列/` (15 file .md)
- `高频面试系列/` (16 file .md)
- `技术/` (7 file .md)
- `多语言解法代码/` (2 file .md)
- `pictures/` (66 thư mục con + 9 file ảnh gốc — giữ nguyên tuyệt đối)
- `README.md`, `STATE.md` ở root — giữ nguyên, không rename
- `docs/` chỉ chứa spec nội bộ (`docs/superpowers/specs/`), không phải content dịch — loại khỏi phạm vi rename

Kết quả scan quan trọng (quyết định độ phức tạp của plan):

- **0 link Markdown nội bộ** trỏ tới file `.md` khác (không có `](./`, `](../*.md)` nào).
  → Rename KHÔNG cần cập nhật link chéo giữa các bài viết.
- Tham chiếu `.md` duy nhất trong repo: 1 URL GitHub trong
  `多语言解法代码/contribution-guide.md` (dạng percent-encoded
  `.../blob/master/%E5%A4%9A.../solution_code.md`).
- Ảnh dùng 2 dạng: relative `../pictures/...` (phụ thuộc vị trí file, không phụ
  thuộc tên file nếu giữ cùng cấp thư mục) và absolute
  `https://labuladong.online/algo/images/...` (không bị ảnh hưởng bởi rename).
- `.github/` không chứa tên path tiếng Trung nào.
- ⚠️ Cây làm việc đang BẨN (~50 file dịch chưa commit). Bắt buộc commit
  translation trước (1 commit riêng), rồi mới rename (1 commit riêng sau).
  Không rename trên cây bẩn — diff sẽ không review được.

## Nguyên tắc bất biến

1. Không đổi tên, di chuyển hoặc xóa `pictures/` trong phase rename nội dung.
2. Không đổi tên thư mục con hoặc file nào bên trong `pictures/` trong phase này.
3. Không thay đổi nội dung code, nội dung bài viết, ảnh, URL LeetCode hoặc URL website.
4. Không đổi độ sâu của file Markdown sau khi rename.
5. Mọi rename dùng `git mv`, không copy-delete thủ công.
6. Tên mới dùng tiếng Anh ASCII, kebab-case, không dấu, không khoảng trắng; giữ đuôi `.md`.
7. Tên mới phải duy nhất trong từng thư mục.
8. Commit translation trước, rename sau — 2 commit riêng biệt, không trộn.
9. File `README.md` ở root và ở các thư mục series giữ nguyên tên (quy ước GitHub
   render); chỉ rename thư mục cha của chúng.
10. Marker `...的多语言解法👇/👆` trong `solution_code.md` là quy ước parser theo
    format dòng, không phụ thuộc path file — rename file không làm hỏng marker.
    Nhưng sau rename phải grep xác nhận không còn tooling nào trỏ path cũ
    (hiện tại chỉ có 1 URL trong contribution-guide).

## Phạm vi rename

### 1. Thư mục series

| Tên hiện tại | Tên mới đề xuất |
|---|---|
| `动态规划系列/` | `dynamic-programming/` |
| `算法思维系列/` | `algorithm-patterns/` |
| `数据结构系列/` | `data-structures/` |
| `高频面试系列/` | `interview-patterns/` |
| `技术/` | `computer-science/` |
| `多语言解法代码/` | `multi-language-solutions/` |

`pictures/` giữ nguyên hoàn toàn.

### 2. File Markdown

Lập một mapping đầy đủ trước khi rename. Một số mapping mẫu:

- `算法思维系列/双指针技巧.md` → `algorithm-patterns/two-pointers.md`
- `算法思维系列/滑动窗口技巧进阶.md` → `algorithm-patterns/sliding-window-advanced.md`
- `算法思维系列/BFS框架.md` → `algorithm-patterns/bfs-framework.md`
- `算法思维系列/回溯算法详解修订版.md` → `algorithm-patterns/backtracking.md`
- `算法思维系列/二分查找详解.md` → `algorithm-patterns/binary-search.md`
- `算法思维系列/前缀和技巧.md` → `algorithm-patterns/prefix-sum.md`
- `算法思维系列/差分技巧.md` → `algorithm-patterns/difference-array.md`
- `动态规划系列/动态规划详解进阶.md` → `dynamic-programming/dp-framework.md`
- `动态规划系列/编辑距离.md` → `dynamic-programming/edit-distance.md`
- `动态规划系列/背包问题.md` → `dynamic-programming/knapsack.md`
- `动态规划系列/团灭股票问题.md` → `dynamic-programming/stock-problems.md`
- `数据结构系列/dijkstra算法.md` → `data-structures/dijkstra.md`
- `数据结构系列/二叉树系列1.md` → `data-structures/binary-tree-part-1.md`
- `数据结构系列/二叉树系列2.md` → `data-structures/binary-tree-part-2.md`
- `数据结构系列/拓扑排序.md` → `data-structures/topological-sort.md`
- `高频面试系列/LRU算法.md` → `interview-patterns/lru-cache.md`
- `高频面试系列/接雨水.md` → `interview-patterns/trapping-rain-water.md`
- `高频面试系列/随机权重.md` → `interview-patterns/random-pick-with-weight.md`
- `多语言解法代码/solution_code.md` → `multi-language-solutions/solution-code.md`
- `多语言解法代码/contribution-guide.md` → `multi-language-solutions/contribution-guide.md`

Các file còn lại phải được bổ sung vào mapping trước khi thực thi; không rename theo phỏng đoán hoặc rename một phần không có bảng đối chiếu.
Tổng cộng ~89 file `.md` tracked — mapping phải đủ 89 dòng (trừ các `README.md`
giữ nguyên và file meta root). Kiểm tra collision 2 lớp: trùng tên trong cùng
thư mục, và trùng tên khi bỏ qua chữ hoa/thường (tránh gãy checkout trên
macOS/Windows, ví dụ `BST1.md` vs `bst1.md`).

## Quy trình thực hiện

### Phase -1 — Commit translation và tạo điểm rollback (bắt buộc, hay bị bỏ sót)

1. Commit toàn bộ thay đổi dịch hiện tại thành 1 commit riêng trên nhánh `vi`.
   Không push khi chưa có human duyệt (quy định repo).
2. Tạo branch backup trỏ đúng commit này (ví dụ `backup/vi-pre-rename`) để có
   đường lui 1 lệnh nếu rename sai.
3. Từ đây về sau mọi phase rename nằm trong 1 commit riêng thứ hai.

### Phase 0 — Inventory và tạo mapping

1. Liệt kê toàn bộ file `.md` và thư mục cần rename.
2. Tạo bảng mapping `old path -> new path` đầy đủ, không trùng tên.
3. Tham chiếu cần phân loại (đã scan baseline 2026-09-15 — nhẹ hơn tưởng):
   - Ảnh tương đối `../pictures/...` — tự đúng sau rename, không cần sửa.
   - Ảnh absolute `labuladong.online/algo/images/...` — không bị ảnh hưởng.
   - Link Markdown nội bộ tới file `.md` — hiện tại = 0, chỉ cần re-scan xác nhận.
   - 1 URL GitHub percent-encoded trong contribution-guide — cập nhật ở Phase 4.
   - Tham chiếu trong `README.md`, `.github/` — scan xác nhận không có path Hán.
4. Dừng nếu mapping thiếu file, có collision (kể cả case-insensitive), hoặc phát
   hiện consumer phụ thuộc vào tên cũ mà chưa có kế hoạch cập nhật.

### Phase 1 — Kiểm tra baseline trước rename

Tạo danh sách tham chiếu trước khi đổi tên:

- Tất cả image target trong Markdown.
- Tất cả relative target bắt đầu bằng `./` hoặc `../`.
- Tất cả URL GitHub trỏ tới repo này.
- Danh sách file ảnh thực tế trong `pictures/`.

Kiểm tra mọi target ảnh tồn tại trên filesystem. Lưu kết quả làm baseline để so sánh sau rename.

### Phase 2 — Rename thư mục ngoài `pictures/`

1. Rename các thư mục series bằng `git mv`.
2. Giữ nguyên cấp thư mục: file Markdown vẫn nằm ngay dưới thư mục series tương ứng.
3. Không rename `pictures/` và không rename thư mục con của `pictures/`.
4. Nếu có link nội bộ dùng path tương đối tới file Markdown, cập nhật link theo mapping.
5. Cập nhật các link GitHub hard-code trong `multi-language-solutions/contribution-guide.md` sau khi thư mục/file đích mới đã tồn tại.

### Phase 3 — Rename file Markdown

1. Rename từng file theo mapping bằng `git mv`.
2. Giữ nguyên đuôi `.md` và vị trí tương đối so với `pictures/`.
3. Không sửa target ảnh chỉ vì tên file Markdown thay đổi; `../pictures/...` vẫn giữ nguyên.
4. Cập nhật mọi link Markdown nội bộ nếu inventory Phase 0 phát hiện có.
5. Cập nhật text hiển thị trong tài liệu hướng dẫn nếu nó còn ghi tên cũ, nhưng không thay đổi URL ngoài không liên quan.

### Phase 4 — Xử lý tham chiếu GitHub

`multi-language-solutions/contribution-guide.md` hiện có URL GitHub hard-code tới
(dạng percent-encoded):

```text
.../blob/master/%E5%A4%9A%E8%AF%AD%E8%A8%80%E8%A7%A3%E6%B3%95%E4%BB%A3%E7%A0%81/solution_code.md
```

Sau rename, cập nhật thành path mới (nhớ encode lại, không paste path thô):

```text
.../blob/master/multi-language-solutions/solution-code.md
```

Lưu ý branch: URL dùng `blob/master` nhưng việc rename đang làm trên nhánh `vi`.
Link mới chỉ hoạt động sau khi `vi` được merge/push lên `master` của remote.
Nếu chưa merge, ghi chú rõ trong commit message thay vì để link gãy âm thầm.

### Phase 5 — Kiểm chứng sau rename

Thực hiện đủ các kiểm tra sau:

1. **Kiểm tra ảnh:** Với mỗi `![](...)` dùng path tương đối, resolve path dựa trên vị trí file Markdown mới và xác nhận file target tồn tại.
2. **Kiểm tra link nội bộ:** re-scan xác nhận vẫn 0 link `.md` nội bộ bị hỏng.
3. **Kiểm tra link GitHub:** không còn URL GitHub trong repo trỏ tới path cũ (kể cả dạng percent-encoded `%E5%A4%9A...`), trừ khi đó là link lịch sử có chủ ý.
4. **Kiểm tra ảnh không bị đụng:** `pictures/` vẫn có cùng danh sách file/thư mục và cùng nội dung/hash như baseline.
5. **Kiểm tra tên cũ:** `git ls-files | rg '[\p{Han}]'` chỉ còn trả về nội dung `pictures/`; mọi path `.md` ngoài `pictures/` phải ASCII thuần.
6. **Kiểm tra Git:** `git status`/`git diff --summary` phải nhận diện các thay đổi là rename (similarity ~100%, vì chỉ đổi path không đổi nội dung), không phải mất ảnh hoặc xóa hàng loạt. Nếu similarity thấp bất thường, dừng và kiểm tra.
7. **Kiểm tra Markdown:** Render hoặc preview README và một file từ mỗi thư mục; xác nhận ảnh hiển thị, code fence và bảng không bị hỏng.
8. **Kiểm tra case-insensitive:** liệt kê toàn bộ filename thường hóa (lowercase) và xác nhận không trùng — tránh lỗi chỉ lộ ra khi checkout trên macOS/Windows.

## Tiêu chí hoàn thành

- Translation đã commit riêng trước rename; có branch backup.
- Mapping đủ ~89 file, không collision (cả case-insensitive).
- Tất cả file/thư mục nằm trong mapping đã được rename đúng.
- Không có target ảnh tương đối bị thiếu.
- Không có link nội bộ bị hỏng (baseline đã là 0).
- Không còn link GitHub hard-code trỏ tới path cũ (kể cả percent-encoded) ngoài các trường hợp được ghi rõ.
- `git ls-files` không còn path Hán nào ngoài `pictures/`.
- `pictures/` và toàn bộ nội dung bên trong không thay đổi.
- Git nhận diện rename với similarity cao.
- README và preview mẫu hiển thị ảnh bình thường.

## Quyết định an toàn

Đổi tên file Markdown và các thư mục series là an toàn **chỉ khi giữ nguyên cấu trúc cấp thư mục**. Không đổi tên `pictures/` hoặc bất kỳ thư mục con/file ảnh nào trong cùng đợt. Nếu sau này muốn dịch tên thư mục ảnh sang tiếng Anh, đó phải là một phase riêng với mapping và cập nhật toàn bộ `../pictures/<subdirectory>/...` trước khi commit.
