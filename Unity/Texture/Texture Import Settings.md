---
aliases:
  - Texture Importer
  - Texture Settings
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Texture Import Settings` là các setting trong Inspector quyết định Unity chuyển file ảnh source thành texture asset runtime như thế nào.

## Key points
- Unity `6.3 LTS (6000.3)`: Texture Import Settings window định nghĩa cách Unity import image file từ `Assets` vào Editor.
- `Texture Type` quyết định mục đích dùng texture, ví dụ Default, Normal Map, Sprite, Cubemap, GUI, hoặc Editor GUI.
- `sRGB (Color Texture)` nên bật cho non-HDR color texture như albedo/base color, và tắt cho data texture cần giá trị tuyến tính chính xác như mask, metallic, roughness, height, normal.
- `Read/Write` cho phép script đọc/ghi pixel nhưng Unity giữ thêm bản copy CPU-side, làm tăng memory.
- `Platform Override` cho phép đặt max size, compression, và format riêng cho từng platform.
- Khi import texture, Unity đọc source image rồi tạo runtime texture theo setting đã chọn.
- `Alpha Source` quyết định alpha lấy từ file, bỏ qua, hoặc tạo từ grayscale.
- `Alpha is Transparency` dilate color quanh alpha edge để giảm artifact khi lọc texture transparent.
- `Non Power of 2` quyết định texture NPOT giữ nguyên hay scale về power-of-two khi import.
- `Wrap Mode` quyết định UV ngoài range `0..1` lặp, clamp, mirror, hoặc per-axis.
- `Filter Mode` quyết định texture sample khi bị phóng to/thu nhỏ: Point, Bilinear, hoặc Trilinear.
- `Aniso Level` cải thiện chất lượng texture nhìn ở góc nghiêng, đặc biệt floor và ground texture, nhưng tốn tài nguyên hơn.

## Decision rules
- Setting đúng giúp texture sắc hơn, ít artifact hơn, và dùng ít memory hơn.
- Setting đúng giúp shader đọc data chính xác, đặc biệt với mask map và normal map.
- Platform override giúp tránh build mobile dùng texture quá lớn hoặc format không phù hợp.
- Dùng `Default` cho albedo, mask, UI texture không phải sprite, noise, LUT đơn giản.
- Dùng `Normal Map` cho normal texture để Unity import đúng format normal mapping.
- Dùng `Sprite (2D and UI)` cho sprite, UI image, tilesheet, hoặc sprite atlas source.
- Dùng `Read/Write` chỉ khi code cần `GetPixels`, `SetPixels`, runtime encode, hoặc CPU processing.
- Dùng `Clamp` cho UI, decal, mask, hoặc LUT để tránh edge lặp.
- Không bật `Read/Write` nếu shader chỉ sample texture trên GPU.
- Không bật `sRGB` cho data texture vì gamma conversion làm sai giá trị số.
- Không dùng `Point` cho texture cần smooth scaling, trừ pixel art hoặc style cố ý.
- Không dùng max size quá cao chỉ vì source file lớn, nếu object không bao giờ cần detail đó trên màn hình.
- Import setting thay đổi có thể làm Unity reimport asset và thay đổi memory/build size.
- Format tương thích tùy platform, nên texture đẹp trên desktop chưa chắc tối ưu trên mobile.
- Một số texture shape luôn dùng mọi mipmap level, không theo Mipmap Limit setting như texture 2D.

## Example
```csharp
```

## Related notes
- [[Definition]]
- [[Mipmap]]
- [[Texture Compression]]
- [[../Shader/HLSL Shader Code|HLSL Shader Code]]
- [[Summary]]
