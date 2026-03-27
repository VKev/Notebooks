---
aliases:
  - Draw Call Optimization
  - Reduce Draw Calls
note_type: permanent
created: 2026-03-27
tags:
  - unity
---

## One-line answer
- Hãy tối ưu `draw call` khi scene bị `CPU-bound` ở rendering, rồi chọn phương pháp theo render pipeline và shape của object trong scene.

## Problem
- Nhiều project cố giảm draw call ngay lập tức mà chưa đo bottleneck thật.
- Kết quả thường là thêm complexity, tăng memory, hoặc phá batching nhưng không giảm frame time thực tế.

## Core idea
- Workflow đúng là:
  - đo trước
  - xác định pipeline
  - chọn đúng kỹ thuật cho đúng loại object
- Unity 6 docs nhấn mạnh rằng recommendation khác nhau giữa `URP`, `HDRP`, và `Built-In Render Pipeline`.

## Mechanism
- Bắt đầu bằng đo:
  - mở `Rendering Statistics` và xem `SetPass` cùng `Batches`
  - mở `Profiler`, chọn phần `Rendering`, rồi kiểm tra số draw call và thời gian render
  - dùng `Frame Debugger` để xem từng rendering event và tìm nơi batching hoặc instancing bị gãy
- Sau đó tối ưu theo pipeline:
  - `URP` hoặc `HDRP`:
    - bật `SRP Batcher`
    - trong Unity 6, `GPU Resident Drawer` là hướng GPU-instancing được docs khuyến nghị cho object tương thích
    - tránh `MaterialPropertyBlock` nếu bạn muốn giữ đường tối ưu thân thiện với SRP
    - manual `GPU Instancing` thường không phải lựa chọn mặc định đầu tiên
  - `Built-In Render Pipeline`:
    - dùng `static batching` cho object tĩnh
    - dùng `GPU Instancing` nếu có nhiều instance của cùng `mesh` và `material`
    - chỉ cân nhắc `dynamic batching` trên thiết bị yếu và sau khi đã profile
- Các rule tối ưu chung:
  - dùng chung `material` nhiều nhất có thể
  - nếu cùng `mesh` nhưng chỉ khác vài property, ưu tiên `Material Variants` thay vì nhân nhiều material riêng
  - nếu có nhiều instance hoặc nhiều lights, `LOD` cũng giúp giảm workload render
- Unity cũng nêu thứ tự ưu tiên trên từng object:
  - `SRP Batcher`, và `static batching` nếu bật
  - `GPU instancing`, bao gồm `GPU Resident Drawer` hoặc `BRG`
  - `dynamic batching`

## When to use
- Dùng note này khi bạn đang tối ưu scene thật, không chỉ học khái niệm.
- Hợp khi bạn cần checklist để quyết định nên bật gì trong project hiện tại.

## Benefits
- Tránh tối ưu nhầm chỗ.
- Giảm khả năng bật mọi feature cùng lúc rồi tự tạo regression.
- Giúp biến số liệu từ `Stats` hoặc `Profiler` thành hành động cụ thể.

## Trade-offs / Limits
- Mỗi pipeline có recommendation khác nhau; không có một checkbox đúng cho mọi project.
- Giảm draw-call overhead có thể đánh đổi bằng:
  - memory cao hơn
  - nhiều shader variants hơn
  - culling kém hơn nếu combine quá tay

## Common mistakes
- Đo mỗi `Batches` rồi kết luận.
- Tạo quá nhiều material gần giống nhau.
- Bật `dynamic batching` như một default optimization.
- Ở `URP/HDRP`, dùng `MaterialPropertyBlock` bừa rồi thắc mắc vì sao đường tối ưu của SRP không còn tốt.
- Cố stack nhiều kỹ thuật lên cùng một object dù Unity chỉ dùng một optimization path chính cho object đó.

## Example
- Decision rule ngắn:

```csharp
if (pipeline == SRP)
    prefer SRPBatcher;
else if (manySameMeshAndMaterial)
    consider GPUInstancing;
else if (staticGeometry)
    consider StaticBatching;
```

## Related notes
- [[Draw Call]]
- [[Definition]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Comparison]]

