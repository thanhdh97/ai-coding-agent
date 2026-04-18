---
name: Git & Commit Workflow
description: Tiêu chuẩn về cấu trúc Git Commit và thao tác quản lý phiên bản (Version Control).
---

# 🌳 Git & Commit Guidelines

Bộ tiêu chuẩn giúp lịch sử mã nguồn luôn chuyên nghiệp, rõ ràng, dễ trace bug và hỗ trợ tối đa việc tự động hóa tạo Changelog.

## 1. CHUẨN MỰC BẮT BUỘC (CONVENTIONAL COMMITS & JIRA TRACE)
Mọi lịch sử commit do AI tạo ra hoặc User tự viết đều phải tuân thủ chuẩn Conventional Commits kèm theo **Mã Task Jira** để tracking công việc.
**Định dạng bắt buộc:**
`<type>[optional scope]: [MÃ_TICKET] <description>`

*(Ví dụ chuẩn: `feat(auth): [CS-12348] add google standard oauth login`)*

**Các `<type>` quy định hợp lệ:**
- `feat:` Khi phát triển, thêm mới một tính năng (feature) hoàn chỉnh.
- `fix:` Khi vá (patch) bug cho một tính năng hiện có.
- `refactor:` Khi cấu trúc lại/tối ưu code mà không làm thay đổi behavior đầu ra (VD: tách component).
- `chore:` Các tác vụ bảo trì siêu nhỏ, cài đặt npm package mới, config linter.
- `style:` Sửa format code (khoảng trắng, dấu chấm phẩy, tab).
- `docs:` Thay đổi nội dung của các file tài liệu.

## 2. QUY TẮC MÔ TẢ MESSAGE
- **Bắt buộc có tiền tố Jira:** AI khi đề xuất lệnh commit LUÔN nhắc User cung cấp, hoặc tự tìm mã JIRA (như `CS-12348`) từ tên branch để chèn vào message.
- **Ngắn gọn & Vào thẳng vấn đề:** Phần description sau mã Jira không nên dài quá 70 ký tự.
- **Có Context:** Tránh tuyệt đối các message rác như `fix fix`, `up code`, `asdasd`.
- **Scope (Tùy chọn):** Gắn module thao tác để khoanh vùng chức năng (Ví dụ: `fix(payment): [CS-55555] update currency format`).

## 3. QUY TRÌNH PHÂN NHÁNH VÀ MERGE (GIT FLOW)
Dự án áp dụng luồng làm việc (Git Flow) cực kỳ nghiêm ngặt:
- **Tạo nhánh Feature:** Luôn phải tạo nhánh `feature/<MÃ_SỐ>` (chỉ lấy phần số của task) tách ra từ nhánh gốc `master` (Ví dụ: `feature/12348`).
- **Tạo nhánh Local/Dev:** Để code thực tế, Dev rẽ một nhánh cá nhân từ nhánh feature theo cú pháp `<tên_dev>/<MÃ_SỐ>` (Ví dụ: `thanhdh/12348`).
- **Quy trình Merge:** 
  - Code xong ở local branch thì đẩy lại vào feature branch: `nhánh local => nhánh feature` để đem đi Testing.
  - **NGHIÊM CẤM:** Lập trình viên tuyệt đối **KHÔNG ĐƯỢC PHÉP** tự ý merge trực tiếp bất kỳ nhánh nào vào `master`.
- **Triển khai (Deploy):** Quyền merge vào `master` và release deploy được cấp độc quyền cho **Project Manager (PM)** sau khi test xong và có thông báo đẩy tính năng.
- Luôn nhớ `pull` code trước khi `push` và merge.

## 4. QUY TẮC CHO AGENT (Dành riêng cho Antigravity/AI Assistant)
- Khi User yêu cầu AI xử lý commit, AI **phải phân tích kỹ** các thay đổi đã thực hiện (sửa lỗi, thêm tính năng, refactor...).
- **Bước 2 (Xác nhận & Chỉnh sửa):** AI đặt câu hỏi xác nhận: *"Tôi đã soạn xong commit message theo chuẩn, bạn có muốn chỉnh sửa gì không hay nhập OK/Đồng ý để tôi commit luôn nhé?"*. 
    - Nếu User phản hồi "OK/Đồng ý": AI dùng message đã đề xuất ở Bước 1.
    - Nếu User cung cấp một message mới (VD: "Sửa lại thành: fix logic issue"): AI sẽ ghi nhận message mới này.
- **Bước 3 (Thực thi):** Ngay sau khi User chốt thông điệp cuối cùng (bằng cách nói OK hoặc cung cấp message mới), AI **CHỦ ĐỘNG** thực hiện chuỗi lệnh `git add .`, `git commit -m "..."` và `git push`. **Lưu ý:** AI đặt `SafeToAutoRun: true` cho lệnh này vì User đã duyệt nội dung commit ở bước trước, tránh việc hỏi đi hỏi lại gây rườm rà.

## 5. QUY TRÌNH TẠO MERGE REQUEST (MR) AUTOMATION
- **Luồng mặc định:** Sau khi commit và push code ở nhánh local thành công, AI Agent **chủ động** đặt câu hỏi: *"Bạn có muốn tôi tự động tạo Merge Request 1 (từ nhánh local lên nhánh feature) không?"*
- **Luồng gộp (Combo):** Nếu ngay từ đầu User yêu cầu gộp (VD: "Commit và tạo Merge Request cho tôi"), thì sau khi User duyệt Commit Message, AI sẽ **TỰ ĐỘNG** thực hiện chuỗi: `Add` ➔ `Commit` ➔ `Push` ➔ `Tạo MR 1` ➔ `Mở Browser` luôn mà **không cần hỏi lại**.
- **MR 1 (Local => Feature):** AI sử dụng GitLab CLI (`glab mr create...`). Ngay sau đó, AI bắt buộc phải dùng lệnh `glab mr view --web` hoặc `xdg-open` để bật MR lên trình duyệt.
- **MR 2 (Feature => Testing):** User tự thao tác thủ công sau khi MR 1 được merge.
