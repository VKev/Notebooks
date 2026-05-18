---
note_type: summary
tags:
  - basic
sticker: lucide//atom
---

## Core keywords
- `Stack`: Vùng nhớ LIFO, lưu local variable, parameter, return address. Mỗi thread có stack riêng, giải phóng tự động khi method kết thúc.
- `Heap`: Vùng nhớ lớn do GC quản lý, lưu tất cả reference type object. Tất cả thread dùng chung một managed heap.
- `Value type`: Kiểu như `int`, `float`, `bool`, `struct`, lưu trực tiếp trên stack khi là local variable. Nếu là field của class thì nằm trên heap cùng object chứa nó.
- `Reference type`: Kiểu như `class`, `interface`, `string`, luôn cấp phát trên heap. Biến reference trỏ tới object nằm trên stack.

## Boxing and unboxing
- `Boxing`: Chuyển value type thành `object` trên heap, implicit và tốn kém. Chậm hơn gấp khoảng 20 lần so với phép gán reference thông thường.
- `Unboxing`: Ép `object` về value type gốc, explicit và phải đúng type. Sai type sẽ throw `InvalidCastException`.
- `Generic collection`: Dùng `List<int>` thay `ArrayList` để tránh boxing. Đây là cách phổ biến để loại bỏ boxing không cần thiết.

## Garbage Collection
- `Generation 0`: Chứa object mới và ngắn hạn, thu gom thường xuyên nhất. Phần lớn object chết ở đây và không sống sót qua collection.
- `Generation 1`: Buffer giữa ngắn hạn và dài hạn. Object sống sót Gen 0 được promote lên đây.
- `Generation 2`: Chứa object tồn tại lâu, thu gom tốn kém nhất. Full collection quét cả Gen 0, Gen 1, và Gen 2.
- `Large Object Heap`: Chứa object lớn hơn `85,000 bytes`, thu gom cùng Gen 2. Không compact, dễ bị fragmentation.
- `Mark-Relocate-Compact`: Ba pha của GC: tìm object sống, cập nhật reference, nén lại. Root gồm static field, local variable, CPU register, GC handle.

## Decision rules
- `Cần data nhỏ, ngắn hạn, không polymorphism`: Ưu tiên `struct` để tận dụng stack allocation. Nhưng struct quá lớn sẽ tốn chi phí copy.
- `Cần truyền value type qua API nhận object`: Boxing là bắt buộc, nhưng giảm thiểu bằng generic overload hoặc generic method. Tránh boxing trong loop.
- `Cần quản lý unmanaged resource`: Implement `IDisposable`, dùng `using` statement. Gọi `GC.SuppressFinalize` trong `Dispose` để tránh overhead finalizer.
- `Cần giảm GC pause trong game hoặc real-time`: Giảm allocation trên heap, đặc biệt trong hot path và mỗi frame. Object pooling và `stackalloc` là hai kỹ thuật phổ biến.

## Common traps
- `Value type luôn ở stack`: Sai. Value type là field của class sẽ nằm trên heap cùng object chứa nó. Biến bị capture bởi lambda cũng được chuyển lên heap.
- `Boxing không xảy ra nếu không cast rõ ràng`: Sai. Boxing là implicit, xảy ra ngầm khi gán value type vào `object`, truyền vào `string.Concat`, hoặc dùng `ArrayList`. Dùng generic để tránh.
- `Gọi GC.Collect() giúp tối ưu performance`: Sai. kích hoạt full collection tốn kém và phá vỡ heuristic tối ưu của GC. đa số trường hợp nên để GC tự quyết định.
- `Finalizer là cách tốt để cleanup resource`: Không đúng. Finalizer delay thu hồi object sang lần collection tiếp theo. Ưu tiên `IDisposable` và `using` statement.
- `Struct luôn tốt hơn class vì ở stack`: Sai. Struct lớn copy tốn kém, struct có reference type field vẫn reference heap. Chỉ ưu tiên struct khi data nhỏ và ngắn hạn.

## Review questions

### Stack và Heap khác nhau thế nào?
- Stack là LIFO, lưu local variable, giải phóng tự động khi method kết thúc. Heap lưu reference type object, do GC quản lý, tất cả thread dùng chung.

### Boxing là gì và tại sao cần tránh?
- Boxing là chuyển value type thành `object` trên heap, tốn kém vì phải allocate object mới, tăng GC pressure, và chậm hơn gấp nhiều lần so với phép gán thông thường.

### GC hoạt động theo generation như thế nào?
- GC chia heap thành Gen 0, 1, 2. Object mới vào Gen 0, sống sót thì promote lên. Thu gom Gen 0 nhanh và thường xuyên, Gen 2 là full collection tốn kém nhất.

### Khi nào value type nằm trên heap?
- Khi là field của class, khi bị boxing thành `object`, hoặc khi bị capture bởi lambda hoặc closure.

### Vì sao không nên gọi GC.Collect() thủ công?
- Vì kích hoạt full collection quét toàn bộ heap, tốn CPU, và phá vỡ heuristic mà GC dùng để tối ưu thời điểm thu gom.

### IDisposable giải quyết vấn đề gì mà GC không giải quyết được?
- GC chỉ thu hồi managed memory. `IDisposable` cho phép giải phóng unmanaged resource như file handle, network connection ngay lập tức thay vì chờ GC.

## Related notes
- [[Memory Management]]
- [[Definition]]
- [[Stack and Heap]]
- [[Boxing and Unboxing]]
- [[Garbage Collection]]
- [[../Basic|Basic]]
