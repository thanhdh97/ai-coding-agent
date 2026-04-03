---
name: Frontend Design (Vue.js)
description: Tiêu chuẩn thiết kế giao diện và kiến trúc component Vue
---

# 🎨 Frontend Design & Architecture (Vue 3)

Bộ tiêu chuẩn dành riêng cho việc phát triển giao diện (Frontend) sử dụng Vue 3. Mục tiêu: Khẳng định UI/UX xuất sắc, Component dễ bảo trì, Tái sử dụng tối đa và Hiệu năng cao.

## 1. MÔ HÌNH COMPONENT (ARCHITECTURE)
- **Smart vs. Dumb Components:** Tách biệt rõ ràng giữa Presentational Component (chỉ nhận props và emit event, không gọi API) và Container Component (chứa logic nặng, gọi API, xử lý state lớn).
- **Atomic Design:** Ưu tiên chia nhỏ UI thành các hạt nhân độc lập (Atoms, Molecules, Organisms). Tránh xây dựng các file `.vue` quá khổng lồ (khuyến nghị < 250 dòng code/file).
- **Composition API First:** Bắt buộc sử dụng `<script setup>` cho các component. Khuyến khích gom nhóm logic theo tính năng (Feature-based) thông qua các Custom Composables (`useSomething.ts`).

## 2. QUẢN LÝ TRẠNG THÁI (STATE MANAGEMENT)
- **Local State (`ref`, `reactive`):** Dùng cho trạng thái nội bộ của một Component đơn lẻ (như form input, toggle modal...).
- **Global State (Pinia):** Chỉ dùng lưu trữ các dữ liệu thiết yếu cần chia sẻ chéo giữa nhiều luồng tính năng khác nhau (User Profile, Auth Token, System Config). Không lôi UI state vụn vặt vào store.
- **Tận dụng Composables:** Ưu tiên tách logic chia sẻ vào Composables để dễ test và độc lập thay vì nhồi nhét quá đà vào Global Store.

## 3. UI/UX & THẨM MỸ (AESTHETICS)
- **Nhất quán Design System:** Tuân thủ chặt chẽ Grid, Theme Color, Typography của project (VD: Ant Design / Tailwind). Hạn chế tối đa việc lạm dụng css inline hoặc override bằng `!important`.
- **Pixel Perfect & Responsive Layout:** Thiết kế không vỡ bố cục trên Mobile/Tablet/Desktop. Khuyến khích sử dụng CSS Flexbox / CSS Grid.
- **Tiểu tiết & Chuyển động (Micro-Animations):** 
  - Thêm hiệu ứng mịn màng cho Hover/Active/Disabled states.
  - Sử dụng `<Transition>` / `<TransitionGroup>` cho việc Ẩn/Hiện phần tử mượt mà.
  - Phải có trạng thái Loading (Skeleton / Spinner) khi call dữ liệu bất đồng bộ.

## 4. TỐI ƯU HIỆU NĂNG (PERFORMANCE)
- **Lazy Loading (Code Splitting):** Với các Page lớn hoặc component dùng cho modal ít khi xuất hiện, hãy import động qua `defineAsyncComponent()` hoặc `() => import(...)` ở Vue Router.
- **Render Optimization:**
  - Hạn chế sử dụng `watch` với cấu hình `{ deep: true }` nếu không thực sự cần. Ưu tiên `computed` sinh giá trị caching.
  - Đảm bảo `v-for` LUÔN LUÔN đi kèm thuộc tính `:key` độc nhất (tránh dùng `index` nếu array có khả năng biến đổi thứ tự hoặc xóa/thêm).
- **Dọn dẹp rác (Cleanup Resource):** Xóa bỏ các Timer (`setInterval`, `setTimeout`) hoặc `window.addEventListener` bên trong hook `onUnmounted` để tránh Memory Leak.

## 5. BẢO MẬT & PHÒNG NGỪA RỦI RO (SECURITY)
- **XSS Prevention:** Cực kỳ thận trọng trước thuộc tính `v-html`. Không bao giờ sử dụng `v-html` cho UGC - User Generated Content (nội dung do người dùng tự nhập) khi chưa sanitize.
- **Lỗi ở Client (Error Boundaries):** Các lỗi do render UI hỏng nên được kiểm soát, không để "trắng trang" mà phải có Fallback UI báo lỗi thân thiện.
