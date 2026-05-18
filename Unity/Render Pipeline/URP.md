---
aliases:
  - Universal Render Pipeline
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `URP` là render pipeline chính thức của Unity cho đa nền tảng, tối ưu hiệu năng từ mobile đến console với single-pass forward rendering mặc định.

## What is it
- Theo tài liệu Unity `6.3 LTS (6000.3)`, `Universal Render Pipeline` là prebuilt `Scriptable Render Pipeline` của Unity, thiết kế cho workflow artist-friendly và đồ họa tối ưu trên nhiều nền tảng từ mobile tới console và PC.
- URP hỗ trợ `Forward`, `Forward+`, `Deferred`, và `Deferred+` rendering path trong Universal Renderer.
- URP có `Universal Renderer` cho 3D/general rendering và `2D Renderer` cho project 2D.
- Trong Unity 6, custom rendering trong URP nên đi qua `Scriptable Render Pass` và `Render Graph`.

## How it works
- Cấu hình qua `UniversalRenderPipelineAsset` gán trong Project Settings > Graphics.
- Khi URP active trong Graphics Settings, Unity dùng URP để render Game view, Scene view, Reflection Probe, và preview window.
- Camera loop chính gồm: setup culling parameters, culling, build rendering data, setup renderer, rồi execute renderer.
- `Renderer Feature` tạo và enqueue `ScriptableRenderPass` để thêm custom pass vào frame rendering loop mà không sửa source code URP.
- `Render Graph` khai báo resource dependency của pass để URP có thể tối ưu pass, texture lifetime, và memory bandwidth.
- URP hỗ trợ `Shader Graph` để tạo shader trực quan, và có sẵn nhiều shader lit/unlit.

## Why use it
- Hiệu năng tốt hơn Built-in RP nhờ single-pass rendering và batching tối ưu.
- Hỗ trợ rộng nhất trong các pipeline hiện đại của Unity: mobile, VR/AR, console, PC, WebGL2, và nhiều graphics API phổ biến.
- `Shader Graph` và `Renderer Feature` giúp mở rộng pipeline mà không cần viết HLSL thủ công.
- URP là lựa chọn SRP mặc định thực tế cho project mới cần cân bằng chất lượng, hiệu năng, và phạm vi nền tảng.

## When to use it
- Dùng cho hầu hết project mới, đặc biệt project cần chạy trên nhiều nền tảng.
- Dùng khi cần cân bằng giữa chất lượng hình ảnh và hiệu năng.
- Dùng khi team muốn tận dụng Shader Graph và Renderer Feature.

## When to not use it
- Không dùng nếu project cần đồ họa cao cấp nhất có thể trên PC/console, dùng `HDRP` thay thế.
- Không dùng nếu project đang dùng Built-in RP với nhiều custom shader phức tạp và không có thời gian migrate.

## Limitations
- Forward rendering path giới hạn số real-time light per-object, dù Forward+ đã nới lỏng giới hạn này.
- Một số feature của Built-in RP chưa có trong URP, ví dụ một số post-processing effect.
- Shader viết cho Built-in RP không tương thích trực tiếp, cần viết lại hoặc dùng Shader Graph.
- Project URP không tương thích trực tiếp với HDRP hoặc Built-in RP, nên nên chọn pipeline sớm.
- Custom pass viết bằng compatibility API cũ nên migrate dần sang Render Graph trong Unity 6.

---

## Example code
```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

// Renderer Feature để inject pass vào URP frame loop
public class ExampleFeature : ScriptableRendererFeature
{
    private ScriptableRenderPass _pass;

    public override void Create()
    {
        _pass = new MyCustomPass
        {
            renderPassEvent = RenderPassEvent.AfterRenderingOpaques
        };
    }

    public override void AddRenderPasses(
        ScriptableRenderer renderer,
        ref RenderingData renderingData)
    {
        if (renderingData.cameraData.cameraType == CameraType.Game)
            renderer.EnqueuePass(_pass);
    }

    private class MyCustomPass : ScriptableRenderPass
    {
        // Xem [[Scriptable Render Pass]] để viết RecordRenderGraph().
    }
}
```

---

## Related notes
- [[Definition]]
- [[SRP]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[HDRP]]
- [[Shader Graph]]
- [[Summary]]
