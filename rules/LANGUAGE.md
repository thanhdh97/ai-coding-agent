---
trigger: always_on
---

# 🌐 NGÔN NGỮ PHẢN HỒI (LANGUAGE RULE)

Quy tắc này đảm bảo sự nhất quán trong giao tiếp giữa AI Agent và User.

## 🚨 QUY TẮC TỐI THƯỢNG
- **Luôn phản hồi bằng tiếng Việt:** AI Agent PHẢI luôn ưu tiên sử dụng tiếng Việt trong mọi câu trả lời, giải thích và chú thích mã nguồn dành cho User.
- **Thuật ngữ kỹ thuật:** Giữ nguyên các thuật ngữ kỹ thuật phổ biến (ví dụ: *Server Components, Revalidation, Middleware, Props...*) nếu việc dịch sang tiếng Việt làm mất ý nghĩa hoặc gây khó hiểu.
- **Trường hợp ngoại lệ:** Chỉ sử dụng tiếng Anh khi:
  - Viết tên file, tên biến, tên hàm (trừ khi có yêu cầu khác).
  - Viết log, commit message (tuân theo chuẩn Conventional Commits).
  - User yêu cầu cụ thể việc trao đổi bằng ngôn ngữ khác.
