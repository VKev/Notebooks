---
aliases:
  - Visual Shader Editor
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Shader Graph` là công cụ visual trong Unity cho phép tạo shader bằng cách nối node trên đồ thị thay vì viết code HLSL.

## Key points
- Unity `6.4 (6000.4)`: `Shader Graph` là visual editor cho phép artist và developer tạo shader bằng cách kéo thả node và nối chúng lại, không cần viết HLSL thủ công.
- Shader Graph chỉ hoạt động với SRP-based pipeline (`URP` và `HDRP`), không hỗ trợ Built-in RP.
- Output của Shader Graph là shader asset có thể gán vào material giống shader viết tay.
- Tạo Shader Graph asset qua Assets > Create > Shader Graph, chọn loại phù hợp với pipeline đang dùng.
- Node graph gồm các node input (texture, color, position, time), node xử lý (math, UV, noise), và Master Stack output.
- Master Stack định nghĩa output cuối cùng: `Base Color`, `Normal`, `Metallic`, `Smoothness`, `Emission`, `Alpha`, v.v.
- Shader Graph hỗ trợ Sub Graph để tái sử dụng logic shader giữa nhiều shader khác nhau.
- Preview window hiển thị kết quả real-time khi chỉnh sửa node.

## Decision rules
- Khi project dùng `URP` hoặc `HDRP` và cần custom shader.
- Khi artist muốn tạo hiệu ứng visual mà không phụ thuộc vào programmer.
- Dùng cho prototype nhanh hiệu ứng shader trước khi optimize bằng HLSL nếu cần.
- Tránh nếu project dùng Built-in RP, vì Shader Graph không hỗ trợ.
- Không dùng cho shader cực kỳ phức tạp hoặc cần tối ưu performance tối đa, viết HLSL trực tiếp có thể hiệu quả hơn.
- Không dùng cho compute shader hoặc shader không liên quan đến rendering visual.
- Artist có thể tạo và chỉnh shader trực quan mà không cần biết HLSL.
- Iteration nhanh hơn nhờ preview real-time và node-based workflow.

## Example
```
Shader Graph không dùng code truyền thống mà dùng node graph.
Workflow cơ bản:
1. Assets > Create > Shader Graph > URP > Lit Shader Graph
2. Mở Shader Graph window
3. Thêm node: Texture 2D Asset → Sample Texture 2D → Base Color
4. Thêm node: Normal Map → Sample Texture 2D → Normal
5. Kết nối output vào Master Stack
6. Save Asset
7. Tạo Material, gán Shader Graph vừa tạo
8. Gán Material lên object trong scene
Custom Function node cho phép viết HLSL inline:
- Thêm node Custom Function
- Chọn mode String hoặc File
- Viết HLSL code cho logic đặc biệt
```

## Related notes
- [[Definition]]
- [[SRP]]
- [[URP]]
- [[HDRP]]
- [[../Shader/ShaderLab|ShaderLab]]
- [[../Shader/HLSL Shader Code|HLSL Shader Code]]
- [[Summary]]
