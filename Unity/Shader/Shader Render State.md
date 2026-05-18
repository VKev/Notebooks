---
aliases:
  - Cull
  - ZWrite
  - ZTest
  - Blend
  - GPU Render State
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Shader Render State` là các lệnh ShaderLab như `Cull`, `ZWrite`, `ZTest`, và `Blend` dùng để điều khiển cách GPU loại mặt, ghi depth, test depth, và trộn màu.

## Key points
- Unity `6.4 (6000.4)`: render state command có thể đặt trong `Pass` để áp dụng cho pass đó, hoặc trong `SubShader` để áp dụng cho mọi pass bên trong.
- `Cull` quyết định GPU bỏ mặt nào dựa trên hướng polygon so với camera.
- `ZWrite` quyết định pass có ghi vào depth buffer hay không.
- `ZTest` quyết định điều kiện so sánh depth khi vẽ pixel.
- `Blend` quyết định cách màu mới trộn với màu đã có trong render target.
- `Cull Back` bỏ mặt sau và là mặc định, phù hợp với mesh kín bình thường.
- `Cull Front` bỏ mặt trước, hay dùng cho hiệu ứng inside-out hoặc outline pass.
- `Cull Off` vẽ cả hai mặt, dùng cho lá cây, vải mỏng, double-sided wall, hoặc transparent effect.

## Decision rules
- Dùng `Cull Back`, `ZWrite On`, `ZTest LEqual` cho opaque object mặc định.
- Dùng `Cull Off` cho lá, grass card, cloth plane, sprite-like mesh, hoặc object cần double-sided.
- Dùng `Blend` và `ZWrite Off` cho transparent surface.
- Dùng `Cull Front` trong outline shader kiểu vẽ mesh phóng to ra sau.
- Không dùng `Cull Off` cho mọi thứ vì tăng chi phí vẽ và dễ gây overdraw.
- Không tắt `ZWrite` cho opaque object vì sẽ làm depth buffer thiếu thông tin.
- Điều khiển đúng render state giúp tránh lỗi depth sorting, mặt trong bị hiện, transparent sai thứ tự, hoặc overdraw không cần thiết.
- `Cull Back` giảm lượng triangle/pixel cần vẽ cho mesh kín.

## Example
```csharp
Pass
{
    Cull Back
    ZWrite On
    ZTest LEqual
}
Pass
{
    Cull Off
    ZWrite Off
    ZTest LEqual
    Blend SrcAlpha OneMinusSrcAlpha
}
```

## Related notes
- [[Definition]]
- [[ShaderLab]]
- [[HLSL Shader Code]]
- [[Shader Pragma]]
- [[../Draw Call/Draw Call|Draw Call]]
- [[Summary]]
