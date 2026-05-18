---
aliases:
  - RenderGraph
  - URP Render Graph
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Render Graph` là hệ thống API trong SRP Core dùng để mô tả các render pass, tài nguyên, và dependency của frame để Unity có thể tối ưu memory, bandwidth, và thứ tự render.

## Key points
- Unity `6.4 (6000.4)`: Render Graph là API dùng để tạo `Scriptable Render Pass` trong `URP`.
- Khi dùng Render Graph, custom pass của bạn trở thành một phần của render graph nội bộ mà URP chạy mỗi frame.
- Render Graph tách rõ hai giai đoạn: `recording` để khai báo input/output resource, và `execution` để chạy graphics command thật sự.
- `HDRP` cũng dùng Render Graph nội bộ để mô tả render loop, tối ưu GPU memory, và loại bỏ pass không cần thiết theo camera frame settings.
- `RecordRenderGraph()` là nơi bạn đăng ký pass, khai báo texture cần đọc, texture cần ghi, render target, và dữ liệu pass.
- `PassData` chỉ nên chứa đúng dữ liệu pass cần dùng, vì dữ liệu thừa có thể tăng chi phí.
- `TextureHandle` là handle tạm thời tới texture trong graph, không phải texture sống độc lập ngoài graph.
- `SetRenderFunc()` đăng ký function sẽ chạy ở execution stage, nơi bạn ghi command như blit, draw object, hoặc dispatch compute shader.

## Decision rules
- Khi viết custom render pass mới trong URP Unity 6 trở lên.
- Khi cần đọc camera color, depth, normal, hoặc tạo temporary texture trong một frame.
- Khi muốn Unity tối ưu lifetime của render texture thay vì tự cấp phát thủ công.
- Khi cần debug dependency giữa các pass bằng Render Graph Viewer, Frame Debugger, hoặc Rendering Debugger.
- Tránh nếu chỉ cần chỉnh shader/material thông mà không can thiệp vào render loop.
- Không dùng `TextureHandle` như texture lưu giữa nhiều frame; nếu cần dữ liệu sống lâu hơn graph, dùng imported external resource.
- Render Graph có thể bỏ resource không dùng trong frame.
- Render Graph có thể bỏ pass nếu output của pass không ảnh hưởng tới frame cuối.

## Example
```csharp
using UnityEngine.Rendering;
using UnityEngine.Rendering.RenderGraphModule;
using UnityEngine.Rendering.Universal;
public class ExampleRenderGraphPass : ScriptableRenderPass
{
    private class PassData
    {
        public TextureHandle sourceColor;
    }
    public override void RecordRenderGraph(
        RenderGraph renderGraph,
        ContextContainer frameContext)
    {
        using (var builder = renderGraph.AddRasterRenderPass<PassData>(
            "Example Render Graph Pass",
            out var passData))
        {
            UniversalResourceData resources = frameContext.Get<UniversalResourceData>();
            passData.sourceColor = resources.activeColorTexture;
            builder.UseTexture(passData.sourceColor);
            builder.SetRenderFunc(static (PassData data, RasterGraphContext context) =>
            {
            });
        }
    }
}
```

## Related notes
- [[Definition]]
- [[SRP]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[HDRP]]
- [[Summary]]
