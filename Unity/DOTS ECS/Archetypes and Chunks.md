---
aliases:
  - Archetypes
  - Chunks
  - ECS Chunks
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Archetype` là tổ hợp component duy nhất, còn `chunk` là block memory chứa các entity cùng archetype; đây là lõi data layout giúp ECS xử lý batch hiệu quả.

## Problem
- Nếu không hiểu archetype và chunk, bạn sẽ không hiểu vì sao ECS nhanh ở scale lớn hoặc vì sao structural changes lại tốn chi phí.

## Core idea
- ECS group entity theo shape của data, không theo class type.
- Cùng component set thì cùng archetype, và cùng archetype thì có thể được lưu sát nhau trong chunk.

## Mechanism
- Archetype được xác định bởi unique combination của component types.
- Chunk là memory block chứa entity của cùng archetype.
- Điều này giúp system query lấy data liên tiếp hơn trong memory và xử lý hàng loạt dễ hơn.
- Nếu bạn add/remove component, entity có thể phải chuyển archetype; đây là structural change và có thể tốn chi phí.

## When to use
- Dùng concept này để reason về performance, query shape, và chi phí structural changes.
- Quan trọng khi optimize ECS code và khi thiết kế component layout.

## Benefits
- Cache locality tốt hơn.
- Batch iteration hiệu quả hơn.
- Là nền cho chunk-based processing và many-entity scaling.

## Trade-offs / Limits
- Structural changes có thể đắt vì entity phải chuyển archetype/chunk.
- Component layout thiết kế kém có thể làm archetype nổ số lượng và khó maintain.

## Common mistakes
- Add/remove component liên tục trong hot path mà không nghĩ tới structural cost.
- Thiết kế component tùy tiện làm archetype fragmentation tăng.
- Nghĩ rằng ECS nhanh chỉ vì "code hay hơn" thay vì memory layout tốt hơn.

## Example
- Cùng `Health + Speed + LocalTransform` thì cùng archetype; thêm `Target` vào một entity có thể đẩy nó sang archetype khác.

```csharp
Enemy = Health + Speed + LocalTransform
EnemyWithTarget = Health + Speed + LocalTransform + Target
```

## Related notes
- [[DOTS ECS]]
- [[Definition]]
- [[Entity]]
- [[Components]]
- [[Systems]]
