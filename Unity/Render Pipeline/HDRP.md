---
aliases:
  - High Definition Render Pipeline
  - HD Render Pipeline
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `HDRP` là render pipeline cao cấp của Unity dành cho đồ họa high-fidelity trên nền tảng mạnh như PC, console, và use case XR.

## Key points
- Unity `6.4 (6000.4)`: `High Definition Render Pipeline` là prebuilt `Scriptable Render Pipeline` high-fidelity, nhắm tới nền tảng hiện đại có hỗ trợ compute shader.
- HDRP dùng physically-based lighting, linear lighting, HDR lighting, và kiến trúc hybrid Tile/Cluster deferred/Forward lighting.
- HDRP có nhiều feature cao cấp như volumetric lighting, screen-space effect, ray tracing, path tracing, physical light units, và material model phức tạp hơn URP.
- Tên viết tắt đúng là `HDRP`, không phải `HDURP`.
- Cấu hình qua `HDRenderPipelineAsset` gán trong Project Settings > Graphics.
- HDRP hỗ trợ cả forward và deferred rendering, trong đó deferred hiệu quả hơn cho material phổ biến còn forward phù hợp material phức tạp.
- HDRP dùng hybrid tile/cluster renderer để tạo local light list và xử lý nhiều light hơn.
- Hỗ trợ ray tracing cho reflection, shadow, ambient occlusion, và global illumination trên hardware tương thích.

## Decision rules
- Khi project nhắm tới PC, console, hoặc XR và cần đồ họa cao cấp nhất.
- Khi team có artist cần physical lighting workflow giống production film.
- Dùng cho cinematic trailer, architectural visualization, automotive rendering.
- Không dùng cho mobile game, vì HDRP không hỗ trợ và yêu cầu phần cứng mạnh.
- Tránh nếu project cần chạy trên nhiều nền tảng khác nhau, dùng URP thay thế.
- Tránh nếu team nhỏ và không cần đồ họa phức tạp, vì learning curve và setup cost cao.
- Chất lượng hình ảnh cao nhất có thể trong Unity: volumetric fog, subsurface scattering, area light, ray tracing.
- Physical light units giúp lighting chính xác và nhất quán với reference thực tế.

## Example
```csharp
using UnityEngine;
using UnityEngine.Rendering.HighDefinition;
public class WeatherSystem : MonoBehaviour
{
    public VolumeProfile stormProfile;
    public VolumeProfile clearProfile;
    private Volume _volume;
    void Awake()
    {
        _volume = GetComponent<Volume>();
    }
    public void SetStorm(bool isStorm)
    {
        _volume.profile = isStorm ? stormProfile : clearProfile;
    }
}
```

## Related notes
- [[Definition]]
- [[SRP]]
- [[Render Graph]]
- [[Scriptable Render Pass]]
- [[URP]]
- [[Shader Graph]]
- [[Summary]]
