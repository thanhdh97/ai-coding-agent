---
trigger: always_on 
---

# 🧠 MODEL ROUTING & TASK DELEGATION RULES

Bộ quy tắc này định nghĩa cách thức AI Agent nhận diện và phân bổ/deal công việc (delegate) cho từng model tuỳ theo mức độ phức tạp của task. Mục tiêu là tối ưu hóa hạn mức quota (rate limit/usage), đồng thời bắt buộc phải giữ vững được bộ "tiêu chuẩn cực kỳ khắt khe" (Clean Code, Kiến trúc v.v) của dự án.

## 1. PHÂN BỔ THEO NĂNG LỰC CÁC MODEL (MODEL CAPABILITIES)

**TUYỆT ĐỐI TUÂN THỦ** nguyên tắc: Chất lượng của mã nguồn (bug-free, chuẩn conventions, đúng cấu trúc) là số một. Nếu model được assign yếu hơn yêu cầu thực tế, phải ngay lập tức báo cho User hoặc chuyển task sang model cấp cao hơn.

### 🟢 1.1 TIER 1 (TỐI A SỨC MẠNH & TƯ DUY KIẾN TRÚC)
**Models:** `Claude Sonnet 4.6 (Thinking)`, `Claude Opus 4.6 (Thinking)`
Sử dụng Claude khi đối mặt với:
-   **Khởi tạo Kiến trúc / Refactor sâu:** Chuyển đổi mô hình component lớn, thiết kế Custom Composables, chia tách API module, hay xây dựng các hook phức tạp.
-   **Chẩn đoán Bug khó (Deep Debugging):** Các vấn đề về lifecycle, logic bất đồng bộ, race condition, hoặc memory leak không rõ nguyên nhân.
-   **Quyết định Thiết kế cốt lõi:** Khi cần xây dựng các Business Logic phức tạp nhất, tính năng thanh toán, bảo mật, hoặc những thứ nằm ngoài phạm vi boilerplate thông thường.
*(Chú ý: Claude Sonnet phù hợp nhất cho Code logic và Refactor tốc độ, Claude Opus nên dành cho quy hoạch lớn / Data Schema / System Design).*

### 🔵 1.2 TIER 2 (HIỆU SUẤT CAO & NGỮ CẢNH LỚN)
**Models:** `Gemini 3.1 Pro (High / Low)`
Sử dụng Gemini Pro 3.1 khi:
-   **Triển khai tính năng (Feature Implementation):** Khởi tạo màn hình mới chuẩn UI/UX, tích hợp Data API chuẩn theo Document, implement Form validation... 
-   **Phân tích & Review Code số lượng lớn:** Review một lượng file khổng lồ, viết hoặc dịch tài liệu kỹ thuật, bổ sung Test cases hoặc kiểm thử hành vi người dùng bằng trình duyệt.
-   **Thao tác Luồng GIT (Git Workflow):** Tổng hợp diff lớn, commit message tự động với tên branch hay phân tích Pull Request.

### 🟡 1.3 TIER 3 (TỐC ĐỘ DIỆN RỘNG & TÁC VỤ LẶP)
**Models:** `Gemini 3 Flash`
Chỉ sử dụng Flash cho các đầu việc quy trình ngắn hạn, rập khuôn để tiết kiệm quota:
-   Fix typo (sửa lỗi chính tả), format lại codebase, chỉnh sửa nhanh CSS/Inline Styles không ảnh hưởng cấu trúc.
-   Viết và bổ sung JSdoc, Code comments cho các file có sẵn.
-   Sửa UI nhẹ nhàng theo phản hồi ngắn. 
-   *NGHIÊM CẤM:* KHÔNG dùng Flash để thiết kế lại State Management (như Pinia) hay cấu trúc lại Services Layer. Flash sẽ dễ bị *hallucination* sinh ra code rác.

### 🔴 1.4 TIER 4 (SKIPPED / RỦI RO CHẤT LƯỢNG)
**Models:** `GPT-OSS 120B (Medium)`
-   **BỎ QUA LUÔN:** Tuyệt đối **KHÔNG SỬ DỤNG** Model này cho mọi tác vụ liên quan đến sửa đổi mã nguồn, gọi kiến trúc API theo BaseClient, hay xử lý Refactor.
-   *Lý do:* Model mã nguồn mở ở Tier này không đáp ứng được quy định "Nhất quán kỹ thuật hạng nặng" và dễ phá vỡ các *Clean Code Guidelines* dự án đã thiết lập.
-   *Ngoại lệ:* Chỉ nên dùng nếu User dùng để chat chit khái niệm, hỏi đáp ý nghĩa của các lệnh cơ bản, hoặc giải thích từ lóng.

## 2. QUY TRÌNH DELEGATE CHUẨN CỦA AGENT
Khi User ném vào một prompt:
1. Xác định "Độ khó kiến trúc" (Architecture Complexity). 
2. Match yêu cầu với một trong các định nghĩa ở `Mục 1`.
3. Nếu Agent hiện tại không đủ sức mạnh (vd: đang chạy ở Flash nhưng bị giao Debug sâu), hãy cảnh báo User bằng 1 dòng duy nhất: *Cảnh báo hạn chế Model: Yêu cầu của bạn cần dùng model mạnh hơn như Claude 4.6 hoặc Gemini Pro.* và từ chối thực hiện để tránh phá hỏng source code.
