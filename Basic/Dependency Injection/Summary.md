---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `Dependency`: Object hoặc service mà class khác cần dùng, ví dụ logger, repository, gateway, hoặc strategy.
- `Injection`: Là hành động cung cấp dependency từ bên ngoài. Class không tự tạo dependency nữa.
- `Constructor Injection`: Kiểu phổ biến và dễ đọc nhất; làm dependency bắt buộc trở nên rõ.

## Decision rules
- `Need easier testing`: Tách dependency qua interface hoặc contract. Sau đó inject implementation vào class cần dùng.
- `Need replace implementation by environment`: Dùng `Dependency Injection`. Ví dụ đổi logger, payment gateway, hoặc storage provider.

## Common traps
- `DI nghĩa là phải dùng container`: Sai. Container chỉ là công cụ; bản chất vẫn là inject dependency từ ngoài vào.
- `Constructor nhiều tham số là bình thường`: Không hẳn. dấu hiệu class có quá nhiều trách nhiệm.

## Review questions

### Dependency Injection là gì?
- Là cách cung cấp dependency từ bên ngoài thay vì để class tự tạo dependency bên trong.

### Vì sao Constructor Injection được ưu tiên?
- Vì dependency bắt buộc được thể hiện rõ, object dễ ở trạng thái hợp lệ, và test dễ hơn.

### DI khác gì với DIP?
- `DI` là kỹ thuật cung cấp dependency, còn `DIP` là nguyên tắc thiết kế yêu cầu phụ thuộc vào abstraction.

## Related notes
- [[Dependency Injection]]
- [[Definition]]
- [[Constructor Injection]]
- [[../Basic|Basic]]
