---
name: Frontend Design (Vue.js)
description: Tiêu chuẩn thiết kế giao diện và kiến trúc component Vue
---

# Frontend Design Guidelines

1. **Component Architecture:**
   - Chia nhỏ UI thành các component con nhỏ hơn để dễ dàng tái sử dụng và kiểm thử.
   - Ưu tiên sử dụng Single File Components (*.vue).

2. **State Management:**
   - Vue 3 khuyến khích Composition API `script setup` giúp code gọn gàng, hiệu năng cao.
   - Với state dùng chung (Global state), có thể sử dụng Pinia.

3. **Design Aesthetics:**
   - Ứng dụng phải có giao diện hiện đại, bóng bẩy (vibrant colors, glassmorphism) mang yếu tố 'vibecoding'.
   - Hỗ trợ responsive layout hoàn chỉnh cho Mobile / Desktop.
   - Bổ sung các micro-animations tinh tế thông qua CSS transitions hoặc Vue `<Transition>`.

4. **Performance:**
   - Lazy load cho components / routes.
   - Thận trọng khi sử dụng watcher để tránh re-render không cần thiết.
   - Khai báo Key đầy đủ trong `v-for`.
