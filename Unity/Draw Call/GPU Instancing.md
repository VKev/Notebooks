---
aliases:
  - Instancing
  - GPU Instance Rendering
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `GPU Instancing` dùng một draw call để render nhiều instance của cùng `mesh` và `material`.

## Problem
- Nhiều object lặp lại như cây, bụi, hoặc prop nhỏ thường tạo ra nhiều draw call gần giống nhau.
- Nếu CPU cứ phải submit từng object riêng lẻ, render overhead tăng nhanh.

## Core idea
- GPU có sẵn cơ chế instancing.
- Unity tận dụng cơ chế đó để render nhiều bản sao của cùng mesh/material trong một instanced draw path, trong khi từng instance vẫn có thể có property khác nhau như màu hoặc scale.

## Mechanism
- Điều kiện cốt lõi là nhiều object dùng cùng `mesh` và cùng `material`.
- Mỗi bản sao là một `instance`.
- Unity docs lưu ý rằng lợi ích performance phụ thuộc platform và GPU:
  - trên mobile, lợi ích thường tốt hơn
  - trên desktop, overhead gom và upload instance data đôi khi có thể lớn hơn lợi ích
- Về pipeline:
  - `GPU Instancing` tương thích với mọi render pipeline
  - trong `URP/HDRP`, custom shader chỉ đi theo đường này nếu bạn tắt `SRP Batcher` hoặc làm shader không tương thích với `SRP Batcher`
  - trong `Built-In Render Pipeline`, đây là một công cụ quan trọng hơn
- Với prebuilt materials:
  - `Mesh Renderer` được hỗ trợ
  - `Skinned Mesh Renderer` không được hỗ trợ

## When to use
- Dùng khi scene có nhiều bản sao của cùng object, nhất là foliage, prop lặp lại, hoặc crowd kiểu đơn giản.
- Hợp nhất trong `Built-In Render Pipeline`, hoặc trong các case cụ thể bạn kiểm soát rõ shader path.

## Benefits
- Có thể giảm draw-call overhead cho object lặp lại.
- Cho phép giữ variation per instance mà không phải nhân toàn bộ material.

## Trade-offs / Limits
- Không phải lúc nào cũng nhanh hơn; Unity phải gom và upload instance data.
- Trong `URP/HDRP`, docs Unity 6 thường ưu tiên `SRP Batcher` và `GPU Resident Drawer` trước khi bạn tự bật instancing thủ công.
- Dùng instancing có thể kéo theo thêm shader variants.

## Common mistakes
- Nghĩ rằng cứ bật `Enable GPU Instancing` là chắc chắn nhanh hơn.
- Dùng nhiều material khác nhau rồi vẫn mong instancing hoạt động tốt.
- Ở `URP/HDRP`, bật instancing như default path mà không xem nó có mâu thuẫn với hướng tối ưu của SRP hay không.

## Example
- Một scene có hàng trăm cây dùng chung cùng `mesh` và `material` là case điển hình cho instancing.

```csharp
// Same mesh + same material + many copies
// -> good candidate for GPU instancing
```

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Optimization]]
- [[Batching]]
- [[SRP Batcher]]
- [[Comparison]]

