---
aliases:
  - GPU Render State
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Render State` là tập hợp trạng thái mà Unity và GPU phải chuẩn bị trước khi vẽ, và đổi trạng thái này quá thường xuyên sẽ làm draw call tốn CPU hơn.

## Key points
- Theo Unity `6.4`, trước mỗi draw call CPU phải cập nhật render state rồi mới submit lệnh vẽ; bước tốn CPU nhất trong draw call.
- Trong ngữ cảnh draw call của Unity, render state bao gồm những thứ GPU cần để vẽ như shader code, texture, buffer, và các thiết lập liên quan.
- Ở mức ShaderLab, Unity cung cấp các GPU render state commands như `Blend`, `Cull`, `ZTest`, `ZWrite`, `Stencil`, `ColorMask`, và `Offset`.
- Khi Unity chuẩn bị vẽ một object, CPU dùng graphics API để set trạng thái mà GPU cần cho pass hiện tại.
- Nếu object kế tiếp dùng state khác, CPU lại phải cập nhật state thêm một lần nữa trước khi submit draw call.
- Nếu nhiều object dùng cùng render state, Unity dễ giảm số lần state update hơn thông qua các kỹ thuật như `Batching` hoặc `SRP Batcher`.

## Decision rules
- Khi bạn muốn phân tích vì sao scene bị CPU-bound ở phần rendering.
- Khi kiểm tra tính tương thích của `Batching`, `GPU Instancing`, hoặc `SRP Batcher`.
- Khi viết hoặc đọc shader có các lệnh như `Blend`, `Cull`, `ZTest`, `ZWrite`, hoặc `Stencil`.
- Tránh tối ưu render state theo cảm giác nếu chưa profile và chưa xác nhận bottleneck nằm ở CPU rendering.
- Tránh coi mọi thay đổi render state đều xấu; có nhiều state là bắt buộc để đúng hình ảnh, nhất là transparent, decal, hoặc effect shader.
- Giúp hiểu vì sao đổi shader, material, pass, hoặc state liên quan có thể làm `SetPass calls` và CPU render cost tăng.
- Giúp đọc `Frame Debugger`, `Profiler`, và `Rendering Statistics` đúng hơn khi phân tích draw call.
- Giúp phân biệt rõ bài toán “giảm số draw call” với bài toán “giảm chi phí đổi render state”.

## Example
```shaderlab
Shader "Custom/RenderStateExample"
{
    SubShader
    {
        Tags { "Queue" = "Transparent" "RenderType" = "Transparent" }
        Pass
        {
            Blend SrcAlpha OneMinusSrcAlpha
            Cull Back
            ZTest LEqual
            ZWrite Off
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            ENDHLSL
        }
    }
}
```

## Related notes
- [[Definition]]
- [[Batching]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Summary]]
