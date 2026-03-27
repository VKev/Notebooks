---
note_type: structure
tags:
  - unity
---

# Draw Call

## Scope
- Nhóm note này chứa mental model về `draw call`, cách đo số lượng và chi phí của nó, cùng các hướng tối ưu chính trong Unity.
- Father note này đóng vai trò `structure note` cho toàn bộ folder.

## Notes
- [[Definition]]: `draw call` là gì, CPU đang làm gì trước khi GPU vẽ, và vì sao `SetPass` quan trọng.
- [[Optimization]]: workflow đo `SetPass`, `Batches`, Profiler, Frame Debugger, rồi chọn hướng tối ưu phù hợp.
- [[Batching]]: static batching, dynamic batching, manual combine, và các giới hạn quan trọng.
- [[GPU Instancing]]: render nhiều instance của cùng `mesh` và `material` bằng instanced draw path.
- [[SRP Batcher]]: giảm render-state overhead trong URP, HDRP, và custom SRP.
- [[Comparison]]: cách chọn giữa `SRP Batcher`, `GPU Instancing`, `Batching`, và manual combine.

## Related structure notes
- [[Unity]]: quy luật note cho Unity field.

