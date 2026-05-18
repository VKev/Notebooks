---
aliases:
  - Scriptable Render Pipeline Batcher
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `SRP Batcher` là kỹ thuật tối ưu draw call trong SRP bằng cách giảm chi phí CPU của render state updates.

## What is it
- Theo Unity `6.3`, `SRP Batcher` không tối ưu bằng cách gộp mesh như batching hay instancing, mà tối ưu bằng cách giảm số lần cập nhật render state.
- Nó là kỹ thuật chỉ có trong Scriptable Render Pipeline như URP và HDRP; Built-in Render Pipeline không hỗ trợ.

## How it works
- `SRP Batcher` giữ dữ liệu material trong GPU buffers bền hơn giữa các draw call tương thích.
- Khi nhiều object dùng cùng shader variant tương thích, Unity giảm lượng công việc CPU phải chuẩn bị trước khi submit draw call.
- Trong Unity `6.3`, trang “Choose a method for optimizing draw calls” khuyến nghị bật `SRP Batcher` trong URP và HDRP.

## Why use it
- Hữu ích khi scene có nhiều renderer, nhiều material, nhưng vẫn bám trên cùng shader variant tương thích.
- Thường là kỹ thuật đầu tiên cần kiểm tra trong URP/HDRP vì nó giảm CPU render overhead mà không yêu cầu object phải cùng mesh.

## When to use it
- Dùng trong URP hoặc HDRP.
- Dùng khi bottleneck nằm ở CPU render preparation và scene có nhiều object dùng shader tương thích.

## When to not use it
- Không dùng được trong Built-in Render Pipeline.
- Không nên nhầm nó với GPU instancing; nếu bài toán của bạn là render rất nhiều instance giống hệt nhau, instancing vẫn có thể phù hợp hơn ở một số case.

## Limitations
- Không phải mọi shader đều tương thích tốt nếu bạn tạo shader hoặc material setup sai.
- Nhiều shader variants làm lợi ích của `SRP Batcher` giảm.
- Trong URP/HDRP, một số workflow dùng `MaterialPropertyBlock` có thể phá lợi thế tối ưu mà Unity khuyến nghị giữ.

---

## Example code
```csharp
using UnityEngine.Rendering;

public static class SRPBatcherExample
{
    public static void Enable()
    {
        GraphicsSettings.useScriptableRenderPipelineBatching = true;
    }
}
```

---

## Related notes
- [[Definition]]
- [[Render State]]
- [[Batching]]
- [[GPU Instancing]]
- [[Summary]]
