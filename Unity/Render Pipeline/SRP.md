---
aliases:
  - Scriptable Render Pipeline
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `SRP` là framework cho phép tạo render pipeline tùy chỉnh bằng C#, là nền tảng của cả `URP` và `HDRP`.

## Key points
- Unity `6.4 (6000.4)`: `Scriptable Render Pipeline` là lớp API mỏng cho phép schedule và configure rendering command bằng C# script.
- `URP` và `HDRP` đều được xây dựng trên SRP, nhưng developer cũng có thể tạo custom render pipeline riêng.
- SRP có hai phần chính: `RenderPipelineAsset` lưu cấu hình và tạo pipeline instance, còn `RenderPipeline` định nghĩa render loop bằng cách override `Render()`.
- `ScriptableRenderContext` là cầu nối giữa code C# của pipeline và low-level graphics code của Unity.
- `RenderPipelineAsset` là `ScriptableObject` được gán trong Project Settings > Graphics hoặc Quality Settings để chọn active pipeline.
- `RenderPipeline` class chứa method `Render()` được Unity gọi mỗi frame, nhận `ScriptableRenderContext` và danh sách camera.
- `ScriptableRenderContext` schedule command như culling, draw renderer, draw shadow, execute command buffer, rồi submit xuống graphics API.
- Ba giai đoạn render chính: `Culling` loại bỏ object ngoài view, `Rendering` vẽ object với lighting, `Post-processing` áp dụng hiệu ứng cuối.

## Decision rules
- Khi cần render pipeline hoàn toàn tùy chỉnh mà `URP` và `HDRP` không đáp ứng.
- Khi phát triển tool hoặc engine feature cần kiểm soát sâu quy trình render.
- Tránh tự viết pipeline từ đầu nếu `URP` hoặc `HDRP` đã đáp ứng nhu cầu, vì maintenance cost cao.
- Không dùng cho beginner, `URP` là lựa chọn mặc định an toàn cho đa số project.
- Giúp kiểm soát hoàn toàn quy trình render, tối ưu cho nhu cầu cụ thể của project.
- `URP` và `HDRP` xây trên SRP, nên hiểu SRP giúp hiểu cách hai pipeline này hoạt động.
- có thể viết custom render feature và render pass để thêm hiệu ứng mà pipeline mặc định không có.
- Viết custom pipeline đòi hỏi kiến thức sâu về graphics programming.

## Example
```csharp
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Rendering;
[CreateAssetMenu(menuName = "Rendering/Custom Render Pipeline")]
public class CustomRenderPipelineAsset : RenderPipelineAsset
{
    protected override RenderPipeline CreatePipeline()
    {
        return new CustomRenderPipeline();
    }
}
public class CustomRenderPipeline : RenderPipeline
{
    protected override void Render(ScriptableRenderContext context, List<Camera> cameras)
    {
        foreach (Camera camera in cameras)
        {
            camera.TryGetCullingParameters(out var cullingParams);
            var cullingResults = context.Cull(ref cullingParams);
            context.SetupCameraProperties(camera);
            var cmd = new CommandBuffer { name = "Clear" };
            cmd.ClearRenderTarget(true, true, Color.clear);
            context.ExecuteCommandBuffer(cmd);
            cmd.Release();
            var drawSettings = new DrawingSettings(
                new ShaderTagId("SRPDefaultUnlit"),
                new SortingSettings(camera));
            var filterSettings = FilteringSettings.defaultValue;
            context.DrawRenderers(cullingResults, ref drawSettings, ref filterSettings);
            context.Submit();
        }
    }
}
```

## Related notes
- [[Definition]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[HDRP]]
- [[Shader Graph]]
- [[Summary]]
