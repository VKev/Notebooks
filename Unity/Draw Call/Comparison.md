---
aliases:
  - Comparison
  - Draw Call Comparison
  - Batching vs Instancing vs SRP Batcher
note_type: permanent
created: 2026-03-27
tags:
  - unity
color: var(--mk-color-base-40)
---

## One-line answer
- Trong Unity 6, cách chọn tối ưu draw call phụ thuộc mạnh vào render pipeline: `URP/HDRP` thường ưu tiên `SRP Batcher`, còn `Built-In Render Pipeline` dựa nhiều hơn vào `static batching` và `GPU Instancing`.

## Problem
- Nhiều người gom `batching`, `GPU instancing`, và `SRP Batcher` thành một nhóm rồi bật tất cả.
- Cách này dễ làm tăng complexity nhưng không đi đúng render path mà Unity khuyến nghị cho pipeline đang dùng.

## Core idea
- Các kỹ thuật này không giống nhau:
  - `SRP Batcher` giảm render-state changes
  - `GPU Instancing` render nhiều bản sao của cùng mesh/material
  - `Batching` gom mesh dùng cùng material
  - manual combine là tối ưu thủ công đổi draw call lấy culling chi tiết

## Mechanism
- Rule of thumb theo pipeline:
  - `URP/HDRP`:
    - bật `SRP Batcher`
    - nếu object tương thích, Unity 6 khuyến nghị `GPU Resident Drawer` cho GPU-driven instancing
    - manual `GPU Instancing` thường không phải first choice
  - `Built-In Render Pipeline`:
    - `static batching` là lựa chọn quan trọng cho object tĩnh
    - `GPU Instancing` hữu ích khi có nhiều object giống nhau
    - `dynamic batching` chỉ nên là lựa chọn hẹp cho lower-end device
- Rule of thumb theo shape dữ liệu:
  - nhiều material nhưng cùng shader variant -> `SRP Batcher`
  - nhiều copy của cùng mesh/material -> `GPU Instancing`
  - world geometry tĩnh -> `static batching`
  - nhiều phần nhỏ luôn đi cùng nhau và không cần cull riêng -> manual combine
- Unity cũng nêu priority order trên từng object:
  - `SRP Batcher`, và `static batching` nếu bật
  - `GPU instancing`, gồm `GPU Resident Drawer` hoặc `BRG`
  - `dynamic batching`

## When to use
- Dùng note này khi bạn phải ra quyết định chọn phương pháp, không chỉ học định nghĩa từng kỹ thuật.
- Hợp khi bạn đang profile scene và cần biết "nên thử cái gì trước".

## Benefits
- Giúp chọn đúng kỹ thuật theo pipeline thay vì theo cảm tính.
- Giảm khả năng xung đột giữa các đường tối ưu khác nhau.

## Trade-offs / Limits
- Không có một phương pháp tốt nhất tuyệt đối cho mọi project.
- Một object không tận dụng đồng thời mọi draw-call optimization path.
- Tối ưu đúng pipeline thường quan trọng hơn việc cố giảm số draw call bằng mọi giá.

## Common mistakes
- Bật `dynamic batching` dù project đang ở `URP/HDRP`.
- Dùng `GPU Instancing` thủ công trong `URP/HDRP` mà không hỏi vì sao không dùng `SRP Batcher` hoặc `GPU Resident Drawer`.
- Combine mesh quá mạnh rồi làm mất culling.
- Quên rằng ít `SetPass` đôi khi quan trọng hơn chỉ số `Batches` thuần túy.

## Example
- Decision rule ngắn:

```csharp
URP/HDRP -> SRP Batcher first
Many same mesh/material -> GPU Instancing candidate
Static world pieces -> Static batching candidate
Very low-end special case -> Dynamic batching maybe
```

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Optimization]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]

