---
note_type: summary
tags:
  - unity
sticker: lucide//atom
---

## Version scope
- `Unity 6.3 LTS (6000.3)`: Texture import setting quyết định runtime texture format, memory, filtering, mipmap, và shader sampling behavior. Platform override cần được kiểm tra riêng cho desktop, iOS, Android, console, và Web.

## Core keywords
- `Texture Type`: Mục đích import texture như Default, Normal Map, Sprite, Cubemap. Chọn sai type dễ làm shader đọc dữ liệu sai.
- `sRGB`: Bật cho color texture như albedo/base color. Tắt cho data texture như mask, metallic, roughness, height, normal.
- `Read/Write`: Cho script đọc/ghi pixel trên CPU. Tốn thêm memory vì Unity giữ copy CPU-side.
- `Mipmap`: Chuỗi resolution nhỏ dần của texture. Giảm aliasing và bandwidth cho texture 3D ở xa.
- `Wrap Mode`: Quyết định UV ngoài `0..1` lặp, clamp, mirror, hoặc per-axis. `Clamp` hay dùng cho UI, decal, mask, LUT.
- `Filter Mode`: Point cho pixel art, Bilinear cho smooth cơ bản, Trilinear cho mip transition mượt. Filter mode ảnh hưởng trực tiếp tới độ sắc và artifact.
- `Aniso Level`: Cải thiện texture nhìn ở góc nghiêng. Hữu ích cho floor/ground nhưng tốn tài nguyên hơn.
- `Compression`: Giảm memory, bandwidth, và build size. Format tốt nhất phụ thuộc platform.

## Decision rules
- `Texture là albedo/base color`: Bật `sRGB`. Bật mipmap nếu dùng trên mesh 3D.
- `Texture là mask/data`: Tắt `sRGB`. Chỉ bật mipmap nếu shader thật sự sample theo khoảng cách hoặc screen size thay đổi.
- `Texture là UI icon`: tắt mipmap nếu luôn hiển thị gần full size. Kiểm tra compression artifact vì UI dễ lộ lỗi.
- `Texture là ground/floor`: Bật mipmap, dùng Trilinear, tăng Aniso Level nếu cần. Texture này nhìn ở góc nghiêng.
- `Texture cần CPU chỉnh pixel`: Bật Read/Write. Tắt lại nếu không cần để giảm memory.
- `Build mobile`: Dùng platform override. Ưu tiên format như ASTC/ETC2 tùy target device.

## Common traps
- `PNG nhỏ nghĩa là runtime memory nhỏ`: Sai. Runtime memory phụ thuộc texture format sau import, resolution, mipmap, và compression.
- `Bật sRGB cho mọi texture`: Sai. Data texture sẽ bị gamma conversion làm sai giá trị.
- `Bật Read/Write cho an toàn`: Sai. Nó giữ thêm copy CPU-side và tăng memory.
- `Tắt mipmap cho mọi texture để tiết kiệm memory`: Sai. Texture 3D ở xa dễ shimmer và có thể tốn bandwidth hơn.
- `Dùng compression cao cho UI text`: Có thể gây artifact rất rõ. Cần preview trên target resolution và platform.

## Review questions

### Mipmap dùng để làm gì?
- Mipmap cung cấp các bản texture nhỏ dần để GPU sample ổn định hơn khi object ở xa, giảm aliasing và bandwidth.

### Mipmap có cost gì?
- Tăng khoảng `33%` dung lượng texture trên disk và memory, và không có lợi nếu texture luôn render full resolution.

### Khi nào tắt sRGB?
- Khi texture lưu data tuyến tính như normal, mask, metallic, roughness, height, lookup table, hoặc giá trị shader cần chính xác.

### Vì sao Read/Write tắt?
- Vì Unity phải giữ thêm bản copy texture data cho CPU, làm tăng memory. Chỉ bật khi script cần đọc/ghi pixel.

## Related notes
- [[Texture]]
- [[Definition]]
- [[Texture Import Settings]]
- [[Mipmap]]
- [[Texture Compression]]
- [[../Shader/HLSL Shader Code|HLSL Shader Code]]
- [[../Unity|Unity]]
