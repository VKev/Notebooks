---
aliases:
  - Generic Type
  - Generic Constraints
  - Variance
note_type: feature
tags:
  - basic
---

## One-line
- `Generics` cho phép viết class, method, interface, hoặc delegate dùng type parameter để giữ type safety mà không cần cast thủ công.

## What is it
- Generic type dùng placeholder như `T` để code có thể làm việc với nhiều type khác nhau.
- Constraint như `where T : IComparable<T>` báo cho compiler biết type argument phải có capability nào.
- Variance mô tả việc generic interface hoặc delegate có thể chuyển đổi giữa type argument có quan hệ inheritance hay không.

## How it works
- Nếu không có constraint, compiler chỉ giả định `T` có member của `object`.
- Constraint mở khóa member hoặc constructor mà generic code được phép gọi.
- Generic type mặc định là invariant, nên `List<Derived>` không thay thế được `List<Base>`.
- Một số interface/delegate có variance: `IEnumerable<out T>` covariant, `Action<in T>` contravariant.

## Why use it
- Tăng type safety và giảm cast runtime.
- Tránh boxing khi dùng collection generic với value type, ví dụ `List<int>` thay vì non-generic collection.
- Reuse algorithm và data structure mà vẫn giữ API rõ ràng.

## When to use it
- Dùng khi logic giống nhau cho nhiều type nhưng vẫn cần compile-time type checking.
- Dùng constraint khi generic code thật sự cần gọi member cụ thể của `T`.
- Dùng interface generic khi muốn contract reusable như `IRepository<T>`, `IComparer<T>`, `IObjectPool<T>`.

## When to not use it
- Không generic hóa code nếu chỉ có một type và không có áp lực tái sử dụng thật.
- Không thêm constraint rộng hoặc phức tạp chỉ để né thiết kế interface rõ ràng.

## Limitations
- Variance chỉ áp dụng cho generic interface và generic delegate, không áp dụng cho class như `List<T>`.
- Variance chỉ áp dụng cho reference type; value type vẫn invariant trong constructed variant type.
- Generic overuse có thể làm API khó đọc hơn lợi ích nó mang lại.

---

## Example code
```csharp
using System.Collections.Generic;

public interface IEntity
{
    int Id { get; }
}

public class Repository<T> where T : IEntity
{
    private readonly Dictionary<int, T> items = new();

    public void Add(T item)
    {
        items[item.Id] = item;
    }

    public bool TryGet(int id, out T item)
    {
        return items.TryGetValue(id, out item);
    }
}
```

---

## Related notes
- [[Definition]]
- [[../Memory Management/Boxing and Unboxing|Boxing and Unboxing]]
- [[../OOP/Polymorphism|Polymorphism]]
- [[Summary]]
