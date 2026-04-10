---
name: API Integration
description: Hướng dẫn chuẩn (Best practices) để gọi và tích hợp API sử dụng kiến trúc BaseClient trong Frontend (Repo-Agnostic)
---

# API Integration Skill (Standard)

Hướng dẫn quy trình chuẩn để gọi và tích hợp API sử dụng kiến trúc phân lớp, đảm bảo sự thống nhất tuyệt đối về cấu trúc và luồng dữ liệu (Flow) cho mọi dự án Frontend (Vue, Next.js, v.v.).

## 1. KIẾN TRÚC LAYERS (CORE ARCHITECTURE)

Trong kiến trúc này, chúng ta tách biệt hoàn toàn việc giao diện tương tác với mạng. Toàn bộ request được quản lý qua 3 lớp:

1.  **Base Client (Tầng lõi):** Các thực thể HTTP Client (axios/fetch) đã được cấu hình sẵn BaseURL, Interceptors (Auth, Headers) và xử lý lỗi tập trung.
2.  **Domain Service Layer (Tầng nghiệp vụ):** Các Class Singleton định nghĩa các phương thức gọi API cho từng thực thể (Entity/Domain).
3.  **UI Layer (Tầng giao diện):** Các thành phần giao diện chỉ được phép gọi đến Service Layer.

Mọi request đi qua hệ thống đều chuẩn hóa kết quả về Interface `IResponse`:

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

## 2. QUY TRÌNH TRIỂN KHAI CHUẨN

### Bước 1: Tạo Domain Service (Singleton Class)

Mọi API endpoint phải được đóng gói bên trong một Service Class. Cách triển khai này giống nhau hoàn toàn cho các nền tảng.

```ts
// src/services/userService.ts
import { apiClient } from '@/core/api'; // Client khởi tạo từ BaseClient
import { IResponse } from '@/interfaces';

class UserService {
  /**
   * Phương thức mẫu lấy thông tin profile
   */
  async getProfile(): Promise<IResponse> {
    return await apiClient.get('/v1/user/profile');
  }

  /**
   * Phương thức mẫu cập nhật profile
   */
  async updateProfile(data: any): Promise<IResponse> {
    return await apiClient.post('/v1/user/profile', data);
  }
}

// Luôn export một Singleton instance
export const userService = new UserService();
```

### Bước 2: Gọi từ UI Layer

Tại Component (Vue Component, React Component, Server Actions), chúng ta chỉ gọi đến instance của Service đã được export.

```ts
// Cách gọi chung cho mọi Framework
const handleFetchData = async () => {
  isLoading.value = true
  const { data, error, message: msg } = await userService.getProfile()..finally(() => (isLoading.value = false));;

  if (error) {
    return;
  }

  // Xử lý logic nghiệp vụ
  // BaseClient đã tự hiển thị Toast/Alert khi có lỗi HTTP (400, 500...),
  // Hiển thị thông báo thành công theo msg nếu cần (sử dụng message.success(msg) của antd)
};
```

## 3. CÁC QUY CHUẨN BẮT BUỘC (RESTRICTIONS)

1.  **Encapsulation**: File UI tuyệt đối không chứa đường dẫn API (`/api/v1/...`). Toàn bộ URL phải nằm trong Service.
2.  **No Direct HTTP Library**: Tuyệt đối không import trực tiếp `axios` hay `fetch` tại UI để gọi API. Phải dùng `apiClient` thông qua Service.
3.  **Type Safety**: Sử dụng Interface/DTO cho cả dữ liệu đầu vào và đầu ra của Service.
4.  **Naming Convention**: Đặt tên Service theo Domain (ví dụ: `orderService`, `authService`).
