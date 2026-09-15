# STATE.md — fucking-algorithm CN→VI (nhánh vi)

## Tiến độ (scan cuối 2026-09-15)
- Baseline: 71.790 ký tự Hán / 76 file.
- Hiện tại: 5.358 ký tự / 31 file (raw).
- Đã dịch: 66.432 ký tự ≈ **92,5%**. Nội dung user-facing: **~100%**.
- Sót toàn keeps cố ý: 724 marker `...的多语言解法` (quy ước parser),
  path ảnh `pictures/...`, badge URL (`精品课程`, `B站`), param `?fname=...`,
  key minh họa grep/footer, + file meta (STATE.md, spec).

## Theo folder
- 技术/ + README: ✅ xong.
- 数据结构系列/: ✅ xong.
- 算法思维系列/: ✅ xong.
- 动态规划系列/: ✅ xong (A2+A3, indent giữ nguyên, pseudocode Hán → snake_case Việt).
- 高频面试系列/: ✅ xong (A4+A5+A6).
- 多语言解法代码/solution_code.md: ✅ xong (3.564 dòng, 72.142 dòng giữ nguyên,
  indent 0 lệch, URL nguyên 100%).

## Quy ước đã chốt
- Giữ nguyên tên file/folder. Chỉ dịch nội dung.
- Giữ nguyên indent comment code. URL/path/key nghi ngờ → giữ + REVIEW_REQUIRED.
- Spec: docs/superpowers/specs/2026-09-15-translation-design.md (chưa commit).

## Rename (2026-09-15, chưa commit — chờ human duyệt)
- Backup: `backup/vi-pre-rename`.
- 6 thư mục → EN + 72 file → kebab-case ASCII (78 staged renames,
  similarity 100%, 0 dòng nội dung đổi).
- GitHub URL trong contribution-guide đã cập nhật path mới.
- `pictures/` nguyên vẹn. `git ls-files` hết path Hán ngoài `pictures/`.
- Ảnh gãy 123 chỗ là tồn tại từ trước (ảnh không có trong repo), không do rename.
