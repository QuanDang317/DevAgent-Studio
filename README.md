# Team AI Studio (`Team-Agents`) — Local-First AI Engineering Workspace

**Team AI Studio** là nền tảng đa tác tử (Multi-Agent) chạy cục bộ (**local-first**, không bắt buộc Docker hay database rời, hỗ trợ chạy trực tiếp trên Windows hoặc đóng gói chạy trên Docker khi cần) giúp đội ngũ 5 AI Agent phối hợp **lập trình, đọc hiểu tài liệu (PDF/Word/Excel + Hybrid RAG), phân tích dataset thực tế (DuckDB/Pandas/Charts), nhìn ảnh chụp màn hình (Multimodal Vision), xử lý media tùy chọn** — kèm lớp **phân quyền 3 cấp, Audit Log, Git Diff/Rollback và Automated Test Runner** có thể kiểm soát & kiểm chứng toàn diện.

> 📘 **Tài liệu nghiên cứu kiến trúc chi tiết toàn bộ dự án, luồng xử lý & các Phase 0–4:** xem tại **[ARCHITECTURE.md](./ARCHITECTURE.md)**.

---

## ✨ Năng lực chính (Hoàn thiện đủ Phase 0 → Phase 4)

| Nhóm năng lực | Chi tiết tính năng đã triển khai |
| :--- | :--- |
| **🛡️ Phase 0 — Tin cậy, Bảo mật & Điều phối** | • **Tool Permission Engine 3 cấp (`LOW` / `MEDIUM` / `HIGH`)** thực thi cứng ở backend theo vai trò Agent & `owned_paths`, lưu đầy đủ **Audit Log** đã ẩn bí mật.<br>• **Task-Based Orchestration (`Task Graph + Task Contract`)** & **Adaptive Routing** (tác vụ nhỏ chỉ điều động 1–2 Agent thay vì gọi cả 5).<br>• **SQLite Persistent Job Queue (`bg_worker`)** xử lý tác vụ nặng chạy nền, tự phục hồi job bị gián đoạn khi khởi động lại.<br>• Tùy chọn tách API keys sang vùng bí mật riêng (`%LOCALAPPDATA%\CrewStudio\secrets.env` trên Windows hoặc `/app/data/secrets.env` trong Docker). |
| **💻 Phase 1 — AI Coding Workspace & Git Safety** | • **Sửa code an toàn (`edit_file`)** kiểm tra `expected_sha256`, khớp duy nhất 1 vị trí, ghi atomic & trả diff; tìm kiếm toàn repo (`grep_search`), đọc theo dòng (`read_file_lines`), soi DB SQLite chỉ đọc (`inspect_sqlite`), test API local (`http_request`).<br>• **Git Worktree + Diff Review + Merge/Discard** và **Git Snapshot & Rollback** 1 click cho chế độ không dùng worktree.<br>• **Automated Test Runner** tự phát hiện `unittest`/`pytest`/`tsc`/`npm test` kèm Windows Job Object chống kẹt tiến trình.<br>• **Project Memory** có `source_ref` + kiểm tra tính mới (`stale`), **Live Web Preview** cô lập (`sandbox="allow-scripts"` + CSP `connect-src 'none'`). |
| **📚 Phase 2 — Đọc PDF/Word/Excel & Local Hybrid RAG** | • Đọc trực tiếp `.pdf` (kèm phát hiện PDF scan), `.docx`, `.xlsx`, code/Markdown và xuất báo cáo Word `.docx` (`export_docx_report`).<br>• **Local Hybrid RAG trên SQLite**: kết hợp **SQLite FTS5 (BM25)** + **Vector Embedding 768 chiều** (`gemini-embedding-001` MRL → fallback Ollama `nomic-embed-text` → fallback FTS5 offline), gộp điểm **RRF**, **Evidence Gate** chống ảo giác, bọc `<retrieved_context trust="untrusted">` và tab **Kho tri thức (RAG)** trên Run Center. |
| **📊 Phase 3 — Dataset Engine & Multimodal Vision** | • **Dataset Inspector & DuckDB SQL**: `profile_dataset` (CSV/Excel/Parquet/JSONL gọn nhẹ không tràn token), `query_dataset_sql` (`read_xlsx`/openpyxl fallback, khóa cứng `SET enable_external_access = false;` chống đọc file ngoài), `run_data_script` (Matplotlib `Agg`, tự gom biểu đồ `.png`/`.svg` hiện thẳng trong khung chat).<br>• **Multimodal Vision**: tải lên ảnh chụp màn hình lỗi UI / sơ đồ kiến trúc ngay trên khung chat để Agent nhìn trực tiếp và sửa code. |
| **🎬 Phase 4 — Optional Media Pack & Workflow Templates** | • **Media tùy chọn**: Chỉnh sửa/nén/ghép ảnh cục bộ bằng **Pillow**, tạo ảnh AI (**Gemini Nano Banana 2.1**), tạo giọng đọc WAV (**Gemini 3.8 Flash TTS**), và **FFmpeg có cấu trúc** (`trim`, `compress`, `extract_audio`, `burn_subtitles`, `slideshow`) tự động nhận diện (không lỗi nếu máy chưa cài `ffmpeg`).<br>• **Workflow Templates**: 5 mẫu quy trình thực chiến sẵn có + tự tạo mẫu riêng, khởi chạy 1 click từ Chat hoặc Run Center. |

