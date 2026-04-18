---
name: Git & Commit Workflow
description: Tiêu chuẩn về cấu trúc Git Commit và thao tác quản lý phiên bản (Version Control).
---

# 🌳 Git & Commit Guidelines

Bộ tiêu chuẩn giúp lịch sử mã nguồn luôn chuyên nghiệp, rõ ràng, dễ trace bug và hỗ trợ tối đa việc tự động hóa tạo Changelog.

> [!IMPORTANT]
> **Quy tắc vàng:** Luôn `git pull --rebase` trước khi `push` để tránh tạo ra các merge commit không cần thiết và giữ lịch sử tuyến tính.

## 1. CHUẨN MỰC BẮT BUỘC (CONVENTIONAL COMMITS & JIRA TRACE)

Mọi commit phải tuân thủ chuẩn **Conventional Commits** kèm theo **Mã Ticket Jira** đầy đủ (bao gồm cả prefix).

**Định dạng bắt buộc:**
`<type>[optional scope]: [MÃ_TICKET] <description>`

*Ví dụ chuẩn: `feat(auth): [CS-12348] add google standard oauth login`*

**Các `<type>` quy định hợp lệ:**
- `feat:` Thêm mới một tính năng (feature) hoàn chỉnh.
- `fix:` Vá bug cho một tính năng hiện có.
- `refactor:` Tối ưu code mà không thay đổi behavior (VD: tách component).
- `chore:` Tác vụ bảo trì, cài đặt package, config linter.
- `style:` Sửa format code (khoảng trắng, tab, semi-colon).
- `docs:` Thay đổi nội dung tài liệu.

## 2. QUY TẮC MÔ TẢ MESSAGE
- **Tiền tố Jira:** Agent LUÔN phải tìm hoặc hỏi mã JIRA (VD: `CS-12348`) để chèn vào message.
- **Ngắn gọn:** Description không dài quá 72 ký tự.
- **Thì hiện tại:** Sử dụng câu mệnh lệnh (VD: "add", "fix", "update" thay vì "added", "fixed").
- **Scope (Tùy chọn):** Gắn module để khoanh vùng chức năng (VD: `fix(api): [CS-5555] fix timeout handling`).

## 3. QUY TRÌNH PHÂN NHÁNH (GIT FLOW)

> [!CAUTION]
> **NGHIÊM CẤM:** Tuyệt đối KHÔNG merge trực tiếp vào nhánh `master`. Chỉ PM mới có quyền này.

- **Nhánh Feature:** Tạo từ `master`, đặt tên theo cú pháp: `feature/<MÃ_SỐ_TICKET>` (Chỉ lấy phần số).
  - *VD: `feature/12348`*
- **Nhánh Local/Dev:** Tạo từ nhánh Feature, đặt tên: `<tên_dev>/<MÃ_SỐ_TICKET>`.
  - *VD: `thanhdh/12348`*
- **Luồng Merge:** `Local Branch` ➔ `Feature Branch` (qua MR) ➔ `Master` (Chỉ PM thực hiện).

## 4. QUY TẮC CHO AGENT (Antigravity Protocol)

Agent cần chủ động (Proactive) để giảm thiểu thao tác lặp lại cho User:

1. **Khảo sát & Chuẩn bị:** Tự động chạy `git status`, `git add .` và phân tích `git diff` để soạn message.
2. **Xác nhận & Thực thi:**
   - Agent soạn xong message và hỏi: *"Tôi đã soạn xong commit message theo chuẩn, bạn có muốn chỉnh sửa gì không hay nhập **'OK'** để tôi push luôn nhé?"*
   - Nếu User gõ **'OK'**: Agent tự động chạy `git commit -m "..."` và `git push` với `SafeToAutoRun: true`.
   - Nếu User cung cấp message mới: Agent dùng message đó để commit & push.

## 5. TỰ ĐỘNG HÓA MERGE REQUEST (MR)

Sau khi `push` thành công, Agent hỗ trợ tạo Merge Request theo quy chuẩn:

1. **Luồng mặc định (Hỏi trước):** Agent PHẢI hỏi: *"Bạn có muốn tôi hỗ trợ tạo Merge Request từ nhánh local lên feature ngay bây giờ không?"*
   - Chỉ khi User đồng ý (OK/Có), Agent mới thực hiện lệnh `glab mr create`.
2. **Luồng chủ động (Combo):** Chỉ khi ngay từ đầu User yêu cầu gộp (VD: *"Commit và tạo MR cho tôi"*), Agent mới thực hiện chuỗi lệnh `Add` ➔ `Commit` ➔ `Push` ➔ `Tạo MR` ➔ `Mở Browser` một cách tự động hoàn toàn.

**Sử dụng GitLab CLI (`glab`):**
```bash
# Lệnh mẫu Agent sẽ chạy
glab mr create --title "[CS-12348] <tóm tắt thay đổi>" \
               --description "Review và merge code từ nhánh local lên feature." \
               --target-branch feature/12348 \
               --fill
```

> [!TIP]
> Ngay sau khi tạo MR thành công, Agent dùng `glab mr view --web` để bật MR lên trình duyệt cho User.

