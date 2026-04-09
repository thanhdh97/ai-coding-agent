# 🤖 VibeCoding AI Agent Base

[![AI Agent](https://img.shields.io/badge/Powered%20by-AI%20Agent-blue.svg)](https://github.com/features/copilot)
[![Clean Code](https://img.shields.io/badge/Standard-Clean%20Code-brightgreen.svg)]()
[![Next.js](https://img.shields.io/badge/Framework-Next.js-000000.svg?logo=next.js)]()

Bộ cấu hình cốt lõi dành riêng cho **AI Coding Assistant**, được thiết kế để định hướng AI hoạt động như một "Senior Developer" thực thụ. Bằng việc định nghĩa rõ ràng tư duy (Mindset) và phương pháp luận (Skills), repository này đảm bảo mọi dòng code do AI sinh ra đều đạt chuẩn khắt khe nhất của dự án, hỗ trợ đa nền tảng (**Vue 3** & **Next.js**).

## 📂 Kiến trúc hệ thống (Architecture)

Hệ thống điều khiển Agent được phân tách thành 2 module chính:

- 📜 **`/rules`**: Nền tảng tư duy, quy trình làm việc (Workflow) và cấu hình ngôn ngữ.
  - Bắt buộc AI tuân thủ quy trình: Khảo sát -> Lên Kế Hoạch -> Triển khai 100% -> Tự kiểm tra.
  - *Xem chi tiết:* [`rules/GEMINI.md`](./rules/GEMINI.md), [`rules/LANGUAGE.md`](./rules/LANGUAGE.md)

- 🛠️ **`/skills`**: Bộ tiêu chuẩn kỹ thuật chuyên môn chuyên sâu.
  - **Clean Code** (`/skills/clean-code`): Quy tắc định danh, cấu trúc file, Strong Typing cho mọi dự án.
  - **API Integration** (`/skills/api-integration`): Kiến trúc BaseClient & Service Layer chuẩn (Sử dụng chung cho Vue/Next.js).
  - **Frontend Design**: Kiến trúc giao diện cho [**Vue 3**](./skills/frontend-design) hoặc [**Next.js**](./skills/frontend-design-nextjs).
  - **Git & Commit Workflow** (`/skills/git-workflow`): Chuẩn Conventional Commits.

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
Dù bạn dùng cách tích hợp nào, ngay ở **prompt đầu tiên** khi bắt đầu một phiên làm việc mới, hãy sử dụng mẫu lệnh phù hợp với dự án của bạn để "kích hoạt" Agent:

- **Dành cho dự án Vue 3:**
  > *"Tôi muốn thực hiện [mục tiêu]. Dự án sử dụng **Vue 3**. Hãy đọc và áp dụng triệt để quy tắc trong `.agent/rules/GEMINI.md` cùng các Skill trong thư mục `.agent/skills/` (ưu tiên các bản cho Vue và bỏ qua các file có đuôi `-nextjs`)."*

- **Dành cho dự án Next.js:**
  > *"Tôi muốn thực hiện [mục tiêu]. Dự án sử dụng **Next.js**. Hãy đọc và áp dụng triệt để quy tắc trong `.agent/rules/GEMINI.md` cùng các Skill trong thư mục `.agent/skills/` (ưu tiên các bản có đuôi **-nextjs** cho phần Frontend)."*

---
*Built with ❤️ for High-Performance Vibecoding.*
