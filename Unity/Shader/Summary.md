---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3 LTS (6000.3)`: Shader viết tay nên dùng `ShaderLab` để tổ chức shader và `HLSL` cho code GPU. Với URP/HDRP, ưu tiên `HLSLPROGRAM` thay vì `CGPROGRAM`.

## Core keywords
- `ShaderLab`: Ngôn ngữ khai báo cấu trúc shader trong Unity. Tổ chức `Shader`, `Properties`, `SubShader`, `Pass`, tag, render state, và HLSL block.
- `HLSLPROGRAM`: Block chứa HLSL code trong một `Pass`. Tương thích mọi render pipeline hiện đại của Unity.
- `#pragma vertex / fragment`: Chỉ định function cho vertex shader và fragment shader. Bắt buộc trong regular graphics shader.
- `multi_compile`: Tạo mọi variant và include vào build. Dùng khi keyword cần tồn tại vì code hoặc hệ thống runtime có thể bật.
- `shader_feature`: Tạo variant nhưng Unity có thể strip variant không dùng. Dùng cho feature bật/tắt trên material.
- `Cull`: Điều khiển bỏ mặt trước, mặt sau, hoặc không bỏ mặt nào. Mặc định là `Cull Back`.
- `ZWrite`: Điều khiển ghi depth buffer. Opaque `On`, transparent `Off`.
- `Blend`: Điều khiển trộn màu với render target. Alpha blending phổ biến là `Blend SrcAlpha OneMinusSrcAlpha`.

## Decision rules
- `Cần shader đơn giản cho artist`: Dùng Shader Graph. Iteration nhanh và ít lỗi cú pháp.
- `Cần kiểm soát GPU code rõ ràng`: Viết HLSL shader tay. Phù hợp effect đặc biệt hoặc tối ưu sâu.
- `Feature là option material`: Dùng `shader_feature_local`. Build có thể strip variant không dùng.
- `Keyword bật từ code runtime`: Dùng `multi_compile_local`. Tránh variant bị strip ngoài ý muốn.
- `Mesh kín bình thường`: Dùng `Cull Back`. Giảm vẽ mặt sau không nhìn thấy.
- `Object mỏng hai mặt`: Dùng `Cull Off`. Chấp nhận chi phí vẽ cao hơn.

## Common traps
- `Dùng CGPROGRAM trong URP/HDRP`: Không nên. `CGPROGRAM` là workflow Built-in/legacy. Dùng `HLSLPROGRAM` cho SRP hiện đại.
- `Lạm dụng multi_compile`: Làm tăng variant count và build size. Dùng `shader_feature` nếu feature chỉ nằm trên material.
- `Tắt ZWrite cho opaque`: Gây depth buffer sai. Chỉ tắt khi cần transparent hoặc effect đặc biệt.
- `Cull Off mọi shader`: Tăng overdraw và chi phí fragment. Chỉ dùng cho geometry thật sự cần hai mặt.

## Review questions

### ShaderLab khác HLSL thế nào?
- ShaderLab mô tả cấu trúc và render state của shader, còn HLSL là code chạy trên GPU.

### `multi_compile` khác `shader_feature` thế nào?
- `multi_compile` giữ mọi variant trong build, còn `shader_feature` cho phép Unity strip variant không dùng.

### Khi nào dùng `Cull Off`?
- Khi object là mặt mỏng cần nhìn từ hai phía, ví dụ lá cây, cloth plane, grass card, hoặc double-sided wall.

### Vì sao transparent dùng `ZWrite Off`?
- Vì transparent object cần blend với object phía sau. Ghi depth quá sớm có thể che sai các layer transparent khác.

## Related notes
- [[Shader]]
- [[Definition]]
- [[ShaderLab]]
- [[HLSL Shader Code]]
- [[Shader Pragma]]
- [[Shader Render State]]
- [[../Render Pipeline/Shader Graph|Shader Graph]]
- [[../Texture/Texture Import Settings|Texture Import Settings]]
- [[../Unity|Unity]]
