---
aliases:
  - RenderGraph
  - URP Render Graph
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## One-line
- `Render Graph` là hệ thống API trong SRP Core dùng để mô tả các render pass, tài nguyên, và dependency của frame để Unity có thể tối ưu memory, bandwidth, và thứ tự render.

## What is it
- Theo tài liệu Unity `6.3 LTS (6000.3)`, Render Graph là API dùng để tạo `Scriptable Render Pass` trong `URP`.
- Khi dùng Render Graph, custom pass của bạn trở thành một phần của render graph nội bộ mà URP chạy mỗi frame.
- Render Graph tách rõ hai giai đoạn: `recording` để khai báo input/output resource, và `execution` để chạy graphics command thật sự.
- `HDRP` cũng dùng Render Graph nội bộ để mô tả render loop, tối ưu GPU memory, và loại bỏ pass không cần thiết theo camera frame settings.

## How it works
- `RecordRenderGraph()` là nơi bạn đăng ký pass, khai báo texture cần đọc, texture cần ghi, render target, và dữ liệu pass.
- `PassData` chỉ nên chứa đúng dữ liệu pass cần dùng, vì dữ liệu thừa có thể tăng chi phí.
- `TextureHandle` là handle tạm thời tới texture trong graph, không phải texture sống độc lập ngoài graph.
- `SetRenderFunc()` đăng ký function sẽ chạy ở execution stage, nơi bạn ghi command như blit, draw object, hoặc dispatch compute shader.
- Render Graph tự tính dependency giữa các pass dựa trên việc pass nào đọc và ghi resource nào.

## Why use it
- Render Graph có thể bỏ resource không dùng trong frame.
- Render Graph có thể bỏ pass nếu output của pass không ảnh hưởng tới frame cuối.
- Render Graph có thể tái sử dụng memory của texture có cùng đặc điểm.
- Render Graph có thể đồng bộ graphics queue và compute queue khi compute shader tham gia frame.
- Trên tile-based GPU, Render Graph có thể merge nhiều pass thành native render pass để giảm memory bandwidth.

## When to use it
- Dùng khi viết custom render pass mới trong URP Unity 6 trở lên.
- Dùng khi cần đọc camera color, depth, normal, hoặc tạo temporary texture trong một frame.
- Dùng khi muốn Unity tối ưu lifetime của render texture thay vì tự cấp phát thủ công.
- Dùng khi cần debug dependency giữa các pass bằng Render Graph Viewer, Frame Debugger, hoặc Rendering Debugger.

## When to not use it
- Không dùng nếu chỉ cần chỉnh shader/material thông thường mà không can thiệp vào render loop.
- Không dùng `TextureHandle` như texture lưu giữa nhiều frame; nếu cần dữ liệu sống lâu hơn graph, dùng imported external resource.
- Không dùng `AllowPassCulling(false)` trong production trừ khi có lý do rõ ràng, vì nó ngăn Unity loại bỏ pass không cần thiết.

## Limitations
- API Render Graph khó hơn `CommandBuffer` kiểu cũ vì bạn phải khai báo dependency chính xác.
- Resource tạo bên trong graph chỉ tồn tại trong lifetime của graph, không truy cập trực tiếp sau frame.
- Nếu khai báo thiếu read/write dependency, Render Graph có thể reorder hoặc cull pass theo cách gây lỗi visual.
- Compatibility Mode API như `SetRenderTarget` chỉ nên dùng khi cần migrate code cũ.

---

## Example code
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

            // Skeleton: pass thật cần khai báo output nếu muốn không bị cull.
            builder.UseTexture(passData.sourceColor);
            builder.SetRenderFunc(static (PassData data, RasterGraphContext context) =>
            {
                // Execution stage: ghi draw, blit, hoặc compute command ở đây.
            });
        }
    }
}
```

---

## Related notes
- [[Definition]]
- [[SRP]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[HDRP]]
- [[Summary]]
