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
- Unity `6.4 (6000.4)`: Texture Import Settings window định nghĩa cách Unity import image file từ `Assets` vào Editor.
- `Texture Type` quyết định mục đích dùng texture, ví dụ Default, Normal Map, Sprite, Cubemap, GUI, hoặc Editor GUI.
- `sRGB (Color Texture)` nên bật cho non-HDR color texture như albedo/base color, và tắt cho data texture cần giá trị tuyến tính chính xác như mask, metallic, roughness, height, normal.
- `Read/Write` cho phép script đọc/ghi pixel nhưng Unity giữ thêm bản copy CPU-side, làm tăng memory.
- `Platform Override` cho phép đặt max size, compression, và format riêng cho từng platform.
- Khi import texture, Unity đọc source image rồi tạo runtime texture theo setting đã chọn.
- `Alpha Source` quyết định alpha lấy từ file, bỏ qua, hoặc tạo từ grayscale.
- `Alpha is Transparency` dilate color quanh alpha edge để giảm artifact khi lọc texture transparent.

## Decision rules
- Dùng `Default` cho albedo, mask, UI texture không phải sprite, noise, LUT đơn giản.
- Dùng `Normal Map` cho normal texture để Unity import đúng format normal mapping.
- Dùng `Sprite (2D and UI)` cho sprite, UI image, tilesheet, hoặc sprite atlas source.
- Dùng `Read/Write` chỉ khi code cần `GetPixels`, `SetPixels`, runtime encode, hoặc CPU processing.
- Dùng `Clamp` cho UI, decal, mask, hoặc LUT để tránh edge lặp.
- Không bật `Read/Write` nếu shader chỉ sample texture trên GPU.
- Setting đúng giúp texture sắc hơn, ít artifact hơn, và dùng ít memory hơn.
- Setting đúng giúp shader đọc data chính xác, đặc biệt với mask map và normal map.

## Example
```csharp
```

## Related notes
- [[Definition]]
- [[Mipmap]]
- [[Texture Compression]]
- [[../Shader/HLSL Shader Code|HLSL Shader Code]]
- [[Summary]]
