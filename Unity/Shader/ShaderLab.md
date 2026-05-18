---
aliases:
  - ShaderLab Structure
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `ShaderLab` là ngôn ngữ khai báo của Unity dùng để tổ chức `Shader`, `Properties`, `SubShader`, `Pass`, tag, render state, và block code HLSL.

## Key points
- Unity `6.4 (6000.4)`: một shader file Unity có cấu trúc lồng nhau gồm `Shader`, một hoặc nhiều `SubShader`, và các `Pass`.
- `Shader` block định nghĩa tên shader trong Material shader menu và bọc toàn bộ cấu hình.
- `Properties` block khai báo giá trị xuất hiện trong Inspector của Material.
- `SubShader` block chứa implementation cho một nhóm pipeline hoặc phần cứng tương thích.
- `Pass` block định nghĩa một lần vẽ cụ thể, gồm tag, render state, và `HLSLPROGRAM`.
- Unity chọn `SubShader` đầu tiên tương thích với render pipeline và target device.
- `Tags { "RenderPipeline" = "UniversalPipeline" }` giới hạn SubShader cho URP.
- `Tags { "RenderPipeline" = "HDRenderPipeline" }` giới hạn SubShader cho HDRP.

## Decision rules
- Khi viết shader tay thay vì dùng Shader Graph.
- Khi cần custom pass, custom render state, hoặc custom GPU code.
- Khi cần shader tương thích với URP/HDRP theo tag cụ thể.
- Không cần viết ShaderLab tay nếu Shader Graph đủ cho hiệu ứng cần làm.
- Tránh viết nhiều SubShader phức tạp nếu project chỉ target một pipeline cố định.
- Giúp material expose property rõ ràng cho artist và designer.
- Giúp shader có nhiều SubShader cho nhiều render pipeline khác nhau.
- Giúp tách cấu trúc render state khỏi logic HLSL.

## Example
```csharp
Shader "Learning/URP/BasicShaderLab"
{
    Properties
    {
        _BaseColor ("Base Color", Color) = (1, 1, 1, 1)
    }
    SubShader
    {
        Tags
        {
            "RenderType" = "Opaque"
            "RenderPipeline" = "UniversalPipeline"
        }
        Pass
        {
            Cull Back
            ZWrite On
            ZTest LEqual
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
- [[HLSL Shader Code]]
- [[Shader Pragma]]
- [[Shader Render State]]
- [[../Render Pipeline/URP|URP]]
- [[../Render Pipeline/HDRP|HDRP]]
- [[Summary]]
