---
name: API Integration
description: Hướng dẫn chuẩn (Best practices) để gọi và tích hợp API sử dụng kiến trúc BaseClient trong Frontend (Repo-Agnostic)
---

# API Integration Skill

Hướng dẫn quy trình chuẩn để gọi và tích hợp API trong các dự án Frontend áp dụng cấu trúc lõi `BaseClient`. Hướng dẫn này mang tính tổng quát (repo-agnostic) giúp maintainer và developer giữ sự thống nhất, dễ bảo trì ở bất kỳ repo nào sử dụng kiến trúc Layer này.

## 1. Tìm hiểu tầng Core Base Client (Kiến trúc chuẩn)

Trong kiến trúc này, toàn bộ request không dùng qua trực tiếp thư viện `axios` hay `fetch` ở tầng UI component. Thay vào đó, chúng ta có các "HTTP Clients" đã được cấu hình sẵn tại tầng Core/Utils (ví dụ: `src/core/api/` hoặc `src/utils/http/`).

Các Clients này đóng vai trò là instance của một `BaseClient` được wrapper lại để thực hiện:

- **BaseURL**: Router API tự động nhận diện theo môi trường (`env`).
- **Authorization & Headers Interceptors**: Tự gán các header cần thiết (ví dụ `Authorization: Bearer <token>`, `app-id`...).
- **Xử lý lỗi tập trung (Error Interceptor)**: Đánh chặn các mã lỗi HTTP (400, 401, 403, 422...), có thể tự động gọi hàm refresh token hoặc tự pop-up hiển thị thông báo lỗi bằng Notification/Toast.

Mọi request đi qua BaseClient đều được chuẩn hóa kết quả đầu ra về một interface duy nhất (thường là `IResponse`):

```ts
interface IResponse<T = any> {
  success: boolean;
  error: boolean;
  data: T;
  statusCode?: number;
  message?: string;
  rawResponse?: any;
}
```

## 2. Các bước tích hợp API chuẩn Pro

### Bước 1: Tạo/Sửa Service lớp Domain

1. Xác định entity. Tạo hoặc mở tệp dịch vụ trong thư mục chứa logic nghiệp vụ (ví dụ `src/services/` hoặc phân theo module `src/modules/.../services/`).
2. Import `apiClient` phù hợp từ lõi dự án (`@/core`, `@/utils/http`, v.v.).
3. Viết một `Class` bao gồm các `methods` bất đồng bộ trả về một khối `Promise<IResponse>`.

```ts
// Ví dụ: src/services/featureService.ts
import { apiClient } from "@/core/api"; // Đường dẫn import tùy dự án
import { IResponse } from "@/interfaces";

class FeatureService {
  /**
   * Gọi API lấy danh sách items
   */
  async getItems(params: any): Promise<IResponse> {
    return await apiClient.get("/v1/feature/items", { params });
  }

  /**
   * Gọi API tạo mới item
   */
  async createItem(data: any): Promise<IResponse> {
    return await apiClient.post("/v1/feature/items", data);
  }
}

// Khởi tạo và export Singleton instance
export const featureService = new FeatureService();
```

_Lưu ý: Luôn nhóm các API endpoints liên quan tới cùng một mục đích (Entity/Domain) vào một Object Class và export chung, chia để trị._

### Bước 2: Gọi tính năng tại UI Layer (Component)

Tại component (Vue/React code), chỉ được phép gọi các methods bên trong Domain Service vừa tạo.
**Chú ý cực kỳ quan trọng:** Vì `BaseClient` ở tầng lõi đã được thiết kế sẵn interceptor để tự hiển thị thông báo Alert/Toast lỗi nếu API thất bại, bạn CHỈ CẦN tập trung xử lý cho nhánh `success`.

```vue
<script lang="ts" setup>
import { ref } from "vue";
import { featureService } from "@/services/featureService"; // Import Logic Layer

const isLoading = ref(false);
const items = ref([]);

const fetchItems = async () => {
  isLoading.value = true;

  // Payload trích xuất được pass thẳng vào Service
  const { data, error, message } = await featureService.getItems({
    page: 1,
    limit: 10,
  });

  // Logic rẽ nhánh: Xử lý khi thành công
  if (error) return;
  // KHÔNG CẦN khối ELSE để thông báo lỗi HTTP.
  // Component bắt Error chủ yếu trong các use-case cần fallback Component State nội bộ.

  items.value = data;
  isLoading.value = false;
};
</script>
```

## 3. Best Practices (Quy chuẩn tuyệt đối)

1. **Naming Service mô tả rõ ngụ ý**: Đặt tên Service bằng danh từ chỉ Domain hoặc Module (vd: `orderService`, `userAccountService`, `paymentService`). Tránh dùng hậu tố chung chung hoặc đặt lẫn lộn tính năng.
2. **Không tự spawn Client mới**: Tuyệt đối không tự import thư viện HTTP (như `axios`) để gọi trực tiếp ở Component. Làm vậy sẽ phá vỡ hệ thống Auth token tự động cũng như luồng báo lỗi tổng. Chỉ sử dụng Instance cung cấp sẵn (như `apiClient`, `authClient`).
3. **Mã hoá URL Path (No String concat in UI)**: File UI không bao giờ được chứa path API (`/api/v1/something...`). Mọi URLs đều được encapsulation hoàn toàn tại `Services`.
4. **Sử dụng DTOs (Data Transfer Objects)**: Hạn chế thấp nhất việc dùng kiểu `any`. Chủ động định nghĩa TS Types/Interfaces cho `Request Params` và `Response Data` để Typescript báo lỗi ngay từ khâu compile nếu truyền sai param.
