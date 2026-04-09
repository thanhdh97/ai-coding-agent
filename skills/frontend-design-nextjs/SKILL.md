---
name: Frontend Design (Next.js)
description: Tiêu chuẩn thiết kế giao diện và kiến thức Next.js App Router
---

# 🎨 Frontend Design & Architecture (Next.js)

Bộ tiêu chuẩn kiến trúc dành riêng cho dự án Next.js (App Router). Mục tiêu: Đảm bảo sự nhất quán về tư duy hệ thống, tối ưu hóa trải nghiệm người dùng và hiệu năng ứng dụng.

## 1. MÔ HÌNH COMPONENT (ARCHITECTURE)
- **Smart vs. Dumb Components:** Tách biệt rõ ràng giữa Presentational Component (Dumb - chỉ nhận props, không logic nặng) và Container Component (Smart - xử lý logic, gọi Server Actions/Services).
- **Atomic Design:** Chia nhỏ UI thành các hạt nhân độc lập (Atoms, Molecules, Organisms). Tránh các file `.tsx` quá lớn (> 200 dòng).
- **Server Components First:** Ưu tiên Server Components (RSC) cho các phần không cần tương tác để tối ưu bundle. Sử dụng `'use client'` tập trung ở các "lá" (leaf components).
- **Strong Typing:** Mọi Props, State và Ref PHẢI được định nghĩa kiểu (Types/Interfaces) rõ ràng.

## 2. QUẢN LÝ TRẠNG THÁI (STATE MANAGEMENT)
- **Local State (`useState`, `useReducer`):** Dùng cho trạng thái nội bộ của một Component đơn lẻ.
- **Global State (Jotai/Zustand hoặc Context):** Lưu trữ dữ liệu thiết yếu cần chia sẻ chéo. Tránh nhồi nhét UI state vụn vặt.
- **Tận dụng Custom Hooks:** Tách logic phức tạp ra các file `useSomething.ts` để tái sử dụng và dễ test.
- **Server State:** Ưu tiên sử dụng cơ chế Caching của Next.js và URL Params cho các trạng thái liên quan đến dữ liệu từ server.

## 3. UI/UX & THẨM MỸ (AESTHETICS)
- **Nhất quán Design System:** Tuân thủ hệ thống Grid, Theme, Typography thông qua Tailwind CSS config.
- **Pixel Perfect & Responsive:** Thiết kế mượt mà trên mọi thiết bị.
- **Tiểu tiết & Chuyển động (Micro-Animations):**
  - Sử dụng **Framer Motion** để tạo hiệu ứng Hover/Active/Transitions mượt mà.
  - Luôn có trạng thái Loading (Skeleton / Suspense) khi xử lý bất đồng bộ.

## 4. TỐI ƯU HIỆU NĂNG (PERFORMANCE)
- **Image & Font Optimization:** Bắt buộc sử dụng `next/image` và `next/font`.
- **Streaming & Suspense:** Chia nhỏ việc tải trang để tăng trải nghiệm người dùng (LCP/FID).
- **Dọn dẹp rác (Cleanup):** Xử lý triệt để Timer hoặc Event Listener trong `useEffect` cleanup function.

## 5. BẢO MẬT & PHÒNG NGỪA RỦI RO
- **XSS & Data Validation:** Sử dụng `zod` để validate input tại Server Actions. Thận trọng với `dangerouslySetInnerHTML`.
- **Error Boundaries:** Sử dụng `error.tsx` để bắt lỗi render, tránh làm sập toàn bộ ứng dụng.
