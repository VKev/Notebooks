---
aliases:
  - Shader Variant
  - Shader Keyword
  - Multi Compile
  - multi_compile
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `#pragma` directive gửi instruction cho Unity shader compiler, ví dụ chọn vertex/fragment function, target shader model, khai báo keyword, và tạo shader variant bằng `multi_compile` hoặc `shader_feature`.

## Key points
- Unity `6.3 LTS (6000.3)`: `#pragma vertex` và `#pragma fragment` là bắt buộc trong regular graphics shader để chỉ định function cho vertex shader và fragment shader.
- `#pragma multi_compile` khai báo keyword set và Unity sẽ include mọi variant vào build.
- `#pragma shader_feature` khai báo keyword set nhưng Unity có thể loại variant không dùng khỏi build.
- `multi_compile_local` và `shader_feature_local` dùng local keyword để giảm va chạm với global keyword.
- `#pragma target` đặt shader model tối thiểu, còn `#pragma require` khai báo GPU feature tối thiểu.
- Mỗi dòng variant như `#pragma multi_compile _ FEATURE_ON` tạo nhiều shader program khác nhau.
- Nhiều dòng variant nhân với nhau, nên variant count tăng rất nhanh.
- Khi runtime bật keyword bằng material hoặc global keyword, Unity chọn shader variant tương ứng.
- `shader_feature` phù hợp cho feature bật/tắt trên Material.
- `multi_compile` phù hợp cho keyword được bật từ code hoặc keyword hệ thống mà build không thể strip an toàn.
- Suffix theo stage như `_fragment` giúp giới hạn variant cho fragment stage khi feature chỉ ảnh hưởng pixel shader.

## Decision rules
- Giúp một shader source hỗ trợ nhiều cấu hình mà không tạo nhiều shader asset riêng.
- Giúp bật/tắt feature như normal map, emission, detail map, alpha clip.
- Giúp tránh branch runtime nếu feature khác nhau đủ lớn để đáng compile thành variant riêng.
- Dùng `shader_feature` cho option trên material mà build có thể strip nếu không dùng.
- Dùng `multi_compile` cho keyword bật bằng code hoặc keyword mà bạn chắc chắn cần trong build.
- Dùng `_local` khi keyword chỉ thuộc shader đó.
- Dùng stage suffix khi keyword chỉ ảnh hưởng một shader stage.
- Không thêm keyword cho mọi option nhỏ vì variant count sẽ phình rất nhanh.
- Không dùng `multi_compile` nếu `shader_feature` đủ, vì `multi_compile` giữ mọi variant trong build.
- Không dùng keyword nếu branch runtime đơn giản hơn và không gây chi phí đáng kể.
- Variant explosion làm tăng thời gian import, thời gian build, kích thước build, và hitch khi load shader.
- Variant bị strip sai có thể làm material mất feature trong build.
- Global keyword có thể ảnh hưởng nhiều shader hơn dự kiến.

## Example
```csharp
HLSLPROGRAM
#pragma vertex vert
#pragma fragment frag
#pragma shader_feature_local _ EMISSION_ON
#pragma multi_compile_local _ OUTLINE_ON
#pragma target 3.5
half4 frag(Varyings input) : SV_Target
{
    half4 color = _BaseColor;
    #if defined(EMISSION_ON)
        color.rgb += _EmissionColor.rgb;
    #endif
    return color;
}
ENDHLSL
```

## Related notes
- [[Definition]]
- [[ShaderLab]]
- [[HLSL Shader Code]]
- [[Shader Render State]]
- [[../Render Pipeline/URP|URP]]
- [[Summary]]
