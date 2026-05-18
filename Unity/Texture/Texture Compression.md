---
aliases:
  - GPU Texture Format
  - Texture Format
  - Platform Texture Override
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Texture Compression` là việc chọn GPU texture format và compression setting để giảm memory, bandwidth, và build size theo từng platform.

## Key points
- Unity `6.3 LTS (6000.3)`: Unity hỗ trợ chọn texture format theo platform vì GPU desktop, iOS, Android, console, và Web có hỗ trợ format khác nhau.
- Desktop dùng nhóm BC/DXT, ví dụ DXT1 cho RGB, DXT5 hoặc BC7 cho RGBA, BC6H cho HDR.
- iOS/tvOS hiện đại ưu tiên ASTC cho RGB/RGBA.
- Android phức tạp hơn vì phụ thuộc GPU và sub-target, nhưng ASTC hoặc ETC2 là lựa chọn chính cho thiết bị hiện đại.
- Platform override trong Texture Import Settings là nơi đặt max size, format, compression quality, và một số option riêng platform.
- Texture source như PNG/TGA/PSD không nhất thiết là format runtime.
- Unity import source file rồi encode sang GPU texture format phù hợp platform.
- Compression giảm memory và bandwidth nhưng có thể tạo artifact.
- Alpha, HDR, normal map, và data texture có yêu cầu format khác nhau.
- Nếu thiết bị không hỗ trợ format đã chọn, Unity có thể phải decompress hoặc fallback, làm tăng memory hoặc CPU cost.

## Decision rules
- Texture chiếm nhiều memory nhất trong game.
- Compression đúng giúp giảm VRAM, RAM, build size, và bandwidth.
- Platform override giúp mobile build không dùng texture format desktop quá nặng.
- Dùng compression cho hầu hết texture lớn trong runtime.
- Dùng higher quality format cho albedo chính, character, hero asset, UI lớn.
- Dùng format phù hợp alpha nếu texture cần transparency.
- Dùng platform override cho Android/iOS riêng thay vì chỉ dùng default import setting.
- Không dùng lossy compression mạnh cho UI text, icon nhỏ sắc nét, hoặc texture có gradient nhạy banding.
- Không dùng format không được target platform hỗ trợ.
- Không assume PNG nhỏ trên disk nghĩa là runtime memory nhỏ, vì runtime memory phụ thuộc texture format sau import.
- Compression artifact tùy nội dung texture, đặc biệt normal map, smooth gradient, và UI.
- ASTC block size càng lớn càng tiết kiệm memory nhưng càng giảm chất lượng.
- Crunch compression giúp giảm disk/build size nhưng texture vẫn cần GPU format runtime sau khi load.
- Format tốt nhất phụ thuộc target device cụ thể, không có một setting tối ưu cho mọi platform.

## Example
```csharp
```

## Related notes
- [[Definition]]
- [[Texture Import Settings]]
- [[Mipmap]]
- [[../Render Pipeline/URP|URP]]
- [[Summary]]
