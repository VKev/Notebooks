---
aliases:
  - Universal Render Pipeline
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `URP` là render pipeline chính thức của Unity cho đa nền tảng, tối ưu hiệu năng từ mobile đến console với single-pass forward rendering mặc định.

## Key points
- Unity `6.4 (6000.4)`: `Universal Render Pipeline` là prebuilt `Scriptable Render Pipeline` của Unity, thiết kế cho workflow artist-friendly và đồ họa tối ưu trên nhiều nền tảng từ mobile tới console và PC.
- URP hỗ trợ `Forward`, `Forward+`, `Deferred`, và `Deferred+` rendering path trong Universal Renderer.
- URP có `Universal Renderer` cho 3D/general rendering và `2D Renderer` cho project 2D.
- Trong Unity 6, custom rendering trong URP nên đi qua `Scriptable Render Pass` và `Render Graph`.
- Cấu hình qua `UniversalRenderPipelineAsset` gán trong Project Settings > Graphics.
- Khi URP active trong Graphics Settings, Unity dùng URP để render Game view, Scene view, Reflection Probe, và preview window.
- Camera loop chính gồm: setup culling parameters, culling, build rendering data, setup renderer, rồi execute renderer.
- `Renderer Feature` tạo và enqueue `ScriptableRenderPass` để thêm custom pass vào frame rendering loop mà không sửa source code URP.

## Decision rules
- Dùng cho đa số project mới, đặc biệt project cần chạy trên nhiều nền tảng.
- Khi cần cân bằng giữa chất lượng hình ảnh và hiệu năng.
- Khi team muốn tận dụng Shader Graph và Renderer Feature.
- Tránh nếu project cần đồ họa cao cấp nhất có thể trên PC/console, dùng `HDRP` thay thế.
- Tránh nếu project đang dùng Built-in RP với nhiều custom shader phức tạp và không có thời gian migrate.
- Hiệu năng tốt hơn Built-in RP nhờ single-pass rendering và batching tối ưu.
- Hỗ trợ rộng nhất trong các pipeline hiện đại của Unity: mobile, VR/AR, console, PC, WebGL2, và nhiều graphics API phổ biến.
- `Shader Graph` và `Renderer Feature` giúp mở rộng pipeline mà không cần viết HLSL thủ công.

## Example
```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;
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
    }
}
```

## Related notes
- [[Definition]]
- [[SRP]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[HDRP]]
- [[Shader Graph]]
- [[Summary]]
