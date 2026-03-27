---
aliases:
  - Draw Call Batching
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `Batching` là cách Unity gom các `mesh` dùng cùng `material` để giảm số lần cập nhật render state và giảm CPU overhead.

## Problem
- Scene có nhiều mesh nhỏ dùng cùng material vẫn có thể làm CPU tốn công setup lặp đi lặp lại.
- Nếu không gom lại hợp lý, render overhead tăng nhanh dù từng object riêng lẻ rất nhỏ.

## Core idea
- Nếu nhiều object dùng cùng render state, Unity có thể xử lý chúng theo batch thay vì setup từng object rời rạc.
- Trong Unity, batching quan trọng nhất là:
  - `static batching`
  - `dynamic batching`
  - manual mesh combine như một lựa chọn thay thế trong vài trường hợp

## Mechanism
- `Static batching`:
  - kết hợp nhiều static mesh vào một vertex buffer và một index buffer trong world space
  - mỗi buffer chứa tối đa `64,000` vertices, nhưng Unity có thể tạo nhiều batch
  - hợp cho `Mesh Renderer`, không áp dụng cho `Skinned Mesh Renderer`
- `Dynamic batching`:
  - transform vertex sang world space trên CPU rồi gom nhiều mesh thành một draw call
  - phù hợp hơn với lower-end device
  - không còn là recommendation mặc định cho đa số project hiện đại vì CPU overhead có thể lớn hơn lợi ích
- `Manual combine`:
  - bạn chủ động gộp nhiều mesh thành một mesh
  - Unity render chúng trong một draw call
  - đổi lại Unity không thể cull từng mesh con riêng lẻ
- Giới hạn quan trọng:
  - transparent object có thể batch kém hơn vì Unity phải sort back-to-front
  - dynamic batching không hỗ trợ tốt cho mesh lớn; giới hạn chính là `300` vertices hoặc `900` vertex attributes
  - dynamic batching chỉ hỗ trợ render pass đầu tiên của multi-pass shader

## When to use
- Dùng `static batching` cho world geometry tĩnh.
- Dùng manual combine khi các mesh ở gần nhau, không di chuyển tương đối với nhau, và không cần individual culling.
- Chỉ cân nhắc `dynamic batching` khi target là thiết bị thấp và profiling cho thấy nó thực sự có ích.

## Benefits
- Giảm CPU render overhead.
- Có thể cải thiện frame time khi scene có nhiều mesh nhỏ lặp lại cùng render state.

## Trade-offs / Limits
- `Static batching` dùng thêm CPU memory để lưu combined meshes.
- Với runtime static batching, bạn chỉ đổi được transform của cả batch, không đổi từng mesh bên trong.
- Manual combine làm culling kém chi tiết hơn.
- `Dynamic batching` có thể làm chậm hơn trên phần cứng hiện đại.

## Common mistakes
- Đánh dấu quá nhiều object là static mà không đo memory.
- Mong `Skinned Mesh Renderer` được hưởng batching như mesh thường.
- Dùng manual combine cho object trải rộng khắp scene rồi làm mất lợi ích culling.
- Bật `dynamic batching` như giải pháp chung cho mọi platform.

## Example
- Một cái tủ có nhiều ngăn kéo nhỏ, luôn đứng yên và ở gần nhau, là ví dụ tốt cho manual combine hoặc static batching.
- Một khu rừng dày đặc có thể batch được, nhưng phải kiểm tra memory cost trước khi đánh dấu toàn bộ là static.

```csharp
// Static geometry -> static batching
// Many close, never-separated parts -> manual combine can work
// Low-end specific case -> dynamic batching only after profiling
```

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Optimization]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Comparison]]

