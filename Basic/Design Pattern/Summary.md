---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `Creational`
  Tập trung vào cách tạo object.
  Ví dụ thường gặp ở section này là `Singleton` và `Factory Method`.
- `Structural`
  Tập trung vào cách ghép object hoặc module.
  Ví dụ thường gặp là `Adapter` và `Decorator`.
- `Behavioral`
  Tập trung vào cách object giao tiếp hoặc đổi hành vi.
  Ví dụ thường gặp là `Strategy`, `Observer`, `Command`, và `State`.

## Popular patterns
- `Singleton`
  Một instance dùng chung cho toàn app hoặc toàn process.
  Nên dùng rất tiết chế vì dễ tạo global state.
- `Factory Method`
  Gom logic tạo object vào một chỗ.
  Hữu ích khi cùng một contract có nhiều implementation.
- `Strategy`
  Đóng gói các thuật toán hoặc rule khác nhau sau cùng một interface.
  Rất hợp khi muốn thay behavior linh hoạt.
- `Observer`
  Phát thông báo cho nhiều listener khi có thay đổi.
  Hợp cho event-driven flow.
- `Decorator`
  Thêm behavior bằng wrapper thay vì sửa class gốc.
  Hợp cho logging, caching, retry, validation.
- `Adapter`
  Chuyển API cũ hoặc API ngoài thành contract nội bộ mong muốn.
  Hợp cho tích hợp external service hoặc legacy code.
- `Command`
  Biểu diễn thao tác dưới dạng object.
  Hợp cho queue, undo/redo, hoặc tách invoker khỏi receiver.
- `State`
  Tách behavior theo từng trạng thái ra thành object riêng.
  Hợp khi một object đổi cách xử lý theo lifecycle hoặc workflow.

## Decision rules
- `Need swap algorithm easily`
  Nghĩ đến `Strategy`.
  Nó hợp khi nhiều rule cùng contract nhưng khác implementation.
- `Need notify many listeners`
  Nghĩ đến `Observer`.
  Nó hợp cho event-driven flow.
- `Need extend behavior without editing core`
  Nghĩ đến `Decorator` hoặc `State`.
  Chọn theo việc bạn đang thêm feature hay mô hình hóa trạng thái.

## Common traps
- `Pattern nào cũng nên dùng càng sớm càng tốt`
  Sai.
  Pattern chỉ đáng dùng khi nó giải quyết một áp lực thiết kế thật sự.
- `Singleton là mặc định tốt`
  Sai.
  Nó có thể tạo global state và làm test khó hơn.

## Interview questions and answers
### Design pattern là gì?
- Là mẫu giải pháp thiết kế cho các vấn đề lặp lại nhiều lần trong phần mềm.

### Khác nhau ngắn gọn giữa Strategy và State là gì?
- `Strategy` thay đổi thuật toán theo lựa chọn, còn `State` thay đổi hành vi theo trạng thái nội bộ của object.

### Vì sao không nên lạm dụng Singleton?
- Vì nó dễ tạo phụ thuộc ẩn, global state, và làm việc test hoặc thay thế implementation khó hơn.

## Related notes
- [[Design Pattern]]
- [[Definition]]
- [[Singleton]]
- [[Factory Method]]
- [[Strategy]]
- [[Observer]]
- [[Decorator]]
- [[Adapter]]
- [[Command]]
- [[State]]
- [[../Basic|Basic]]
