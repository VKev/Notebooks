---
aliases:
  - ECS Entity
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Entity` là định danh nhẹ trong ECS, dùng để gom các component liên quan lại với nhau mà không tự chứa logic.

## Key points
- Theo tài liệu `Entities 1.4.2`, một `Entity` đại diện cho một thứ rời rạc trong chương trình, như character, effect, UI element, hoặc cả một khái niệm trừu tượng.
- giống một `GameObject` nhẹ theo nghĩa nhận diện, nhưng bản thân `Entity` không chứa code và không trực tiếp chứa data field như OOP object.
- `Entity` hoạt động như một ID, còn dữ liệu thật nằm trong các component gắn với nó.
- `EntityManager` của world tạo, hủy, nhân bản, thêm component, xóa component, và đọc/ghi component cho entity.
- Khi bạn tạo hoặc hủy entity, hoặc thêm/xóa component type, Unity phải thực hiện structural change.

## Decision rules
- Tách identity khỏi data và behavior giúp hệ thống xử lý theo batch tốt hơn.
- hợp cho số lượng object lớn, nơi cách tổ chức theo `GameObject` và `MonoBehaviour` bắt đầu tốn CPU và cache.
- Khi bạn muốn mô hình hóa số lượng lớn object có cấu trúc dữ liệu rõ ràng.
- Khi object chủ yếu được mô tả bởi data và được nhiều system xử lý tuần tự hoặc song song.
- Không cần dùng nếu bài toán nhỏ, ít object, hoặc logic phụ thuộc nặng vào workflow `MonoBehaviour`.
- Không phù hợp nếu bạn kỳ vọng mỗi entity là object giàu behavior như trong OOP.
- `Entity` chỉ là ID, nên khó hiểu hơn `GameObject` nếu bạn quen tư duy object-oriented.
- Structural change khi tạo/hủy hoặc đổi component type có chi phí và cần tránh trong hot path.

## Example
```csharp
using Unity.Entities;
public partial struct CreateEntityExampleSystem : ISystem
{
    public void OnCreate(ref SystemState state)
    {
        Entity e = state.EntityManager.CreateEntity();
        state.EntityManager.AddComponentData(e, new Speed { Value = 5f });
    }
}
public struct Speed : IComponentData
{
    public float Value;
}
```

## Related notes
- [[Definition]]
- [[Components]]
- [[World]]
- [[Archetypes and Chunks]]
- [[Summary]]
