---
name: Unit Testing (Vitest)
description: Tiêu chuẩn viết unit test đảm bảo tính đúng đắn của logic nghiệp vụ.
---

# 🧪 Unit Testing Guidelines (Vitest)

Bộ tiêu chuẩn giúp đảm bảo logic của ứng dụng luôn chạy đúng như thiết kế, hỗ trợ refactor an toàn và giảm thiểu lỗi regression.

## 1. PHẠM VI TEST (TESTING SCOPE)
AI Agent **bắt buộc** phải viết unit test cho các thành phần sau:
- **Utility Functions:** Các hàm xử lý dữ liệu, định dạng (formatters), tính toán logic độc lập.
- **Domain Services:** Các phương thức trong Service xử lý logic nghiệp vụ (business logic) trước khi gửi/nhận từ API.
- **Custom Composables:** Các logic state phức tạp được tách ra từ component Vue.
- **Xử lý Error:** Test xem hệ thống có throw lỗi đúng như mong đợi khi gặp data sai không.

*(Lưu ý: Hạn chế viết unit test cho UI component nếu nó chỉ hiển thị đơn thuần, ưu tiên test logic).*

## 2. QUY TRÌNH & CẤU TRÚC FILE
- **Vị trí:** File test phải nằm ngay cạnh file logic được test hoặc trong thư mục `__tests__` cùng cấp.
- **Đặt tên:** `[filename].spec.ts` hoặc `[filename].test.ts`.
- **Cấu trúc test case (AAA Pattern):**
  - **Arrange:** Chuẩn bị dữ liệu mẫu (mock data), khởi tạo đối tượng.
  - **Act:** Thực thi hàm/phương thức cần test.
  - **Assert:** Kiểm tra kết quả trả về có khớp với mong đợi không.

## 3. TIÊU CHUẨN VIẾT TEST
- **Sử dụng Mocks/Spies:** Sử dụng `vi.mock()` hoặc `vi.spyOn()` từ Vitest để giả lập các dependencies (như API Client) để test logic độc lập hoàn toàn với network.
- **Mô tả rõ ràng:** `describe` và `it/test` phải mô tả đúng hành vi bằng tiếng Anh hoặc tiếng Việt chuyên môn (Ví dụ: `it('should return total price correctly including tax', () => { ... })`).
- **Coverage:** Ưu tiên cover các trường hợp biên (Edge cases), giá trị rỗng (null/undefined), và logic rẽ nhánh (`if/else`).
- **Độc lập:** Mỗi test case phải độc lập, không phụ thuộc vào kết quả của test case trước đó.

## 4. CÔNG CỤ (TOOLS)
- **Framework:** Vitest (mặc định cho Vue 3 / Vite).
- **Assertion:** Sử dụng `expect(...).toBe(...)`, `toEqual`, `toContain`, `toThrow`...
- **Mocking:** Sử dụng Vitest Mock Functions.
