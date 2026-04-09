# 🧪 Unit Testing Guidelines (Next.js)

Bộ tiêu chuẩn giúp đảm bảo logic ứng dụng luôn chạy đúng, hỗ trợ refactor an toàn và giảm thiểu lỗi regression trong các dự án Next.js.

## 1. PHẠM VI TEST (TESTING SCOPE)
AI Agent **bắt buộc** phải viết unit test cho các thành phần sau:
- **Utility Functions:** Các hàm xử lý dữ liệu, formatters, tính toán logic độc lập.
- **Domain Services:** Các phương thức trong Service xử lý logic nghiệp vụ (business logic) trước khi gọi/nhận từ API.
- **Custom Hooks:** Logic state phức tạp được tách ra khỏi React components (tương đương Composables).
- **Xử lý Error:** Kiểm tra việc throw/catch lỗi đúng mong đợi khi gặp dữ liệu sai.

*(Lưu ý: Hạn chế viết unit test cho UI component nếu nó chỉ hiển thị đơn thuần, ưu tiên test logic).*

## 2. QUY TRÌNH & CẤU TRÚC FILE
- **Vị trí:** File test nằm trong thư mục `__tests__` cùng cấp.
- **Đặt tên:** `[filename].test.ts` hoặc `[filename].spec.ts`.
- **Cấu trúc test case (AAA Pattern):**
  - **Arrange:** Chuẩn bị dữ liệu mẫu (mock data), khởi tạo đối tượng/hook.
  - **Act:** Thực thi hàm/phương thức cần test.
  - **Assert:** Kiểm tra kết quả trả về có khớp với mong đợi không.

## 3. TIÊU CHUẨN VIẾT TEST
- **Sử dụng Mocks/Spies:** Sử dụng `vi.mock()` hoặc `vi.spyOn()` từ Vitest để giả lập các dependencies (như API Client) để test logic độc lập hoàn toàn.
- **Mô tả rõ ràng:** `describe` và `it/test` phải mô tả đúng hành vi bằng tiếng Anh hoặc tiếng Việt chuyên môn.
- **Coverage:** Ưu tiên cover các trường hợp biên (Edge cases), giá trị rỗng (null/undefined), và logic rẽ nhánh.
- **Độc lập:** Mỗi test case phải độc lập, không phụ thuộc vào kết quả của test case trước đó.

## 4. CÔNG CỤ (TOOLS)
- **Framework:** Vitest (Nhanh và tương thích tốt với Next.js hiện đại).
- **Assertion:** Sử dụng các hàm `expect` tiêu chuẩn của Vitest.
- **Mocking:** Sử dụng Vitest Mock Functions.
