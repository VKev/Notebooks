---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `SRP`: Một class nên có một lý do chính để thay đổi. Nó giúp class nhỏ và rõ trách nhiệm hơn.
- `OCP`: Nên mở rộng bằng abstraction hoặc composition. Tránh sửa nhiều vào code đã ổn định.
- `LSP`: Subtype phải thay thế được base type mà không phá kỳ vọng. nền quan trọng để inheritance an toàn.
- `ISP`: Client chỉ nên phụ thuộc vào method mà nó thật sự cần. Interface nhỏ và focused dễ dùng hơn.
- `DIP`: Phụ thuộc vào abstraction thay vì implementation cụ thể. nền quan trọng cho `Dependency Injection`.

## Decision rules
- `Need easier testing`: Ưu tiên interface và dependency rõ ràng. Điều này kéo theo `DIP`.
- `Need safer extension`: Cân nhắc strategy, decorator, hoặc factory thay vì if-else lớn. chỗ `OCP` phát huy giá trị.

## Common traps
- `SOLID nghĩa là phải có interface cho mọi thứ`: Sai. Interface chỉ nên có khi thật sự tạo giá trị ở boundary hoặc variation point.
- `SRP nghĩa là class phải rất nhỏ`: Không hẳn. Quan trọng là một trách nhiệm chính, không phải số dòng.

## Review questions

### SOLID là gì?
- Là 5 nguyên tắc thiết kế hướng đối tượng giúp code dễ mở rộng và dễ bảo trì hơn.

### Nguyên tắc nào liên quan mạnh nhất đến DI?
- `Dependency Inversion Principle`, vì nó yêu cầu code cấp cao phụ thuộc abstraction thay vì implementation cụ thể.

### OCP áp dụng bằng cách nào?
- qua interface, composition, strategy, decorator, hoặc factory.

## Related notes
- [[SOLID]]
- [[Definition]]
- [[Single Responsibility Principle]]
- [[Open Closed Principle]]
- [[Liskov Substitution Principle]]
- [[Interface Segregation Principle]]
- [[Dependency Inversion Principle]]
- [[../Basic|Basic]]
