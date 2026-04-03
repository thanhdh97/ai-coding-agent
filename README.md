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

## 🎯 Mục tiêu cốt lõi

1. **Chuẩn hóa (Standardization):** Khắc phục tình trạng sinh mã nguồn "rác" hay "placeholder", bắt buộc AI đồng nhất Design Pattern/Style Guide với team dev.
2. **Quản trị rủi ro (Risk Management):** Kiểm soát nghiêm ngặt các hành vi "ảo giác" (hallucination) của AI: cấm chế thư viện, bỏ thói quen dùng `any`, xử lý chặt XSS.
3. **Trải nghiệm Vibecoding (High Productivity):** Đẩy nhanh tốc độ phát triển nhờ việc AI đã thấu hiểu trước toàn bộ Context & Convention của dự án mà không cần nhắc lại mỗi lần chat.

## 🚀 Hướng dẫn tích hợp (Setup & Usage)

Để AI Assistant có thể "học" được bộ quy tắc này, cấu hình rất đơn giản:

1. Clone nội dung của repository này.
2. Đặt hợp nhất thư mục (`rules/`, `skills/`) trực tiếp vào vị trí khai báo dành riêng cho AI Workspace trong dự án của bạn (VD: thư mục quản lý `.agent` / `.gemini` hoặc tùy thuộc cấu hình IDE).
3. Agent khi được invoke sẽ lập tức kế thừa toàn bộ bộ DNA kỹ thuật này.

---
*Built with ❤️ for High-Performance Vibecoding.*
