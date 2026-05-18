---
aliases:
  - ECS Components
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Components` là nơi ECS lưu dữ liệu của entity, còn system chỉ đọc hoặc ghi dữ liệu này để tạo hành vi.

## What is it
- Theo tài liệu `Entities 1.4.2`, component chứa data mà system có thể đọc hoặc ghi.
- Dạng phổ biến nhất là `IComponentData`, thường là `struct` chứa unmanaged data.
- ECS còn có nhiều loại component khác như managed, shared, tag, buffer, chunk, enableable, và singleton.

## How it works
- Mỗi entity được xác định bởi tập component type mà nó sở hữu.
- Tập component type này quyết định archetype của entity, và dữ liệu component được lưu trong chunk theo archetype đó.
- Một số loại component như `Enableable` cho phép bật/tắt trạng thái mà không cần structural change đắt đỏ.

## Why use it
- Giúp dữ liệu tách khỏi logic và dễ tổ chức theo cache-friendly layout.
- Cho phép bạn mô tả entity bằng những mảnh data nhỏ, rõ nghĩa, dễ query.
- Linh hoạt hơn khi thêm capability mới cho entity bằng component thay vì inheritance chain.

## When to use it
- Dùng khi bạn muốn biểu diễn state game bằng data nhỏ, rõ ràng, và có thể query theo kiểu component composition.
- Dùng khi muốn hệ thống xử lý hàng loạt entity có chung data shape.

## When to not use it
- Không nên nhét behavior phức tạp hoặc reference managed nặng vào component chỉ vì muốn giống class OOP.
- Không phù hợp nếu bạn vẫn đang thiết kế logic theo object graph phụ thuộc lẫn nhau kiểu `MonoBehaviour`.

## Limitations
- Quá nhiều component nhỏ nhưng đặt tên mơ hồ sẽ làm model khó hiểu.
- Managed component giảm bớt lợi thế hiệu năng so với unmanaged component.
- Đổi component type thường xuyên có thể gây structural change.

---

## Example code
```csharp
using Unity.Entities;

public struct Speed : IComponentData
{
    public float Value;
}

public struct EnemyTag : IComponentData
{
}
```

---

## Related notes
- [[Definition]]
- [[Entity]]
- [[Systems]]
- [[Archetypes and Chunks]]
- [[Summary]]
