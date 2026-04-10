---
name: Clean Code
description: Best practices cho việc viết mã nguồn sạch và dễ bảo trì
---

# Clean Code Guidelines

1. **Naming Conventions:**
   - Sử dụng tên biến, hàm, class có ý nghĩa rõ ràng, tránh viết tắt khó hiểu.
   - **Variables/Functions:** Sử dụng `camelCase`.
     - Biến (`variable`) thường bắt đầu bằng danh từ (VD: `userName`, `totalPrice`).
     - Hàm (`function`) thường bắt đầu bằng động từ (VD: `fetchData`, `handleUserClick`).
   - **Booleans:** Bắt đầu bằng `is`, `has`, `can`, `should` (VD: `isLoading`, `hasError`, `canSubmit`).
   - **Classes / Components / Interfaces / Types:** Sử dụng `PascalCase` (VD: `UserProfile`, `Button`, `IUserRegistration`).
   - **Constants:** Sử dụng `SCREAMING_SNAKE_CASE` (VD: `MAX_LOGIN_ATTEMPTS`, `API_URL`).
   - **Folders & Files:** Cần có tính nhất quán (VD: Component file dùng `PascalCase`, các file logic khác dùng `camelCase`).

2. **Functions/Methods:**
   - Hạn chế hàm quá dài, nên tách hàm nếu vượt qua 50 dòng logic.
   - Mỗi hàm chỉ nên làm một việc duy nhất.
   - Tối đa 3 tham số.
   - No Side Effects.

3. **Comments:**
   - Hạn chế sử dụng comment khi không cần thiết, mã nguồn nên tự giải thích. Chỉ dùng comment nếu biểu thức hoặc logic không dễ đọc hoặc mang ý đồ thiết kế nghiệp vụ riêng.

4. **Tránh Magic Values:**
   - Không sử dụng các giá trị số, chuỗi, boolean mà không có giải thích.
   - Thay vào đó, sử dụng hằng số (constant) hoặc biến có ý nghĩa.
   - Tốt: `const MAX_LOGIN_ATTEMPTS = 3;`
   - Không tốt: `if (attempts > 3) { ... }`

5. **Early Return:**
   - Sử dụng early return để giảm độ sâu của code.
   - Tránh lồng ghép điều kiện quá sâu (nested if) - tối đa 2 level.

6. **DRY (Don't Repeat Yourself):**
   - Tránh lặp lại code.
   - Nếu có code lặp lại, hãy tách ra thành hàm riêng hoặc sử dụng Hooks.

7. **Import Style:**
   - Tránh import sâu.
   - Tốt: `import { Button } from "@/components";`
   - Không tốt: `import Button from '../../../../../components/Button';`

8. **Tư duy lập trình:**
   - Luôn suy nghĩ về việc tối ưu hóa code.
   - Luôn suy nghĩ về việc tái sử dụng code.
   - Luôn suy nghĩ về việc bảo trì code.

9. **Data Types / Strong Typing:**
   - Nếu dự án sử dụng TypeScript, hạn chế tối đa việc sử dụng `any`.
   - **Explicit Typing:** Khai báo rõ interface, type cho:
     - Dữ liệu trả về từ API.
     - Props / Params của Component và Function.
     - **Biến nội bộ (Local variables) & Hằng số:** Tuyệt đối không để kiểu ngầm định nếu giá trị khởi tạo không đủ để TS suy luận chính xác (ví dụ: khởi tạo mảng rỗng `[]` hoặc `null`).

10. **Library & Framework:**
    - **Native First:** Ưu tiên sử dụng các phương thức Native JS (ES6+) cho các tác vụ đơn giản.
    - **Lodash:** Chỉ sử dụng Lodash cho các xử lý phức tạp (VD: `cloneDeep`, `debounce`, `throttle`, `merge`...).
    - **Tree-shaking:** Luôn import cụ thể từng hàm từ thư viện để tối ưu bundle size (VD: `import get from 'lodash/get'`).
    - **Project Utilities:** Luôn kiểm tra và ưu tiên sử dụng các Utils/Hooks đã có sẵn trong dự án trước khi tìm giải pháp bên ngoài.
    - **Strict Policy:** Tuyệt đối không tự ý cài đặt thêm thư viện mới (`npm install`) mà không qua thảo luận và đồng ý của User.

11. **Code Structuring & Files:**
    - Giữ file nhỏ gọn, tập trung.
    - Một file chỉ nên chứa một Component hoặc một logic group duy nhất (1 File = 1 Component).
    - Tính nhất quán trong cách đặt tên thư mục và file phải được tuân thủ tùy theo tiêu chuẩn của framework.
    - **Feature Module:** Khi thêm mới 1 tính năng, phải tạo 1 module mới chuyên biệt. Cấu trúc thư mục của một feature module tiêu chuẩn nên bao gồm:
      ```text
      src/modules/FeatureName/
      ├── components/       # Các UI components dùng riêng cho module này
      │   └── index.ts      # File export các component con (Barrel Export) để hỗ trợ Import Style
      ├── constants/        # Các hằng số, cấu hình tĩnh riêng của module
      ├── types/            # Định nghĩa TypeScript interfaces/types tương ứng
      ├── helpers/          # (hoặc utils) Các hàm tiện ích, xử lý logic nội bộ
      ├── hooks/            # Custom hooks chứa logic hoặc xử lý API/Data nội bộ
      ├── pages/            # (Tùy chọn) Chứa các component đại diện cho từng trang nếu module có nhiều màn hình
      └── index.ts          # File export các thành phần ra bên ngoài (Public API)
      ```
    - **Folder-only Component & Barrel Export:** 
      - Các component phải được tổ chức theo dạng thư mục riêng (ví dụ: `components/Header/index.tsx`). 
      - Thư mục cha (như `components/`) nên có một file `index.ts` để gom các component lại nhằm tránh import sâu theo quy tắc số 7.
      - **Component Granularity (Quy mô Component):** 
        - Với các Component nhỏ/phụ trợ (Sub-components) bên trong một Module lớn: Giữ Logic (hooks), Types, và Helpers ở tầng Module cha (`src/modules/FeatureName/hooks/`, `src/modules/FeatureName/types/`) thay vì tạo các file này bên trong folder component. Việc này giúp folder component chỉ tập trung vào UI (Dumb/Presentational) và tránh làm rác thư mục.
        - Chỉ khi Component trở nên cực kỳ phức tạp hoặc là một Feature độc lập, mới cân nhắc tách logic/types vào folder riêng của nó.

12. **Formatting, Linting & Code Review:**
    - Đảm bảo thực hiện auto-format trước khi commit (sử dụng prettier/eslint).
    - Mã nguồn nên được review chéo (Cross-review) trước khi merge.
