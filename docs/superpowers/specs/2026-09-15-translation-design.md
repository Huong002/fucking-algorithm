# Translation Orchestration Spec — fucking-algorithm CN→VI

Date: 2026-09-15. Branch: `vi`. Approved by human: keep filenames, content only.

## Scope

- 76 `.md` files, ~71.8k CJK chars remaining (scan 2026-09-15, `/tmp/opencode/cjk-per-file.txt`).
- Content only. NO file/folder rename. NO code/logic/key/link changes.

## Workers (one folder each, never same file)

| Worker | Folder | Chars | Files |
|---|---|---|---|
| W1 | 技术/ + README.md | ~1.9k | 8 |
| W2 | 数据结构系列/ | ~3.3k | 15 |
| W3 | 算法思维系列/ | ~4.1k | 17 |
| W4 | 多语言解法代码/ | ~14.0k | 2 |
| W5 | 高频面试系列/ | ~21.2k | 16 |
| W6 | 动态规划系列/ | ~27.4k | 18 |

Batch 1 (this run): W1+W2+W3 (~9.2k chars). Batch 2+: W4, W5, W6 (heavy, separate runs — token budget).

## Rules per worker

1. Translate prose + code comments CN→VI, natural, technical context.
2. Glossary (mandatory): 数组→mảng, 对象→đối tượng, 函数→hàm, 数据库→cơ sở dữ liệu,
   缓存→bộ nhớ đệm, 组件→component, 请求→request, 响应→response.
   Keep English where standard: API, endpoint, cache, component, request, response.
3. NEVER touch: logic, identifiers, keys, imports, routes, links, paths, commands.
4. Preserve comment indentation exactly (pass-1 bug: 8 spaces collapsed to 1).
5. User-facing strings only if certain; else keep + `<!-- REVIEW_REQUIRED -->`.
6. No refactor, no bugfix, no rename.

## Review gate (orchestrator, after workers)

- `rg '[\p{Han}]'` rescan → delta per file.
- `git diff --stat` sanity + spot-check indent + links.
- Repo has no build/test → verification = CJK≈0 (outside whitelist) + diff clean.

## Self-review

- No TBD. Scope = one batch, not whole repo (budget). No contradictions.
- Ambiguity fixed: filenames frozen; strings uncertain → keep + mark.
