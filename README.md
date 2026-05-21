# Claude SEO Workspace

Personal workspace cho SEO content workflow của Seongon Content, dựng bằng [Claude Code](https://claude.com/claude-code) skills + MCP integrations.

## Structure

```
.claude/skills/             # 4 personal skills (project-level)
├── review-outline/          # Duyệt outline/bài CTV theo rubric user-centric 7 chiều
├── write-content/           # Lên outline đáp ứng search intent (Google Drive MCP)
├── brand-image/             # Tạo image theo brand guideline (Canva MCP)
└── chen-anh-bai/            # Auto-chèn ảnh vào bài SEO trong Google Doc (Drive MCP + Unsplash + Canva/Gemini)

outputs/                    # File output từ chạy skills
├── outline-cach-cham-soc-mat-v2.md           # Outline mẫu (chạy /write-content)
├── chat-history-2026-05-18.txt               # Lịch sử trò chuyện (/export) — 3 skills đầu
├── chen-anh-bai-chat-history-2026-05-21.txt  # Lịch sử trò chuyện (/export) — build chen-anh-bai + run thật với Doc Elite Dental
└── red-flags-infographic.png                 # Infographic Red Flags (chạy /brand-image qua Canva)
```

## 4 Skills

### 1. `review-outline` — Duyệt outline/bài CTV

Đầu vào: outline hoặc bài viết kèm keyword chính.
Output: điểm /70 theo 7 dimension (user-centric, không technical SEO), 3 strength + 5 issue cụ thể, verdict ACCEPT/REVISE/REJECT.

**Files**:
- `SKILL.md` — quy trình + pre-delivery checklist + common pitfalls + output format
- `rubric.md` — 7 dimension × 10đ (search intent, depth, actionability, trust, originality, readability, CTA)
- `checklist.md` — pre-publish user-centric check
- `examples/good-review.md` — review mẫu reference

### 2. `write-content` — Lên outline / viết bài SEO

Đầu vào: keyword chính + intent + audience.
Output: outline đầy đủ đáp ứng search intent, vượt top 10. Tích hợp Google Drive MCP để load research + save Google Doc.

**Files**:
- `SKILL.md` — quy trình + load Google Drive MCP + output format
- `intent-guide.md` — decision tree 4 loại intent
- `examples/sample-outline.md` — outline mẫu đầy đủ 2,800 từ

### 3. `brand-image` — Tạo image theo brand guideline

Đầu vào: loại image + headline + bài liên quan.
Output: design Canva + PNG export theo brand guideline (colors / fonts / logo). Tích hợp Canva MCP.

**Files**:
- `SKILL.md` — quy trình + Canva MCP call sequence
- `brand-guidelines/` — 4 file: colors.md, fonts.md, logo-usage.md, tone.md (fill với brand Trung tâm Mắt Hồng Ngọc)
- `templates/` — 3 template: blog-hero (1200×628), social-post (1080×1080), infographic (1080×1920)

### 4. `chen-anh-bai` — Auto-chèn ảnh vào bài SEO trong Google Doc

Đầu vào: link Google Doc (share `anyone with link`) + keyword chính + brand preset (Hồng Ngọc / Seongon / Custom / Elite Dental...).
Output: Insertion plan markdown table + ảnh thật (Unsplash/Pexels) + gen prompts cho Canva/Gemini cho infographic. Tùy chọn build HTML có ảnh chèn sẵn → upload Drive thành Doc mới.

**Files**:
- `SKILL.md` — pipeline 7 bước (parse → đọc Doc curl → tính số ảnh → phân loại → fetch/gen → caption → output)
- `EVALS.md` — 3 test scenario (Golden/Edge/Anti)
- `references/image-checklist.md` — UX rules + vị trí + caption + brand định vị
- `references/insertion-plan-format.md` — markdown table format chính xác cho output
- `references/brand-presets.md` — preset Hồng Ngọc + Seongon + Elite Dental + template Custom

**Tích hợp ngoài**: Google Drive MCP (read/write Doc) + curl Bash (CLI, đọc Doc export public) + WebFetch/WebSearch (Unsplash) + invoke `/brand-image` + `/seo-image-gen`.

## Tiêu chí mỗi SKILL.md

- [x] Quy trình từng bước rõ ràng + cụ thể
- [x] Chỉ dẫn file bổ trợ load-on-demand
- [x] Common pitfalls (vết xe đổ) với Why + Fix
- [x] Pre-delivery checklist
- [x] Output Format rõ
- [x] Frontmatter đầy đủ (name, description, user-invokable, argument-hint, license, compatibility, metadata)

## Cách sử dụng

1. Cài [Claude Code](https://claude.com/claude-code)
2. Clone repo này
3. Mở Claude Code trong dir chứa `.claude/`
4. Gõ `/review-outline` / `/write-content` / `/brand-image` để invoke skill
5. Tùy chọn: kết nối MCP Google Drive + Canva trong Claude Settings → Connectors để tận dụng full flow

## Workflow tham khảo (chạy thực tế)

### Workflow 1: 3 skills đầu (review + write + brand-image)

Outline `outputs/outline-cach-cham-soc-mat-v2.md` là kết quả của workflow:

1. CTV submit outline v1 (Google Doc) → user mở Claude Code
2. `/review-outline` → chấm v1, ra điểm 43/70 + 5 fix
3. `/seo-sxo cách chăm sóc mắt` → fetch top 10 live, confirm gap
4. `/write-content` → ra outline v2 đáp ứng 5 fix + lấp gap top 10
5. Save xuống Markdown để upload Google Drive cho CTV
6. `/brand-image` → tạo Red Flags infographic (Canva MCP, brand Hồng Ngọc)

Toàn bộ flow + AI conversations xem trong `outputs/chat-history-2026-05-18.txt`.

### Workflow 2: chen-anh-bai (auto-chèn ảnh bài SEO)

`outputs/chen-anh-bai-chat-history-2026-05-21.txt` ghi lại flow tạo skill `chen-anh-bai` + run thật:

1. `/create-skill` → tạo skill chen-anh-bai theo 6 tiêu chí Seongon standard
2. Iterate skill 3 lần (rename `inserting-blog-images` → `chen-anh-bai`, đổi flow paste-1-lần → AskUserQuestion, bỏ MCP Drive auth → curl export URL)
3. `/chen-anh-bai <Google Doc URL Elite Dental "Trồng răng implant có đau không">` → run thật
4. Brand "Elite Dental Custom" extract HEX từ logo user share (primary #C2185B magenta + grey #6B6B6B)
5. Skill đọc Doc 2030 từ, tính 6 ảnh, build HTML có 1 ảnh Unsplash thật + 5 placeholder brand → output `/tmp/clean-elite.html` user drag-drop Drive convert thành Doc mới

## Lưu ý

- 4 skills này được build cho Seongon Content workflow (SEO content tiếng Việt cho client y tế).
- Brand-guidelines/* của `brand-image` đã fill với info Trung tâm Mắt Hồng Ngọc (extract từ logo, không phải brand book chính thức) — cần refine cho project khác.
- `chen-anh-bai/references/brand-presets.md` có preset Hồng Ngọc (đầy đủ) + Seongon (placeholder fill khi dùng lần đầu) + Elite Dental Group (đầy đủ, extract từ logo).
- Memory feedback "user-centric review" (bỏ tiêu chí technical SEO) đã apply trực tiếp vào rubric của `review-outline`.

## License

MIT (skills + workflow). Outline content + chat history là property của Seongon Content + client.
