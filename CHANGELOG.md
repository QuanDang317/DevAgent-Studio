# 📝 Changelog — DevAgent Studio

Lịch sử phát triển và các mốc kiểm định kỹ thuật của **DevAgent Studio**.

---

## [v2.0.0] — AI Engineering Workspace (Full Roadmap V2)

### Phase 4 — Optional Media Pack, Workflow Templates & Docker
- Bổ sung `media_tools.py`: xử lý ảnh cục bộ bằng **Pillow** (`resize`, `crop`, `convert`, `thumbnail`, `watermark`, `grid`, giới hạn `MAX_IMAGE_PIXELS = 40M`), tạo ảnh AI (**Gemini Nano Banana 2.1**), tạo giọng đọc WAV (**Gemini 3.8 Flash TTS**) và 5 thao tác **FFmpeg có cấu trúc** ở chế độ tùy chọn tự phát hiện.
- Bổ sung `workflow_templates.py` với 5 mẫu quy trình thực chiến sẵn có, CRUD mẫu riêng và khởi chạy 1 click từ khung Chat hoặc Run Center.
- Nâng cấp `ArtifactDrawer` phát `<audio>`/`<video>` và `ChatArea` phát audio inline + chọn nhanh mẫu quy trình.
- Đóng gói tùy chọn bằng `Dockerfile` (multi-stage Node 22 + Python 3.13 + non-root `crewuser` + `ffmpeg`), `.dockerignore` và `docker-compose.yml` bind an toàn về `127.0.0.1`.
- **Kiểm chứng:** `47/47` unit/security/integration tests PASS; Independent QA & Security Audit đạt `0` lỗi P0/P1/P2.

### Phase 2 & 3 — Document Intelligence, Local Hybrid RAG, DuckDB & Multimodal Vision
- Bổ sung `doc_parser.py` đọc trực tiếp `.pdf` (kèm phát hiện bản scan), `.docx`, `.xlsx`, code/Markdown và xuất báo cáo Word `.docx`.
- Bổ sung `rag_service.py`: Local Hybrid RAG trên SQLite (**FTS5 BM25** + **Embedding 768d MRL** + **NumPy RRF** + **Evidence Gate** + `<retrieved_context trust="untrusted">` + `benchmark_rag`).
- Bổ sung `data_tools.py`: `profile_dataset`, `query_dataset_sql` (DuckDB `enable_external_access = false`, `read_xlsx`/openpyxl fallback) và `run_data_script` (`MPLBACKEND=Agg`, tự gom biểu đồ `.png`/`.svg` hiển thị inline trong Chat).
- Bổ sung **Multimodal Vision** cho Gemini Interactions API & OpenAI-compat (kèm fallback text-only) và **Dataset Inspector + DuckDB SQL** trực tiếp trong `WorkspacePanel`.
- **Kiểm chứng:** `42/42` tests PASS; RAG benchmark mẫu đạt `Recall@5 = 1.0`, `MRR = 1.0`, độ trễ ~12ms.

### Phase 0 & 1 — Security Governance, Task Graph, Coding Tools & Git Safety
- Bổ sung **Tool Permission Engine 3 cấp (`LOW`/`MEDIUM`/`HIGH`)**, bảng `audit_logs` ẩn bí mật, vùng lưu `secrets.env` tách biệt và **SQLite Persistent Job Queue (`bg_worker`)**.
- Nâng cấp Orchestrator sang **Task Graph + Task Contract** và **Adaptive Routing**; cộng dồn `tokens_total`/`cost_total` cho từng Run.
- Bổ sung `code_tools.py` (`edit_file` SHA-256 + diff, `read_file_lines`, `grep_search`, `inspect_sqlite` chỉ đọc, `http_request` chống SSRF), **Git Snapshot & Rollback**, **Automated Test Runner** (Windows Job Object), **Project Memory** có kiểm tra `stale`, upload cách ly `_uploads/`, và **Live HTML Preview** cô lập CSP.
- **Kiểm chứng:** `35/35` tests PASS; `npm run build` PASS.

---

## [v1.1.0] — Security & Reliability Hardening
- Vá lỗ hổng bỏ qua `SESSION_TOKEN` khi thiếu header, thêm `Host` header guard chống DNS Rebinding, chặn SSRF `OLLAMA_BASE_URL` và khóa ranh giới chọn Workspace.
- Khắc phục zombie process trên Windows (`taskkill /T /F`), tách luồng thực thi `/ws/terminal` cho phép `kill` giữa chừng, chuyển Git/I/O nặng sang `asyncio.to_thread`, khử N+1 SQLite và sửa race condition Frontend.
- **Kiểm chứng:** Nâng bộ test cô lập từ `12` lên `22` tests PASS.

## [v1.0.0] — Initial Release
- Khởi tạo ứng dụng web cục bộ FastAPI + React + SQLite với 5 AI Agents, phòng họp chung, chat 1-1, Kanban, Workspace, Terminal WebSocket, LLM Gateway (Gemini/Groq/OpenRouter/Ollama) và Quota Panel.
