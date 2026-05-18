---
aliases:
  - Boxing Unboxing
note_type: feature
tags:
  - basic
---

## One-line
- `Boxing` là chuyển value type lên heap thành `object`, còn `Unboxing` là ép ngược từ `object` về value type.

## What is it
- Theo tài liệu Microsoft, `Boxing` là quá trình chuyển đổi implicit từ value type sang `object` hoặc interface type mà value type đó implement, CLR sẽ bọc giá trị trong một `System.Object` instance và lưu trên managed heap.
- `Unboxing` là quá trình chuyển đổi explicit từ `object` về value type, gồm hai bước: kiểm tra type có đúng không, rồi copy giá trị ra khỏi object về biến value type.
- Boxing và unboxing là nền tảng của unified type system trong C#, nơi mọi giá trị đều có thể được xem như `object`.

## How it works
- Khi boxing xảy ra, CLR cấp phát object mới trên heap, copy giá trị từ stack vào object đó, và trả về reference.
- Giá trị gốc trên stack và bản copy trên heap là độc lập, thay đổi bên này không ảnh hưởng bên kia.
- Khi unboxing, CLR kiểm tra type tại runtime, nếu sai sẽ throw `InvalidCastException`, nếu null sẽ throw `NullReferenceException`.
- Boxing là implicit, xảy ra tự động khi gán value type vào biến kiểu `object` hoặc interface.
- Unboxing là explicit, bắt buộc phải cast rõ ràng về đúng type gốc.
- Boxing thường xảy ra ngầm khi dùng `ArrayList`, `string.Format` với value type argument, `String.Concat` với value type, hoặc gán value type vào biến kiểu interface.

## Why use it
- Hiểu boxing giúp tránh allocation không cần thiết trên heap và giảm GC pressure.
- Boxing có thể chậm hơn gấp `20 lần` so với một phép gán reference thông thường.
- Mỗi boxed value tạo thêm overhead bộ nhớ do pointer và sync block index, ví dụ boxed `int` chiếm `12-16 bytes` thay vì `4 bytes`.

## When to use it
- Boxing xảy ra tự nhiên trong unified type system khi cần truyền value type qua API nhận `object`, đây là điều khó tránh hoàn toàn.
- Trong code legacy dùng `ArrayList` hoặc API non-generic, boxing là bắt buộc.

## When to not use it
- Tránh boxing trong loop hoặc hot path, vì mỗi lần boxing tạo object mới trên heap.
- Dùng generic collection như `List<int>`, `Dictionary<int, string>` thay vì `ArrayList` hoặc `List<object>` để tránh boxing.
- Dùng `string interpolation` hoặc overload nhận type cụ thể thay vì `string.Format` với `object` parameter khi có thể.

## Limitations
- Boxing tạo object trên heap, tăng GC pressure và có thể gây GC pause khi số lượng boxed value lớn.
- Unboxing phải cast đúng type gốc, cast sang type khác dù compatible cũng throw `InvalidCastException`, ví dụ boxed `int` không thể unbox thành `short`.
- Không có cách ngăn boxing hoàn toàn ở mức ngôn ngữ khi API yêu cầu `object`, chỉ có thể giảm thiểu.

---

## Example code
```csharp
public class BoxingExample
{
    public void Demo()
    {
        // Boxing: implicit, int được bọc thành object trên heap
        int value = 42;
        object boxed = value;

        // Unboxing: explicit, phải cast đúng type
        int unboxed = (int)boxed;

        // Sai: boxed int không thể unbox thành short
        // short wrong = (short)boxed; // InvalidCastException

        // Boxing ngầm trong string.Concat
        string result = string.Concat("Score: ", value);
        // value bị box thành object trước khi Concat

        // Tránh boxing bằng generic collection
        var scores = new System.Collections.Generic.List<int>();
        scores.Add(100); // không boxing, List<int> lưu trực tiếp

        // So sánh: ArrayList gây boxing
        var old = new System.Collections.ArrayList();
        old.Add(100); // boxing: int -> object
        int fromOld = (int)old[0]; // unboxing
    }
}
```

---

## Related notes
- [[Definition]]
- [[Stack and Heap]]
- [[Garbage Collection]]
- [[Summary]]
