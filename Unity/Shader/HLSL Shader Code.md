---
aliases:
  - Unity HLSL
  - Hand Written Shader
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `HLSL Shader Code` là phần code chạy trên GPU bên trong `HLSLPROGRAM ... ENDHLSL`, gồm vertex shader, fragment shader, input struct, output struct, và texture sampling.

## Key points
- Unity `6.3 LTS (6000.3)`: Unity dùng `HLSL` để viết shader program.
- Trong shader viết tay, HLSL nằm trong `Pass` block của ShaderLab.
- Với URP, `HLSLPROGRAM` và include file như `Core.hlsl` là workflow hiện đại; `CGPROGRAM` chỉ nên dùng cho Built-in RP hoặc code legacy.
- Regular graphics shader cần ít nhất `#pragma vertex` và `#pragma fragment` để chỉ định function cho vertex stage và fragment stage.
- Vertex shader nhận vertex data như position, normal, UV từ mesh.
- Vertex shader trả về dữ liệu nội suy cho fragment shader, bắt buộc có clip-space position với semantic `SV_POSITION`.
- Fragment shader chạy cho pixel/fragment và trả về color với semantic như `SV_Target`.
- Include file của URP cung cấp helper như `TransformObjectToHClip()` để chuyển object space sang homogeneous clip space.
- Texture sampling cần khai báo texture, sampler, UV, và dùng property từ Material.

## Decision rules
- Cho kiểm soát trực tiếp hơn Shader Graph và dễ tối ưu cho effect đặc biệt.
- Phù hợp với full-screen effect, custom lighting, stylized rendering, debug rendering, và shader cần pattern code rõ ràng.
- Dễ review bằng diff khi shader logic phức tạp.
- Khi Shader Graph không biểu diễn được logic cần thiết.
- Khi cần tối ưu instruction, variant, precision, hoặc texture sampling.
- Khi cần share code bằng `.hlsl` include giữa nhiều shader.
- Tránh nếu artist cần iteration nhanh bằng node graph và shader không quá phức tạp.
- Không dùng Surface Shader cho URP/HDRP, vì Surface Shader là workflow của Built-in Render Pipeline.
- Không đổi `CGPROGRAM` sang `HLSLPROGRAM` máy móc trong shader cũ, vì `CGPROGRAM` tự include một số Built-in helper.
- HLSL phụ thuộc render pipeline vì include file, lighting function, và pass tag khác nhau.
- Sai semantic hoặc sai transform space tạo lỗi compile hoặc hình ảnh sai.
- Code shader có thể compile khác nhau theo graphics API, platform, và shader model target.

## Example
```csharp
Shader "Learning/URP/SolidColor"
{
    Properties
    {
        _BaseColor ("Base Color", Color) = (0.5, 0, 0, 1)
    }
    SubShader
    {
        Tags { "RenderType" = "Opaque" "RenderPipeline" = "UniversalPipeline" }
        Pass
        {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"
            CBUFFER_START(UnityPerMaterial)
                half4 _BaseColor;
            CBUFFER_END
            struct Attributes
            {
                float4 positionOS : POSITION;
            };
            struct Varyings
            {
                float4 positionHCS : SV_POSITION;
            };
            Varyings vert(Attributes input)
            {
                Varyings output;
                output.positionHCS = TransformObjectToHClip(input.positionOS.xyz);
                return output;
            }
            half4 frag(Varyings input) : SV_Target
            {
                return _BaseColor;
            }
            ENDHLSL
        }
    }
}
```

## Related notes
- [[Definition]]
- [[ShaderLab]]
- [[Shader Pragma]]
- [[Shader Render State]]
- [[../Render Pipeline/Shader Graph|Shader Graph]]
- [[Summary]]

