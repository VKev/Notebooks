---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.4`: UI Toolkit hỗ trợ cả Editor UI và runtime UI. Đang dần thay thế UGUI cho project mới.

## Core keywords
- `VisualElement`: Building block cơ bản, tương tự `div` trong HTML. Mọi control đều kế thừa từ nó.
- `UXML`: Markup format khai báo cấu trúc UI, giống HTML/XML. Tách structure khỏi style và logic.
- `USS`: Style sheet format giống CSS, hỗ trợ selector, class, pseudo-class. Cho phép restyle toàn bộ UI bằng cách đổi file USS.
- `UIDocument`: Component gắn lên `GameObject` để hiển thị UI tại runtime. Kết nối UXML template với scene qua `PanelSettings`.
- `UI Builder`: Visual editor kéo thả để tạo UXML và USS. Artist có thể thiết kế UI mà không viết code.

## Layout and events
- `Flexbox`: Hệ thống layout giống CSS flexbox. Hỗ trợ `flex-direction`, `justify-content`, `align-items`, `flex-grow`.
- `Query system`: `Q<T>(name)` tìm một element, `Query<T>(className)` tìm nhiều. Thay thế việc serialize reference UI element.
- `Event system`: `RegisterCallback<TEvent>()` để lắng nghe event. Hỗ trợ `ClickEvent`, `ChangeEvent<T>`, `MouseEnterEvent`, v.v.

## Comparison with UGUI
- `Canvas vs UIDocument`: UGUI dùng `Canvas` với `RectTransform`, UI Toolkit dùng `UIDocument` với `VisualElement`. UI Toolkit layout bằng flexbox, UGUI layout bằng anchor/pivot.
- `Separation of concerns`: UGUI gộp layout, style, logic trên `GameObject`. UI Toolkit tách ra UXML, USS, và C# riêng biệt.
- `World-space`: UGUI hỗ trợ world-space Canvas tốt hơn. UI Toolkit runtime world-space còn hạn chế.

## Decision rules
- `Project mới cần runtime UI`: Cân nhắc UI Toolkit nếu không cần world-space UI phức tạp. Mô hình tách biệt UXML/USS/C# dễ maintain hơn.
- `Cần world-space UI phức tạp`: Dùng UGUI Canvas world space. UI Toolkit chưa hỗ trợ đầy đủ.
- `Cần Editor tool UI`: Dùng UI Toolkit, hướng chính thức cho Editor UI. `EditorWindow` hỗ trợ UI Toolkit native.
- `Artist cần thiết kế UI`: Dùng UI Builder. Tách biệt design và code, tránh file conflict.

## Common traps
- `Nghĩ UI Toolkit hoàn toàn thay thế UGUI ngay`: Chưa đúng. World-space UI và feature vẫn tốt hơn ở UGUI. Kiểm tra feature support trước khi quyết định.
- `Viết tất cả UI bằng C# code`: Mất lợi thế tách biệt concern. Dùng UXML cho structure, USS cho style, C# cho logic.
- `Quên gán PanelSettings cho UIDocument`: UI không render nếu thiếu `PanelSettings`. Mỗi `UIDocument` cần reference tới `PanelSettings` asset.
- `Dùng USS property giống hệt CSS`: USS chỉ hỗ trợ subset của CSS, property khác tên. Kiểm tra tài liệu USS property reference.

## Review questions

### UI Toolkit khác UGUI ở điểm nào?
- UI Toolkit tách structure (UXML), style (USS), logic (C#) giống web development, trong khi UGUI gộp tất cả trên `GameObject` với `RectTransform`.

### VisualElement là gì?
- Là building block cơ bản của mọi UI element trong UI Toolkit, tương tự `div` trong HTML, mọi control đều kế thừa từ nó.

### UXML và USS có vai trò gì?
- UXML định nghĩa cấu trúc UI bằng markup, USS định nghĩa style bằng cú pháp giống CSS, tách biệt structure và presentation.

### Khi nào nên dùng UGUI thay vì UI Toolkit?
- Khi cần world-space UI phức tạp hoặc dùng feature mà UI Toolkit chưa hỗ trợ đầy đủ.

### UIDocument hoạt động thế nào?
- `UIDocument` là component gắn lên `GameObject`, load UXML template và hiển thị UI qua `PanelSettings`. Code truy cập UI qua `rootVisualElement`.

## Related notes
- [[UI Toolkit]]
- [[Definition]]
- [[VisualElement]]
- [[UXML and USS]]
- [[UIDocument]]
- [[../Unity|Unity]]
