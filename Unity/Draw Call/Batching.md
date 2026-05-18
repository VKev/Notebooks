---
aliases:
  - Draw Call Batching
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `Batching` là nhóm kỹ thuật gộp mesh hoặc gộp dữ liệu vẽ để Unity giảm số lần chuẩn bị và gửi draw call.

## Key points
- Theo Unity `6.4`, batching là cách kết hợp các mesh dùng cùng render state để vẽ cùng nhau.
- Thực tế bạn sẽ gặp chủ yếu `static batching` và `dynamic batching`.
- `Static batching` phù hợp với object không di chuyển; `dynamic batching` là cơ chế cũ hơn cho object nhỏ đang di chuyển.
- `Static batching` kết hợp các mesh static thành mesh lớn hơn để giảm draw call.
- `Dynamic batching` transform vertex trên CPU sang world space rồi gom object nhỏ lại nếu chi phí này rẻ hơn chi phí draw call.
- Unity chỉ batch được khi object đủ điều kiện về material, shader pass, lightmap, và các ràng buộc khác.

## Decision rules
- Dùng `static batching` khi có nhiều object không di chuyển và dùng material tương thích.
- Tránh bật `dynamic batching` mặc định rồi giả định luôn nhanh hơn, vì Unity `6.4` nêu rõ trên hardware hiện đại có thể tệ hơn.
- Không phù hợp nếu bạn dùng object có material instance khác nhau, multi-pass shader, hoặc các trường hợp không thỏa điều kiện batch.
- Giảm CPU overhead khi scene có nhiều mesh nhỏ tương tự nhau.
- Hữu ích với Built-in Render Pipeline hoặc các trường hợp static geometry lặp lại nhiều.
- Chỉ cân nhắc `dynamic batching` sau khi profile, nhất là trên thiết bị thấp hoặc scene có nhiều mesh nhỏ.
- `Dynamic batching` cho mesh có giới hạn về vertex attributes và số vertex.
- `Static batching` tốn thêm memory vì Unity phải giữ dữ liệu mesh đã batch.

## Example
```csharp
using UnityEngine;
public class StaticBatchingExample : MonoBehaviour
{
    [SerializeField] private GameObject staticRoot;
    private void Start()
    {
        StaticBatchingUtility.Combine(staticRoot);
    }
}
```

## Related notes
- [[Definition]]
- [[Render State]]
- [[GPU Instancing]]
- [[SRP Batcher]]
- [[Summary]]
