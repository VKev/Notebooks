---
aliases:
  - Mip Map
  - Mipmaps
  - Mipmap Streaming
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Mipmap` là chuỗi phiên bản texture có resolution nhỏ dần để GPU sample texture ổn định hơn khi object ở xa hoặc bị nhìn ở góc nghiêng.

## Key points
- Unity `6.4 (6000.4)`: mipmap level là version của texture ở resolution cụ thể, và mipmaps là tập hợp nhiều level nhỏ dần của cùng texture.
- Mipmap level `0` là texture gốc, các level sau là bản downsample nhỏ hơn.
- GPU chọn mip level dựa trên UV derivative của pixel hiện tại và các pixel lân cận.
- Mipmap có thể giảm artifact khi texture bị render nhỏ hơn resolution gốc và có thể tăng tốc sampling.
- Khi `Generate Mipmap` bật, Unity tạo các mip level khi import texture.
- Khi object ở xa hoặc texture chiếm ít pixel hơn, GPU dùng mip level thấp hơn để tránh aliasing và giảm sampling cost.
- `Trilinear` filtering blend giữa hai mip level để transition mượt hơn.
- `Mipmap Filtering` có `Box` để smooth dần và `Kaiser` để sharpen khi mip level nhỏ hơn.

## Decision rules
- Dùng `Kaiser` khi mip level quá mờ và texture cần giữ detail.
- Không bật cho UI texture luôn render gần full resolution.
- Không bật cho pixel art nếu muốn giữ cạnh sắc và kiểm soát scaling bằng Point filter.
- Không bật cho lookup texture, mask nhỏ, hoặc data texture cần exact texel nếu shader sample theo tọa độ cố định.
- Giảm shimmering, moire, và aliasing khi texture ở xa.
- Giảm texture bandwidth khi object nhỏ trên màn hình.
- Hỗ trợ streaming để tiết kiệm texture memory trong scene lớn.
- Bật cho đa số texture 3D như albedo, normal, roughness, terrain, prop, environment.

## Example
```csharp
```

## Related notes
- [[Definition]]
- [[Texture Import Settings]]
- [[Texture Compression]]
- [[../Shader/HLSL Shader Code|HLSL Shader Code]]
- [[Summary]]
