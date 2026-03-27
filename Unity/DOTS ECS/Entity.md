---
aliases:
  - Entities
  - Thực thể ECS
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Entity` là identifier rất nhẹ trong ECS; nó không tự mang data hay behavior, mà chỉ là điểm gắn các component với nhau.

## Problem
- Trong OOP, object thường vừa là identity, vừa chứa state, vừa chứa logic.
- ECS cần tách riêng identity khỏi data và logic để data layout tối ưu hơn.

## Core idea
- Hãy xem entity như một `ID` thuần.
- Bản thân entity không làm gì cả; mọi meaning của entity đến từ component gắn trên nó.

## Mechanism
- Entity docs mô tả entity như lightweight unmanaged alternative to a `GameObject`.
- Một entity có thể có nhiều component.
- Nếu hai entity có cùng set component, ECS có thể group chúng cùng archetype và chunk.

## When to use
- Dùng như runtime identity cho mọi object bạn muốn ECS quản lý.
- Hợp khi identity chỉ là "đây là một instance của data set này", không cần object-oriented behavior riêng.

## Benefits
- Identity rất nhẹ.
- Phối hợp tốt với component composition.
- Giúp ECS quản lý và group data hiệu quả hơn.

## Trade-offs / Limits
- Entity không tự mang logic.
- Nếu tư duy còn theo kiểu "entity như GameObject", bạn sẽ dễ thiết kế sai component và system.

## Common mistakes
- Mong entity tự có behavior như `MonoBehaviour`.
- Nhét quá nhiều meaning vào entity thay vì để component nói lên state.
- Dùng entity như object wrapper cho managed object graph.

## Example
- Entity chỉ trở nên có meaning khi có component:

```csharp
Entity enemy;
// enemy + Health + Speed + LocalTransform => một enemy runtime
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Components]]
- [[Systems]]
- [[Archetypes and Chunks]]
