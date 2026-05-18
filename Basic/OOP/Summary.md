---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `Object`
  Là đơn vị chứa dữ liệu và hành vi.
  Nó đại diện cho một thực thể trong bài toán.
- `Class`
  Là bản thiết kế để tạo object.
  Nó định nghĩa field, property, và method.

## Four core principles
- `Encapsulation`
  Giấu chi tiết nội bộ và kiểm soát truy cập.
  Giúp giữ state ổn định hơn.
- `Abstraction`
  Giữ lại phần quan trọng của mô hình.
  Giúp nơi sử dụng bớt phụ thuộc chi tiết cài đặt.
- `Inheritance`
  Cho phép class con nhận lại hoặc mở rộng behavior từ class cha.
  Chỉ nên dùng khi quan hệ cha con thật sự hợp lý.
- `Polymorphism`
  Cùng một contract nhưng nhiều implementation.
  Giúp thay thế object linh hoạt hơn.

## Decision rules
- `Need clear domain model`
  Ưu tiên nghĩ theo object và responsibility.
  Mỗi class nên có vai trò dễ gọi tên.
- `Need reuse behavior`
  Xem composition trước rồi mới nghĩ đến inheritance.
  Composition thường mềm dẻo hơn khi requirements thay đổi.

## Common traps
- `Một class làm quá nhiều việc`
  Đây là dấu hiệu responsibility chưa tách rõ.
  Sau này rất khó test và khó sửa.
- `Inheritance là cách reuse mặc định`
  Không đúng.
  Nhiều trường hợp composition an toàn hơn.

## Interview questions and answers
### OOP là gì?
- Là cách tổ chức chương trình quanh object, nơi dữ liệu và hành vi được gom lại trong cùng một đơn vị.

### 4 nguyên tắc cốt lõi của OOP là gì?
- `Encapsulation`, `Abstraction`, `Inheritance`, và `Polymorphism`.

### Khi nào nên tránh inheritance?
- Khi class con chỉ reuse một phần nhỏ hành vi hoặc khi quan hệ cha con không thực sự bền vững.

## Related notes
- [[OOP]]
- [[Definition]]
- [[Encapsulation]]
- [[Abstraction]]
- [[Inheritance]]
- [[Polymorphism]]
- [[../Basic|Basic]]
