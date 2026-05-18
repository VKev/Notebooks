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
- Theo Unity `6.3`, GPU instancing là draw call optimization dựa trên khả năng phần cứng của GPU.
- Mỗi bản sao của mesh là một instance, và mỗi instance vẫn có thể có vài property khác nhau như màu hoặc scale.
- Unity gom nhiều instance dùng cùng mesh và material vào một lệnh vẽ.
- Material hoặc shader phải hỗ trợ instancing.
- Một số API script-side như `Graphics.RenderMeshInstanced` cũng hỗ trợ kiểu render này.

## Decision rules
- Rất hợp cho các đối tượng lặp lại nhiều lần như cây, bụi cỏ, cột đèn, prop giống nhau.
- Có thể giảm draw call mạnh khi scene có nhiều instance thực sự giống nhau.
- Khi có nhiều object cùng mesh và material lặp lại nhiều lần.
- Trong Built-in Render Pipeline, Unity `6.3` khuyến nghị cân nhắc bật khi có nhiều instance hoặc nhiều lights.
- Dùng sau khi profile để xác nhận lợi ích trên platform mục tiêu.
- Không phải lựa chọn mặc định cho URP/HDRP nếu `SRP Batcher` hoặc kỹ thuật SRP-native khác phù hợp hơn.
- Không hữu ích nếu mesh hoặc material quá khác nhau, vì khi đó Unity không thể instancing chung.
- Theo Unity `6.3`, lợi ích của GPU instancing phụ thuộc platform và GPU; có trường hợp overhead thu thập và upload property làm lợi ích bị giảm.
- `Skinned Mesh Renderer` không nằm trong nhóm mesh renderer được note giới thiệu cho prebuilt material instancing.
- Trong URP/HDRP, với custom shader bạn phải cân nhắc quan hệ với `SRP Batcher`.

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