---

## 🚀 Cách chạy nhanh

### Cách 1: Chạy trực tiếp trên Windows (Mặc định — Nhanh & Nhẹ nhất)
1. Nháy đúp **`start.bat`** (hoặc chạy `.\start.ps1` trong PowerShell).
2. Mở trình duyệt tại **http://localhost:8899**.
3. Bấm biểu tượng **Cài đặt ⚙️** (góc dưới bên trái):
   - Dán **Gemini API Key** (lấy miễn phí tại https://aistudio.google.com/apikey) vào mục *API Keys* rồi bấm **Lưu**.
   - *(Tùy chọn)* Thêm **Groq API Key** (https://console.groq.com/keys), **OpenRouter API Key** hoặc **Ollama Local** làm dự phòng.
   - *(Tùy chọn)* Bật **Chế độ Demo** để trải nghiệm ngay mà chưa cần API key.
4. Dừng ứng dụng bằng **`stop.bat`**. Nếu có sửa mã nguồn giao diện `frontend/src`, chạy **`rebuild-ui.bat`** để build lại `frontend/dist`.

### Cách 2: Chạy bằng Docker (Có sẵn luôn `git` và `ffmpeg` trong container)
Phù hợp khi bạn muốn cô lập hoàn toàn môi trường thực thi hoặc cần sẵn **FFmpeg** cho tác vụ video/audio:

```bash
# 1. (Tùy chọn) Tạo file .env từ mẫu và điền API key
copy .env.example .env

# 2. Build và khởi chạy (chỉ bind về 127.0.0.1:8899 trên máy của bạn)
docker compose up --build -d

# 3. Xem log & kiểm tra trạng thái
docker compose logs -f

# 4. Dừng container (dữ liệu trong ./data và ./workspace vẫn giữ nguyên)
docker compose down
```

- Mở trình duyệt tại **http://localhost:8899**.
- Dữ liệu bền vững được mount ra máy host tại `./data` (SQLite DB + `secrets.env`) và `./workspace` (mã nguồn, tài liệu, file upload).
- Muốn container gọi **Ollama đang chạy trên máy Windows host**, đặt trong `.env`:
  ```env
  OLLAMA_BASE_URL=http://host.docker.internal:11434/v1
  ALLOW_REMOTE_OLLAMA=1
  ```
- Muốn ảnh Docker nhẹ hơn (~150MB) khi không dùng FFmpeg, đổi `INSTALL_FFMPEG: "0"` trong `docker-compose.yml`.

---

## 🧪 Chạy bộ kiểm thử tự động

Dự án có bộ **47 bài kiểm thử tự động** (`unittest`, chạy cô lập trên DB/thư mục tạm, không chạm dữ liệu thật) bao phủ bảo mật, Permission Engine, Git Worktree/Snapshot, RAG + Benchmark, DuckDB Isolation, Vision, Media & Templates:

```powershell
# Kiểm thử Backend (47 tests)
$env:PYTHONDONTWRITEBYTECODE="1"
.\.venv\Scripts\python.exe -B -m unittest -v backend/tests/test_security_and_core.py

# Kiểm tra TypeScript & build Frontend
cd frontend
npx tsc --noEmit
npm run build
```

---

## 📂 Tổng quan cấu trúc thư mục

```text
Team-Agents/
├── backend/                     # FastAPI, WebSockets, Orchestrator, 26 Tools, RAG, DuckDB, Media
│   ├── main.py                  # Entrypoint API REST + WebSocket + lifespan worker + static SPA
│   ├── orchestrator.py          # Task Graph, Adaptive Routing, Git Snapshot/Worktree, Approvals
│   ├── llm_gateway.py           # Gemini Interactions API (Text/Vision), Groq, OpenRouter, Ollama
│   ├── permission_engine.py     # Phân quyền Tool 3 cấp (LOW/MEDIUM/HIGH) + Audit Redaction
│   ├── tools.py                 # Registry 26 Tools & ToolExecutionContext
│   ├── code_tools.py            # edit_file (SHA-256), read_file_lines, grep, sqlite RO, HTTP
│   ├── worktree_manager.py      # Git Worktree Diff/Merge & Git Snapshot/Rollback
│   ├── test_runner.py           # Automated Test Runner + Windows Job Object
│   ├── doc_parser.py            # Đọc PDF/DOCX/XLSX/Code & xuất báo cáo Word (.docx)
│   ├── rag_service.py           # Local Hybrid RAG (SQLite FTS5 + 768d Vector RRF + Benchmark)
│   ├── data_tools.py            # DuckDB SQL (external access off), Pandas Profile, Matplotlib Agg
│   ├── media_tools.py           # Pillow, Gemini Nano Banana 2.1, Gemini 3.8 TTS, FFmpeg tùy chọn
│   ├── workflow_templates.py    # 5 mẫu quy trình thực chiến + CRUD
│   ├── project_memory.py        # Bộ nhớ dự án có kiểm chứng nguồn & phát hiện stale
│   ├── job_worker.py            # SQLite Persistent Background Job Queue
│   ├── database.py              # SQLite WAL + 16 bảng & additive migrations
│   ├── security.py              # Session Token, Host/DNS Rebinding Guard, CORS, Env Sanitizer
│   ├── ws_manager.py            # WebSocket broadcast manager (per-socket lock & timeout)
│   ├── terminal_ws.py           # Terminal WebSocket (hỗ trợ kill cây tiến trình giữa chừng)
│   └── tests/                   # 47 bài test bảo mật, đồng thời, RAG, Data, Media & tích hợp
├── frontend/                    # React 19 + TypeScript + Vite + Tailwind CSS
├── data/                        # SQLite DB (team_agents.db) — nằm trong .gitignore
├── workspace/                   # Thư mục dự án & _uploads/ cho Agent thao tác
├── Dockerfile                   # Multi-stage build (Node 22 + Python 3.13 + non-root + FFmpeg)
├── docker-compose.yml           # Cấu hình chạy Docker an toàn bind 127.0.0.1
├── .dockerignore                # Chặn .env, DB, node_modules, .venv lọt vào Docker image
├── ARCHITECTURE.md              # Sổ tay kiến trúc, luồng xử lý, vai trò & lịch sử Phase 0–4
└── start.bat / start.ps1 / stop.bat / rebuild-ui.bat
```
