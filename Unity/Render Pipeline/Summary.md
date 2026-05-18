---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.4 (6000.4)`: URP và HDRP là pipeline dựng trên `Scriptable Render Pipeline`. Custom rendering mới trong URP nên dùng `Scriptable Render Pass` với `Render Graph`.

## Core keywords
- `SRP`: Lớp API mỏng cho phép schedule và configure rendering command bằng C#. Nền tảng của URP, HDRP, và custom render pipeline.
- `Render Graph`: API mô tả render pass, resource, và dependency của frame. Giúp URP tối ưu memory, bandwidth, pass culling, và pass merging.
- `Scriptable Render Pass`: Pass tùy chỉnh được inject vào URP frame rendering loop. Unity 6 workflow mới dùng `RecordRenderGraph()` thay vì chỉ dựa vào `Execute()`.
- `URP`: Render pipeline đa nền tảng, tối ưu từ mobile tới console và PC. Hỗ trợ Universal Renderer, 2D Renderer, Renderer Feature, và Render Graph.
- `HDRP`: Render pipeline high-fidelity cho nền tảng mạnh có compute shader. Hỗ trợ physical lighting, volumetric lighting, ray tracing, path tracing, và Custom Pass.
- `Shader Graph`: Visual editor tạo shader bằng node graph. Chỉ hỗ trợ URP và HDRP, không hỗ trợ Built-in RP.

## Rendering paths
- `Forward`: Tính lighting per-object trong một pass. Nhanh nhưng giới hạn số light per-object.
- `Forward+`: Mở rộng Forward, nới lỏng giới hạn light. Có trong URP từ Unity 2022.
- `Deferred`: Vẽ geometry vào G-Buffer rồi tính lighting riêng. Không giới hạn số light cho opaque object.
- `Deferred+`: Biến thể deferred mới trong URP Universal Renderer. Dùng khi cần nhiều light hơn và target hardware chịu được chi phí G-Buffer.

## Pipeline comparison
- `Built-in RP`: Pipeline cũ, customization hạn chế hơn SRP. Chỉ dùng cho legacy project.
- `URP`: Scalable, đa nền tảng, Renderer Feature và Render Graph mở rộng dễ. Lựa chọn mặc định cho project mới.
- `HDRP`: Chất lượng cao nhất, physical lighting, ray tracing. Không hỗ trợ mobile/WebGL/OpenGL, yêu cầu compute shader và phần cứng mạnh.

## Decision rules
- `Project mới, đa nền tảng`: Dùng URP. Đây là hướng chính thức của Unity cho tương lai.
- `Project cần đồ họa AAA trên PC/console`: Dùng HDRP. Chấp nhận giới hạn nền tảng để đổi lấy chất lượng.
- `Cần custom shader mà không viết HLSL`: Dùng Shader Graph. Artist có thể tự tạo và iterate nhanh.
- `Cần custom post-processing hoặc custom draw trong URP`: Dùng Scriptable Render Pass với Render Graph. Inject bằng Scriptable Renderer Feature nếu effect cần dùng rộng trong project.
- `Cần render pipeline hoàn toàn tùy chỉnh`: Viết custom SRP. Chỉ khi URP và HDRP không đáp ứng nhu cầu đặc biệt.

## Common traps
- `Shader Built-in RP dùng được trong URP`: Sai. Shader không tương thích giữa các pipeline. Cần viết lại hoặc dùng Shader Graph.
- `URP yếu hơn Built-in RP`: Sai. URP nhanh hơn nhờ single-pass và batching tối ưu. Unity đang deprecate Built-in RP vì Lý do này.
- `HDRP dùng được trên mobile`: Sai. HDRP không hỗ trợ mobile, WebGL, OpenGL, hoặc OpenGL ES. Dùng URP cho mobile.
- `TextureHandle có thể lưu qua frame`: Sai. Handle của Render Graph chỉ hợp lệ trong graph execution hiện tại. Muốn giữ texture qua frame thì dùng imported external resource.
- `Tạo material hoặc render texture trong AddRenderPasses`: Sai. `AddRenderPasses()` chạy mỗi frame mỗi camera. Tạo resource trong `Create()` hoặc quản lý lifetime rõ ràng.
- `Shader Graph tạo shader tối ưu như viết tay`: Không hẳn. Output có thể verbose hơn HLSL tự viết. Với đa số project, sự khác biệt không đáng kể.

## Review questions

### SRP là gì và tại sao quan trọng?
- SRP là API layer cho phép schedule/render bằng C#, là nền tảng của URP và HDRP, và là điểm bắt đầu nếu muốn viết render pipeline riêng.

### Render Graph giải quyết vấn đề gì?
- khai báo dependency giữa pass và resource để Unity có thể cull pass không dùng, tái sử dụng memory, đồng bộ compute/graphics, và giảm bandwidth.

### Scriptable Render Pass khác Renderer Feature thế nào?
- `ScriptableRenderPass` chứa logic render, còn `ScriptableRendererFeature` tạo và enqueue pass vào URP renderer.

### URP và HDRP khác nhau thế nào?
- URP tối ưu cho đa nền tảng từ mobile đến console, HDRP cho đồ họa cao cấp trên phần cứng mạnh. Shader/material không tương thích trực tiếp giữa hai pipeline.

### Vì sao project mới chọn URP thay vì Built-in RP?
- Vì URP là SRP-based pipeline, dễ mở rộng bằng Renderer Feature/Render Graph hơn và nhắm tới nhiều nền tảng hiện đại.

### Shader Graph có thay thế hoàn toàn HLSL không?
- Không. Shader Graph phù hợp cho đa số use case nhưng shader cực kỳ tối ưu hoặc compute shader vẫn cần HLSL.

### Forward và Deferred rendering khác nhau thế nào?
- Forward tính lighting per-object trong một pass, nhanh nhưng giới hạn light. Deferred vẽ geometry trước rồi tính lighting riêng, không giới hạn light cho opaque.

## Related notes
- [[Render Pipeline]]
- [[Definition]]
- [[SRP]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[HDRP]]
- [[Shader Graph]]
- [[../Unity|Unity]]
