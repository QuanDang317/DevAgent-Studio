# ✨ Feature Matrix — DevAgent Studio

Tài liệu liệt kê minh bạch trạng thái từng tính năng trong **DevAgent Studio**, phân tách rõ giữa **Đã hoàn thiện & Kiểm thử (Implemented)**, **Module Tùy chọn (Optional)** và **Định hướng mở rộng (Planned)**.

---

## 1. Trạng Thái Tính Năng Chi Tiết

| Nhóm | Tính năng | Trạng thái | Ghi chú kiểm chứng |
| :--- | :--- | :---: | :--- |
| **Điều phối Multi-Agent** | Phòng họp nhóm & Chat 1-1 với 5 Agent tùy chỉnh | ✅ Implemented | Hỗ trợ `@Tên` (lọc code block chống gọi nhầm) & `ask_teammate` |
| **Điều phối Multi-Agent** | Task Graph, Task Contract & Adaptive Routing | ✅ Implemented | Tự điều phối 1–3 Agent theo độ phức tạp; theo dõi `tokens_total`/`cost_total` |
| **Điều phối Multi-Agent** | Workflow Templates (5 mẫu sẵn + CRUD tùy chỉnh) | ✅ Implemented | Bảo vệ mẫu mặc định, khởi chạy 1 click từ Chat & Run Center |
| **Bảo mật & Quản trị** | Tool Permission Engine 3 cấp (`LOW` / `MEDIUM` / `HIGH`) | ✅ Implemented | Thực thi cứng ở backend cho 26 tools, chặn `owned_paths` traversal `..` |
| **Bảo mật & Quản trị** | Audit Log tự ẩn bí mật (`api_key`, `token`, `password`) | ✅ Implemented | Lưu SQLite, hiển thị trực tiếp trong Run Center |
| **Bảo mật & Quản trị** | Session Token, Host/DNS Rebinding & SSRF Guard | ✅ Implemented | Xác thực vô điều kiện mọi REST & WebSocket nhạy cảm |
| **Bảo mật & Quản trị** | Tách khóa API sang vùng bí mật `secrets.env` | ✅ Implemented | Hỗ trợ di dời 1 click từ Settings hoặc mount `/app/data/secrets.env` |
| **Lập trình & Git Safety** | `edit_file` kiểm tra SHA-256 + khớp duy nhất + trả Diff | ✅ Implemented | Ghi atomic qua temp file + retry Windows `PermissionError` |
| **Lập trình & Git Safety** | `grep_search`, `read_file_lines`, `inspect_sqlite` (RO), `http_request` | ✅ Implemented | Chặn tự gọi cổng Crew Studio & chặn IP LAN/Metadata |
| **Lập trình & Git Safety** | Git Worktree cô lập + Diff Viewer + Merge / Discard | ✅ Implemented | Hỗ trợ tên file tiếng Việt (`core.quotePath=false`) |
| **Lập trình & Git Safety** | Git Snapshot & Rollback cho chế độ không dùng Worktree | ✅ Implemented | Chỉ hoàn tác thay đổi của Run, bảo toàn file untracked cũ |
| **Lập trình & Git Safety** | Automated Test Runner (`unittest`/`pytest`/`tsc`/`npm test`) | ✅ Implemented | Gắn Windows Job Object chống zombie process |
| **Lập trình & Git Safety** | Project Memory có `source_ref` + tự kiểm tra `stale` | ✅ Implemented | Tự đánh dấu cũ khi file nguồn đổi SHA-256 hoặc bị xóa |
| **Lập trình & Git Safety** | Live HTML Web Preview cô lập | ✅ Implemented | `<iframe sandbox="allow-scripts">` (cấm `allow-same-origin`) + CSP `connect-src 'none'` |
| **Hạ tầng nền** | SQLite Persistent Job Queue & Local Worker (`bg_worker`) | ✅ Implemented | Claim atomic `BEGIN IMMEDIATE`, phục hồi job `interrupted` sau restart |
| **Tài liệu & Hybrid RAG** | Đọc `.pdf` (kèm phát hiện PDF scan), `.docx`, `.xlsx`, code | ✅ Implemented | Hỗ trợ xuất báo cáo Word `.docx` atomic |
| **Tài liệu & Hybrid RAG** | Local Hybrid RAG (SQLite FTS5 BM25 + Embedding 768d + RRF) | ✅ Implemented | Evidence Gate chống ảo giác; benchmark mẫu `Recall@5 = 1.0`, `MRR = 1.0` |
| **Dataset & Vision** | Dataset Inspector & DuckDB SQL (`profile_dataset`, `query_dataset_sql`) | ✅ Implemented | Hỗ trợ `read_xlsx` + fallback `openpyxl`, khóa `enable_external_access = false` |
| **Dataset & Vision** | Chạy script phân tích & vẽ biểu đồ Matplotlib (`Agg`) inline | ✅ Implemented | Đòi duyệt cấp `HIGH`, tự gom `.png`/`.svg` hiển thị ngay trong Chat |
| **Dataset & Vision** | Multimodal Vision (gửi ảnh chụp màn hình lỗi UI / sơ đồ) | ✅ Implemented | Gemini Interactions API + OpenAI-compat kèm fallback text-only |
| **Media & Đóng gói** | Chỉnh sửa ảnh cục bộ Pillow & Tạo ảnh/TTS Gemini | ✅ Implemented | Nano Banana 2.1 (`gemini-nano-banana-2.1`) + Gemini 3.8 Flash TTS (`.wav`) |
| **Media & Đóng gói** | Xử lý Video/Audio FFmpeg có cấu trúc (5 thao tác an toàn) | 🧩 Optional | Tự phát hiện `ffmpeg` trên host (cài sẵn trong Docker image) |
| **Media & Đóng gói** | Docker Multi-Stage (`Dockerfile` + `docker-compose.yml`) | 🧩 Optional | Non-root user, bind `127.0.0.1`, `no-new-privileges`, `HEALTHCHECK` |
| **Định hướng tiếp theo** | OCR cục bộ tự động cho PDF scan không có text layer | 🗓️ Planned | Hiện đã tự phát hiện PDF scan và gợi ý đọc qua Vision |
| **Định hướng tiếp theo** | Cross-Encoder Reranking cho kho tài liệu > 50.000 chunks | 🗓️ Planned | Chỉ kích hoạt sau khi benchmark chứng minh tăng MRR thực tế |
| **Định hướng tiếp theo** | Tích hợp MCP (Model Context Protocol) Client mở rộng | 🗓️ Planned | Tận dụng ranh giới Tool Registry & Permission Engine sẵn có |
