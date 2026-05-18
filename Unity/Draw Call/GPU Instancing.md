---
aliases:
  - GPU Instancing
note_type: feature
tags:
  - unity
sticker: lucide//align-justify
---

## Core idea
- `GPU Instancing` là kỹ thuật dùng một draw call để render nhiều GameObject dùng cùng mesh và material.

## Key points
- Theo Unity `6.4`, GPU instancing là draw call optimization dựa trên khả năng phần cứng của GPU.
- Mỗi bản sao của mesh là instance, và mỗi instance vẫn có thể có vài property khác nhau như màu hoặc scale.
- Unity gom nhiều instance dùng cùng mesh và material vào một lệnh vẽ.
- Material hoặc shader phải hỗ trợ instancing.
- API script-side như `Graphics.RenderMeshInstanced` cũng hỗ trợ kiểu render này.

## Decision rules
- Khi có nhiều object cùng mesh và material lặp lại nhiều lần.
- Dùng sau khi profile để xác nhận lợi ích trên platform mục tiêu.
- Không phải lựa chọn mặc định cho URP/HDRP nếu `SRP Batcher` hoặc kỹ thuật SRP-native khác phù hợp hơn.
- Không hữu ích nếu mesh hoặc material quá khác nhau, vì khi đó Unity không thể instancing chung.
- hợp cho các đối tượng lặp lại nhiều lần như cây, bụi cỏ, cột đèn, prop giống nhau.
- có thể giảm draw call mạnh khi scene có nhiều instance thực sự giống nhau.
- Trong Built-in Render Pipeline, Unity `6.4` khuyến nghị cân nhắc bật khi có nhiều instance hoặc nhiều lights.
- Theo Unity `6.4`, lợi ích của GPU instancing phụ thuộc platform và GPU; có trường hợp overhead thu thập và upload property làm lợi ích bị giảm.

## Example
```csharp
using UnityEngine;
public class EnableInstancingExample : MonoBehaviour
{
    [SerializeField] private Material material;
    private void Awake()
    {
        material.enableInstancing = true;
    }
}
```

## Related notes
- [[Definition]]
- [[Render State]]
- [[Batching]]
- [[SRP Batcher]]
- [[Summary]]
