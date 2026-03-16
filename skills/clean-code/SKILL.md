---
name: Clean Code
description: Best practices cho việc viết mã nguồn sạch và dễ bảo trì
---

# Clean Code Guidelines

1. **Naming Conventions:**
   - Sử dụng tên biến, hàm, class có ý nghĩa rõ ràng.
   - Hàm (`function`) thường sử dụng động từ để bắt đầu (VD: `fetchData`, `handleUserClick`).

2. **Functions/Methods:**
   - Mỗi hàm chỉ nên làm một việc duy nhất.
   - Chiều dài của hàm nên được giữ ở mức tối thiểu, tránh phức tạp hóa.

3. **Comments:**
   - Hạn chế sử dụng comment khi không cần thiết, mã nguồn nên tự giải thích. Chỉ dùng comment nếu biểu thức hoặc logic không dễ đọc hoặc mang ý đồ thiết kế nghiệp vụ riêng.

4. **Code Review:**
   - Đảm bảo thực hiện auto-format trước khi commit (nếu có sử dụng prettier).
   - Loại bỏ các `console.log` thừa thải trên Production.
