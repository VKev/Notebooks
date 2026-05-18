---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `Delegate`
  Type-safe reference tới method có signature tương thích.
  Dùng để truyền behavior, callback, hoặc tạo multicast invocation list.
- `Event`
  Cơ chế publish/subscribe xây trên delegate.
  Bên ngoài class thường chỉ subscribe và unsubscribe, không tự raise event.
- `Interface`
  Contract cho method, property, event, hoặc indexer.
  Hợp với capability cắt ngang nhiều hierarchy.
- `Abstract class`
  Base class không instantiate trực tiếp.
  Hợp khi các class con cần share state, constructor, protected helper, hoặc implementation chung.
- `Generics`
  Dùng type parameter để giữ type safety và reuse code.
  Constraint bằng `where` cho compiler biết capability của type argument.
- `Variance`
  Quy tắc chuyển đổi generic interface/delegate theo inheritance của type argument.
  `out` là covariance, `in` là contravariance.
- `LINQ`
  Query data bằng syntax tích hợp trong C#.
  Nhiều operator dùng deferred execution, chỉ chạy khi enumerate.

## Decision rules
- `Cần callback một hàm`
  Dùng delegate, `Action`, hoặc `Func`.
  Nếu có nhiều listener và ý nghĩa là thông báo sự kiện, dùng event.
- `Cần contract cho nhiều type không cùng hierarchy`
  Dùng interface.
  Nó giúp dependency injection và testing tự nhiên hơn.
- `Cần reuse state hoặc behavior trong cùng một family`
  Dùng abstract class.
  Chỉ nên dùng khi quan hệ inheritance thật sự bền.
- `Cần collection hoặc service type-safe cho nhiều model`
  Dùng generics.
  Thêm constraint khi generic code cần gọi member cụ thể.
- `Cần query collection dễ đọc`
  Dùng LINQ ngoài hot path.
  Trong loop performance-critical, cân nhắc loop thường để kiểm soát allocation.

## Common traps
- `Delegate và event giống hệt nhau`
  Sai. Event dùng delegate nhưng giới hạn cách code ngoài tương tác với invocation list.
- `Subscribe lambda inline rồi unsubscribe bằng lambda giống hệt`
  Sai. Lambda mới là instance handler khác, thường không remove được handler cũ.
- `Interface luôn tốt hơn abstract class`
  Sai. Interface tốt cho capability; abstract class tốt khi cần share state hoặc implementation trong hierarchy.
- `List<Derived> dùng được như List<Base>`
  Sai. `List<T>` invariant. `IEnumerable<Derived>` có thể dùng như `IEnumerable<Base>` vì `IEnumerable<out T>` covariant.
- `LINQ luôn chạy ngay khi khai báo`
  Sai. Nhiều query deferred, chỉ chạy khi enumerate hoặc materialize.

## Interview questions and answers
### Delegate là gì?
- Delegate là type đại diện cho method có parameter list và return type tương thích, dùng để truyền method như dữ liệu một cách type-safe.

### Event khác delegate ở điểm nào?
- Event được xây trên delegate nhưng giới hạn code bên ngoài class, thường chỉ cho subscribe `+=` và unsubscribe `-=`, không cho tự gán lại hoặc raise event tùy tiện.

### Vì sao cần unsubscribe event?
- Vì subscriber còn nằm trong invocation list của publisher có thể bị giữ reference lâu hơn mong muốn hoặc nhận callback khi lifecycle đã kết thúc.

### Interface và abstract class khác nhau thế nào?
- Interface mô tả contract/capability và một class có thể implement nhiều interface. Abstract class là base class có thể chứa state, constructor, protected member, và implementation chung.

### Generic constraint để làm gì?
- Constraint báo cho compiler biết type argument phải thỏa điều kiện nào, nhờ đó generic code được phép gọi member hoặc constructor tương ứng.

### Covariance và contravariance là gì?
- Covariance cho phép dùng type derived ở vị trí type base trong output, ví dụ `IEnumerable<Derived>` thành `IEnumerable<Base>`. Contravariance cho phép dùng type base ở vị trí input, ví dụ `Action<Base>` thành `Action<Derived>`.

### Deferred execution trong LINQ là gì?
- Là việc query chưa chạy khi khai báo mà chỉ chạy khi enumerate, ví dụ qua `foreach`, `ToList()`, hoặc `Count()`.

### Vì sao cần cẩn thận với LINQ trong Unity `Update`?
- LINQ có thể tạo allocation, iterator, delegate, hoặc query chạy lại nhiều lần; trong hot path mỗi frame, loop thường dễ kiểm soát performance hơn.

## Related notes
- [[C Sharp]]
- [[Definition]]
- [[Delegate and Event]]
- [[Interface and Abstract Class]]
- [[Generics]]
- [[LINQ]]
- [[../OOP/OOP|OOP]]
- [[../SOLID/SOLID|SOLID]]
- [[../Memory Management/Memory Management|Memory Management]]
- [[../Basic|Basic]]
