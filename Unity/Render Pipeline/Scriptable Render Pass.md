---
aliases:
  - ScriptableRenderPass
  - Custom Render Pass
  - URP Render Pass
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Scriptable Render Pass` là pass tùy chỉnh trong URP cho phép chèn logic render riêng vào frame rendering loop.

## Key points
- Unity `6.3 LTS (6000.3)`: Scriptable Render Pass là cách thay đổi cách URP render scene hoặc object trong scene.
- Pass kế thừa `ScriptableRenderPass` và được inject vào pipeline bằng `ScriptableRendererFeature` hoặc `RenderPipelineManager`.
- Trong Unity 6, workflow mới nên dùng `RecordRenderGraph()` và Render Graph API; workflow `Execute()` kiểu compatibility mode chủ yếu để migrate code cũ.
- Injection point quyết định pass chạy lúc nào, ví dụ sau skybox, sau opaques, trước post-processing, hoặc sau post-processing.
- Tạo class kế thừa `ScriptableRenderPass`.
- Trong `RecordRenderGraph()`, khai báo resource cần đọc/ghi bằng Render Graph API.
- Đăng ký render function bằng `SetRenderFunc()` hoặc dùng helper như `AddBlitPass()`.
- Tạo `ScriptableRendererFeature` để giữ instance của pass và gọi `renderer.EnqueuePass(pass)`.
- URP gọi `AddRenderPasses()` mỗi frame, một lần cho mỗi camera, nên không nên allocate resource mới trong method này.

## Decision rules
- Thêm hiệu ứng full-screen custom như blur, outline, grayscale, fog, heat distortion.
- Render thêm object đặc biệt bằng layer, shader tag, hoặc material override.
- Đọc camera buffer như color/depth/normal để tạo hiệu ứng hậu kỳ.
- Chèn debug pass hoặc visualization pass vào render loop mà không sửa source code URP.
- Khi hiệu ứng cần truy cập camera color/depth hoặc cần chạy đúng thời điểm trong frame.
- Khi built-in Renderer Feature không đủ cho nhu cầu của project.
- Khi cần đóng gói custom rendering thành asset có thể bật/tắt trong Universal Renderer.
- Không dùng cho logic gameplay hoặc logic không liên quan tới rendering.
- Tránh nếu shader hoặc material đã giải quyết được vấn đề.
- Không instantiate material, render texture, hoặc pass mới trong `AddRenderPasses()`.
- Chỉ áp dụng cho URP; HDRP dùng hệ thống `Custom Pass` riêng.
- Cần khai báo đúng input/output resource để Render Graph không cull hoặc reorder sai pass.
- Pass chạy theo camera, nên camera stack, Scene View, Preview Camera, và XR có thể cần điều kiện lọc riêng.
- Renderer Feature phải được thêm vào Universal Renderer asset đang active, nếu không pass sẽ không chạy.

## Example
```csharp
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.RenderGraphModule;
using UnityEngine.Rendering.RenderGraphModule.Util;
using UnityEngine.Rendering.Universal;
public class ExampleRendererFeature : ScriptableRendererFeature
{
    private ExamplePass _pass;
    public Material blitMaterial;
    public override void Create()
    {
        _pass = new ExamplePass(blitMaterial)
        {
            renderPassEvent = RenderPassEvent.AfterRenderingPostProcessing
        };
    }
    public override void AddRenderPasses(
        ScriptableRenderer renderer,
        ref RenderingData renderingData)
    {
        if (renderingData.cameraData.cameraType == CameraType.Game)
            renderer.EnqueuePass(_pass);
    }
    private class ExamplePass : ScriptableRenderPass
    {
        private readonly Material _material;
        public ExamplePass(Material material)
        {
            _material = material;
        }
        public override void RecordRenderGraph(
            RenderGraph renderGraph,
            ContextContainer frameContext)
        {
            if (_material == null)
                return;
            UniversalResourceData resources = frameContext.Get<UniversalResourceData>();
            TextureHandle source = resources.activeColorTexture;
            TextureDesc desc = source.GetDescriptor(renderGraph);
            desc.name = "Example Blit Result";
            desc.depthBufferBits = 0;
            TextureHandle destination = renderGraph.CreateTexture(desc);
            var parameters = new RenderGraphUtils.BlitMaterialParameters(
                source,
                destination,
                _material,
                0);
            renderGraph.AddBlitPass(parameters, "Example Material Blit");
            resources.cameraColor = destination;
        }
    }
}
```

## Related notes
- [[Definition]]
- [[SRP]]
- [[Render Graph]]
- [[URP]]
- [[HDRP]]
- [[Summary]]
