---
aliases:
  - Archetypes
  - Chunks
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Archetype` gom các entity có cùng tập component type, còn `Chunk` là block memory đồng nhất nơi Unity lưu data của archetype đó.

## What is it
- Theo `Entities 1.4.2`, archetype là định danh cho tất cả entity trong một world có cùng tổ hợp component type.
- Tất cả entity và component có cùng archetype được lưu trong các block memory đồng nhất gọi là chunks.
- Mỗi chunk có kích thước `16 KiB`.

## How it works
- Khi thêm hoặc xóa component type trên một entity, `EntityManager` phải chuyển entity đó sang archetype phù hợp.
- Dữ liệu trong chunk được lưu theo mảng cho từng component type, cộng thêm mảng entity IDs.
- Các phần tử trong chunk được đóng gói chặt, nên iteration theo archetype rất cache-friendly.
- Khi entity rời chunk, Unity có thể dời entity cuối chunk lên lấp chỗ trống.

## Why use it
- Đây là lý do cốt lõi khiến ECS query nhanh hơn việc quét từng object rời rạc.
- Data layout theo chunk giúp CPU cache và batch processing hoạt động tốt hơn.

## When to use it
- Dùng như mental model bắt buộc khi bạn muốn hiểu vì sao ECS nhanh.
- Dùng khi tối ưu query, chunk utilization, và giảm structural changes.

## When to not use it
- Không nên nghĩ archetype là “class type” theo nghĩa OOP.
- Không nên đổi component type quá thường xuyên nếu không cần, vì việc chuyển archetype tốn chi phí.

## Limitations
- Structural change làm entity chuyển archetype, đây là thao tác đắt hơn thay đổi giá trị component.
- Nếu data shape phân mảnh quá nhiều, chunk utilization và hiệu quả query có thể giảm.

---

## Example code
```csharp
using Unity.Entities;

public partial struct ArchetypeExampleSystem : ISystem
{
    public void OnCreate(ref SystemState state)
    {
        var archetype = state.EntityManager.CreateArchetype(
            typeof(Speed),
            typeof(Health));

        state.EntityManager.CreateEntity(archetype);
    }
}

public struct Speed : IComponentData
{
    public float Value;
}

public struct Health : IComponentData
{
    public float Value;
}
```

---

## Related notes
- [[Definition]]
- [[Entity]]
- [[Components]]
- [[World]]
- [[Summary]]
