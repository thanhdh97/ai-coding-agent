# 🤖 VibeCoding AI Agent Base

[![AI Agent](https://img.shields.io/badge/Powered%20by-AI%20Agent-blue.svg)](https://github.com/features/copilot)
[![Clean Code](https://img.shields.io/badge/Standard-Clean%20Code-brightgreen.svg)]()
[![Vue 3](https://img.shields.io/badge/Framework-Vue%203-4FC08D.svg?logo=vue.js)]()

Bộ cấu hình cốt lõi dành riêng cho **AI Coding Assistant**, được thiết kế để định hướng AI hoạt động như một "Senior Developer" thực thụ. Bằng việc định nghĩa rõ ràng tư duy (Mindset) và phương pháp luận (Skills), repository này đảm bảo mọi dòng code do AI sinh ra đều đạt chuẩn khắt khe nhất của dự án.

## 📂 Kiến trúc hệ thống (Architecture)

Hệ thống điều khiển Agent được phân tách thành 2 module chính:

- 📜 **`/rules`**: Nền tảng tư duy và quy trình làm việc (Workflow).
  - Bắt buộc AI tuân thủ quy trình: Khảo sát -> Lên Kế Hoạch -> Triển khai 100% -> Tự kiểm tra.
  - *Xem chi tiết:* [`rules/GEMINI.md`](./rules/GEMINI.md)

- 🛠️ **`/skills`**: Bộ tiêu chuẩn kỹ thuật chuyên môn chuyên sâu.
  - **Clean Code** (`/skills/clean-code`): Quy tắc định danh (Naming), cấu trúc file, Strong Typing và tối ưu logic.
  - **Frontend Design** (`/skills/frontend-design`): Kiến trúc giao diện Vue 3 (Composition API, Pinia, UI/UX Micro-animations, Performance).
  - **API Integration** (`/skills/api-integration`): Kiến trúc BaseClient, xử lý lỗi tập trung, DTOs và nguyên tắc gọi API từ lớp Service.
  - **Git & Commit Workflow** (`/skills/git-workflow`): Chuẩn Conventional Commits, quy trình rẽ nhánh kiểm soát chặt chẽ tích hợp mã Jira.

## 🎯 Mục tiêu cốt lõi

1. **Chuẩn hóa (Standardization):** Khắc phục tình trạng sinh mã nguồn "rác" hay "placeholder", bắt buộc AI đồng nhất Design Pattern/Style Guide với team dev.
2. **Quản trị rủi ro (Risk Management):** Kiểm soát nghiêm ngặt các hành vi "ảo giác" (hallucination) của AI: cấm chế thư viện, bỏ thói quen dùng `any`, xử lý chặt XSS.
3. **Trải nghiệm Vibecoding (High Productivity):** Đẩy nhanh tốc độ phát triển nhờ việc AI đã thấu hiểu trước toàn bộ Context & Convention của dự án mà không cần nhắc lại mỗi lần chat.

## 🚀 Hướng dẫn tích hợp (Setup & Usage)

Để AI Assistant (đặc biệt là các Agent như **Antigravity**) có thể "gắn kết" và học được bộ quy tắc này, bạn có thể thiết lập theo các cách sau:

### Cách 1: Sử dụng Git Submodule (Khuyên dùng)
Đây là cách tối ưu nhất để đồng bộ và tái sử dụng bộ Agent trên nhiều dự án khác nhau. Ngay tại thư mục root của dự án mục tiêu, chạy:
```bash
git submodule add <link-github-repo-ai-agent> .agent
```
*(Đối với hệ thống Antigravity, bạn có thể dùng tên thư mục là `.agent` hoặc `.gemini`)*

### Cách 2: Copy thư mục thủ công
1. Clone repository này về máy.
2. Đặt hợp nhất thư mục (`rules/`, `skills/`) trực tiếp vào cấu trúc dự án của bạn (thường là thư mục quản lý như `.agent` / `.gemini` ở root).

### 💡 Thủ thuật "Prompt Injection" để khởi động (Rất quan trọng)
Dù bạn dùng cách tích hợp nào, ngay ở **prompt đầu tiên** khi bắt đầu một phiên làm việc mới (New Chat / New Task), hãy dán câu lệnh sau vào khung chat để đảm bảo Agent nắm rõ toàn bộ quy định trước khi vào việc:

> *"Tôi muốn thực hiện [viết mục tiêu của bạn vào đây]. Trước khi bắt đầu, hãy tìm, đọc và áp dụng triệt để nội dung tư duy trong `.agent/rules/GEMINI.md` cùng các tiêu chuẩn trong thư mục `.agent/skills/` làm cơ sở phân tích và đưa ra kế hoạch."*

---
*Built with ❤️ for High-Performance Vibecoding.*
