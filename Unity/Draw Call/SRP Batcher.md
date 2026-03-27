---
aliases:
  - Scriptable Render Pipeline Batcher
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- `SRP Batcher` là tối ưu draw-call dành cho SRP, giúp Unity giảm CPU time cần để chuẩn bị và dispatch draw call cho các material dùng cùng shader variant.

## Problem
- Scene dùng nhiều material có thể làm CPU tốn nhiều thời gian setup GPU state trước mỗi draw.
- Nếu chỉ cố giảm draw-call count theo cách truyền thống, bạn vẫn có thể bỏ lỡ cost lớn nhất là state setup.

## Core idea
- `SRP Batcher` không tập trung chính vào chuyện "vẽ ít lệnh hơn".
- Thay vào đó, nó giảm chi phí render-state changes giữa các draw call bằng cách batch chuỗi `bind` và `draw` command trong SRP.

## Mechanism
- `SRP Batcher` chỉ hỗ trợ:
  - `URP`
  - `HDRP`
  - custom `SRP`
- Nó không hỗ trợ `Built-In Render Pipeline`.
- Unity docs nhấn mạnh:
  - mỗi `SRP batch` nên chứa càng nhiều `bind` và `draw` commands càng tốt
  - để làm vậy, hãy dùng càng ít `shader variants` càng tốt
  - bạn vẫn có thể có nhiều material khác nhau miễn là chúng dùng cùng shader variant
- Cách nó nhanh hơn:
  - material data có thể persist trong GPU memory
  - Unity dùng code path chuyên dụng để update per-object engine properties trong buffer lớn
- Trong workflow thực tế:
  - hãy bật `SRP Batcher` trước nếu project dùng `URP/HDRP`
  - tránh các pattern làm đường SRP kém hiệu quả, đặc biệt là lạm dụng `MaterialPropertyBlock`

## When to use
- Đây là lựa chọn mặc định đầu tiên cho `URP/HDRP` khi bạn muốn giảm CPU render overhead.
- Hợp nhất khi scene có nhiều material nhưng vẫn xoay quanh ít shader variants.

## Benefits
- Có thể cải thiện mạnh CPU rendering time trong scene có nhiều material.
- Không bắt bạn gộp tất cả object thành cùng một material.
- Rất hợp với pipeline hiện đại của Unity hơn batching kiểu cũ.

## Trade-offs / Limits
- Không dùng được trong `Built-In Render Pipeline`.
- Nếu project có quá nhiều shader variants, batch size sẽ nhỏ đi và lợi ích giảm.
- Không nên đọc hiệu quả của nó chỉ bằng `Batches`; hãy nhìn cả render CPU time và `SetPass`.

## Common mistakes
- Nghĩ rằng `SRP Batcher` là cùng bản chất với static batching.
- Tạo quá nhiều shader keywords và variants rồi kỳ vọng SRP batch vẫn lớn.
- Dùng `MaterialPropertyBlock` rộng khắp trong `URP/HDRP` mà không để ý chi phí tương thích.

## Example
- Scene có nhiều material khác nhau nhưng đều dùng cùng một lit shader variant là case phù hợp cho `SRP Batcher`.

```csharp
// Many materials, same shader variant
// -> strong candidate for SRP Batcher
```

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Optimization]]
- [[Batching]]
- [[GPU Instancing]]
- [[Comparison]]

