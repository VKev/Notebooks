---
aliases:
  - Component Data
  - ECS Components
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Component` trong ECS là data unit gắn lên entity; nó nên diễn tả state chứ không chứa behavior theo kiểu `MonoBehaviour`.

## Problem
- Nếu vẫn nhét logic và reference phức tạp vào component, bạn sẽ phá lợi thế data-oriented của ECS.
- ECS cần data rõ ràng để system có thể query và batch-process hiệu quả.

## Core idea
- Component nói "entity này có state gì", không nói "entity này tự làm gì".
- Behavior nên nằm ở system, còn component giữ data tối thiểu nhưng đủ nghĩa.

## Mechanism
- Trong Entities workflow, các kiểu component phổ biến gồm:
  - `IComponentData`: data component cơ bản
  - tag component: marker zero-size
  - buffer component: mảng data kích thước thay đổi
  - shared component hoặc grouped data theo đặc điểm chung
- Component set của entity quyết định archetype mà entity thuộc về.

## When to use
- Dùng component để biểu diễn state như `Health`, `Speed`, `Target`, `Damage`, `Faction`.
- Hợp khi data có thể tách thành các đơn vị rõ ràng và reusable.

## Benefits
- Composition linh hoạt hơn inheritance chain dài.
- Dễ query theo data shape.
- Giúp system xử lý batch data đồng nhất.

## Trade-offs / Limits
- Chia component quá nhỏ hoặc quá rời rạc có thể làm model khó hiểu.
- Dùng managed data hoặc object reference không đúng chỗ có thể giảm lợi ích ECS.

## Common mistakes
- Đặt method behavior vào component như đang viết class OOP.
- Tạo component chỉ để "cho đủ khái niệm" mà không phục vụ query hay layout.
- Gộp data unrelated vào cùng một component lớn.

## Example
- Component data thuần:

```csharp
public struct Speed : IComponentData
{
    public float Value;
}
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Entity]]
- [[Systems]]
- [[Archetypes and Chunks]]
